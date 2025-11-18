# Implementation Plan

- [x] 1. Set up backend database connection and configuration
  - Create `backend/config.py` for configuration management
  - Create `backend/db.py` with Oracle connection pool setup using cx_Oracle
  - Implement `get_connection()` function with error handling
  - Create `.env.example` file with required environment variables
  - Add comments explaining connection pool parameters and error handling
  - _Requirements: 1.1, 1.2, 1.3, 1.4_

- [x] 2. Implement backend dependencies and Flask app initialization
  - Update `backend/requirements.txt` with all required packages (Flask, Flask-CORS, cx_Oracle, python-dotenv)
  - Create `backend/app.py` with Flask app initialization
  - Configure CORS for frontend communication
  - Add health check endpoint to verify server is running
  - _Requirements: 12.7, 12.8_

- [x] 3. Implement Customer API endpoints
  - Implement GET `/api/customers` endpoint to retrieve all customers
  - Implement GET `/api/customers/<id>` endpoint to retrieve single customer
  - Implement POST `/api/customers` endpoint to create new customer with validation
  - Implement PUT `/api/customers/<id>` endpoint to update customer
  - Implement DELETE `/api/customers/<id>` endpoint with foreign key error handling
  - Add SQL query comments explaining each operation
  - _Requirements: 2.1, 2.2, 2.3, 2.4, 2.5, 2.6, 2.7, 12.1, 12.2, 12.3, 12.4, 12.5_

- [x] 4. Implement Driver API endpoints
  - Implement GET `/api/drivers` endpoint to retrieve all drivers
  - Implement GET `/api/drivers/<id>` endpoint to retrieve single driver
  - Implement POST `/api/drivers` endpoint to create new driver with validation
  - Implement PUT `/api/drivers/<id>` endpoint to update driver
  - Implement DELETE `/api/drivers/<id>` endpoint with foreign key error handling
  - Add SQL query comments explaining each operation
  - _Requirements: 3.1, 3.2, 3.3, 3.4, 3.5, 3.6, 3.7, 12.1, 12.2, 12.3, 12.4, 12.5_

- [x] 5. Implement Vehicle API endpoints
  - Implement GET `/api/vehicles` endpoint with JOIN to include driver information
  - Implement GET `/api/vehicles/<vin>` endpoint to retrieve single vehicle
  - Implement POST `/api/vehicles` endpoint to create new vehicle with driver validation
  - Implement PUT `/api/vehicles/<vin>` endpoint to update vehicle
  - Implement DELETE `/api/vehicles/<vin>` endpoint with foreign key error handling
  - Add SQL query comments explaining JOIN operations
  - _Requirements: 4.1, 4.2, 4.3, 4.4, 4.5, 4.6, 4.7, 12.1, 12.2, 12.3, 12.4, 12.5_

- [x] 6. Implement Location API endpoints
  - Implement GET `/api/locations` endpoint to retrieve all locations
  - Implement GET `/api/locations/<id>` endpoint to retrieve single location
  - Implement POST `/api/locations` endpoint to create new location with validation
  - Implement PUT `/api/locations/<id>` endpoint to update location
  - Implement DELETE `/api/locations/<id>` endpoint
  - Add SQL query comments explaining each operation
  - _Requirements: 5.1, 5.2, 5.3, 5.4, 5.5, 5.6, 12.1, 12.2, 12.3, 12.4, 12.5_

- [x] 7. Implement Ride API endpoints with complex JOINs
  - Implement GET `/api/rides` endpoint with JOINs to Customer, Driver, Vehicle, and Location tables
  - Implement GET `/api/rides/<id>` endpoint with full ride details
  - Implement POST `/api/rides` endpoint with foreign key validation
  - Implement PUT `/api/rides/<id>` endpoint to update ride
  - Implement DELETE `/api/rides/<id>` endpoint
  - Add detailed SQL query comments explaining multi-table JOINs
  - _Requirements: 6.1, 6.2, 6.3, 6.4, 6.5, 6.6, 6.7, 12.1, 12.2, 12.3, 12.4, 12.5_

- [x] 8. Implement Payment API endpoints
  - Implement GET `/api/payments` endpoint with JOIN to Ride table
  - Implement GET `/api/payments/<id>` endpoint to retrieve single payment
  - Implement POST `/api/payments` endpoint with ride validation
  - Implement PUT `/api/payments/<id>` endpoint to update payment
  - Implement DELETE `/api/payments/<id>` endpoint
  - Add SQL query comments explaining each operation
  - _Requirements: 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, 7.7, 12.1, 12.2, 12.3, 12.4, 12.5_

- [x] 9. Implement Rating API endpoints
  - Implement GET `/api/ratings` endpoint with JOIN to Ride table
  - Implement GET `/api/ratings/<id>` endpoint to retrieve single rating
  - Implement POST `/api/ratings` endpoint with ride validation
  - Implement PUT `/api/ratings/<id>` endpoint to update rating
  - Implement DELETE `/api/ratings/<id>` endpoint
  - Add SQL query comments explaining each operation
  - _Requirements: 8.1, 8.2, 8.3, 8.4, 8.5, 8.6, 8.7, 12.1, 12.2, 12.3, 12.4, 12.5_

- [x] 10. Implement advanced report endpoints
  - Implement GET `/api/reports/top-drivers` with GROUP BY and COUNT aggregate
  - Implement GET `/api/reports/revenue-by-method` with GROUP BY and SUM aggregate
  - Implement GET `/api/reports/average-ratings` with multiple JOINs and AVG aggregate
  - Implement GET `/api/reports/rides-by-location` with complex JOIN and COUNT
  - Implement GET `/api/reports/customer-history/<customer_id>` with multi-table JOIN
  - Add detailed comments explaining each complex query and aggregation
  - _Requirements: 9.1, 9.2, 9.3, 9.4, 9.5, 9.6, 9.7, 12.6_

- [x] 11. Set up React frontend project structure
  - Create `frontend/src` directory structure (components, services, utils)
  - Create `frontend/index.html` as entry point
  - Create `frontend/src/main.jsx` to render React app
  - Create `frontend/vite.config.js` for Vite configuration
  - Create `frontend/src/App.jsx` as root component
  - Create `frontend/src/App.css` for global styles
  - _Requirements: 10.5_

- [x] 12. Implement API service layer in frontend
  - Create `frontend/src/services/api.js` with Axios configuration
  - Configure base URL for backend API
  - Implement error interceptor for handling API errors
  - Create service functions for all entity endpoints (customers, drivers, vehicles, locations, rides, payments, ratings)
  - Create service functions for report endpoints
  - _Requirements: 11.4, 12.7, 12.8_

- [x] 13. Implement Navigation component
  - Create `frontend/src/components/Navigation.jsx`
  - Add navigation links for all main sections
  - Implement active link highlighting
  - Add responsive styling for mobile devices
  - _Requirements: 10.1, 10.2_

- [x] 14. Implement shared UI components
  - Create `frontend/src/components/LoadingSpinner.jsx` for loading states
  - Create `frontend/src/components/ErrorMessage.jsx` for error display
  - Create `frontend/src/components/SuccessMessage.jsx` for success notifications
  - Create `frontend/src/components/ConfirmDialog.jsx` for delete confirmations
  - Add styling for each component
  - _Requirements: 10.4_

- [x] 15. Implement Customer management components
  - Create `frontend/src/components/CustomerList.jsx` to display all customers in a table
  - Create `frontend/src/components/CustomerForm.jsx` for add/edit customer
  - Implement form validation for required fields and email format
  - Implement CRUD operations using API service
  - Add error handling and success messages
  - _Requirements: 2.1, 2.2, 2.3, 2.4, 2.5, 2.6, 11.1, 11.2_

- [x] 16. Implement Driver management components
  - Create `frontend/src/components/DriverList.jsx` to display all drivers in a table
  - Create `frontend/src/components/DriverForm.jsx` for add/edit driver
  - Implement form validation for required fields
  - Implement CRUD operations using API service
  - Add error handling and success messages
  - _Requirements: 3.1, 3.2, 3.3, 3.4, 3.5, 3.6, 11.1, 11.2_

- [x] 17. Implement Vehicle management components
  - Create `frontend/src/components/VehicleList.jsx` to display all vehicles with driver names
  - Create `frontend/src/components/VehicleForm.jsx` for add/edit vehicle
  - Implement dropdown to select driver from available drivers
  - Implement form validation for required fields
  - Implement CRUD operations using API service
  - Add error handling and success messages
  - _Requirements: 4.1, 4.2, 4.3, 4.4, 4.5, 4.6, 11.1, 11.2_

- [x] 18. Implement Location management components
  - Create `frontend/src/components/LocationList.jsx` to display all locations in a table
  - Create `frontend/src/components/LocationForm.jsx` for add/edit location
  - Implement form validation for required fields
  - Implement CRUD operations using API service
  - Add error handling and success messages
  - _Requirements: 5.1, 5.2, 5.3, 5.4, 5.5, 5.6, 11.1, 11.2_

- [x] 19. Implement Ride management components
  - Create `frontend/src/components/RideList.jsx` to display all rides with related entity names
  - Create `frontend/src/components/RideForm.jsx` for add/edit ride
  - Implement dropdowns for customer, driver, vehicle, pickup location, and dropoff location
  - Implement datetime inputs for start time and arrival time
  - Implement form validation for required fields
  - Implement CRUD operations using API service
  - Add error handling and success messages
  - _Requirements: 6.1, 6.2, 6.3, 6.4, 6.5, 6.6, 6.7, 11.1, 11.2_

- [x] 20. Implement Payment management components
  - Create `frontend/src/components/PaymentList.jsx` to display all payments with ride details
  - Create `frontend/src/components/PaymentForm.jsx` for add/edit payment
  - Implement dropdown to select ride
  - Implement dropdown for payment method and payment status
  - Implement form validation for required fields and amount format
  - Implement CRUD operations using API service
  - Add error handling and success messages
  - _Requirements: 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, 7.7, 11.1, 11.2_

- [x] 21. Implement Rating management components
  - Create `frontend/src/components/RatingList.jsx` to display all ratings with ride details
  - Create `frontend/src/components/RatingForm.jsx` for add/edit rating
  - Implement dropdown to select ride
  - Implement number inputs for customer rating and driver rating (1-5 scale)
  - Implement textarea for comments
  - Implement form validation for required fields and rating range
  - Implement CRUD operations using API service
  - Add error handling and success messages
  - _Requirements: 8.1, 8.2, 8.3, 8.4, 8.5, 8.6, 8.7, 11.1, 11.2_

- [x] 22. Implement Reports component
  - Create `frontend/src/components/Reports.jsx` with tabs or dropdown for report selection
  - Implement "Top Drivers by Ride Count" report display
  - Implement "Revenue by Payment Method" report display
  - Implement "Average Rating by Driver" report display
  - Implement "Rides by Location" report display
  - Implement "Customer Ride History" report with customer selector dropdown
  - Display report results in formatted tables
  - Add error handling for report API calls
  - _Requirements: 9.1, 9.2, 9.3, 9.4, 9.5, 9.6_

- [x] 23. Integrate all components in App.jsx with routing
  - Implement client-side routing (using state or React Router if needed)
  - Add Navigation component to App.jsx
  - Create routes/views for each entity management section
  - Create route for Reports section
  - Implement view switching without full page reload
  - Add default/home view
  - _Requirements: 10.1, 10.2, 10.3_

- [x] 24. Add comprehensive error handling and validation
  - Implement try-catch blocks in all API service calls
  - Add network error handling with user-friendly messages
  - Implement form validation for all input forms
  - Add validation for email format, phone numbers, and numeric fields
  - Display validation errors inline on form fields
  - Handle foreign key constraint errors with specific messages
  - _Requirements: 11.1, 11.2, 11.3, 11.4, 11.5_

- [x] 25. Create database initialization script with dummy data
  - Create `backend/init_db.sql` or `backend/seed_data.py` script
  - Add INSERT statements for dummy customers (at least 5 records)
  - Add INSERT statements for dummy drivers (at least 5 records)
  - Add INSERT statements for dummy vehicles (at least 5 records)
  - Add INSERT statements for dummy locations (at least 10 records)
  - Add INSERT statements for dummy rides (at least 15 records)
  - Add INSERT statements for dummy payments (at least 10 records)
  - Add INSERT statements for dummy ratings (at least 8 records)
  - Add comments explaining the test data scenarios
  - _Requirements: 1.3_

- [x] 26. Create documentation and setup instructions
  - Update `README.md` with project overview and architecture
  - Add backend setup instructions (dependencies, .env configuration, running server)
  - Add frontend setup instructions (dependencies, running dev server)
  - Document all API endpoints with example requests and responses
  - Add database setup instructions (Oracle connection, running seed script)
  - Include screenshots or descriptions of key features
  - Add troubleshooting section for common issues
  - _Requirements: 1.1, 1.2, 1.3, 1.4_

- [x] 27. Test all functionality end-to-end
  - Test all CRUD operations for each entity through the UI
  - Verify foreign key relationships work correctly
  - Test all advanced reports display correct data
  - Test error handling for invalid inputs
  - Test error handling for foreign key constraint violations
  - Verify responsive design works on different screen sizes
  - Test database connection error handling
  - Verify all success and error messages display correctly
  - _Requirements: All requirements_
