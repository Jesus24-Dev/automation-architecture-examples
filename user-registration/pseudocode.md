```Flow
BEGIN UserRegistration

SET user_data = RECEIVE form_input

IF NOT VALIDATE(user_data)
    RETURN validation_errors
END IF

SET normalized_data = NORMALIZE(user_data)

IF EXISTS user WITH email = normalized_data.email
    RETURN error "User already exists"
END IF

TRY
    CREATE user_record WITH status = "pending_verification"
CATCH db_error
    LOG db_error
    RETURN error "Database failure"
END TRY

TRY
    SEND verification_email
CATCH email_error
    LOG email_error
    RETRY SEND verification_email MAX 3 TIMES WITH exponential_backoff
    IF email still fails
        UPDATE user_record status = "email_failed"
        RETURN error "Verification email could not be sent"
    END IF
END TRY

RETURN success "User created successfully"

END

```