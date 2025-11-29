
> [!quote] **मनुस्मृति** (Dharmashastra, India, ~2nd Century BCE)  
> **“उद्योगो हि नरस्य भक्षणम्।”**  
> _Pronunciation: udyogo hi narasya bhakṣaṇam_  
> _“Effort is the sustenance of a human being.”_

---

# **2.1 Code Examples – growing difficulty (WordPress)**

---

### **[[2.1.1 Basic Examples]]**

[[Enqueue Styles & Scripts]] • [[#Add Shortcodes]] • [[#Custom Post Type]] • [[#Custom Taxonomy]] • [[#Basic Theme Template]]

- **[[Enqueue Styles & Scripts]]** – proper loading of CSS & JS using `wp_enqueue_script()`
    
- **Add Shortcodes** – reusable content components inside posts & pages
    
- **Custom Post Type** – define structured content like `Books` or `Products`
    
- **Custom Taxonomy** – organize information with controlled categories
    
- **Basic Theme Template** – template hierarchy example using `single.php` / `page.php`
    

---

### **2.1.2 Intermediate Examples**

[[#WP_Query]] • [[#Custom Meta Fields & Meta Box]] • [[#AJAX in WordPress]] • [[#REST API Endpoints]] • [[#User Roles & Capabilities]]

- **WP_Query** – custom, optimized database queries
    
- **Custom Meta Fields & Meta Box** – storing extra structured content
    
- **AJAX in WordPress** – server interaction without page refresh
    
- **REST API Endpoints** – JSON-based API for headless frontends & apps
    
- **User Roles & Capabilities** – permission-based logic for secure functionality
    

---

### **2.1.3 Advanced Examples**

[[#Building a Full Plugin Architecture]] • [[#OOP in WordPress]] • [[#Security Best Practices]] • [[#Caching & Performance]] • [[#Multisite & Scaling Patterns]]

- **Building a Full Plugin Architecture** – structured, enterprise-grade plugin skeleton
    
- **OOP in WordPress** – organizing classes, dependency injection patterns
    
- **Security Best Practices** – escaping, sanitization, nonces, SQL safety
    
- **Caching & Performance** – object cache, transient API, Redis/CDN
    
- **Multisite & Scaling Patterns** – multiple site network, sharding, load balancing
    

---

# **2.2 Hands-on Mini Projects**

### **2.2.1 Beginner Project – WordPress Basics Lab**

[[#Personal Blog Theme]] • [[#Simple Plugin]] • [[#Content Customization Tools]]

- **Personal Blog Theme** – modify templates & custom layouts
    
- **Simple Plugin** – build plugin with shortcode & settings page
    
- **Content Tools** – custom blocks / Gutenberg basics
    

### **2.2.2 Intermediate Project – Mini CMS System**

[[#Directory Listing App]] • [[#CPT + Taxonomy + Meta]] • [[#Search & Filtering UI]]

- **Directory Listing App** – CPT-based listing system
    
- **Structured CPT & Taxonomies** – organized searchable content
    
- **Advanced Search & Filters** – using WP_Query + AJAX
    

### **2.2.3 Production Project – Real-World System**

[[#E-commerce System with WooCommerce]] • [[#Custom REST API Backend]] • [[#Docker Deployment]]

- **WooCommerce customization** – product types, checkout custom logic
    
- **Custom REST API Backend** – headless CMS for SPA/mobile apps
    
- **Docker / CI Deployment** – scalable, reproducible staging & production
    

---

# **2.3 Patterns & Workflows**

### **2.3.1 Design Patterns**

[[#Factory, Strategy, Singleton, Repository]] • [[#MVC-like WordPress Patterns]] • [[#Hooks-based Architecture]]

- **Factory / Strategy / Singleton / Repository** – reusable OOP organization
    
- **MVC-like patterns in WordPress** – controller/service separation
    
- **Hooks-based architecture** – event-driven modular design
    

### **2.3.2 Common Workflows**

[[#Theme Development Workflow]] • [[#Plugin Development Workflow]] • [[#CI/CD Deployment Flow]]

- **Theme workflow** – local development → templating → build pipeline
    
- **Plugin workflow** – planning → UI → logic → testing
    
- **CI/CD workflow** – versioning → staging → deploy
    

### **2.3.3 Anti-patterns**

[[#Editing Core Files]] • [[#Too Many Plugins]] • [[#Direct SQL Queries & No Security]]

- **Editing Core Files** – breaks update chain & maintainability
    
- **Too Many Plugins** – bloat, conflicts, performance downgrade
    
- **Direct SQL without escaping** – security disaster
    

---

# **2.4 Tools, Tips & Debugging Notes**

[[#WP-CLI]] • [[#Query Monitor]] • [[#Debug Log]] • [[#Xdebug]] • [[#Performance Profiling]]

- **WP-CLI** – automate tasks, managing users/tools from terminal
    
- **Query Monitor** – bottleneck detection, debugging query logic
    
- **Debug Log** – track warnings & failures
    
- **Xdebug** – advanced step debugging inside IDE
    
- **Performance Profiling** – inspect slow code and optimize
    

---

# **2.5 Real-World Use Cases**

### **2.5.1 Industry Applications**

E-commerce, LMS, Booking Systems, News Platforms, Intranets, Community Portals

### **2.5.2 Business Applications**

Landing pages, CRM dashboards, automation tools, branded customer systems

### **2.5.3 System Integrations**

Headless / Jamstack (React/Next.js, Vue/Nuxt), ERP, Payment gateways, AI-based services

---

---

## **Next options**

a) Convert these into **clickable wiki pages** (`[[CPT]]`, `[[WP_Query]]`, etc.)  
b) Create **real code snippets** per section  
c) Build **mind-map / diagram**  
d) Add **Keywords section** summary  
e) Start **Applied Practice build series**

**Choose a / b / c / d / e** 🌿