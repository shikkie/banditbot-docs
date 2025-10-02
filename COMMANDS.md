# BanditBot Commands Documentation

*Generated automatically on 2025-10-02 01:10:36 UTC*

## Overview

BanditBot features a sophisticated modular command system with 30 commands, 7 triggers, 2 action flows, and 20 event handlers.

## 🤖 Bot Commands

### Command Reference

### `?add_trigger`

**Description:** Add a new regex trigger with specified actions

**Usage:** `?add_trigger <name> <pattern> <actions>`

**Parameters:**
- **name** (required) - *string* - Unique name for the trigger
- **pattern** (required) - *string* - Regex pattern to match against messages
- **actions** (required) - *string* - Collects all remaining arguments - Actions to execute (format: act:reply:"message" act:react:emoji act:delete_message act:flow:flow_name)

**Required Roles:** Moderator

**Actions:**
- **add_trigger**

---

### `?arrest`

**Description:** Temporarily detain a user by assigning the 'detained' role with a fun arrest message

**Usage:** `?arrest <user> [reason]`

**Parameters:**
- **user** (required) - *user* - The user to arrest/detain (mention, username, or user ID)
- **reason** (optional) - *string* - Collects all remaining arguments - Reason for the arrest (will be logged as part of the sticky role assignment)

**Required Roles:** Moderator, Admin

**Actions:**
- **stickyrole_add**: Apply role `detained`
- **delete_message**
- **message**: `{user.mention} just found out.`
- **message**: `{$images:arrest}`

---

### `?coon`

**Description:** Post a random cute raccoon image with accompanying text

**Usage:** `?coon`

**Permissions:** Available to all users

**Actions:**
- **message**: `OMG LOOK ITS [{$texts:coon}]({$images:coon})`
- **delete_message**

---

### `?defenestrate`

**Description:** Defenestrate (throw out the window) a user by assigning the 'detained' role

**Usage:** `?defenestrate <user> [reason]`

**Parameters:**
- **user** (required) - *user* - The user to defenestrate (mention, username, or user ID)
- **reason** (optional) - *string* - Collects all remaining arguments - Reason for the defenestration (will be logged as part of the sticky role assignment)

**Required Roles:** Moderator, Admin

**Actions:**
- **stickyrole_add**: Apply role `detained`
- **delete_message**
- **message**: `{user.mention} has been defenestrated. `
- **message**: `{$images:defenestrate}`

---

### `?del_trigger`

**Description:** Delete a regex trigger by name

**Usage:** `?del_trigger <name>`

**Parameters:**
- **name** (required) - *string* - Name of the trigger to delete

**Required Roles:** Moderator

**Actions:**
- **del_trigger**

---

### `?delay_msg`

**Description:** Schedule a delayed message to be sent to a channel

**Usage:** `?delay_msg <delay> [target_channel] <message>`

**Parameters:**
- **delay** (required) - *string* - Delay duration (e.g., '5m', '1h', '30s')
- **target_channel** (optional) - *channel* - Target channel (defaults to current channel)
- **message** (required) - *string* - Collects all remaining arguments - Message content to send after delay

**Required Roles:** Moderator, Admin

**Actions:**
- **delay_queue**
- **reply**: `⏰ Message scheduled to be sent in {delay} to {targ...`

---

### `?dn`

**Description:** A fun 'deez nuts' meme response command

**Usage:** `?dn`

**Permissions:** Available to all users

**Actions:**
- **reply**: `Hah! GOTEEEEEM`
- **message**: `https://obj.shitpost.sh/obj/9c4a3233-f091-408a-94b...`

---

### `?force_sticky`

**Description:** Force apply all sticky roles for a user (testing and enforcement)

**Usage:** `?force_sticky <user>`

**Parameters:**
- **user** (required) - *user* - User to enforce sticky roles for (mention, username, or user ID)

**Required Roles:** Moderator

**Actions:**
- **force_sticky**

---

### `?gtfo`

**Description:** Eject a user by assigning the 'detained' role with an ejection message

**Usage:** `?gtfo <user> [reason]`

**Parameters:**
- **user** (required) - *user* - The user to eject (mention, username, or user ID)
- **reason** (optional) - *string* - Collects all remaining arguments - Reason for the ejection (will be logged as part of the sticky role assignment)

**Required Roles:** Moderator, Admin

**Actions:**
- **stickyrole_add**: Apply role `detained`
- **delete_message**
- **message**: `{user.mention} has been ejected`
- **message**: `{$images:arrest}`

---

### `?joke`

**Description:** Tell a random dad joke from the bot's collection

**Usage:** `?joke`

**Permissions:** Available to all users

**Actions:**
- **message**: `📢 **Dad Joke Time!** {$texts:dadjokes}`
- **react**
- **delete_message**

---

### `?list_triggers`

**Description:** List all configured regex triggers and their actions

**Usage:** `?list_triggers`

**Required Roles:** Moderator

**Actions:**
- **list_triggers**

---

### `?lock`

**Description:** Lock a text channel by removing send message permissions for @everyone role

**Usage:** `?lock [target_channel] [reason]`

**Parameters:**
- **target_channel** (optional) - *channel* - The channel to lock (mention, name, or ID). If not provided, locks the current channel
- **reason** (optional) - *string* - Collects all remaining arguments - Reason for locking the channel (will be logged)

**Required Roles:** Moderator, Admin

**Actions:**
- **lock_channel**

---

### `?modlogs`

**Description:** View the moderation logs for a specific user

**Usage:** `?modlogs <user>`

**Parameters:**
- **user** (required) - *user* - The user to view moderation logs for (mention, username, or user ID)

**Required Roles:** Moderator

**Actions:**
- **get_modlogs**

---

### `?mute`

**Description:** Temporarily mute a user by caching their roles, removing all roles, applying 'muted' role, with automatic restoration after specified duration

**Usage:** `?mute <duration> <user> <reason>`

**Parameters:**
- **duration** (required) - *string* - Duration (e.g., "5m", "1h", "30s")
- **user** (required) - *user*
- **reason** (required) - *string* - Collects all remaining arguments

**Required Roles:** Moderator, Admin

**Actions:**
- **cache_user_roles**
- **remove_user_roles**
- **set_role**: Apply role `Muted`
- **delay_queue**
- **make_log**
- **reply**: `🔇 Muted {user} for {duration}. Their roles have be...`

---

### `?note`

**Description:** Add a moderation note for a user (creates a log entry without taking action)

**Usage:** `?note <user> <note>`

**Parameters:**
- **user** (required) - *user* - The user to add a moderation note for (mention, username, or user ID)
- **note** (required) - *string* - Collects all remaining arguments - The note content to add to the user's moderation history

**Required Roles:** Moderator

**Actions:**
- **make_log**
- **reply**: `✅ Note added for {user}: {note}`

---

### `?ocr`

**Description:** Extract text from images using OCR (Optical Character Recognition)

**Usage:** `?ocr`

**Permissions:** Available to all users

**Actions:**
- **ocr_to_text**

---

### `?pending_delays`

**Description:** Show pending delay queue actions

**Usage:** `?pending_delays`

**Required Roles:** Moderator, Admin

**Actions:**
- **show_pending_delays**

---

### `?purge`

**Description:** Bulk delete messages from the current channel (supports up to 999 messages + command message, with automatic batching)

**Usage:** `?purge <count> [reason]`

**Parameters:**
- **count** (required) - *integer* - Number of messages to delete BEFORE the command (1-999). The command message is automatically included, so total deletion will be count+1. For counts >100, the bot will automatically batch the operation
- **reason** (optional) - *string* - Collects all remaining arguments - Reason for the bulk deletion (will be logged in moderation logs)

**Required Roles:** Moderator

**Actions:**
- **purge**

---

### `?release`

**Description:** Release a user by removing the 'detained' role

**Usage:** `?release <user> [reason]`

**Parameters:**
- **user** (required) - *user* - The user to release (mention, username, or user ID)
- **reason** (optional) - *string* - Collects all remaining arguments - Reason for the release (will be logged)

**Required Roles:** Moderator

**Actions:**
- **stickyrole_rem**: Apply role `detained`
- **reply**: `✅ {user.mention} has been released from detention....`

---

### `?show_sticky`

**Description:** Show what sticky roles a user has configured

**Usage:** `?show_sticky <user>`

**Parameters:**
- **user** (required) - *user* - User to show sticky roles for (mention, username, or user ID)

**Required Roles:** Moderator

**Actions:**
- **show_sticky**

---

### `?stickyrole`

**Description:** Assign a role to a user that will be automatically re-assigned if they leave and rejoin the server

**Usage:** `?stickyrole <role> <user> [reason]`

**Parameters:**
- **role** (required) - *role* - The role to assign as a sticky role (mention, role name, or role ID)
- **user** (required) - *user* - The user to assign the sticky role to (mention, username, or user ID)
- **reason** (optional) - *string* - Collects all remaining arguments - Optional reason for assigning the sticky role (will be logged)

**Permissions:** Available to all users

**Actions:**
- **stickyrole_add**: Apply role `{role}`
- **reply**: `✅ Sticky role assigned: {role} to {user}`
- **log**: `User {user} was given the sticky role {role}`

---

### `?stickyrole_rem`

**Description:** Remove a sticky role from a user and delete it from the database

**Usage:** `?stickyrole_rem <role> <user> [reason]`

**Parameters:**
- **role** (required) - *role* - The sticky role to remove (mention, role name, or role ID)
- **user** (required) - *user* - The user to remove the sticky role from (mention, username, or user ID)
- **reason** (optional) - *string* - Collects all remaining arguments - Optional reason for removing the sticky role (will be logged)

**Permissions:** Available to all users

**Actions:**
- **stickyrole_rem**: Apply role `{role}`
- **reply**: `✅ Sticky role removed: {role} from {user}`
- **log**: `User {user} had the sticky role {role} removed`

---

### `?thotbgone`

**Description:** Detain a user with a thot-be-gone themed message and assign the 'detained' role

**Usage:** `?thotbgone <user> [reason]`

**Parameters:**
- **user** (required) - *user* - The user to detain (mention, username, or user ID)
- **reason** (optional) - *string* - Collects all remaining arguments - Reason for the detention (will be logged as part of the sticky role assignment)

**Required Roles:** Moderator, Admin

**Actions:**
- **stickyrole_add**: Apply role `detained`
- **delete_message**
- **message**: `{user.mention} has been invited for a ride in the ...`
- **message**: `{$images:thotbgone}`

---

### `?timeout`

**Description:** Timeout a user for a specified duration using Discord's built-in timeout feature

**Usage:** `?timeout <duration> <user> [reason]`

**Parameters:**
- **duration** (required) - *string* - Duration of the timeout (e.g., 5m for 5 minutes, 1h for 1 hour, 2d for 2 days). Supports s/m/h/d units
- **user** (required) - *user* - The user to timeout (mention, username, or user ID)
- **reason** (optional) - *string* - Collects all remaining arguments - Reason for the timeout (will be shown in Discord's audit log and stored in moderation logs)

**Required Roles:** Moderator, Admin

**Actions:**
- **timeout**: Duration `{duration}`
- **reply**: `⏰ {user} has been timed out for {duration}. Reason...`
- **delete_message**

---

### `?touch`

**Description:** A simple test command to verify the bot is responding

**Usage:** `?touch`

**Permissions:** Available to all users

**Actions:**
- **reply**: `👋 Touch command received! Hello, {author}!`

---

### `?unlock`

**Description:** Unlock a text channel by restoring send message permissions for @everyone role

**Usage:** `?unlock [target_channel] [reason]`

**Parameters:**
- **target_channel** (optional) - *channel* - The channel to unlock (mention, name, or ID). If not provided, unlocks the current channel
- **reason** (optional) - *string* - Collects all remaining arguments - Reason for unlocking the channel (will be logged)

**Required Roles:** Moderator, Admin

**Actions:**
- **unlock_channel**

---

### `?unlock_all`

**Description:** Unlock all channels in the server by restoring @everyone send message permissions

**Usage:** `?unlock_all [reason]`

**Parameters:**
- **reason** (optional) - *string* - Collects all remaining arguments - Reason for unlocking all channels (will be logged)

**Required Roles:** Moderator, Admin

**Actions:**
- **unlock_all_channels**

---

### `?unmute`

**Description:** Immediately unmute a user by restoring their cached roles and clearing any pending delayed actions

**Usage:** `?unmute <user> [reason]`

**Parameters:**
- **user** (required) - *user* - The user to unmute (mention, username, or user ID)
- **reason** (optional) - *string* - Collects all remaining arguments - Reason for unmuting (will be logged)

**Required Roles:** Moderator, Admin

**Actions:**
- **clear_delayed_roles**
- **restore_cached_roles**
- **make_log**
- **reply**: `🔊 Unmuted {user}. Their roles have been restored a...`

---

### `?whois`

**Description:** Display detailed information about a user including roles, join date, account age, and permissions

**Usage:** `?whois <user>`

**Parameters:**
- **user** (required) - *user* - The user to get detailed information about (mention, username, or user ID)

**Permissions:** Available to all users

**Actions:**
- **whois**

---

### `?worm`

**Description:** Post a cute worm image with descriptive text

**Usage:** `?worm`

**Permissions:** Available to all users

**Actions:**
- **message**: `look at this cute [{$texts:worm}]({$images:worm}) `

---

## 🎯 Automatic Triggers

These triggers automatically respond to message patterns:

### bigfoot_trigger

**Pattern:** `\bbigfoot\b`

**Description:** Responds when someone mentions Bigfoot

**Actions:**
- **react**

---

### chloe_trigger

**Pattern:** `\bchloe\b`

**Description:** Responds when someone mentions Chloe

**Actions:**
- **react**

---

### null_mention_gay_react

**Pattern:** `<@!?100418001488064512>`

**Description:** Reacts with GAY when specific user is mentioned

**Actions:**
- **react**
- **react**
- **react**

---

### simon

**Pattern:** `\bsimon\b`

**Description:** Responds when someone mentions simon

**Actions:**
- **reply**: `simon!!!!!`
- **react**

---

### zoomer_fr

**Pattern:** `\bfr\b`

**Description:** Detects 'fr' zoomer slang from unverified users

**Required Roles:** unverified

**Actions:**
- **action_flow**

---

### zoomer_no_cap

**Pattern:** `\bno cap\b`

**Description:** Detects 'no cap' zoomer slang from unverified users

**Required Roles:** unverified

**Actions:**
- **action_flow**

---

### zoomer_wsp

**Pattern:** `\bwsp\b`

**Description:** Detects 'wsp' zoomer slang from unverified users

**Required Roles:** unverified

**Actions:**
- **action_flow**

---

## 🔄 Action Flows

Reusable action sequences:

### ram

**Description:** Ramming action for zoomer slang violations

**Actions:**
- **message**: `❌ {author_mention}, please use proper English. Zoo...`
- **timeout**: Duration `30s`
- **delete_message**

---

### warning_flow

**Description:** Standard warning flow for various infractions

**Actions:**
- **reply**: `⚠️ {author_mention}, this is a warning. Please rev...`
- **make_log**

---

## 📡 Event Handlers

Automatic responses to Discord events:

### on_bulk_message_delete

**Description:** Log bulk message deletions to the configured log channel

**Actions:**
- **chan_log**

---

### on_guild_channel_create

**Description:** Log channel creation to the configured log channel

**Actions:**
- **chan_log**

---

### on_guild_channel_delete

**Description:** Log channel deletion to the configured log channel

**Actions:**
- **chan_log**

---

### on_guild_channel_update

**Description:** Log channel updates to the configured log channel

**Actions:**
- **chan_log**

---

### on_guild_emojis_update

**Description:** Log emoji changes to the configured log channel

**Actions:**
- **chan_log**

---

### on_guild_role_create

**Description:** Log role creation to the configured log channel

**Actions:**
- **chan_log**

---

### on_guild_role_delete

**Description:** Log role deletion to the configured log channel

**Actions:**
- **chan_log**

---

### on_guild_role_update

**Description:** Log role updates to the configured log channel

**Actions:**
- **chan_log**

---

### on_image

**Description:** Process images with OCR and log extracted text

**Actions:**
- **ocr_image_url**
- **conditional_chan_log**

---

### on_invite_create

**Description:** Log invite creation to the configured log channel

**Actions:**
- **chan_log**

---

### on_invite_delete

**Description:** Log invite deletion to the configured log channel

**Actions:**
- **chan_log**

---

### on_member_ban

**Description:** Log member bans to the configured log channel

**Actions:**
- **chan_log**

---

### on_member_join

**Description:** Handle new members joining the server

**Actions:**
- **set_role**: Apply role `Unverified`
- **enforce_sticky_roles**
- **chan_log**

---

### on_member_remove

**Description:** Log members leaving the server

**Actions:**
- **chan_log**

---

### on_member_unban

**Description:** Log member unbans to the configured log channel

**Actions:**
- **chan_log**

---

### on_member_update

**Description:** Log member role changes and nickname changes

**Actions:**
- **chan_log**

---

### on_message_delete

**Description:** Log deleted messages to the configured log channel

**Actions:**
- **chan_log**

---

### on_message_edit

**Description:** Log edited messages to the configured log channel

**Actions:**
- **chan_log**

---

### on_ready

**Description:** Log when the bot comes online

**Actions:**
- **chan_log**

---

### on_voice_state_update

**Description:** Log voice channel activity to the configured log channel

**Actions:**
- **chan_log**

---

## ⚙️ Bot Configuration

- **log_channel:** `bandit-logs`
- **show_commands_on_unknown:** `False`


## 📞 Support

For more information about using BanditBot:

- **Configuration:** Use the web interface at `http://localhost:3000`
- **API Documentation:** See `docs/API.md`
- **Action Reference:** See `docs/ACTIONS.md`
- **User Guide:** See `docs/USER_GUIDE.md`

---

*This documentation is automatically generated from `modular_commands.yml`. Last updated: 2025-10-02 01:10:36 UTC*
