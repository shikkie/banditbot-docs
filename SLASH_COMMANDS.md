# BanditBot Slash Commands Documentation

*Generated automatically on 2025-10-04 00:57:18 UTC*

## Overview

BanditBot supports Discord slash commands with 7 configured commands:
- **6 Mapped Commands**: Link to existing bot commands
- **1 Standalone Commands**: Custom actions without corresponding bot commands

## 📋 Slash Command Types

### Mapped Commands
Mapped commands link to existing bot commands (those that start with `?`) and inherit their functionality with Discord's native slash command interface.

### Standalone Commands  
Standalone commands define their own custom actions and don't require a corresponding bot command.

## ⚡ Available Slash Commands

### /kick

**Description**: Kick a user from the server

**Type**: Mapped Command → `?kick`

**Base Command Description**: Kick a user from the server

**Required Roles**: Moderator, Admin

**Parameters**: None

**Usage**: `/kick`

---

### /mute

**Description**: Temporarily mute a user

**Type**: Mapped Command → `?mute`

**Base Command Description**: Temporarily mute a user by caching their roles, removing all roles, applying 'muted' role, with automatic restoration after specified duration

**Required Roles**: Moderator, Admin

**Parameters**:
- **duration** (required) - *string* - Duration (e.g., 5m, 1h, 30s)
- **user** (required) - *user* - User to mute
- **reason** (optional) - *string* - Default: `No reason provided` - Reason for muting

**Usage**: `/mute duration:<value> user:<value> [reason:<value>]`

---

### /note

**Description**: Add a moderation note for a user (creates a log entry without taking action)

**Type**: Mapped Command → `?note`

**Base Command Description**: Add a moderation note for a user (creates a log entry without taking action)

**Required Roles**: Moderator

**Parameters**:
- **user** (required) - *user* - The user to add a moderation note for
- **note** (required) - *string* - The note content to add to the user's moderation history

**Usage**: `/note user:<value> note:<value>`

---

### /quickban

**Description**: Quickly ban a user with a standard message

**Type**: Standalone Command

**Required Roles**: Admin

**Parameters**:
- **user** (required) - *user* - User to ban
- **reason** (optional) - *string* - Default: `Violating server rules` - Reason for ban

**Actions**:
- **ban**
- **chan_log**: `🔨 **{interaction.user.display_name}** banned **{us...`

**Usage**: `/quickban user:<value> [reason:<value>]`

---

### /timeout

**Description**: Timeout a user using Discord's timeout feature

**Type**: Mapped Command → `?timeout`

**Base Command Description**: Timeout a user for a specified duration using Discord's built-in timeout feature

**Required Roles**: Moderator, Admin

**Parameters**: None

**Usage**: `/timeout`

---

### /unmute

**Description**: Remove mute from a user

**Type**: Mapped Command → `?unmute`

**Base Command Description**: Immediately unmute a user by restoring their cached roles and clearing any pending delayed actions

**Required Roles**: Moderator, Admin

**Parameters**: None

**Usage**: `/unmute`

---

### /warn

**Description**: Issue a warning to a user

**Type**: Mapped Command → `?note`

**Base Command Description**: Add a moderation note for a user (creates a log entry without taking action)

**Required Roles**: Moderator, Admin

**Parameters**: None

**Usage**: `/warn`

---

## 🔧 Configuration

Slash commands are configured in the `slash_commands` section of `modular_commands.yml`:

### Mapped Command Example
```yaml
slash_commands:
  mute:
    description: "Temporarily mute a user"
    roles: ["Moderator", "Admin"]
    command: "mute"  # Maps to ?mute command
    parameters:
      duration:
        type: "string"
        required: true
        description: "Duration (e.g., 5m, 1h, 30s)"
      user:
        type: "user"
        required: true
        description: "User to mute"
      reason:
        type: "string"
        required: false
        description: "Reason for muting"
        default: "No reason provided"
```

### Standalone Command Example
```yaml
slash_commands:
  quickban:
    description: "Quickly ban a user with a standard message"
    roles: ["Admin"]
    parameters:
      user:
        type: "user"
        required: true
        description: "User to ban"
      reason:
        type: "string"
        required: false
        description: "Reason for ban"
        default: "Violating server rules"
    actions:
      - type: "ban"
        user: "{user}"
        reason: "{reason}"
      - type: "chan_log"
        message: "🔨 **{interaction.user.display_name}** banned **{user.display_name}** via slash command\n📝 Reason: {reason}"
```

### Parameter Types

| Type | Description | Discord UI |
|------|-------------|------------|
| `string` | Text input | Text field |
| `user` | Discord user | User selector with autocomplete |
| `role` | Discord role | Role selector |
| `channel` | Discord channel | Channel selector |
| `integer` | Whole number | Number input |
| `number` | Decimal number | Number input |
| `boolean` | True/false | Checkbox |

### Parameter Properties

- **`type`** (required): Parameter type from the table above
- **`required`** (required): Whether the parameter is mandatory
- **`description`** (required): Help text shown to users
- **`default`** (optional): Default value for optional parameters

## 🔗 Integration

Slash commands integrate seamlessly with BanditBot's existing systems:

- **Permission System**: Uses the same role-based access control as regular commands
- **Action System**: Standalone commands use the same action types as regular commands
- **Parameter Substitution**: Supports the same `{parameter}` syntax for dynamic values
- **Logging**: All slash command usage is logged through the standard logging system

## 🚀 Benefits

1. **Native Discord Integration**: Commands appear in Discord's autocomplete
2. **Better UX**: Clear parameter types and descriptions
3. **Flexible Architecture**: Mix of mapped and standalone commands
4. **Type Safety**: Discord validates parameter types automatically
5. **Consistent Permissions**: Same role system as regular commands

---

*For more information about regular commands, see [COMMANDS.md](COMMANDS.md)*
*For action documentation, see [ACTIONS.md](ACTIONS.md)*
