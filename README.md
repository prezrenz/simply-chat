# simply-chat
A learning project for real-time web-based chat apps.

## Features
- Landing page
- Sign In/Sign Up (Cookie-based, JWT)
    - Secure with HTTPOnly, SecureSite, proper CORS, CSRF Tokens (research)
    - Authentication via Bearer Tokens in header
- Users list with search
- Single user real-time chat
- Chat notifications
- Responsive design (web and mobile)

## To do
- [x] design schema
- [ ] design API
- [ ] design user interface (web and mobile)
- [ ] design client-side functions
- [ ] design API tests
- [ ] implement based on tests
- [ ] design client-side tests
- [ ] implement based on tests

## Database Design
users {
    id BIGSERIAL NOTNULL PK,
    username TEXT NOTNULL,
    email TEXT NOTNULL,
    password TEXT NOTNULL
}

messages {
    id BIGSERIAL NOTNULL PK,
    sender_id INTEGER NULL FK(users, id, CASCADE, SETNULL)
    receiver_id INTEGER NULL FK(users, id, CASCADE, SETNULL),
    message TEXT NULL,
    receiver_seen BOOLEAN NULL
}

## API Design
/users {
    get {
        description: Get all users.
        responses: {
            200: {
                body: JSON{
                    "data": { id, username, email }[]
                }
            }
        }
    }
    post {
        description: Create new user.
        body: {
            "username": username,
            "email": email,
            "password": password
        }
        responses: {
            200: {
                cookie: {
                    "atok": JWTToken,
                    "HTTPOnly": true,
                    "SameSite": Strict
                }
            }
        }
    }
    /users/{id} {
        get {
            description: Get user by id.
            responses: {
                200: {
                    body: JSON{
                        "data": { id, username, email }
                    }
                }
            }
        }
    }
}
