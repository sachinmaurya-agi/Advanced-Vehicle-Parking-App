# Vehicle Parking App - V2
## Private companion repository

This project has a private companion repository with the full source and additional assets:

**Vehicle Parking App (private)** — https://github.com/sachinmaurya-agi/Vehicle-Parking-App-V-2

> This repo is **private**. If you'd like access, please **request it** using **one** of the methods below:
>
> 1. Open an issue in this (public) repo titled **"Request access: Vehicle-Parking-App-V-2"** and include your GitHub username and the reason for access.
>
>    👉 Create an issue prefilled:  
>    `https://github.com/sachinmaurya-agi/Advanced-Vehicle-Parking-App/issues/new?title=Request%20access%3A%20Vehicle-Parking-App-V-2&body=Hi%2C%20please%20grant%20access%20to%20the%20private%20repo%20%60Vehicle-Parking-App-V-2%60%20for%20GitHub%20user%3A%20%40sachinmaurya-agi.%0A%0AThanks!`
>
> 2. Or email me at: `sachinmaurya4104@gmail.com` with your GitHub username and reason for access.
>
> Once I receive your request I will add you as a **collaborator** (or invite you) to the private repo.


A comprehensive multi-user parking management system built with Flask (backend) and Vue.js (frontend).

## Features

### Admin Features
- **Dashboard**: Overview of parking lots, spots, and users
- **Parking Lot Management**: Create, edit, and delete parking lots
- **Spot Management**: View real-time status of all parking spots
- **User Management**: View all registered users
- **Reports**: Visual charts and statistics

### User Features
- **Registration/Login**: Secure user authentication
- **Parking Booking**: Book available parking spots automatically
- **Reservation Management**: View and manage active reservations
- **Spot Release**: Release parking spots when leaving
- **Data Export**: Export parking history as CSV

### Background Jobs (Celery + Redis)
- **Daily Reminders**: Automated reminders for inactive users (6 PM daily)
- **Monthly Reports**: Email reports with parking statistics (1st of month)
- **CSV Export**: Asynchronous data export functionality
- **Redis Caching**: Performance optimization with intelligent cache invalidation

### Technical Features
- **JWT Authentication**: Secure token-based authentication
- **Redis Caching**: Performance optimization with caching
- **Celery Tasks**: Background job processing
- **Responsive Design**: Bootstrap-based modern UI
- **Real-time Updates**: Live parking spot status

## Technology Stack

### Backend
- **Flask**: Web framework
- **SQLite**: Database with raw SQL operations
- **Redis**: Caching and message broker for Celery
- **Celery**: Background task processing with scheduled jobs
- **JWT**: Token-based authentication
- **Raw SQL**: All database operations use raw SQL queries

### Frontend
- **Vue.js 3**: Frontend framework
- **Bootstrap 5**: UI framework
- **Chart.js**: Data visualization
- **Axios**: HTTP client

## Installation & Setup

### Prerequisites
- Python 3.8+
- Redis server (for full version)
- Git

### Quick Start

**Windows ( My system):**
```bash
# Full version with Redis and Celery
start_full_app.bat

# Simple version without Redis/Celery
start_simple.bat
```

**Cross-Platform Options:**
```bash
# Python launcher (works on all platforms)
python start_app.py

# Unix/Linux/macOS
./start_app.sh
```

### Backend Setup

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd vehicle-parking-app
   ```

2. **Install Python dependencies**
   ```bash
   cd backend
   pip install -r requirements.txt
   ```

3. **Start Redis server (Full Version Only)**
   ```bash
   # On Windows
   redis-server
   
   # On macOS (if installed via Homebrew)
   brew services start redis
   
   # On Linux
   sudo systemctl start redis
   ```

4. **Start Celery worker (Full Version Only)**
   ```bash
   cd backend
   celery -A celery_worker.celery worker --loglevel=info
   ```

5. **Start Celery beat (Full Version Only)**
   ```bash
   cd backend
   celery -A celery_worker.celery beat --loglevel=info
   ```

6. **Start Flask application**
   ```bash
   cd backend
   python app.py  # Full version
   # OR
   python simple_app.py  # Simple version
   ```

### Frontend Setup

1. **Serve the frontend**
   ```bash
   cd frontend
   # Use any static file server, for example:
   python -m http.server 8080
   # or
   npx serve .
   ```

2. **Access the application**
   - Frontend: http://localhost:8080
   - Backend API: http://localhost:5000

## Default Credentials

### Admin Account
- **Username**: admin
- **Password**: admin123
- **Role**: admin (superuser)

### User Registration
- Users can register with username, email, and password
- All new users have 'user' role by default

## Database Schema

### Users Table
- `id`: Primary key
- `username`: Unique username
- `email`: Unique email address
- `password_hash`: Hashed password
- `role`: 'admin' or 'user'
- `created_at`: Registration timestamp

### Parking Lots Table
- `id`: Primary key
- `prime_location_name`: Location name
- `price`: Price per parking session
- `address`: Physical address
- `pin_code`: Postal code
- `number_of_spots`: Total parking spots
- `created_at`: Creation timestamp

### Parking Spots Table
- `id`: Primary key
- `lot_id`: Foreign key to parking_lots
- `spot_number`: Spot number within the lot
- `status`: 'A' (Available) or 'O' (Occupied)

### Reservations Table
- `id`: Primary key
- `spot_id`: Foreign key to parking_spots
- `user_id`: Foreign key to users
- `parking_timestamp`: Parking start time
- `leaving_timestamp`: Parking end time
- `parking_cost`: Cost of parking session
- `status`: 'active' or 'completed'

## API Endpoints

### Authentication
- `POST /api/register` - User registration
- `POST /api/login` - User login

### Admin Endpoints
- `GET /api/admin/parking-lots` - Get all parking lots
- `POST /api/admin/parking-lots` - Create parking lot
- `PUT /api/admin/parking-lots/<id>` - Update parking lot
- `DELETE /api/admin/parking-lots/<id>` - Delete parking lot
- `GET /api/admin/parking-spots/<lot_id>` - Get spots for a lot
- `GET /api/admin/users` - Get all users

### User Endpoints
- `GET /api/user/parking-lots` - Get available parking lots
- `POST /api/user/book-parking` - Book a parking spot
- `POST /api/user/release-parking` - Release a parking spot
- `GET /api/user/my-reservations` - Get user's reservations
- `POST /api/user/export-csv` - Trigger CSV export
- `GET /api/user/download-csv` - Download CSV file

## Background Jobs

### Daily Reminders
- **Schedule**: Every day at 6:00 PM
- **Purpose**: Send reminders to users who haven't used parking recently
- **Implementation**: Celery beat task

### Monthly Reports
- **Schedule**: First day of every month
- **Purpose**: Generate and send monthly activity reports
- **Implementation**: Celery beat task with email functionality

### CSV Export
- **Trigger**: User-initiated
- **Purpose**: Export user's parking history
- **Implementation**: Celery task with Redis storage

## Caching Strategy

- **Redis**: Used for session storage and task results
- **Cache Expiry**: 1 hour for CSV exports
- **Performance**: Reduces database queries for frequently accessed data

## Security Features

- **JWT Tokens**: Secure authentication
- **Password Hashing**: Werkzeug security
- **Role-based Access**: Admin and user permissions
- **Input Validation**: Server-side validation
- **CORS**: Cross-origin request handling

## Development Notes

### Database Initialization
- Database is created automatically on first run
- Admin user is created with default credentials
- All tables use raw SQL for creation and operations

### Error Handling
- Comprehensive error handling in API endpoints
- User-friendly error messages
- Proper HTTP status codes

### Performance Optimizations
- Redis caching for frequently accessed data
- Efficient SQL queries with proper indexing
- Asynchronous task processing

## Testing

### Manual Testing Checklist
- [ ] User registration and login
- [ ] Admin login with default credentials
- [ ] Create, edit, and delete parking lots
- [ ] Book and release parking spots
- [ ] View parking spot status
- [ ] Export CSV functionality
- [ ] Dashboard statistics and charts
- [ ] Responsive design on different screen sizes

### API Testing
Use tools like Postman or curl to test API endpoints:

```bash
# Login
curl -X POST http://localhost:5000/api/login \
  -H "Content-Type: application/json" \
  -d '{"username": "admin", "password": "admin123"}'

# Get parking lots (with token)
curl -X GET http://localhost:5000/api/admin/parking-lots \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## Deployment Considerations

### Production Setup
1. Use a production WSGI server (Gunicorn)
2. Set up proper environment variables
3. Configure Redis for production
4. Set up proper logging
5. Use HTTPS for security
6. Configure database backups

### Environment Variables
```bash
export FLASK_ENV=production
export SECRET_KEY=your-secret-key
export JWT_SECRET_KEY=your-jwt-secret
export REDIS_URL=redis://localhost:6379/0
```

## Troubleshooting

### Common Issues

1. **Redis Connection Error**
   - Ensure Redis server is running
   - Check Redis connection settings

2. **Database Errors**
   - Check SQLite file permissions
   - Verify database initialization

3. **Celery Tasks Not Working**
   - Ensure Celery worker is running
   - Check Redis connection for Celery

4. **CORS Issues**
   - Verify Flask-CORS configuration
   - Check frontend URL settings

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request


