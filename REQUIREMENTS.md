# BanditBot Software Requirements Specification

**Generated:** 2025-10-03 22:27:19 UTC

**Version:** Inferred from codebase analysis

---

## Table of Contents

1. [Introduction](#introduction)
2. [System Architecture](#system-architecture)
3. [Functional Requirements](#functional-requirements)
4. [Discord Bot Requirements](#discord-bot-requirements)
5. [API Requirements](#api-requirements)
6. [Data Management Requirements](#data-management-requirements)
7. [Non-Functional Requirements](#non-functional-requirements)
8. [Integration Requirements](#integration-requirements)

---

## Introduction

BanditBot is a sophisticated Discord bot with modular command system, web-based 
configuration interface, and comprehensive logging capabilities. This document 
specifies the software requirements inferred from the codebase analysis.

## System Architecture

### AR-001: Multi-Component Architecture
The system SHALL consist of the following components:

- **Discord Bot**: Core bot application with command processing
- **Web API**: RESTful Flask API for configuration and data management
- **Frontend**: React-based visual configuration interface
- **Database**: MongoDB for persistent data storage

### AR-002: Configuration-Driven Design
The system SHALL use YAML configuration files to define:

- Bot commands with parameters and validation
- Regex triggers for message pattern matching
- Action sequences and workflows
- Event handlers for Discord events
- Resource collections (images, text templates)

## Functional Requirements

### FR-CMD: Command System (62 commands)

#### FR-CMD-001: Modular Command Processing
The system SHALL process commands defined in YAML configuration with:

- Dynamic command registration from configuration
- Parameter parsing and validation
- Role-based access control
- Action sequence execution

#### FR-CMD-002: Command Categories

The system SHALL support the following command categories:

- **Moderation Commands** (7): `arrest`, `ban`, `kick`, `mute`, `timeout`
- **Role Management** (6): `createrole`, `deleterole`, `force_sticky`, `show_sticky`, `stickyrole`
- **Utility Commands** (49): `add_social_monitor`, `add_trigger`, `analyze_document`, `analyze_receipt`, `coon`

### FR-TRG: Regex Trigger System (7 triggers)

#### FR-TRG-001: Pattern Matching
The system SHALL support regex-based message triggers with:

- Regular expression pattern matching
- Capture group extraction and parameter substitution
- Pattern group reuse for common patterns
- Role-based trigger filtering

#### FR-TRG-002: Configured Triggers

Active triggers include:

- **bigfoot_trigger**: Responds when someone mentions Bigfoot
- **chloe_trigger**: Responds when someone mentions Chloe
- **instagram_rewriter**: Rewrites Instagram links to ddinstagram.com for better viewing
- **null_mention_gay_react**: Reacts with GAY when specific user is mentioned
- **simon**: Responds when someone mentions simon
- **twitter_rewriter**: Rewrites Twitter/X links to vxtwitter.com for better viewing
- **zoomer_slang**: Detects zoomer slang from unverified users using pattern group

### FR-EVT: Event Handling (26 event handlers)

#### FR-EVT-001: Discord Event Processing
The system SHALL handle Discord events with configurable actions:

- **on_bulk_message_delete**: Log bulk message deletions to the configured log channel
- **on_guild_channel_create**: Log channel creation to the configured log channel
- **on_guild_channel_delete**: Log channel deletion to the configured log channel
- **on_guild_channel_update**: Log channel updates to the configured log channel
- **on_guild_emojis_update**: Log emoji changes to the configured log channel
- **on_guild_role_create**: Log role creation to the configured log channel
- **on_guild_role_delete**: Log role deletion to the configured log channel
- **on_guild_role_update**: Log role updates to the configured log channel
- **on_guild_update**: Log server settings changes to the configured log channel
- **on_image**: Process images with OCR and log extracted text
- **on_invite_create**: Log invite creation to the configured log channel
- **on_invite_delete**: Log invite deletion to the configured log channel
- **on_member_ban**: Log member bans to the configured log channel
- **on_member_join**: Handle new members joining the server
- **on_member_remove**: Log members leaving the server
- *(and 11 more)*

### FR-ACT: Action System (69 actions)

#### FR-ACT-001: Action Registry
The system SHALL provide a registry-based action system with decorated handlers.

#### FR-ACT-002: Available Actions

The system SHALL support the following action types:

**Message Actions**:
- `reply`: Action handler: reply_action
- `message`: Action handler: message_action
- `dm`: Action handler: dm_action
- `log`: Action handler: log_action
- `delete_message`: Action handler: delete_message_action

**Role Actions**:
- `stickyrole_add`: Action handler: stickyrole_add_action
- `stickyrole_rem`: Action handler: stickyrole_rem_action
- `set_role`: Action handler: set_role_action
- `rem_role`: Action handler: rem_role_action
- `cache_user_roles`: Action handler: cache_user_roles_action
- `restore_cached_roles`: Action handler: restore_cached_roles_action

**Moderation Actions**:
- `get_modlogs`: Action handler: get_modlogs_action
- `make_log`: Action handler: make_log_action
- `timeout`: Action handler: timeout_action

**Utility Actions**:
- `react`: Action handler: react_action
- `action_flow`: Action handler: action_flow_action

**Other Actions**:
- `add_social_monitor`: Action handler: add_social_monitor_action
- `add_trigger`: Action handler: add_trigger_action
- `ban`: Action handler: ban_action
- `chan_log`: Action handler: chan_log_action
- `check_reaction_threshold`: Action handler: check_reaction_threshold_action
- `clear_delayed_roles`: Action handler: clear_delayed_roles_action
- `commands_by_role`: Action handler: commands_by_role_action
- `conditional_chan_log`: Action handler: conditional_chan_log_action
- `create_role`: Action handler: create_role_action
- `del_trigger`: Action handler: del_trigger_action
- *(and 43 more)*

## API Requirements

### FR-API: RESTful API (32 endpoints)

#### FR-API-001: HTTP API Server
The system SHALL provide a Flask-based REST API with:

- CORS-enabled endpoints for web frontend
- JSON request/response format
- Error handling with appropriate HTTP status codes
- MongoDB integration for data operations

#### FR-API-002: API Endpoints

**Configuration Endpoints**:
- `GET /api/config/yaml`: Get the current YAML configuration.
- `PUT /api/config/yaml`: Update the YAML configuration.
- `POST /api/config/yaml/validate`: Validate YAML configuration without saving.
- `GET /api/config/backups`: List available configuration backup files.
- `GET /api/config/backups/<filename>`: Get a specific configuration backup.

**Data Management Endpoints**:
- `GET /api/stickyroles`: Get all sticky roles with optional filtering.
- `POST /api/stickyroles`: Create a new sticky role.
- `GET /api/stickyroles/<doc_id>`: Get a specific sticky role by ID.
- `PUT /api/stickyroles/<doc_id>`: Update a specific sticky role by ID.
- `DELETE /api/stickyroles/<doc_id>`: Delete a specific sticky role by ID.
- `GET /api/modlogs`: Get all mod logs with optional filtering.
- `POST /api/modlogs`: Create a new mod log entry.
- `GET /api/modlogs/<doc_id>`: Get a specific mod log by ID.
- `PUT /api/modlogs/<doc_id>`: Update a specific mod log by ID.
- `DELETE /api/modlogs/<doc_id>`: Delete a specific mod log by ID.
- *(and 7 more)*

**Utility Endpoints**:
- `GET /api/health`: Health check endpoint.
- `GET /api/schema`: Get the MongoDB schema definitions.
- `POST /api/reload`: Trigger bot configuration reload via signal file.
- `POST /api/regex-test`: Test regex patterns against test strings.
- `GET /api/discord/guilds`: Get list of guilds the bot is in.
- `GET /api/discord/guilds/<guild_id>/members`: Get members from a specific guild with optional search.
- `GET /api/discord/guilds/<guild_id>/roles`: Get roles from a specific guild.
- `GET /api/discord/guilds/<guild_id>/channels`: Get channels from a specific guild.
- `GET /api/discord/users/<user_id>`: Get information about a specific user.
- `GET /api/discord/guilds/<guild_id>/members/<user_id>`: Get information about a specific member in a guild.

## Data Management Requirements

### FR-DB: Database Schema (5 collections)

#### FR-DB-001: MongoDB Storage
The system SHALL use MongoDB for persistent storage with:

- Schema validation using JSON Schema
- BSON datetime objects for timestamps
- 64-bit integers for Discord IDs
- Capped collections for log data
- TTL indexes for automatic cleanup

#### FR-DB-002: Data Collections

**stickyRoles**

Fields:
- `user` (long): Discord user ID as 64-bit integer
- `role` (long): Discord role ID as 64-bit integer
- `guild` (long): Discord guild ID as 64-bit integer

Required fields: `user`, `role`, `guild`

**modLogs**

Fields:
- `guild` (long): Discord guild ID as 64-bit integer
- `moderator` (long): Discord moderator user ID as 64-bit integer
- `user` (long): Discord user ID that the action was performed on
- `timestamp` (date): BSON date of when action occurred
- `action` (string): Action performed by moderator
- `reason` (string): Optional reason for the action

Required fields: `guild`, `moderator`, `user`, `timestamp`, `action`

**delayQueue**

Fields:
- `guild` (long): Discord guild ID as 64-bit integer
- `executeAt` (date): BSON date of when to execute the action chain
- `actionChain` (array): Array of actions to execute when delay expires
- `paramValues` (object): Expanded parameter values for action execution
- `createdAt` (date): BSON date of when this delay was created
- `createdBy` (long): Discord user ID who created this delayed action
- `commandName` (string): Original command name that created this delay
- `executed` (boolean): Whether this delayed action has been executed

Required fields: `guild`, `executeAt`, `actionChain`, `paramValues`, `createdAt`, `createdBy`, `executed`

**roleCaches**

Fields:
- `cache_id` (string): Unique identifier for this role cache
- `user_id` (long): Discord user ID as 64-bit integer
- `guild_id` (long): Discord guild ID as 64-bit integer
- `cached_roles` (array): Array of cached role data
- `cached_at` (date): BSON date of when roles were cached
- `created_by` (long): Discord user ID who created this cache as 64-bit integer
- `reason` (string): Reason for caching (usually command name)

Required fields: `cache_id`, `user_id`, `guild_id`, `cached_roles`, `cached_at`

**socialMediaMonitors**

Fields:
- `monitor_id` (string): Unique identifier for this monitor (e.g., 'rss_blog', 'youtube_channel123')
- `monitor_type` (string): Type of social media monitor
- `guild` (long): Discord guild ID as 64-bit integer
- `channel` (long): Discord channel ID where posts should be sent as 64-bit integer
- `source_url` (string): Source URL (RSS feed URL, YouTube channel ID, Twitter handle)
- `last_check` (date): BSON date of last check
- `last_post_id` (string): ID of the last post that was processed
- `last_post_date` (date): BSON date of last post that was processed
- `enabled` (boolean): Whether this monitor is currently active
- `created_at` (date): BSON date when monitor was created
- *(and 2 more fields)*

Required fields: `monitor_id`, `monitor_type`, `guild`, `channel`, `source_url`, `enabled`, `created_at`

## Non-Functional Requirements

### NFR-001: Performance
- The bot SHALL process commands within 1 second under normal load
- The API SHALL respond to requests within 200ms for data operations
- The system SHALL support multiple concurrent users without degradation

### NFR-002: Reliability
- The bot SHALL automatically reconnect to Discord on connection loss
- The system SHALL handle MongoDB connection failures gracefully
- Configuration errors SHALL be detected and logged without crashing

### NFR-003: Security
- Command access SHALL be controlled via Discord role permissions
- API SHALL validate all input data before processing
- Sensitive data SHALL NOT be logged or exposed via API
- Discord tokens SHALL be stored in environment variables

### NFR-004: Maintainability
- The system SHALL support hot-reload of configuration without restart
- Actions SHALL be registered via decorators for easy extension
- Code SHALL follow type annotation standards for clarity
- Logging SHALL use structured subsystem with multiple destinations

### NFR-005: Scalability
- The database SHALL use capped collections to prevent unbounded growth
- Log data SHALL expire automatically via TTL indexes
- The system SHALL support Docker deployment for easy scaling

## Integration Requirements

### INT-001: Discord Integration
The system SHALL integrate with Discord API for:

- Message sending and receiving
- Role management and permissions
- User and member information
- Channel and guild operations
- Reactions and embeds
- Event notifications (member join/leave, role changes, etc.)

### INT-002: Frontend Integration
The system SHALL provide a React frontend that:

- Connects to Flask API for data operations
- Provides visual YAML configuration editor
- Validates configuration in real-time
- Manages backups and restore operations
- Generates and displays help documentation

### INT-003: Docker Integration
The system SHALL support containerized deployment via:

- Multi-container Docker Compose setup
- Separate containers for bot, API, frontend, and database
- Volume mounts for configuration and logs
- Health checks and automatic restarts
- Resource limits for production deployment

---

## Requirements Summary

| Category | Count |
|----------|-------|
| Discord Commands | 62 |
| Regex Triggers | 7 |
| Event Handlers | 26 |
| Action Types | 69 |
| API Endpoints | 32 |
| Database Collections | 5 |

---

*This document was automatically generated from codebase analysis. 
For implementation details, refer to the source code and technical documentation.*
