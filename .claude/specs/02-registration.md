# Spec: Registration

## Overview

Implement user registration so new visitors can create a Spendly account. This step adds the `POST /register` route that processes the registration form already present in `register.html`, validates input, checks for duplicate emails, hashes the password, persists the user, and logs them in via a Flask session. It also introduces `app.secret_key` (required for sessions) and two new DB helpers in `database/db.py`.

---

## Depends on

- Step 1 — Database Setup (`users` table and `get_db()` must exist)

---

## Routes

- `GET /register` — already implemented, renders `register.html` — public
- `POST /register` — **new** — processes registration form — public
  - Validates: name, email, password all required; password ≥ 8 characters
  - Checks email is not already registered (return error if duplicate)
  - Hashes password with `werkzeug.security.generate_password_hash`
  - Inserts user via `create_user()` DB helper
  - Sets `session['user_id']` on success
  - Redirects to `url_for('landing')` on success
  - Re-renders `register.html` with `error=` message on failure

---

## Database changes

No schema changes. Two new helper functions added to `database/db.py`:

- `get_user_by_email(email)` — returns a `sqlite3.Row` or `None`
- `create_user(name, email, password_hash)` — inserts a new user row, returns the new `id`

---

## Templates

- **Modify:** `templates/register.html`
  - Change `action="/register"` → `action="{{ url_for('register') }}"` (hardcoded URL violates CLAUDE.md)
  - No other changes — `{% if error %}` block and all form fields are already in place

- **Modify:** `templates/base.html`
  - No changes in this step — nav session-awareness is deferred to Step 3 (logout)

---

## Files to change

- `app.py` — add imports, `secret_key`, `POST /register` route, update `GET /register` to accept `methods=["GET"]` explicitly
- `database/db.py` — add `get_user_by_email()` and `create_user()`
- `templates/register.html` — fix hardcoded `action` URL

---

## Files to create

- None

---

## New dependencies

No new pip packages. Uses:
- `werkzeug.security.generate_password_hash` (already installed)
- `flask.session`, `flask.request`, `flask.redirect` (already available)

---

## Rules for implementation

- No SQLAlchemy or ORMs
- Parameterised queries only — no f-strings or string formatting in SQL
- Passwords hashed with `werkzeug.security.generate_password_hash`
- All templates extend `base.html`
- Use CSS variables — never hardcode hex values
- `secret_key` must be set before any `session` use — set on the `app` object directly in `app.py`
- All DB logic in `database/db.py` — no SQL in route functions
- Use `abort(400)` / `abort(500)` for unrecoverable errors; re-render template with `error=` for user-facing validation failures
- `url_for()` for every internal link and form `action` — never hardcode paths

---

## Definition of done

- [ ] Submitting the registration form with valid data creates a new user in the database
- [ ] The new user's password is stored as a hash, not plaintext
- [ ] `session['user_id']` is set after successful registration
- [ ] Successful registration redirects to the landing page
- [ ] Submitting with a duplicate email re-renders the form with an error message (no crash)
- [ ] Submitting with an empty field re-renders the form with an error message
- [ ] Submitting with a password shorter than 8 characters re-renders the form with an error message
- [ ] App starts without errors
- [ ] No hardcoded URLs remain in `register.html`
