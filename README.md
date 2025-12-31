# CareerPilot AI - Agentic AI Career Co-Pilot

A comprehensive, hackathon-ready multi-agent AI system for career development and interview preparation. Built with FastAPI, SQLite, and autonomous AI agents orchestrated through LangGraph-inspired flows.

## 🎯 Features

### 8 Autonomous Agents
1. **Profile Intelligence Agent** - Extracts and validates user profile data
2. **Market Intelligence Agent** - Maps roles to required skills and market demand
3. **Skill Gap Reasoning Agent** - Identifies missing and weak skills
4. **Career Planning Agent** - Generates week-wise learning roadmaps
5. **Mock Interview Agent** - Conducts technical, HR, and behavioral interviews
6. **Coding Practice Agent** - Generates and evaluates coding problems
7. **Aptitude & Reasoning Agent** - Creates and scores aptitude questions
8. **Performance & Feedback Learning Agent** - Analyzes results and provides recommendations

### Frontend Features
- Clean, responsive UI with modern design
- Landing page with feature showcase
- User authentication (Signup/Login)
- Profile setup with resume support
- Domain and role selection
- Interactive dashboard with career readiness score
- Practice areas: Coding, Interviews, Aptitude
- Job and hackathon opportunities
- Real-time feedback and analytics

### Backend Features
- FastAPI REST APIs
- SQLite database with comprehensive schema
- JWT authentication
- Agent orchestration with sequential execution
- Feedback loops for continuous improvement
- Mock data for demo purposes

## 📋 Project Structure

```
careerpilot-ai/
├── backend/
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py                 # FastAPI entry point
│   │   ├── config.py               # Configuration
│   │   ├── database.py             # Database connection
│   │   ├── models.py               # SQLAlchemy models
│   │   ├── schemas.py              # Pydantic schemas
│   │   ├── auth.py                 # Authentication
│   │   ├── utils.py                # Helper functions
│   │   │
│   │   ├── agents/                 # 8 Autonomous Agents
│   │   │   ├── profile_agent.py
│   │   │   ├── market_agent.py
│   │   │   ├── skill_gap_agent.py
│   │   │   ├── planner_agent.py
│   │   │   ├── mock_interview_agent.py
│   │   │   ├── coding_agent.py
│   │   │   ├── aptitude_agent.py
│   │   │   └── feedback_agent.py
│   │   │
│   │   ├── orchestrator/           # Agent Orchestration
│   │   │   └── agent_flow.py       # LangGraph-inspired flow
│   │   │
│   │   └── routes/                 # API Routes
│   │       ├── auth_routes.py
│   │       ├── profile_routes.py
│   │       ├── roadmap_routes.py
│   │       ├── practice_routes.py
│   │       └── opportunity_routes.py
│   │
│   ├── requirements.txt
│   └── careerpilot.db              # SQLite database
│
├── frontend/
│   ├── index.html                  # Landing page
│   ├── login.html
│   ├── signup.html
│   ├── profile.html
│   ├── domain.html
│   ├── dashboard.html
│   ├── practice.html
│   │
│   ├── css/
│   │   └── style.css              # Responsive styling
│   │
│   ├── js/
│   │   ├── auth.js
│   │   ├── profile.js
│   │   ├── dashboard.js
│   │   └── practice.js
│   │
│   └── assets/
│       └── images/                # Placeholder for images
│
└── README.md
```

## 🚀 Quick Start

### Prerequisites
- Python 3.8+
- Modern web browser
- Git

### Backend Setup

1. **Clone and navigate:**
```bash
cd careerpilot-ai/backend
```

2. **Create virtual environment:**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install dependencies:**
```bash
pip install -r requirements.txt
```

4. **Run the server:**
```bash
python -m uvicorn app.main:app --reload
```

Backend will be available at `http://localhost:8000`

**API Documentation:** `http://localhost:8000/docs`

### Frontend Setup

1. **Navigate to frontend:**
```bash
cd careerpilot-ai/frontend
```

2. **Serve static files:**
Using Python's built-in server:
```bash
python -m http.server 3000
```

Frontend will be available at `http://localhost:3000`

## 📚 API Endpoints

### Authentication
- `POST /api/v1/auth/signup` - Create new account
- `POST /api/v1/auth/login` - Login and get JWT token

### Profile
- `POST /api/v1/profiles/setup` - Create/update profile
- `GET /api/v1/profiles/{user_id}` - Get user profile
- `PUT /api/v1/profiles/{user_id}` - Update profile

### Roadmap
- `POST /api/v1/roadmaps/generate/{user_id}` - Generate learning roadmap (triggers all agents)
- `GET /api/v1/roadmaps/{user_id}` - Get user's roadmap
- `GET /api/v1/roadmaps/{user_id}/weekly/{week_number}` - Get weekly tasks

### Practice
- `POST /api/v1/practice/coding/generate` - Generate coding problem
- `POST /api/v1/practice/coding/submit/{user_id}` - Submit code solution
- `POST /api/v1/practice/interview/generate` - Generate interview question
- `POST /api/v1/practice/interview/evaluate/{user_id}` - Evaluate answer
- `POST /api/v1/practice/aptitude/generate` - Generate aptitude question
- `POST /api/v1/practice/aptitude/evaluate/{user_id}` - Evaluate aptitude answer
- `GET /api/v1/practice/stats/{user_id}` - Get practice statistics

### Opportunities
- `GET /api/v1/opportunities/` - List all opportunities
- `GET /api/v1/opportunities/{opportunity_id}` - Get opportunity details
- `POST /api/v1/opportunities/apply/{user_id}` - Apply for opportunity
- `GET /api/v1/opportunities/recommendations/{user_id}` - Get recommended opportunities

## 🤖 Agent Architecture

### Agent Flow

```
User Input
    ↓
Profile Intelligence Agent → Extract & Validate Profile
    ↓
Market Intelligence Agent → Analyze Role Requirements
    ↓
Skill Gap Reasoning Agent → Identify Gaps
    ↓
Career Planning Agent → Generate 12-week Roadmap
    ↓
┌─────────┬──────────────┬──────────────┐
│         │              │              │
Mock      Coding         Aptitude      
Interview Practice       Practice      
Agent     Agent          Agent         
│         │              │              
└─────────┴──────────────┴──────────────┘
    ↓
Feedback Learning Agent → Analyze & Recommend
    ↓
Updated Roadmap + Readiness Score
```

### Agent Input/Output Schemas

Each agent has:
- **Input Schema**: Pydantic model defining required inputs
- **Output Schema**: Structured JSON response
- **State-less Reasoning**: No persistent memory (can add later)
- **Run Method**: Compatible with orchestrator

Example:
```python
# Profile Intelligence Agent
input: {
    "user_id": 1,
    "full_name": "John Doe",
    "education": "B.Tech CS",
    "skills": ["Python", "JavaScript"]
}

output: {
    "user_id": 1,
    "extracted_profile": {...},
    "confidence_scores": {...},
    "missing_fields": [...],
    "extracted_skills": [...]
}
```

## 💾 Database Schema

### Tables
- **users** - User accounts
- **profiles** - User profile information
- **roadmaps** - Learning roadmaps
- **applications** - Job/hackathon applications
- **interview_scores** - Mock interview results
- **coding_submissions** - Coding practice submissions
- **aptitude_scores** - Aptitude test results
- **feedback_logs** - Performance feedback

## 🎨 Frontend Features

### Pages
1. **Landing Page** (index.html)
   - Hero section with CTA
   - Features showcase
   - How it works
   - Testimonials

2. **Authentication** (login.html, signup.html)
   - User registration
   - Credential validation
   - JWT token management

3. **Profile Setup** (profile.html, domain.html)
   - User information collection
   - Role and domain selection
   - Roadmap generation trigger

4. **Dashboard** (dashboard.html)
   - Career readiness score
   - Profile overview
   - Quick practice links
   - Opportunity previews

5. **Practice** (practice.html)
   - Coding problem generation and evaluation
   - Interview question generation and scoring
   - Aptitude question generation and evaluation
   - Real-time feedback

### Styling
- **Modern Design**: Gradient backgrounds, smooth transitions
- **Responsive Layout**: Mobile-first approach
- **Accessibility**: Semantic HTML, proper contrast
- **UX-Focused**: Clear CTAs, intuitive navigation

## 📊 Example Workflows

### 1. New User Journey
1. Signup → Creates user account
2. Complete profile → Profile setup page
3. Select domain/role → Triggers roadmap generation
4. System generates comprehensive plan via agents
5. Access dashboard → See personalized roadmap
6. Start practicing → Get feedback from agents

### 2. Coding Practice Flow
1. User requests coding problem
2. Coding Agent generates problem
3. User submits code
4. Agent evaluates logic and complexity
5. Receives score and feedback
6. Suggestions recorded for improvement

### 3. Interview Preparation
1. User selects interview type
2. Interview Agent generates question
3. User records/types answer
4. Agent evaluates response
5. Provides score and improvement areas
6. Feedback updates readiness score

## 🔧 Customization

### Adding New Skills
Edit `skill_learning_hours` in `skill_gap_agent.py`:
```python
self.skill_learning_hours = {
    'your_skill': 50,  # hours to learn
    ...
}
```

### Modifying Roadmap Duration
In `planner_agent.py`:
```python
self.weeks_per_roadmap = 16  # Change from 12
```

### Extending Question Banks
Add questions to agent's `_initialize_question_bank()` method.

## 📝 Demo Credentials

For testing without signup:
```
Email: demo@example.com
Password: demo123
User ID: 1 (hardcoded for demo)
```

## 🚢 Deployment

### Backend (Heroku/AWS)
```bash
gunicorn -w 4 -b 0.0.0.0:$PORT app.main:app
```

### Frontend (Vercel/Netlify)
- Build: Static HTML/CSS/JS (no build step needed)
- Deploy directory: `frontend/`

## 🛣️ Future Enhancements

1. **LLM Integration**: Replace mock data with OpenAI/Claude API
2. **Vector Database**: Add FAISS for semantic search
3. **Real Interviewer**: Video-based mock interviews
4. **Leaderboard**: Community rankings and achievements
5. **Payment**: Freemium model with advanced features
6. **Mobile App**: React Native or Flutter version
7. **Analytics Dashboard**: Detailed progress tracking
8. **Peer Learning**: Collaboration and study groups

## 📖 Documentation

### Agent Development
- Each agent is self-contained
- Implements `run()` method for orchestrator compatibility
- Can be extended independently
- Uses Pydantic for schema validation

### Adding a New Agent
1. Create `new_agent.py` in `agents/`
2. Define Input/Output Pydantic models
3. Implement agent logic
4. Add to orchestrator `__init__`
5. Create route if needed

## 🐛 Troubleshooting

**Port already in use:**
```bash
lsof -i :8000  # Find process
kill -9 <PID>  # Kill it
```

**Database errors:**
```bash
rm careerpilot.db  # Reset database
```

**CORS issues:**
- Check `ALLOWED_ORIGINS` in `config.py`
- Ensure frontend URL is whitelisted

## 📄 License

MIT License - Feel free to use for learning and hackathons!

## 🙌 Contributing

Contributions welcome! Please follow:
1. PEP 8 for Python
2. Semantic commits
3. Descriptive PR titles
4. Test before submitting

## 📞 Support

For issues or questions:
1. Check existing GitHub issues
2. Create detailed issue with reproduction steps
3. Include Python/Node version info

---

**Built with ❤️ for Career Development**

Perfect for hackathons, coding interviews, and personal career growth!
