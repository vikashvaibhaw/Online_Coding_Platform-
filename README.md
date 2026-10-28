# Online Coding Platform

A full-stack online coding platform where users can browse coding problems, write solutions in multiple programming languages, execute code against test cases, and track their submissions.

The platform integrates **Judge0** for code execution and uses **JWT authentication, MongoDB, and Redis** to support user authentication, submission management, and session invalidation.

## Features

- **User Authentication:** Register and log in securely using password hashing with bcrypt and JWT-based authentication.
- **Problem Management:** Retrieve coding problems with descriptions, difficulty levels, tags, starter code, and visible test cases.
- **Code Execution:** Execute solutions in C++, Java, and JavaScript through the Judge0 API.
- **Test Case Evaluation:** Evaluate submitted code against hidden test cases and calculate the number of passed cases.
- **Submission Tracking:** Store submission details, including status, runtime, memory usage, and error messages.
- **Admin APIs:** Create, update, and delete coding problems through protected admin routes.
- **Solved Problem Tracking:** Maintain a user's solved-problem list.
- **Redis Integration:** Use Redis to invalidate logged-out JWTs before their expiration.
- **Protected Routes:** Authenticate users before allowing access to protected APIs.

## Tech Stack

| Technology | Purpose |
|---|---|
| React | Frontend user interface |
| Node.js | Backend JavaScript runtime |
| Express.js | REST API and routing |
| MongoDB | Store users, problems, and submissions |
| Mongoose | MongoDB object modeling |
| JWT | Authentication and authorization |
| bcrypt | Password hashing |
| Redis | Token invalidation during logout |
| Judge0 | Execute submitted code |
| Axios | HTTP requests to external services |
| Vite | Frontend development and build tooling |

## System Architecture

```text
                React Frontend
                      |
                 HTTP / Axios
                      |
                      v
               Express Backend
                      |
              Authentication
                 Middleware
                      |
                      v
                  Controllers
                      |
             +--------+--------+
             |                 |
             v                 v
          MongoDB            Redis
             |
             |        Code Submission
             |                 |
             |                 v
             |              Judge0 API
             |                 |
             |          Execute Test Cases
             |                 |
             +--------<--------+
                      |
                      v
               JSON Response
                      |
                      v
                React UI Update
```

## Code Submission Workflow

1. The user selects a coding problem and writes a solution in the editor.
2. The frontend sends the source code and programming language to the backend.
3. The authentication middleware verifies the user's JWT.
4. The backend retrieves the problem and its hidden test cases from MongoDB.
5. A submission record is created with an initial `pending` status.
6. The backend maps the selected language to the corresponding Judge0 language ID.
7. The source code is submitted to Judge0 for execution against the hidden test cases.
8. The backend retrieves the execution results and evaluates the test cases.
9. The submission record is updated with the status, passed test cases, runtime, memory usage, and error information.
10. The backend returns the submission result to the frontend, where the UI can display the outcome.

## Authentication Workflow

The platform uses JWT-based authentication with cookies.

1. During registration, the password is hashed using bcrypt before being stored.
2. During login, the backend verifies the submitted password against the stored hash.
3. If the credentials are valid, the backend generates a signed JWT and stores it in a cookie.
4. Protected routes use authentication middleware to verify the token and identify the user.
5. Redis is used to block logged-out tokens until their expiration time.

## Project Structure

The repository contains incremental development folders (`Day01` through `Day09`) and a frontend application. The final backend implementation is located in `Day09/src`.

```text
LeetCode_App/
├── Day01/
├── Day02/
├── ...
├── Day09/
│   └── src/
│       ├── Middleware/
│       ├── config/
│       ├── controllers/
│       ├── models/
│       ├── routes/
│       ├── utils/
│       └── index.js
└── Leetcode_Frontend/
    ├── public/
    ├── src/
    ├── package.json
    └── ...
```

### Backend organization

- `Middleware/` — Authentication and authorization middleware.
- `config/` — Database and Redis configuration.
- `controllers/` — Business logic for authentication, problems, and submissions.
- `models/` — Mongoose schemas for users, problems, and submissions.
- `routes/` — Express API endpoints.
- `utils/` — Judge0 integration and code-execution utilities.
- `index.js` — Express application setup and service initialization.

## Getting Started

### Prerequisites

Install the following before running the project:

- Node.js and npm
- MongoDB
- Redis
- A Judge0 API account or compatible Judge0 instance
- Git

### 1. Clone the repository

```bash
git clone https://github.com/vikashvaibhaw/LeetCode_App.git
cd LeetCode_App
```

### 2. Configure the backend

Navigate to the final backend source directory:

```bash
cd Day09/src
```

Install the backend dependencies using the package configuration in the applicable backend project directory.

Create a `.env` file and configure the environment variables required by the application. For example:

```env
PORT=3000
JWT_KEY=your_secure_jwt_secret
MONGODB_URI=your_mongodb_connection_string
REDIS_URL=your_redis_connection_url
```

Configure the Judge0 API credentials using the environment variable names expected by `utils/problemUtility.js`.

**Important:** Never commit real passwords, JWT secrets, database credentials, or API keys to GitHub. Keep them in your local `.env` file and exclude that file from version control.

### 3. Start the backend

From the backend directory containing `index.js`, run:

```bash
node index.js
```

Use `npm start` instead if the backend's `package.json` defines an appropriate start script.

Ensure MongoDB, Redis, and the Judge0 service are configured and accessible.

### 4. Start the frontend

Open a separate terminal and navigate to the frontend directory:

```bash
cd Leetcode_Frontend
npm install
npm run dev
```

Open the local URL printed by Vite in your terminal.

**Configuration note:** The backend CORS configuration currently allows `http://localhost:5173`. Update it if your frontend runs at a different origin.

## API Overview

The backend exposes the following main route groups.

| Route group | Purpose |
|---|---|
| `/user` | Registration, login, logout, profile, and authentication checks |
| `/problem` | Problem creation, updates, retrieval, deletion, and user problem tracking |
| `/submission` | Code execution and code submission |

### Submission endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/submission/run/:id` | Run code for a problem |
| POST | `/submission/submit/:id` | Submit code for evaluation |

Protected endpoints require a valid authenticated session.

## Error Handling and Future Improvements

Potential improvements for a more production-ready system include:

- Add HTTP timeouts, bounded retries, and exponential backoff for Judge0 requests.
- Introduce a job queue and background workers to handle code execution asynchronously.
- Handle Judge0 compilation errors, runtime errors, time-limit exceeded results, and service failures separately.
- Add API rate limiting and stronger request validation.
- Improve database health checks, logging, monitoring, and recovery behavior.
- Add automated tests for authentication, problem APIs, and submission workflows.
- Add pagination for problem lists and submission history.
- Load-test the application to measure performance under concurrent traffic.

## Learning Outcomes

Through this project, I explored:

- Designing REST APIs with Express.js.
- Structuring backend code into routes, middleware, controllers, models, and utilities.
- Implementing JWT authentication and protected routes.
- Modeling users, problems, and submissions using MongoDB and Mongoose.
- Integrating an external code execution service.
- Handling asynchronous requests and test-case evaluation.
- Understanding service failures, polling, and scalability improvements.

## Author

**Vikash Kumar**

- GitHub: [vikashvaibhaw](https://github.com/vikashvaibhaw)
- Project Repository: [LeetCode_App](https://github.com/vikashvaibhaw/LeetCode_App)

---

*Built as a full-stack learning project to explore online judge architecture, backend development, authentication, and external code-execution integration.*
