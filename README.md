# UrbanNest (Society Management Platform)
# 🌌 UrbanNest — Enterprise Society Management Platform
UrbanNest (formerly MyGate Alternative) is a comprehensive, multi-portal society management platform designed to streamline gate security, daily helper tracking, resident amenities, billing, and helpdesk operations. It integrates a secure **Flask (Python)** backend with a real-time **Supabase (PostgreSQL)** database and features three specialized frontend web portals.
UrbanNest (formerly MyGate Alternative) is an enterprise-grade, multi-portal security and community administration platform for modern residential complexes. The system orchestrates daily gate operations, visitor tracking, resident communications, staff shifts, amenities booking, helpdesk ticketing, and automated billing.
It features a decoupled architecture combining a **Flask (Python 3.x)** backend with a real-time **Supabase (PostgreSQL)** database and three specialized client-side Web Portals (Admin, Guard, Resident).

## 🏗️ System Architecture
The platform follows a decoupled Client-Server architecture:
```mermaid
graph TD
    subgraph Frontend Portals (Vanilla JS & CSS)
    subgraph Client Tier (HTML5, Vanilla CSS3, Modern JS)
        A[Resident Portal]
        B[Guard Portal]
        C[Admin Portal]
    end
    subgraph Backend Server (Flask)
        D[Auth Middleware]
        E[API Blueprints]
        F[SMS / Twilio Helper]
        G[Scheduler / Tasks]
    subgraph Service Tier (Flask Application Factory)
        D[Auth Middleware & JWT Validation]
        E[Feature Blueprints]
        F[Twilio / MSG91 SMS Engine]
        G[APScheduler Billing Engine]
        H[ReportLab PDF Engine]
    end
    subgraph Database
        H[Supabase PostgreSQL / RLS]
    subgraph Database Tier (Supabase Cloud)
        I[PostgreSQL Database]
        J[Row-Level Security RLS Rules]
    end
    A & B & C -->|REST API with JWT| D
    A & B & C -->|REST API with Bearer JWT| D
    D --> E
    E --> I
    E --> F
    E --> G
    E --> H
    F -->|Sends SMS/TOTP| A
    E --> F
    I --> J
```
* **Frontend**: Three client-side Single Page Applications (SPAs) built with modern vanilla HTML, CSS, and JavaScript. Shared styling and API request helpers reside in the `shared` directory.
* **Backend**: A Python Flask application serving endpoints grouped via blueprints (modular components). Authentication is enforced via custom decorators validating Supabase JWTs.
* **Database**: Hosted on Supabase with Row Level Security (RLS) tables representing societies, residents, vehicles, visitors, tickets, billing, and daily helpers.
### Key Highlights
- **Client Tier**: Three responsive, CSS-variables-themed client-side apps. They share layouts, utility scripts (`api.js`), and styles in `frontend/shared`.
- **Service Tier**: Modular Flask blueprints utilizing JWT authentication via custom decorators, standardizing error formats, and implementing asynchronous background tasking.
- **Database Tier**: Supabase PostgreSQL with relational database triggers, table constraints, and foreign key cascades.
---
## 🛠️ Technology Stack
## 🔑 Portals & Core Modules
* **Frontend**: HTML5, Vanilla CSS3 (curated theme colors, CSS variables, glassmorphism), Vanilla JavaScript (modern ES6+ async/await, promise-chaining), PWA Service Workers (`sw.js`).
* **Backend**: Flask 3.x, python-dotenv, Supabase Python Client (2.x), pyotp (OTP Generation), APScheduler (automated billing schedules), ReportLab (PDF invoicing and gate pass reports).
* **Communication & Gateway**: Twilio Integration (for real-time verification SMS delivery).
### 1. 🏡 Resident Portal (`/resident/`)
Designed for society homeowners and tenants to manage their household operations.
* **Gate Security & Invites**: Create instant OTP guest passes with expiry windows, or authorize regular delivery platforms.
* **Pre-Approved Cab Clearance**: Pre-register cab plate numbers (e.g., Ola, Uber) to prompt instant entry clearance on the guard portal.
* **Kids Checkout Control**: Toggle permissions for children checking out past gate security, allowing immediate authorization or SMS escalation.
* **Domestic Help Registry**: Browse society-authorized helpers (cooks, maids, drivers), inspect reviews/ratings, link helpers to your flat, and view their real-time gate entry/exit logs.
* **Amenities Booking**: View slots and reserve community spaces (gym, party hall, clubhouse) with integrated double-booking protection.
* **Marketplace Classifieds**: Buy, sell, or giveaway items to other society members directly.
* **Helpdesk Ticketing**: Submit plumbing, carpentry, or electrical requests, upload photo attachments, and track resolution.
* **Billing Center**: View monthly maintenance invoices, utility charges, and perform payment simulations.
---
### 2. 🛡️ Guard Portal (`/guard/`)
A mobile-optimized, fast UI designed for security personnel at gates.
* **Plate & Vehicle Audit**: Look up incoming vehicle license plates to verify resident status.
* **Instant Guest/Visitor Check-In**: Check-in walk-in guests or verify OTP passes.
* **Pre-Approved Alert Banner**: High-visibility alert banners that flash when a pre-approved cab arrives, enabling entry logging in a single tap.
* **SOS Alert Monitoring**: Receive instantaneous alerts if a resident triggers a panic/SOS alarm.
* **Dispute Logs**: Document parking rule violations, blocked spaces, or unrecognized vehicles with logs.
* **Domestic Help Passcode Verification**: Check in domestic helpers using their passcodes.
## 🔑 Portals & Portlets
### 3. ⚙️ Admin Portal (`/admin/`)
The administrative command center for the managing committee and society managers.
* **Society Management**: Manage buildings, register flats, review resident applications, and assign parking spaces.
* **Staff Profiles & Shifts**: Register society plumbers, electricians, and gardeners. Schedule shifts and log attendance.
* **Billing & Dues Management**: Set up billing parameters (flat-rate or area-based), review balance sheets, and trigger monthly maintenance invoices.
* **Helpdesk Dispatch**: View open issues, assign maintenance staff, log progress, and upload photo completion proof.
* **Audits**: Pull historical logs of visitors, vehicle transits, and helper entries.
### 1. Resident Portal (`/frontend/resident/`)
Designed for homeowners and tenants to manage daily flat operations.
* **Visitor Passes**: Generate OTPs with validation durations for visitors, delivery agents, or guest invitations.
* **Pre-Approved Cab Auto-Clearance**: Pre-register cab plate numbers (Ola, Uber, etc.) to trigger instant gate approvals.
* **Community Daily Helper Directory**: Browse registered helper services (maids, cooks, drivers), read ratings & text reviews, link helpers to a flat, and view their attendance check-in/out timestamps.
* **Helpdesk Ticketing**: Log flat issues (plumbing, electrical), track resolution statuses, and view photo proofs of resolved jobs.
* **Billing & Payments**: Review invoices (maintenance, amenities) and perform simulated payments.
* **Amenity Booking**: Schedule reservations for shared spaces (clubhouse, gym).
* **Notification Center**: View a historical list of flat gate alerts and messages.
### 2. Guard Portal (`/frontend/guard/`)
An optimized interface for gate security personnel.
* **Vehicle Lookup**: Query incoming license plate numbers to instantly verify if they are registered resident vehicles.
* **Pre-Approved Cab Banner**: Displays a bright confirmation alert for pre-registered cabs with a one-click "Approve & Log Entry" feature.
* **Visitor Check-In/Check-Out**: Input visitor details, select flat destinations, and verify OTP security passes.
* **Dispute Logs**: Document parking conflicts, unauthorized parking spots, or unknown vehicles.
### 3. Admin Portal (`/frontend/admin/`)
The command center for society management and committee members.
* **Society Management**: Register new flats, activate/deactivate resident profiles, and review member directories.
* **Billing & Invoices**: Generate custom invoices or schedule society-wide monthly maintenance fees.
* **Helpdesk Operations**: Assign incoming tickets to maintenance staff, post resolution notes, and capture photographic work proof using the integrated camera interface.
* **Security & Audits**: View system-wide visitor history logs and daily helper directory listings.
---
## 📂 Directory Structure
## 📂 Directory Layout
```text
├── backend/
│   ├── app.py                # Flask Application Factory
│   ├── config.py             # Configures Supabase, Twilio, MSG91, Razorpay, VAPID
│   ├── scheduler.py          # APScheduler rules for automated billing runs
│   ├── blueprints/           # Modular API endpoints
│   │   ├── admin.py          # Admin actions, profile management, stats
│   │   ├── auth.py           # User enrollment, logins, OTP, JWT refresh
│   │   ├── billing.py        # Invoicing, payments, Razorpay callbacks
│   │   ├── domestic_help.py  # Helper registration, ratings, directory
│   │   ├── helpdesk.py       # Flat tickets, status updates, attachments
│   │   ├── vehicles.py       # Parking spots, disputes, plates lookup
│   │   └── visitors.py       # OTP generation, check-in, auto-clearance
│   ├── utils/                # System utilities (auth_middleware, Twilio SMS)
│   ├── app.py                # Flask application factory
│   ├── config.py             # Environment configurations
│   ├── requirements.txt      # Backend Python dependencies
│   └── scheduler.py          # Recurring tasks (monthly billing auto-runs)
│   │   ├── admin.py          # Resident management, stats, dashboard configs
│   │   ├── auth.py           # SMS OTP, passwordless logins, JWT auth
│   │   ├── billing.py        # Invoicing, payments, payment gateway integration
│   │   ├── domestic_help.py  # Helper registry, reviews, ratings
│   │   ├── helpdesk.py       # Ticketing, updates, staff assignments
│   │   ├── vehicles.py       # Parking spots, vehicle logs, disputes
│   │   ├── visitors.py       # OTP invites, guard guest log validations
│   │   └── ... (kids-checkout, marketplace, security-alerts, reports)
│   └── utils/
│       ├── auth_middleware.py # Decorator enforcing role-based JWT validation
│       └── sms.py            # Twilio / MSG91 SMS verification engine
├── frontend/
│   ├── admin/                # Admin SPA files & scripts
│   ├── guard/                # Guard SPA files & scripts
│   ├── resident/             # Resident SPA, service workers, manifests
│   └── shared/               # Shared stylesheets, logo assets, and api.js
├── run.py                    # Application startup script
└── .env                      # Application environment variables (gitignored)
│   ├── admin/                # Admin SPA files (HTML, CSS, JS)
│   ├── guard/                # Guard SPA files (HTML, CSS, JS)
│   ├── resident/             # Resident SPA (with service worker & PWA manifests)
│   └── shared/               # Shared stylesheets, colors, and API client scripts
├── database_schema.sql       # Raw PostgreSQL schema structure for Supabase imports
├── run.py                    # Server startup script
└── .env                      # Local environment configurations (gitignored)
```
---
## ⚡ Setup & Run Instructions
## 🛠️ Data Model (Schema Overview)
The database schema, defined in [database_schema.sql](file:///Users/bhavana/Downloads/Mygate-alternative-main/database_schema.sql), consists of 35 tables. The core tables include:
|
 Table 
|
 Purpose 
|
 Key Relations 
|
|
:---
|
:---
|
:---
|
|
`societies`
|
 Primary society configuration, balance sheets, and documents 
|
 
|
|
`buildings`
|
 Blocks/towers inside a society 
|
`societies(id)`
|
|
`flats`
|
 Individual flats, square footage, and configuration 
|
`buildings(id)`
, 
`societies(id)`
|
|
`user_profiles`
|
 User profiles mapping to Supabase auth accounts 
|
`flats(id)`
, 
`societies(id)`
|
|
`visitor_otps`
|
 Visitor OTP codes issued by residents 
|
`flats(id)`
, 
`auth.users(id)`
|
|
`visitor_logs`
|
 Real-time entry logs validated at gates 
|
`flats(id)`
, 
`auth.users(guard_id)`
|
|
`vehicles`
|
 Resident vehicles and registered license plates 
|
`flats(id)`
, 
`auth.users(owner_id)`
|
|
`domestic_helpers`
|
 Directory of verified helpers 
|
`flats(id)`
, 
`societies(id)`
|
|
`tickets`
|
 Helpdesk issues and completion logs 
|
`flats(id)`
, 
`auth.users(created_by)`
|
|
`invoices`
|
 Billing statements issued to flats 
|
`flats(id)`
, 
`societies(id)`
|
|
`staff_profiles`
|
 Registry of society electricians, plumbers, etc. 
|
`societies(id)`
|
|
`security_alerts`
|
 SOS events triggered by residents 
|
`flats(id)`
, 
`auth.users(triggered_by)`
|
---*
## ⚡ Setup & Installation
### 1. Prerequisites
Ensure you have Python 3.10+ installed.
- **Python 3.10+**
- **pip** (Python package installer)
- A **Supabase** account (Free tier is perfect)
### 2. Install Dependencies
Navigate to the root directory and install requirements:
### 2. Install Backend Dependencies
Clone the repository and install dependencies from the root directory:
```bash
pip install -r backend/requirements.txt
```
### 3. Configure Environment Variables
Create a `.env` file in the project root directory with the following variables:
### 3. Initialize the Database
1. Go to your **Supabase Dashboard** > **SQL Editor**.
2. Create a new query.
3. Paste the contents of [database_schema.sql](file:///Users/bhavana/Downloads/Mygate-alternative-main/database_schema.sql) and execute it. This configures the tables, registers the default society (`UrbanNest Heights`), builds the default building (`Tower A`), and registers a default flat (`101`).
### 4. Create the Environment File
Create a `.env` file in the root directory:
```env
SUPABASE_URL="https://your-supabase-project.supabase.co"
SUPABASE_ANON_KEY="your-anon-key"
# Supabase Configuration
SUPABASE_URL="https://your-project-id.supabase.co"
SUPABASE_ANON_KEY="your-anon-public-key"
SUPABASE_SERVICE_ROLE_KEY="your-service-role-key"
FLASK_SECRET_KEY="some-secure-random-string"
# Twilio Configuration (Optional - falls back to Terminal simulation)
# Flask Configuration
FLASK_SECRET_KEY="generate-a-long-random-string"
FLASK_ENV="development"
# Twilio Credentials (Optional - logs to server console if not provided)
TWILIO_ACCOUNT_SID="your-twilio-sid"
TWILIO_AUTH_TOKEN="your-twilio-token"
TWILIO_PHONE_NUMBER="+1234567890"
# Developer Bypass Credentials (For quick portal testing)
# Developer Login Bypass (For testing local portals without real SMS verification)
DEV_ADMIN_PHONE="9999999999"
DEV_ADMIN_OTP="1234"
DEV_GUARD_PHONE="8888888888"
DEV_GUARD_OTP="1234"
DEV_RESIDENT_PHONE="7777777777"
DEV_RESIDENT_OTP="1234"
```
### 4. Run the Application
Start the Flask backend local server:
---
## 🚀 Running Locally
Launch the server using:
```bash
python run.py
```
By default, the server will start at `http://localhost:5001/`.
The server will boot on `http://localhost:5001/` with live debugger active.
### 5. Access the Portals
Once the server is running, access the portals using a web browser:
* **Resident Portal**: [http://localhost:5001/resident/](http://localhost:5001/resident/)
* **Guard Portal**: [http://localhost:5001/guard/](http://localhost:5001/guard/)
* **Admin Portal**: [http://localhost:5001/admin/](http://localhost:5001/admin/)
### Portal Access Links:
- **Resident Portal**: [http://localhost:5001/resident](http://localhost:5001/resident)
- **Guard Portal**: [http://localhost:5001/guard](http://localhost:5001/guard)
- **Admin Portal**: [http://localhost:5001/admin](http://localhost:5001/admin)
> [!WARNING]
> Do not double-click or open the HTML files directly from your file manager (i.e. `file://` protocol). Browsers block local file API requests. Always use the local server links above.
> Always load the portals via the server URL above. Do not open `index.html` files directly through a web browser using the `file://` protocol, as the client-side JavaScript APIs require a valid HTTP origin context for cross-origin requests (CORS) to function.
---
## ⚡ PWA & Offline Support
Both **Resident** and **Guard** portals include progressive web app manifests and service workers:
- Cache key assets (CSS, images, UI templates) for faster loading times.
- Provide a basic fallback page when network requests fail.
- Establish foundation structures for local caching of visitor validation checks during brief gate network interruptions.
---
## 🔒 Security Practices
- **Row Level Security (RLS)**: PostgreSQL tables are set up to reject direct client access outside of role scopes.
- **Backend Authentication**: Flask backend validates the custom authorization headers (`Authorization: Bearer <JWT>`) against Supabase credentials before servicing any requests.
- **Credential Rotation**: Never check in the `.env` configuration file. Keep `SUPABASE_SERVICE_ROLE_KEY` secure.
