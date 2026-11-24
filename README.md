# AI-Assisted Web-Based Booking Management System for a Local Guest House

> **Rashmika Home Stay** — Maskeliya, Nuwara Eliya, Sri Lanka

---

## Student Details

| Field | Details |
|---|---|
| **Student Name** | Demuna Tharidi Radeesha Dilmi Shehani De Zoysa |
| **Student ID** | 34165517 |
| **Module Code** | CP6SL46O |
| **Programme** | BSc (Hons) Information Technology |
| **Supervisor** | Mr. Mahen Jayalath |
| **Institution** | University of West London, School of Computing and Engineering |

---

## Project Overview

This project delivers a full-stack AI-assisted web-based booking management system for Rashmika Home Stay, a small family-run guesthouse in Maskeliya, Nuwara Eliya — the gateway to Adam's Peak, Sri Lanka.

The system replaces manual paper-based reservation processes with an end-to-end digital platform featuring real-time room availability, secure authentication, automated email notifications, an AI chatbot for 24/7 guest enquiries, and a full admin dashboard.

---

## System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    PRESENTATION LAYER                       │
│          Bootstrap 5  │  Vanilla JS  │  Responsive CSS      │
└───────────────────────┬─────────────────────────────────────┘
                        │
┌───────────────────────▼─────────────────────────────────────┐
│                   APPLICATION LAYER                         │
│    PHP 8.2  │  PDO Prepared Statements  │  Session RBAC     │
│    PHPMailer SMTP  │  Rule-Based AI Chatbot (19 topics)     │
└───────────────────────┬─────────────────────────────────────┘
                        │
┌───────────────────────▼─────────────────────────────────────┐
│                     DATA LAYER                              │
│          MySQL / MariaDB  │  booking_system database        │
│    users │ rooms │ bookings │ (relational schema)           │
└─────────────────────────────────────────────────────────────┘
```

---

## Technology Stack

### Backend
| Component | Technology |
|---|---|
| Server-side language | PHP 8.2 |
| Database | MySQL / MariaDB |
| Database access | PDO with prepared statements |
| Email | PHPMailer 6.x via Gmail SMTP (STARTTLS port 587) |
| Authentication | bcrypt password hashing, session management |
| Access control | Role-Based Access Control (RBAC) — Guest / Admin |

### Frontend
| Component | Technology |
|---|---|
| CSS framework | Bootstrap 5 |
| Scripting | Vanilla JavaScript (ES6+) |
| Chatbot | Rule-based JS (19 keyword-response categories) |
| Design | Responsive, mobile-first layout |

---

## Project Folder Structure

```
AI-Assisted-Web-Based-Booking-Management-System/
│
├── admin/                      # Admin-only pages (RBAC protected)
│   ├── _sidebar.php            # Reusable admin sidebar
│   ├── dashboard.php           # KPI summary cards (bookings, revenue, occupancy)
│   ├── manage-bookings.php     # View / approve / cancel all bookings
│   ├── manage-rooms.php        # Add, edit, delete room listings
│   ├── manage-users.php        # User account management
│   └── update-booking.php      # Update booking status (confirmed/cancelled)
│
├── api/                        # AJAX / JSON API endpoints
│   ├── cancel-booking.php      # Guest self-cancel endpoint
│   └── style.css               # API error page styling
│
├── assets/
│   ├── css/style.css           # Global stylesheet (Rashmika brand colours)
│   ├── images/                 # Hero background + room photos (4 rooms)
│   └── js/chatbot.js           # AI chatbot — 19-topic rule-based engine
│
├── auth/
│   ├── login.php               # Login form + bcrypt verify + session init
│   ├── logout.php              # Session destroy + redirect
│   └── register.php            # Guest registration with bcrypt hashing
│
├── config/
│   ├── db.example.php          # ← COPY to db.php and add your credentials
│   └── db.php                  # ← NOT committed (in .gitignore) — create from example
│
├── includes/
│   ├── footer.php              # Shared page footer
│   ├── header.php              # Shared page header + nav
│   ├── mailer.example.php      # ← COPY to mailer.php and add Gmail App Password
│   ├── mailer.php              # ← NOT committed (in .gitignore) — create from example
│   └── PHPMailer/src/          # PHPMailer library (Exception, PHPMailer, SMTP)
│
├── logs/
│   └── .gitkeep                # Logs folder tracked; email_log.txt excluded
│
├── pages/
│   ├── booking.php             # Room booking form + availability check
│   ├── index.php               # Homepage (hero, rooms preview, chatbot)
│   ├── my-bookings.php         # Guest: view own bookings, cancel
│   └── rooms.php               # Room listings with availability filter
│
├── .gitignore                  # Excludes db.php, mailer.php, logs/, dev scripts
└── README.md                   # This file
```

---

## Database Schema

```sql
-- Create the database
CREATE DATABASE IF NOT EXISTS booking_system CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE booking_system;

-- Users table
CREATE TABLE users (
    id         INT AUTO_INCREMENT PRIMARY KEY,
    name       VARCHAR(100)        NOT NULL,
    email      VARCHAR(150) UNIQUE NOT NULL,
    password   VARCHAR(255)        NOT NULL,   -- bcrypt hash
    role       ENUM('guest','admin') DEFAULT 'guest',
    created_at TIMESTAMP           DEFAULT CURRENT_TIMESTAMP
);

-- Rooms table
CREATE TABLE rooms (
    id           INT AUTO_INCREMENT PRIMARY KEY,
    room_number  VARCHAR(10) UNIQUE NOT NULL,
    room_type    VARCHAR(50)        NOT NULL,
    capacity     INT                NOT NULL,
    price_per_night DECIMAL(10,2)  NOT NULL,
    description  TEXT,
    image_url    VARCHAR(255),
    is_available TINYINT(1)        DEFAULT 1
);

-- Bookings table
CREATE TABLE bookings (
    id           INT AUTO_INCREMENT PRIMARY KEY,
    user_id      INT                NOT NULL,
    room_id      INT                NOT NULL,
    guest_name   VARCHAR(100)       NOT NULL,
    guest_email  VARCHAR(150)       NOT NULL,
    guest_phone  VARCHAR(20),
    check_in     DATE               NOT NULL,
    check_out    DATE               NOT NULL,
    total_price  DECIMAL(10,2)      NOT NULL,
    status       ENUM('pending','confirmed','cancelled') DEFAULT 'pending',
    created_at   TIMESTAMP          DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (room_id) REFERENCES rooms(id) ON DELETE CASCADE
);
```

---

## Setup & Installation

### Prerequisites
- PHP 8.0 or higher
- MySQL 5.7+ / MariaDB 10.4+
- XAMPP / WAMP / LAMP or any PHP-capable web server
- Gmail account with 2-Step Verification and an App Password

### Steps

**1. Clone the repository**
```bash
git clone https://github.com/shehanidilmi52-ctrl/AI-Assisted-Web-Based-Booking-Management-System-for-a-Local-Guest-House.git
cd AI-Assisted-Web-Based-Booking-Management-System-for-a-Local-Guest-House
```

**2. Place files in your web root**
```
XAMPP: C:/xampp/htdocs/Rashmika/
WAMP:  C:/wamp64/www/Rashmika/
Linux: /var/www/html/Rashmika/
```

**3. Set up the database**
- Open phpMyAdmin → Create database: `booking_system`
- Run the SQL schema above (copy into the SQL tab)
- Insert sample room data:
```sql
INSERT INTO rooms (room_number, room_type, capacity, price_per_night, description) VALUES
('101', 'Standard Single', 1, 2500.00, 'Comfortable single room with mountain view'),
('102', 'Standard Double', 2, 4500.00, 'Spacious double room with garden view'),
('103', 'Deluxe Double',   2, 6500.00, 'Deluxe room with private balcony and tea-estate view'),
('104', 'Family Suite',    4, 9500.00, 'Large family suite with two bedrooms and lounge');

-- Create admin user (password: admin123)
INSERT INTO users (name, email, password, role) VALUES
('Admin', 'admin@rashmika.lk', '$2y$10$YourBcryptHashHere', 'admin');
```
*(Generate a real bcrypt hash: `password_hash('admin123', PASSWORD_BCRYPT)`)*

**4. Configure database credentials**
```bash
cp config/db.example.php config/db.php
```
Edit `config/db.php` with your MySQL host, port, database name, username and password.

**5. Configure email (PHPMailer + Gmail)**
```bash
cp includes/mailer.example.php includes/mailer.php
```
Edit `includes/mailer.php`:
- Set `GMAIL_USER` to your Gmail address
- Set `GMAIL_PASS` to your 16-character Gmail App Password
  (Get one at: https://myaccount.google.com/apppasswords)

**6. Open the system**
Navigate to: `http://localhost/Rashmika/pages/index.php`

---

## Key Features

### Guest Features
- Register / login with bcrypt-secured authentication
- Browse rooms with real-time availability filtering
- Book a room (overlap-prevention SQL logic prevents double bookings)
- Receive instant email confirmation via PHPMailer SMTP
- View and cancel own bookings from the My Bookings dashboard
- Use the AI chatbot 24/7 for enquiries (19 topic categories)

### Admin Features
- Secure admin dashboard with KPI summary cards
- Manage all bookings: approve, cancel, update status
- Manage room listings: add, edit, delete rooms
- Manage user accounts
- All status changes trigger automatic email notifications to guests

### AI Chatbot (19 Topic Categories)
Rooms, Pricing, Availability, Booking Process, Check-in/Check-out, Directions,
Adam's Peak, Nearby Attractions, Meals, Parking, Wi-Fi, Family-friendly amenities,
Payment methods, Cancellation policy, Contact details, Weather, Transportation,
Luggage storage, Accessibility

---

## Security Implementation

| Feature | Implementation |
|---|---|
| Password hashing | PHP `password_hash()` / `password_verify()` with `PASSWORD_BCRYPT` |
| SQL injection prevention | PDO prepared statements throughout |
| Session management | Session fixation prevention; role check on every admin page |
| RBAC | `$_SESSION['role'] === 'admin'` guard on all admin routes |
| Credentials | `config/db.php` and `includes/mailer.php` excluded via `.gitignore` |

---

## Testing Results

### Functional Testing — 24 Test Cases (100% Pass Rate)

| TC Range | Feature Area | Result |
|---|---|---|
| TC01–TC04 | User registration & authentication | ✅ PASS |
| TC05–TC08 | Room listing & availability filter | ✅ PASS |
| TC09–TC14 | Booking lifecycle (create/confirm/cancel) | ✅ PASS |
| TC15–TC20 | Admin dashboard & management panels | ✅ PASS |
| TC21–TC22 | AI chatbot responses | ✅ PASS |
| TC23–TC24 | Email notification delivery | ✅ PASS |

### Non-Functional Testing

| Metric | Result | Target |
|---|---|---|
| Page load time | 1.2 seconds | < 3 seconds ✅ |
| Google Lighthouse mobile | 82 / 100 | > 70 ✅ |
| bcrypt password hashing | Verified | Implemented ✅ |
| SQL injection (PDO) | Confirmed blocked | Implemented ✅ |

### User Acceptance Testing — SUS Score

| Metric | Value |
|---|---|
| SUS Score | **74.5 / 100** |
| Grade | **Good** (above 68 industry average) |
| Participants | 10 (mixed technical backgrounds) |
| Key feedback | Chatbot accuracy praised by all; booking form rated highly intuitive; email confirmation improved guest trust |

---

## Questionnaire Findings (n = 35)

| Finding | % |
|---|---|
| Require real-time availability checking | 96% |
| Expect automated booking confirmation emails | 89% |
| Require mobile-friendly responsive design | 85% |
| Interested in AI chatbot for pre-stay enquiries | 72% |

---

## References

1. Buhalis, D. & Law, R. (2008). Progress in information technology and tourism management: 20 years on and 10 years after the internet. *Tourism Management*, 29(4), 609–623.
2. O'Connor, P. & Frew, A.J. (2002). The future of hotel electronic distribution: Expert and industry perspectives. *Cornell Hotel and Restaurant Administration Quarterly*, 43(3), 33–45.
3. Murphy, J., Chen, M. & Cossutta, M. (2017). An investigation of multiple devices and information sources used in hotel booking. *Tourism Management*, 61, 357–369.
4. Brooke, J. (1996). SUS: A quick and dirty usability scale. In P. W. Jordan et al. (Eds.), *Usability Evaluation in Industry*. Taylor & Francis, 189–194.
5. Ramos, C.M., Correia, M.B., Rodrigues, J.M.F. & Cascada, P. (Eds.) (2019). *Information & Communication Technologies in Tourism and Hospitality Industries*. Springer.
6. OWASP (2023). *OWASP Top 10 Web Application Security Risks*. https://owasp.org/Top10/

---

## Repository Contents

| File / Folder | Description |
|---|---|
| `admin/` | Admin panel PHP pages (RBAC protected) |
| `api/` | AJAX API endpoints |
| `assets/css/style.css` | Global stylesheet |
| `assets/images/` | Hero and room images |
| `assets/js/chatbot.js` | Rule-based AI chatbot engine |
| `auth/` | Login, logout, registration |
| `config/db.example.php` | Database config template (copy → db.php) |
| `includes/footer.php` | Shared footer |
| `includes/header.php` | Shared header + navigation |
| `includes/mailer.example.php` | Email config template (copy → mailer.php) |
| `includes/PHPMailer/` | PHPMailer library source |
| `logs/` | Email log directory (contents excluded from git) |
| `pages/` | Public-facing pages (index, rooms, booking, my-bookings) |
| `.gitignore` | Excludes credentials and dev-only files |

---

*University of West London — School of Computing and Engineering — CP6SL46O — 2026*
