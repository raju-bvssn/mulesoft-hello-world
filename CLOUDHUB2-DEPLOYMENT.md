# CloudHub 2.0 Deployment Guide

This guide explains how to deploy the MuleSoft Hello World application to CloudHub 2.0 dev environment.

## Prerequisites

1. **Anypoint Platform Account** with CloudHub 2.0 access
2. **Connected App** or **Username/Password** credentials
3. **Maven** installed and configured
4. **Business Group ID** from your Anypoint Platform organization

## Configuration

### 1. Update Business Group ID

Edit `pom.xml` and replace `your-business-group` with your actual Business Group ID:

```xml
<cloudhub2.businessGroup>YOUR_BUSINESS_GROUP_ID</cloudhub2.businessGroup>
```

To find your Business Group ID:
- Go to https://anypoint.mulesoft.com
- Access Management → Organization
- Copy the Organization ID or Business Group ID

### 2. Configure Authentication

#### Option A: Using Connected App (Recommended for CI/CD)

Create or update `~/.m2/settings.xml`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 
          http://maven.apache.org/xsd/settings-1.0.0.xsd">
    <servers>
        <server>
            <id>anypoint-exchange</id>
            <username>~~~Client~~~</username>
            <password>CLIENT_ID~?~CLIENT_SECRET</password>
        </server>
    </servers>
</settings>
```

To create a Connected App:
1. Go to Anypoint Platform → Access Management → Connected Apps
2. Create new app with these scopes:
   - Design Center Developer
   - CloudHub Organization Admin
   - Runtime Manager (Deploy, Read Applications)
3. Copy the Client ID and Client Secret
4. Replace in settings.xml: `CLIENT_ID~?~CLIENT_SECRET`

#### Option B: Using Username/Password

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 
          http://maven.apache.org/xsd/settings-1.0.0.xsd">
    <servers>
        <server>
            <id>anypoint-exchange</id>
            <username>YOUR_ANYPOINT_USERNAME</username>
            <password>YOUR_ANYPOINT_PASSWORD</password>
        </server>
    </servers>
</settings>
```

## Deployment Commands

### Deploy to CloudHub 2.0 Dev Environment

From the dev branch:

```bash
# Switch to dev branch (if not already on it)
git checkout dev

# Clean and package
mvn clean package

# Deploy to CloudHub 2.0
mvn deploy -DmuleDeploy
```

### Check Deployment Status

```bash
# Using Anypoint CLI (if installed)
anypoint-cli runtime-mgr cloudhub-application describe mulesoft-hello-world-dev

# Or check in Anypoint Platform UI
# Go to Runtime Manager → Applications
```

## Application Configuration

The dev environment deployment is configured with:

- **Application Name**: `mulesoft-hello-world-dev`
- **Environment**: `dev`
- **Region**: `us-east-1` (Cloudhub-US-East-1)
- **Runtime**: Mule 4.4.0
- **vCores**: 0.1 (smallest size)
- **Replicas**: 1
- **Deployment Timeout**: 10 minutes

### Customizing Configuration

You can override these properties at deployment time:

```bash
mvn deploy -DmuleDeploy \
  -Dcloudhub2.environment=dev \
  -Dcloudhub2.replicas=1 \
  -Dcloudhub2.vCores=0.1 \
  -Dcloudhub2.region=us-east-1
```

## Testing the Deployed Application

Once deployed, your application will be available at:

```
https://mulesoft-hello-world-dev.{region}.cloudhub.io/hello
```

For us-east-1:
```bash
curl https://mulesoft-hello-world-dev.us-e1.cloudhub.io/hello
```

Expected response:
```
Hello World from MuleSoft!
```

## Available Regions

CloudHub 2.0 supports multiple regions:

- `Cloudhub-US-East-1` (North Virginia)
- `Cloudhub-US-West-2` (Oregon)
- `Cloudhub-EU-Central-1` (Frankfurt)
- `Cloudhub-AP-Southeast-1` (Singapore)
- `Cloudhub-AP-Southeast-2` (Sydney)

Update the `<target>` in `pom.xml` to use a different region.

## Troubleshooting

### Authentication Failed

```
[ERROR] Failed to execute goal org.mule.tools.maven:mule-maven-plugin:4.0.0:deploy: 
Authentication failed
```

**Solution**: Check your credentials in `~/.m2/settings.xml`

### Business Group Not Found

```
[ERROR] Business group not found
```

**Solution**: Verify your Business Group ID is correct in `pom.xml`

### Insufficient Permissions

```
[ERROR] User does not have permission to deploy
```

**Solution**: Ensure your user/Connected App has:
- Runtime Manager Deploy permission
- Access to the dev environment
- CloudHub Organization Admin role

### Application Already Exists

If the application already exists with the same name, the deployment will update it. To force a new deployment:

```bash
# Delete existing application first (via UI or CLI)
anypoint-cli runtime-mgr cloudhub-application delete mulesoft-hello-world-dev

# Then deploy
mvn deploy -DmuleDeploy
```

## Deployment Logs

View deployment logs in:
1. **Maven Console**: Shows deployment progress
2. **Runtime Manager UI**: 
   - Go to https://anypoint.mulesoft.com
   - Runtime Manager → Applications → mulesoft-hello-world-dev
   - Click "Logs"

## CI/CD Integration

### GitHub Actions Example

```yaml
name: Deploy to CloudHub 2.0 Dev

on:
  push:
    branches: [ dev ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Set up JDK 8
        uses: actions/setup-java@v3
        with:
          java-version: '8'
          distribution: 'adopt'
      
      - name: Deploy to CloudHub 2.0
        run: mvn deploy -DmuleDeploy
        env:
          ANYPOINT_CLIENT_ID: ${{ secrets.ANYPOINT_CLIENT_ID }}
          ANYPOINT_CLIENT_SECRET: ${{ secrets.ANYPOINT_CLIENT_SECRET }}
```

## Next Steps

1. Configure your credentials in `~/.m2/settings.xml`
2. Update Business Group ID in `pom.xml`
3. Run deployment: `mvn deploy -DmuleDeploy`
4. Monitor deployment in Runtime Manager
5. Test the deployed application

## Resources

- [CloudHub 2.0 Documentation](https://docs.mulesoft.com/cloudhub-2/)
- [Mule Maven Plugin Guide](https://docs.mulesoft.com/mule-runtime/latest/deploy-to-cloudhub-2)
- [Connected Apps Setup](https://docs.mulesoft.com/access-management/connected-apps-overview)
- [Runtime Manager](https://anypoint.mulesoft.com/runtime-manager)

