 Insurance Policy and Management System Documentation (REST + Modern Frontend)


1. Project Description
PolicyHub is a web-based  insurance management system designed to streamline policy management, claim submission, review, and auth processes. It allows policyholders to view, enroll, and manage policies, submit claims, and track their status. Admins, agents, and claim can manage policies and review claims.


2. System Architecture
The project follows a Client-Server Architecture with a clear separation between the frontend (Client) and backend (Server), communicating entirely via RESTful APIs.


2.1 Backend (Server - Spring Boot)
This layer provides the API endpoints, business logic, and database access.
Module | Purpose | Key Components
Auth | Handle user authentication, registration, profile, and session management. | User |entity, AuthController (handles login/logout/session), AuthService (BCrypt hashing).
Policy | Manage insurance policies (CRUD) and handle user enrollment. | Policy entity, PolicyController, PolicyService.
Claim | Submit, review, assign, and update insurance claims. | Claim entity, ClaimController, ClaimService.



2.2 Frontend (Client - HTML/CSS/JS)
This layer handles the user interface and interacts with the backend via asynchronous JavaScript calls.
Technology | Purpose in Project
HTML5 / CSS3 | Defines the structure and presentation of the application.
Bootstrap | Provides a responsive, mobile-first design framework and standardized UI components.
JavaScript (JS) | Executes all client-side logic, data manipulation, and event handling.
jQuery | Simplifies DOM traversal, manipulation, and AJAX (Asynchronous JavaScript and XML) calls for REST communication.
REST/AJAX | Mechanism used by JS/jQuery to send and receive JSON data from the Spring Boot API.


3. Database Design & Configuration

3.1 Database Overview
Database Name: PolicyHub.


3.2 Database Tables and Columns
Tables	Columns
user	userId (PK), email, password, role, username
policy	policyId (PK), coverageAmount, createdDate, policyNumber, policyStatus
policy_enrollment	policyId (FK), userId (FK)
claim	claimId (PK), claimAmount, claimDate, claimStatus, policyId (FK to policy), userId (FK to user)


3.3 Relationships
•	User <-> Policy: Many-to-many via policy_enrollment.
•	User <-> Claim: Many-to-one — a user (policyholder) can have multiple claims.
•	Claim <-> Policy: Many-to-one — multiple claims can belong to the same policy.



4. Modules, APIs, and Backend Functionality


4.1 Authentication Module (Auth)
Purpose: Handles user login, registration, logout, and profile management using HTTP Sessions/Cookies for stateful authentication after initial login.
Notes: Passwords are stored using BCryptPasswordEncoder.
Key Functions & Endpoints:
HTTP Method	Endpoint	Function	Frontend Interaction
GET	/auth/profile	showProfile	JS fetches user profile for dashboard personalization.
POST	/auth/login	login	JS sends username/password and receives session cookie.
POST	/auth/register	registerUser	JS sends customer/user JSON payload.
GET	/auth/logout	logout	JS sends request to invalidate session.


4.2 Policy Module
Purpose: Allows admins to manage policies and users to view/enroll policies.
PolicyController Endpoints:
HTTP Method	Endpoint	Function	Role
POST	/policies/create	createPolicySubmit	Admin
POST	/policies/update/{policyId}	updatePolicy	Admin
DELETE	/policies/delete/{policyId}	deletePolicy	Admin
GET	/policies/view	viewPolicies	All
GET	/policies/my-policies	myPolicies	Policyholder
POST	/policies/enroll/{policyId}	enrollPolicy	Policyholder


4.3 Claim Module
Purpose: Handles submission, review, and management of insurance claims.
Notes: Validations ensure claim amount is within policy coverage.
ClaimController Endpoints:
HTTP Method	Endpoint	Function	Role
GET	/claims/my-claims	viewMyClaims	Policyholder
POST	/claims/submit	submitClaim	Policyholder/Agent
POST	/claims/{claimId}/status	updateClaimStatus	Adjuster/Admin
GET	/claims/{claimId}	getClaimDetails	All
GET	/claims/review	reviewClaims	Adjuster/Admin


4.4 Document Module
Purpose: Handles upload, viewing, and deletion of claim-related documents.
DocumentController Endpoints:
HTTP Method	Endpoint	Function
POST	/documents/upload	uploadDocument
GET	/documents/view/{documentId}	getDocumentDetails
POST	/documents/delete/{documentId}	deleteDocument


4.5 Support Module
Purpose: Manage user support tickets and admin resolution.
SupportController Endpoints:
HTTP Method	Endpoint	Function	Role
POST	/support/create	createTicket	Policyholder
GET	/support/admin	showAllTicketsForAdmin	Admin/Adjuster/Agent
POST	/support/{ticketId}/resolve	resolveTicket	Admin/Adjuster/Agent
GET	/support	showUserTickets	Policyholder


5. Frontend Design and User Interface (HTML/CSS/JS/Bootstrap)
The frontend is a Single Page Application (SPA)-like experience achieved through dynamically hiding and showing content sections using JavaScript, jQuery, and Bootstrap classes, all communicating with the REST API via AJAX.
Page	Frontend Components	Data Interaction (JS/jQuery)
index.html	Login/Register forms powered by Bootstrap Modals.	auth.js: Captures form data, sends AJAX POST to /auth/login or /auth/register, and handles successful redirection.
customer-dashboard.html	Section navigation, dynamic tables (My Policies, Claims, Premiums), Profile display.	customer.js: Fetches data from multiple endpoints (e.g., /customers/policies, /customers/claims) and dynamically builds HTML table rows using received JSON arrays.
admin-dashboard.html	Section navigation, dashboard statistics cards, Policies/Claims/Customers management tables.	admin.js: Executes an initial check via /auth/current for ADMIN role. Uses AJAX GET, POST, PUT, DELETE calls for all CRUD operations on policy and claim data.


6. Project Setup and Execution Guide


6.1 Database Configuration
1.	Create a new MySQL database: CREATE DATABASE PolicyHub;
2.	Import the provided SQL dump (MySQL_Dump.sql).
3.	Verify the database tables (user, policy, claim, etc.) are created.


6.2 Configure application.properties
Ensure the following configurations match your environment:
Property	Value
server.port	8082
spring.datasource.url	jdbc:mysql://localhost:3306/PolicyHub?createDatabaseIfNotExist=true
spring.datasource.username	root
spring.datasource.password	Ram54321@
spring.jpa.hibernate.ddl-auto	update


6.3 Building and Running the Application
1.	Start the Spring Boot backend application.
2.	Once started, the API endpoints are available on http://localhost:8082/api.
3.	Access the frontend by opening index.html in your web browser.


7. Testing and Validation
Testing was primarily manual and integration-focused, validating the end-to-end functionality from the browser UI through the REST layer to the database.
Key Test Scenarios Covered:
Test Case	Result
User registration and login	✅ Passed
Policy creation and enrollment	✅ Passed
Claim submission 	✅ Passed
Logout and session expiry	✅ Passed


8. Conclusion
The Insurance Management System (PolicyHub) successfully demonstrates a decoupled full-stack implementation using Spring Boot (REST API) and a HTML/CSS/JavaScript/jQuery frontend. This modular design ensures efficient data handling, role-based access control, and a scalable architecture.
