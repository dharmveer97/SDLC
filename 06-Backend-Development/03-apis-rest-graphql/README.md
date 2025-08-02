# 🔌 APIs & REST/GraphQL - BA Guide

## 🎯 What Are APIs?

APIs (Application Programming Interfaces) are like **digital contracts** that allow different software systems to communicate with each other. They define how applications can request and exchange data.

Think of them as:

- **Restaurant menu** - Shows what's available and how to order
- **Universal translator** - Enables different systems to understand each other
- **Business agreement** - Defines terms for data exchange
- **Digital handshake** - Establishes communication protocols

## 🌐 Real-World Business Analogy

**Traditional Business Partnership:**

- **Written contract** defines terms
- **Specific procedures** for requests
- **Standard format** for communication
- **Expected responses** to requests

**API Partnership:**

- **API documentation** defines capabilities
- **Specific endpoints** for different requests
- **Standard data format** (JSON/XML)
- **Predictable responses** with status codes

## 🎯 What This Means for Business Analysts

### 1. **System Integration**

```markdown
Without APIs:

- Manual data entry between systems
- Data inconsistencies across platforms
- Time-consuming synchronization
- Limited automation capabilities

With APIs:

- Automatic data synchronization
- Real-time information sharing
- Seamless system integration
- Powerful automation workflows
```

### 2. **Business Agility**

APIs enable rapid business capabilities:

- **Payment processing** - Integrate Stripe, PayPal in days
- **Shipping** - Connect FedEx, UPS tracking automatically
- **Communication** - Add SMS, email notifications
- **Analytics** - Pull data from Google Analytics, social media

### 3. **Platform Strategy**

APIs as business assets:

- **Partner integrations** - Allow others to connect to your systems
- **Mobile apps** - Same backend serves web and mobile
- **Third-party developers** - Expand your platform's capabilities
- **Data monetization** - Sell access to your data/services

## 🔄 REST APIs (Representational State Transfer)

### REST Principles for Business

```markdown
REST = Simple, Predictable Web APIs

Business Benefits:

- Easy to understand and implement
- Works with existing web infrastructure
- Cacheable for better performance
- Stateless (each request independent)
- Widely supported by tools and services
```

### REST API Example

```javascript
// Customer Management REST API
app.get('/api/customers', async (req, res) => {
  // GET = Retrieve data
  const customers = await getCustomers();
  res.json({
    success: true,
    data: customers,
    total: customers.length,
  });
});

app.post('/api/customers', async (req, res) => {
  // POST = Create new resource
  const newCustomer = await createCustomer(req.body);
  res.status(201).json({
    success: true,
    data: newCustomer,
    message: 'Customer created successfully',
  });
});

app.put('/api/customers/:id', async (req, res) => {
  // PUT = Update existing resource
  const updatedCustomer = await updateCustomer(req.params.id, req.body);
  res.json({
    success: true,
    data: updatedCustomer,
    message: 'Customer updated successfully',
  });
});

app.delete('/api/customers/:id', async (req, res) => {
  // DELETE = Remove resource
  await deleteCustomer(req.params.id);
  res.status(204).send(); // No content
});
```

### REST Business Operations

```markdown
Common Business Patterns:

GET /api/orders → List all orders
GET /api/orders/123 → Get specific order
POST /api/orders → Create new order
PUT /api/orders/123 → Update entire order
PATCH /api/orders/123 → Update part of order
DELETE /api/orders/123 → Cancel/delete order

GET /api/customers/456/orders → Get orders for specific customer
POST /api/orders/123/ship → Ship an order (action)
GET /api/reports/sales → Get sales reports
```

## 🔗 GraphQL (Modern API Alternative)

### GraphQL Benefits for Business

```markdown
GraphQL = Flexible Data Querying

Business Advantages:

- Request exactly the data you need
- Single endpoint for all operations
- Faster mobile apps (less data transfer)
- Easier frontend development
- Real-time subscriptions built-in
```

### GraphQL Example

```graphql
# Single request gets all needed data
query CustomerDashboard($customerId: ID!) {
  customer(id: $customerId) {
    name
    email
    orders(last: 5) {
      id
      total
      status
      items {
        name
        quantity
        price
      }
    }
    preferences {
      newsletter
      notifications
    }
  }
}
```

Equivalent REST would require multiple requests:

```markdown
GET /api/customers/123
GET /api/customers/123/orders?limit=5
GET /api/customers/123/preferences
```

### When to Choose REST vs GraphQL

```markdown
Choose REST when:

- Simple CRUD operations
- Team familiar with REST
- Caching is important
- Third-party integrations needed

Choose GraphQL when:

- Complex data relationships
- Mobile app performance critical
- Multiple frontend applications
- Real-time features needed
```

## 📊 API Business Patterns

### Pagination for Large Datasets

```javascript
// REST pagination
app.get('/api/orders', async (req, res) => {
  const page = parseInt(req.query.page) || 1;
  const limit = parseInt(req.query.limit) || 20;
  const offset = (page - 1) * limit;

  const orders = await getOrders({ limit, offset });
  const total = await countOrders();

  res.json({
    data: orders,
    pagination: {
      page,
      limit,
      total,
      pages: Math.ceil(total / limit),
      hasNext: page * limit < total,
      hasPrev: page > 1,
    },
  });
});
```

### API Versioning Strategy

```javascript
// Version in URL path
app.use('/api/v1', v1Routes);
app.use('/api/v2', v2Routes);

// Version in headers
app.use((req, res, next) => {
  const version = req.headers['api-version'] || 'v1';
  req.apiVersion = version;
  next();
});
```

### Error Handling Standards

```javascript
// Consistent error responses
app.use((error, req, res, next) => {
  const errorResponse = {
    success: false,
    error: {
      code: error.code || 'INTERNAL_ERROR',
      message: error.message || 'An unexpected error occurred',
      details: error.details || null,
    },
    timestamp: new Date().toISOString(),
    path: req.path,
  };

  // Log error for debugging
  console.error('API Error:', errorResponse);

  // Send appropriate status code
  const statusCode = error.statusCode || 500;
  res.status(statusCode).json(errorResponse);
});
```

## 📋 Common BA Questions & Answers

**Q: How long does it take to build an API?**
A: Basic CRUD API: 2-3 weeks. Complex business logic API: 6-12 weeks.

**Q: Can APIs be changed after they're built?**
A: Yes, but breaking changes require versioning strategy to avoid disrupting existing users.

**Q: How do we secure our APIs?**
A: Authentication (API keys, JWT), authorization (role-based access), rate limiting, HTTPS.

**Q: What if external APIs we depend on go down?**
A: Implement fallback strategies, caching, and error handling for external dependencies.

**Q: How do we test APIs?**
A: Automated testing with tools like Postman, unit tests, integration tests, load testing.

## 🎯 What BAs Should Include in Requirements

### API Functional Requirements

```markdown
✅ Good Requirements:

- "Customer API must return customer data with orders, preferences, and payment methods"
- "Order API must validate inventory before accepting orders"
- "Search API must support filtering by price range, category, and availability"
- "Payment API must handle failed transactions gracefully with retry logic"
- "All APIs must return responses within 200ms for 95% of requests"

❌ Vague Requirements:

- "Create customer API"
- "Handle payments"
- "Search functionality"
```

### API Security Requirements

```markdown
Include in Requirements:

- Authentication method (API keys, OAuth, JWT)
- Rate limiting (requests per minute/hour)
- Data validation rules
- Access control (who can access what)
- Encryption requirements (HTTPS, data encryption)
```

## 🚦 API Development Process

### API Design First Approach

```markdown
1. Document API endpoints before coding
2. Define request/response formats
3. Specify error handling
4. Review with stakeholders
5. Build according to specification
6. Test against documentation

Benefits:

- Clearer requirements
- Faster development
- Better team communication
- Easier testing and validation
```

### API Documentation Example

```yaml
# OpenAPI specification example
/api/customers/{id}:
  get:
    summary: Get customer details
    parameters:
      - name: id
        in: path
        required: true
        schema:
          type: string
    responses:
      200:
        description: Customer found
        content:
          application/json:
            schema:
              type: object
              properties:
                id:
                  type: string
                name:
                  type: string
                email:
                  type: string
      404:
        description: Customer not found
```

## 📈 Measuring API Success

### Technical Metrics

- **Response time** (average, 95th percentile)
- **Error rate** (percentage of failed requests)
- **Throughput** (requests per second)
- **Availability** (uptime percentage)

### Business Metrics

- **API adoption** (number of consumers)
- **Integration success rate** (successful implementations)
- **Developer satisfaction** (ease of use surveys)
- **Business value generated** (revenue from API-enabled features)

### API Analytics

```javascript
// Basic API analytics middleware
const analytics = {
  requests: new Map(),
  errors: new Map(),
};

app.use((req, res, next) => {
  const start = Date.now();
  const endpoint = `${req.method} ${req.path}`;

  // Count requests
  analytics.requests.set(endpoint, (analytics.requests.get(endpoint) || 0) + 1);

  res.on('finish', () => {
    const duration = Date.now() - start;

    // Track errors
    if (res.statusCode >= 400) {
      analytics.errors.set(endpoint, (analytics.errors.get(endpoint) || 0) + 1);
    }

    // Log performance
    console.log(`${endpoint} - ${res.statusCode} - ${duration}ms`);
  });

  next();
});
```

## 🌟 Advanced API Concepts

### Microservices APIs

```javascript
// Service mesh communication
const userService = 'http://user-service:3001';
const orderService = 'http://order-service:3002';
const paymentService = 'http://payment-service:3003';

app.post('/api/checkout', async (req, res) => {
  try {
    // Call multiple services
    const userResponse = await fetch(`${userService}/users/${req.body.userId}`);
    const user = await userResponse.json();

    const orderResponse = await fetch(`${orderService}/orders`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(req.body.order),
    });
    const order = await orderResponse.json();

    const paymentResponse = await fetch(`${paymentService}/process`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        amount: order.total,
        paymentMethod: req.body.paymentMethod,
      }),
    });
    const payment = await paymentResponse.json();

    res.json({
      success: true,
      orderId: order.id,
      transactionId: payment.id,
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      message: 'Checkout failed',
    });
  }
});
```

### Real-time APIs with WebSockets

```javascript
const WebSocket = require('ws');
const wss = new WebSocket.Server({ port: 8080 });

// Real-time order updates
wss.on('connection', (ws) => {
  ws.on('message', (message) => {
    const data = JSON.parse(message);

    if (data.type === 'subscribe') {
      ws.userId = data.userId;
    }
  });
});

// Broadcast order updates to subscribed users
function broadcastOrderUpdate(userId, orderData) {
  wss.clients.forEach((client) => {
    if (client.userId === userId && client.readyState === WebSocket.OPEN) {
      client.send(
        JSON.stringify({
          type: 'order_update',
          data: orderData,
        }),
      );
    }
  });
}
```

## ✅ Key Takeaways for BAs

1. **APIs enable system integration** - Connect different software systems
2. **REST is simple and widely supported** - Good for most business needs
3. **GraphQL offers flexibility** - Better for complex data requirements
4. **Design APIs first** - Documentation before implementation
5. **Security is essential** - Authentication, authorization, rate limiting
6. **Versioning strategy needed** - Plan for API evolution
7. **Monitor and measure** - Track usage, performance, and errors

## 🔗 Next Steps

- **Inventory integration needs** - What systems need to connect?
- **Define API endpoints** - What data and operations are needed?
- **Plan authentication** - How will API access be controlled?
- **Design error handling** - How should failures be communicated?
- **Set up monitoring** - How will API health be tracked?

---

**Remember:** APIs are the nervous system of modern business applications. They enable integration, automation, and new business models. Well-designed APIs become valuable business assets that can drive innovation and partnerships.

Next: [Authentication & Security →](../04-authentication-security/README.md)
