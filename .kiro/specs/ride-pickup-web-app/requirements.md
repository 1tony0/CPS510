# Requirements Document

## Introduction

This document outlines the requirements for a web-based Ride & Pickup Database Management System (DBMS) application. The system will provide a user interface for managing and interacting with a ride-sharing database that includes customers, drivers, vehicles, locations, rides, payments, and ratings. The application uses a React frontend, Python backend (Flask/FastAPI), and Oracle Database, with all tables normalized to BCNF as per the database design completed in previous assignments.

## Requirements

### Requirement 1: Database Connection and Setup

**User Story:** As a system administrator, I want the application to connect to an Oracle database, so that all ride-sharing data can be stored and retrieved reliably.

#### Acceptance Criteria

1. WHEN the backend application starts THEN it SHALL establish a connection to the Oracle database using cx_Oracle or oracledb library
2. IF the database connection fails THEN the system SHALL log an error message and provide clear feedback
3. WHEN the application initializes THEN it SHALL verify that all required tables (Customer, Driver, Vehicle, Location, Ride, Payment, Rating) exist
4. THE system SHALL support connection to both local Oracle Express and school Oracle DB instances through configuration

### Requirement 2: Customer Management

**User Story:** As a user, I want to view, add, update, and delete customer records, so that I can manage customer information in the system.

#### Acceptance Criteria

1. WHEN a user navigates to the customers page THEN the system SHALL display a list of all customers with Customer_ID, Customer_Name, Phone_Number, and Email
2. WHEN a user clicks "Add Customer" THEN the system SHALL display a form to input customer details
3. WHEN a user submits a new customer form with valid data THEN the system SHALL insert the record into the Customer table and display a success message
4. WHEN a user selects a customer and clicks "Edit" THEN the system SHALL display a pre-filled form with the customer's current data
5. WHEN a user updates customer information THEN the system SHALL update the corresponding record in the database
6. WHEN a user selects a customer and clicks "Delete" THEN the system SHALL remove the customer record from the database
7. IF a user attempts to delete a customer with associated rides THEN the system SHALL display an error message about foreign key constraints

### Requirement 3: Driver Management

**User Story:** As a user, I want to view, add, update, and delete driver records, so that I can manage driver information in the system.

#### Acceptance Criteria

1. WHEN a user navigates to the drivers page THEN the system SHALL display a list of all drivers with Driver_ID, Driver_Name, Phone_Number, and License_Number
2. WHEN a user clicks "Add Driver" THEN the system SHALL display a form to input driver details
3. WHEN a user submits a new driver form with valid data THEN the system SHALL insert the record into the Driver table
4. WHEN a user selects a driver and clicks "Edit" THEN the system SHALL display a pre-filled form with the driver's current data
5. WHEN a user updates driver information THEN the system SHALL update the corresponding record in the database
6. WHEN a user selects a driver and clicks "Delete" THEN the system SHALL remove the driver record from the database
7. IF a user attempts to delete a driver with associated vehicles or rides THEN the system SHALL display an error message about foreign key constraints

### Requirement 4: Vehicle Management

**User Story:** As a user, I want to view, add, update, and delete vehicle records, so that I can manage vehicle information and their driver assignments.

#### Acceptance Criteria

1. WHEN a user navigates to the vehicles page THEN the system SHALL display a list of all vehicles with Vehicle_VIN, Model, Color, Registration_Year, and assigned Driver_ID
2. WHEN a user clicks "Add Vehicle" THEN the system SHALL display a form to input vehicle details including a dropdown to select an existing driver
3. WHEN a user submits a new vehicle form with valid data THEN the system SHALL insert the record into the Vehicle table
4. WHEN a user selects a vehicle and clicks "Edit" THEN the system SHALL display a pre-filled form with the vehicle's current data
5. WHEN a user updates vehicle information THEN the system SHALL update the corresponding record in the database
6. WHEN a user selects a vehicle and clicks "Delete" THEN the system SHALL remove the vehicle record from the database
7. IF a user attempts to delete a vehicle with associated rides THEN the system SHALL display an error message about foreign key constraints

### Requirement 5: Location Management

**User Story:** As a user, I want to view, add, update, and delete location records, so that I can manage pickup and dropoff locations in the system.

#### Acceptance Criteria

1. WHEN a user navigates to the locations page THEN the system SHALL display a list of all locations with Location_ID, Address, City, and Postal_Code
2. WHEN a user clicks "Add Location" THEN the system SHALL display a form to input location details
3. WHEN a user submits a new location form with valid data THEN the system SHALL insert the record into the Location table
4. WHEN a user selects a location and clicks "Edit" THEN the system SHALL display a pre-filled form with the location's current data
5. WHEN a user updates location information THEN the system SHALL update the corresponding record in the database
6. WHEN a user selects a location and clicks "Delete" THEN the system SHALL remove the location record from the database

### Requirement 6: Ride Management

**User Story:** As a user, I want to view, add, update, and delete ride records, so that I can track all rides in the system with their associated customers, drivers, vehicles, and locations.

#### Acceptance Criteria

1. WHEN a user navigates to the rides page THEN the system SHALL display a list of all rides with Ride_ID, Customer_ID, Driver_ID, Vehicle_VIN, Pickup_Location, Dropoff_Location, Start_Time, and Arrival_Time
2. WHEN a user clicks "Add Ride" THEN the system SHALL display a form with dropdowns to select existing customers, drivers, vehicles, and locations
3. WHEN a user submits a new ride form with valid data THEN the system SHALL insert the record into the Ride table
4. WHEN displaying ride information THEN the system SHALL show related names (customer name, driver name, vehicle model, location addresses) instead of just IDs for better readability
5. WHEN a user selects a ride and clicks "Edit" THEN the system SHALL display a pre-filled form with the ride's current data
6. WHEN a user updates ride information THEN the system SHALL update the corresponding record in the database
7. WHEN a user selects a ride and clicks "Delete" THEN the system SHALL remove the ride record from the database

### Requirement 7: Payment Management

**User Story:** As a user, I want to view, add, update, and delete payment records, so that I can track payment transactions for rides.

#### Acceptance Criteria

1. WHEN a user navigates to the payments page THEN the system SHALL display a list of all payments with Transaction_ID, Ride_ID, Amount, Payment_Method, Payment_Status, and Payment_Date
2. WHEN a user clicks "Add Payment" THEN the system SHALL display a form with a dropdown to select an existing ride and fields for payment details
3. WHEN a user submits a new payment form with valid data THEN the system SHALL insert the record into the Payment table
4. WHEN displaying payment information THEN the system SHALL show related ride details for context
5. WHEN a user selects a payment and clicks "Edit" THEN the system SHALL display a pre-filled form with the payment's current data
6. WHEN a user updates payment information THEN the system SHALL update the corresponding record in the database
7. WHEN a user selects a payment and clicks "Delete" THEN the system SHALL remove the payment record from the database

### Requirement 8: Rating Management

**User Story:** As a user, I want to view, add, update, and delete rating records, so that I can track customer and driver ratings for completed rides.

#### Acceptance Criteria

1. WHEN a user navigates to the ratings page THEN the system SHALL display a list of all ratings with Rating_ID, Ride_ID, Customer_Rating, Driver_Rating, and Comments
2. WHEN a user clicks "Add Rating" THEN the system SHALL display a form with a dropdown to select an existing ride and fields for rating details
3. WHEN a user submits a new rating form with valid data THEN the system SHALL insert the record into the Rating table
4. WHEN displaying rating information THEN the system SHALL show related ride details for context
5. WHEN a user selects a rating and clicks "Edit" THEN the system SHALL display a pre-filled form with the rating's current data
6. WHEN a user updates rating information THEN the system SHALL update the corresponding record in the database
7. WHEN a user selects a rating and clicks "Delete" THEN the system SHALL remove the rating record from the database

### Requirement 9: Advanced Reports and Queries

**User Story:** As a user, I want to run advanced reports and queries on the database, so that I can gain insights into ride patterns, driver performance, and revenue.

#### Acceptance Criteria

1. WHEN a user navigates to the reports page THEN the system SHALL display options for multiple report types
2. WHEN a user selects "Top Drivers by Ride Count" THEN the system SHALL execute a query that groups rides by driver and displays drivers sorted by number of rides
3. WHEN a user selects "Revenue by Payment Method" THEN the system SHALL execute a query that aggregates payment amounts grouped by payment method
4. WHEN a user selects "Average Rating by Driver" THEN the system SHALL execute a query that calculates average driver ratings
5. WHEN a user selects "Rides by Location" THEN the system SHALL execute a query showing the most popular pickup and dropoff locations
6. WHEN a user selects "Customer Ride History" THEN the system SHALL display a form to select a customer and show all their rides with details
7. THE system SHALL include comments in the backend code explaining the SQL queries and any special cases

### Requirement 10: User Interface and Navigation

**User Story:** As a user, I want an intuitive and responsive web interface, so that I can easily navigate between different sections of the application.

#### Acceptance Criteria

1. WHEN a user accesses the application THEN the system SHALL display a navigation menu with links to all main sections (Customers, Drivers, Vehicles, Locations, Rides, Payments, Ratings, Reports)
2. WHEN a user clicks on a navigation link THEN the system SHALL load the corresponding page without a full page reload
3. THE application SHALL use a responsive design that works on different screen sizes
4. WHEN the system performs an operation THEN it SHALL provide visual feedback (loading indicators, success/error messages)
5. THE user interface SHALL use React components for modularity and reusability

### Requirement 11: Error Handling and Validation

**User Story:** As a user, I want clear error messages and input validation, so that I can understand and correct any issues when interacting with the system.

#### Acceptance Criteria

1. WHEN a user submits a form with missing required fields THEN the system SHALL display validation errors highlighting the missing fields
2. WHEN a database operation fails THEN the system SHALL display a user-friendly error message
3. WHEN a foreign key constraint is violated THEN the system SHALL explain the relationship preventing the operation
4. WHEN the backend API returns an error THEN the frontend SHALL catch and display the error appropriately
5. THE system SHALL validate data types and formats (e.g., email format, phone number format) before submission

### Requirement 12: API Endpoints

**User Story:** As a developer, I want well-structured RESTful API endpoints, so that the frontend can communicate effectively with the backend and database.

#### Acceptance Criteria

1. THE backend SHALL provide GET endpoints for retrieving all records from each table
2. THE backend SHALL provide GET endpoints for retrieving a single record by ID from each table
3. THE backend SHALL provide POST endpoints for creating new records in each table
4. THE backend SHALL provide PUT endpoints for updating existing records in each table
5. THE backend SHALL provide DELETE endpoints for removing records from each table
6. THE backend SHALL provide GET endpoints for advanced reports and queries
7. ALL API endpoints SHALL return appropriate HTTP status codes (200, 201, 400, 404, 500)
8. ALL API endpoints SHALL return JSON responses with consistent structure
