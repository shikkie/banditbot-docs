# BanditBot API Documentation

*Generated automatically on 2025-10-04 05:30:40 UTC*

## Overview

The BanditBot API provides programmatic access to bot configuration, moderation logs, and system management. The API is built with Flask and provides JSON responses.

**Base URL:** `http://localhost:5000/api`

## 🔗 API Endpoints

### Configuration Management

#### `GET /api/config/yaml`

**Description:** Get the current YAML configuration.

**Example:**
```bash
curl -X GET http://localhost:5000/api/config/yaml
```

---

#### `PUT /api/config/yaml`

**Description:** Update the YAML configuration.

---

#### `POST /api/config/yaml/validate`

**Description:** Validate YAML configuration without saving.

**Example:**
```bash
curl -X POST http://localhost:5000/api/config/yaml/validate \\
  -H "Content-Type: application/json" \\
  -d '{"key": "value"}'
```

---

#### `GET /api/config/backups`

**Description:** List available configuration backup files.

**Example:**
```bash
curl -X GET http://localhost:5000/api/config/backups
```

---

#### `GET /api/config/backups/<filename>`

**Description:** Get a specific configuration backup.

**Example:**
```bash
curl -X GET http://localhost:5000/api/config/backups/<filename>
```

---

### Data Management

#### `GET /api/stickyroles`

**Description:** Get all sticky roles with optional filtering.

**Example:**
```bash
curl -X GET http://localhost:5000/api/stickyroles
```

---

#### `POST /api/stickyroles`

**Description:** Create a new sticky role.

**Example:**
```bash
curl -X POST http://localhost:5000/api/stickyroles \\
  -H "Content-Type: application/json" \\
  -d '{"key": "value"}'
```

---

#### `GET /api/stickyroles/<doc_id>`

**Description:** Get a specific sticky role by ID.

**Example:**
```bash
curl -X GET http://localhost:5000/api/stickyroles/<doc_id>
```

---

#### `PUT /api/stickyroles/<doc_id>`

**Description:** Update a specific sticky role by ID.

---

#### `DELETE /api/stickyroles/<doc_id>`

**Description:** Delete a specific sticky role by ID.

---

#### `GET /api/modlogs`

**Description:** Get all mod logs with optional filtering.

**Example:**
```bash
curl -X GET http://localhost:5000/api/modlogs
```

---

#### `POST /api/modlogs`

**Description:** Create a new mod log entry.

**Example:**
```bash
curl -X POST http://localhost:5000/api/modlogs \\
  -H "Content-Type: application/json" \\
  -d '{"key": "value"}'
```

---

#### `GET /api/modlogs/<doc_id>`

**Description:** Get a specific mod log by ID.

**Example:**
```bash
curl -X GET http://localhost:5000/api/modlogs/<doc_id>
```

---

#### `PUT /api/modlogs/<doc_id>`

**Description:** Update a specific mod log by ID.

---

#### `DELETE /api/modlogs/<doc_id>`

**Description:** Delete a specific mod log by ID.

---

#### `GET /api/delayqueue`

**Description:** Get all delay queue entries with optional filtering.

**Example:**
```bash
curl -X GET http://localhost:5000/api/delayqueue
```

---

#### `GET /api/delayqueue/<doc_id>`

**Description:** Get a specific delay queue entry by ID.

**Example:**
```bash
curl -X GET http://localhost:5000/api/delayqueue/<doc_id>
```

---

#### `DELETE /api/delayqueue/<doc_id>`

**Description:** Delete a specific delay queue entry by ID.

---

#### `POST /api/delayqueue/<doc_id>/execute`

**Description:** Schedule a delay queue entry for immediate execution by setting executeAt to current time.

**Example:**
```bash
curl -X POST http://localhost:5000/api/delayqueue/<doc_id>/execute \\
  -H "Content-Type: application/json" \\
  -d '{"key": "value"}'
```

---

### System Management

#### `GET /api/health`

**Description:** Health check endpoint.

**Example:**
```bash
curl -X GET http://localhost:5000/api/health
```

---

#### `GET /api/schema`

**Description:** Get the MongoDB schema definitions.

**Example:**
```bash
curl -X GET http://localhost:5000/api/schema
```

---

#### `GET /api/rolecaches`

**Description:** Get all role caches with optional filtering and pagination.

**Example:**
```bash
curl -X GET http://localhost:5000/api/rolecaches
```

---

#### `GET /api/rolecaches/<doc_id>`

**Description:** Get a specific role cache by ID.

**Example:**
```bash
curl -X GET http://localhost:5000/api/rolecaches/<doc_id>
```

---

#### `DELETE /api/rolecaches/<doc_id>`

**Description:** Delete a specific role cache by ID.

---

#### `POST /api/reload`

**Description:** Trigger bot configuration reload via signal file.

**Example:**
```bash
curl -X POST http://localhost:5000/api/reload \\
  -H "Content-Type: application/json" \\
  -d '{"key": "value"}'
```

---

#### `POST /api/regex-test`

**Description:** Test regex patterns against test strings.

**Example:**
```bash
curl -X POST http://localhost:5000/api/regex-test \\
  -H "Content-Type: application/json" \\
  -d '{"key": "value"}'
```

---

#### `GET /api/discord/guilds`

**Description:** Get list of guilds the bot is in.

**Example:**
```bash
curl -X GET http://localhost:5000/api/discord/guilds
```

---

#### `GET /api/discord/guilds/<guild_id>/members`

**Description:** Get members from a specific guild with optional search.

**Example:**
```bash
curl -X GET http://localhost:5000/api/discord/guilds/<guild_id>/members
```

---

#### `GET /api/discord/guilds/<guild_id>/roles`

**Description:** Get roles from a specific guild.

**Example:**
```bash
curl -X GET http://localhost:5000/api/discord/guilds/<guild_id>/roles
```

---

#### `GET /api/discord/guilds/<guild_id>/channels`

**Description:** Get channels from a specific guild.

**Example:**
```bash
curl -X GET http://localhost:5000/api/discord/guilds/<guild_id>/channels
```

---

#### `GET /api/discord/users/<user_id>`

**Description:** Get information about a specific user.

**Example:**
```bash
curl -X GET http://localhost:5000/api/discord/users/<user_id>
```

---

#### `GET /api/discord/guilds/<guild_id>/members/<user_id>`

**Description:** Get information about a specific member in a guild.

**Example:**
```bash
curl -X GET http://localhost:5000/api/discord/guilds/<guild_id>/members/<user_id>
```

---

## 📊 Database Schema

The following collections are used by BanditBot:

### stickyRoles

**Fields:**

- **user** (*long*): Discord user ID as 64-bit integer
- **role** (*long*): Discord role ID as 64-bit integer
- **guild** (*long*): Discord guild ID as 64-bit integer

**Required Fields:** user, role, guild

---

### modLogs

**Fields:**

- **guild** (*long*): Discord guild ID as 64-bit integer
- **moderator** (*long*): Discord moderator user ID as 64-bit integer
- **user** (*long*): Discord user ID that the action was performed on
- **timestamp** (*date*): BSON date of when action occurred
- **action** (*string*): Action performed by moderator
- **reason** (*string*): Optional reason for the action

**Required Fields:** guild, moderator, user, timestamp, action

---

### delayQueue

**Fields:**

- **guild** (*long*): Discord guild ID as 64-bit integer
- **executeAt** (*date*): BSON date of when to execute the action chain
- **actionChain** (*array*): Array of actions to execute when delay expires
- **paramValues** (*object*): Expanded parameter values for action execution
- **createdAt** (*date*): BSON date of when this delay was created
- **createdBy** (*long*): Discord user ID who created this delayed action
- **commandName** (*string*): Original command name that created this delay
- **executed** (*boolean*): Whether this delayed action has been executed

**Required Fields:** guild, executeAt, actionChain, paramValues, createdAt, createdBy, executed

---

### roleCaches

**Fields:**

- **cache_id** (*string*): Unique identifier for this role cache
- **user_id** (*long*): Discord user ID as 64-bit integer
- **guild_id** (*long*): Discord guild ID as 64-bit integer
- **cached_roles** (*array*): Array of cached role data
- **cached_at** (*date*): BSON date of when roles were cached
- **created_by** (*long*): Discord user ID who created this cache as 64-bit integer
- **reason** (*string*): Reason for caching (usually command name)

**Required Fields:** cache_id, user_id, guild_id, cached_roles, cached_at

---

### socialMediaMonitors

**Fields:**

- **monitor_id** (*string*): Unique identifier for this monitor (e.g., 'rss_blog', 'youtube_channel123')
- **monitor_type** (*string*): Type of social media monitor
- **guild** (*long*): Discord guild ID as 64-bit integer
- **channel** (*long*): Discord channel ID where posts should be sent as 64-bit integer
- **source_url** (*string*): Source URL (RSS feed URL, YouTube channel ID, Twitter handle)
- **last_check** (*date*): BSON date of last check
- **last_post_id** (*string*): ID of the last post that was processed
- **last_post_date** (*date*): BSON date of last post that was processed
- **enabled** (*boolean*): Whether this monitor is currently active
- **created_at** (*date*): BSON date when monitor was created
- **created_by** (*long*): Discord user ID who created this monitor as 64-bit integer
- **action_chain** (*array*): Array of actions to execute when new post is found

**Required Fields:** monitor_id, monitor_type, guild, channel, source_url, enabled, created_at

---

## 🔐 Authentication

Currently, the API does not require authentication for local development. In production, consider implementing:

- API key authentication
- JWT tokens
- Rate limiting
- CORS configuration

## 📝 Request/Response Format

### Standard Response Format

```json
{
  "success": true,
  "data": {},
  "message": "Operation completed successfully",
  "timestamp": "2025-10-01T12:00:00Z"
}
```

### Error Response Format

```json
{
  "success": false,
  "error": "Error description",
  "code": "ERROR_CODE",
  "timestamp": "2025-10-01T12:00:00Z"
}
```

## 🚀 Usage Examples

### Get Configuration
```bash
curl -X GET http://localhost:5000/api/config/yaml
```

### Update Configuration
```bash
curl -X POST http://localhost:5000/api/config/yaml \
  -H "Content-Type: application/json" \
  -d '{"yaml_content": "..."}'
```

### Get Moderation Logs
```bash
curl -X GET http://localhost:5000/api/modlogs/user/123456789
```

## 🔧 Development

### Running the API Server

```bash
# Development mode
python api.py

# Docker mode
docker-compose up banditbot-api
```

### Health Check

The API provides a health check endpoint:

```bash
curl http://localhost:5000/api/health
```

Expected response:
```json
{
  "status": "healthy",
  "timestamp": "2025-10-01T12:00:00Z",
  "database": "connected"
}
```


---

*This documentation is automatically generated from the Flask application code. Last updated: 2025-10-04 05:30:40 UTC*
