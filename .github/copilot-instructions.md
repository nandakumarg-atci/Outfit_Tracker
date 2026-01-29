# OctoFit Tracker - AI Coding Agent Instructions

## Project Overview

OctoFit Tracker is a fitness application for Mergington High School enabling students to log workouts, earn badges, and compete in fitness challenges. The app includes user authentication, activity logging, team management, leaderboards, and personalized workout suggestions.

**Tech Stack:** Django 4.1.7 (REST API) + React (Frontend) + MongoDB

## Architecture & Components

### Backend Structure
- **Framework:** Django REST Framework with MongoDB integration (djongo)
- **Database:** MongoDB with pymongo driver (djongo ORM)
- **Auth:** django-allauth + dj-rest-auth for JWT-based authentication
- **Key features:** REST API endpoints for activities, teams, leaderboards, user profiles

Located in: `octofit-tracker/backend/`
- Backend app code: `octofit_tracker/` (empty until first Django app created)
- Virtual environment: `venv/`
- Dependencies: `requirements.txt` (Django, djangorestframework, mongo packages, etc.)

### Frontend Structure
- **Framework:** React with Bootstrap for UI
- **Routing:** react-router-dom
- **Status:** Frontend directory exists but not initialized yet

Located in: `octofit-tracker/frontend/`

## Critical Development Workflows

### Python Environment Setup
```bash
# Activate virtual environment (MUST do before any Python work)
source octofit-tracker/backend/venv/bin/activate

# Install/update packages
pip install -r octofit-tracker/backend/requirements.txt
```

**Important:** Always point to the backend directory in commands - never change directories when running agent commands.

### Django Development Commands
```bash
# Run migrations (after creating models)
python octofit-tracker/backend/manage.py migrate

# Create superuser
python octofit-tracker/backend/manage.py createsuperuser

# Start dev server (runs on port 8000)
python octofit-tracker/backend/manage.py runserver
```

### React Development Commands
```bash
# Install npm dependencies (always point to frontend directory)
npm install --prefix octofit-tracker/frontend

# Start dev server (runs on port 3000)
npm start --prefix octofit-tracker/frontend

# Build for production
npm build --prefix octofit-tracker/frontend
```

### Testing Endpoints
Use `curl` to test REST API endpoints (documented in backend instructions)

## Forwarded Ports

- **8000** (public): Django backend API
- **3000** (public): React frontend
- **27017** (private): MongoDB - do NOT make public

Do not propose other ports or public/private configurations.

## Project-Specific Conventions

### Django Backend Patterns

**settings.py Requirements:**
```python
import os
ALLOWED_HOSTS = ['localhost', '127.0.0.1']
if os.environ.get('CODESPACE_NAME'):
    ALLOWED_HOSTS.append(f"{os.environ.get('CODESPACE_NAME')}-8000.app.github.dev")
```

**urls.py Pattern:**
```python
import os
codespace_name = os.environ.get('CODESPACE_NAME')
if codespace_name:
    base_url = f"https://{codespace_name}-8000.app.github.dev"
else:
    base_url = "http://localhost:8000"
```

**Serializer Convention:** ObjectId fields must be converted to strings in serializers.py

**CORS Configuration:** django-cors-headers is configured in requirements - ensure proper CORS settings for frontend-backend communication.

### MongoDB Integration with Django

- Use **djongo** as the Django ORM for MongoDB (not direct MongoDB scripts)
- Data structure and creation must go through Django's ORM
- Connection via pymongo driver (djongo abstraction)
- Verify MongoDB is running: `ps aux | grep mongod`

### React Frontend Patterns

Ensure all npm commands include `--prefix octofit-tracker/frontend` to avoid directory changes.

Bootstrap is the CSS framework - import already added in src/index.js:
```javascript
import 'bootstrap/dist/css/bootstrap.min.css';
```

**App Logo:** Use `docs/octofitapp-small.png` for the OctoFit Tracker branding

## Integration Points & External Dependencies

**Key integrations:**
1. **Frontend ↔ Backend:** REST API via fetch/axios (port 8000)
2. **Backend ↔ MongoDB:** djongo ORM (port 27017)
3. **Auth Flow:** django-allauth + dj-rest-auth → JWT tokens

**Environment Configuration:**
- Codespace-aware URLs using `CODESPACE_NAME` environment variable
- Local development: http://localhost:8000 (backend), http://localhost:3000 (frontend)
- Codespace URL: https://{codespace-name}-8000.app.github.dev

## Essential Files to Reference

- [Detailed backend setup](octofit_tracker_setup_project.instructions.md)
- [Django backend conventions](octofit_tracker_django_backend.instructions.md)
- [React frontend setup](octofit_tracker_react_frontend.instructions.md)
- [Project requirements](octofit-tracker/backend/requirements.txt)
- [Project story & context](docs/octofit_story.md)

## Command Reference - Never Change Directories

Always use full paths or `--prefix` flag:
```bash
# ✓ Correct - point to directory
pip install -r octofit-tracker/backend/requirements.txt

# ✓ Correct - use npm prefix flag
npm install --prefix octofit-tracker/frontend

# ✗ Avoid - changing directory
cd octofit-tracker/backend && pip install ...
```
