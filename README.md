# Uluran Tangan (Donation Web App)

A small Flask-based donation web application (Indonesian) with MySQL backend. The project includes user registration/login and several donation pages.

## Repository structure

- `app.py` and `myapp/app.py` - Flask application entrypoints (duplicate copies; use one)
- `myapp/controllers/controller.py` - Route handlers and view logic
- `myapp/models/model.py` - Database functions (uses `flask_mysqldb`)
- `templates/` and `static/` - HTML templates, CSS and JS assets

## Key features

- User registration and login (passwords hashed using Werkzeug)
- Multiple donation pages (`donasikonten1` .. `donasikonten9`)
- Saving donations into `donasi` table and showing donations on profile

## Prerequisites

- Python 3.8+
- MySQL server

## Setup

1. Create and activate a virtual environment:

```bash
python -m venv venv
# Windows
venv\Scripts\activate
# macOS / Linux
source venv/bin/activate
```

2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Configure MySQL

The app expects a MySQL database named `donation` and the connection configured in `app.py` / `myapp/app.py` (currently set to localhost, root, no password). Update the config if you use different credentials or host.

Suggested SQL schema (run in your MySQL client):

```sql
CREATE DATABASE IF NOT EXISTS donation;
USE donation;

CREATE TABLE users (
  user_id INT AUTO_INCREMENT PRIMARY KEY,
  fullname VARCHAR(255) NOT NULL,
  email VARCHAR(255) NOT NULL UNIQUE,
  no_hp VARCHAR(50),
  password VARCHAR(255) NOT NULL
);

CREATE TABLE donasi (
  id INT AUTO_INCREMENT PRIMARY KEY,
  jumlah_donasi INT NOT NULL,
  nama VARCHAR(255),
  pesan TEXT,
  user_id INT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(user_id)
);
```

4. Run the app

From the project root run (use either `app.py` at root or `myapp/app.py` — they are duplicates):

```bash
python app.py
```

Then open http://127.0.0.1:5000 in your browser.

## Notes and caveats

- There are two copies of the Flask app (`app.py` and `myapp/app.py`). Keep only one to avoid confusion.
- `myapp/models/model.py` imports `mysql` from `myapp.app`. If you run the root `app.py` instead of `myapp/app.py`, ensure imports resolve correctly (or update imports to import from `app` module path you use).
- Passwords are hashed using Werkzeug; the code uses `check_password_hash` / `generate_password_hash`.
- No tests are included.

## Security

- Do NOT run with debug=True in production.
- Set a secure `app.secret_key` and avoid committing real credentials.
- Use environment variables (e.g., via a `.env` file) for DB credentials and secret keys in production.

## Improvements (suggested)

- Deduplicate `app.py` files and centralize app factory pattern
- Add input validation and better error handling
- Add CSRF protection (e.g., Flask-WTF)
- Add unit tests and CI workflow

---
