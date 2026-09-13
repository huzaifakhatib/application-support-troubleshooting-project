# application-support-troubleshooting-project
# Application Support Troubleshooting Project — WordPress + MySQL on AWS EC2

## Overview
A hands-on Application Support simulation project: deployed a WordPress website backed by a MySQL database on an AWS EC2 instance, then diagnosed and resolved two real incidents — one encountered unexpectedly during setup, and one simulated intentionally to practice incident response.

## Architecture
- AWS EC2 instance (Ubuntu 22.04, t2.micro)
- Apache2 web server + PHP 8.3
- MySQL 8.0 database
- WordPress application
- 1GB swap file configured to handle low-memory conditions on the instance

## Setup Steps
1. Installed Apache, MySQL, and PHP (LAMP stack) on the EC2 instance
2. Secured MySQL using `mysql_secure_installation`
3. Created a dedicated database and user for WordPress
4. Downloaded and configured WordPress, connecting it to the database via `wp-config.php`
5. Completed the WordPress setup wizard and verified the site was live

## Incidents Handled

### Incident 1: Website Down — Database Connection Error

| Field | Details |
|---|---|
| **Severity** | P1 — Critical (full site outage) |
| **Symptom/Impact** | Browser displayed "Error establishing a database connection" when accessing the site |
| **Root Cause** | The MySQL service was stopped (simulated intentionally to practice incident response) |
| **Diagnosis Steps** | Ran `sudo systemctl status mysql`, which confirmed the service was in an inactive/dead state |
| **Resolution** | Restarted the service using `sudo systemctl start mysql` |
| **Verification** | Confirmed the service was active via `systemctl status mysql`, then reloaded the website and confirmed it loaded normally |
| **Prevention** | Configure `Restart=on-failure` in the systemd service file and set up a monitoring alert for service downtime |

## Tools Used
AWS EC2, Ubuntu Linux, Apache2, PHP 8.3, MySQL 8.0, WordPress, Bash/systemd troubleshooting commands (`systemctl`, `dpkg`,  `free`)

## Screenshots
See the `screenshots/` folder for:
- Raw PHP code being displayed (Incident 1 — before fix)
- Website rendering correctly after fix
- Database connection error message (Incident 2 — before fix)
- Website working after MySQL restart
- Terminal output showing diagnosis and resolution commands for both incidents

## Key Learnings
- Diagnosing broken/incomplete package installations using `dpkg --configure -a`
- Understanding and resolving Out-of-Memory (OOM) issues on resource-constrained servers by configuring swap space
- Enabling and troubleshooting Apache modules (PHP integration)
- Following a structured incident response process: Detect → Diagnose → Resolve → Verify → Document
- Writing clear Root Cause Analysis (RCA) documentation for technical incidents
