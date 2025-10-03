# Reaction-Based Moderation Examples

This document provides comprehensive examples of using the `on_raw_reaction_add` event handler for reaction-based moderation and automation.

## Table of Contents

1. [Basic Delete Post Example](#basic-delete-post-example)
2. [Multiple Role Thresholds](#multiple-role-thresholds)
3. [Content Approval System](#content-approval-system)
4. [Auto-Report System](#auto-report-system)
5. [Highlight Excellent Content](#highlight-excellent-content)
6. [Multi-Stage Moderation](#multi-stage-moderation)

---

## Basic Delete Post Example

Delete a post when 3 or more moderators react with 😠 emoji.

```yaml
events:
  on_raw_reaction_add:
    description: Delete posts when 3+ moderators react with angry emoji
    actions:
    - type: check_reaction_threshold
      role: Moderator
      emoji: "😠"
      threshold: 3
      actions:
      - type: delete_message
        reason: "Community moderation via reactions"
      - type: chan_log
        color: orange
        title: "Post Deleted by Reaction Moderation"
        description: "A post was deleted after {threshold} moderators reacted with 😠"
        fields:
        - name: "Original Author"
          value: "{message_author_mention} ({message_author_id})"
          inline: true
        - name: "Channel"
          value: "{channel_mention}"
          inline: true
        - name: "Triggered By"
          value: "{reactor_mention}"
          inline: true
        - name: "Message Content"
          value: "{message_content}"
          inline: false
```

**How it works:**
1. A moderator sees an inappropriate post
2. They react with 😠 emoji
3. When 3 or more moderators have reacted with 😠, the post is automatically deleted
4. The deletion is logged to the mod channel with context

---

## Multiple Role Thresholds

Different thresholds for different roles (admins can act faster).

```yaml
events:
  on_raw_reaction_add:
    description: Multi-tier reaction moderation
    actions:
    # Admin threshold - only need 2 admins
    - type: check_reaction_threshold
      role: Admin
      emoji: "🚫"
      threshold: 2
      actions:
      - type: delete_message
        reason: "2+ admins flagged content"
      - type: chan_log
        color: red
        title: "Post Deleted by Admin Vote"
        description: "Content removed by admin consensus"
        fields:
        - name: "Admin Count"
          value: "2+ admins voted to remove"
          inline: true
        - name: "Author"
          value: "{message_author_mention}"
          inline: true
    
    # Moderator threshold - need 4 moderators
    - type: check_reaction_threshold
      role: Moderator
      emoji: "🚫"
      threshold: 4
      actions:
      - type: delete_message
        reason: "4+ moderators flagged content"
      - type: chan_log
        color: orange
        title: "Post Deleted by Moderator Vote"
        description: "Content removed by moderator consensus"
        fields:
        - name: "Moderator Count"
          value: "4+ moderators voted to remove"
          inline: true
        - name: "Author"
          value: "{message_author_mention}"
          inline: true
```

**How it works:**
- If 2 admins react with 🚫, the post is deleted immediately
- If 4 moderators react with 🚫, the post is deleted
- Both thresholds work independently, whichever is reached first triggers deletion

---

## Content Approval System

Approve user-submitted content when enough reviewers give thumbs up.

```yaml
events:
  on_raw_reaction_add:
    description: Auto-approve content submissions
    actions:
    - type: check_reaction_threshold
      role: Content Reviewer
      emoji: "👍"
      threshold: 3
      actions:
      - type: set_role
        user: message_author
        role: Approved Creator
        reason: "Content approved by reviewers"
      - type: react
        emoji: "✅"
      - type: reply
        message: "🎉 Congratulations {message_author_mention}! Your content has been approved by the review team and you've been granted the Approved Creator role."
      - type: chan_log
        color: green
        title: "Content Approved"
        description: "User content approved by reviewer consensus"
        fields:
        - name: "Creator"
          value: "{message_author_mention}"
          inline: true
        - name: "Approvers"
          value: "3+ Content Reviewers"
          inline: true
        - name: "Content Link"
          value: "[Jump to Message](https://discord.com/channels/{guild_id}/{channel_id}/{message_id})"
          inline: false
```

**How it works:**
1. User submits content in a review channel
2. Content reviewers react with 👍 if they approve
3. When 3 reviewers have approved, user automatically gets "Approved Creator" role
4. Bot adds ✅ reaction to show approval and replies to congratulate the user
5. Approval is logged for audit purposes

---

## Auto-Report System

Automatically report content to moderators when regular members flag it.

```yaml
events:
  on_raw_reaction_add:
    description: User-driven content reporting
    actions:
    - type: check_reaction_threshold
      role: Member
      emoji: "🚩"
      threshold: 5
      actions:
      - type: chan_log
        color: yellow
        title: "⚠️ Content Flagged by Community"
        description: "A message has been flagged by 5+ members for moderator review"
        fields:
        - name: "Flagged Message"
          value: "[Jump to Message](https://discord.com/channels/{guild_id}/{channel_id}/{message_id})"
          inline: false
        - name: "Message Author"
          value: "{message_author_mention} ({message_author_id})"
          inline: true
        - name: "Channel"
          value: "{channel_mention}"
          inline: true
        - name: "Total Flags"
          value: "{total_reactions} 🚩"
          inline: true
        - name: "Message Preview"
          value: "{message_content}"
          inline: false
        - name: "Action Required"
          value: "@Moderator please review this flagged content"
          inline: false
      - type: dm
        user: message_author
        message: "⚠️ Your message in {channel_mention} has been flagged by the community for moderator review. A moderator will look into this shortly."
```

**How it works:**
1. Members can flag problematic content by reacting with 🚩
2. When 5 members flag a message, it's automatically reported to mod channel
3. Original author gets a DM notification
4. Moderators can click the link to review and take action

---

## Highlight Excellent Content

Pin or highlight messages that receive many positive reactions from trusted members.

```yaml
events:
  on_raw_reaction_add:
    description: Highlight high-quality content
    actions:
    - type: check_reaction_threshold
      role: Trusted Member
      emoji: "⭐"
      threshold: 10
      actions:
      - type: pin_message
      - type: react
        emoji: "📌"
      - type: chan_log
        color: gold
        title: "⭐ Excellent Content Highlighted"
        description: "Community has highlighted exceptional content"
        fields:
        - name: "Author"
          value: "{message_author_mention}"
          inline: true
        - name: "Channel"
          value: "{channel_mention}"
          inline: true
        - name: "Stars"
          value: "{total_reactions} ⭐"
          inline: true
        - name: "Message Link"
          value: "[Jump to Message](https://discord.com/channels/{guild_id}/{channel_id}/{message_id})"
          inline: false
```

**How it works:**
1. Trusted members can star excellent content with ⭐
2. When 10 trusted members have starred a message, it's automatically pinned
3. Bot adds 📌 to show it's been highlighted
4. Highlight is logged to track community-valued content

---

## Multi-Stage Moderation

Combine warnings, timeouts, and deletions based on different reaction thresholds.

```yaml
events:
  on_raw_reaction_add:
    description: Progressive moderation system
    actions:
    # Stage 1: Warning at 2 mod reactions with ⚠️
    - type: check_reaction_threshold
      role: Moderator
      emoji: "⚠️"
      threshold: 2
      actions:
      - type: reply
        message: "⚠️ {message_author_mention} - Moderators have flagged this content as potentially problematic. Please review our community guidelines."
      - type: chan_log
        color: yellow
        title: "Warning Issued"
        description: "User received warning for flagged content"
        fields:
        - name: "User"
          value: "{message_author_mention}"
          inline: true
        - name: "Moderators"
          value: "2+ warned"
          inline: true
    
    # Stage 2: Timeout at 3 mod reactions with 🔇
    - type: check_reaction_threshold
      role: Moderator
      emoji: "🔇"
      threshold: 3
      actions:
      - type: timeout
        user: message_author
        duration: 1h
        reason: "Repeated problematic content flagged by moderators"
      - type: delete_message
      - type: chan_log
        color: orange
        title: "User Timed Out"
        description: "User timed out for 1 hour due to flagged content"
        fields:
        - name: "User"
          value: "{message_author_mention}"
          inline: true
        - name: "Duration"
          value: "1 hour"
          inline: true
    
    # Stage 3: Delete at 4 mod reactions with 🚫
    - type: check_reaction_threshold
      role: Moderator
      emoji: "🚫"
      threshold: 4
      actions:
      - type: delete_message
        reason: "Content deleted by moderator consensus"
      - type: dm
        user: message_author
        message: "Your message in {channel_mention} was removed by the moderation team for violating community guidelines. Repeated violations may result in further action."
      - type: chan_log
        color: red
        title: "Content Removed"
        description: "Message deleted by moderator vote"
        fields:
        - name: "Author"
          value: "{message_author_mention}"
          inline: true
        - name: "Moderators"
          value: "4+ voted to remove"
          inline: true
```

**How it works:**
1. Different emojis trigger different moderation actions
2. ⚠️ (2+ mods) = Warning message to user
3. 🔇 (3+ mods) = 1-hour timeout + message deletion
4. 🚫 (4+ mods) = Message deletion + DM notification
5. All actions are logged for accountability

---

## Best Practices

### Choosing Thresholds

- **Too Low (1-2)**: Risk of false positives, single moderator bias
- **Optimal (3-5)**: Good balance, requires consensus
- **Too High (6+)**: Ineffective, threshold rarely met

### Emoji Selection

- **Clear Intent**: Use emojis that clearly communicate purpose
- **Consistent**: Document what each emoji means for your team
- **Limited Set**: Don't have too many different reaction systems

### Recommended Emojis

- 😠 or 🚫 - Delete/Remove
- ⚠️ - Warning
- 🚩 - Flag for review
- 👍 or ✅ - Approve
- ⭐ - Highlight quality
- 🔇 - Timeout/Mute

### Logging

- **Always log actions**: Include `chan_log` for accountability
- **Include context**: Message content, author, timestamp
- **Track who triggered**: Useful for auditing
- **Link to original**: Makes review easier

### Testing

1. **Test in development** first with `./run-dev.sh`
2. **Use test channel** before deploying to production
3. **Start with higher thresholds** and adjust down if needed
4. **Monitor logs** to see if thresholds are being reached

### Role Configuration

- **Dedicated roles**: Create "Content Reviewer" or similar specific roles
- **Multiple tiers**: Different thresholds for different trust levels
- **Clear permissions**: Document who has which roles

---

## Troubleshooting

### Threshold Never Triggers

- Check that users have the exact role name specified
- Verify emoji matches exactly (including custom emoji format)
- Ensure threshold isn't set too high for your team size

### Wrong Messages Being Deleted

- Threshold might be too low
- Consider requiring specific emoji instead of any reaction
- Add more restrictive role requirements

### Multiple Triggers

- This is expected behavior - if multiple thresholds are met, all actions execute
- Use different emojis for different purposes
- Design actions to be idempotent (safe to run multiple times)

---

## See Also

- [User Guide - Event Handlers](USER_GUIDE.md#-event-handlers)
- [Actions Documentation](ACTIONS.md)
- [YAML Configuration Examples](../config/modular_commands.yml)
