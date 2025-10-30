# MuleSoft Hello World Application

A simple Hello World application built with MuleSoft 4.x that demonstrates basic HTTP listener functionality.

## Project Structure

```
mulesoft-hello-world/
├── pom.xml                          # Maven project configuration
├── mule-artifact.json               # Mule artifact descriptor
├── src/
│   ├── main/
│   │   ├── mule/
│   │   │   └── hello-world.xml      # Main Mule configuration
│   │   └── resources/               # Application resources
│   └── test/
│       └── munit/                   # MUnit test files
└── README.md                        # This file
```

## Features

- Simple HTTP listener on port 8081
- Returns "Hello World from MuleSoft!" message
- Logs each request

## Prerequisites

- Java 8 or 11
- Maven 3.3.9 or later
- MuleSoft Runtime 4.4.0 or later (or Anypoint Studio)

## Building the Application

To package the application into a deployable JAR file:

```bash
mvn clean package
```

This will create: `target/mulesoft-hello-world-1.0.0-SNAPSHOT-mule-application.jar`

## Running the Application

### Option 1: Using Anypoint Studio (Recommended for Development)

1. Download and install [Anypoint Studio](https://www.mulesoft.com/platform/studio) if you haven't already
2. Open Anypoint Studio
3. File → Import → Anypoint Studio → Anypoint Studio project from File System
4. Select this project folder
5. Right-click the project → Run As → Mule Application

### Option 2: Using Standalone Mule Runtime

1. Download [Mule Runtime 4.4.0](https://docs.mulesoft.com/mule-runtime/4.4/runtime-installation-task) or later
2. Package the application (see above)
3. Deploy the generated JAR to Mule Runtime:
   ```bash
   cp target/mulesoft-hello-world-1.0.0-SNAPSHOT-mule-application.jar $MULE_HOME/apps/
   $MULE_HOME/bin/mule
   ```

### Option 3: Using CloudHub

Deploy to MuleSoft's cloud platform for production:
```bash
mvn clean deploy -DmuleDeploy
```
(Requires CloudHub credentials configured in settings.xml)

**Important Notes:**
- The `mvn mule:run` command is NOT available in mule-maven-plugin 4.x
- For local development, **Anypoint Studio is the recommended approach**
- Maven is used primarily for packaging and CI/CD pipelines

## Testing the Application

Once the application is running, you can test it using:

### Using curl:
```bash
curl http://localhost:8081/hello
```

### Using a web browser:
Open: http://localhost:8081/hello

### Expected Response:
```
Hello World from MuleSoft!
```

## Endpoints

- **GET** `/hello` - Returns a Hello World message

## Configuration

The application runs on:
- **Host**: 0.0.0.0 (all interfaces)
- **Port**: 8081

You can modify these settings in `src/main/mule/hello-world.xml`

## Dependencies

- Mule HTTP Connector 1.7.3
- Mule Sockets Connector 1.2.3

## Notes

- This is a basic example for learning purposes
- In production, you would typically add error handling, validation, and security
- The application uses Mule Runtime 4.4.0

## Next Steps

To extend this application, you could:
- Add more endpoints
- Integrate with databases
- Connect to external APIs
- Add DataWeave transformations
- Implement error handling
- Add MUnit tests

## License

This is a sample project for educational purposes.

