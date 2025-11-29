## Overview - (50w)

Before WordPress had a proper asset system, developers hard-coded `<script>` and `<link>` tags directly into header.php and footer.php. The result?  
- Duplicate files loaded by multiple plugins  
- Broken dependency order (scripts running before their libraries)  
- Impossible cache-busting  
- Endless conflicts in large plugin/theme ecosystems  

`wp_enqueue_scripts` was introduced to fix all of that. It is the official, required way to load CSS and JavaScript on the frontend.

### Where It Fires in the WordPress Lifecycle

``` ruby
BOOT     → Core, plugins, theme loaded
INIT     → CPTs, taxonomies, shortcodes registered
QUERY    → URL parsed → WP_Query decides page type
TEMPLATE → Correct template file chosen
ASSETS
   ↓ wp_head()
       → wp_enqueue_scripts fires 🔥
           (all CSS/JS registered here)
RENDER   → header.php → content → footer.php → wp_footer()
SHUTDOWN → Output sent to browser

```

### Production-Ready Example (2025 Best Practices)

```php
add_action( 'wp_enqueue_scripts', 'mytheme_assets' );
function mytheme_assets() {
    $version = wp_get_theme()->get( 'Version' );

    // Global stylesheet
    wp_enqueue_style(
        'mytheme-style',
        get_template_directory_uri() . '/dist/css/style.css',
        [],           // no dependencies
        $version,
        'all'
    );

    // Global script – jQuery dependent, deferred, footer
    wp_enqueue_script(
        'mytheme-app',
        get_template_directory_uri() . '/dist/js/app.js',
        ['jquery'],   // dependency
        $version,
        [
            'in_footer' => true,
            'strategy'  => 'defer',   // native defer since WP 6.3
        ]
    );

    // Pass PHP values to JavaScript
    wp_localize_script( 'mytheme-app', 'MYTHEME', [
        'ajaxUrl' => admin_url( 'admin-ajax.php' ),
        'nonce'   => wp_create_nonce( 'mytheme_nonce' ),
        'isHome'  => is_front_page(),
    ] );

    // Page-specific assets (example: single product page)
    if ( is_singular( 'product' ) ) {
        wp_enqueue_script(
            'mytheme-product',
            get_template_directory_uri() . '/dist/js/product.js',
            ['mytheme-app'],
            $version,
            ['strategy' => 'defer']
        );
    }
}
```


---
## Tecnical's

```php
wp_enqueue_style( string $handle, string $src = '', array $deps = array(), string|bool|null $ver = false, string $media = 'all' );
```

---
### **`string $handle` | required**
> Unique identifier for the stylesheet used by WordPress to reference, control, and manage it.  

Always prefix it (e.g., `mytheme-`, `myplugin-`) to avoid naming conflicts across themes and plugins.

### **`string|bool $src` | required when enqueuing**
> Defines the exact source location of the stylesheet — either a URL or a file path.  
> Used to load theme assets, plugin assets, CDN resources, or registered-but-not-loaded placeholders.

Theme path → `get_stylesheet_directory_uri() . '/dist/style.css'`  
Plugin path → `plugins_url( 'css/style.css', __FILE__ )`  
Use `false` only when registering without loading via `wp_register_style()`

### **`array $deps` | optional (default: `[]`)**

> List of stylesheet handles that must load before this one to maintain correct dependency order.  
> Ensures proper styling overrides and avoids conflicts with framework or parent theme CSS.

Common examples: `parent-style`, `bootstrap-css`, `wp-block-library`

### **`string|bool|null $ver` | optional (default: `null`)**

> Version value used for cache busting and update control.  
> Helps browsers detect changes to compiled assets, improving performance and reliability.

Recommended 2025 usage:

- `filemtime( get_stylesheet_directory() . '/dist/style.css' )` — auto-update on file changes
    
- `wp_get_theme()->get( 'Version' )` — uses version from theme header
    
- `null` — applies WordPress core version
    
- `false` — removes version query entirely
    

### **`string $media` | optional (default: `'all'`)**

> Defines when the stylesheet should be applied.  
> Useful for performance optimization and responsive styles.

Accepted values: `'all'`, `'screen'`, `'print'`, `'(min-width: 768px)'`

---

## Interview Questions and answers