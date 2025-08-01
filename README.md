# Explore With Me

Explore With Me is a Spring Boot application designed to manage events, users, and participation requests. It provides functionality for creating, updating, and retrieving events, as well as managing user accounts and event participation. The application includes both public and private APIs, with administrative controls for managing event states.

## Table of Contents

- Features
- Technologies
- Project Structure
- Setup and Installation
- API Endpoints
- Usage
- Contributing
- License

## Features

- **User Management**: Create, retrieve, and delete user accounts with validation for email and name.
- **Event Management**:
  - Create and update events with details like title, description, location, and participant limits.
  - Support for event states (PENDING, PUBLISHED, CANCELED).
  - Public event search with filtering by text, categories, paid status, and date range.
  - Administrative event management, including publishing and updating events.
- **Participation Requests**: Manage event participation requests, including approval and rejection.
- **Statistics Tracking**: Integration with a stats service to track event views.
- **Validation**: Robust input validation for all API endpoints.

## Technologies

- **Java**: 17
- **Spring Boot**: 3.x
- **Spring Data JPA**: For database interactions
- **MapStruct**: For object mapping between entities and DTOs
- **Hibernate**: ORM for database persistence
- **Lombok**: To reduce boilerplate code
- **H2/PostgreSQL**: Database (configurable)
- **Maven**: Dependency management
- **SLF4J**: Logging

## Project Structure

```
main-service/
├── src/main/java/ru/practicum/
│   ├── category/               # Category-related entities, DTOs, and services
│   ├── event/                  # Event-related entities, DTOs, services, and controllers
│   │   ├── admin/             # Admin-specific event management
│   │   ├── pub/               # Public event APIs
│   │   ├── mapper/            # Mappers for event-related DTOs
│   ├── user/                   # User-related entities, DTOs, services, and controllers
│   ├── request/                # Participation request entities and services
│   ├── utils/                  # Utility classes for validation and logging
├── src/main/resources/
│   ├── application.properties  # Configuration for database, stats service, etc.
```

### Key Components

- **Entities**: `User`, `Event`, `Location`, `Category`
- **DTOs**: Data Transfer Objects for users (`UserDto`, `UserShortDto`), events (`EventFullDto`, `EventShortDto`, `NewEventDto`, `UpdateEventRequest`), and locations (`LocationDto`)
- **Services**:
  - `UserService`: Manages user CRUD operations
  - `EventService`: Handles event creation, updates, and retrieval
  - `EventPublicService`: Public event search and retrieval
  - `EventAdminService`: Admin controls for event management
- **Controllers**: REST endpoints for user, event, and participation request operations
- **Repositories**: JPA repositories for database access
- **Mappers**: MapStruct-based mappings between entities and DTOs

## Setup and Installation

1. **Clone the Repository**:

   ```bash
   git clone https://github.com/your-repo/explore-with-me.git
   cd explore-with-me
   ```

2. **Install Dependencies**: Ensure Maven is installed, then run:

   ```bash
   mvn clean install
   ```

3. **Configure the Database**: Update `application.properties` to configure the database (e.g., H2 for development or PostgreSQL for production):

   ```properties
   spring.datasource.url=jdbc:h2:mem:test
   spring.datasource.driverClassName=org.h2.Driver
   spring.jpa.hibernate.ddl-auto=update
   ```

4. **Configure Stats Service**: Set the stats service URL in `application.properties`:

   ```properties
   my.url=http://stats-service-url
   my.app=explore-with-me
   ```

5. **Run the Application**:

   ```bash
   mvn spring-boot:run
   ```

6. **Access the API**: The application runs on `http://localhost:8080` by default.

## API Endpoints

### User Management (Admin)

- **POST /admin/users**: Create a new user
- **GET /admin/users**: Retrieve users (with optional filtering by IDs and pagination)
- **DELETE /admin/users/{userId}**: Delete a user by ID

### Event Management (Private)

- **POST /users/{userId}/events**: Create a new event
- **PATCH /users/{userId}/events/{eventId}**: Update an event
- **GET /users/{userId}/events**: Retrieve events by initiator
- **GET /users/{userId}/events/{eventId}**: Retrieve a specific event by ID
- **GET /users/{userId}/events/{eventId}/requests**: Retrieve participation requests for an event
- **PATCH /users/{userId}/events/{eventId}/requests**: Update participation request status

### Event Management (Public)

- **GET /events**: Search for events with filters (text, categories, paid, date range, etc.)
- **GET /events/{id}**: Retrieve a specific event by ID

### Event Management (Admin)

- **GET /admin/events**: Retrieve events with filters (users, states, categories, date range)
- **PATCH /admin/events/{eventId}**: Update and publish an event

## Usage

1. **Creating a User**: Send a POST request to `/admin/users` with a JSON body:

   ```json
   {
       "name": "John Doe",
       "email": "john.doe@example.com"
   }
   ```

2. **Creating an Event**: Send a POST request to `/users/{userId}/events`:

   ```json
   {
       "annotation": "Event description",
       "category": 1,
       "description": "Detailed event description",
       "eventDate": "2025-12-01 12:00:00",
       "location": { "lat": 55.7558, "lon": 37.6173 },
       "paid": false,
       "participantLimit": 100,
       "requestModeration": true,
       "title": "Sample Event"
   }
   ```

3. **Searching Events**: Send a GET request to `/events` with query parameters:

   ```
   /events?text=party&categories=1,2&paid=false&rangeStart=2025-01-01%2000:00:00&rangeEnd=2025-12-31%2023:59:59
   ```

4. **Publishing an Event (Admin)**: Send a PATCH request to `/admin/events/{eventId}` with a JSON body:

   ```json
   {
       "stateAction": "PUBLISH_EVENT"
   }
   ```

## Contributing

1. Fork the repository.
2. Create a new branch (`git checkout -b feature/your-feature`).
3. Commit your changes (`git commit -m 'Add your feature'`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a Pull Request.