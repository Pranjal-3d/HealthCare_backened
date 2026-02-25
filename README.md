# Healthcare Backend - Django REST API

## Project Structure
```
healthcare_backend/
├── healthcare_backend/      # Main Django project
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── authentication/          # Auth app (register/login)
├── patients/                # Patient management
├── doctors/                 # Doctor management
├── mappings/                # Patient-Doctor mapping
├── manage.py
├── requirements.txt
└── .env                     # Your environment variables (create this)
```

---

## Step-by-Step Setup in VS Code

### 1. Install Prerequisites
- [Python 3.10+](https://www.python.org/downloads/)
- [PostgreSQL](https://www.postgresql.org/download/)
- [VS Code](https://code.visualstudio.com/) with Python extension

### 2. Open Project in VS Code
```bash
# Open this folder in VS Code
code healthcare_backend/
```

### 3. Create Virtual Environment
Open the **VS Code Terminal** (`Ctrl + ~`) and run:
```bash
python -m venv venv

# Activate it:
# On Windows:
venv\Scripts\activate
# On Mac/Linux:
source venv/bin/activate
```

### 4. Install Dependencies
```bash
pip install -r requirements.txt
```

### 5. Create PostgreSQL Database
Open pgAdmin or psql terminal:
```sql
CREATE DATABASE healthcare_db;
```

### 6. Create `.env` File
Copy `.env.example` to `.env` and fill in your values:
```bash
cp .env.example .env
```
Edit `.env`:
```
SECRET_KEY=your-very-secret-key-change-this
DEBUG=True
DB_NAME=healthcare_db
DB_USER=postgres
DB_PASSWORD=your_postgres_password
DB_HOST=localhost
DB_PORT=5432
```

### 7. Run Migrations
```bash
python manage.py makemigrations
python manage.py migrate
```

### 8. Create Superuser (Optional - for Django Admin)
```bash
python manage.py createsuperuser
```

### 9. Run the Server
```bash
python manage.py runserver
```
Server runs at: **http://127.0.0.1:8000**

---

## API Endpoints

### Authentication
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/api/auth/register/` | Register new user | No |
| POST | `/api/auth/login/` | Login & get JWT token | No |

### Patients
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/api/patients/` | Add new patient | Yes |
| GET | `/api/patients/` | Get all your patients | Yes |
| GET | `/api/patients/<id>/` | Get specific patient | Yes |
| PUT | `/api/patients/<id>/` | Update patient | Yes |
| DELETE | `/api/patients/<id>/` | Delete patient | Yes |

### Doctors
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/api/doctors/` | Add new doctor | Yes |
| GET | `/api/doctors/` | Get all doctors | Yes |
| GET | `/api/doctors/<id>/` | Get specific doctor | Yes |
| PUT | `/api/doctors/<id>/` | Update doctor | Yes |
| DELETE | `/api/doctors/<id>/` | Delete doctor | Yes |

### Mappings
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/api/mappings/` | Assign doctor to patient | Yes |
| GET | `/api/mappings/` | Get all mappings | Yes |
| GET | `/api/mappings/<patient_id>/` | Get doctors for patient | Yes |
| DELETE | `/api/mappings/delete/<id>/` | Remove mapping | Yes |

---

## Testing with Postman

### Step 1: Register
**POST** `http://127.0.0.1:8000/api/auth/register/`
```json
{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password123"
}
```

### Step 2: Login
**POST** `http://127.0.0.1:8000/api/auth/login/`
```json
{
    "email": "john@example.com",
    "password": "password123"
}
```
Copy the `access` token from the response.

### Step 3: Use Token in All Other Requests
In Postman → **Authorization** tab → Type: **Bearer Token** → paste your access token.

### Step 4: Add a Patient
**POST** `http://127.0.0.1:8000/api/patients/`
```json
{
    "name": "Alice Smith",
    "age": 30,
    "gender": "F",
    "contact_number": "9876543210",
    "address": "123 Main St",
    "medical_history": "Diabetes Type 2"
}
```

### Step 5: Add a Doctor
**POST** `http://127.0.0.1:8000/api/doctors/`
```json
{
    "name": "Robert Brown",
    "specialization": "Endocrinologist",
    "email": "dr.brown@hospital.com",
    "contact_number": "9123456789",
    "experience_years": 10
}
```

### Step 6: Assign Doctor to Patient
**POST** `http://127.0.0.1:8000/api/mappings/`
```json
{
    "patient": 1,
    "doctor": 1,
    "notes": "Primary care physician"
}
```
