# Desjardin Backend

Flask-based backend API with health-check and rate-limited request handling.
Use only with authorized test data and after removing the unsafe credential-forwarding behavior

## Current behavior

- `GET /` returns a basic health response.
- `POST /api/save` expects a JSON body containing `username` and `password`.
- The current implementation sends both values to Telegram when `TELEGRAM_BOT_TOKEN` and `TELEGRAM_CHAT_ID` are configured.
- Requests are rate-limited in memory.
- Successful requests return a redirect URL configured in `app.py`.

## Recommended remediation

Before using this code for any legitimate application:

1. Remove `send_to_telegram(username, password)` and stop logging or transmitting passwords.
2. Never store or forward plaintext passwords. Use a vetted authentication provider or a properly designed password-hashing flow.
3. Replace the `/api/save` endpoint with a legitimate, documented business operation that does not collect unnecessary secrets.
4. Rotate any Telegram bot token or other credentials that may have been exposed.
5. Update `REDIRECT_URL` and add authentication, validation, audit logging, and production error handling appropriate to the intended service.

## Project files

- `backend/app.py` - Flask application and API routes.
- `backend/requirements.txt` - Python dependencies.
- `backend/Procfile` - Gunicorn process definition.

## Dependencies

- Flask
- Flask-CORS
- Flask-Limiter
- Gunicorn

## Configuration

The current code reads these environment variables:

- `TELEGRAM_BOT_TOKEN` - Telegram bot token. Do not configure this for real user passwords.
- `TELEGRAM_CHAT_ID` - Telegram destination chat ID. Do not configure this for real user passwords.
- `PORT` - Optional HTTP port; defaults to `5000`.

## Local development

Create a virtual environment, install the dependencies, and run the Flask application only with test data in an isolated environment:

```powershell
cd backend
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python app.py
```

The health endpoint is available at `http://localhost:5000/`.

## License

No license has been specified for this project.
