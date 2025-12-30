# 📱 NFC Attendance System

A modern, production-ready web application for contactless attendance tracking using NFC-enabled ID cards and smartphones. Built with Flask backend and Web NFC API frontend.

## ✨ Features

- **Student Management**: Add students manually or via Excel/CSV bulk upload
- **NFC Tag Registration**: Register NFC-enabled ID cards to student profiles
- **Contactless Attendance**: Scan NFC cards using Android phones to record attendance
- **Real-time Dashboard**: View attendance statistics and recent records
- **Search & Filter**: Find students by name, register number, section, or department
- **Attendance History**: Track individual student attendance over time
- **Hybrid Database**: SQLite for development, PostgreSQL for production

## 🎯 Technology Stack

- **Backend**: Flask 3.0 (Python)
- **Database**: SQLAlchemy ORM (SQLite/PostgreSQL)
- **Frontend**: HTML5, CSS3, Vanilla JavaScript
- **NFC**: Web NFC API (Chrome on Android)
- **Styling**: Custom green-themed design system

## 📋 Prerequisites

- **Python 3.8+**
- **Android phone with NFC** (for NFC features)
- **Chrome browser on Android** (version 89+)
- **HTTPS** (required for production Web NFC)

> **⚠️ Important**: Web NFC API is **only supported on Chrome for Android**. iOS devices do not support Web NFC.

## 🚀 Quick Start

### 1. Clone or Navigate to Project

```bash
cd "d:\NFC ANTI"
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Set Up Environment (Optional)

For development, no configuration needed (uses SQLite by default).

For production with PostgreSQL:

```bash
# Copy example environment file
cp .env.example .env

# Edit .env and set DATABASE_URL
# DATABASE_URL=postgresql://user:password@host:port/dbname
```

### 4. Run the Application

```bash
python run.py
```

The server will start at `http://localhost:5000`

### 5. Access the Application

- **On your computer**: Open `http://localhost:5000` in any browser
- **For NFC features**: Access from Android phone via `http://YOUR_IP:5000`

## 📱 Using the System

### Adding Students

**Option 1: Manual Entry**
1. Navigate to "Add Student"
2. Fill in student details
3. Click "Save Student"

**Option 2: Bulk Upload**
1. Download the sample template from "Upload Students" page
2. Fill in student data in Excel
3. Upload the file
4. Review import results

### Registering NFC Tags

1. Go to "Students" page
2. Click on a student to view their profile
3. Click "Register NFC Tag"
4. Tap the student's ID card near your Android phone
5. Tag will be registered automatically

### Recording Attendance

1. Navigate to "Scan Attendance"
2. Enter your name (faculty)
3. Click "Start Scanning"
4. Tap student ID cards near your phone
5. Attendance is recorded instantly with visual feedback

## 🧪 Running Tests

```bash
# Run all tests
pytest tests/ -v

# Run with coverage
pytest tests/ --cov=src --cov-report=html

# Run specific test file
pytest tests/test_student_service.py -v
```

## 📁 Project Structure

```
d:/NFC ANTI/
├── README.md                          # This file
├── requirements.txt                   # Python dependencies
├── .env.example                       # Environment template
├── .gitignore                         # Git ignore rules
├── sample_students_template.xlsx     # Excel template
├── run.py                            # Application entry point
├── src/
│   ├── __init__.py
│   ├── app.py                        # Flask app factory
│   ├── models.py                     # Database models
│   ├── services/                     # Business logic
│   │   ├── student_service.py
│   │   ├── nfc_service.py
│   │   └── attendance_service.py
│   ├── api/                          # REST API endpoints
│   │   ├── students.py
│   │   ├── nfc.py
│   │   └── attendance.py
│   ├── utils/                        # Utilities
│   │   ├── validators.py
│   │   └── excel_parser.py
│   └── static/                       # Frontend files
│       ├── css/
│       │   └── styles.css
│       ├── js/
│       │   ├── app.js
│       │   ├── nfc-handler.js
│       │   └── student-manager.js
│       ├── index.html
│       ├── dashboard.html
│       ├── students.html
│       ├── add-student.html
│       ├── upload-students.html
│       ├── student-profile.html
│       └── scan-attendance.html
└── tests/
    ├── test_student_service.py
    └── test_api.py
```

## 🔌 API Documentation

### Students

- `POST /api/students` - Create a student
- `POST /api/students/upload` - Upload Excel/CSV
- `GET /api/students` - List all students (supports filters)
- `GET /api/students/<id>` - Get student details
- `DELETE /api/students/<id>` - Delete a student

### NFC Operations

- `POST /api/nfc/register` - Register NFC tag to student
- `POST /api/nfc/unregister/<student_id>` - Remove NFC tag
- `GET /api/nfc/student/<tag_id>` - Get student by NFC tag
- `GET /api/nfc/check/<tag_id>` - Check if tag is registered

### Attendance

- `POST /api/attendance/record` - Record attendance
- `GET /api/attendance/student/<id>` - Get student attendance history
- `GET /api/attendance/recent` - Get recent attendance records
- `GET /api/attendance/date?date=YYYY-MM-DD` - Get attendance by date
- `GET /api/attendance/stats` - Get attendance statistics

## 🌐 Deployment

### Deploy to Render (Recommended)

1. **Create PostgreSQL Database**
   - Go to [Render Dashboard](https://dashboard.render.com/)
   - Create new PostgreSQL database (free tier available)
   - Copy the "Internal Database URL"

2. **Create Web Service**
   - Create new Web Service
   - Connect your GitHub repository
   - Set build command: `pip install -r requirements.txt`
   - Set start command: `python run.py`

3. **Set Environment Variables**
   ```
   DATABASE_URL=<your-postgresql-url>
   SECRET_KEY=<generate-random-secret>
   FLASK_ENV=production
   ```

4. **Deploy**
   - Render will automatically deploy your app
   - Access via the provided `.onrender.com` URL

### Deploy to Railway

1. Create new project on [Railway](https://railway.app/)
2. Add PostgreSQL plugin
3. Add your GitHub repository
4. Set environment variables (same as above)
5. Deploy automatically

### Important for Production

- **HTTPS Required**: Web NFC only works over HTTPS
- **Database**: Use PostgreSQL for data persistence
- **Secret Key**: Generate a strong random secret key
- **CORS**: Configure allowed origins if needed

## 🔧 Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `DATABASE_URL` | PostgreSQL connection string | SQLite (dev) |
| `SECRET_KEY` | Flask secret key | Auto-generated |
| `FLASK_ENV` | Environment (development/production) | development |
| `HOST` | Server host | 0.0.0.0 |
| `PORT` | Server port | 5000 |

### Database Migration

The application automatically creates database tables on startup. For production:

```python
# Tables are created automatically via:
with app.app_context():
    db.create_all()
```

## 🐛 Troubleshooting

### NFC Not Working

- ✅ Ensure you're using Chrome on Android (version 89+)
- ✅ Check that NFC is enabled in phone settings
- ✅ For production, ensure site is served over HTTPS
- ✅ Grant NFC permissions when prompted

### Database Issues

- ✅ For SQLite: Check file permissions in project directory
- ✅ For PostgreSQL: Verify DATABASE_URL is correct
- ✅ Check database connection logs in console

### Excel Upload Failing

- ✅ Ensure file has correct column headers
- ✅ Check for duplicate register numbers
- ✅ Verify file format (.xlsx, .xls, or .csv)
- ✅ Review error messages for specific issues

### Port Already in Use

```bash
# Change port in .env file or run with custom port
PORT=8000 python run.py
```

## 📊 Sample Data

Use the provided `sample_students_template.xlsx` to test bulk upload:

| Name | Register Number | Section | Department | Duration |
|------|----------------|---------|------------|----------|
| John Doe | 2021CS001 | A | Computer Science | Year 3 |
| Jane Smith | 2021CS002 | A | Computer Science | Year 3 |
| Alice Johnson | 2021EC001 | B | Electronics | Year 2 |

## 🤝 Contributing

This is a student project. Feel free to:
- Report bugs
- Suggest features
- Submit pull requests
- Improve documentation

## 📄 License

This project is open source and available for educational purposes.

## 🙏 Acknowledgments

- Built with Flask and Web NFC API
- Green theme inspired by modern material design
- Icons: Unicode emoji characters

## 📞 Support

For issues or questions:
1. Check the Troubleshooting section
2. Review API documentation
3. Check browser console for errors
4. Verify NFC hardware compatibility

---

**Made with 💚 for contactless attendance tracking**
