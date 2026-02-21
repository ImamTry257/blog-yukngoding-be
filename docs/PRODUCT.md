1. Product Overview

This product is a simple blogging platform.
Main goal:
- Users can read content
- Authors can create content
- Admin can manage system

2. Core Entities
    2.1 User
        Represent user 

        Attributes :
        - name
        - email
        - phone_number
        - role_name ( guest, author, admin, superadmin )
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
        - User can do forgot password


    2.2 Content
        Represent blog articles

        Attributes :
        - title
        - slug
        - descriptions
        - status ( draf, review, published )
        - authord_id
        - category_id
        - created_at
        - updated_at
        - deleted_at

        Rules :
        - Only Author can create content
        - Content must be reviewd before published
        - Only Author and Admin can deleted published content


    2.3 Categories
        Represent category for articles

        Attributes :
        - Name
        - Descriptions
        - Status ( active, inactive )
        - created_at
        - updated_at
        - deleted_at

        Rules :
        - Only Admin can action CRUD the categories
        - Content only have one categories
        - Categories can have many content