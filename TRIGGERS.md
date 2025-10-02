# BanditBot Regex Triggers Documentation

*Last updated: 2025-01-20*

## Overview

Regex triggers allow BanditBot to automatically respond to message patterns without requiring the `?` command prefix. Triggers use regular expressions (regex) to match message content and execute configured actions when a match is found.

## Table of Contents

1. [Basic Concepts](#basic-concepts)
2. [Creating Triggers](#creating-triggers)
3. [Pattern Groups](#pattern-groups)
4. [Match Group Capture](#match-group-capture)
5. [Managing Triggers](#managing-triggers)
6. [Testing Triggers](#testing-triggers)
7. [Practical Examples](#practical-examples)
8. [Available Variables](#available-variables)
9. [Regex Flags](#regex-flags)
10. [Best Practices](#best-practices)
11. [Troubleshooting](#troubleshooting)

---

## Basic Concepts

### What is a Regex Trigger?

A regex trigger monitors all messages in a Discord server and automatically executes actions when a message matches a defined pattern. This is useful for:

- **Link rewriting** (Twitter/X → vxtwitter, Instagram → ddinstagram)
- **Content moderation** (detecting inappropriate language or spam)
- **Auto-responses** (responding to specific words or phrases)
- **Fun interactions** (easter eggs, memes, reactions)

### Trigger Structure

Each trigger consists of:

- **Name**: Unique identifier for the trigger
- **Pattern**: Regex expression to match against messages
- **Flags**: Optional regex modifiers (case-insensitive, multiline, etc.)
- **Roles/NotRoles**: Optional role-based filtering
- **Actions**: What to do when a match is found

---

## Creating Triggers

### Method 1: Using Discord Commands

You can create triggers directly in Discord using the `?add_trigger` command:

**Command Syntax:**
```
?add_trigger <name> <pattern> <actions>
```

**Example:**
```
?add_trigger hello_bot \bhello\b act:reply:"Hello there! 👋"
```

This creates a trigger named `hello_bot` that matches the word "hello" and replies with a greeting.

**Action Format:**
- `act:reply:"message"` - Reply to the message
- `act:react:emoji` - React with an emoji
- `act:delete_message` - Delete the triggering message
- `act:flow:flow_name` - Execute an action flow

Multiple actions can be chained:
```
?add_trigger excited_bot \bexcited\b act:react:🎉 act:reply:"I'm excited too!"
```

**Required Permissions:** Moderator role

### Method 2: Using YAML Configuration

For more complex triggers, edit `config/modular_commands.yml`:

```yaml
triggers:
  my_trigger:
    description: "What this trigger does"
    pattern: '\bpattern\b'
    flags: i
    roles:
      - Moderator
    actions:
      - type: react
        emoji: "👍"
      - type: reply
        message: "Trigger activated!"
```

After editing the YAML file, reload the configuration:
```
?reload
```

---

## Pattern Groups

Pattern groups allow you to define **multiple regex patterns** that trigger the **same actions**. This is perfect when different patterns should have identical behavior.

### Example Use Case: Twitter/X Link Rewriter

Both `twitter.com` and `x.com` should be rewritten to `vxtwitter.com`. Instead of creating two separate triggers, use a pattern group:

### Step 1: Define the Pattern Group

In `modular_commands.yml`, add to the `$pattern_groups` section:

```yaml
$pattern_groups:
  twitter_links:
    - 'https://(x\.com)/([^\s]+)'
    - 'https://(twitter\.com)/([^\s]+)'
```

### Step 2: Reference the Pattern Group in a Trigger

```yaml
triggers:
  twitter_rewriter:
    description: "Rewrites Twitter/X links to vxtwitter.com for better viewing"
    patterns: twitter_links  # References the pattern group
    flags: i
    actions:
      - type: text_replace
        text: "{match_0}"
        find: "{match_1}"
        replace_with: "vxtwitter.com"
        store_as: "rewritten_url"
      - type: reply
        message: "🐦 Better Twitter/X link: {rewritten_url}"
```

**Important Rules:**
- Use `pattern:` for a single pattern OR `patterns:` for a pattern group reference (not both)
- The bot checks patterns in order and stops at the first match
- Flags apply to all patterns in the group
- Pattern groups are validated at startup

### Pattern Group Benefits

✅ **Maintainability**: Update all related patterns in one place  
✅ **Readability**: Separate pattern definitions from trigger logic  
✅ **Reusability**: Multiple triggers can reference the same pattern group  

---

## Match Group Capture

When a pattern uses **capturing groups** (parentheses in regex), the captured content becomes available as variables in your actions.

### Basic Example

**Pattern with capturing group:**
```yaml
pattern: '\b(fr|no cap|wsp)\b'
```

When someone types "no cap", the pattern matches and captures "no cap" as `{match_1}`.

**Using the capture in actions:**
```yaml
actions:
  - type: message
    message: '{author_mention}, use proper English. "{match_1}" is not proper.'
```

**Result:** "use proper English. "no cap" is not proper."

### Available Match Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `{match_0}` | Full matched text | "https://x.com/user/status/123" |
| `{match_1}` | First captured group | "x.com" |
| `{match_2}` | Second captured group | "user/status/123" |
| `{match_N}` | Nth captured group | (additional captures) |

### Multiple Capture Groups Example

**Pattern:**
```yaml
pattern: 'https://(x\.com|twitter\.com)/([^\s]+)'
```

**Message:** "Check out https://x.com/user/status/123"

**Variables Available:**
- `{match_0}` = `"https://x.com/user/status/123"` (full match)
- `{match_1}` = `"x.com"` (domain)
- `{match_2}` = `"user/status/123"` (path)

**Action using captures:**
```yaml
- type: text_replace
  text: "{match_0}"
  find: "{match_1}"
  replace_with: "vxtwitter.com"
  store_as: "rewritten_url"
```

### Pattern Group with Captures

When using pattern groups, each pattern can have capturing groups:

```yaml
$pattern_groups:
  zoomer_slang:
    - '\b(fr)\b'           # Captures "fr" as group 1
    - '\b(no cap)\b'       # Captures "no cap" as group 1
    - '\b(wsp)\b'          # Captures "wsp" as group 1
    - '\b(ong)\b'          # Captures "ong" as group 1

triggers:
  zoomer_detector:
    patterns: zoomer_slang
    actions:
      - type: message
        message: 'Please avoid using "{match_1}" in this server.'
```

All patterns capture to the same group number (`{match_1}`), making it easy to write generic responses.

---

## Managing Triggers

### List All Triggers

View all configured triggers:
```
?list_triggers
```

**Output includes:**
- Trigger name
- Pattern or pattern group reference
- Actions configured
- Role requirements

### List Pattern Groups

View all defined pattern groups:
```
?list_pattern_groups
```

### Delete a Trigger

Remove a trigger by name:
```
?del_trigger <name>
```

**Example:**
```
?del_trigger hello_bot
```

**Required Permissions:** Moderator role

### View Trigger in Action

Triggers automatically fire when matching messages are sent. Watch for:
- Bot reactions to messages
- Bot replies to messages
- Messages being deleted (if configured)

---

## Testing Triggers

### Using the `?test_regex` Command

Before creating a trigger, test your regex pattern to ensure it works as expected:

**Command Syntax:**
```
?test_regex [flags] <pattern> <test_string>
```

**Examples:**

**Test basic word matching:**
```
?test_regex i \bhello\b Hello there, friend!
```

**Test with capture groups:**
```
?test_regex i https://(x\.com|twitter\.com)/([^\s]+) Check out https://x.com/user/status/123
```

**Output includes:**
- ✅ Match status (matched or not)
- Full matched text
- Capture groups with their values
- Available match variables (`{match_0}`, `{match_1}`, etc.)
- Position of match in the string

**Flags:**
- `i` - Case insensitive
- `m` - Multiline mode
- `s` - Dot matches newline

### Testing Through Web UI

The BanditBot web interface includes a **Regex Tester** tool:

1. Navigate to `http://localhost:3000` (development) or your deployed URL
2. Go to the **Regex Tester** tab
3. Enter your pattern and test strings
4. View results with capture groups highlighted
5. See exactly what match variables will be available

---

## Practical Examples

### Example 1: Twitter/X Link Rewriter

**Goal:** Convert Twitter/X links to vxtwitter.com for better embeds

**Pattern Group:**
```yaml
$pattern_groups:
  twitter_links:
    - 'https://(x\.com)/([^\s]+)'
    - 'https://(twitter\.com)/([^\s]+)'
```

**Trigger:**
```yaml
triggers:
  twitter_rewriter:
    description: "Rewrites Twitter/X links to vxtwitter.com for better viewing"
    patterns: twitter_links
    flags: i
    actions:
      - type: text_replace
        text: "{match_0}"           # Full URL
        find: "{match_1}"           # Domain (x.com or twitter.com)
        replace_with: "vxtwitter.com"
        store_as: "rewritten_url"
      - type: reply
        message: "🐦 Better Twitter/X link: {rewritten_url}"
```

**How it works:**
1. User posts: "https://x.com/user/status/123"
2. Pattern matches, captures domain in `{match_1}`
3. `text_replace` swaps domain with "vxtwitter.com"
4. Bot replies with rewritten link

### Example 2: Content Moderation

**Goal:** Warn users about using slang terms

**Pattern Group:**
```yaml
$pattern_groups:
  zoomer_slang:
    - '\b(fr)\b'
    - '\b(no cap)\b'
    - '\b(wsp)\b'
    - '\b(ong)\b'
```

**Action Flow:**
```yaml
action_flows:
  slang_warning:
    description: "Warning for inappropriate slang"
    actions:
      - type: delete_message
      - type: message
        message: '{author_mention}, please use proper English. "{match_1}" is not appropriate for this server.'
```

**Trigger:**
```yaml
triggers:
  zoomer_slang_detector:
    description: "Detects zoomer slang from unverified users"
    patterns: zoomer_slang
    flags: i
    roles:
      - unverified
    actions:
      - type: action_flow
        flow: slang_warning
```

**How it works:**
1. Only triggers for users with "unverified" role
2. Matches any slang term from pattern group
3. Deletes the message
4. Sends warning mentioning the specific term used

### Example 3: Instagram Link Rewriter

**Goal:** Convert Instagram links to ddinstagram for better viewing

**Trigger:**
```yaml
triggers:
  instagram_rewriter:
    description: "Rewrites Instagram links to ddinstagram.com for better viewing"
    pattern: 'https://instagram\.com/([^\s]+)'
    flags: i
    actions:
      - type: text_replace
        text: "{match_0}"
        find: "instagram.com"
        replace_with: "ddinstagram.com"
        store_as: "rewritten_url"
      - type: reply
        message: "📱 Better Instagram link: {rewritten_url}"
```

### Example 4: Auto-React to Keywords

**Goal:** React with emoji when specific keywords are mentioned

**Trigger:**
```yaml
triggers:
  bigfoot_trigger:
    description: "Responds when someone mentions Bigfoot"
    pattern: '\bbigfoot\b'
    flags: i
    actions:
      - type: react
        emoji: "🦶"
```

**How it works:**
1. Matches the word "bigfoot" (case-insensitive)
2. Bot reacts to the message with 🦶 emoji
3. Uses word boundaries (`\b`) to avoid matching "bigfoots" or "mybigfoot"

### Example 5: User Mention Detection

**Goal:** React when a specific user is mentioned

**Trigger:**
```yaml
triggers:
  special_mention:
    description: "Reacts with GAY when specific user is mentioned"
    pattern: '<@!?100418001488064512>'
    actions:
      - type: react
        emoji: 🇬
      - type: react
        emoji: 🇦
      - type: react
        emoji: 🇾
```

**Pattern breakdown:**
- `<@!?` - Matches user mention format
- `100418001488064512` - Specific user ID
- `>` - Closing bracket

---

## Available Variables

Triggers have access to special variables in their action messages:

### Message Context Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `{author}` | Username of message author | "JohnDoe#1234" |
| `{author_mention}` | Mention of the author | "@JohnDoe" |
| `{channel}` | Channel name | "general" |
| `{channel_mention}` | Mention of the channel | "#general" |
| `{trigger_name}` | Name of the trigger | "twitter_rewriter" |

### Match Variables

| Variable | Description |
|----------|-------------|
| `{match_0}` | Full matched text |
| `{match_1}` | First captured group |
| `{match_2}` | Second captured group |
| `{match_N}` | Nth captured group |
| `{matched_text}` | Alias for `{match_0}` |
| `{matched_pattern}` | The specific pattern that matched (useful with pattern groups) |

### Using Variables in Actions

```yaml
actions:
  - type: reply
    message: "Hey {author_mention}, I detected: {match_0}"
  
  - type: chan_log
    description: "Trigger {trigger_name} fired in {channel_mention}"
    color: yellow
```

---

## Regex Flags

Regex flags modify how pattern matching works:

| Flag | Name | Description | Example |
|------|------|-------------|---------|
| `i` | Case Insensitive | Matches regardless of case | `hello` matches "Hello", "HELLO", "HeLLo" |
| `m` | Multiline | `^` and `$` match line boundaries | Useful for multi-line messages |
| `s` | Dotall | `.` matches newline characters | Pattern can span multiple lines |

### Using Flags

**YAML configuration:**
```yaml
triggers:
  my_trigger:
    pattern: '\bhello\b'
    flags: i
```

**Discord command:**
```
?add_trigger my_trigger \bhello\b act:reply:"Hi!" i
```

**Multiple flags:**
```yaml
flags: im
```

### Examples

**Case insensitive matching:**
```yaml
pattern: '\bhello\b'
flags: i
# Matches: hello, Hello, HELLO, HeLLo
```

**Multiline pattern:**
```yaml
pattern: '^ERROR:'
flags: m
# Matches ERROR: at the start of any line in a multi-line message
```

**Dotall for multi-line content:**
```yaml
pattern: 'start.*end'
flags: s
# Matches "start" and "end" even if there are newlines between them
```

---

## Best Practices

### 1. Use Word Boundaries

**❌ Bad:** `pattern: 'hello'`  
Matches "hello", "hellos", "othello"

**✅ Good:** `pattern: '\bhello\b'`  
Matches only "hello" as a complete word

### 2. Test Before Deploying

Always use `?test_regex` to verify your pattern works:

```
?test_regex i \bhello\b Hello there, friend!
```

### 3. Use Pattern Groups for Related Patterns

**❌ Bad:** Multiple separate triggers for x.com and twitter.com

**✅ Good:** One pattern group with both patterns:
```yaml
$pattern_groups:
  twitter_links:
    - 'https://(x\.com)/([^\s]+)'
    - 'https://(twitter\.com)/([^\s]+)'
```

### 4. Be Specific with Patterns

**❌ Bad:** `pattern: '.*'` (matches everything)

**✅ Good:** `pattern: 'https://example\.com/[^\s]+'` (specific URL pattern)

### 5. Use Role Restrictions Wisely

Prevent spam by limiting who triggers can affect:

```yaml
triggers:
  moderator_only:
    pattern: '\bsecret\b'
    roles:
      - Moderator
    # Only moderators trigger this
```

Or exclude certain roles:

```yaml
triggers:
  verified_users:
    pattern: '\bslang\b'
    notroles:
      - Moderator
      - Admin
    # Doesn't trigger for moderators/admins
```

### 6. Capture Groups for Reusable Patterns

Use capturing groups to extract parts of the match:

```yaml
pattern: 'https://([^/]+)/(.+)'
# Captures domain and path separately
# {match_1} = domain
# {match_2} = path
```

### 7. Document Your Triggers

Always include a clear description:

```yaml
triggers:
  my_trigger:
    description: "What this trigger does and why it exists"
```

### 8. Avoid Overlapping Triggers

Be careful with triggers that might match the same content:

**❌ Problematic:**
```yaml
trigger_a:
  pattern: 'hello'
trigger_b:
  pattern: 'hello world'
```

Both will fire for "hello world" - consider which should take priority.

---

## Troubleshooting

### Trigger Not Firing

**Check these common issues:**

1. **Pattern doesn't match:**
   - Test with `?test_regex` to verify your pattern
   - Check for typos in the pattern
   - Verify you're using the correct regex syntax

2. **Role requirements:**
   - Ensure you have the required role(s)
   - Check if you're excluded by `notroles:`

3. **Trigger not loaded:**
   - Run `?list_triggers` to verify the trigger exists
   - If using YAML, run `?reload` after making changes

4. **Case sensitivity:**
   - Add `flags: i` if you want case-insensitive matching
   - Test both uppercase and lowercase versions

### Pattern Matches Too Much

**Solution:** Use word boundaries and more specific patterns

**❌ Too broad:** `pattern: 'test'`  
Matches: "test", "testing", "contest", "latest"

**✅ More specific:** `pattern: '\btest\b'`  
Matches: only "test" as a complete word

### Capture Groups Not Working

**Check these:**

1. **Using parentheses?**
   ```yaml
   pattern: 'hello (world|there)'  # ✅ Has capture group
   pattern: 'hello world'           # ❌ No capture group
   ```

2. **Accessing correct group:**
   - `{match_0}` is always the full match
   - `{match_1}` is the first capture group (first set of parentheses)
   - `{match_2}` is the second capture group, etc.

3. **Non-capturing groups:**
   ```yaml
   pattern: 'hello (?:world|there)'  # (?:...) doesn't capture
   pattern: 'hello (world|there)'    # (...) captures to {match_1}
   ```

### Pattern Group Not Found

**Error:** "Pattern group 'name' not found"

**Solutions:**

1. **Check spelling:**
   ```yaml
   $pattern_groups:
     my_patterns:  # Definition
   
   triggers:
     my_trigger:
       patterns: my_patterns  # ✅ Matches
       patterns: mypatterns   # ❌ Typo
   ```

2. **Verify definition exists:**
   - Run `?list_pattern_groups` to see all defined groups
   - Ensure the pattern group is in the `$pattern_groups:` section

3. **Reload configuration:**
   ```
   ?reload
   ```

### Regex Syntax Errors

**Common mistakes:**

1. **Unescaped special characters:**
   ```yaml
   pattern: 'example.com'      # ❌ . matches any character
   pattern: 'example\.com'     # ✅ Escaped period
   ```

2. **Unmatched brackets:**
   ```yaml
   pattern: '[abc'             # ❌ Missing closing ]
   pattern: '[abc]'            # ✅ Matched brackets
   ```

3. **Invalid escape sequences:**
   ```yaml
   pattern: '\d+'              # ✅ Valid (digits)
   pattern: '\x'               # ❌ Invalid escape
   ```

**Use `?test_regex` to validate:**
```
?test_regex \d+ Test 123
```

### Getting Help

1. **Test your pattern:**
   ```
   ?test_regex i <your_pattern> <test_message>
   ```

2. **View existing triggers:**
   ```
   ?list_triggers
   ```

3. **Check pattern groups:**
   ```
   ?list_pattern_groups
   ```

4. **Examine configuration:**
   - Review `config/modular_commands.yml`
   - Check for YAML syntax errors
   - Verify indentation is correct

5. **Check bot logs:**
   - Look for error messages in console/log files
   - Check for validation errors during startup or reload

---

## Additional Resources

- **[Commands Reference](COMMANDS.md)** - Full command documentation
- **[Action System](ACTIONS.md)** - Available actions for triggers
- **[User Guide](USER_GUIDE.md)** - General bot usage guide
- **[API Documentation](API.md)** - REST API for programmatic access

### Regular Expression Resources

- [Regex101](https://regex101.com/) - Online regex tester with explanations
- [RegExr](https://regexr.com/) - Interactive regex learning tool
- [Regex Cheat Sheet](https://www.rexegg.com/regex-quickstart.html) - Quick reference guide

---

*This documentation is maintained as part of the BanditBot project. For issues or suggestions, please open a GitHub issue.*
