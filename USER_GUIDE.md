# BanditBot User Guide

*Generated automatically on 2025-10-01 06:47:25 UTC*

## 📖 Table of Contents

1. [Getting Started](#-getting-started)
2. [Using Commands](#-using-commands)
3. [Available Commands](#-available-commands)
4. [Moderation Features](#-moderation-features)
5. [Automatic Triggers](#-automatic-triggers)
6. [Web Configuration](#-web-configuration)
7. [Troubleshooting](#-troubleshooting)

## 🚀 Getting Started

### What is BanditBot?

BanditBot is a sophisticated Discord moderation and utility bot designed for server management, entertainment, and automation. It features:

- **29 custom commands** for various functions
- **7 automatic triggers** that respond to message patterns
- **Advanced moderation tools** including sticky roles and timed actions
- **Web-based configuration** for easy management
- **Comprehensive logging** of all activities

### Basic Usage

All bot commands use the `?` prefix. For example:
- `?help` - Show help information
- `?joke` - Tell a random dad joke
- `?mute @user 10m Spamming` - Mute a user for 10 minutes

### Getting Help

- Use `?help` for a list of available commands
- Visit the web interface at `http://localhost:3000` for configuration
- Check the documentation at `/docs/` for detailed information

## 💬 Using Commands

### Command Structure

Commands follow this basic structure:
```
?command_name <required_parameter> [optional_parameter]
```

- **Required parameters** are shown in `<angle brackets>`
- **Optional parameters** are shown in `[square brackets]`
- **Multiple word parameters** should be quoted if they contain spaces

### Parameter Types

| Type | Description | Example |
|------|-------------|---------|
| `user` | Discord user mention or ID | `@username` or `123456789` |
| `role` | Role name or mention | `Moderator` or `@Moderator` |
| `channel` | Channel mention or ID | `#general` or `987654321` |
| `string` | Text content | `"Some text here"` |
| `integer` | Whole number | `42` |
| `duration` | Time duration | `10m`, `1h30m`, `2d` |

### Examples

```bash
# Simple command with no parameters
?joke

# Command with required parameter
?whois @username

# Command with multiple parameters
?mute @spammer 30m "Excessive spamming"

# Command with optional parameters
?purge 10 "Cleaning up chat"
```

## 📋 Available Commands

### Moderation Commands

#### `?add_trigger <name> <pattern> <actions>`
Add a new regex trigger with specified actions
*Requires: Moderator*

#### `?arrest <user> [reason]`
Temporarily detain a user by assigning the 'detained' role with a fun arrest message
*Requires: Moderator, Admin*

#### `?defenestrate <user> [reason]`
Defenestrate (throw out the window) a user by assigning the 'detained' role
*Requires: Moderator, Admin*

#### `?del_trigger <name>`
Delete a regex trigger by name
*Requires: Moderator*

#### `?delay_msg <delay> [target_channel] <message>`
Schedule a delayed message to be sent to a channel
*Requires: Moderator, Admin*

#### `?force_sticky <user>`
Force apply all sticky roles for a user (testing and enforcement)
*Requires: Moderator*

#### `?gtfo <user> [reason]`
Eject a user by assigning the 'detained' role with an ejection message
*Requires: Moderator, Admin*

#### `?list_triggers`
List all configured regex triggers and their actions
*Requires: Moderator*

#### `?lock [target_channel] [reason]`
Lock a text channel by removing send message permissions for @everyone role
*Requires: Moderator, Admin*

#### `?modlogs <user>`
View the moderation logs for a specific user
*Requires: Moderator*

#### `?mute <duration> <user> <reason>`
Temporarily mute a user by caching their roles, removing all roles, applying 'muted' role, with automatic restoration after specified duration
*Requires: Moderator, Admin*

#### `?note <user> <note>`
Add a moderation note for a user (creates a log entry without taking action)
*Requires: Moderator*

#### `?pending_delays`
Show pending delay queue actions
*Requires: Moderator, Admin*

#### `?purge <count> [reason]`
Bulk delete messages from the current channel (supports up to 999 messages + command message, with automatic batching)
*Requires: Moderator*

#### `?release <user> [reason]`
Release a user by removing the 'detained' role
*Requires: Moderator*

#### `?show_sticky <user>`
Show what sticky roles a user has configured
*Requires: Moderator*

#### `?thotbgone <user> [reason]`
Detain a user with a thot-be-gone themed message and assign the 'detained' role
*Requires: Moderator, Admin*

#### `?timeout <duration> <user> [reason]`
Timeout a user for a specified duration using Discord's built-in timeout feature
*Requires: Moderator, Admin*

#### `?unlock [target_channel] [reason]`
Unlock a text channel by restoring send message permissions for @everyone role
*Requires: Moderator, Admin*

#### `?unlock_all [reason]`
Unlock all channels in the server by restoring @everyone send message permissions
*Requires: Moderator, Admin*

#### `?unmute <user> [reason]`
Immediately unmute a user by restoring their cached roles and clearing any pending delayed actions
*Requires: Moderator, Admin*

### Utility Commands

#### `?stickyrole <role> <user> [reason]`
Assign a role to a user that will be automatically re-assigned if they leave and rejoin the server

#### `?stickyrole_rem <role> <user> [reason]`
Remove a sticky role from a user and delete it from the database

#### `?touch`
A simple test command to verify the bot is responding

#### `?whois <user>`
Display detailed information about a user including roles, join date, account age, and permissions

### Fun Commands

#### `?coon`
Post a random cute raccoon image with accompanying text

#### `?dn`
A fun 'deez nuts' meme response command

#### `?joke`
Tell a random dad joke from the bot's collection

#### `?worm`
Post a cute worm image with descriptive text


## 🛡️ Moderation Features

### Sticky Roles

Sticky roles are automatically re-assigned to users when they rejoin the server. This is useful for:
- **Muted users** - Prevents mute evasion by leaving/rejoining
- **Special roles** - Maintains VIP, verified, or other status roles
- **Punishment roles** - Ensures temporary restrictions persist

Commands:
- `?stickyrole <role> <user>` - Add a sticky role
- `?stickyrole_rem <role> <user>` - Remove a sticky role
- `?show_sticky <user>` - View a user's sticky roles

### Timed Actions

Many moderation actions support automatic expiration:
- `?mute <user> <duration>` - Temporary mutes with auto-unmute
- `?timeout <user> <duration>` - Discord native timeouts
- Custom timed actions via the delay queue system

### Moderation Logging

All moderation actions are automatically logged:
- **Action taken** - What was done
- **Target user** - Who was affected
- **Moderator** - Who performed the action
- **Reason** - Why the action was taken
- **Timestamp** - When it occurred

View logs with: `?modlogs <user>`

### Advanced Features

- **Bulk actions** - `?purge` for mass message deletion
- **Channel management** - `?lock`/`?unlock` for channel restrictions
- **Role caching** - Automatic role backup during mutes
- **Permission checking** - Role-based command access

## 🎯 Automatic Triggers

BanditBot can automatically respond to message patterns using regex triggers:

### bigfoot_trigger
**Pattern:** `\bbigfoot\b`
**Response:** Responds when someone mentions Bigfoot

### chloe_trigger
**Pattern:** `\bchloe\b`
**Response:** Responds when someone mentions Chloe

### null_mention_gay_react
**Pattern:** `<@!?100418001488064512>`
**Response:** Reacts with GAY when specific user is mentioned

### simon
**Pattern:** `\bsimon\b`
**Response:** Responds when someone mentions simon

### zoomer_fr
**Pattern:** `\bfr\b`
**Response:** Detects 'fr' zoomer slang from unverified users

### zoomer_no_cap
**Pattern:** `\bno cap\b`
**Response:** Detects 'no cap' zoomer slang from unverified users

### zoomer_wsp
**Pattern:** `\bwsp\b`
**Response:** Detects 'wsp' zoomer slang from unverified users


## 🌐 Web Configuration

BanditBot includes a comprehensive web interface for configuration management.

### Accessing the Interface

1. Start the bot with Docker: `./docker-setup.sh dev`
2. Open your browser to: `http://localhost:3000`
3. Use the visual editor to modify configuration

### Features

- **Visual Command Editor** - Create and modify commands with a GUI
- **Action Builder** - Drag-and-drop action configuration
- **Trigger Management** - Regex pattern editor with testing
- **Resource Manager** - Upload and organize images/text resources
- **Live Preview** - See changes before applying them
- **Backup System** - Automatic configuration backups

### Making Changes

1. **Edit configuration** using the visual interface
2. **Test changes** in a development environment
3. **Apply changes** using the "Save Configuration" button
4. **Reload bot** using `?reload` command in Discord


## 🔧 Troubleshooting

### Common Issues

#### Bot Not Responding
- Check if the bot is online in your server
- Verify the bot has permission to read/send messages
- Ensure you're using the correct command prefix (`?`)

#### Permission Errors
- Check if you have the required role for the command
- Verify the bot has the necessary Discord permissions
- Contact a server administrator if needed

#### Command Not Found
- Use `?help` to see available commands
- Check spelling and parameter format
- Some commands may be restricted to certain roles

#### Web Interface Issues
- Ensure the API server is running (`docker-compose ps`)
- Check if port 3000 is accessible
- Clear browser cache and refresh the page

### Getting Support

1. **Check the logs** - Look for error messages in console output
2. **Review documentation** - Check `/docs/` for detailed information
3. **Test in development** - Use `./run-dev.sh` to test locally
4. **Contact administrators** - Report issues to server staff

### Useful Commands for Troubleshooting

- `?touch` - Test if the bot is responding
- `?whois @yourself` - Test parameter parsing
- `?pending_delays` - Check queued actions
- `?status` - Bot status information (admin only)


## 📚 Additional Documentation

- **[Commands Reference](COMMANDS.md)** - Complete command documentation
- **[API Documentation](API.md)** - REST API reference
- **[Action System](ACTIONS.md)** - Action development guide
- **[Docker Deployment](DOCKER_DEPLOYMENT.md)** - Production deployment

---

*This user guide is automatically generated. Last updated: 2025-10-01 06:47:25 UTC*

**Bot Statistics:**
- Commands: 29
- Triggers: 7
- Action Flows: 2
- Event Handlers: 19
