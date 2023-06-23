## Screenshots

![App Screenshot](https://drive.google.com/uc?export=view&id=1is6ban4yh8KkdI55-M0ULHBvnitutIry)

# Library management System

- This project is a library management system built using the MERN stack.
- Currently, this basic library management system is under active development and is in its initial phase.
- I've incorporated the Google Play Books API, allowing administrators to directly import books from Play Books if they are accessible there.

# Flow

- The system currently features two roles: student and admin. The admin holds all privileges, including adding and removing students, as well as managing books.
- Students cannot self-register. Instead, an admin registers them by providing an official email and student details. An auto-generated password is then sent to the given email, and this password is also securely stored in the database after hashing.
- Students can log into their accounts using the password sent to them. Once logged in, they can perform all functions allocated to the student role, such as borrowing books, but they cannot add new books.

# Authentication Flow:

- Upon login, an access token and a refresh token are generated. These are JWTs with a payload consisting of {sub:userId}, and they're created using a private access token key that is stored in the .env file.
- Along with the access token generation, a session is also created in Redis, a high-speed session store. The session is stored with the key user.id.
- The access token, refresh token, and a login flag are all sent as HTTP-only cookies to the front end.
- For access to protected routes, the client must send the access token in the header in the Bearer format, or they must include it in a cookie using the 'credentials:true' property.
- If the client possesses a valid access token, it can be verified using a public key to extract the payload, which includes the user's ID. This ID can then be used to retrieve the associated session from Redis.
- Only if the client has both a valid access token and a session will they be granted access to the protected routes; otherwise, access will be denied.

## API Reference

There are 4 core/base routes:

- Authentication route:

```http
/api/auth
```

- Users route:

```http
/api/users
```

- Book route:

```http
/api/book
```

- Student route:

```http
/api/student
```

---

- SUB ROUTES FOR /auth :-

```http
POST /login

---------------
Registration can only be done by admin

POST /register
POST /logout
```

- SUB ROUTES FOR /book :-

```http
GET /all
POST /add
PUT /update
```

- SUB ROUTES FOR /student :-

```http
GET /mybooks
POST /issue/:bookId
POST /return/:bookId
```

- SUB ROUTES FOR /user :-

```http
GET /
GET /me
```

| Parameter      | Type     | Description                                        |
| :------------- | :------- | :------------------------------------------------- |
| `accessToken`  | `cookie` | **Required**. Required for all routes except login |
| `refreshToken` | `cookie` | **Required**. Required to refresh the access token |

- In the backend, a session is maintained in the Redis database. During this process, the user's token is deserialized and the session is verified.
  
- This session is maintained using the user's ID, which is also utilized as payload during the JWT signing process.

- Consequently, when accessing any protected route, the JWT carries the user ID and the user session is verified through the ID stored in the JWT.

- Therefore, it's essential that access tokens be sent with every request after the initial login. These tokens can be sent via:

  - Authorization headers in bearer format.
  - Cookies, which is the preferred method. Just ensure that 'credentials: include' is set when sending requests from the frontend.

- Upon successful login, three cookies are automatically set:

  - logged_in: a boolean for frontend use.
  - accessToken: the access token.
  - refreshToken: the refresh token.
  
  All three of these are HTTP-only cookies.

## Run Locally

Clone the project

```bash
  git clone https://github.com/SNR02/LMS-Frontend-Backend.git
```

For running the project locally,
first clone the project using git clone

- Project has 2 sections

1. frontend directory
2. backend directory

requirement:

- docker should be installed already

Go to the project directory

```bash
   cd backend
 npm i
 docker-compose up -d
 npm run start

cd ..
cd frontend
npm i
npm run start
```

With this both backend and frontend will be running locally in development environment.

## Environment Variables

To run this project, you will need to add the following environment variables to your .env file

### Frontend:

- `REACT_APP_BASE_URL`="http://localhost:8000"
- `REACT_APP_GOOGLE_BOOKS_KEY`="(Your google play books api)"

### Backend:

- `NODE_ENV=development`
- `MONGODB_USERNAME=(any username for mongodb)`
- `MONGODB_PASSWORD=(any pass)`
- `MONGODB_DATABASE_NAME=(Enter any database name)`

- `ACCESS_TOKEN_PRIVATE_KEY`=
- `ACCESS_TOKEN_PUBLIC_KEY`=
- `REFRESH_TOKEN_PRIVATE_KEY`=
- `REFRESH_TOKEN_PUBLIC_KEY`=

- (Generate the keys from "https://travistidwell.com/jsencrypt/demo/")
- (Convert them to base64 encoding first then paste here: to convert use:"https://www.base64encode.org/")

<!-- These are smtp settings for nodemailer -->

- `EMAIL_USER=(Your email which you will use to send mail to others)`
- `EMAIL_PASS=(Email pass)`
- `EMAIL_HOST=(host of email) || smtp.ethereal.email`
- `EMAIL_PORT=(Email port)`

- For using nodemailer with gmail, read this article:

1. "https://miracleio.me/snippets/use-gmail-with-nodemailer/"
2. "https://mailtrap.io/blog/nodemailer-gmail/"
