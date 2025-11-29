
> [!quote] **महाभारतम्** (Epic Literature, India, ~400 BCE)  
> **“विद्या विवादाय धनं मदाय शक्तिः परेषां परिपीडनाय।”**  
> _“Knowledge used for arguments, wealth used for pride, and power used to crush others are all misused.”_

---

## **3. QUICK REFERENCE (WORDPRESS – XX Sheets)**

High-speed access to essential WordPress knowledge — ready for real-world use without deep-dive reading.  
Copy, paste, lookup, fix, deploy — instantly.

**WordPress Roadmap**  
WordPress Roadmap#3.1 Cheatsheets | WordPress Roadmap#3.2 Snippets | WordPress Roadmap#3.3 Templates | WordPress Roadmap#3.4 Architecture Diagrams | WordPress Roadmap#3.5 Error Playbook | WordPress Roadmap#3.6 Best Practices  
**Back to Top**

---

### **3.1 Cheatsheets – condensed one-page summaries**

Fast-lookup documents summarizing hooks, patterns, and workflows.

- WP-CLI cheatsheet & most-used commands
    
- Template hierarchy map + theme file resolution logic
    
- Actions & Filters reference (hooks quick index)
    
- WP_Query parameters map + common examples
    
- Gutenberg Block editor + block.json structure
    
- Roles & Capabilities matrix
    
- REST API routes & authentication flow
    
- WooCommerce essential hooks + templates override index
    

---

### **3.2 Snippets – copy-paste ready solutions**

Small blocks solving high-frequency WordPress dev needs.

- `wp_enqueue_script()` & `wp_enqueue_style()` boilerplates
    
- Add Shortcode + shortcode attributes + template rendering
    
- Register CPT + taxonomy + rewrite rules flush
    
- Custom Meta Boxes (Admin UI)
    
- `WP_Query()` examples (filters, ordering, pagination)
    
- AJAX request handler (`wp_ajax_` + `wp_ajax_nopriv_`)
    
- Custom REST API endpoint + JSON response
    
- User role creation + capability assignment
    
- Basic security utilities (nonce, sanitize, escape)
    

---

### **3.3 Templates – reusable structures for faster work**

#### **3.3.1 Prompt Templates – structured instruction patterns**

Reusable AI/tool prompts for consistent results.

- Debug performance → Query Monitor trace prompt
    
- Security audit prompt → sanitization & escaping check
    
- Plugin architecture review prompt
    
- Documentation generator prompt (hooks & features)
    

#### **3.3.2 Code Templates – common reusable outlines**

Blueprints for common WordPress systems.

- Starter Plugin skeleton (OOP architecture)
    
- CPT + taxonomy + meta + admin settings page template
    
- REST API + controller + schema + permission callback
    
- Gutenberg Block template with render callback
    
- Theme template list (header, footer, archive, 404)
    

#### **3.3.3 Boilerplates – full starter projects**

Ready-to-use scaffolds / git-clone-and-run.

- Plugin boilerplate (OOP + services + autoload + hooks)
    
- Block Theme + pattern library + theme.json config
    
- WooCommerce custom checkout fields + custom product type
    
- Docker WP + MySQL + phpMyAdmin + Mailhog environment
    

---

### **3.4 Condensed Architecture Diagrams – minimal visual summaries**

Mermaid-ready mini-maps (copy-paste into Obsidian)

- Request → Hooks → Theme selection → Loop → Response
    
- Template Hierarchy resolution map
    
- WP_Query lifecycle (SQL build → hydrate → render)
    
- Block Editor pipeline (Gutenberg → REST API → front-render)
    
- Deployment workflow (Local → Staging → Production)
    

---

### **3.5 Error & Issue Playbook – common problems and fixes**

#### **3.5.1 Common Errors**

- WSOD (White Screen Of Death)
    
- 500 internal errors / PHP Fatal error
    
- Database connection errors
    
- 404 custom post type permalink issues
    
- Plugin/theme conflict crashes
    
- “Headers already sent” warning
    
- HTTP API & CORS failures
    

#### **3.5.2 Typical Causes**

- Missing `wp_head()` / `wp_footer()` in theme
    
- Forgot to flush rewrite rules after CPT registration
    
- Plugin conflict or bad hook priority ordering
    
- Memory limit / max execution time
    
- Deprecated function calls
    
- Mixed HTTP/HTTPS
    

#### **3.5.3 One-line Fixes & Commands**

- `define('WP_DEBUG', true); define('WP_DEBUG_LOG', true);`
    
- `flush_rewrite_rules();`
    
- `wp config set WP_MEMORY_LIMIT 256M`
    
- `wp plugin deactivate --all` (to detect conflicts)
    
- `wp search-replace http:// https://`
    

---

### **3.6 Best Practices – recommended techniques**

#### **3.6.1 Do’s & Don’ts**

- **Do:** enqueue assets, use hooks, escape/validate/sanitize, separate logic/UI
    
- **Don’t:** edit core files, mix theme & plugin responsibilities, use inline SQL, store sensitive data in options table
    

#### **3.6.2 Performance Guidelines**

- Cache query results (`transients`, `object cache`)
    
- Use CDN + persistent cache + minified assets
    
- Reduce plugins & implement lazy loading
    
- Optimize database indexes
    
- Avoid nested loops with WP_Query
    

#### **3.6.3 Security Considerations**

- Nonce verification on forms & AJAX
    
- Capability checks on sensitive endpoints
    
- Always `sanitize_*()` input + `esc_*()` output
    
- Disable file edit in wp-admin
    
- Keep plugins, themes, and PHP updated
    

---

---

### **Next options**

a) Turn Cheatsheets into real formatted PDF pages  
b) Add real code snippets per section  
c) Create a visual mind-map version  
d) Add “Keywords” section (short glossary)  
e) Build full documentation navigation (sidebar style)

**Choose (a/b/c/d/e)** 🌿