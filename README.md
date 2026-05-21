
🛡️ PolicyHub: Insurance Policy and Management System
PolicyHub is a web-based insurance management system designed to streamline policy management, claim submission, review, and authorization processes. It utilizes a Client-Server Architecture with a RESTful API backend built on Spring Boot and a modern frontend using HTML/CSS/JS.

✨ Features
Policy Management: Policyholders can view, enroll in, and manage policies. Admins/Agents can manage (CRUD) policies.

Claim Management: Users can submit claims and track their status. Admins/Adjusters/Agents can review and update claims.


Role-Based Access Control: Distinct functionality is available for Policyholders, Admins, Agents, and Adjusters.


User Authentication: Handles user login, registration, and session management with BCrypt password hashing.


Document Handling: Support for uploading, viewing, and deleting claim-related documents.


Support Tickets: Policyholders can create support tickets, which Admins/Adjusters/Agents can resolve.

🏗️ Architecture and Technologies
PolicyHub follows a Client-Server Architecture communicating via RESTful APIs.

1. Backend (Server)

Framework: Spring Boot.


Purpose: Provides API endpoints, business logic, and database access.

Key Modules:


Auth: Handles authentication, registration, and session.


Policy: Manages policies and user enrollment.


Claim: Manages claim submission, review, and assignment.


Database: MySQL.


Tables: user, policy, claim, and the junction table policy_enrollment.

Relationships:


User <-> Policy: Many-to-many via policy_enrollment.


User <-> Claim: Many-to-one (a user can have multiple claims).


Claim <-> Policy: Many-to-one (multiple claims can belong to the same policy).

2. Frontend (Client)

Technologies: HTML5/CSS3, JavaScript (JS).


Frameworks/Libraries: Bootstrap (responsive design) and jQuery (DOM manipulation, AJAX).


Interaction: Uses AJAX/REST for all communication (sending and receiving JSON data) with the Spring Boot API.


Design: Provides an SPA-like experience through dynamic content manipulation.

⚙️ Setup and Execution Guide
1. Database Setup

Create Database: Create a MySQL database named PolicyHub.

SQL
CREATE DATABASE PolicyHub;

Import Schema: Import the provided SQL dump file (MySQL_Dump.sql) to create the necessary tables (user, policy, claim, etc.).

2. Backend Configuration
Open the application.properties file.

Ensure the database properties match your local MySQL setup: | Property | Example Value | | :--- | :--- | | server.port | 8082 | | spring.datasource.url | jdbc:mysql://localhost:3306/PolicyHub?createDatabaseIfNotExist=true | | spring.datasource.username | root | | spring.datasource.password | Ram54321@ |


Run the Application: Start the Spring Boot application. The API endpoints will be available at http://localhost:8082/api.

3. Frontend Access
Open the index.html file in your web browser.

Use the Login/Register forms (powered by Bootstrap Modals) to begin using the system.

🚪 Key API Endpoints (Auth Module)
HTTP Method	Endpoint	Function	Role
POST	/auth/login	
Log in and receive a session cookie 

All
POST	/auth/register	
Register a new customer/user 

All
GET	/auth/profile	
Fetch user profile data 

All
GET	/auth/logout	
Invalidate the session 

All
