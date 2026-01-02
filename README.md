# Exam Platform Backend

REST API backend for an exam practice platform that serves enriched question content with intelligent progress tracking and spaced repetition learning.

## 🎯 Overview

This backend serves as the API layer for the exam practice platform, providing:
- **Question Content API**: Serves enriched exam questions from the exam-content-factory repository
- **User Management**: Authentication and authorization with JWT
- **Progress Tracking**: Records and analyzes user performance across attempts
- **Spaced Repetition**: Intelligent question selection using SM-2 algorithm
- **Study Features**: Bookmarking, weak area identification, and personalized recommendations

## 📊 Current Status

🚧 **Project Status**: Initial Setup Phase  
📅 **Created**: January 2, 2026  
🎓 **First Exam**: AWS SAA-C03 (1,003 enriched questions available)

## 🏗️ Architecture

### Tech Stack (To Be Determined)
Options being considered:
- **Option A**: Node.js/TypeScript + Express + PostgreSQL + Prisma
- **Option B**: Python/FastAPI + PostgreSQL + SQLAlchemy

### Core Components
```
┌─────────────────┐
│   REST API      │  ← Express/FastAPI
├─────────────────┤
│   Services      │  ← Business Logic
├─────────────────┤
│  Repositories   │  ← Data Access Layer
├─────────────────┤
│   Database      │  ← PostgreSQL (User Data)
│   + Content     │  ← JSON Files (Questions)
└─────────────────┘
```

### Data Sources
1. **Content Repository** (exam-content-factory): Read-only access to enriched questions
2. **User Database** (PostgreSQL): User accounts, progress, bookmarks, study sessions

## 📋 API Features (Planned)

### Authentication
- `POST /api/v1/auth/register` - User registration
- `POST /api/v1/auth/login` - Login with JWT
- `GET /api/v1/auth/me` - Get current user profile

### Questions
- `GET /api/v1/questions` - List questions with filters (exam, topic, difficulty, tags)
- `GET /api/v1/questions/:id` - Get single question with full details
- `GET /api/v1/questions/random` - Get random question(s) by criteria
- `GET /api/v1/exams` - List available exams
- `GET /api/v1/topics` - List topics/taxonomy

### Progress Tracking
- `POST /api/v1/progress/attempt` - Record question attempt
- `GET /api/v1/progress/stats` - User's overall statistics
- `GET /api/v1/progress/weak-areas` - Identify weak topics/services
- `GET /api/v1/progress/history` - Attempt history with filters

### Study Features
- `GET /api/v1/study/next-question` - Smart question selection (spaced repetition)
- `POST /api/v1/study/bookmark` - Bookmark question for review
- `GET /api/v1/study/bookmarks` - List bookmarked questions
- `GET /api/v1/study/review` - Questions due for review

## 🗄️ Database Schema (Planned)

### Core Tables
- **users** - User accounts and profiles
- **attempts** - Question attempt history with performance tracking
- **bookmarks** - User-bookmarked questions with notes
- **study_sessions** - Spaced repetition tracking (SM-2 algorithm)
- **user_stats** - Denormalized statistics for performance

## 🚀 Getting Started

### Prerequisites
- Node.js 18+ OR Python 3.11+ (TBD)
- PostgreSQL 15+
- Access to exam-content-factory repository (content source)

### Installation

```bash
# Clone the repository
git clone https://github.com/aneesahamed/exam-platform-backend.git
cd exam-platform-backend

# Install dependencies (once tech stack is chosen)
# npm install  OR  pip install -r requirements.txt

# Set up environment variables
cp .env.example .env
# Edit .env with your configuration

# Run database migrations
# npm run migrate  OR  alembic upgrade head

# Start development server
# npm run dev  OR  uvicorn app.main:app --reload
```

### Environment Variables
```env
DATABASE_URL=postgresql://user:password@localhost:5432/exam_platform
JWT_SECRET=your-secret-key-here
CONTENT_PATH=../exam-content-factory/data/exam-packs/
PORT=3000
NODE_ENV=development
```

## 📁 Project Structure (Planned)

```
exam-platform-backend/
├── src/                      # Source code
│   ├── controllers/          # Request handlers
│   ├── services/             # Business logic
│   ├── repositories/         # Data access layer
│   ├── models/               # Database models
│   ├── middleware/           # Express/FastAPI middleware
│   ├── routes/               # API routes
│   ├── utils/                # Utilities
│   ├── config/               # Configuration
│   └── types/                # TypeScript types
├── tests/                    # Test files
│   ├── unit/
│   └── integration/
├── docker-compose.yml        # Docker setup
├── Dockerfile
└── README.md
```

## 🎓 Content Integration

This backend reads from the **exam-content-factory** repository:
- **Content Location**: `../exam-content-factory/data/exam-packs/`
- **Content Format**: JSON files with Schema v2 structure
- **Access Pattern**: Read-only (content is immutable from backend perspective)
- **Current Content**: AWS SAA-C03 (1,003 questions, 98.7% valid)

### Question Schema (from exam-content-factory)
```json
{
  "question_id": "SAA-C03-Q1-001",
  "exam": "AWS-SAA-C03",
  "question_text_raw": "...",
  "options_raw": {"A": "...", "B": "...", "C": "...", "D": "..."},
  "correct_answers": ["A", "C"],
  "explanation": "2-4 sentence explanation",
  "why_others_are_wrong": {"B": "...", "D": "..."},
  "memory_hook": "Vivid memorable hook",
  "taxonomy": {
    "primary_topic_id": "compute",
    "service_ids": ["ec2", "auto-scaling"],
    "concept_tag_ids": ["high-availability"],
    "difficulty": 3
  },
  "quality": {"confidence": "high", "flags": []},
  "status": "reviewed_ai"
}
```

## 🧠 Spaced Repetition

Uses **SM-2 algorithm** for intelligent question scheduling:
- Tracks easiness factor, interval, and repetitions per question
- Adapts to user performance (quality 0-5)
- Schedules reviews at optimal intervals for long-term retention

## 🧪 Testing

```bash
# Run unit tests
# npm test  OR  pytest tests/unit

# Run integration tests
# npm run test:integration  OR  pytest tests/integration

# Run with coverage
# npm run test:coverage  OR  pytest --cov=app tests/
```

## 🐳 Docker

```bash
# Build and run with Docker Compose
docker-compose up -d

# Run migrations
docker-compose exec api npm run migrate

# View logs
docker-compose logs -f api
```

## 📈 Development Roadmap

### Phase 1: MVP (Weeks 1-2)
- [ ] Tech stack selection and project setup
- [ ] Database schema and migrations
- [ ] Content repository implementation
- [ ] Authentication system (JWT)
- [ ] Core question endpoints
- [ ] Basic progress tracking

### Phase 2: Study Features (Weeks 3-4)
- [ ] Spaced repetition algorithm
- [ ] Smart question selection
- [ ] Bookmarking system
- [ ] Weak area identification
- [ ] Statistics and analytics

### Phase 3: Polish (Week 5)
- [ ] API documentation (OpenAPI/Swagger)
- [ ] Comprehensive testing
- [ ] Error handling and logging
- [ ] Docker deployment
- [ ] CI/CD pipeline

### Future Enhancements
- GraphQL API
- Real-time features (WebSockets)
- AI-powered recommendations
- Social features (leaderboards)
- Admin panel
- Multi-exam support expansion

## 🤝 Contributing

This is a personal learning project. Contributions, issues, and feature requests are welcome!

## 📝 License

[MIT License](LICENSE) (to be added)

## 🔗 Related Projects

- **exam-content-factory**: Content enrichment and question generation
- **exam-platform-frontend**: React frontend (coming soon)

## 📧 Contact

**Developer**: Anees Ahamed  
**GitHub**: [@aneesahamed](https://github.com/aneesahamed)

---

**Last Updated**: January 2, 2026  
**Status**: 🚧 Initial Setup - Ready for development!
