# Airline Seat Reservation System (ASRS)

A web-based airline seat reservation system built with PHP, MySQL, Bootstrap, and jQuery. This application allows users to book airline seats, manage reservations, and generate QR codes for boarding passes.

---

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Database Schema](#database-schema)
- [Usage](#usage)
- [Screenshots](#screenshots)
- [Security Considerations](#security-considerations)
- [Contributors](#contributors)
- [License](#license)

---

## Features

### User Features
- **User Registration & Login** - Secure authentication system with email/password
- **Profile Management** - Update personal information and upload KYC documents (PDF)
- **PNR-based Booking** - Link existing flight bookings using PNR numbers
- **Interactive Seat Selection** - Visual seat map with real-time availability
- **Payment & Ticketing** - View booking details with QR code generation
- **Booking History** - Track pending, completed, and boarded reservations

### Airport Agent Features
- **Agent Login** - Separate authentication portal for airport staff
- **Booking Management** - View and manage passenger reservations

### System Features
- **Responsive Design** - Mobile-friendly Bootstrap interface
- **QR Code Generation** - Generate scannable QR codes for boarding passes
- **Session Management** - Secure PHP session handling
- **Print Ticket** - Printable ticket format

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Frontend** | HTML5, CSS3, JavaScript, jQuery, Bootstrap 4.5 |
| **Backend** | PHP 7+ |
| **Database** | MySQL |
| **Libraries** | jQuery Seat Charts, QRCode.js, Font Awesome |
| **Server** | Apache/Nginx |

---

## Project Structure

```
ASRS/
├── about.php              # About Us page
├── bookings.php           # Pending bookings list
├── contact.php            # Contact information page
├── footer.php             # Common footer component
├── head.php               # Common head/meta tags component
├── headerasrs.php         # Navigation header component
├── index.html             # QR Code generator demo
├── layout.css             # Seat chart styling
├── layout.html            # Seat selection template
├── layout.js              # jQuery Seat Charts library
├── layout.php             # Main seat selection page
├── loginasrs.php          # Login page (User & Agent)
├── logout.php             # Session logout handler
├── main.php               # Homepage with carousel
├── message.php            # Alert/notification component
├── payment.php            # Payment & ticket details
├── pnrinfo.php            # PNR linking page
├── qrcode.js              # QR Code generation library
├── qrcode.min.js          # Minified QR Code library
├── qrcode.php             # PHP QR Code implementation
├── register.php           # User & Agent registration
├── rprofile.php           # User profile management
├── script.js              # Hamburger menu animation
├── Userpage.html          # User dashboard
├── bower.json             # Package configuration
├── jquery.min.js          # jQuery library
├── css/
│   ├── style.css          # Custom styles
│   └── styles.css         # Additional styles
├── image/
│   ├── ASR1.jpg - ASR12.jpg   # Banner images
│   └── ASR6.png               # Logo/favicon
├── file/
│   ├── agentlogin.php     # Agent authentication
│   ├── agentreg.php       # Agent registration handler
│   ├── booking_connection.php  # Database connection
│   ├── bookind_add.php    # Add booking handler
│   ├── delete.php         # Delete booking handler
│   ├── depart_from_to.php # Flight details helper
│   ├── get_accomodation.php   # Class info helper
│   ├── updateprofile.php  # Profile update handler
│   ├── userlogin.php      # User authentication
│   └── userreg.php        # User registration handler
└── phpqrcode/             # PHP QR Code library
```

---

## Installation

### Prerequisites
- PHP 7.0 or higher
- MySQL 5.6 or higher
- Apache/Nginx web server
- Web browser with JavaScript enabled

### Step-by-Step Setup

1. **Clone/Download** the project to your web server directory:
   ```bash
   # For XAMPP
   cp -r ASRS /opt/lampp/htdocs/

   # For WAMP
   cp -r ASRS C:/wamp64/www/
   ```

2. **Create the database**:
   ```sql
   CREATE DATABASE airliness;
   ```

3. **Import database tables** (create the following tables):
   - `user` - User accounts
   - `agent` - Airport agent accounts
   - `passenger_details` - Flight passenger information
   - `user_bookings` - User-booking associations

4. **Configure database connection**:
   Edit `file/booking_connection.php`:
   ```php
   $servername = "localhost";
   $username = "root";
   $password = "your_password";
   $dbname = "airliness";
   ```

5. **Set permissions**:
   ```bash
   chmod 755 -R ASRS/
   chmod 777 ASRS/images/  # For QR code generation
   ```

6. **Access the application**:
   Open `http://localhost/ASRS/main.php` in your browser.

---

## Database Schema

### User Table
```sql
CREATE TABLE user (
    id INT AUTO_INCREMENT PRIMARY KEY,
    uname VARCHAR(30) NOT NULL,
    uemail VARCHAR(30) UNIQUE NOT NULL,
    upassword VARCHAR(255) NOT NULL,
    uphone VARCHAR(12) NOT NULL,
    ucity VARCHAR(20) NOT NULL,
    pdf_file VARCHAR(255)
);
```

### Agent Table
```sql
CREATE TABLE agent (
    id INT AUTO_INCREMENT PRIMARY KEY,
    aid VARCHAR(12) UNIQUE NOT NULL,
    aname VARCHAR(30) NOT NULL,
    aemail VARCHAR(30) UNIQUE NOT NULL,
    apassword VARCHAR(255) NOT NULL,
    aphone VARCHAR(12) NOT NULL,
    acity VARCHAR(20) NOT NULL
);
```

### Passenger Details Table
```sql
CREATE TABLE passenger_details (
    p_pnr VARCHAR(20) PRIMARY KEY,
    p_name VARCHAR(50),
    p_age INT,
    p_sex VARCHAR(10),
    p_fno VARCHAR(20),
    p_from VARCHAR(50),
    p_to VARCHAR(50),
    p_dedate DATE,
    p_ardate DATE,
    p_detime TIME,
    p_artime TIME,
    p_status VARCHAR(20),
    p_class VARCHAR(20),
    p_passtype VARCHAR(20),
    p_fid VARCHAR(20),
    p_seats VARCHAR(50),
    p_extra VARCHAR(100),
    uid INT,
    FOREIGN KEY (uid) REFERENCES user(id)
);
```

### User Bookings Table
```sql
CREATE TABLE user_bookings (
    ubid INT AUTO_INCREMENT PRIMARY KEY,
    uid INT,
    p_pnr VARCHAR(20),
    FOREIGN KEY (uid) REFERENCES user(id),
    FOREIGN KEY (p_pnr) REFERENCES passenger_details(p_pnr)
);
```

---

## Usage

### For Passengers

1. **Register** at `register.php` - Create a user account with KYC document upload
2. **Login** at `loginasrs.php` - Access your dashboard
3. **Link PNR** at `pnrinfo.php` - Add your flight booking using PNR number
4. **Select Seat** at `bookings.php` - Choose available seats from interactive map
5. **Payment** at `payment.php` - View ticket details and QR code
6. **Print Ticket** - Use browser print function for physical copy

### For Airport Agents

1. **Register** as agent with personal ID
2. **Login** via agent tab
3. **Manage** passenger check-ins and boarding

---

## Key Components

### Seat Selection System (`layout.php`)
- Interactive seat map using jQuery Seat Charts
- Real-time seat availability (green = available, red = unavailable)
- Price calculation with cart functionality
- Keyboard navigation support (arrow keys, spacebar)

### QR Code Generation (`payment.php`)
- Generates QR code containing PNR information
- Uses QRCode.js library for client-side generation
- Alternative PHP-based generation available

### Authentication System
- Session-based authentication
- Separate login portals for users and agents
- Password validation with minimum length requirements

---

## Security Considerations

> ⚠️ **Important**: This is a student/academic project. The following security improvements are recommended for production use:

1. **SQL Injection** - Use prepared statements instead of string concatenation
2. **Password Hashing** - Implement `password_hash()` instead of plaintext storage
3. **XSS Protection** - Sanitize all user inputs with `htmlspecialchars()`
4. **CSRF Tokens** - Add CSRF protection to all forms
5. **File Upload** - Validate and restrict file types/sizes for KYC uploads
6. **HTTPS** - Enable SSL/TLS for all communications
7. **Input Validation** - Strengthen client-side and server-side validation

---

## Contributors

| Name | Contact | Email |
|------|---------|-------|
| Chandana GV | +91-9902477354 | gchandana7799@gmail.com |
| Rithish Reddy | +91-9380073437 | rithishreddy7007@gmail.com |
| Jasti SriHarsha | +91-9110342158 | sri814823@gmail.com |
| Balaji Subhash | +91-7036739735 | gbalajisubash@gmail.com |
| Bilakanti Varun | +91-8265739735 | billannara@gmail.com |
| CH Mounish | +91-8374827408 | k.bsairam1210@gmail.com |

---

## License

This project is created for educational purposes. 

Copyright © 2021 ASRS. All Rights Reserved.

---

## Acknowledgments

- [jQuery Seat Charts](https://github.com/mateuszmarkowski/jQuery-Seat-Charts) - Interactive seat map library
- [QRCode.js](https://github.com/davidshimjs/qrcodejs) - QR code generation
- [Bootstrap](https://getbootstrap.com/) - Frontend framework
- [jQuery](https://jquery.com/) - JavaScript library

---

## Support

For issues or questions, please contact the contributors listed above or raise an issue in the project repository.
