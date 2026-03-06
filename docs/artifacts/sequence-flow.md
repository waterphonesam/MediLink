# Sequence Flow

**Type:** Sequence Diagram
**Exported:** 2026-03-06T05:00:08.053Z
**Source:** PlanVersion

## Linked Requirements

- fdf2c097-1cb9-4b69-a746-f7256e7acda7
- 47b5e19c-7014-424e-825e-d0cd1f8af9a3
- d020d77c-ddd7-4dda-a23f-0182e89b6595

## Diagram

```mermaid
sequenceDiagram
  actor User
  participant WebApp
  participant AuthService
  participant IdentityService
  participant MFAService
  participant SessionService
  participant AuditLog

  note right of WebApp: UC-001 User Secure Login with MFA - Preconditions active account and TOTP enrolled - Trace IDs ae72f9fd-b969-45fb-a496-7844ff378d68, 8bb058ac-209e-432a-9029-74c68a705731, 81903960-53e9-46b5-a1ce-3e41e628c4a2

  User->>WebApp: Navigate to login page
  User->>WebApp: Submit email and password
  WebApp->>AuthService: Validate credentials for tenant
  AuthService->>IdentityService: Verify user and password

  alt Invalid Credentials
    IdentityService-->>AuthService: Credentials invalid
    AuthService->>AuditLog: Log failed login attempt
    AuthService-->>WebApp: Invalid credentials response
    WebApp-->>User: Display invalid credentials message
    opt Throttling threshold exceeded
      AuthService->>AuditLog: Log throttling event
      AuthService-->>WebApp: Account temporarily locked response
      WebApp-->>User: Display account locked message
    end
  else Credentials valid
    IdentityService-->>AuthService: Credentials valid
    AuthService->>MFAService: Request TOTP challenge
    MFAService-->>WebApp: Prompt for 6 digit TOTP code
    WebApp-->>User: Prompt for TOTP code
    User->>WebApp: Submit TOTP code
    WebApp->>MFAService: Verify TOTP code

    alt TOTP invalid
      MFAService-->>WebApp: TOTP invalid response
      MFAService->>AuditLog: Log failed TOTP attempt
      WebApp-->>User: Display TOTP invalid message
    else TOTP valid
      MFAService-->>AuthService: TOTP validated
      AuthService->>SessionService: Create secure session and issue encrypted JWT
      note right of SessionService: Encrypted JWT issued, session stored securely, secure cookie set, avoid sensitive data in messages - Trace FR-002 FR-003 FR-004
      SessionService-->>WebApp: Return session token and redirect
      AuthService->>AuditLog: Log successful login event
      WebApp-->>User: Redirect to role specific dashboard
    end
  end

  note right of AuditLog: Postconditions user authenticated and active session, successful login event recorded - Trace IDs ac7a2a1f-b9f4-4b63-adad-1e3a52a72a2a, 40db6e09-d7b2-4c61-8cc8-44038f53c1b1, a9d07d5e-cd5e-489b-aab0-41999f77b12f, 9f7bd7ed-f1b9-4833-b6b0-b708553a11c2, 06307acb-b02b-44a0-be08-30a36a48fb4a
```
