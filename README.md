# Job Portal Application

!(https://example.com/job-portal-image.png)

This is a full-stack job portal application built as a comprehensive project for BIT students learning web development. The application allows for user authentication, role-based access control, and CRUD operations for managing jobs and applications.

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [Project Structure](#project-structure)
- [API Endpoints](#api-endpoints)
- [Database Schema](#database-schema)
- [Contributing](#contributing)
- [License](#license)

## Features

The application provides a complete solution for a job portal with distinct user roles:

**Admin Features:**
- ⚙️ **Admin Dashboard**: Manage all aspects of the job portal.
- ➕ **Job Management**: Create, read, update, and delete job listings.
- 📄 **Application Management**: View and manage all job applications.
- 🔒 **Role-Based Access**: Restricted access to administrative functionalities.

**Job Seeker Features:**
- 🔍 **Job Browse**: View and search for available job listings.
- 📝 **Job Application**: Apply for jobs with a cover letter.
- 📋 **Application History**: View a history of all jobs they have applied for.
- 👤 **User Authentication**: Secure registration and login.

## Tech Stack

This project is built using a modern and robust full-stack tech stack:

-   **Backend**:
    -   [**Node.js**](https://nodejs.org/en/): A JavaScript runtime for building server-side applications.
    -   [**Express.js**](https://expressjs.com/): A fast, unopinionated, minimalist web framework for Node.js.
    -   [**PostgreSQL**](https://www.postgresql.org/): A powerful, open-source relational database system.
    -   [**pg**](https://www.npmjs.com/package/pg): A non-blocking PostgreSQL client for Node.js.
    -   [**bcrypt**](https://www.npmjs.com/package/bcrypt): A library for hashing passwords securely.
    -   [**jsonwebtoken**](https://www.npmjs.com/package/jsonwebtoken): An implementation of JSON Web Tokens for authentication.
-   **Frontend**:
    -   [**React.js**](https://reactjs.org/): A JavaScript library for building user interfaces.
    -   [**React Router DOM**](https://reactrouter.com/web/guides/quick-start): For handling client-side routing.
    -   [**axios**](https://axios-http.com/docs/intro): A promise-based HTTP client for the browser and Node.js.
-   **Development Tools**:
    -   [**Visual Studio Code**](https://code.visualstudio.com/): The primary code editor.
    -   [**Git & GitHub**](https://github.com/): For version control and collaboration.
    -   [**Postman**](https://www.postman.com/)/[**Insomnia**](https://insomnia.rest/): For testing API endpoints.

## Getting Started

Follow these instructions to get a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

You need the following software installed on your machine:
-   **Node.js** (v16 or higher)
-   **PostgreSQL** (v12 or higher)
-   **npm** (comes with Node.js)

### Installation

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/your-username/job-portal.git](https://github.com/your-username/job-portal.git)
    cd job-portal
    ```

2.  **Set up the backend:**
    -   Navigate to the backend directory: `cd backend`
    -   Install dependencies: `npm install`
    -   Create a `.env` file in the `backend` directory and add your PostgreSQL database credentials:
        ```env
        DB_USER=postgres
        DB_HOST=localhost
        DB_NAME=job_portal
        DB_PASSWORD=your_password
        DB_PORT=5432
        JWT_SECRET=your_jwt_secret_key
        ```
    -   Create the database and tables using the SQL scripts provided in the course:
        ```sql
        -- Create the database
        CREATE DATABASE job_portal;
        -- Use the SQL schema to create tables
        -- (e.g., in a tool like pgAdmin or a terminal)
        ```
    -   Start the backend server: `npm start` or `node server.js`
    -   The server will run at `http://localhost:5000`.

3.  **Set up the frontend:**
    -   Navigate to the frontend directory: `cd frontend`
    -   Install dependencies: `npm install`
    -   Start the React development server: `npm start`
    -   The application will be available at `http://localhost:3000`.

## Project Structure

The project is divided into two main parts: `backend` for the Node.js API and `frontend` for the React.js client.

job-portal/
├── backend/
│   ├── config/              # Database configuration
│   ├── middleware/          # Authentication middleware
│   ├── routes/              # API routes (auth, jobs, etc.)
│   ├── server.js            # Main Express server file
│   └── package.json
└── frontend/
├── public/
├── src/
│   ├── components/      # Reusable React components
│   ├── pages/           # Main application pages
│   ├── App.js           # Main application component
│   └── index.js
└── package.json


## API Endpoints

The API is built following RESTful principles. Here are some of the key endpoints:

| Method   | Endpoint                     | Description                                      | Access       |
| -------- | ---------------------------- | ------------------------------------------------ | ------------ |
| `POST`   | `/api/auth/register`         | Register a new user (`admin` or `jobseeker`)     | Public       |
| `POST`   | `/api/auth/login`            | Log in and receive a JWT token                   | Public       |
| `POST`   | `/api/jobs`                  | Create a new job listing                         | Admin only   |
| `GET`    | `/api/jobs`                  | Get all job listings                             | Public       |
| `GET`    | `/api/jobs/:id`              | Get a single job listing                         | Public       |
| `PUT`    | `/api/jobs/:id`              | Update an existing job                           | Admin only   |
| `DELETE` | `/api/jobs/:id`              | Delete a job listing                             | Admin only   |
| `POST`   | `/api/jobs/:jobId/apply`     | Apply for a specific job                         | JobSeeker only |
| `GET`    | `/api/applications`          | View all applications for a user                 | Authenticated|

## Database Schema

The database schema is designed to support the core functionalities of the job portal.

```sql
-- Users table
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    full_name VARCHAR(255) NOT NULL,
    user_type VARCHAR(20) CHECK (user_type IN ('admin', 'jobseeker')),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Jobs table
CREATE TABLE jobs (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    company VARCHAR(255) NOT NULL,
    location VARCHAR(255),
    salary_range VARCHAR(100),
    posted_by INTEGER REFERENCES users(id),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Applications table
CREATE TABLE applications (
    id SERIAL PRIMARY KEY,
    job_id INTEGER REFERENCES jobs(id),
    applicant_id INTEGER REFERENCES users(id),
    cover_letter TEXT,
    status VARCHAR(20) DEFAULT 'pending',
    applied_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
Contributing
Contributions are welcome! If you'd like to improve the project, please follow these steps:

Fork the repository.

Create a new branch (git checkout -b feature/AmazingFeature).

Commit your changes (git commit -m 'Add some AmazingFeature').

Push to the branch (git push origin feature/AmazingFeature).

Open a Pull Request.

License
Distributed under the MIT License. See LICENSE for more information.
