# Running MuleSoft Hello World on macOS (Apple Silicon)

## ⚠️ Important Note for Apple Silicon Macs

The standalone Mule Runtime does not support Apple Silicon (M1/M2/M3) architecture when run from the terminal. 

**You must use Anypoint Studio** to run MuleSoft applications on Apple Silicon Macs.

## ✅ How to Run Your Application

### You Have Anypoint Studio Installed!

Location: `/Applications/AnypointStudio.app`

### Steps to Run:

1. **Open Anypoint Studio**:
   ```bash
   open /Applications/AnypointStudio.app
   ```

2. **Import the Project**:
   - In Anypoint Studio, go to: **File → Import**
   - Select: **Anypoint Studio → Anypoint Studio project from File System**
   - Click **Next**
   - Browse to: `/Users/vbolisetti/mulesoft-hello-world`
   - Click **Finish**

3. **Run the Application**:
   - In the **Package Explorer**, right-click on `mulesoft-hello-world`
   - Select: **Run As → Mule Application**
   - Wait for the console to show: `DEPLOYED`

4. **Test the Endpoint**:
   Open a new terminal and run:
   ```bash
   curl http://localhost:8081/hello
   ```
   
   **Expected Response**:
   ```
   Hello World from MuleSoft!
   ```

   Or open in browser: http://localhost:8081/hello

## What the Application Does

**Endpoint**: `GET /hello` on port 8081

**Flow**:
1. HTTP Listener receives request at `/hello`
2. Set Payload returns "Hello World from MuleSoft!"
3. Logger logs the request

## Project Structure

```
/Users/vbolisetti/mulesoft-hello-world/
├── pom.xml                          # ✅ Maven config (builds successfully)
├── mule-artifact.json               # Runtime descriptor
├── src/main/mule/hello-world.xml    # Main flow definition
└── target/*.jar                     # ✅ Deployable JAR (built)
```

## Troubleshooting

### Port 8081 Already in Use?
You have several Mule runtimes already running from previous Anypoint Studio sessions.

**Check what's on port 8081**:
```bash
lsof -i :8081
```

**To use a different port**, edit `src/main/mule/hello-world.xml`:
```xml
<http:listener-connection host="0.0.0.0" port="8082" />
```

### Application Won't Deploy?
- Clean the project: **Project → Clean**
- Rebuild: `mvn clean package`
- Restart Anypoint Studio

## Why Can't I Run from Terminal?

The standalone Mule Runtime uses a Java wrapper that doesn't support Apple Silicon architecture. 

Error you'll see:
```
Your machine's hardware type (uname -m) was not recognized by the Service Wrapper
```

**Solutions**:
1. ✅ **Use Anypoint Studio** (recommended - already installed)
2. Deploy to CloudHub (cloud-based)
3. Use Docker with x86 emulation (not recommended - slow)

## Next Steps

1. Run the application in Anypoint Studio
2. Test the `/hello` endpoint
3. Explore adding more endpoints
4. Learn DataWeave transformations
5. Connect to databases and APIs

## Resources

- **Your Project**: `/Users/vbolisetti/mulesoft-hello-world`
- **Anypoint Studio**: `/Applications/AnypointStudio.app`
- **Documentation**: https://docs.mulesoft.com
- **DataWeave**: https://docs.mulesoft.com/dataweave

---

**Ready to code!** 🚀 Open Anypoint Studio and import your project!

