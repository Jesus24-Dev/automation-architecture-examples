# Failures – User Registration Process

## 1. Validation Errors (Client/Data Level)

- Invalid email format
- Weak password
- Missing required fields
- Invalid characters in input
- Email already registered

Impact:
- User cannot proceed.
- No system retry required.

Type:
Permanent error.

## 2. Database Errors (Infrastructure Level)

- Database connection timeout
- Database unavailable
- Unique constraint violation
- Transaction rollback failure
- Write operation timeout

Impact:
- User record not created.
- System integrity risk.

Type:
Usually transient (except constraint violations).

## 3. Email Service Errors (External Dependency)

- SMTP server unavailable
- Email provider API timeout
- DNS resolution failure
- Rate limit exceeded
- Email rejected by provider

Impact:
- User created but cannot verify account.

Type:
Usually transient.

## 4. Concurrency & Idempotency Issues

- Double form submission
- Race condition creating duplicate user
- Email sent twice
- Retry creates duplicate verification tokens

Impact:
- Inconsistent system state.
- Duplicate users or spam emails.

Type:
Logical/system design error.

## 5. Unexpected System Failures

- Server crash during process
- Memory overflow
- Container restart mid-transaction
- Network partition

Impact:
- Partial execution.
- Unknown state consistency.

Type:
Infrastructure/system error.