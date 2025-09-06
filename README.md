# FitMap

A fitness mapping application built with Java Google Cloud Functions and Firebase authentication.

## Overview

FitMap is a cloud-native application that helps users discover and share fitness-related locations and information. The application consists of a Java-based backend running on Google Cloud Functions and a Firebase-powered authentication frontend.

## Architecture

The project is organized into two main components:

### 1. Backend Functions (`functions/`)
- **Technology**: Java 11, Spring Boot, Google Cloud Functions
- **Framework**: Maven-based project with Google Cloud Functions Framework
- **Database**: Google Cloud Firestore
- **Authentication**: Firebase Admin SDK
- **Main Function**: `SetRolesFunction` - Manages user role assignment
- **Testing**: Comprehensive test suite with JUnit 5, Mockito, and AssertJ

### 2. Frontend Authentication (`web-auth/`)
- **Technology**: HTML, JavaScript, Firebase Authentication
- **Hosting**: Firebase Hosting
- **Features**: User sign-in/sign-up interface with Firebase Auth UI
- **Authentication Methods**: Email/password authentication

## Prerequisites

- **Java 11** (required for compilation and execution)
- **Maven 3.6+** for building the functions
- **Docker** for local development
- **Google Cloud SDK** (gcloud CLI) for deployment
- **Firebase CLI** for frontend deployment
- **Node.js** (if extending the frontend)

## Project Structure

```
fitmap/
├── functions/                  # Backend Cloud Functions
│   ├── src/
│   │   ├── main/java/         # Source code
│   │   │   └── com/fitmap/function/
│   │   │       ├── common/    # Shared utilities and services
│   │   │       └── setroles/  # Role management function
│   │   └── test/java/         # Test code
│   ├── pom.xml                # Maven configuration
│   ├── docker-compose.yml     # Local development setup
│   └── service.sh             # Build and deployment scripts
├── web-auth/                  # Frontend authentication
│   ├── public/                # Static web files
│   │   ├── index.html         # Authentication interface
│   │   └── 404.html           # Error page
│   └── firebase.json          # Firebase hosting configuration
└── README.md                  # This file
```

## Getting Started

### Development Setup

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd fitmap
   ```

2. **Set up Java 11**
   ```bash
   # Ensure Java 11 is installed and active
   java -version  # Should show Java 11
   ```

3. **Set up Google Cloud**
   ```bash
   # Install and configure Google Cloud SDK
   gcloud auth login
   gcloud config set project YOUR_PROJECT_ID
   ```

4. **Set up Firebase**
   ```bash
   # Install Firebase CLI
   npm install -g firebase-tools
   firebase login
   ```

### Local Development

#### Backend Functions

1. **Build the project**
   ```bash
   cd functions
   mvn clean compile
   ```

2. **Run tests**
   ```bash
   mvn test
   ```

3. **Run locally with Docker**
   ```bash
   # Start the function locally
   ./service.sh up
   
   # View logs
   ./service.sh log set-roles
   
   # Stop the service
   ./service.sh down
   ```

4. **Manual Docker build**
   ```bash
   ./service.sh build
   ```

The function will be available at `http://localhost:9050`

#### Frontend Authentication

1. **Navigate to web-auth directory**
   ```bash
   cd web-auth
   ```

2. **Serve locally**
   ```bash
   firebase serve
   ```

The web interface will be available at `http://localhost:5000`

## API Documentation

### SetRoles Function

**Endpoint**: `/` (HTTP POST)  
**Content-Type**: `application/json`

Manages user role assignment in the system.

**Request Body**:
```json
{
  "userId": "string",
  "roles": ["role1", "role2"]
}
```

**Responses**:
- `200 OK`: Role assignment successful
- `400 Bad Request`: Invalid request body or validation errors
- `405 Method Not Allowed`: Non-POST request
- `415 Unsupported Media Type`: Invalid content type
- `500 Internal Server Error`: Server error

## Deployment

### Backend Deployment

1. **Build the package**
   ```bash
   cd functions
   ./service.sh build
   ```

2. **Check deployment files**
   ```bash
   ./service.sh check-deploy
   ```

3. **Deploy to Google Cloud**
   ```bash
   ./service.sh deploy FUNCTION_NAME ENTRY_POINT
   # Example:
   ./service.sh deploy set-roles com.fitmap.function.setroles.SetRolesFunction
   ```

The function will be deployed to the `southamerica-east1` region with the following configuration:
- Runtime: Java 11
- Trigger: HTTP
- Max instances: 30

### Frontend Deployment

```bash
cd web-auth
firebase deploy
```

## Testing

The project includes comprehensive testing for the backend functions:

```bash
cd functions
mvn test
```

**Test Coverage**:
- Unit tests for all service classes
- Integration tests for the main function
- Mock testing for external dependencies (Firebase Auth, Firestore)

## Configuration

### Environment Variables

The application uses the following configuration:

- **Google Cloud Project**: Set via `gcloud config`
- **Firebase Configuration**: Managed through Firebase console
- **Time Zone**: UTC (configured in `SystemTimeZoneConfig`)

### Firebase Configuration

Update `web-auth/firebase.json` for hosting configuration and ensure Firebase project is properly configured with:
- Authentication providers (Email/Password)
- Firestore database
- Hosting rules

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Use Java 11 for compatibility
- Follow existing code style and patterns
- Add tests for new functionality
- Update documentation as needed
- Use Lombok annotations appropriately

## Troubleshooting

### Common Issues

1. **Java Version Mismatch**
   - Ensure Java 11 is installed and active
   - Check with `java -version` and `javac -version`

2. **Maven Build Failures**
   - Clean and rebuild: `mvn clean compile`
   - Check internet connectivity for dependency downloads

3. **Docker Issues**
   - Ensure Docker is running
   - Check if port 9050 is available
   - Verify Docker Compose network setup

4. **Firebase Deployment Issues**
   - Verify Firebase CLI is logged in: `firebase login`
   - Check project configuration: `firebase projects:list`

## License

This project is licensed under [LICENSE_TYPE] - see the LICENSE file for details.

## Support

For support and questions:
- Create an issue in the repository
- Contact the development team
- Check the troubleshooting section above

---

**Note**: This project requires Java 11 for proper compilation and execution. Using newer Java versions may cause compatibility issues with Lombok and other dependencies.