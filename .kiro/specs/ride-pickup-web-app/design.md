# Design Document

## Overview

The Ride & Pickup DBMS web application follows a three-tier architecture:
- **Frontend**: React application using Vite as the build tool, with Axios for HTTP requests
- **Backend**: Python Flask REST API server handling business logic and database operations
- **Database**: Oracle Database with tables in BCNF (Customer, Driver, Vehicle, Location, Ride, Payment, Rating)

The application provides full CRUD operations for all entities and includes advanced reporting capabilities with aggregated queries.

## Architecture

### System Architecture Diagram

```
┌─────────────────────────────────────────┐
│         React Frontend (Vite)           │
│  - Component-based UI                   │
│  - Axios for API calls                  │
│  - Client-side routing                  │
└──────────────┬──────────────────────────┘
               │ HTTP/REST (JSON)
               │ Port 5000
┌──────────────▼──────────────────────────┐
│      Python Flask Backend API           │
│  - RESTful endpoints                    │
│  - Business logic layer                 │
│  - Database connection pool             │
└──────────────┬──────────────────────────┘
               │ cx_Oracle / oracledb
               │ SQL queries
┌──────────────▼──────────────────────────┐
│         Oracle Database                 │
│  - 7 tables in BCNF                     │
│  - Foreign key constraints              │
│  - Dummy data for demonstration         │
└─────────────────────────────────────────┘
```

### Technology Stack

**Frontend:**
- React 19.2.0
- Vite 7.2.2 (development server and build tool)
- Axios 1.13.2 (HTTP client)
- CSS for styling (can use CSS modules or styled-components)

**Backend:**
- Python 3.x
- Flask (web framework)
- Flask-CORS (cross-origin resource sharing)
- cx_Oracle or python-oracledb (Oracle database driver)
- python-dotenv (environment variable management)

**Database:**
- Oracle Database (Express Edition or school DB)

## Components and Interfaces

### Frontend Components

#### 1. App Component (App.jsx)
- Root component managing routing and global state
- Navigation bar for switching between sections
- Error boundary for catching React errors

#### 2. Navigation Component (Navigation.jsx)
- Displays links to all main sections
- Highlights active section
- Responsive menu for mobile devices

#### 3. Entity Management Components
Each entity (Customer, Driver, Vehicle, Location, Ride, Payment, Rating) has three components:

**EntityList.jsx**
- Displays table of all records
- Provides buttons for Add, Edit, Delete actions
- Implements pagination if needed
- Shows related data (e.g., driver name instead of just driver ID)

**EntityForm.jsx**
- Reusable form for creating and editing records
- Handles form validation
- Provides dropdowns for foreign key relationships
- Submits data to backend API

**EntityDetail.jsx** (optional)
- Shows detailed view of a single record
- Displays related records (e.g., all rides for a customer)

#### 4. Reports Component (Reports.jsx)
- Dropdown or tabs to select report type
- Displays report results in tables or charts
- Includes the following reports:
  - Top Drivers by Ride Count
  - Revenue by Payment Method
  - Average Rating by Driver
  - Rides by Location
  - Customer Ride History (with customer selector)

#### 5. Shared Components
- **LoadingSpinner.jsx**: Shows loading state during API calls
- **ErrorMessage.jsx**: Displays error messages
- **SuccessMessage.jsx**: Displays success notifications
- **ConfirmDialog.jsx**: Confirmation dialog for delete operations

### Backend API Structure

#### Directory Structure
```
backend/
├── app.py              # Main Flask application and routes
├── db.py               # Database connection and query functions
├── models.py           # Data models and validation (optional)
├── config.py           # Configuration management
├── requirements.txt    # Python dependencies
└── .env               # Environment variables (not in git)
```

#### API Endpoints

**Customer Endpoints:**
- `GET /api/customers` - Get all customers
- `GET /api/customers/<id>` - Get customer by ID
- `POST /api/customers` - Create new customer
- `PUT /api/customers/<id>` - Update customer
- `DELETE /api/customers/<id>` - Delete customer

**Driver Endpoints:**
- `GET /api/drivers` - Get all drivers
- `GET /api/drivers/<id>` - Get driver by ID
- `POST /api/drivers` - Create new driver
- `PUT /api/drivers/<id>` - Update driver
- `DELETE /api/drivers/<id>` - Delete driver

**Vehicle Endpoints:**
- `GET /api/vehicles` - Get all vehicles (with driver info)
- `GET /api/vehicles/<vin>` - Get vehicle by VIN
- `POST /api/vehicles` - Create new vehicle
- `PUT /api/vehicles/<vin>` - Update vehicle
- `DELETE /api/vehicles/<vin>` - Delete vehicle

**Location Endpoints:**
- `GET /api/locations` - Get all locations
- `GET /api/locations/<id>` - Get location by ID
- `POST /api/locations` - Create new location
- `PUT /api/locations/<id>` - Update location
- `DELETE /api/locations/<id>` - Delete location

**Ride Endpoints:**
- `GET /api/rides` - Get all rides (with joined data)
- `GET /api/rides/<id>` - Get ride by ID
- `POST /api/rides` - Create new ride
- `PUT /api/rides/<id>` - Update ride
- `DELETE /api/rides/<id>` - Delete ride

**Payment Endpoints:**
- `GET /api/payments` - Get all payments
- `GET /api/payments/<id>` - Get payment by transaction ID
- `POST /api/payments` - Create new payment
- `PUT /api/payments/<id>` - Update payment
- `DELETE /api/payments/<id>` - Delete payment

**Rating Endpoints:**
- `GET /api/ratings` - Get all ratings
- `GET /api/ratings/<id>` - Get rating by ID
- `POST /api/ratings` - Create new rating
- `PUT /api/ratings/<id>` - Update rating
- `DELETE /api/ratings/<id>` - Delete rating

**Report Endpoints:**
- `GET /api/reports/top-drivers` - Top drivers by ride count
- `GET /api/reports/revenue-by-method` - Revenue grouped by payment method
- `GET /api/reports/average-ratings` - Average ratings by driver
- `GET /api/reports/rides-by-location` - Popular pickup/dropoff locations
- `GET /api/reports/customer-history/<customer_id>` - Ride history for a customer

#### Response Format

All API responses follow this structure:

**Success Response:**
```json
{
  "success": true,
  "data": { ... } or [ ... ],
  "message": "Operation completed successfully"
}
```

**Error Response:**
```json
{
  "success": false,
  "error": "Error message describing what went wrong",
  "details": "Additional error details (optional)"
}
```

### Database Connection Management

**Connection Pool:**
- Use cx_Oracle connection pooling for efficient database connections
- Configure pool with min=2, max=10, increment=1
- Handle connection errors gracefully with retry logic

**Configuration:**
- Store database credentials in `.env` file
- Support both local Oracle Express and remote school DB
- Environment variables:
  - `DB_USER`: Database username
  - `DB_PASSWORD`: Database password
  - `DB_DSN`: Data source name (host:port/service_name)

**Example connection code:**
```python
import cx_Oracle
import os
from dotenv import load_dotenv

load_dotenv()

pool = cx_Oracle.SessionPool(
    user=os.getenv('DB_USER'),
    password=os.getenv('DB_PASSWORD'),
    dsn=os.getenv('DB_DSN'),
    min=2,
    max=10,
    increment=1
)

def get_connection():
    return pool.acquire()
```

## Data Models

### Database Schema (BCNF)

**Customer Table:**
```sql
Customer_ID (PK)
Customer_Name
Phone_Number
Email
```

**Driver Table:**
```sql
Driver_ID (PK)
Driver_Name
Phone_Number
License_Number
```

**Vehicle Table:**
```sql
Vehicle_VIN (PK)
Model
Color
Registration_Year
Driver_ID (FK -> Driver)
```

**Location Table:**
```sql
Location_ID (PK)
Address
City
Postal_Code
```

**Ride Table:**
```sql
Ride_ID (PK)
Customer_ID (FK -> Customer)
Driver_ID (FK -> Driver)
Vehicle_VIN (FK -> Vehicle)
Pickup_Location (FK -> Location)
Dropoff_Location (FK -> Location)
Start_Time
Arrival_Time
```

**Payment Table:**
```sql
Transaction_ID (PK)
Ride_ID (FK -> Ride)
Amount
Payment_Method
Payment_Status
Payment_Date
```

**Rating Table:**
```sql
Rating_ID (PK)
Ride_ID (FK -> Ride)
Customer_Rating
Driver_Rating
Comments
```

### Frontend Data Flow

1. User interacts with React component
2. Component calls API service function using Axios
3. Axios sends HTTP request to Flask backend
4. Backend processes request and queries Oracle DB
5. Backend returns JSON response
6. Frontend updates component state and re-renders UI

## Error Handling

### Frontend Error Handling

**Network Errors:**
- Catch Axios errors in try-catch blocks
- Display user-friendly error messages
- Provide retry option for failed requests

**Validation Errors:**
- Client-side validation before API calls
- Display inline error messages on form fields
- Prevent form submission if validation fails

**Example error handling:**
```javascript
try {
  const response = await axios.post('/api/customers', customerData);
  setSuccessMessage('Customer created successfully');
} catch (error) {
  if (error.response) {
    // Server responded with error
    setErrorMessage(error.response.data.error);
  } else if (error.request) {
    // No response received
    setErrorMessage('Network error. Please check your connection.');
  } else {
    setErrorMessage('An unexpected error occurred.');
  }
}
```

### Backend Error Handling

**Database Errors:**
- Catch cx_Oracle exceptions
- Log errors for debugging
- Return appropriate HTTP status codes
- Handle foreign key constraint violations specifically

**Validation Errors:**
- Validate input data before database operations
- Return 400 Bad Request for invalid data
- Provide specific error messages

**Example error handling:**
```python
@app.route('/api/customers/<int:customer_id>', methods=['DELETE'])
def delete_customer(customer_id):
    try:
        conn = get_connection()
        cursor = conn.cursor()
        cursor.execute("DELETE FROM Customer WHERE Customer_ID = :id", [customer_id])
        conn.commit()
        cursor.close()
        conn.close()
        return jsonify({"success": True, "message": "Customer deleted"}), 200
    except cx_Oracle.IntegrityError as e:
        return jsonify({
            "success": False,
            "error": "Cannot delete customer with existing rides"
        }), 400
    except Exception as e:
        return jsonify({
            "success": False,
            "error": str(e)
        }), 500
```

## Testing Strategy

### Frontend Testing

**Manual Testing:**
- Test all CRUD operations for each entity
- Verify form validation works correctly
- Test navigation between pages
- Verify error messages display properly
- Test responsive design on different screen sizes

**Component Testing:**
- Test individual components in isolation
- Verify props are passed correctly
- Test state management and updates

### Backend Testing

**API Testing:**
- Test all endpoints with valid data
- Test endpoints with invalid data
- Test foreign key constraint handling
- Verify correct HTTP status codes
- Test database connection error handling

**Manual Testing with Tools:**
- Use Postman or curl to test API endpoints
- Verify JSON response structure
- Test edge cases (empty results, large datasets)

### Database Testing

**Data Integrity:**
- Verify foreign key constraints work
- Test cascade delete behavior (if implemented)
- Ensure BCNF normalization is maintained
- Verify dummy data is properly inserted

**Query Testing:**
- Test all SELECT queries return correct results
- Test JOIN queries for rides with related data
- Test aggregate queries for reports
- Verify query performance with larger datasets

## Advanced Reports Implementation

### Report 1: Top Drivers by Ride Count
```sql
SELECT d.Driver_ID, d.Driver_Name, COUNT(r.Ride_ID) as Ride_Count
FROM Driver d
LEFT JOIN Ride r ON d.Driver_ID = r.Driver_ID
GROUP BY d.Driver_ID, d.Driver_Name
ORDER BY Ride_Count DESC
```

### Report 2: Revenue by Payment Method
```sql
SELECT Payment_Method, SUM(Amount) as Total_Revenue, COUNT(*) as Transaction_Count
FROM Payment
GROUP BY Payment_Method
ORDER BY Total_Revenue DESC
```

### Report 3: Average Rating by Driver
```sql
SELECT d.Driver_ID, d.Driver_Name, AVG(rt.Driver_Rating) as Avg_Rating, COUNT(rt.Rating_ID) as Rating_Count
FROM Driver d
LEFT JOIN Ride r ON d.Driver_ID = r.Driver_ID
LEFT JOIN Rating rt ON r.Ride_ID = rt.Ride_ID
WHERE rt.Driver_Rating IS NOT NULL
GROUP BY d.Driver_ID, d.Driver_Name
ORDER BY Avg_Rating DESC
```

### Report 4: Rides by Location
```sql
SELECT l.Location_ID, l.Address, l.City, 
       COUNT(DISTINCT r1.Ride_ID) as Pickup_Count,
       COUNT(DISTINCT r2.Ride_ID) as Dropoff_Count
FROM Location l
LEFT JOIN Ride r1 ON l.Location_ID = r1.Pickup_Location
LEFT JOIN Ride r2 ON l.Location_ID = r2.Dropoff_Location
GROUP BY l.Location_ID, l.Address, l.City
ORDER BY (Pickup_Count + Dropoff_Count) DESC
```

### Report 5: Customer Ride History
```sql
SELECT r.Ride_ID, r.Start_Time, r.Arrival_Time,
       d.Driver_Name, v.Model as Vehicle_Model,
       l1.Address as Pickup_Address, l2.Address as Dropoff_Address,
       p.Amount, p.Payment_Status
FROM Ride r
JOIN Driver d ON r.Driver_ID = d.Driver_ID
JOIN Vehicle v ON r.Vehicle_VIN = v.Vehicle_VIN
JOIN Location l1 ON r.Pickup_Location = l1.Location_ID
JOIN Location l2 ON r.Dropoff_Location = l2.Location_ID
LEFT JOIN Payment p ON r.Ride_ID = p.Ride_ID
WHERE r.Customer_ID = :customer_id
ORDER BY r.Start_Time DESC
```

## Deployment and Running

### Development Setup

**Backend:**
1. Install Python dependencies: `pip install -r requirements.txt`
2. Configure `.env` file with database credentials
3. Run Flask server: `python app.py` (runs on port 5000)

**Frontend:**
1. Install Node dependencies: `npm install`
2. Configure API base URL in Axios configuration
3. Run development server: `npm start` (runs on port 5173)

### Production Considerations

- Use environment-specific configuration
- Enable Flask production mode
- Build React app for production: `npm run build`
- Serve React build files through Flask or separate web server
- Implement proper logging
- Add authentication/authorization if needed
- Use HTTPS for secure communication

## Code Comments and Documentation

All code will include:
- Function/method docstrings explaining purpose and parameters
- Inline comments for complex logic
- SQL query comments explaining joins and special cases
- README files with setup instructions
- API documentation with example requests/responses
