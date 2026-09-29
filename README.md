# ⚡ FitBuddy – AI Fitness Plan Generator

FitBuddy is a full-stack, AI-powered web application that leverages Google's Gemini AI to generate personalized 7-day workout plans, custom nutrition & recovery recommendations, and feedback-driven plan iterations based on user physical profiles.

---

## 🌟 Key Features

1. **User Profile & Input Validation**: Accepts User Name, User ID, Age, Weight (kg), Fitness Goal (*Weight Loss, Muscle Gain, General Wellness, Flexibility*), and Workout Intensity (*Low, Medium, High*) with strict input validation.
2. **7-Day Workout Generation (Gemini AI)**: Generates a complete 7-day structured plan with Day 1–7 workout focus, warm-up routines, exercise sets & reps, rest intervals, cooldowns, and safety notes.
3. **Nutrition & Recovery Tip**: Generates concise, goal-oriented nutritional and recovery guidance (protein intake, hydration, sleep, post-workout meals).
4. **Feedback-Based Plan Updating**: Users can submit custom feedback (e.g. *"Add more cardio"*, *"Include rest on Day 3"*, *"Focus on leg exercises"*). Gemini revises the plan while **preserving the original plan** in the SQLite database.
5. **SQLite & SQLAlchemy ORM Database**: Automatically creates and manages relational tables (`User` and `WorkoutPlan`).
6. **Admin & Coach Dashboard (`/view-all-users`)**: View registered users, total statistics counters, search/filter user directory, and inspect original vs. updated workout plans.
7. **REST API & Swagger UI**: Built-in REST API endpoints with auto-generated documentation accessible at `/docs` and `/redoc`.
8. **Modern Glassmorphism UI**: High-energy dark mode user interface built with HTML5, CSS3, Google Fonts (`Outfit` & `Inter`), and responsive design.

---

## 🛠️ Technology Stack

* **Backend**: Python 3.10+, FastAPI, Uvicorn, Pydantic V2
* **Frontend**: Jinja2 Templates, Vanilla HTML5, Custom CSS3 (Glassmorphism design system), JavaScript
* **Database**: SQLite3, SQLAlchemy ORM
* **AI Engine**: Google Gemini API (`google-genai` SDK)
* **Testing**: Pytest, FastAPI TestClient (`httpx`)
* **Environment**: `python-dotenv`

---

## 📁 Project Structure

```text
FITNESS BUDDY/
│
├── app/
│   ├── __init__.py                # Package initializer
│   ├── main.py                    # FastAPI application entry point & lifespan
│   ├── routes.py                  # HTML template routes & REST API endpoints
│   ├── database.py                # SQLAlchemy engine & session management
│   ├── models.py                  # User and WorkoutPlan ORM models
│   ├── schemas.py                 # Pydantic validation schemas
│   ├── gemini_generator.py        # 7-Day workout plan AI generator (Feature 1)
│   ├── gemini_flash_generator.py  # Nutrition & recovery tip AI generator (Feature 2)
│   ├── updated_plan.py            # Feedback-driven plan update AI module (Feature 3)
│   └── config.py                  # Configuration & environment variables
│
├── templates/
│   ├── index.html                 # Homepage with interactive profile form
│   ├── result.html                # 7-Day workout result & nutrition display
│   ├── feedback.html              # Updated plan comparison view
│   ├── all_users.html             # Admin & Coach dashboard
│   └── error.html                 # Error page template
│
├── static/
│   ├── css/
│   │   └── style.css              # Custom glassmorphism stylesheet
│   └── js/
│       └── app.js                 # Frontend interactions & loading handlers
│
├── tests/
│   ├── __init__.py
│   └── test_app.py                # Pytest test suite with mocked AI calls
│
├── .env.example                   # Environment variable template
├── .env                           # Local environment file (API keys)
├── .gitignore                     # Git ignore rules
├── requirements.txt               # Python package dependencies
├── fitbuddy.db                    # Auto-generated SQLite Database file
└── README.md                      # Documentation
```

---

## 🚀 Installation & Setup Guide (Windows)

Follow these step-by-step instructions to set up and run FitBuddy on Windows.

### Step 1: Open Command Prompt or PowerShell
Navigate to your project directory:
```cmd
cd "C:\Users\user\Documents\FITNESS BUDDY"
```

### Step 2: Create and Activate Virtual Environment
```cmd
python -m venv venv
venv\Scripts\activate
```

### Step 3: Install Dependencies
```cmd
pip install -r requirements.txt
```

---

## 🔑 Gemini API Key Configuration

1. Visit [Google AI Studio](https://aistudio.google.com/) and generate a free API key.
2. Copy `.env.example` to `.env`:
   ```cmd
   copy .env.example .env
   ```
3. Open `.env` and set your API key:
   ```env
   GEMINI_API_KEY=your_actual_gemini_api_key_here
   STRONG_GEMINI_MODEL=gemini-2.5-flash
   FAST_GEMINI_MODEL=gemini-2.5-flash
   DATABASE_URL=sqlite:///./fitbuddy.db
   HOST=127.0.0.1
   PORT=8000
   DEBUG=True
   ```

*Note: If no API key is set or if the network is offline, FitBuddy automatically uses its built-in fallback workout generator so the application remains 100% operational.*

---

## ▶️ Running the Application

To start the local Uvicorn development server:

```cmd
uvicorn app.main:app --reload
```

Or run via Python directly:
```cmd
python -m app.main
```

Once running, access the application in your web browser:
* 🌐 **Homepage**: `http://127.0.0.1:8000/`
* 👑 **Admin Dashboard**: `http://127.0.0.1:8000/view-all-users`
* 📖 **Interactive API Docs (Swagger)**: `http://127.0.0.1:8000/docs`
* 📑 **ReDoc Documentation**: `http://127.0.0.1:8000/redoc`

---

## 🧪 Testing

Run the automated test suite with `pytest`:

```cmd
pytest -v
```

All Gemini API calls are mocked during testing to guarantee fast execution without requiring API usage.

---

## 🌐 API Routes Summary

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `GET` | `/` | Renders FitBuddy home page & input form |
| `POST` | `/generate-workout` | Processes user info, calls Gemini AI, saves plan, returns result page |
| `POST` | `/submit-feedback` | Receives User ID + feedback, generates updated plan, preserves original |
| `GET` | `/user-plan/{user_id}` | View workout plan directly for a specific User ID |
| `GET` | `/view-all-users` | Admin dashboard listing all users, statistics, and workout plans |
| `POST` | `/api/generate-workout` | REST API endpoint to generate plan (JSON) |
| `POST` | `/api/submit-feedback` | REST API endpoint to update plan via feedback (JSON) |
| `GET` | `/api/users` | REST API endpoint listing registered users |
| `GET` | `/api/user/{user_id}` | REST API endpoint fetching profile & plan for a single user |

---

## 🛡️ Common Errors & Troubleshooting

1. **`ModuleNotFoundError`**:
   Make sure your virtual environment is activated (`venv\Scripts\activate`) before running `uvicorn` or `pytest`.
2. **`OperationalError: no such table`**:
   The database auto-initializes on startup. Delete `fitbuddy.db` and re-launch `uvicorn app.main:app --reload` to rebuild fresh tables.
3. **Invalid API Key**:
   Verify that your key in `.env` is correct and has access to Gemini models.

---

## 🔮 Future Improvements

* PDF export for 7-day workout schedules.
* Calorie and macro calculator integration.
* Interactive workout completion tracker with progress charts.
* User authentication with JWT tokens.
