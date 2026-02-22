0. Technology Stack
Backend:
- Language: Go 1.24
- Framework: Fiber
- Architecture: Clean Architecture

Database:
- PostgreSQL 15
- ORM: GORM
- Primary Key: ID
- Migration Tool: golang-migrate

Authentication:
- JWT (HS256)
- Password Hashing: bcrypt

Testing:
- Go test
- Testify

Containerization:
- Docker

1. Architecture Principles
- Use Clean Architecture pattern.
- Separate layer: Handler, Usecase, Repository, Domain.
- Business rules must not depend on framework.
- No direct database access from Handler.
- Use dependency injection.
- Generate Code using Go lang go.1.24.* fiber MVC

2. Folder Structure
    /cmd
    /internal
        /domain
        /repository
        /usecase
        /delivery (http handler)
        /middleware
    /pkg

3. Naming Convention
- Use singular entity name (User, Content, Category, Comment, Like).
- Table name must be plural (users, contents, categories, comments, likes).
- Slug format: lowercase with dash.
- Enum must use uppercase.
- File name use camelCase.

4. Route Clasification
    6.1 Front Page
        - Represent Page for user guest and author access the website
        - Only read all content on public route
        - Comment and like content on protected route

    6.2 Backoffice / Dashboard Page
        - Represent Environment Page for Admin and Superadmin
        - Only Login, Forgot Password, Activation User Flow on public route
        - All access transaction data on protected route

5. API Response Standard
All API responses must follow format:
{
  "success": boolean,
  "message": string,
  "data": object | null,
  "error": object | null
}

HTTP Status Code:
200 - Success
201 - Created
400 - Validation error
401 - Unauthorized
403 - Forbidden
404 - Not found
500 - Internal error

6. Security Rules
- Password must be hashed using sha256.
- JWT must be signed with secret key.
- Never expose password field in API response.
- Validate role after JWT verification.
- All write operations require authorization middleware.

7. Database Rules
- Use ID as primary key.
- Use soft delete with deleted_at.
- All foreign keys must use constraint.
- Add index on:
  - email
  - slug
  - content_id + user_id

8. ORM Rules:
- All models must use ID as primary key.
- Soft delete must use gorm.DeletedAt.
- Do not use AutoMigrate in production.
- Use explicit migration SQL files.
- Use Preload for relations.
- Do not expose GORM model directly in API response.
- All relations must define foreign key explicitly.
- Cascade delete must be restricted.
- Soft delete must not cascade automatically.

9. Logging Rules
- Log all user and admin actions.
- Log login attempt.
- Log failed OTP verification.
- Do not log password or sensitive token.

10. Testing Strategy
- Unit test for usecase layer.
- Integration test for API endpoint.
- Minimum 70% coverage.
- Mock external dependency.

11. Git Rules
- Use feature branch.
- No direct push to main branch.
- All PR must be reviewed.

12. AI Code Generation Rules
- AI must follow PRODUCT.md strictly.
- AI must not change domain rules without explicit instruction.
- AI must generate modular code.
- AI must not mix business logic inside handler.
- AI must follow naming convention.

