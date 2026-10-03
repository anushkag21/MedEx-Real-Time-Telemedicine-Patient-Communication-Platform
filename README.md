# MedEx — Real-Time Telemedicine & Patient Communication Platform

> A full-stack telemedicine platform connecting patients and doctors through appointment management, medical reports, real-time communication, and video consultations.

MedEx is a MERN-based healthcare communication platform designed to streamline interactions between patients and healthcare professionals. The application provides separate workflows for patients and doctors, enabling users to discover doctors, request appointments, manage consultations, exchange medical information, communicate in real time, and conduct video consultations.

---

## ✨ Key Features

### 👤 Patient Portal

- Patient registration and authentication
- Patient profile management
- Search and discover doctors
- Filter doctors by:
  - Name
  - Specialization
  - Location
- Find doctors based on geographical distance
- Send doctor/patient connection requests
- Request appointments
- View upcoming appointments
- Receive appointment and request notifications
- View assigned doctors
- View medical reports
- Submit doctor ratings and reviews
- Real-time communication with doctors
- Video consultation through WebRTC

### 🩺 Doctor Portal

- Doctor registration and authentication
- Doctor profile management
- Specify:
  - Medical specialization
  - Consultation fee
  - Location
  - Availability
- View incoming patient requests
- Accept or reject patient requests
- View connected patients
- Manage appointment requests
- Accept appointment bookings
- View scheduled appointments
- Create and update patient medical reports
- Record:
  - Symptoms
  - Medicines
  - Recommended tests
- View patient information
- Receive patient reviews and ratings
- Real-time patient communication
- Video consultations

### 💬 Real-Time Communication

MedEx uses **Socket.IO** for real-time communication between patients and doctors.

The communication layer supports:

- Room-based conversations
- Real-time message delivery
- Socket-based connection handling
- Patient-doctor communication rooms

### 📹 Video Consultation

The platform includes a WebRTC-based video consultation system.

The implementation uses:

- WebRTC `RTCPeerConnection`
- Browser camera and microphone APIs
- Socket.IO signaling
- STUN servers
- Offer/answer negotiation
- ICE/network negotiation
- Local and remote media streams

This allows a patient and doctor to establish a peer-to-peer audio/video connection.

### 📋 Medical Reports

When a doctor accepts a patient request, MedEx creates a medical report associated with the doctor-patient relationship.

Reports can contain:

- Patient information
- Doctor information
- Symptoms
- Medicines
- Medical tests
- Timestamped records

Patients can subsequently access their reports through the platform.

### 📍 Location-Based Doctor Discovery

Doctor and patient locations are geocoded during registration/profile updates.

The platform stores:

- Latitude
- Longitude
- Location

It then calculates geographical distance between patients and doctors using the **Haversine formula**, allowing doctors to be sorted by proximity.

### ⭐ Doctor Reviews

Patients who are connected with a doctor can submit:

- Star rating
- Written review

The doctor profile maintains aggregate rating information and individual patient reviews.

---

# 🏗️ System Architecture

```text
                         ┌─────────────────────┐
                         │      React UI       │
                         │   Patient / Doctor  │
                         └──────────┬──────────┘
                                    │
                           HTTP / REST API
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │   Express Backend   │
                         │      Node.js        │
                         └───────┬─────┬───────┘
                                 │     │
                   ┌─────────────┘     └─────────────┐
                   ▼                                 ▼
          ┌─────────────────┐              ┌─────────────────┐
          │    MongoDB      │              │    Socket.IO    │
          │                 │              │                 │
          │ Patients        │              │ Real-time Chat  │
          │ Doctors         │              │ Signaling       │
          │ Reports         │              └────────┬────────┘
          │ Appointments    │                       │
          └─────────────────┘                       ▼
                                            ┌─────────────────┐
                                            │     WebRTC      │
                                            │ Video / Audio   │
                                            └─────────────────┘
```

---

# 🛠️ Tech Stack

## Frontend

| Technology | Purpose |
|---|---|
| React 18 | Frontend framework |
| React Router | Client-side routing |
| Redux Toolkit | Global state management |
| Redux Persist | Persistent Redux state |
| Axios | API communication |
| Socket.IO Client | Real-time communication |
| WebRTC | Video/audio consultation |
| React Player | Media stream rendering |
| React Hook Form | Form handling |
| Tailwind CSS | Styling |
| Bootstrap / React Bootstrap | UI components |
| Ant Design | UI components |
| React Hot Toast | User notifications |
| React Icons / Heroicons | Icons |

## Backend

| Technology | Purpose |
|---|---|
| Node.js | Runtime |
| Express.js | REST API |
| MongoDB | Database |
| Mongoose | MongoDB ODM |
| Socket.IO | Real-time communication |
| JSON Web Token | Authentication |
| bcrypt | Password hashing |
| Multer | File uploads |
| Axios | External API requests |
| Zod | Input validation |
| dotenv | Environment configuration |
| CORS | Cross-origin requests |

## Communication

- REST APIs — application data and business operations
- Socket.IO — real-time messaging and WebRTC signaling
- WebRTC — peer-to-peer audio/video communication

---

# 📁 Project Structure

```text
MedEx/
│
├── frontend/
│   ├── public/
│   │
│   └── src/
│       ├── components/
│       ├── constants/
│       ├── context/
│       ├── form/
│       ├── Middleware/
│       ├── Pages/
│       ├── Redux/
│       ├── screens/
│       └── service/
│
├── Backend/
│   ├── controllers/
│   ├── middleware/
│   ├── models/
│   ├── routes/
│   ├── public/
│   │   └── assets/
│   ├── index.js
│   └── package.json
│
└── README.md
```

---

# 🔐 Authentication

MedEx implements separate authentication flows for patients and doctors.

### Registration

Patients and doctors register using dedicated endpoints.

```text
POST /auth/patient/register
POST /auth/doctor/register
```

Passwords are hashed using **bcrypt** before being stored.

### Login

```text
POST /auth/patient/login
POST /auth/doctor/login
```

Successful authentication returns a JWT session token.

The frontend stores the authenticated session information and uses the token for protected API requests.

---

# 🗄️ Data Model

The backend uses MongoDB with Mongoose models.

### Patient

Stores:

```text
Patient
├── fullName
├── email
├── password
├── picturePath
├── location
├── latitude
├── longitude
├── blood
├── age
├── sex
├── doctorList
├── notifications
├── files
└── appointments
```

### Doctor

Stores:

```text
Doctor
├── fullName
├── email
├── password
├── picturePath
├── location
├── latitude
├── longitude
├── patientList
├── appointmentRequests
├── appointments
├── files
├── fee
├── startTime
├── stopTime
├── specialist
├── requests
├── reviews
└── rating
```

### Medical Report

```text
Report
├── basicInformation
├── doctorInformation
├── patientId
├── doctorId
├── medicine
├── symptoms
└── tests
```

The repository also contains models for chat rooms and messages, while the current chat UI primarily uses Socket.IO room-based communication.

---

# 🔄 Core Application Flow

## 1. Patient Registration

```text
Patient
   │
   ├── Personal information
   ├── Location
   ├── Blood group
   ├── Age
   ├── Sex
   └── Profile picture
          │
          ▼
      Backend API
          │
          ├── Password hashing
          ├── Location geocoding
          └── MongoDB
```

---

## 2. Doctor Discovery

```text
Patient
   │
   ▼
Doctor Search
   │
   ├── Name
   ├── Location
   └── Specialization
   │
   ▼
Doctor Profiles
```

Patients can also request doctors sorted by geographical distance.

---

## 3. Doctor-Patient Connection

```text
Patient
   │
   │ Request doctor
   ▼
Doctor
   │
   ├── Accept
   │      │
   │      ▼
   │   Patient ↔ Doctor relationship
   │      │
   │      └── Medical report created
   │
   └── Reject
```

---

## 4. Appointment Workflow

```text
Patient
   │
   ▼
Request Appointment
   │
   ▼
Doctor receives request
   │
   ├── Accept ──► Appointment confirmed
   │
   └── Reject
```

Both users receive relevant notifications through the application.

---

## 5. Medical Report Workflow

After a doctor accepts a patient:

```text
Doctor
   │
   ▼
Patient consultation
   │
   ├── Symptoms
   ├── Medicines
   └── Tests
   │
   ▼
Medical Report
   │
   ▼
Patient can view report
```

---

# 📡 Real-Time Communication

MedEx uses Socket.IO for real-time communication.

The backend creates a Socket.IO server alongside the Express application.

Chat rooms are identified using a combination of patient and doctor identifiers.

The communication flow is approximately:

```text
Patient Browser
      │
      │ Socket.IO
      ▼
Socket.IO Server
      │
      │ Room
      ▼
Doctor Browser
```

Messages are emitted to the relevant room and delivered to connected clients in real time.

---

# 🎥 WebRTC Video Architecture

The video consultation system uses Socket.IO as the signaling layer and WebRTC for peer-to-peer media transfer.

```text
Patient                         Doctor
   │                               │
   │──── Join Room ───────────────►│
   │                               │
   │──── WebRTC Offer ────────────►│
   │                               │
   │◄─── WebRTC Answer ────────────│
   │                               │
   │──── Negotiation ─────────────►│
   │                               │
   │◄══════ WebRTC Media ═════════►│
```

The WebRTC peer connection is configured with public STUN servers to assist with network traversal.

---

# 🌍 Location & Distance Calculation

Doctor discovery supports geographical sorting.

The backend calculates distance using the Haversine formula:

```text
distance = Haversine(
    patient.latitude,
    patient.longitude,
    doctor.latitude,
    doctor.longitude
)
```

Doctors are then returned in ascending order of calculated distance.

Location coordinates are obtained through a geocoding service when users register or update their profiles.

---

# 🚀 Getting Started

## Prerequisites

Make sure the following are installed:

- Node.js
- npm
- MongoDB
- Git

A browser with camera/microphone permissions is required for video consultation functionality.

---

## 1. Clone the Repository

```bash
git clone https://github.com/anushkag21/MedEx-Real-Time-Telemedicine-Patient-Communication-Platform.git

cd MedEx-Real-Time-Telemedicine-Patient-Communication-Platform
```

> Replace the repository URL above if the GitHub repository has a different final URL.

---

# ⚙️ Backend Setup

Navigate to the backend:

```bash
cd Backend
```

Install dependencies:

```bash
npm install
```

Create a `.env` file:

```env
MONGO_URL=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret
PORT=3001
```

Start the backend:

```bash
node index.js
```

The REST API runs on:

```text
http://localhost:3001
```

The WebRTC signaling server runs separately on:

```text
http://localhost:8000
```

---

# 💻 Frontend Setup

Open another terminal:

```bash
cd frontend
```

Install dependencies:

```bash
npm install
```

Start the React development server:

```bash
npm start
```

The frontend will be available at:

```text
http://localhost:3000
```

---

# 🔌 Important Local Ports

| Service | Port |
|---|---:|
| React Frontend | `3000` |
| Express REST API | `3001` |
| WebRTC Signaling Socket | `8000` |
| MongoDB | `27017` by default |

---

# 🔑 Environment Variables

The backend requires environment configuration.

Example:

```env
MONGO_URL=mongodb://localhost:27017/medex
JWT_SECRET=replace_with_a_secure_secret
PORT=3001
```

### Environment variable overview

| Variable | Description |
|---|---|
| `MONGO_URL` | MongoDB connection string |
| `JWT_SECRET` | Secret used to sign JWT authentication tokens |
| `PORT` | Express backend port |

Never commit real credentials or secrets to GitHub.

---

# 🧪 Available Scripts

## Frontend

```bash
npm start
```

Starts the React development server.

```bash
npm run build
```

Creates a production build.

```bash
npm test
```

Runs the React test suite.

---

## Backend

The backend currently starts through:

```bash
node index.js
```

---

# 🔗 Main API Endpoints

## Authentication

```http
POST /auth/patient/register
POST /auth/doctor/register

POST /auth/patient/login
POST /auth/doctor/login
```

## Doctors

```http
GET /doctor/fetchAll
GET /doctor/getdoctor/:doctorId
GET /doctor/fetchByDistance

GET /doctor/getallrequests/:doctorId
GET /doctor/getAppointmentsreq/:doctorId
GET /doctor/getdocAppointments/:doctorId
GET /doctor/getReviews/:doctorId

PATCH /doctor/request/:patientId
PATCH /doctor/booking
POST /doctor/createReport
```

## Patients

```http
GET /patient/getpatient/:patientId
GET /patient/getMyReports/:patientId
GET /patient/getMyDoctors/:patientId
GET /patient/getAppointments/:patientId
GET /patient/sendnotifications/:patientId

PATCH /patient/request/:doctorId
PATCH /patient/booking/:doctorId
PATCH /patient/review/:doctorId

POST /patient/handleNotification
```

## Profile Management

```http
PATCH /edit/patient
PATCH /edit/doctor
```

All endpoints that require authentication use the JWT verification middleware.

---

# 🔒 Protected Routes

The application separates public and authenticated routes on the frontend.

Examples include:

```text
/dashboard
/dashboardP
/doctors
/PatientAPP
/MyDoctors
/Requests
/docappointments
/Prof
/Profd
/viewreport/:doctorId/:patientId
```

The backend also uses JWT-based middleware for protected API endpoints.

---

# 📸 File Uploads

The backend uses **Multer** for profile-picture uploads.

Uploaded assets are served through:

```text
/assets/<filename>
```

Files are stored under:

```text
Backend/public/assets/
```

---

# 🧩 Major Frontend Modules

### Authentication

```text
Pages/
├── LoginP.jsx
├── SignupP.jsx
└── SignupD.jsx
```

### Patient

```text
Pages/
├── DashboardP.jsx
├── PatientAPP.jsx
├── Mydoctors.jsx
├── Patient_Profile.jsx
├── Apprequest.jsx
└── ViewallReport.jsx
```

### Doctor

```text
Pages/
├── Dashboard.jsx
├── Doctor_Profile.jsx
├── Requestpage.jsx
├── Docappointments.jsx
└── Doctor/
    └── Doctor_Desciption.jsx
```

### Communication

```text
Pages/
└── ChatTest.jsx

context/
└── SocketProvider.jsx

screens/
├── Lobby.jsx
└── Room.jsx

service/
└── peer.js
```

---

# 🧠 Backend Architecture

The backend follows a controller-route-model architecture.

```text
Request
   │
   ▼
Express Route
   │
   ▼
JWT Middleware
   │
   ▼
Controller
   │
   ▼
Mongoose Model
   │
   ▼
MongoDB
```

### Controllers

```text
controllers/
├── auth.js
├── chat.js
├── doctor.js
├── patient.js
└── socket.js
```

### Routes

```text
routes/
├── auth.js
├── chat.js
├── doctor.js
└── patient.js
```

### Models

```text
models/
├── Patient.js
├── doctor.js
├── Report.js
├── ChatRoom.js
└── Messages.js
```

---

# 🛡️ Security Considerations

The current implementation includes several foundational security mechanisms:

- Password hashing using bcrypt
- JWT-based authentication
- Protected backend routes
- Protected frontend routes
- CORS configuration
- Request validation using Zod in authentication flow
- Environment variables for MongoDB and JWT configuration

For production deployment, additional hardening should be performed, including:

- Moving all third-party API keys to environment variables
- Stronger request validation
- Rate limiting
- Secure HTTP headers
- HTTPS
- Secure cookie/session strategy where appropriate
- File upload validation
- MIME/type and size restrictions
- More granular authorization checks
- Production CORS configuration
- Removal of development-only localhost URLs

---

# ⚠️ Current Development Notes

This repository is currently structured primarily as a development/prototype application.

Before production deployment, the following areas should be reviewed:

### API URLs

Several frontend components currently reference localhost directly, for example:

```text
http://localhost:3001
http://localhost:8000
```

These should be replaced with environment-based configuration for deployment.

### Third-Party API Key

The geocoding implementation currently contains an API key directly in backend source code.

For production:

```text
GEOCODING_API_KEY=...
```

should be stored in environment variables instead.

### WebRTC Deployment

Production WebRTC deployments generally require:

- HTTPS
- Proper STUN/TURN configuration
- Production signaling infrastructure
- NAT traversal testing

### Real-Time Chat

The current chat implementation uses Socket.IO rooms for live message delivery. The repository contains MongoDB models for chat rooms/messages, but the current chat controller/UI does not yet provide a complete persistent chat-history workflow.

---

# 🚧 Future Improvements

Potential improvements for a production-ready version include:

- Persistent chat history
- Doctor verification
- Patient/doctor authorization scopes
- Appointment conflict detection
- Calendar integration
- Prescription generation
- Medical document uploads
- Search pagination
- Advanced doctor filtering
- Better form validation
- Email/SMS appointment notifications
- Password reset functionality
- Two-factor authentication
- HTTPS/TLS
- TURN server support for WebRTC
- Cloud object storage for medical documents
- Automated testing
- API documentation with OpenAPI/Swagger
- Centralized error handling
- Structured application logging
- Dockerized deployment
- CI/CD pipeline

---

# 📊 Engineering Highlights

MedEx demonstrates implementation across several full-stack engineering areas:

- Full-stack MERN development
- REST API design
- JWT authentication
- Password hashing
- MongoDB data modeling
- Role-based application flows
- Appointment workflow management
- Medical record management
- Real-time Socket.IO communication
- WebRTC peer-to-peer video
- Geolocation and distance calculation
- File upload handling
- Redux state management
- Protected React routes
- Responsive frontend development

---

# 🎯 Project Goals

MedEx was designed around a simple goal:

> **Make doctor-patient communication and remote consultations more accessible through a single digital platform.**

Instead of separating doctor discovery, appointment requests, communication, reports, and video consultations across different systems, MedEx brings these workflows together into one application.

---

# 👥 User Roles

| Role | Capabilities |
|---|---|
| 👨‍⚕️ Doctor | Manage patients, appointments, reports, reviews and consultations |
| 🧑‍💻 Patient | Discover doctors, request appointments, access reports and consult doctors |

---

# 📌 Project Status

**Status:** Development / Prototype

The core application workflows are implemented, including authentication, doctor discovery, patient-doctor relationships, appointment management, reports, real-time communication, and WebRTC video consultation.

Production deployment would require additional security, infrastructure, testing, and persistence improvements.

---

# 📄 License

Add the project's intended license here, for example:

```text
MIT License
```

if the repository is intended to be released under MIT.

---

# 👩‍💻 Author

**Anushka Gupta**

B.Tech — Computer Science & Engineering  
Specialization: Big Data Analytics

GitHub: `@anushkag21`

---

## ⭐ If You Found This Project Useful

Consider giving the repository a ⭐ on GitHub.
