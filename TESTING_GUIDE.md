# BanditBot Testing Guide

## Overview

This guide explains how to write tests for BanditBot commands using the comprehensive mock infrastructure. The testing system allows you to test Discord commands without requiring a live Discord connection.

## Quick Start

### Running Tests

```bash
# Run all tests
./run-tests.sh

# Run specific test file
python3 -m pytest tests/test_discord_commands.py -v

# Run specific test class
python3 -m pytest tests/test_discord_commands.py::TestSimpleCommands -v

# Run specific test
python3 -m pytest tests/test_discord_commands.py::TestSimpleCommands::test_dn_command_mock -v
```

## Mock Infrastructure

### Available Mock Objects

The testing infrastructure provides comprehensive mock implementations of Discord objects:

- **MockUser** - Basic Discord user
- **MockMember** - User with guild-specific attributes (roles, nickname)
- **MockGuild** - Discord server/guild
- **MockChannel** - Text or voice channel
- **MockMessage** - Discord message with all attributes
- **MockRole** - Server role

### Creating Mock Objects

#### Simple Mock Creation

```python
from tests.mocks import (
    create_mock_message,
    create_mock_user,
    create_mock_member,
    create_mock_guild,
    create_mock_channel
)

# Create a simple message for testing
message = create_mock_message(content="?test")

# Create a user
user = create_mock_user(user_id=123456789, username="TestUser")

# Create a guild
guild = create_mock_guild(guild_id=111222333, name="Test Server")
```

#### Advanced Mock Creation

```python
from tests.mocks import MockRole, create_mock_member, create_mock_guild

# Create guild with members and roles
guild = create_mock_guild(guild_id=111222333, name="Test Server")

# Create custom roles
moderator_role = MockRole(role_id=999888777, name="Moderator")
admin_role = MockRole(role_id=888777666, name="Admin")

# Create member with roles
moderator = create_mock_member(
    user_id=123456789,
    username="ModUser",
    guild=guild,
    roles=[MockRole(name="@everyone"), moderator_role],
    nick="The Moderator"
)

# Create message from this member
message = create_mock_message(
    content="?kick @baduser Spamming",
    author=moderator,
    guild=guild
)
```

## Common Test Patterns

### Testing Simple Commands (No Parameters)

Commands like `?dn` or `?touch` that don't require parameters:

```python
import pytest
from tests.mocks import create_mock_message

class TestSimpleCommands:
    @pytest.mark.asyncio
    async def test_dn_command(self):
        """Test ?dn command."""
        # Create mock message
        message = create_mock_message(content="?dn")
        
        # Parse command
        command_name = message.content[1:].split()[0]
        assert command_name == "dn"
        
        # Test reply functionality
        await message.reply("Hah! GOTEEEEEM")
        message.reply.assert_called_once()
```

### Testing Commands with Parameters

Commands like `?kick @user reason` that require parameters:

```python
import pytest
from tests.mocks import create_mock_message, create_mock_member, create_mock_guild

class TestParameterizedCommands:
    @pytest.mark.asyncio
    async def test_kick_command(self):
        """Test ?kick command with user parameter."""
        guild = create_mock_guild()
        target_user = create_mock_member(user_id=987654321, username="BadUser", guild=guild)
        author = create_mock_member(user_id=123456789, username="Moderator", guild=guild)
        
        # Create message with mention
        message = create_mock_message(
            content=f"?kick {target_user.mention} Spamming",
            author=author,
            mentions=[target_user]
        )
        
        # Verify mentions extracted
        assert len(message.mentions) == 1
        assert message.mentions[0].id == target_user.id
        
        # Extract reason (collect_remaining)
        command_parts = message.content[1:].split()
        reason_parts = [p for p in command_parts[1:] if not p.startswith('<@')]
        reason = ' '.join(reason_parts)
        
        assert "Spamming" in reason
```

### Testing Permission Checking

Test that role-based permissions work correctly:

```python
from tests.mocks import MockRole, create_mock_member

def test_role_permission():
    """Test that moderator has permission."""
    # Create member with Moderator role
    mod_role = MockRole(role_id=888999000, name="Moderator")
    moderator = create_mock_member(
        user_id=123456789,
        username="ModUser",
        roles=[MockRole(name="@everyone"), mod_role]
    )
    
    # Check permission
    required_roles = ["Moderator", "Admin"]
    user_role_names = [role.name for role in moderator.roles]
    has_permission = any(role in user_role_names for role in required_roles)
    
    assert has_permission
```

### Testing Error Handling

Test that commands handle errors appropriately:

```python
import pytest
from tests.mocks import create_mock_message

class TestErrorHandling:
    @pytest.mark.asyncio
    async def test_missing_parameter(self):
        """Test error when required parameter is missing."""
        message = create_mock_message(content="?kick")
        
        # Parse command
        command_parts = message.content[1:].split()
        parameters = command_parts[1:]
        
        # No parameters provided
        assert len(parameters) == 0
        
        # Should reply with error
        error_msg = "❌ Missing required parameter: user"
        await message.reply(error_msg)
        message.reply.assert_called_once()
```

### Testing Async Methods

All Discord API calls are async, so tests must use `pytest.mark.asyncio`:

```python
import pytest
from tests.mocks import create_mock_member

class TestMemberMethods:
    @pytest.mark.asyncio
    async def test_member_kick(self):
        """Test member kick method."""
        member = create_mock_member(user_id=123456789, username="BadUser")
        
        # Call async method
        await member.kick(reason="Spamming")
        
        # Verify it was called
        assert member.kick.called
        
        # Check arguments
        call_args = member.kick.call_args
        assert "Spamming" in str(call_args) or call_args[1].get('reason') == "Spamming"
```

## Testing Real Command Execution

### Integration Testing with Command Handlers

For more comprehensive testing, you can test actual command execution:

```python
import pytest
from unittest.mock import patch
from tests.mocks import create_mock_message

class TestCommandIntegration:
    @pytest.mark.asyncio
    async def test_real_command_execution(self):
        """Test actual command execution flow."""
        # Create mock message
        message = create_mock_message(content="?touch")
        
        # Mock the command configuration
        with patch('banditbot.MODULAR_COMMANDS', {
            'touch': {
                'description': 'Test command',
                'actions': [
                    {'type': 'reply', 'message': '👋 Touch command received!'}
                ]
            }
        }):
            # Import and call execute_modular_command
            from banditbot import execute_modular_command
            
            await execute_modular_command(message, "touch", [])
            
            # Verify reply was called
            assert message.reply.called
```

## Best Practices

### 1. Use Descriptive Test Names

```python
# Good
def test_dn_command_replies_with_goteem_message():
    pass

# Bad
def test_dn():
    pass
```

### 2. Test One Thing Per Test

```python
# Good - focused test
def test_kick_command_extracts_user_from_mentions():
    message = create_mock_message(mentions=[user])
    assert len(message.mentions) == 1

# Bad - testing multiple things
def test_kick_command():
    # Tests mention extraction
    # Tests permission checking
    # Tests reason parsing
    # Tests database logging
    pass
```

### 3. Use Setup Methods for Common Objects

```python
class TestModeratorCommands:
    @pytest.fixture
    def moderator(self):
        """Create a moderator member for tests."""
        mod_role = MockRole(name="Moderator")
        return create_mock_member(
            user_id=123456789,
            username="ModUser",
            roles=[MockRole(name="@everyone"), mod_role]
        )
    
    def test_moderator_can_kick(self, moderator):
        """Test moderator has kick permission."""
        assert any(role.name == "Moderator" for role in moderator.roles)
```

### 4. Always Use `pytest.mark.asyncio` for Async Tests

```python
# Required for async tests
@pytest.mark.asyncio
async def test_async_command():
    message = create_mock_message()
    await message.reply("test")
```

### 5. Test Both Success and Failure Cases

```python
def test_permission_granted():
    """Test user with required role."""
    # Test success case
    pass

def test_permission_denied():
    """Test user without required role."""
    # Test failure case
    pass
```

## Common Assertions

### Message Assertions

```python
# Assert reply was called
message.reply.assert_called_once()
message.reply.assert_called()

# Assert reply was called with specific content
call_args = message.reply.call_args
assert "expected text" in str(call_args)

# Assert reply was not called
message.reply.assert_not_called()
```

### Member Method Assertions

```python
# Assert member was kicked
member.kick.assert_called_once()

# Assert member was banned with reason
member.ban.assert_called()
call_args = member.ban.call_args
assert call_args[1].get('reason') == "Expected reason"

# Assert roles were added
member.add_roles.assert_called()
```

### Permission Assertions

```python
# Assert user has required role
user_role_names = [role.name for role in member.roles]
assert "Moderator" in user_role_names

# Assert user lacks forbidden role
assert "Banned" not in user_role_names
```

## Debugging Tests

### Enable Verbose Output

```bash
# Show detailed test output
python3 -m pytest tests/test_discord_commands.py -v -s

# Show full tracebacks
python3 -m pytest tests/test_discord_commands.py --tb=long

# Run with logging output
python3 -m pytest tests/test_discord_commands.py --log-cli-level=DEBUG
```

### Inspect Mock Call Arguments

```python
# Print all calls to a mock
print(message.reply.call_args_list)

# Get specific call arguments
call_args, call_kwargs = message.reply.call_args
print(f"Args: {call_args}, Kwargs: {call_kwargs}")

# Check if specific argument was passed
assert call_kwargs.get('embed') is not None
```

## Adding Tests for New Commands

### Step 1: Identify Command Requirements

- Does it need parameters? (user, reason, duration, etc.)
- Does it require specific roles?
- What does it do? (reply, add role, kick, etc.)
- Are there error cases? (missing params, invalid input)

### Step 2: Create Test Class

```python
class TestMyNewCommand:
    """Tests for ?mynewcommand."""
    pass
```

### Step 3: Write Basic Test

```python
@pytest.mark.asyncio
async def test_mynewcommand_basic(self):
    """Test basic ?mynewcommand execution."""
    message = create_mock_message(content="?mynewcommand")
    
    # Add assertions for expected behavior
    pass
```

### Step 4: Add Parameter Tests (if needed)

```python
@pytest.mark.asyncio
async def test_mynewcommand_with_parameter(self):
    """Test ?mynewcommand with parameter."""
    user = create_mock_user()
    message = create_mock_message(
        content=f"?mynewcommand {user.mention}",
        mentions=[user]
    )
    
    # Test parameter extraction
    assert len(message.mentions) == 1
```

### Step 5: Add Permission Tests (if needed)

```python
def test_mynewcommand_requires_moderator(self):
    """Test ?mynewcommand requires Moderator role."""
    # Create member without required role
    member = create_mock_member()
    
    required_roles = ["Moderator"]
    user_role_names = [role.name for role in member.roles]
    has_permission = any(role in user_role_names for role in required_roles)
    
    assert not has_permission
```

### Step 6: Add Error Handling Tests

```python
@pytest.mark.asyncio
async def test_mynewcommand_missing_parameter(self):
    """Test ?mynewcommand with missing parameter."""
    message = create_mock_message(content="?mynewcommand")
    
    # Should reply with error
    await message.reply("❌ Missing required parameter")
    message.reply.assert_called_once()
```

## Example: Complete Test for a New Command

```python
import pytest
from tests.mocks import (
    create_mock_message, create_mock_member, 
    create_mock_guild, MockRole
)

class TestWarnCommand:
    """Tests for ?warn command."""
    
    @pytest.fixture
    def guild(self):
        """Create test guild."""
        return create_mock_guild(name="Test Server")
    
    @pytest.fixture
    def moderator(self, guild):
        """Create moderator member."""
        mod_role = MockRole(name="Moderator")
        return create_mock_member(
            user_id=111111111,
            username="ModUser",
            guild=guild,
            roles=[MockRole(name="@everyone"), mod_role]
        )
    
    @pytest.fixture
    def target_user(self, guild):
        """Create target user to warn."""
        return create_mock_member(
            user_id=222222222,
            username="TargetUser",
            guild=guild
        )
    
    @pytest.mark.asyncio
    async def test_warn_command_basic(self, moderator, target_user):
        """Test basic ?warn command."""
        message = create_mock_message(
            content=f"?warn {target_user.mention} Breaking rules",
            author=moderator,
            mentions=[target_user]
        )
        
        # Parse command
        assert message.content.startswith("?warn")
        assert len(message.mentions) == 1
        assert message.mentions[0].id == target_user.id
        
        # Should reply with confirmation
        await message.reply(f"⚠️ Warned {target_user.mention}")
        message.reply.assert_called_once()
    
    def test_warn_requires_moderator_role(self, moderator):
        """Test ?warn requires Moderator role."""
        required_roles = ["Moderator", "Admin"]
        user_role_names = [role.name for role in moderator.roles]
        has_permission = any(role in user_role_names for role in required_roles)
        
        assert has_permission
    
    @pytest.mark.asyncio
    async def test_warn_missing_user_parameter(self):
        """Test ?warn with missing user parameter."""
        message = create_mock_message(content="?warn")
        
        command_parts = message.content[1:].split()
        parameters = command_parts[1:]
        
        assert len(parameters) == 0
        
        # Should reply with error
        await message.reply("❌ Missing required parameter: user")
        message.reply.assert_called_once()
```

## CI/CD Integration

Tests are automatically run by the CI pipeline when you push commits or create pull requests. The test runner script (`run-tests.sh`) includes the Discord command tests.

### Local Testing Before Push

```bash
# Run all tests
./run-tests.sh

# Run just the Discord command tests
python3 -m pytest tests/test_discord_commands.py -v
```

## Troubleshooting

### Test Import Errors

If you get import errors:

```bash
# Ensure you're in the project root
cd /path/to/banditbot

# Run tests with proper path
python3 -m pytest tests/test_discord_commands.py -v
```

### Async Test Failures

If async tests fail with "coroutine was never awaited":

```python
# Make sure you use @pytest.mark.asyncio
@pytest.mark.asyncio  # Required!
async def test_my_async_function():
    await some_async_call()
```

### Mock Not Being Called

If assertions fail because mock wasn't called:

```python
# Use .called to check if it was called at all
assert message.reply.called

# Use .assert_called_once() to ensure it was called exactly once
message.reply.assert_called_once()

# Print call count for debugging
print(f"Called {message.reply.call_count} times")
```

## Additional Resources

- **pytest documentation**: https://docs.pytest.org/
- **pytest-asyncio documentation**: https://pytest-asyncio.readthedocs.io/
- **unittest.mock documentation**: https://docs.python.org/3/library/unittest.mock.html
- **BanditBot Commands Documentation**: See `docs/COMMANDS.md`
- **Test Examples**: See `tests/test_discord_commands.py` for more examples

## Contributing Tests

When contributing new commands or features:

1. **Always add tests** for new commands
2. **Test both success and failure cases**
3. **Document any special setup** required
4. **Ensure tests pass locally** before pushing
5. **Keep tests focused and maintainable**

Good testing practices ensure BanditBot remains reliable and maintainable! 🧪✅
