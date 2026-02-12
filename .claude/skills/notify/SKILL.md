---
name: notify
description: 'Send push notifications to mobile via ntfy.sh. Use this skill to notify the user when tasks complete, when input is needed, or for important alerts. Triggers on: "notify me", "send notification", "alert me", "ping me", or when completing long-running tasks.'
---

# Notify Skill

Send push notifications to mobile devices via [ntfy.sh](https://ntfy.sh).

## Configuration

The ntfy topic is configured via environment variable:

```bash
export NTFY_TOPIC="your-secret-topic"
```

Or set in `~/.copilot/config.json`:
```json
{
  "ntfy_topic": "your-secret-topic"
}
```

## How to Send Notifications

### Basic notification
```bash
curl -s -d "Your message here" ntfy.sh/$NTFY_TOPIC
```

### With title and priority
```bash
curl -s \
  -H "Title: Task Complete" \
  -H "Priority: high" \
  -H "Tags: white_check_mark" \
  -d "Build finished successfully" \
  ntfy.sh/$NTFY_TOPIC
```

### Common Tags (emoji)
- `white_check_mark` ✅ - Success
- `warning` ⚠️ - Warning
- `x` ❌ - Error
- `question` ❓ - Input needed
- `hourglass` ⏳ - In progress
- `chart_with_upwards_trend` 📈 - Results

## When to Notify

### Always notify:
1. **Task completion** - When a long-running task finishes
2. **Input needed** - When user decision is required
3. **Errors** - When something fails unexpectedly

### Notification templates:

#### Task Complete
```bash
curl -s -H "Title: ✅ Task Complete" -H "Priority: default" \
  -d "Completed: [task description]" ntfy.sh/$NTFY_TOPIC
```

#### Input Needed
```bash
curl -s -H "Title: ❓ Input Needed" -H "Priority: high" \
  -d "Waiting for your input: [question]" ntfy.sh/$NTFY_TOPIC
```

#### Error
```bash
curl -s -H "Title: ❌ Error" -H "Priority: urgent" \
  -d "Failed: [error description]" ntfy.sh/$NTFY_TOPIC
```

#### Long Task Started
```bash
curl -s -H "Title: ⏳ Task Started" -H "Priority: low" \
  -d "Started: [task description]" ntfy.sh/$NTFY_TOPIC
```

## Setup Instructions

1. Install ntfy app on your phone:
   - iOS: https://apps.apple.com/app/ntfy/id1625396347
   - Android: https://play.google.com/store/apps/details?id=io.heckel.ntfy

2. Subscribe to your topic in the app (e.g., `your-secret-topic`)

3. Set the environment variable:
   ```bash
   echo 'export NTFY_TOPIC="your-secret-topic"' >> ~/.zshrc
   source ~/.zshrc
   ```

4. Test it works:
   ```bash
   curl -d "Test notification from Copilot" ntfy.sh/$NTFY_TOPIC
   ```

## Integration with Workflows

When completing tasks or needing input, Claude should:

1. Check if `$NTFY_TOPIC` is set
2. Send appropriate notification
3. Continue with normal response

Example at end of long task:
```bash
if [ -n "$NTFY_TOPIC" ]; then
  curl -s -H "Title: ✅ Done" -d "Task complete" ntfy.sh/$NTFY_TOPIC
fi
```
