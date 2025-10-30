# 🚀 MuleSoft Deployment Commands Guide

## ⚠️ IMPORTANT: Understanding Maven Commands

There are **different Maven commands** for different purposes. Using the wrong one causes errors!

---

## 📦 **Maven Command Types**

### ❌ **WRONG Command: `mvn deploy`**
```bash
mvn deploy  # DON'T USE THIS FOR CLOUDHUB!
```

**What it does:**
- Publishes artifacts to Maven repository (Anypoint Exchange)
- Used for sharing libraries/APIs with other projects
- **NOT for deploying applications to CloudHub**

**Error you'll get:**
```
status code: 412, reason phrase: Precondition Failed (412)
```

---

### ✅ **CORRECT Commands**

#### **For CloudHub 2.0 Deployment:**
```bash
mvn clean package mule:deploy -DmuleDeploy
```

**What it does:**
- Builds your application
- Deploys to CloudHub 2.0 using configuration in `<cloudhub2Deployment>`
- Uses your pom.xml CloudHub settings

#### **For Building Only:**
```bash
mvn clean package
```

**What it does:**
- Just builds the JAR file
- Creates: `target/mulesoft-hello-world-1.0.0-SNAPSHOT-mule-application.jar`
- Use this before manual upload via Anypoint Platform UI

---

## 🎯 **How to Deploy Your Application**

### **Method 1: Deploy from Anypoint Studio (EASIEST)** ⭐

1. **In Studio:**
   - Right-click project → **Anypoint Platform** → **Deploy to CloudHub**

2. **Configure:**
   - Application Name: `mulesoft-hello-world-dev`
   - Environment: **Sandbox**
   - Target: **Cloudhub-US-East-2**
   - Runtime: **4.9.10**

3. **Click Deploy**

**This is the recommended method!**

---

### **Method 2: Deploy via Maven Command Line**

```bash
cd /Users/vbolisetti/mulesoft-hello-world
mvn clean package mule:deploy -DmuleDeploy
```

**Prerequisites:**
- Credentials configured in `~/.m2/settings.xml` (already done ✅)
- CloudHub settings in `pom.xml` (already configured ✅)

---

### **Method 3: Build + Upload via Anypoint Platform UI**

```bash
# Step 1: Build the JAR
mvn clean package

# Step 2: Manual upload
# Go to https://anypoint.mulesoft.com
# Runtime Manager → Deploy Application
# Upload: target/mulesoft-hello-world-1.0.0-SNAPSHOT-mule-application.jar
```

---

## 🔧 **POM.xml Configuration Explained**

### **Version: MUST be SNAPSHOT for Development**
```xml
<version>1.0.0-SNAPSHOT</version>  ✅ Correct
<version>1.0.0</version>            ❌ Wrong for dev
```

**Why?**
- `-SNAPSHOT` = Development version (can be redeployed)
- Without it = Release version (immutable, can't redeploy)

### **Classifier: MUST be mule-application**
```xml
<classifier>mule-application</classifier>        ✅ Correct
<classifier>---- select project type ----</classifier>  ❌ Wrong
```

**Why?**
- Tells Maven what type of artifact to build
- Must match the packaging type

### **Plugins Configuration**

```xml
<!-- Disable Exchange publication -->
<plugin>
    <groupId>org.mule.tools.maven</groupId>
    <artifactId>exchange-mule-maven-plugin</artifactId>
    <executions>
        <execution>
            <id>default-exchange-pre-deploy</id>
            <phase>none</phase>  ✅ Skips Exchange publishing
        </execution>
    </executions>
</plugin>

<!-- Disable Maven deploy -->
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-deploy-plugin</artifactId>
    <configuration>
        <skip>true</skip>  ✅ Prevents publishing to Exchange
    </configuration>
</plugin>

<!-- CloudHub deployment -->
<plugin>
    <groupId>org.mule.tools.maven</groupId>
    <artifactId>mule-maven-plugin</artifactId>
    <configuration>
        <cloudhub2Deployment>
            <!-- Your CloudHub config here -->
        </cloudhub2Deployment>
    </configuration>
</plugin>
```

---

## 📝 **Common Errors & Solutions**

### Error 1: "Precondition Failed (412)"
```
Failed to deploy artifacts: status code: 412
```

**Cause:** Using `mvn deploy` instead of `mvn mule:deploy`

**Solution:** Use correct command:
```bash
mvn clean package mule:deploy -DmuleDeploy
```

---

### Error 2: "Exchange publication failed"
```
Exchange publication failed: Unexpected error
```

**Cause:** Exchange plugin trying to publish application

**Solution:** Already fixed! ✅
- Exchange plugin is disabled in your pom.xml
- Maven deploy plugin is skipped

---

### Error 3: "Unknown packaging: mule-application"
```
Unknown packaging: mule-application
```

**Cause:** Mule Maven plugin not loaded properly

**Solution:** Use Anypoint Studio to deploy instead

---

## 🎯 **Quick Reference**

| Task | Command | Where |
|------|---------|-------|
| **Deploy to CloudHub** | Use Studio UI | Anypoint Studio ⭐ |
| **Deploy via CLI** | `mvn clean package mule:deploy -DmuleDeploy` | Terminal |
| **Just build JAR** | `mvn clean package` | Terminal |
| **Test locally** | Run in Studio | Anypoint Studio |
| **Publish API to Exchange** | `anypoint-cli exchange asset upload ...` | Terminal |

---

## ✅ **Current Configuration Status**

Your project is now properly configured:

- ✅ Version: `1.0.0-SNAPSHOT` (correct)
- ✅ Classifier: `mule-application` (correct)
- ✅ Exchange plugin: Disabled
- ✅ Maven deploy: Skipped
- ✅ CloudHub 2.0 config: Ready
- ✅ API in Exchange: Published
- ✅ APIkit: Scaffolded

**You're ready to deploy!**

---

## 🚀 **Deploy NOW - Step by Step**

### **If using Anypoint Studio:**

1. Open `/Applications/AnypointStudio.app`
2. Right-click `mulesoft-hello-world` project
3. **Anypoint Platform** → **Deploy to CloudHub**
4. Fill in:
   - Name: `mulesoft-hello-world-dev`
   - Environment: `Sandbox`
   - Target: `Cloudhub-US-East-2`
   - Runtime: `4.9.10`
5. Click **Deploy**

### **If using Command Line:**

```bash
cd /Users/vbolisetti/mulesoft-hello-world
mvn clean package mule:deploy -DmuleDeploy
```

---

## 📊 **After Deployment**

Test your API at:
```
https://mulesoft-hello-world-dev.us-e2.cloudhub.io/api/v1/hello
https://mulesoft-hello-world-dev.us-e2.cloudhub.io/console
```

---

## 💡 **Remember**

1. **DON'T use:** `mvn deploy` (publishes to Exchange)
2. **DO use:** Anypoint Studio UI or `mvn mule:deploy -DmuleDeploy`
3. **Keep version:** `1.0.0-SNAPSHOT` during development
4. **Keep classifier:** `mule-application`

---

**Your application is ready to deploy! Choose Method 1 (Anypoint Studio) for the easiest experience.** 🎉

