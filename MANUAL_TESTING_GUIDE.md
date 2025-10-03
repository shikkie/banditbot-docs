# BanditBot Manual Testing Guide

*This guide covers manual Discord testing that cannot be automated in unit tests*

## 🎯 Overview

While BanditBot has comprehensive automated testing, certain features require manual testing in a live Discord environment to verify proper integration with Discord's API and user interactions.

## 📋 Pre-Testing Setup

### Test Server Requirements
- **Discord Server**: Dedicated test server with bot installed
- **Bot Permissions**: Administrator permissions for full feature testing
- **Test Roles**: Create test roles: `Moderator`, `Admin`, `Banned`, `Muted`, `VIP`
- **Test Channels**: Multiple text channels, voice channels, and categories
- **Test Users**: Multiple test accounts or willing volunteers

### Environment Setup
```bash
# Start bot in development mode
./run-dev.sh bot

# Start API for configuration changes
./run-dev.sh api

# Access web interface for config changes
http://localhost:3000
```

## 🧪 Critical Manual Tests

### 1. Command System Tests

#### Basic Command Functionality
- [ ] **?help** - Verify help command displays available commands
- [ ] **Invalid command** - Verify unknown commands are handled gracefully
- [ ] **Permission denied** - Test commands without required roles
- [ ] **Parameter validation** - Test commands with missing/invalid parameters

#### Parameter Extraction Tests
- [ ] **User mentions** - `?kick @user reason` with actual user mentions
- [ ] **Channel mentions** - Commands requiring channel parameters
- [ ] **Role mentions** - Commands requiring role parameters
- [ ] **Collect remaining** - Test parameters that collect multiple words

### 2. Slash Commands Tests

#### Slash Command Registration
- [ ] **Command visibility** - Verify slash commands appear in Discord's autocomplete
- [ ] **Parameter types** - Test user, channel, role parameter autocomplete
- [ ] **Ephemeral responses** - Verify private responses work correctly

#### Slash Command Functionality
- [ ] **/modlogs @user** - Test user autocomplete and modlog display
- [ ] **/quickban @user** - Test standalone slash command
- [ ] **/timeout @user 5m** - Test mapped command with parameters
- [ ] **/blackjack** - Test interactive game functionality

### 3. Moderation Features

#### User Management
- [ ] **Kick user** - `?kick @user reason` and verify user is removed
- [ ] **Ban user** - `?ban @user reason` and verify ban
- [ ] **Timeout user** - `?timeout @user 5m reason` and verify Discord timeout
- [ ] **Unban user** - `?unban user_id` and verify ban removal

#### Role Management
- [ ] **Add role** - `?addrole @user rolename` and verify role assignment
- [ ] **Remove role** - `?removerole @user rolename` and verify removal
- [ ] **Sticky roles** - Add sticky role, user leaves/rejoins, verify persistence
- [ ] **Role caching** - Test role cache/restore functionality

#### Channel Management
- [ ] **Lock channel** - Verify users cannot send messages
- [ ] **Unlock channel** - Verify messaging is restored
- [ ] **Lock all channels** - Test server-wide channel locking
- [ ] **Slowmode** - Test slowmode activation and timing

### 4. Advanced Features

#### Voice Features
- [ ] **Voice message transcription** - Post voice message, verify STT works
- [ ] **Text-to-speech** - `?tts Hello world` and verify audio playback
- [ ] **Voice channel monitoring** - Join/leave voice channels, verify logging

#### OCR Features
- [ ] **Image OCR** - `?ocr` with image attachment, verify text extraction
- [ ] **Multiple OCR providers** - Test different OCR actions if available
- [ ] **Image formats** - Test PNG, JPG, GIF images

#### Blackjack Game
- [ ] **/blackjack** - Start game and verify interface
- [ ] **Hit/Stand buttons** - Test game interactions
- [ ] **Betting system** - Test money management
- [ ] **Game completion** - Play through full game cycle

### 5. Event System Tests

#### Discord Events
- [ ] **Member join** - New user joins, verify welcome messages/roles
- [ ] **Member leave** - User leaves, verify logging
- [ ] **Message events** - Post messages, verify triggers activate
- [ ] **Reaction events** - Add reactions, verify reaction-based moderation

#### Trigger System
- [ ] **Regex triggers** - Post messages matching configured patterns
- [ ] **Case sensitivity** - Test case-sensitive vs case-insensitive patterns
- [ ] **Pattern groups** - Test grouped trigger patterns
- [ ] **Action chains** - Verify multiple actions execute in sequence

### 6. Database Integration

#### Data Persistence
- [ ] **Sticky roles persist** - Verify data survives bot restart
- [ ] **Moderation logs** - Verify modlog entries are created
- [ ] **Delayed actions** - Set timed actions, verify execution
- [ ] **Social monitors** - Verify RSS/social feeds are tracked

#### Database Operations
- [ ] **Modlog retrieval** - `?modlogs @user` displays history
- [ ] **Data cleanup** - Verify old data is cleaned up appropriately
- [ ] **Schema validation** - Verify data integrity

### 7. Social Media Monitoring

#### RSS Feed Monitoring
- [ ] **Add RSS monitor** - Configure RSS feed, verify posts appear
- [ ] **Update intervals** - Verify feeds check at configured intervals
- [ ] **Feed parsing** - Verify different RSS formats work
- [ ] **Error handling** - Test with broken/invalid feeds

#### YouTube Monitoring
- [ ] **Channel monitoring** - Monitor YouTube channel for new videos
- [ ] **Video notifications** - Verify new video announcements
- [ ] **Metadata extraction** - Verify title, description, thumbnail

### 8. Configuration Management

#### Hot Reload Testing
- [ ] **Config changes** - Modify config via web interface
- [ ] **?reload command** - Test hot reload functionality
- [ ] **Validation errors** - Test invalid config handling
- [ ] **Backup/restore** - Test configuration backup system

#### Web Interface
- [ ] **Visual editor** - Create commands via web interface
- [ ] **YAML synchronization** - Verify visual changes update YAML
- [ ] **Validation feedback** - Test error reporting in UI
- [ ] **Real-time updates** - Verify live config updates

### 9. Error Handling & Edge Cases

#### Discord API Errors
- [ ] **Permission errors** - Test actions without proper bot permissions
- [ ] **Rate limiting** - Test bulk operations
- [ ] **User not found** - Test commands with invalid user IDs
- [ ] **Channel not found** - Test commands with invalid channels

#### Network Issues
- [ ] **Database disconnection** - Test behavior when MongoDB unavailable
- [ ] **Discord disconnection** - Test reconnection behavior
- [ ] **External API failures** - Test OCR/TTS failures

#### Resource Limitations
- [ ] **Large file uploads** - Test limits on voice/image files
- [ ] **Long messages** - Test message length limits
- [ ] **Concurrent operations** - Test multiple simultaneous commands

### 10. Performance Testing

#### Load Testing
- [ ] **Rapid commands** - Send many commands quickly
- [ ] **Large servers** - Test in servers with many members
- [ ] **Memory usage** - Monitor bot memory over time
- [ ] **Response times** - Verify commands respond promptly

#### Scalability
- [ ] **Multiple servers** - Test bot across multiple Discord servers
- [ ] **High activity** - Test during periods of high chat activity
- [ ] **Resource monitoring** - Check CPU, memory, database performance

## 🚨 Critical Path Testing

### Essential Features (Must Test Before Each Release)
1. **Basic commands work** (help, ping, simple actions)
2. **Moderation functions** (kick, ban, timeout)
3. **Slash commands respond** (at least one mapped and one standalone)
4. **Permissions enforced** (role restrictions work)
5. **Database operations** (modlogs, sticky roles)
6. **Hot reload works** (config changes apply)

### Extended Features (Test Before Major Releases)
1. **Voice features** (TTS, STT, voice monitoring)
2. **OCR functionality** (image text extraction)
3. **Blackjack game** (full game flow)
4. **Social media monitoring** (RSS feeds)
5. **Advanced triggers** (regex patterns)
6. **Event handling** (join/leave events)

## 📝 Test Documentation

### Test Report Template
```markdown
## Manual Test Report - [Date]

### Environment
- Bot Version: [commit hash]
- Discord Server: [server name]
- Tester: [name]

### Test Results
- [ ] ✅ Basic Commands
- [ ] ✅ Slash Commands  
- [ ] ❌ Moderation (Issue: timeout not working)
- [ ] ⚠️ Voice Features (TTS works, STT failed)

### Issues Found
1. **Timeout command fails**: Error message "Missing permissions"
   - Steps to reproduce: `/timeout @user 5m`
   - Expected: User gets Discord timeout
   - Actual: Error in chat

### Recommendations
- Fix timeout permission issue
- Retest voice STT with different audio formats
```

### Bug Report Template
```markdown
## Bug Report

**Feature**: [Slash Commands / Moderation / etc.]
**Severity**: [Critical / High / Medium / Low]

**Description**: Brief description of the issue

**Steps to Reproduce**:
1. Step one
2. Step two
3. Step three

**Expected Behavior**: What should happen

**Actual Behavior**: What actually happens

**Environment**:
- Bot version: [commit hash]
- Discord server: [server name]
- User permissions: [role list]

**Additional Info**: Any relevant logs, screenshots, etc.
```

## 🔄 Regular Testing Schedule

### Daily (During Development)
- Basic command functionality
- Current feature being developed
- Critical path commands

### Weekly (Development Cycle)
- Full basic feature set
- Configuration management
- Database operations

### Pre-Release (Before Deployment)
- Complete manual test suite
- Performance testing
- Error handling verification
- All advanced features

### Post-Deployment (Production Verification)
- Critical path functionality
- Monitor for errors in production logs
- Verify performance metrics

## 🎯 Testing Best Practices

### Test Environment
- Use separate test Discord server
- Have multiple test accounts available
- Test with various permission levels
- Use realistic data volumes

### Test Data
- Create reproducible test scenarios
- Use consistent test user/channel names
- Document test configurations
- Clean up test data regularly

### Documentation
- Record all test procedures
- Document expected vs actual results
- Track bug reports and resolutions
- Maintain test case library

### Automation Opportunities
- Identify repetitive manual tests
- Create helper scripts for setup
- Use Discord bots for automated testing where possible
- Document automation gaps

---

*This guide should be updated as new features are added to BanditBot. Each PR should include relevant manual testing verification.*