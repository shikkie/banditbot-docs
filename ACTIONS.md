# BanditBot Action System Documentation

*Generated automatically on 2025-10-03 03:32:31 UTC*

## Overview

BanditBot uses a sophisticated action system with 57 registered actions. Actions are the building blocks of commands, triggers, and event handlers.

## 🎬 Action Registry

The action system uses Python decorators to register action handlers:

```python
@action(name="action_name")
async def action_handler(message, action_config, param_values, ...):
    # Action implementation
    pass
```

All actions receive the same standardized parameters for consistency and flexibility.

## 📋 Available Actions

### Channel Actions

#### `chan_log`

**Function:** `chan_log_action`

**Usage:**
```yaml
actions:
  - type: "chan_log"
    # Add configuration parameters here
```

---

#### `lock_all_channels`

**Description:** Lock all text channels by removing send_messages permission from @everyone role.

**Configuration:**
- reason: Reason for locking all channels (optional)
- exclude_channels: List of channel names/IDs to exclude from locking (optional)

**Function:** `lock_all_channels_action`

**Usage:**
```yaml
actions:
  - type: "lock_all_channels"
    # Add configuration parameters here
```

---

#### `lock_channel`

**Description:** Lock a channel by denying send_messages permission for @everyone role.

**Configuration:**
- channel: Parameter name containing the channel to lock (optional, defaults to current channel)
- reason: Reason for locking the channel (optional)

**Function:** `lock_channel_action`

**Usage:**
```yaml
actions:
  - type: "lock_channel"
    # Add configuration parameters here
```

---

#### `slowmode`

**Description:** Set channel slowmode delay.

**Configuration:**
- delay: Slowmode delay in seconds (0-21600, 0 disables)
- channel: Parameter name containing the channel (optional, defaults to current)
- reason: Reason for setting slowmode (optional)

**Function:** `slowmode_action`

**Usage:**
```yaml
actions:
  - type: "slowmode"
    # Add configuration parameters here
```

---

#### `unlock_all_channels`

**Description:** Unlock all text channels by restoring send_messages permission for @everyone role.

**Configuration:**
- reason: Reason for unlocking all channels (optional)
- exclude_channels: List of channel names/IDs to exclude from unlocking (optional)

**Function:** `unlock_all_channels_action`

**Usage:**
```yaml
actions:
  - type: "unlock_all_channels"
    # Add configuration parameters here
```

---

#### `unlock_channel`

**Description:** Unlock a channel by restoring send_messages permission for @everyone role.

**Configuration:**
- channel: Parameter name containing the channel to unlock (optional, defaults to current channel)
- reason: Reason for unlocking the channel (optional)

**Function:** `unlock_channel_action`

**Usage:**
```yaml
actions:
  - type: "unlock_channel"
    # Add configuration parameters here
```

---


### Message Actions

#### `dm`

**Function:** `dm_action`

**Usage:**
```yaml
actions:
  - type: "dm"
    # Add configuration parameters here
```

---

#### `edit_message`

**Function:** `edit_message_action`

**Usage:**
```yaml
actions:
  - type: "edit_message"
    # Add configuration parameters here
```

---

#### `message`

**Description:** Send a message to a channel.

**Configuration:**
- message: The message template to send (supports parameter substitution)
- channel: Optional channel to send to (parameter name or literal). Defaults to current channel.
- Args:
- message: Discord message object
- action_config: Action configuration dict
- param_values: Dictionary of parameter name -> value mappings
- command_name: Original command name
- raw_parameters: Original parameter list
- resource_sections: Resource sections for substitution
- last_bot_message: Last message sent by the bot

**Function:** `message_action`

**Usage:**
```yaml
actions:
  - type: "message"
    # Add configuration parameters here
```

---

#### `pin_message`

**Function:** `pin_message_action`

**Usage:**
```yaml
actions:
  - type: "pin_message"
    # Add configuration parameters here
```

---

#### `reply`

**Description:** Reply to the message with a formatted string.

**Configuration:**
- message: The message template to send (supports parameter substitution)
- Args:
- message: Discord message object
- action_config: Action configuration dict
- param_values: Dictionary of parameter name -> value mappings
- command_name: Original command name
- raw_parameters: Original parameter list
- resource_sections: Resource sections for substitution
- last_bot_message: Last message sent by the bot

**Function:** `reply_action`

**Usage:**
```yaml
actions:
  - type: "reply"
    # Add configuration parameters here
```

---

#### `unpin_message`

**Function:** `unpin_message_action`

**Usage:**
```yaml
actions:
  - type: "unpin_message"
    # Add configuration parameters here
```

---


### Moderation Actions

#### `ban`

**Description:** Ban a user from the server.

**Configuration:**
- user: Parameter name containing the user to ban (or direct reference)
- reason: Reason for the ban (optional)
- delete_message_days: Days of messages to delete (0-7, default: 1)

**Function:** `ban_action`

**Usage:**
```yaml
actions:
  - type: "ban"
    # Add configuration parameters here
```

---

#### `delete_message`

**Description:** Delete the original message that triggered the command.

**Configuration:**
- delay: Optional delay in seconds before deleting (default: 0)
- reason: Optional reason for the deletion (logged to console only)
- Args:
- message: Discord message object
- action_config: Action configuration dict
- param_values: Dictionary of parameter name -> value mappings
- command_name: Original command name
- raw_parameters: Original parameter list
- resource_sections: Resource sections for substitution
- last_bot_message: Last message sent by the bot

**Function:** `delete_message_action`

**Usage:**
```yaml
actions:
  - type: "delete_message"
    # Add configuration parameters here
```

---

#### `kick`

**Description:** Kick a user from the server.

**Configuration:**
- user: Parameter name containing the user to kick (or direct reference)
- reason: Reason for the kick (optional)

**Function:** `kick_action`

**Usage:**
```yaml
actions:
  - type: "kick"
    # Add configuration parameters here
```

---

#### `purge`

**Function:** `purge_action`

**Usage:**
```yaml
actions:
  - type: "purge"
    # Add configuration parameters here
```

---

#### `timeout`

**Function:** `timeout_action`

**Usage:**
```yaml
actions:
  - type: "timeout"
    # Add configuration parameters here
```

---

#### `unban`

**Description:** Unban a user from the server by user ID or username#discriminator.

**Configuration:**
- user: Parameter name containing the user ID or username#discriminator to unban
- reason: Reason for the unban (optional)

**Function:** `unban_action`

**Usage:**
```yaml
actions:
  - type: "unban"
    # Add configuration parameters here
```

---


### Role Actions

#### `cache_user_roles`

**Function:** `cache_user_roles_action`

**Usage:**
```yaml
actions:
  - type: "cache_user_roles"
    # Add configuration parameters here
```

---

#### `create_role`

**Description:** Create a new role in the server.

**Configuration:**
- name: Parameter name containing the role name
- color: Role color (hex code or color name, optional)
- hoist: Whether role should be displayed separately (optional, default: false)
- mentionable: Whether role can be mentioned (optional, default: false)
- reason: Reason for creating the role (optional)

**Function:** `create_role_action`

**Usage:**
```yaml
actions:
  - type: "create_role"
    # Add configuration parameters here
```

---

#### `delete_role`

**Description:** Delete a role from the server.

**Configuration:**
- role: Parameter name containing the role to delete (or direct reference)
- reason: Reason for deleting the role (optional)

**Function:** `delete_role_action`

**Usage:**
```yaml
actions:
  - type: "delete_role"
    # Add configuration parameters here
```

---

#### `enforce_sticky_roles`

**Description:** Enforce sticky roles for a user (typically called when they join the server). This action is silent and logs results instead of sending user messages.

**Configuration:**
- user: Parameter name containing the user/member to enforce sticky roles for

**Function:** `enforce_sticky_roles_action`

**Usage:**
```yaml
actions:
  - type: "enforce_sticky_roles"
    # Add configuration parameters here
```

---

#### `force_sticky`

**Description:** Force apply all sticky roles for a user (testing and enforcement purposes).

**Configuration:**
- user: Parameter name containing the user to force sticky roles for (or direct reference)

**Function:** `force_sticky_action`

**Usage:**
```yaml
actions:
  - type: "force_sticky"
    # Add configuration parameters here
```

---

#### `rem_role`

**Function:** `rem_role_action`

**Usage:**
```yaml
actions:
  - type: "rem_role"
    # Add configuration parameters here
```

---

#### `remove_user_roles`

**Function:** `remove_user_roles_action`

**Usage:**
```yaml
actions:
  - type: "remove_user_roles"
    # Add configuration parameters here
```

---

#### `restore_cached_roles`

**Function:** `restore_cached_roles_action`

**Usage:**
```yaml
actions:
  - type: "restore_cached_roles"
    # Add configuration parameters here
```

---

#### `set_role`

**Function:** `set_role_action`

**Usage:**
```yaml
actions:
  - type: "set_role"
    # Add configuration parameters here
```

---

#### `show_sticky`

**Description:** Show all sticky roles configured for a user.

**Configuration:**
- user: Parameter name containing the user to show sticky roles for (or direct reference)

**Function:** `show_sticky_action`

**Usage:**
```yaml
actions:
  - type: "show_sticky"
    # Add configuration parameters here
```

---

#### `stickyrole_add`

**Function:** `stickyrole_add_action`

**Usage:**
```yaml
actions:
  - type: "stickyrole_add"
    # Add configuration parameters here
```

---

#### `stickyrole_rem`

**Function:** `stickyrole_rem_action`

**Usage:**
```yaml
actions:
  - type: "stickyrole_rem"
    # Add configuration parameters here
```

---


### Utility Actions

#### `action_flow`

**Description:** Execute a predefined action flow (sequence of actions).

**Configuration:**
- flow: Name of the action flow to execute
- Args:
- message: Discord message object
- action_config: Action configuration dict
- param_values: Dictionary of parameter name -> value mappings
- command_name: Original command name
- raw_parameters: Original parameter list
- resource_sections: Resource sections for substitution
- mongo_db: MongoDB database connection
- action_flows: Action flows dictionary
- last_bot_message: Last message sent by the bot (for targeting reactions, etc.)

**Function:** `action_flow_action`

**Usage:**
```yaml
actions:
  - type: "action_flow"
    # Add configuration parameters here
```

---

#### `add_trigger`

**Function:** `add_trigger_action`

**Usage:**
```yaml
actions:
  - type: "add_trigger"
    # Add configuration parameters here
```

---

#### `clear_delayed_roles`

**Function:** `clear_delayed_roles_action`

**Usage:**
```yaml
actions:
  - type: "clear_delayed_roles"
    # Add configuration parameters here
```

---

#### `commands_by_role`

**Function:** `commands_by_role_action`

**Usage:**
```yaml
actions:
  - type: "commands_by_role"
    # Add configuration parameters here
```

---

#### `del_trigger`

**Description:** Delete a regex trigger from the configuration.

**Configuration:**
- name: Parameter name containing the trigger name to delete

**Function:** `del_trigger_action`

**Usage:**
```yaml
actions:
  - type: "del_trigger"
    # Add configuration parameters here
```

---

#### `delay_queue`

**Function:** `delay_queue_action`

**Usage:**
```yaml
actions:
  - type: "delay_queue"
    # Add configuration parameters here
```

---

#### `get_modlogs`

**Description:** Retrieve and display moderation logs for a specific user in condensed format.

**Configuration:**
- user: Parameter name containing the user to get logs for (or direct reference)
- limit: Maximum number of logs to display (default: 10)
- Args:
- message: Discord message object
- action_config: Action configuration dict
- param_values: Dictionary of parameter name -> value mappings
- command_name: Original command name
- raw_parameters: Original parameter list
- resource_sections: Resource sections for substitution
- mongo_db: MongoDB database connection

**Function:** `get_modlogs_action`

**Usage:**
```yaml
actions:
  - type: "get_modlogs"
    # Add configuration parameters here
```

---

#### `get_user_nickname`

**Function:** `get_user_nickname_action`

**Usage:**
```yaml
actions:
  - type: "get_user_nickname"
    # Add configuration parameters here
```

---

#### `list_triggers`

**Description:** List all configured regex triggers and their actions.

**Configuration:**
- embed: Whether to use embed format (default: true)

**Function:** `list_triggers_action`

**Usage:**
```yaml
actions:
  - type: "list_triggers"
    # Add configuration parameters here
```

---

#### `log`

**Function:** `log_action`

**Usage:**
```yaml
actions:
  - type: "log"
    # Add configuration parameters here
```

---

#### `make_log`

**Function:** `make_log_action`

**Usage:**
```yaml
actions:
  - type: "make_log"
    # Add configuration parameters here
```

---

#### `my_commands`

**Function:** `my_commands_action`

**Usage:**
```yaml
actions:
  - type: "my_commands"
    # Add configuration parameters here
```

---

#### `ocr_to_text`

**Function:** `ocr_to_text_action`

**Usage:**
```yaml
actions:
  - type: "ocr_to_text"
    # Add configuration parameters here
```

---

#### `react`

**Function:** `react_action`

**Usage:**
```yaml
actions:
  - type: "react"
    # Add configuration parameters here
```

---

#### `show_pending_delays`

**Description:** Show all pending delayed actions in the database.

**Configuration:**
- limit: Maximum number of results to show (default: 20)
- embed: Whether to use embed format (default: true)

**Function:** `show_pending_delays_action`

**Usage:**
```yaml
actions:
  - type: "show_pending_delays"
    # Add configuration parameters here
```

---

#### `strip_emoji`

**Function:** `strip_emoji_action`

**Usage:**
```yaml
actions:
  - type: "strip_emoji"
    # Add configuration parameters here
```

---

#### `test_regex`

**Description:** Test a regex pattern against a test string using the same pipeline as triggers.

**Configuration:**
- pattern: Regex pattern to test (parameter name or literal string)
- test_string: String to test pattern against (parameter name or literal string)
- flags: Optional regex flags string (i=case insensitive, m=multiline, s=dotall)
- Returns:
- Sends a message with test results including match status and captured groups

**Function:** `test_regex_action`

**Usage:**
```yaml
actions:
  - type: "test_regex"
    # Add configuration parameters here
```

---

#### `text_replace`

**Function:** `text_replace_action`

**Usage:**
```yaml
actions:
  - type: "text_replace"
    # Add configuration parameters here
```

---

#### `url_encode`

**Function:** `url_encode_action`

**Usage:**
```yaml
actions:
  - type: "url_encode"
    # Add configuration parameters here
```

---

#### `whois`

**Description:** Display detailed information about a user.

**Configuration:**
- user: Parameter name containing the user to get info for (or direct reference)
- embed: Whether to use an embed (default: true)
- Args:
- message: Discord message object
- action_config: Action configuration dict
- param_values: Dictionary of parameter name -> value mappings
- command_name: Original command name
- raw_parameters: Original parameter list
- resource_sections: Resource sections for substitution
- last_bot_message: Last message sent by the bot

**Function:** `whois_action`

**Usage:**
```yaml
actions:
  - type: "whois"
    # Add configuration parameters here
```

---


### Other Actions

#### `conditional_chan_log`

**Function:** `conditional_chan_log_action`

**Usage:**
```yaml
actions:
  - type: "conditional_chan_log"
    # Add configuration parameters here
```

---

#### `list_action_flows`

**Description:** List all configured action flows.

**Configuration:**
- embed: Whether to use embed format (default: true)

**Function:** `list_action_flows_action`

**Usage:**
```yaml
actions:
  - type: "list_action_flows"
    # Add configuration parameters here
```

---

#### `list_pattern_groups`

**Description:** List all available pattern groups.

**Configuration:**
- embed: Whether to use embed format (default: true)

**Function:** `list_pattern_groups_action`

**Usage:**
```yaml
actions:
  - type: "list_pattern_groups"
    # Add configuration parameters here
```

---

#### `ocr_image_url`

**Function:** `ocr_image_url_action`

**Usage:**
```yaml
actions:
  - type: "ocr_image_url"
    # Add configuration parameters here
```

---

#### `show_action_flow`

**Description:** Show the detailed configuration of a specific action flow.

**Configuration:**
- flow_name: Parameter name containing the action flow name
- embed: Whether to use embed format (default: true)

**Function:** `show_action_flow_action`

**Usage:**
```yaml
actions:
  - type: "show_action_flow"
    # Add configuration parameters here
```

---

#### `show_pattern_group`

**Description:** Show the details of a specific pattern group.

**Configuration:**
- group_name: Parameter name containing the pattern group name
- embed: Whether to use embed format (default: true)

**Function:** `show_pattern_group_action`

**Usage:**
```yaml
actions:
  - type: "show_pattern_group"
    # Add configuration parameters here
```

---

#### `show_trigger`

**Description:** Show the detailed configuration of a specific trigger.

**Configuration:**
- trigger_name: Parameter name containing the trigger name
- embed: Whether to use embed format (default: true)

**Function:** `show_trigger_action`

**Usage:**
```yaml
actions:
  - type: "show_trigger"
    # Add configuration parameters here
```

---


## 🔧 Action Parameters

All action functions receive the following standardized parameters:

| Parameter | Type | Description |
|-----------|------|-------------|
| `message` | `nextcord.Message` | The Discord message that triggered the action |
| `action_config` | `dict` | Configuration for this specific action |
| `param_values` | `dict` | Resolved parameter values |
| `command_name` | `str` | Name of the command being executed |
| `raw_parameters` | `list` | Raw parameter list from user input |
| `resource_sections` | `dict` | Available resource sections ($images, $texts) |
| `mongo_db` | `Database` | MongoDB database instance |
| `action_flows` | `dict` | Available action flows |
| `last_bot_message` | `Message` | Last message sent by the bot (for chaining) |
| `bot_config` | `dict` | Bot configuration settings |
| `get_log_channel_func` | `function` | Function to get the log channel |
| `modular_commands` | `dict` | All available commands |
| `check_user_permissions_func` | `function` | Function to check user permissions |

## 📝 Action Configuration

Actions are configured in YAML with a consistent structure:

```yaml
actions:
  - type: "action_name"
    parameter1: "value1"
    parameter2: "{parameter_reference}"
    parameter3: "{$resource:section}"
```

### Parameter Resolution

- **Direct values**: `"literal string"` or `42`
- **Parameter references**: `"{parameter_name}"` - resolved from command parameters
- **Resource references**: `"{$images:section}"` - resolved from resource sections
- **User references**: `"{user}"`, `"{author}"`, `"{mention}"` - Discord user objects

## 🔄 Action Chaining

Actions can be chained together and reference previous actions:

```yaml
actions:
  - type: "message"
    message: "Processing request..."
  - type: "timeout"
    user: "{user}"
    duration: "10m"
  - type: "react"
    emoji: "✅"
    # This will react to the message from the first action
```

The `last_bot_message` parameter allows actions to reference the previous bot message for reactions, replies, or edits.

## 🛠️ Creating Custom Actions

To create a new action:

1. **Add the action function** in `command_actions.py`:
   ```python
   @action(name="my_custom_action")
   async def my_custom_action(message, action_config: dict, param_values: dict, ...):
       """
       Description of what this action does.
       
       Action config:
           parameter_name: Description of expected parameter
       """
       # Implementation here
       pass
   ```

2. **Add frontend schema** in `frontend/src/actionSchema.ts`:
   ```typescript
   my_custom_action: {
     name: 'my_custom_action',
     description: 'Description for the UI',
     category: 'utility',
     fields: [
       { name: 'parameter_name', type: 'string', required: true, description: '...' }
     ]
   }
   ```

3. **Use in configuration**:
   ```yaml
   actions:
     - type: "my_custom_action"
       parameter_name: "value"
   ```

## 🧪 Testing Actions

Actions can be tested using the development interface:

1. Start the development environment: `./run-dev.sh all`
2. Open the web interface: `http://localhost:3000`
3. Use the visual editor to create test commands with your actions
4. Test in Discord using the `?reload` command


---

*This documentation is automatically generated from the action registry. Last updated: 2025-10-03 03:32:31 UTC*
