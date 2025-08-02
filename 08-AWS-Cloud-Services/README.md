# ☁️ AWS & Cloud Services

## 🎯 What Is Cloud Computing?

Think of cloud computing like renting vs buying:
- **Traditional**: Buy a car (own servers)
- **Cloud**: Use Uber (rent servers)

You get:
- No upfront costs
- Pay only for what you use
- Scale up or down instantly
- Someone else maintains it

## 🌍 AWS Overview

AWS (Amazon Web Services) is like a massive tech mall where you can rent:
- Computers (EC2)
- Storage units (S3)
- Databases (RDS, DynamoDB)
- And 200+ other services

### AWS Regions and Availability Zones

```
World
├── Region (e.g., US-East-1)
│   ├── Availability Zone A (data center)
│   ├── Availability Zone B (data center)
│   └── Availability Zone C (data center)
├── Region (e.g., EU-West-1)
│   ├── Availability Zone A
│   └── Availability Zone B
```

## 📦 S3 (Simple Storage Service)

### What Is S3?

S3 = Unlimited Google Drive for developers

Use cases:
- Store images, videos, files
- Host static websites
- Backup data
- Archive old data

### S3 Concepts

```
S3
├── Bucket (like a folder)
│   ├── Object (file)
│   ├── Object
│   └── Folder
│       └── Object
```

### Working with S3

```javascript
const AWS = require('aws-sdk');
const s3 = new AWS.S3();

// Upload file
async function uploadFile(fileName, fileContent) {
  const params = {
    Bucket: 'my-app-bucket',
    Key: `uploads/${fileName}`,
    Body: fileContent,
    ContentType: 'image/jpeg'
  };
  
  const result = await s3.upload(params).promise();
  return result.Location; // URL of uploaded file
}

// Download file
async function downloadFile(fileName) {
  const params = {
    Bucket: 'my-app-bucket',
    Key: `uploads/${fileName}`
  };
  
  const data = await s3.getObject(params).promise();
  return data.Body;
}

// Generate presigned URL (temporary access)
function getSignedUrl(fileName, expiresIn = 3600) {
  const params = {
    Bucket: 'my-app-bucket',
    Key: fileName,
    Expires: expiresIn // Seconds
  };
  
  return s3.getSignedUrl('getObject', params);
}

// Delete file
async function deleteFile(fileName) {
  const params = {
    Bucket: 'my-app-bucket',
    Key: fileName
  };
  
  await s3.deleteObject(params).promise();
}
```

### S3 Bucket Policies

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-public-bucket/*"
    }
  ]
}
```

### S3 Storage Classes

- **Standard**: Frequently accessed data
- **Standard-IA**: Infrequent access, cheaper
- **Glacier**: Archive, very cheap, slow retrieval
- **Intelligent-Tiering**: Automatically moves data

## 🖥️ EC2 (Elastic Compute Cloud)

### What Is EC2?

EC2 = Renting computers in the cloud

Like having a computer that:
- Lives in Amazon's data center
- You can access from anywhere
- Can be turned on/off anytime
- You only pay when it's running

### EC2 Instance Types

```
Instance Types (like computer specs):
├── t2.micro    (1 CPU, 1GB RAM)    - Testing
├── t2.small    (1 CPU, 2GB RAM)    - Small apps
├── t2.medium   (2 CPU, 4GB RAM)    - Medium apps
├── m5.large    (2 CPU, 8GB RAM)    - Production
├── m5.xlarge   (4 CPU, 16GB RAM)   - Heavy load
└── p3.2xlarge  (8 CPU, 61GB, GPU)  - Machine learning
```

### Setting Up EC2

```bash
# 1. Launch instance from AWS Console
# 2. Choose AMI (Amazon Machine Image) - like OS
# 3. Choose instance type
# 4. Configure security group (firewall)
# 5. Create/select key pair for SSH access

# Connect to EC2
ssh -i "my-key.pem" ec2-user@ec2-xx-xx-xx-xx.compute.amazonaws.com

# Install Node.js on EC2
sudo yum update -y
curl -sL https://rpm.nodesource.com/setup_16.x | sudo bash -
sudo yum install nodejs -y

# Clone and run your app
git clone https://github.com/yourname/yourapp.git
cd yourapp
npm install
npm start
```

### EC2 with Load Balancer

```
Internet
    │
    ▼
Load Balancer
    │
    ├──► EC2 Instance 1
    ├──► EC2 Instance 2
    └──► EC2 Instance 3
```

## ⚡ Lambda (Serverless Functions)

### What Is Lambda?

Lambda = Code that runs without servers

Like having a butler who:
- Only works when called
- You pay per task
- No need to manage them
- Can handle millions of tasks

### Lambda Function Example

```javascript
// Lambda function
exports.handler = async (event) => {
  console.log('Event:', event);
  
  // Process the event
  const name = event.name || 'World';
  
  // Return response
  return {
    statusCode: 200,
    body: JSON.stringify({
      message: `Hello ${name}!`
    })
  };
};
```

### Lambda Triggers

```
Lambda can be triggered by:
├── API Gateway (HTTP requests)
├── S3 (file uploads)
├── DynamoDB (database changes)
├── CloudWatch (scheduled tasks)
├── SQS (message queue)
└── Many more...
```

### Serverless Image Resizer

```javascript
const AWS = require('aws-sdk');
const sharp = require('sharp');
const s3 = new AWS.S3();

exports.handler = async (event) => {
  // Get the uploaded image
  const bucket = event.Records[0].s3.bucket.name;
  const key = event.Records[0].s3.object.key;
  
  // Download from S3
  const image = await s3.getObject({
    Bucket: bucket,
    Key: key
  }).promise();
  
  // Resize image
  const resized = await sharp(image.Body)
    .resize(200, 200)
    .toBuffer();
  
  // Upload thumbnail
  await s3.putObject({
    Bucket: bucket,
    Key: `thumbnails/${key}`,
    Body: resized,
    ContentType: 'image/jpeg'
  }).promise();
  
  return 'Thumbnail created';
};
```

## 🔧 Serverless vs Server-full

### Server-full (Traditional)

```
Your responsibility:
├── Server setup
├── OS updates
├── Security patches
├── Scaling
├── Load balancing
├── Monitoring
└── 24/7 running cost
```

**Example**: Running Node.js on EC2
```javascript
// Always running, always costing money
const express = require('express');
const app = express();

app.get('/api/users', (req, res) => {
  res.json({ users: [...] });
});

app.listen(3000); // Running 24/7
```

### Serverless

```
AWS responsibility:
├── All infrastructure
├── Automatic scaling
├── Security patches
├── Only runs when needed
└── Pay per execution
```

**Example**: Same API with Lambda
```javascript
// Only runs when called
exports.handler = async (event) => {
  return {
    statusCode: 200,
    body: JSON.stringify({ users: [...] })
  };
};
```

### When to Use What?

**Use Serverless when:**
- Sporadic traffic
- Event-driven tasks
- Microservices
- Cost optimization
- Quick prototypes

**Use Server-full when:**
- Constant high traffic
- Long-running processes
- WebSocket connections
- Complex applications
- More control needed

## 📧 SES (Simple Email Service)

### Sending Emails

```javascript
const AWS = require('aws-sdk');
const ses = new AWS.SES();

async function sendEmail(to, subject, body) {
  const params = {
    Source: 'noreply@yourapp.com',
    Destination: {
      ToAddresses: [to]
    },
    Message: {
      Subject: {
        Data: subject
      },
      Body: {
        Html: {
          Data: `
            <html>
              <body>
                <h1>${subject}</h1>
                <p>${body}</p>
              </body>
            </html>
          `
        }
      }
    }
  };
  
  await ses.sendEmail(params).promise();
}

// Send welcome email
await sendEmail(
  'user@example.com',
  'Welcome to our app!',
  'Thanks for signing up...'
);
```

### Email Templates

```javascript
// Create template
await ses.createTemplate({
  Template: {
    TemplateName: 'WelcomeEmail',
    SubjectPart: 'Welcome {{name}}!',
    HtmlPart: `
      <h1>Welcome {{name}}!</h1>
      <p>Thanks for joining {{company}}.</p>
      <a href="{{link}}">Get Started</a>
    `
  }
}).promise();

// Send using template
await ses.sendTemplatedEmail({
  Source: 'noreply@yourapp.com',
  Destination: {
    ToAddresses: ['user@example.com']
  },
  Template: 'WelcomeEmail',
  TemplateData: JSON.stringify({
    name: 'John',
    company: 'YourApp',
    link: 'https://yourapp.com/start'
  })
}).promise();
```

## 🌐 Route 53 (DNS Service)

### What Is Route 53?

Route 53 = Phone book for the internet

Converts:
- `yourapp.com` → `54.123.45.67` (IP address)

### DNS Record Types

```
A Record:     yourapp.com → 54.123.45.67
CNAME:        www.yourapp.com → yourapp.com
MX:           Email routing
TXT:          Domain verification
```

### Setting Up Domain

```javascript
// After buying domain
1. Create hosted zone in Route 53
2. Update nameservers at registrar
3. Create records:

A Record:
- Name: yourapp.com
- Value: Your EC2/Load Balancer IP

CNAME Record:
- Name: www
- Value: yourapp.com
```

## 🚀 Amplify (Full-Stack Platform)

### What Is Amplify?

Amplify = All-in-one platform for web/mobile apps

Includes:
- Hosting
- Authentication
- APIs
- Database
- Storage

### Amplify Setup

```bash
# Install CLI
npm install -g @aws-amplify/cli

# Configure
amplify configure

# Initialize project
amplify init

# Add authentication
amplify add auth

# Add API
amplify add api

# Add storage
amplify add storage

# Deploy
amplify push
```

### Using Amplify in React

```javascript
import { Amplify, Auth, API } from 'aws-amplify';
import config from './aws-exports';

Amplify.configure(config);

// Sign up
async function signUp(email, password) {
  try {
    const { user } = await Auth.signUp({
      username: email,
      password,
      attributes: { email }
    });
    return user;
  } catch (error) {
    console.error('Sign up error:', error);
  }
}

// Sign in
async function signIn(email, password) {
  try {
    const user = await Auth.signIn(email, password);
    return user;
  } catch (error) {
    console.error('Sign in error:', error);
  }
}

// API call
async function getPosts() {
  const posts = await API.get('myapi', '/posts');
  return posts;
}
```

## 🔗 API Gateway

### Creating REST APIs

```javascript
// API Gateway + Lambda
exports.handler = async (event) => {
  const method = event.httpMethod;
  const path = event.path;
  
  // Route requests
  if (method === 'GET' && path === '/users') {
    return {
      statusCode: 200,
      headers: {
        'Content-Type': 'application/json',
        'Access-Control-Allow-Origin': '*'
      },
      body: JSON.stringify({
        users: ['John', 'Jane', 'Bob']
      })
    };
  }
  
  if (method === 'POST' && path === '/users') {
    const body = JSON.parse(event.body);
    // Create user...
    return {
      statusCode: 201,
      body: JSON.stringify({
        message: 'User created',
        user: body
      })
    };
  }
  
  return {
    statusCode: 404,
    body: JSON.stringify({
      error: 'Not found'
    })
  };
};
```

## 💾 DynamoDB (NoSQL Database)

### DynamoDB Basics

```javascript
const AWS = require('aws-sdk');
const dynamodb = new AWS.DynamoDB.DocumentClient();

// Create item
async function createUser(user) {
  const params = {
    TableName: 'Users',
    Item: {
      userId: user.id,
      email: user.email,
      name: user.name,
      createdAt: new Date().toISOString()
    }
  };
  
  await dynamodb.put(params).promise();
}

// Get item
async function getUser(userId) {
  const params = {
    TableName: 'Users',
    Key: { userId }
  };
  
  const result = await dynamodb.get(params).promise();
  return result.Item;
}

// Query items
async function getUserPosts(userId) {
  const params = {
    TableName: 'Posts',
    KeyConditionExpression: 'userId = :userId',
    ExpressionAttributeValues: {
      ':userId': userId
    }
  };
  
  const result = await dynamodb.query(params).promise();
  return result.Items;
}

// Update item
async function updateUser(userId, updates) {
  const params = {
    TableName: 'Users',
    Key: { userId },
    UpdateExpression: 'SET #name = :name, updatedAt = :updatedAt',
    ExpressionAttributeNames: {
      '#name': 'name'
    },
    ExpressionAttributeValues: {
      ':name': updates.name,
      ':updatedAt': new Date().toISOString()
    }
  };
  
  await dynamodb.update(params).promise();
}
```

## 🔒 IAM (Identity and Access Management)

### What Is IAM?

IAM = Security guard for AWS

Controls:
- Who can access (Users)
- What they can access (Permissions)
- How they access (Policies)

### IAM Policy Example

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::my-bucket/*"
    },
    {
      "Effect": "Allow",
      "Action": "dynamodb:*",
      "Resource": "arn:aws:dynamodb:*:*:table/Users"
    }
  ]
}
```

## 📊 CloudWatch (Monitoring)

### Monitoring Your Application

```javascript
const AWS = require('aws-sdk');
const cloudwatch = new AWS.CloudWatch();

// Send custom metric
async function trackMetric(metricName, value) {
  const params = {
    Namespace: 'MyApp',
    MetricData: [
      {
        MetricName: metricName,
        Value: value,
        Unit: 'Count',
        Timestamp: new Date()
      }
    ]
  };
  
  await cloudwatch.putMetricData(params).promise();
}

// Track user signups
await trackMetric('UserSignups', 1);

// Track API calls
await trackMetric('APIRequests', 1);
```

### CloudWatch Logs

```javascript
// Lambda automatically logs to CloudWatch
console.log('User signed up:', userId);
console.error('Error processing payment:', error);

// View logs in CloudWatch Console
// Filter, search, and create alarms
```

## 🎯 Common AWS Patterns

### 1. Static Website Hosting

```
Route 53 (domain)
    │
    ▼
CloudFront (CDN)
    │
    ▼
S3 Bucket (static files)
```

### 2. Serverless API

```
Client
    │
    ▼
API Gateway
    │
    ▼
Lambda Function
    │
    ▼
DynamoDB
```

### 3. Full-Stack Application

```
Client (React)
    │
    ├──► CloudFront → S3 (static assets)
    │
    └──► API Gateway
              │
              ├──► Lambda (business logic)
              ├──► RDS (relational data)
              ├──► DynamoDB (NoSQL data)
              └──► S3 (file storage)
```

## 💰 Cost Optimization

### Free Tier Limits

- EC2: 750 hours/month (t2.micro)
- S3: 5GB storage
- Lambda: 1M requests/month
- DynamoDB: 25GB storage
- RDS: 750 hours/month

### Cost Saving Tips

1. **Use Serverless** for variable traffic
2. **Turn off** unused EC2 instances
3. **Set up billing alerts**
4. **Use S3 lifecycle policies**
5. **Choose right instance types**
6. **Use Reserved Instances** for steady workloads

## 🎯 What This Means for BAs

Understanding AWS helps you:

1. **Infrastructure Requirements** - Know what services are needed
2. **Cost Estimation** - Understand pricing models
3. **Scalability Planning** - Design for growth
4. **Integration Points** - See how services connect
5. **Performance Expectations** - Know service limits

## ✅ Key Takeaways

- Cloud = Renting instead of buying
- S3 for file storage
- EC2 for virtual servers
- Lambda for serverless functions
- Pay only for what you use
- Serverless vs Server-full depends on use case
- AWS has 200+ services for every need

Next Module: [AI & Modern Tech →](../09-AI-Modern-Tech/README.md)