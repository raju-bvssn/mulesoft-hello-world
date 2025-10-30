# Use Cases & Integration Patterns

Practical examples and integration patterns for the Hello World API.

---

## Table of Contents

1. [Microservices Health Monitoring](#1-microservices-health-monitoring)
2. [Multi-Language User Welcome](#2-multi-language-user-welcome)
3. [API Gateway Integration](#3-api-gateway-integration)
4. [Event-Driven Greetings](#4-event-driven-greetings)
5. [Chatbot Integration](#5-chatbot-integration)
6. [Mobile App Welcome Screen](#6-mobile-app-welcome-screen)
7. [Email Personalization](#7-email-personalization)
8. [Learning & Training](#8-learning--training)

---

## 1. Microservices Health Monitoring

Monitor the API's availability and integrate with your observability stack.

### Scenario

You have a microservices architecture and need to monitor all services for availability.

### Implementation

**Kubernetes Liveness Probe**:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-world-consumer
spec:
  containers:
  - name: app
    image: my-app:latest
    livenessProbe:
      httpGet:
        path: /api/health
        port: 8081
      initialDelaySeconds: 30
      periodSeconds: 10
```

**Prometheus Monitoring**:
```yaml
scrape_configs:
  - job_name: 'hello-world-api'
    metrics_path: '/api/health'
    static_configs:
      - targets: ['mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io']
```

**Shell Script Health Check**:
```bash
#!/bin/bash
HEALTH_URL="https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/health"

response=$(curl -s -o /dev/null -w "%{http_code}" "$HEALTH_URL")

if [ "$response" -eq 200 ]; then
    echo "✅ API is healthy"
    exit 0
else
    echo "❌ API is down (HTTP $response)"
    exit 1
fi
```

---

## 2. Multi-Language User Welcome

Personalize user experiences based on their language preferences.

### Scenario

Your application serves users globally and needs to greet them in their preferred language.

### Implementation

**React Component**:
```javascript
import React, { useState, useEffect } from 'react';

function WelcomeComponent({ userName, userLanguage }) {
  const [greeting, setGreeting] = useState('');

  useEffect(() => {
    const fetchGreeting = async () => {
      const response = await fetch(
        'https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/greet',
        {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            name: userName,
            language: userLanguage || 'en'
          })
        }
      );
      const data = await response.json();
      setGreeting(data.message);
    };

    fetchGreeting();
  }, [userName, userLanguage]);

  return <h1>{greeting}</h1>;
}

export default WelcomeComponent;
```

**Usage**:
```javascript
<WelcomeComponent userName="Maria" userLanguage="es" />
// Displays: "Hola, Maria!"

<WelcomeComponent userName="Pierre" userLanguage="fr" />
// Displays: "Bonjour, Pierre!"
```

---

## 3. API Gateway Integration

Use as a backend service behind an API Gateway.

### Scenario

Aggregate multiple APIs behind a central gateway with authentication and rate limiting.

### Implementation

**MuleSoft API Manager Policy**:
```yaml
policies:
  - policyName: client-id-enforcement
    configurationData:
      credentialsOriginHasHttpBasicAuthenticationHeader: true
  - policyName: rate-limiting
    configurationData:
      rateLimits:
        - maximumRequests: 1000
          timePeriodInMilliseconds: 60000
```

**Kong Gateway Configuration**:
```bash
# Add service
curl -X POST http://localhost:8001/services \
  --data name=hello-world \
  --data url='https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io'

# Add route
curl -X POST http://localhost:8001/services/hello-world/routes \
  --data 'paths[]=/hello' \
  --data name=hello-route

# Add rate limiting
curl -X POST http://localhost:8001/services/hello-world/plugins \
  --data "name=rate-limiting" \
  --data "config.minute=100"
```

---

## 4. Event-Driven Greetings

Trigger personalized greetings based on events.

### Scenario

Send welcome messages when users sign up, log in, or complete actions.

### Implementation

**Mule Flow with Event Listener**:
```xml
<flow name="user-signup-greeting-flow">
    <jms:listener config-ref="JMS_Config" destination="user.signup.queue"/>
    
    <set-variable variableName="userInfo" 
                  value="#[payload]"/>
    
    <http:request method="POST" 
                  url="https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/greet">
        <http:body><![CDATA[#[{
            name: vars.userInfo.firstName,
            language: vars.userInfo.preferredLanguage
        }]]]></http:body>
    </http:request>
    
    <logger message="#['Greeting sent: ' ++ payload.message]"/>
    
    <email:send config-ref="Email_Config">
        <email:to>#[vars.userInfo.email]</email:to>
        <email:subject>Welcome!</email:subject>
        <email:body>#[payload.message]</email:body>
    </email:send>
</flow>
```

**AWS Lambda Trigger**:
```python
import json
import requests

def lambda_handler(event, context):
    # Event triggered on user signup
    user_data = json.loads(event['body'])
    
    # Call greeting API
    response = requests.post(
        'https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/greet',
        json={
            'name': user_data['firstName'],
            'language': user_data.get('language', 'en')
        }
    )
    
    greeting = response.json()['message']
    
    # Send to notification service
    send_notification(user_data['userId'], greeting)
    
    return {
        'statusCode': 200,
        'body': json.dumps({'message': 'Greeting sent'})
    }
```

---

## 5. Chatbot Integration

Integrate greetings into chatbot conversations.

### Scenario

A chatbot needs to greet users naturally in multiple languages.

### Implementation

**Slack Bot**:
```javascript
const { WebClient } = require('@slack/web-api');
const axios = require('axios');

const slack = new WebClient(process.env.SLACK_TOKEN);

async function greetUser(userId, userName, language = 'en') {
  // Get personalized greeting from API
  const response = await axios.post(
    'https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/greet',
    {
      name: userName,
      language: language
    }
  );
  
  const greeting = response.data.message;
  
  // Send to Slack
  await slack.chat.postMessage({
    channel: userId,
    text: greeting
  });
}

// Usage
greetUser('U12345', 'Maria', 'es');
```

**Microsoft Teams Bot**:
```csharp
using System.Net.Http;
using System.Text;
using Newtonsoft.Json;

public async Task SendGreeting(string userName, string language)
{
    var client = new HttpClient();
    var payload = new
    {
        name = userName,
        language = language
    };
    
    var content = new StringContent(
        JsonConvert.SerializeObject(payload),
        Encoding.UTF8,
        "application/json"
    );
    
    var response = await client.PostAsync(
        "https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/greet",
        content
    );
    
    var result = await response.Content.ReadAsStringAsync();
    var greeting = JsonConvert.DeserializeObject<dynamic>(result);
    
    // Send to Teams channel
    await SendTeamsMessage(greeting.message);
}
```

---

## 6. Mobile App Welcome Screen

Display personalized greetings in mobile applications.

### Scenario

Mobile app shows welcome message on launch or after login.

### Implementation

**Flutter (Dart)**:
```dart
import 'package:http/http.dart' as http;
import 'dart:convert';

class GreetingService {
  static const String baseUrl = 
    'https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io';
  
  Future<String> getPersonalizedGreeting(
    String userName, 
    String language
  ) async {
    final response = await http.post(
      Uri.parse('$baseUrl/api/greet'),
      headers: {'Content-Type': 'application/json'},
      body: jsonEncode({
        'name': userName,
        'language': language,
      }),
    );
    
    if (response.statusCode == 200) {
      final data = jsonDecode(response.body);
      return data['message'];
    } else {
      throw Exception('Failed to load greeting');
    }
  }
}

// Usage in Widget
class WelcomeScreen extends StatefulWidget {
  @override
  _WelcomeScreenState createState() => _WelcomeScreenState();
}

class _WelcomeScreenState extends State<WelcomeScreen> {
  String _greeting = '';
  
  @override
  void initState() {
    super.initState();
    _loadGreeting();
  }
  
  Future<void> _loadGreeting() async {
    final service = GreetingService();
    final greeting = await service.getPersonalizedGreeting(
      'User',
      Localizations.localeOf(context).languageCode
    );
    setState(() {
      _greeting = greeting;
    });
  }
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text(
          _greeting,
          style: Theme.of(context).textTheme.headline4,
        ),
      ),
    );
  }
}
```

**React Native**:
```javascript
import React, { useState, useEffect } from 'react';
import { View, Text, StyleSheet } from 'react-native';
import AsyncStorage from '@react-native-async-storage/async-storage';

const WelcomeScreen = () => {
  const [greeting, setGreeting] = useState('');

  useEffect(() => {
    const fetchGreeting = async () => {
      const userName = await AsyncStorage.getItem('userName');
      const language = await AsyncStorage.getItem('language') || 'en';
      
      const response = await fetch(
        'https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/greet',
        {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            name: userName,
            language: language
          })
        }
      );
      
      const data = await response.json();
      setGreeting(data.message);
    };

    fetchGreeting();
  }, []);

  return (
    <View style={styles.container}>
      <Text style={styles.greeting}>{greeting}</Text>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  greeting: {
    fontSize: 24,
    fontWeight: 'bold',
  },
});

export default WelcomeScreen;
```

---

## 7. Email Personalization

Generate personalized email greetings.

### Scenario

Marketing emails need personalized greetings in customer's language.

### Implementation

**Email Template Service**:
```python
import requests
from jinja2 import Template

def generate_email(recipient_name, recipient_email, language='en'):
    # Fetch personalized greeting
    response = requests.post(
        'https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/greet',
        json={
            'name': recipient_name,
            'language': language
        }
    )
    
    greeting_data = response.json()
    
    # Email template
    template = Template('''
    <html>
    <body>
        <h1>{{ greeting }}</h1>
        <p>Thank you for being a valued customer!</p>
        <p>We hope you enjoy our services.</p>
    </body>
    </html>
    ''')
    
    html_content = template.render(greeting=greeting_data['message'])
    
    return {
        'to': recipient_email,
        'subject': f'Welcome, {recipient_name}!',
        'html': html_content
    }

# Batch processing
def send_welcome_emails(recipients):
    for recipient in recipients:
        email = generate_email(
            recipient['name'],
            recipient['email'],
            recipient.get('language', 'en')
        )
        send_email(email)  # Your email sending function
```

---

## 8. Learning & Training

Perfect for teaching API development concepts.

### Scenario

Training new developers on REST APIs, MuleSoft, and API-first development.

### Learning Objectives

1. **REST API Basics**
   - HTTP methods (GET, POST)
   - Request/response structure
   - Status codes
   - Headers

2. **API-First Design**
   - RAML specifications
   - Design before implementation
   - Contract-driven development

3. **MuleSoft Development**
   - APIkit scaffolding
   - DataWeave transformations
   - CloudHub deployment

4. **Integration Patterns**
   - HTTP clients
   - Error handling
   - Testing strategies

### Training Exercises

**Exercise 1: Make Your First API Call**
```bash
curl https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/hello
```

**Exercise 2: Add Query Parameters (Advanced)**
```bash
# Challenge: Modify the API to accept query parameters
curl "https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/hello?name=Student"
```

**Exercise 3: POST Request**
```bash
curl -X POST https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/greet \
  -H "Content-Type: application/json" \
  -d '{"name":"Student","language":"fr"}'
```

**Exercise 4: Error Handling**
```bash
# Send invalid request to see error response
curl -X POST https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/greet \
  -H "Content-Type: application/json" \
  -d '{}'
```

---

## Integration Best Practices

### 1. Error Handling
Always handle API errors gracefully:
```javascript
try {
  const response = await fetch(API_URL);
  if (!response.ok) {
    throw new Error(`HTTP ${response.status}`);
  }
  const data = await response.json();
  return data;
} catch (error) {
  console.error('API Error:', error);
  return { message: 'Default greeting' };
}
```

### 2. Retry Logic
Implement exponential backoff for resilience:
```python
import time
import requests

def call_api_with_retry(url, max_retries=3):
    for attempt in range(max_retries):
        try:
            response = requests.get(url, timeout=5)
            response.raise_for_status()
            return response.json()
        except requests.RequestException as e:
            if attempt == max_retries - 1:
                raise
            time.sleep(2 ** attempt)  # Exponential backoff
```

### 3. Caching
Cache responses to reduce API calls:
```javascript
const cache = new Map();
const CACHE_TTL = 60000; // 1 minute

async function getCachedGreeting(name) {
  const cacheKey = `greeting_${name}`;
  const cached = cache.get(cacheKey);
  
  if (cached && Date.now() - cached.timestamp < CACHE_TTL) {
    return cached.data;
  }
  
  const response = await fetch(`${API_URL}/api/hello/${name}`);
  const data = await response.json();
  
  cache.set(cacheKey, {
    data: data,
    timestamp: Date.now()
  });
  
  return data;
}
```

### 4. Monitoring
Track API usage and performance:
```python
import time
from functools import wraps

def monitor_api_call(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        try:
            result = func(*args, **kwargs)
            duration = time.time() - start
            log_metric('api_call_success', duration)
            return result
        except Exception as e:
            duration = time.time() - start
            log_metric('api_call_failure', duration, error=str(e))
            raise
    return wrapper

@monitor_api_call
def call_greeting_api(name):
    # API call implementation
    pass
```

---

## Support & Resources

- **GitHub Repository**: [mulesoft-hello-world](https://github.com/raju-bvssn/mulesoft-hello-world)
- **API Console**: [Interactive Testing](https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/console)
- **Getting Started**: See Getting Started guide
- **API Reference**: Complete endpoint documentation

---

**Have a unique use case? Share it with us!** 🚀

