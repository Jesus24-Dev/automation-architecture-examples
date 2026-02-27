# Retry Strategy – User Registration Process

## Validation Errors
Retry Policy:
- No retry.
- Return error immediately to user.

Reason:
These are permanent errors caused by invalid input.

## Database Errors
Retry Policy:
- Retry up to 3 times.
- Fixed delay: 2 seconds between attempts.
- Only retry for connection/timeouts.
- Do NOT retry for unique constraint violations.

If all retries fail:
- Log structured error.
- Return generic server error.
- Trigger monitoring alert.

## Email Sending Errors
Retry Policy:
- Max 3 attempts.
- Exponential backoff:
    1st retry: 5 seconds
    2nd retry: 15 seconds
    3rd retry: 45 seconds
- Add jitter (+/- random 20%).

If still failing:
- Update user status to "email_failed".
- Send message to Dead Letter Queue.
- Notify admin via monitoring system.
- Allow manual resend later.