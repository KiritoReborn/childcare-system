# ChildCare - Child Health Record Booklet System

A comprehensive offline-capable child health record management system designed for field workers in remote areas. Built with React, Node.js, and PWA capabilities for seamless offline data collection and secure synchronization.

## 🌟 Features

### Core Features
- **Offline-First Design**: Complete functionality without internet connectivity
- **Child Health Data Collection**: Comprehensive health assessment forms
- **eSignet Integration**: Secure authentication via national ID and OTP
- **PDF Health Booklets**: Generate printable health records with QR codes
- **Real-time Sync**: Automatic data synchronization when online
- **PWA Support**: Install as mobile/desktop app
- **Multi-language Ready**: Internationalization support

### Data Collection
- Child demographics and photos
- Physical measurements (weight, height, BMI)
- Nutritional status assessment
- Malnutrition signs detection
- Recent illness history
- Parental consent management
- GPS location tagging

### Security & Privacy
- Local data encryption
- Secure authentication
- GDPR-compliant data handling
- Role-based access control
- Audit trails

### Admin Features
- Dashboard with analytics
- Data export (JSON/CSV)
- Representative management
- System health monitoring
- Sync status tracking

## 🏗️ Architecture

### Frontend (React + PWA)
```
src/
├── components/          # Reusable UI components
├── pages/              # Main application pages
├── contexts/           # React contexts (Auth, Offline)
├── utils/              # Utility functions
├── entities/           # Data models
└── integrations/       # External service integrations
```

### Backend (Node.js + Express)
```
server/
├── routes/             # API endpoints
├── database/           # Database setup and models
├── middleware/         # Custom middleware
├── utils/              # Server utilities
└── uploads/            # File storage
```

### Database Schema
- **representatives**: Field worker profiles and authentication
- **child_records**: Health data and measurements
- **auth_sessions**: JWT token management
- **sync_logs**: Synchronization tracking

## 🚀 Quick Start

### Prerequisites
- Node.js 18+ 
- npm or yarn
- Git

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/your-org/childcare-system.git
cd childcare-system
```

2. **Install dependencies**
```bash
# Install frontend dependencies
npm install

# Install backend dependencies
cd server
npm install
cd ..
```

3. **Environment Setup**
```bash
# Create environment file
cp .env.example .env

# Edit .env with your configuration
nano .env
```

4. **Start Development Servers**
```bash
# Start both frontend and backend
npm run dev

# Or start separately
npm run client  # Frontend (port 3000)
npm run server  # Backend (port 5000)
```

5. **Access the Application**
- Frontend: http://localhost:3000
- Backend API: http://localhost:5000/api
- API Documentation: http://localhost:5000/api/docs

### Production Deployment

1. **Build the Application**
```bash
npm run build
```

2. **Start Production Server**
```bash
npm start
```

## 📱 PWA Installation

The application can be installed as a Progressive Web App:

1. **Mobile**: Add to home screen via browser menu
2. **Desktop**: Install via browser address bar
3. **Offline**: Full functionality without internet

## 🔐 Authentication

### Demo Credentials
```
Field Worker:
- Email: john.doe@childcare.org
- Password: password123

Admin:
- Email: jane.smith@childcare.org  
- Password: password123
```

### eSignet Integration
- Mock OTP system for demonstration
- National ID verification
- Secure session management

## 📊 API Endpoints

### Authentication
```
POST /api/auth/login          # User login
POST /api/auth/logout         # User logout
GET  /api/auth/me            # Get current user
POST /api/auth/refresh       # Refresh token
```

### Child Records
```
GET    /api/child-records              # List records
POST   /api/child-records              # Create record
GET    /api/child-records/:id          # Get record
PUT    /api/child-records/:id          # Update record
DELETE /api/child-records/:id          # Delete record
POST   /api/child-records/upload-photo # Upload photo
POST   /api/child-records/bulk-upload  # Bulk sync
GET    /api/child-records/pending/sync # Pending records
```

### PDF Generation
```
GET /api/pdf/health-booklet/:healthId     # Generate health booklet
GET /api/pdf/health-booklet-by-id/:id     # Generate by record ID
GET /api/pdf/summary-report               # Admin summary report
```

### eSignet Integration
```
POST /api/esignet/initiate-auth    # Start authentication
POST /api/esignet/verify-otp       # Verify OTP
GET  /api/esignet/auth-status      # Check auth status
GET  /api/esignet/health           # Service health
```

### Admin Endpoints
```
GET    /api/admin/dashboard        # Admin dashboard
GET    /api/admin/records          # All records
GET    /api/admin/sync-logs        # Sync history
GET    /api/admin/system-health    # System status
GET    /api/admin/export           # Data export
```

## 🔧 Configuration

### Environment Variables

```env
# Server Configuration
PORT=5000
NODE_ENV=development

# Database
DB_PATH=./data/childcare.db

# JWT
JWT_SECRET=your-secret-key-change-in-production

# Client URL
CLIENT_URL=http://localhost:3000

# File Upload
MAX_FILE_SIZE=5242880
UPLOAD_PATH=./uploads

# eSignet (Mock)
ESIGNET_ENABLED=true
ESIGNET_MOCK_OTP=true
```

### Offline Configuration

The application uses IndexedDB for offline storage:

```javascript
// Database configuration
const dbConfig = {
  name: 'ChildCareDB',
  version: 1,
  stores: {
    childRecords: { keyPath: 'id', indexes: ['health_id', 'upload_status'] },
    syncQueue: { keyPath: 'id', indexes: ['status', 'created_at'] },
    userData: { keyPath: 'id' }
  }
};
```

## 📋 Data Models

### Child Record Schema
```json
{
  "id": "uuid",
  "health_id": "CH123456ABC",
  "child_name": "string",
  "age": "number (months)",
  "weight": "number (kg)",
  "height": "number (cm)",
  "bmi": "number",
  "nutritional_status": "normal|underweight|severely_underweight|overweight",
  "parent_guardian_name": "string",
  "malnutrition_signs": ["array"],
  "recent_illnesses": ["array"],
  "parental_consent": "boolean",
  "location": {
    "latitude": "number",
    "longitude": "number",
    "address": "string"
  },
  "representative_id": "uuid",
  "upload_status": "pending|uploaded|failed",
  "created_at": "timestamp",
  "updated_at": "timestamp"
}
```

## 🧪 Testing

### Run Tests
```bash
# Frontend tests
npm run test

# Backend tests
cd server
npm test

# E2E tests
npm run test:e2e
```

### Test Coverage
```bash
npm run test:coverage
```

## 📦 Build & Deploy

### Docker Deployment
```bash
# Build image
docker build -t childcare-system .

# Run container
docker run -p 3000:3000 -p 5000:5000 childcare-system
```

### Manual Deployment
```bash
# Build frontend
npm run build

# Start production server
NODE_ENV=production npm start
```

## 🔍 Monitoring & Logging

### Health Checks
- `/api/health` - Application health
- `/api/esignet/health` - eSignet service status
- `/api/admin/system-health` - System statistics

### Logging
- Request/response logging
- Error tracking
- Performance monitoring
- Sync activity logs

## 🛠️ Development

### Code Structure
```
├── Components/          # Existing components
├── Pages/              # Existing pages
├── server/             # Backend API
├── src/                # Frontend source
│   ├── components/     # New components
│   ├── contexts/       # React contexts
│   ├── utils/          # Utilities
│   └── entities/       # Data models
├── public/             # Static assets
└── docs/               # Documentation
```

### Adding New Features
1. Create feature branch
2. Implement frontend components
3. Add backend API endpoints
4. Update database schema if needed
5. Add tests
6. Update documentation

## 🤝 Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- UNICEF for the challenge and requirements
- eSignet team for authentication integration
- Open source community for libraries and tools

## 📞 Support

- **Documentation**: [Wiki](https://github.com/your-org/childcare-system/wiki)
- **Issues**: [GitHub Issues](https://github.com/your-org/childcare-system/issues)
- **Email**: support@childcare.example.com

## 🔄 Changelog

### v1.0.0 (2024-01-15)
- Initial release
- Offline-first architecture
- eSignet integration
- PDF generation
- PWA support
- Admin dashboard
- Complete CRUD operations

---

**Built with ❤️ for child health and nutrition**
