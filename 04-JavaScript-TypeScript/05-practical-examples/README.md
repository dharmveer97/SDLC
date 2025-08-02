# 💼 Practical Examples - BA Guide

## 🎯 Real-World JavaScript/TypeScript Applications

This section shows practical examples of how JavaScript and TypeScript solve real business problems. Each example includes both the technical implementation and business value explanation.

## 🛒 E-commerce Shopping Cart

### Business Problem

Customers need to add items to cart, modify quantities, and see updated totals without page reloads.

### Solution Example

```typescript
interface CartItem {
  productId: string;
  name: string;
  price: number;
  quantity: number;
  imageUrl: string;
}

interface ShoppingCart {
  items: CartItem[];
  subtotal: number;
  tax: number;
  shipping: number;
  total: number;
}

class CartManager {
  private cart: ShoppingCart = {
    items: [],
    subtotal: 0,
    tax: 0,
    shipping: 0,
    total: 0,
  };

  addItem(product: CartItem): void {
    const existingItem = this.cart.items.find(
      (item) => item.productId === product.productId,
    );

    if (existingItem) {
      existingItem.quantity += product.quantity;
    } else {
      this.cart.items.push(product);
    }

    this.updateTotals();
    this.saveToStorage();
    this.updateDisplay();
  }

  private updateTotals(): void {
    this.cart.subtotal = this.cart.items.reduce(
      (sum, item) => sum + item.price * item.quantity,
      0,
    );

    this.cart.tax = this.cart.subtotal * 0.08; // 8% tax
    this.cart.shipping = this.cart.subtotal > 50 ? 0 : 9.99;
    this.cart.total = this.cart.subtotal + this.cart.tax + this.cart.shipping;
  }
}
```

### Business Value

- **Instant feedback** - Customers see changes immediately
- **Reduced bounce rate** - No page reloads keep users engaged
- **Increased sales** - Easy cart management encourages more purchases
- **Better UX** - Smooth, responsive shopping experience

---

## 📝 Dynamic Form Validation

### Business Problem

Forms need real-time validation to guide users and prevent submission errors.

### Solution Example

```typescript
interface ValidationRule {
  field: string;
  validator: (value: string) => boolean;
  message: string;
}

class FormValidator {
  private rules: ValidationRule[] = [
    {
      field: 'email',
      validator: (value) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value),
      message: 'Please enter a valid email address',
    },
    {
      field: 'phone',
      validator: (value) => /^\(\d{3}\) \d{3}-\d{4}$/.test(value),
      message: 'Phone format: (123) 456-7890',
    },
    {
      field: 'password',
      validator: (value) =>
        value.length >= 8 && /(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/.test(value),
      message:
        'Password must be 8+ characters with uppercase, lowercase, and number',
    },
  ];

  validateField(
    fieldName: string,
    value: string,
  ): { isValid: boolean; message?: string } {
    const rule = this.rules.find((r) => r.field === fieldName);
    if (!rule) return { isValid: true };

    const isValid = rule.validator(value);
    return {
      isValid,
      message: isValid ? undefined : rule.message,
    };
  }

  validateForm(formData: Record<string, string>): boolean {
    let isValid = true;

    for (const rule of this.rules) {
      const result = this.validateField(rule.field, formData[rule.field] || '');
      if (!result.isValid) {
        this.showError(rule.field, result.message!);
        isValid = false;
      } else {
        this.clearError(rule.field);
      }
    }

    return isValid;
  }
}
```

### Business Value

- **Higher conversion rates** - Users complete forms successfully
- **Reduced support tickets** - Fewer invalid submissions
- **Better data quality** - Validated data from the start
- **Improved user satisfaction** - Clear guidance prevents frustration

---

## 📊 Real-time Dashboard

### Business Problem

Business stakeholders need live updates of key metrics without manual refresh.

### Solution Example

```typescript
interface DashboardMetrics {
  totalSales: number;
  todayOrders: number;
  activeUsers: number;
  conversionRate: number;
  lastUpdated: Date;
}

class LiveDashboard {
  private metrics: DashboardMetrics = {
    totalSales: 0,
    todayOrders: 0,
    activeUsers: 0,
    conversionRate: 0,
    lastUpdated: new Date(),
  };

  async initialize(): Promise<void> {
    // Load initial data
    await this.refreshMetrics();

    // Set up real-time updates every 30 seconds
    setInterval(() => this.refreshMetrics(), 30000);

    // Set up WebSocket for instant notifications
    this.setupRealTimeUpdates();
  }

  private async refreshMetrics(): Promise<void> {
    try {
      const response = await fetch('/api/dashboard/metrics');
      const newMetrics: DashboardMetrics = await response.json();

      this.updateMetrics(newMetrics);
      this.updateDisplay();
    } catch (error) {
      this.showError('Unable to update dashboard metrics');
    }
  }

  private updateMetrics(newMetrics: DashboardMetrics): void {
    // Animate changes for visual impact
    this.animateChange(
      'total-sales',
      this.metrics.totalSales,
      newMetrics.totalSales,
    );
    this.animateChange(
      'today-orders',
      this.metrics.todayOrders,
      newMetrics.todayOrders,
    );

    this.metrics = newMetrics;
    this.metrics.lastUpdated = new Date();
  }

  private animateChange(
    elementId: string,
    oldValue: number,
    newValue: number,
  ): void {
    const element = document.getElementById(elementId);
    if (!element) return;

    // Highlight changes
    if (oldValue !== newValue) {
      element.classList.add('updated');
      setTimeout(() => element.classList.remove('updated'), 2000);
    }

    // Animate number changes
    this.animateNumber(element, oldValue, newValue, 1000);
  }
}
```

### Business Value

- **Real-time decision making** - Current data enables quick responses
- **Increased engagement** - Stakeholders check dashboard more frequently
- **Operational efficiency** - Immediate awareness of issues or opportunities
- **Better performance tracking** - Live metrics show impact of changes

---

## 🔍 Advanced Search with Autocomplete

### Business Problem

Users need to find products quickly with intelligent search suggestions.

### Solution Example

```typescript
interface SearchResult {
  id: string;
  title: string;
  category: string;
  price: number;
  imageUrl: string;
  relevanceScore: number;
}

class SmartSearch {
  private searchCache = new Map<string, SearchResult[]>();
  private searchTimeout: number | null = null;

  async performSearch(query: string): Promise<SearchResult[]> {
    // Return cached results if available
    if (this.searchCache.has(query)) {
      return this.searchCache.get(query)!;
    }

    try {
      const response = await fetch(
        `/api/search?q=${encodeURIComponent(query)}&limit=10`,
      );
      const results: SearchResult[] = await response.json();

      // Cache results for faster subsequent searches
      this.searchCache.set(query, results);

      return results;
    } catch (error) {
      console.error('Search failed:', error);
      return [];
    }
  }

  setupAutoComplete(inputElement: HTMLInputElement): void {
    inputElement.addEventListener('input', (event) => {
      const query = (event.target as HTMLInputElement).value.trim();

      // Clear previous timeout
      if (this.searchTimeout) {
        clearTimeout(this.searchTimeout);
      }

      // Debounce search requests
      this.searchTimeout = window.setTimeout(async () => {
        if (query.length >= 2) {
          const results = await this.performSearch(query);
          this.showSuggestions(results, inputElement);
        } else {
          this.hideSuggestions();
        }
      }, 300);
    });
  }

  private showSuggestions(
    results: SearchResult[],
    inputElement: HTMLInputElement,
  ): void {
    const suggestionsBox = document.getElementById('search-suggestions');
    if (!suggestionsBox) return;

    suggestionsBox.innerHTML = '';

    results.slice(0, 5).forEach((result) => {
      const suggestion = document.createElement('div');
      suggestion.className = 'search-suggestion';
      suggestion.innerHTML = `
        <img src="${result.imageUrl}" alt="${
        result.title
      }" class="suggestion-image">
        <div class="suggestion-content">
          <div class="suggestion-title">${result.title}</div>
          <div class="suggestion-price">$${result.price.toFixed(2)}</div>
        </div>
      `;

      suggestion.addEventListener('click', () => {
        window.location.href = `/products/${result.id}`;
      });

      suggestionsBox.appendChild(suggestion);
    });

    suggestionsBox.style.display = 'block';
  }
}
```

### Business Value

- **Improved product discovery** - Users find items faster
- **Higher search conversion** - Better results lead to more purchases
- **Reduced bounce rate** - Users stay engaged with helpful suggestions
- **Better user experience** - Intelligent search feels more responsive

---

## 📱 Progressive Web App Features

### Business Problem

Mobile users expect app-like experiences with offline capabilities.

### Solution Example

```typescript
class PWAManager {
  private isOnline = navigator.onLine;
  private pendingActions: Array<{ action: string; data: any }> = [];

  initialize(): void {
    this.registerServiceWorker();
    this.setupOfflineHandling();
    this.setupPushNotifications();
  }

  private async registerServiceWorker(): Promise<void> {
    if ('serviceWorker' in navigator) {
      try {
        await navigator.serviceWorker.register('/sw.js');
        console.log('Service Worker registered successfully');
      } catch (error) {
        console.log('Service Worker registration failed');
      }
    }
  }

  private setupOfflineHandling(): void {
    window.addEventListener('online', () => {
      this.isOnline = true;
      this.syncPendingActions();
      this.showStatus('Back online - syncing data...');
    });

    window.addEventListener('offline', () => {
      this.isOnline = false;
      this.showStatus('You are offline - changes will sync when connected');
    });
  }

  async saveData(data: any): Promise<boolean> {
    if (this.isOnline) {
      try {
        await this.sendToServer(data);
        return true;
      } catch (error) {
        // Queue for later if network fails
        this.queueAction('save', data);
        return false;
      }
    } else {
      // Store locally when offline
      this.queueAction('save', data);
      this.saveToLocalStorage(data);
      return true;
    }
  }

  private queueAction(action: string, data: any): void {
    this.pendingActions.push({ action, data });
    localStorage.setItem('pendingActions', JSON.stringify(this.pendingActions));
  }

  private async syncPendingActions(): Promise<void> {
    const actions = [...this.pendingActions];
    this.pendingActions = [];

    for (const { action, data } of actions) {
      try {
        await this.sendToServer(data);
      } catch (error) {
        // Re-queue failed actions
        this.queueAction(action, data);
      }
    }

    localStorage.setItem('pendingActions', JSON.stringify(this.pendingActions));
  }
}
```

### Business Value

- **Increased mobile engagement** - App-like experience on web
- **Better conversion rates** - Works even with poor connectivity
- **Reduced app development costs** - One codebase for web and mobile
- **Improved user retention** - Push notifications bring users back

---

## 📈 A/B Testing Framework

### Business Problem

Need to test different features and measure their impact on user behavior.

### Solution Example

```typescript
interface ExperimentConfig {
  name: string;
  variants: string[];
  trafficSplit: number[];
  conversionGoal: string;
}

class ABTestManager {
  private activeExperiments = new Map<string, string>();

  initializeExperiment(config: ExperimentConfig): string {
    const userId = this.getUserId();
    const variantIndex = this.getVariantForUser(userId, config);
    const variant = config.variants[variantIndex];

    this.activeExperiments.set(config.name, variant);

    // Track experiment participation
    this.trackEvent('experiment_started', {
      experiment: config.name,
      variant: variant,
      userId: userId,
    });

    return variant;
  }

  trackConversion(experimentName: string, conversionValue?: number): void {
    const variant = this.activeExperiments.get(experimentName);
    if (!variant) return;

    this.trackEvent('conversion', {
      experiment: experimentName,
      variant: variant,
      value: conversionValue || 1,
      timestamp: new Date().toISOString(),
    });
  }

  private getVariantForUser(userId: string, config: ExperimentConfig): number {
    // Consistent assignment based on user ID
    const hash = this.hashUserId(userId);
    const bucket = hash % 100;

    let cumulativeWeight = 0;
    for (let i = 0; i < config.trafficSplit.length; i++) {
      cumulativeWeight += config.trafficSplit[i];
      if (bucket < cumulativeWeight) {
        return i;
      }
    }

    return 0; // Default to first variant
  }

  // Example usage in application
  setupCheckoutExperiment(): void {
    const experiment = this.initializeExperiment({
      name: 'checkout_button_text',
      variants: ['Buy Now', 'Complete Purchase', 'Order Now'],
      trafficSplit: [33, 33, 34], // Equal split
      conversionGoal: 'purchase_completed',
    });

    // Apply the variant
    const checkoutButton = document.getElementById('checkout-button');
    if (checkoutButton) {
      checkoutButton.textContent = experiment;
    }
  }
}
```

### Business Value

- **Data-driven decisions** - Measure actual impact of changes
- **Increased conversion rates** - Optimize based on real user behavior
- **Risk mitigation** - Test changes on subset before full rollout
- **Continuous improvement** - Systematic approach to optimization

## ✅ Key Takeaways for BAs

1. **JavaScript/TypeScript solve real business problems** - Not just technical exercises
2. **User experience drives business value** - Smooth interactions increase conversions
3. **Real-time features increase engagement** - Users expect immediate feedback
4. **Data validation prevents costly errors** - Catch issues early in the process
5. **Progressive enhancement works** - Start simple, add advanced features gradually
6. **Performance impacts business metrics** - Fast, responsive apps perform better
7. **Testing and optimization are ongoing** - Use data to drive improvements

## 🔗 Next Steps

- **Identify similar use cases** in your projects
- **Plan progressive enhancement** approach for new features
- **Consider offline capabilities** for mobile users
- **Implement analytics tracking** for data-driven decisions
- **Start with simple examples** and build complexity gradually

---

**Remember:** These examples show how JavaScript and TypeScript directly contribute to business success. Each technical implementation should map to clear business value - improved user experience, increased conversions, better data quality, or operational efficiency.

**Module Complete!** 🎉

Next: [React & Modern Frameworks →](../../05-React-Modern-Frameworks/README.md)
