1. Product Overview

This product is a simple blogging platform designed for educational and small community use.

Goals:
- Users can read blog content publicly 
- Registered users can interact ( comment & likes )
- Author can create and manage own their content
- Admin manage moderation
- Superadmin manage system-level configuration

Out of scopes:
- Payment system
- Advertisement
- Multi-language
- Real-time notification

2. Core Entities
    2.1 User
        Represent user 

        Attributes :
        - name
        - email
        - phone_number
        - role_name ( guest, reader, author, admin, superadmin )
        - password
        - is_login_verified
        - email_verified_token
        - email_verified_token_at
        - email_verified_at
        - otp_code
        - otp_code_token
        - otp_code_token_at
        - otp_code_verified_at
        - forgot_password_token
        - forgot_password_token_at
        - forgot_password_last_at
        - created_at
        - updated_at
        - deleted_at

        Rules :
        - Only Superadmin can action CRUD user
        - Required activation email for action create password
        - Login using otp verification
        - User can do forgot password expect guest


    2.2 Content
        Represent blog contents

        Attributes :
        - title
        - slug
        - type
        - descriptions
        - status ( draft, review, published )
        - user_id
        - category_id
        - created_at
        - updated_at
        - deleted_at

        Rules :
        - Only Author can create content
        - Content must be review before published
        - Only Admin and Superadmin can soft delete published content
        - Draft content can be edited on by author
        - Review content can not be edited expect by admin
        - Publish content can not be edited

    2.3 Categories
        Represent category for contents

        Attributes :
        - name
        - descriptions
        - status ( active, inactive )
        - created_at
        - updated_at
        - deleted_at

        Rules :
        - Only Admin can action CRUD the categories
        - Content only have one categories
        - Categories can have many content

    2.4 ContentComment
        Represent comment for contents

        Attributes :
        - content_id
        - parent_id
        - descriptions
        - status ( draft, published, deleted )
        - user_id
        - created_at
        - updated_at
        - deleted_at

        Rules : 
        - Only authorized user can comment content
        - User can comment on comment before
        - user reader can set comment with draft status
        - Admin must approve comment before published
        - Published status can be edited by admin user
        - Only admin can delete a comment

    2.5 ContentLike
        Represent like for content

        Attributes :
        - content_id
        - user_id
        - created_at
        - updated_at
        - deleted_at

        Rules :
        - Only authorized user can like or unliked content
        - Like count is calculated by grouping by content_id
        - Unlike is implemented as soft delete

    2.6 Setting
        Represent for setting website

        Attributes :
        - title
        - descriptions
        - status ( UP, DOWN, MAINTENANCE )
        - start_date_maintenance
        - end_date_maintenance
        - created_by
        - updated_by
        - created_at
        - updated_at
        - deleted_at

        Rules :
        - Only superadmin can change settings

3. Role & Permission Matrix ( Global )
    | Action                | Guest | Reader | Author | Admin | Superadmin |
    | ---------------       | ----- | ------ | ------ | ----- | ---------- |
    | Read Content          | ✔     | ✔      | ✔     | ✔     | ✔         |
    | Comment Content       | ✖     | ✔      | ✔     | ✔     | ✔         |
    | Like Content          | ✖     | ✔      | ✔     | ✔     | ✔         |
    | Create Content        | ✖     | ✖      | ✔     | ✔     | ✔         |
    | Approve Content       | ✖     | ✖      | ✖     | ✔     | ✔         |
    | Delete Content        | ✖     | ✖      | ✖     | ✔     | ✔         |
    | Manage User           | ✖     | ✖      | ✖     | ✔     | ✔         |
    | Manage Admin          | ✖     | ✖      | ✖     | ✖     | ✔         |
    | Manage Setting        | ✖     | ✖      | ✖     | ✖     | ✔         |

4. State Content Machine Rules
    States:
    - draft
    - review
    - published
    - deleted ( soft deleted )

    Transitions:
    draft -> review ( Author )
    review -> published ( Admin )
    published -> deleted ( Admin )

    Rules:
    - Author can move draft to review
    - Admin can move review to published
    - Published can not moved draft or review
    - Deleted is soft deleted
    - Only admin can soft delete published content
    - Draft can be edited only by author

5. State User Authentication Machine Rules
    User Authentication Flow
    login -> verify otp -> Authentication session

    Rules
    - otp code is six digit integer
    - State verify otp can not comment and liked content
    - Token verify otp can be expired on 30 minutes

6. State User Authorized Machine Rules 
    Rules
    - Token use JWT
    - Token authorized user can be expired on 1 day
    - Unauthorized user cannot access protected routes
    - Once use access route all transaction such comment and like a content, required middleware for validating token authorized JWT user

7. Entities Relationship
    - One User can have many Content
    - One User can have many Comment
    - One User can have many ContentLike
    - One Content belongs to one User
    - One Content belongs to one Category
    - One Content can have many Comment
    - One Comment can have many child Comment (max 2 levels)
    - One Content can have many ContentLike

8. Technical Constraints
    - Use soft delete for all entities
    - Slug must be unique
    - Email must be unique
    - Phone number must be unique
    - One user can like one content only once
    - Comment can be nested up to 2 levels

9. Application Action
    9.1 User Action
     - Register
     - Verify Email
     - Login
     - Verify OTP
     - Forgot Password
     - Reset Password

    9.2 Content Action
     - Create draft Content
     - Get Content by Id
     - Update draft Content
     - Submit content to review
     - Approve content
     - Soft delete content

    9.3 Categories Action
     - Create Categories
     - Update Categories
     - Get Categories by Id
     - Soft delete Categories

    9.4 Comment Action
     - Create draft comment
     - Approve comment
     - Delete comment

    9.5 Like Action
     - Like Content
     - Unlike Content