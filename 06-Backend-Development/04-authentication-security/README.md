# 🔐 Authentication & Security - BA Guide

## 🎯 What Is Authentication & Security?

Authentication is **verifying who users are**, while security is **protecting data and systems** from unauthorized access. In business applications, this is like having ID checks, security guards, and safe deposit boxes for digital assets.

Think of it as:
- **ID verification** - Proving you are who you claim to be
- **Access control** - Determining what you're allowed to do
- **Digital fortress** - Protecting valuable business data
- **Trust infrastructure** - Building confidence in your systems

## 🏢 Real-World Business Analogy

**Physical Office Security:**
- **Badge swipe** to enter building (authentication)
- **Different access levels** for different areas (authorization)
- **Security cameras** monitoring activity (auditing)
- **Safes and locks** protecting valuable items (data encryption)

**Digital Application Security:**
- **Login credentials** to access system (authentication)
- **User roles** determining available features (authorization)
- **Activity logs** tracking user actions (auditing)
- **Encrypted data** protecting sensitive information (data security)

## 🎯 What This Means for Business Analysts

### 1. **Risk Management**
```markdown
Security Risks Without Proper Authentication:
- Data breaches (customer information stolen)
- Financial losses (unauthorized transactions)
- Compliance violations (GDPR, HIPAA fines)
- Reputation damage (customer trust lost)
- Business disruption (systems compromised)

Security Benefits:
- Protected customer data
- Regulatory compliance
- Customer trust and confidence
- Business continuity assurance
```

### 2. **Business Process Impact**
Authentication affects user experience:
- **Onboarding** - How users register and verify accounts
- **Daily usage** - How often users need to log in
- **Security vs convenience** - Balance between protection and usability
- **Support burden** - Password resets, account issues

### 3. **Compliance Requirements**
Many industries require specific security measures:
- **Healthcare** - HIPAA compliance for patient data
- **Finance** - PCI DSS for payment card data
- **EU customers** - GDPR data protection requirements
- **Government** - Various federal security standards

## 🔑 Authentication Methods

### 1. Username/Password Authentication
```javascript
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');

// User registration with secure password hashing
app.post('/api/auth/register', async (req, res) => {
  try {
    const { email, password, firstName, lastName } = req.body;
    
    // Validate password strength
    if (password.length < 8) {
      return res.status(400).json({
        success: false,
        message: 'Password must be at least 8 characters long'
      });
    }
    
    // Check if user already exists
    const existingUser = await User.findOne({ email });
    if (existingUser) {
      return res.status(409).json({
        success: false,
        message: 'User already exists'
      });
    }
    
    // Hash password securely (never store plain text)
    const saltRounds = 12;
    const hashedPassword = await bcrypt.hash(password, saltRounds);
    
    // Create user account
    const user = await User.create({
      email,
      password: hashedPassword,
      firstName,
      lastName,
      role: 'customer',
      createdAt: new Date()
    });
    
    // Generate authentication token
    const token = jwt.sign(
      { 
        userId: user._id, 
        email: user.email,
        role: user.role 
      },
      process.env.JWT_SECRET,
      { expiresIn: '24h' }
    );
    
    res.status(201).json({
      success: true,
      token,
      user: {
        id: user._id,
        email: user.email,
        firstName: user.firstName,
        lastName: user.lastName,
        role: user.role
      }
    });
    
  } catch (error) {
    res.status(500).json({
      success: false,
      message: 'Registration failed'
    });
  }
});

// Secure login process
app.post('/api/auth/login', async (req, res) => {
  try {
    const { email, password } = req.body;
    
    // Find user by email
    const user = await User.findOne({ email });
    if (!user) {
      return res.status(401).json({
        success: false,
        message: 'Invalid credentials'
      });
    }
    
    // Verify password
    const validPassword = await bcrypt.compare(password, user.password);
    if (!validPassword) {
      return res.status(401).json({
        success: false,
        message: 'Invalid credentials'
      });
    }
    
    // Update last login
    await User.findByIdAndUpdate(user._id, { 
      lastLogin: new Date() 
    });
    
    // Generate new token
    const token = jwt.sign(
      { 
        userId: user._id, 
        email: user.email,
        role: user.role 
      },
      process.env.JWT_SECRET,
      { expiresIn: '24h' }
    );
    
    res.json({
      success: true,
      token,
      user: {
        id: user._id,
        email: user.email,
        firstName: user.firstName,
        lastName: user.lastName,
        role: user.role
      }
    });
    
  } catch (error) {
    res.status(500).json({
      success: false,
      message: 'Login failed'
    });
  }
});
```

### 2. Multi-Factor Authentication (MFA)
```javascript
const speakeasy = require('speakeasy');
const QRCode = require('qrcode');

// Enable 2FA for user account
app.post('/api/auth/enable-2fa', authenticateUser, async (req, res) => {
  try {
    const user = await User.findById(req.user.userId);
    
    // Generate secret for 2FA
    const secret = speakeasy.generateSecret({
      name: `MyApp (${user.email})`,
      issuer: 'MyApp'
    });
    
    // Store secret temporarily (user needs to verify)
    await User.findByIdAndUpdate(user._id, {
      twoFactorSecret: secret.base32,
      twoFactorEnabled: false // Will be true after verification
    });
    
    // Generate QR code for authenticator app
    const qrCodeUrl = await QRCode.toDataURL(secret.otpauth_url);
    
    res.json({
      success: true,
      qrCode: qrCodeUrl,
      manualEntryKey: secret.base32,
      message: 'Scan QR code with authenticator app'
    });
    
  } catch (error) {
    res.status(500).json({
      success: false,
      message: '2FA setup failed'
    });
  }
});

// Verify 2FA code and complete login
app.post('/api/auth/verify-2fa', async (req, res) => {
  try {
    const { email, password, code } = req.body;
    
    // Regular login verification
    const user = await User.findOne({ email });
    const validPassword = await bcrypt.compare(password, user.password);
    
    if (!user || !validPassword) {
      return res.status(401).json({
        success: false,
        message: 'Invalid credentials'
      });
    }
    
    // Verify 2FA code
    const verified = speakeasy.totp.verify({
      secret: user.twoFactorSecret,
      encoding: 'base32',
      token: code,
      window: 2 // Allow 2 time steps for clock drift
    });
    
    if (!verified) {
      return res.status(401).json({
        success: false,
        message: 'Invalid 2FA code'
      });
    }
    
    // Generate token after successful 2FA
    const token = jwt.sign(
      { userId: user._id, email: user.email, role: user.role },
      process.env.JWT_SECRET,
      { expiresIn: '24h' }
    );
    
    res.json({
      success: true,
      token,
      message: '2FA verification successful'
    });
    
  } catch (error) {
    res.status(500).json({
      success: false,
      message: '2FA verification failed'
    });
  }
});
```

### 3. OAuth/Social Login
```javascript
const passport = require('passport');
const GoogleStrategy = require('passport-google-oauth20').Strategy;

// Configure Google OAuth
passport.use(new GoogleStrategy({
  clientID: process.env.GOOGLE_CLIENT_ID,
  clientSecret: process.env.GOOGLE_CLIENT_SECRET,
  callbackURL: "/api/auth/google/callback"
}, async (accessToken, refreshToken, profile, done) => {
  try {
    // Check if user exists
    let user = await User.findOne({ googleId: profile.id });
    
    if (user) {
      return done(null, user);
    }
    
    // Create new user from Google profile
    user = await User.create({
      googleId: profile.id,
      email: profile.emails[0].value,
      firstName: profile.name.givenName,
      lastName: profile.name.familyName,
      profilePicture: profile.photos[0].value,
      role: 'customer',
      emailVerified: true // Google emails are verified
    });
    
    done(null, user);
  } catch (error) {
    done(error, null);
  }
}));

// Google OAuth routes
app.get('/api/auth/google',
  passport.authenticate('google', { scope: ['profile', 'email'] })
);

app.get('/api/auth/google/callback',
  passport.authenticate('google', { session: false }),
  (req, res) => {
    // Generate JWT token
    const token = jwt.sign(
      { userId: req.user._id, email: req.user.email, role: req.user.role },
      process.env.JWT_SECRET,
      { expiresIn: '24h' }
    );
    
    // Redirect to frontend with token
    res.redirect(`${process.env.FRONTEND_URL}?token=${token}`);
  }
);
```

## 🛡️ Authorization & Access Control

### Role-Based Access Control (RBAC)
```javascript
// Define user roles and permissions
const roles = {
  customer: {
    permissions: ['read:profile', 'update:profile', 'create:order', 'read:own_orders']
  },
  admin: {
    permissions: ['read:all_users', 'update:all_users', 'delete:users', 'read:all_orders', 'update:orders']
  },
  moderator: {
    permissions: ['read:all_users', 'update:user_status', 'read:all_orders']
  }
};

// Middleware to check permissions
const requirePermission = (permission) => {
  return async (req, res, next) => {
    try {
      const user = await User.findById(req.user.userId);
      const userPermissions = roles[user.role]?.permissions || [];
      
      if (!userPermissions.includes(permission)) {
        return res.status(403).json({
          success: false,
          message: 'Insufficient permissions'
        });
      }
      
      next();
    } catch (error) {
      res.status(500).json({
        success: false,
        message: 'Permission check failed'
      });
    }
  };
};

// Protected routes with specific permissions
app.get('/api/admin/users', 
  authenticateUser, 
  requirePermission('read:all_users'), 
  async (req, res) => {
    const users = await User.find({}, '-password -twoFactorSecret');
    res.json({ success: true, data: users });
  }
);

app.delete('/api/admin/users/:id', 
  authenticateUser, 
  requirePermission('delete:users'),
  async (req, res) => {
    await User.findByIdAndDelete(req.params.id);
    res.json({ success: true, message: 'User deleted' });
  }
);
```

## 🔒 Data Security Measures

### Encryption and Data Protection
```javascript
const crypto = require('crypto');

// Encrypt sensitive data before storing
function encryptSensitiveData(text) {
  const algorithm = 'aes-256-gcm';
  const key = Buffer.from(process.env.ENCRYPTION_KEY, 'hex');
  const iv = crypto.randomBytes(16);
  
  const cipher = crypto.createCipher(algorithm, key);
  cipher.setAAD(Buffer.from('additional-data'));
  
  let encrypted = cipher.update(text, 'utf8', 'hex');
  encrypted += cipher.final('hex');
  
  const authTag = cipher.getAuthTag();
  
  return {
    encrypted,
    iv: iv.toString('hex'),
    authTag: authTag.toString('hex')
  };
}

// Decrypt sensitive data when retrieving
function decryptSensitiveData(encryptedData) {
  const algorithm = 'aes-256-gcm';
  const key = Buffer.from(process.env.ENCRYPTION_KEY, 'hex');
  
  const decipher = crypto.createDecipher(algorithm, key);
  decipher.setAAD(Buffer.from('additional-data'));
  decipher.setAuthTag(Buffer.from(encryptedData.authTag, 'hex'));
  
  let decrypted = decipher.update(encryptedData.encrypted, 'hex', 'utf8');
  decrypted += decipher.final('utf8');
  
  return decrypted;
}

// Example: Storing encrypted payment information
app.post('/api/payment-methods', authenticateUser, async (req, res) => {
  try {
    const { cardNumber, expiryDate, cardholderName } = req.body;
    
    // Encrypt sensitive card data
    const encryptedCardNumber = encryptSensitiveData(cardNumber);
    
    // Store only encrypted data and metadata
    const paymentMethod = await PaymentMethod.create({
      userId: req.user.userId,
      cardNumber: encryptedCardNumber,
      last4: cardNumber.slice(-4), // Store last 4 digits for display
      cardType: detectCardType(cardNumber),
      expiryDate: encryptSensitiveData(expiryDate),
      cardholderName,
      createdAt: new Date()
    });
    
    res.status(201).json({
      success: true,
      paymentMethod: {
        id: paymentMethod._id,
        last4: paymentMethod.last4,
        cardType: paymentMethod.cardType,
        cardholderName: paymentMethod.cardholderName
      }
    });
    
  } catch (error) {
    res.status(500).json({
      success: false,
      message: 'Failed to save payment method'
    });
  }
});
```

### Security Headers and Middleware
```javascript
const helmet = require('helmet');
const rateLimit = require('express-rate-limit');

// Security headers
app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      scriptSrc: ["'self'"],
      imgSrc: ["'self'", "data:", "https:"]
    }
  },
  hsts: {
    maxAge: 31536000,
    includeSubDomains: true
  }
}));

// Rate limiting to prevent abuse
const authLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 5, // limit each IP to 5 requests per windowMs for auth routes
  message: 'Too many authentication attempts, please try again later',
  standardHeaders: true,
  legacyHeaders: false
});

// Apply rate limiting to authentication routes
app.use('/api/auth', authLimiter);

// General API rate limiting
const apiLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // limit each IP to 100 requests per windowMs
  message: 'Too many requests, please try again later'
});

app.use('/api', apiLimiter);

// Input validation and sanitization
const { body, validationResult } = require('express-validator');

const validateUserRegistration = [
  body('email').isEmail().normalizeEmail(),
  body('password').isLength({ min: 8 }).matches(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/),
  body('firstName').trim().isLength({ min: 1, max: 50 }),
  body('lastName').trim().isLength({ min: 1, max: 50 })
];

app.post('/api/auth/register', validateUserRegistration, (req, res) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).json({
      success: false,
      message: 'Validation failed',
      errors: errors.array()
    });
  }
  
  // Continue with registration...
});
```

## 📋 Common BA Questions & Answers

**Q: How long does it take to implement authentication?**
A: Basic auth: 1-2 weeks. Advanced features (2FA, OAuth): 3-4 weeks additional.

**Q: What's the cost of a security breach?**
A: Average cost: $4.45 million globally. Includes fines, remediation, lost business.

**Q: Do we need 2FA for all users?**
A: Recommended for admin users and sensitive operations. Optional for regular users.

**Q: How often should users change passwords?**
A: Modern guidance: Only when compromised. Focus on strong passwords and 2FA instead.

**Q: Can we use social login only?**
A: Yes, but provide email backup for users who don't use social platforms.

## 🎯 What BAs Should Include in Requirements

### Authentication Requirements
```markdown
✅ Good Requirements:
- "Users must create accounts with email and password (8+ characters)"
- "Admin users must enable 2FA within 30 days of account creation"
- "Password reset via email link, valid for 1 hour"
- "Social login options: Google, Facebook, LinkedIn"
- "Account lockout after 5 failed login attempts for 30 minutes"

❌ Vague Requirements:
- "Secure login system"
- "User accounts"
- "Authentication needed"
```

### Security Requirements
```markdown
Include in Requirements:
- Data encryption standards (AES-256 for sensitive data)
- Session management (24-hour token expiry)
- API security (rate limiting, input validation)
- Audit logging (who did what when)
- Compliance requirements (GDPR, HIPAA, etc.)
```

## 📈 Measuring Security Success

### Security Metrics
- **Authentication success rate** (percentage of successful logins)
- **Account takeover attempts** (blocked unauthorized access)
- **Password reset frequency** (indicator of security issues)
- **2FA adoption rate** (percentage of users with 2FA enabled)

### Compliance Metrics
- **Security audit results** (passed/failed assessments)
- **Data breach incidents** (number and severity)
- **Response time** (time to detect and respond to threats)
- **Training completion** (security awareness training)

## ✅ Key Takeaways for BAs

1. **Authentication is business critical** - Protects customers and company
2. **Balance security with usability** - Too complex drives users away
3. **2FA significantly improves security** - Recommend for sensitive accounts
4. **Compliance is often required** - Know your industry regulations
5. **Regular security updates needed** - Ongoing maintenance required
6. **Monitor and log everything** - Detect threats early
7. **Plan for incident response** - Have procedures for breaches

## 🔗 Next Steps

- **Identify sensitive data** - What needs the highest protection?
- **Choose authentication methods** - Password, 2FA, social login?
- **Plan user roles** - What permissions does each role need?
- **Set compliance requirements** - What regulations apply?
- **Design security monitoring** - How will threats be detected?

---

**Remember:** Security is not just about technology - it's about protecting your business, customers, and reputation. Invest in security early; it's much more expensive to add later or recover from a breach.

Next: [Real-World Backend →](../05-real-world-backend/README.md)