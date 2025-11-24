# PQ(PopQuiz) - AI-Driven Interactive Teaching Mini Program

PQ is an AI-powered interactive platform specifically designed for knowledge-sharing scenarios. It aims to transform traditional one-way lectures into immersive, quantifiable two-way exchanges through real-time speech analysis, dynamic question generation, and instant feedback, maximizing the efficiency and depth of knowledge transfer.


# ✨ Core Features & System Implementation

### 🎯 Activity Management
- **Full Activity Lifecycle Management**: Speakers can create, configure, start, and end sharing activities
- **Presentation Status Control**: Supports starting, pausing, resuming, and ending presentations, with precise recording of presentation duration
- **Audience Subscription Management**: Real-time tracking of audience joining status, statistics for online numbers and participation
- **Activity Code Sharing**: Generate unique activity codes, audience can quickly join via activity code

### 📚 Intelligent Content Processing
- **Smart Handout Parsing**: Supports uploading handouts in `PPTX`, `PDF`, `DOCX`, `MD` formats, automatically extracting core content
- **AI Asynchronous Question Generation**: Based on uploaded handouts, large language models (LLM) asynchronously generate challenging and in-depth multiple-choice questions in the background
- **Filename Optimization**: Automatically saves original filenames, providing a friendly file display experience

### 🔄 Real-time Interaction System
- **Intelligent Push Mechanism**: Supports dual modes of WebSocket real-time push and HTTP polling, ensuring reliable question delivery
- **Manual Push Control**: Speakers have complete control over question push timing, system provides push reminders but requires manual confirmation
- **Push Reminder Optimization**: Automatically reminds speakers to push questions based on preset times, supports immediate push, skip, or delayed push
- **Auto-Push Scheduling**: Delayed push questions are automatically pushed at specified times, no repeat confirmation needed
- **Real-time Answer Feedback**: Audience can answer in real-time, speakers can view answer statistics and analysis
- **Question Rating System**: Both speakers and audience can like 👍 or dislike 👎 questions, with real-time statistics of ratings

### 💬 Feedback & Evaluation
- **Real-time Feedback System**: Audience can send 6 types of feedback: 😕 Don't understand, 🐌 Too fast, ⚡ Too slow, 📢 Poor sound, 🔊 Noisy environment, 📶 Poor network
- **Question Quality Rating**: Each question supports like/dislike function, helping improve question quality
- **Feedback Statistics Panel**: Speakers can view real-time audience feedback statistics, understanding presentation effectiveness
- **Feedback Panel Optimization**: Audience feedback panel supports close function, better user experience
- **Comment Interaction System**: Supports activity-level comments and question-level comments, promoting knowledge exchange

### 📊 Data Analysis & Reports
- **Audience Statistics Analysis**: Real-time display of total audience, online audience, active audience, and other statistics
- **Presentation Duration Recording**: Precisely records presentation duration, supports pause and resume functions
- **Personalized Learning Reports**: After activity ends, generates personal reports for each attendee including answer accuracy, ranking, and question review
- **Intelligent Ranking System**: Synchronous + asynchronous hybrid ranking calculation, quickly displays answer rankings, preventing long display of "#?"
- **Activity Data Export**: Supports export of answer statistics, user participation data
- **Email Report Function**: Regularly auto-generates and sends activity statistics reports

## 🛠️ Tech Stack

- **Backend**: Python 3.11+, FastAPI, SQLAlchemy, WebSocket, Alembic
- **Frontend**: WeChat Mini Program (Native Development)
- **Database**: PostgreSQL (Supports connection pooling and transaction optimization)
- **AI Integration**: Supports multiple large language models including OpenAI, Tongyi Qianwen, DeepSeek, with backup model mechanism
- **Network Communication**: Enhanced HTTP client, supports automatic token refresh and error retry
- **Message Push**: WebSocket + HTTP polling dual guarantee
- **Dependency Management**: pip
- **Development Tools**: Integrated stress testing tools, supports 50-200 concurrent user testing

---

## 🚀 Quick Start

Please follow these steps to configure and run this project.

### 1. Prerequisites

- **Python 3.11 or higher**
- **WeChat Developer Tools**
- **PostgreSQL**
- **Large Language Model API Key** (e.g., DeepSeek, Tongyi Qianwen, etc.)

### 2. Backend Configuration & Startup

#### a. Install Dependencies

First, install all required Python libraries for the backend. Open a terminal in the project root directory and run:

```bash
pip install -r backend/requirements.txt
```

#### b. Modify Configuration Files (Important!)

AI functionality and user authentication depend on some keys that must be manually configured.

1. **Copy configuration files**:
   ```bash
   cp .env.example .env
   cp config.js.example config.js
   ```

2.1 **Edit `.env` file**:
   - `LLM_API_KEY`: Set your large language model API key
   - `LLM_ENDPOINT`: Set your large language model base URL
   - `CHAT_MODEL`: Set your large language model name
   - `LLM_MAX_TPM`: Set maximum TPM supported
   - `LLM_MAX_RPM`: Set maximum RPM supported
   - `LLM_BACK_ENDPOINT`: Set your backup large language model API key
   - `LLM_BACK_API_KEY`: Set your backup large language model base URL
   - `LLM_BACK_CHAT_MODEL`: Set your backup large language model name
   - `LLM_BACK_MAX_TPM`: Set backup model maximum TPM supported
   - `LLM_BACK_MAX_RPM`: Set backup model maximum RPM supported
   - `SECRET_KEY`: Set a long, random string to secure user login sessions
   - `WECHAT_APPID` and `WECHAT_SECRET`: Set WeChat mini-program AppID and Secret (for production)
   - `DATABASE_URL`: Set your PostgreSQL database connection string (e.g., `postgresql://user:password@host:port/database`)

2.2 **Edit `config.js` file**:
   - **Mini Program AppID Configuration**: Set correct AppID in each environment, must match WECHAT_APPID in .env
   - **Auto Environment Recognition**: System configured for automatic WeChat environment recognition (develop→test, trial→trial, release→prod)
   - **Service Address Configuration**: Configure correct backend service IP or domain for each environment
   - **Environment Management Tools**: Use `npm run check-env` to check current environment, `npm run auto-env` for intelligent configuration
   - See `miniprogram/ENVIRONMENT_GUIDE.md` for more details

#### c. Initialize Database

First, set DATABASE_URL in .env file, then execute:

```bash
alembic upgrade head
```

#### d. Start Backend Service

After completing configuration, run the following command in the backend directory to start the FastAPI backend service:

```bash
uvicorn main:app --reload
```

After successful startup, you should see output similar to the following in the terminal, indicating the service is running at `http://127.0.0.1:8000`:

```
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
INFO:     Started reloader process [xxxxx] using statreload
INFO:     Started server process [xxxxx]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
```

#### e. Install Chinese Fonts (Optional for PDF functionality)

If you need to use PDF export functionality, the project provides automated installation scripts supporting Ubuntu/Debian/CentOS/RHEL/Alpine systems:

```bash
# Use automated script to install fonts (recommended)
cd backend
sudo bash scripts/install-fonts.sh
```

The script will automatically:
- Detect operating system type
- Install corresponding Chinese font packages (Noto Sans CJK)
- Update font cache
- Verify installation results

**Test PDF generation and font display:**
```bash
# Run test script
cd backend
python scripts/test_pdf.py
```

The test script generates a PDF file (`/tmp/pdf_font_test.pdf`) containing various Chinese characters to verify:
- Common Chinese characters, rare characters, traditional characters
- Chinese-English mixed text
- Tables, lists, and other formats

**Note**:
- PDF functionality depends on WeasyPrint library, must first install dependencies in `requirements.txt`
- Font files are large (about 100-200MB), installation may take a while
- Can skip this step if PDF functionality not needed

### 3. WeChat Mini Program Configuration & Startup

#### a. Import Project

1. Open **WeChat Developer Tools**.
2. On the startup page, select **"Import"**.
3. In the pop-up file selection window, select the `$RootDIR\miniprogram` folder of this project.
4. If needed, fill in your own mini-program `AppID`, or use a test account.

#### b. Configure Development Settings (Important!)

To allow the mini-program to access the backend service running locally, you must make the following settings:

1. In the upper right corner of WeChat Developer Tools, click the **"Details"** button.
2. In the pop-up panel, select **"Local Settings"**.
3. Check **"Do not verify legal domain names, web-view (business domain names), TLS version, and HTTPS certificates"** option.

**Warning**: This option is only for local development. When you need to publish the mini-program online, you must deploy your backend service to a domain with a valid HTTPS certificate and configure that domain as a legal domain in the WeChat public platform backend.

#### c. Verify Environment Configuration

Before starting the mini-program, it's recommended to verify environment configuration:

```bash
# Enter mini-program directory
cd miniprogram

# Check current environment configuration
npm run check-env

# Intelligent detection of all environment AppID configurations
npm run auto-env

# Manually sync AppID to project configuration if needed
npm run sync-appid
```

#### d. Install Towxml Dependencies (Markdown Report Rendering)

The mini-program uses the Towxml library to render activity analysis reports in Markdown format, requiring installation and building:

```bash
# Execute in miniprogram directory
cd miniprogram
npm install
```

Then in WeChat Developer Tools:
1. Click menu **"Tools"**
2. Select **"Build npm"**
3. Wait for build to complete

**Verify Installation:**
- Check if `miniprogram/miniprogram_npm/` directory exists
- Confirm `towxml` folder exists in that directory

**Troubleshooting:**
- Ensure Node.js version >= 14.0
- If encountering issues, try clearing cache: `npm cache clean --force`
- Delete `node_modules` and `miniprogram_npm` then reinstall and rebuild

#### e. Run Mini Program

After completing the above configuration, the developer tools will automatically compile the mini-program. You can see the mini-program's login page in the left simulator, indicating the frontend is running successfully.

**Environment Notes**:
- 🔧 **Develop Version**: Auto-connects to test environment, for daily development and debugging
- 🧪 **Trial Version**: Auto-connects to trial environment, for internal testing and preview
- 🚀 **Release Version**: Auto-connects to prod environment, for production release

---

## 📖 Usage Guide

### Speaker Workflow

1. **Login**: On the mini-program's main page, select "Speaker" role to login
2. **Create Activity**: After login, enter activity name and description on main interface, click create
3. **Manage Activity**: Click the newly created activity to enter activity management page
4. **Upload Handout**: Click "Select Document" button, upload handout file in `PPTX`, `PDF`, `DOCX`, or `MD` format
5. **Async Question Generation**: After successful upload, click "Intelligently Generate Questions" button, system will asynchronously generate questions in background
6. **Monitor Generation Progress**: Wait for AI generation to complete, can view generation progress and status in real-time
7. **Start Presentation**: Click "Start Presentation" button, audience can now join activity, system starts timing
8. **Push Reminder Management**:
   - System automatically reminds speaker to push questions based on preset times
   - Upon receiving reminder, speaker can choose: push immediately, skip, or delay push (2 minutes)
   - **Delayed Push Optimization**: Delayed push questions automatically push after 2 minutes, no manual confirmation needed again
   - **Status Display Fix**: After delayed push, status correctly displays as "Pending Push", automatically changes to "Pushed" after 2 minutes
   - **Push Reliability**: Supports dual mechanisms of WebSocket and polling push, ensuring question delivery
9. **Real-time Monitoring**: View audience statistics, answer status, and question ratings
10. **Presentation Control**: Supports pause, resume, and end presentation, system precisely records presentation duration
11. **Question Rating**: Can like 👍 or dislike 👎 generated questions

### Audience Workflow

1. **Login**: On the mini-program's main page, select "Audience" role to login
2. **Join Activity**: Enter activity code to join ongoing activity
3. **Auto Subscribe**: System automatically records audience joining status, speaker can view online count
4. **Real-time Answering**: When speaker pushes questions, answer promptly and view results
5. **Question Rating**: After answering, can like 👍 or dislike 👎 questions
6. **Anonymous Feedback**: Can click "Feedback" button anytime to send anonymous feedback (e.g., "Too fast", "Noisy environment", "Poor network", etc.)
7. **View My Learning**: View all participated activities and answer records on "My Learning" page
8. **View Reports**: After activity ends, can view personal answer reports and statistics
9. **Smart Login Recovery**: Supports automatic token refresh, friendly prompt to re-login when login expires

### 🔄 Push Reminder Mechanism Details

System uses **manual confirmation push** mechanism, ensuring speakers have complete control over question push timing:

1. **Push Plan Setting**: When presentation starts, system automatically calculates push times based on number of questions and presentation duration
2. **Auto Reminder**: At preset time, system pops up push reminder window
3. **Manual Confirmation**: Speaker must manually choose action:
   - **Push Immediately**: Questions immediately sent to all online audience
   - **Skip**: Skip current question, won't push again
   - **Delay Push**: Remind again after 1 minute delay
4. **Status Tracking**: System real-time tracks push status of each question (Pending Push/Pushed/Skipped/Delayed Push)

## ⚙️ Configuration Management

### Environment Configuration System

Project supports **WeChat environment auto-recognition** and **intelligent AppID management**, no manual environment switching needed:

#### Environment Mapping
- **Develop Version** (develop) → test environment (daily development)
- **Trial Version** (trial) → trial environment (internal testing)
- **Release Version** (release) → prod environment (production release)

#### Configuration File Description (miniprogram/config/config.js)

- `baseUrl`: HTTP API base address
- `wsUrl`: WebSocket connection address
- `appid`: Mini-program AppID (must match WECHAT_APPID in .env)
- `apiTimeout`: API request timeout (milliseconds)
- `uploadTimeout`: File upload timeout (milliseconds)

### AppID Configuration and Sync

System has implemented automatic AppID management:
- ✅ `project.config.json` auto-generated and updated by script
- ✅ Supports multi-environment AppID configuration
- ✅ Auto-syncs with .env file

#### Environment Management Commands
```bash
# Check current environment configuration
npm run check-env

# Intelligent environment detection and AppID configuration
npm run auto-env

# Sync AppID to project configuration file
npm run sync-appid

# Manual environment switching (for debugging)
npm run dev/test/prod
```

#### Configuration File Reference

Using configuration in mini-program pages:

```javascript
const app = getApp()
const baseUrl = app.globalData.baseUrl
const wsUrl = app.globalData.wsUrl

// API request
wx.request({
  url: `${baseUrl}/api/activities`,
  // ...
})

// WebSocket connection
wx.connectSocket({
  url: `${wsUrl}/interactions/ws/audience/${activity_id}/${client_id}`
})
```

**Detailed Configuration Guide**: See `miniprogram/ENVIRONMENT_GUIDE.md`

## 🗄️ Database Description

Project uses PostgreSQL as database, supporting complete relational data storage and transaction processing.

**For detailed database architecture description, see: [DATABASE_SCHEMA.md](DATABASE_SCHEMA.md)**

This document includes:
- Complete table structure definitions and field descriptions
- Database relationship diagrams and index information
- Data migration script descriptions
- Performance optimization recommendations and monitoring metrics
- Backup recovery strategies and troubleshooting guides

## 🧪 Test File Structure

All test files are located in the `tests/` directory:

```
tests/
├── check_db_data.py              # Database data check
├── check_db_schema.py            # Database structure check
├── check_wechat_config.py        # WeChat configuration check
├── test_activities_api.py        # Activity API test
├── test_activity_code.py         # Activity code function test
├── test_activity_detail.py       # Activity detail test
├── test_activity_end_status.py   # Activity end status test
├── test_activity_flow.py         # Activity flow test
├── test_activity_status.py       # Activity status test
├── test_api_full.py              # Complete API test
├── test_api.py                   # Basic API test
├── test_async_features.py        # Async feature test
├── test_audience_fixes.py        # Audience function fix test
├── test_complete_flow.py         # Complete flow test
├── test_create_activity.py       # Create activity test
├── test_frontend_api.py          # Frontend API test
├── test_presentation_timer.py    # Presentation timer test
├── test_question_generation.py   # Question generation test
├── test_smart_questions.py       # Smart question test
├── test_status_fix.py            # Status fix test
└── test_wechat_login.py          # WeChat login test
```

### Running Tests

```bash
# Run single test
python tests/test_activity_code.py

# Run API tests
python tests/test_activities_api.py

# Check database status
python tests/check_db_data.py

# Test complete flow
python tests/test_complete_flow.py

# Test manual push system (new)
python test_manual_push_system.py

# Test question like/dislike functionality (new)
python test_rating_api.py

# Test audience subscription management (new)
python test_complete_push_system.py
```

### Test Coverage

- **API Testing**: Verify all backend interface functionality
- **Functional Testing**: Test core business logic
- **Integration Testing**: Test frontend-backend interaction
- **Database Testing**: Verify data storage and queries
- **Configuration Testing**: Verify environment configuration correctness

## 📁 Project Structure

```
PQ/
├── backend/                    # FastAPI backend code
│   ├── api/                   # API routes and endpoints
│   │   ├── endpoints/         # Specific API endpoints
│   │   └── models.py          # Database models
│   ├── core/                  # Core logic
│   │   ├── ai.py             # AI integration
│   │   ├── config.py         # Configuration management
│   │   ├── parser.py         # Document parsing
│   │   └── smart_question_generator.py
│   ├── crud/                  # Database operations
│   ├── db/                    # Database session and initialization
│   ├── schemas/               # Pydantic data models
│   ├── uploads/               # File upload directory
│   ├── main.py               # FastAPI app entry point
│   ├── migrate_*.py          # Database migration scripts
│   └── requirements.txt      # Python dependencies
├── miniprogram/               # WeChat mini-program frontend code
│   ├── config/               # Configuration files
│   │   └── config.js.example # Environment config example
│   ├── pages/                # Mini-program pages
│   │   ├── index/            # Home page
│   │   ├── activity-manage/  # Activity management
│   │   ├── audience/         # Audience page
│   │   ├── audience-login/   # Audience login
│   │   ├── qr-code/          # QR code page
│   │   └── report/           # Report page
│   ├── utils/                # Utility functions
│   ├── app.js               # Mini-program global logic
│   └── project.config.json  # Mini-program configuration
├── tests/                    # Test files directory
│   ├── check_*.py           # Check scripts
│   └── test_*.py            # Test scripts
├── .env                     # Environment variables configuration
├── .env.example            # Environment variables example
└── README.md               # Project documentation
```

## 🤝 Contributing

Welcome to submit Issues and Pull Requests to help improve the project.

### Contribution Guidelines

1. Fork this project
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request
