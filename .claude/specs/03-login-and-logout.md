# Spec: Login and Logout

## Overview

Implement user login and logout so registered users can authenticate and end their sessions. This step adds the `POST /login` route that processes the login form already present in `login.html`, verifies credentials against the database, establishes a Flask session on success, and implements the `GET /logout` stub that clears the session. It also makes the navbar in `base.html` session-aware — showing Sign in / Get started to guests, and a Logout link to authenticated users.

---

## Depends on

- Step 1 — Database Setup (`users` table, `get_db()`, and `get_user_by_email()` must exist)
- Step 2 — Registration (`session['user_id']` pattern and `app.secret_key` established there)

---

## Routes

- `GET /login` — already implemented, renders `login.html` — public
- `POST /login` — **new** — processes login form — public
  - Reads `email` and `password` from `request.form`
  - Validates both fields are non-empty
  - Looks up user by email via `get_user_by_email()`
  - Verifies password hash with `werkzeug.security.check_password_hash`
  - On success: sets `session['user_id']`, redirects to `url_for('landing')`
  - On failure: re-renders `login.html` with a generic `error=` message (do not distinguish between "no account" and "wrong password")
- `GET /logout` — **implement stub** — logged-in only — clears session, redirects to `url_for('landing')`

---

## Database changes

No database changes. All required helpers (`get_user_by_email`) already exist from Step 2.

---

## Templates

- **Modify:** `templates/login.html`
  - Fix hardcoded `action="/login"` → `action="{{ url_for('login') }}"`
  - No other changes — `{% if error %}` block and all form fields are already in place

- **Modify:** `templates/base.html`
  - Update the `nav-links` div to be session-aware:
    - If `session['user_id']` is set: show only a Logout link (`url_for('logout')`)
    - If not set: show Sign in (`url_for('login')`) and Get started (`url_for('register')`) as currently

---

## Files to change

- `app.py` — add `POST` to the `/login` route's `methods`, implement login logic, implement `/logout`
- `templates/login.html` — fix hardcoded form `action` URL
- `templates/base.html` — make navbar session-aware

---

## Files to create

None.

---

## New dependencies

No new pip packages. Uses:
- `werkzeug.security.check_password_hash` (already installed)
- `flask.session`, `flask.request`, `flask.redirect`, `flask.url_for` (already imported)

---

## Rules for implementation

- No SQLAlchemy or ORMs
- Parameterised queries only — no f-strings or string formatting in SQL
- Passwords verified with `werkzeug.security.check_password_hash` — never compare plaintext
- Use CSS variables — never hardcode hex values
- All templates extend `base.html`
- All DB logic stays in `database/db.py` — no SQL in route functions
- Use `abort()` for unrecoverable server errors; re-render template with `error=` for login failures
- `url_for()` for every internal link and form `action` — never hardcode paths
- Logout must call `session.clear()` (not `session.pop`) to wipe all session data

---

## Definition of done

- [ ] Submitting the login form with valid credentials sets `session['user_id']` and redirects to the landing page
- [ ] Submitting with an unknown email re-renders the login form with a generic error (no crash)
- [ ] Submitting with a wrong password re-renders the login form with the same generic error (no account/password distinction)
- [ ] Submitting with an empty email or password field re-renders the form with a validation error
- [ ] Visiting `/logout` clears the session and redirects to the landing page
- [ ] After logout, `session['user_id']` is no longer set
- [ ] The navbar shows "Sign in" and "Get started" to unauthenticated users
- [ ] The navbar shows only "Logout" to authenticated users
- [ ] `login.html` has no hardcoded URLs — `action` uses `url_for('login')`
- [ ] App starts without errors
