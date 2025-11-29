```ruby
BOOT (Stage 1 — System Boot)
└─ Load core environment & configuration
   ├─ Load wp-config.php (DB, constants, site config)
   ├─ Load wp-settings.php (bootstrap WordPress core)
   │   ├─ Initialize global variables ($wpdb, $wp_query, $wp_rewrite)
   │   ├─ Load core libraries & APIs
   │   └─ Load default system components
   ├─ Load MU Plugins
   ├─ Load Active Plugins
   ├─ Load Drop-ins (advanced-cache.php, db.php, etc.)
   └─ Load active theme
       ├─ Parent theme functions.php
       └─ Child theme functions.php (if present)

INIT (Stage 2 — Initialization & Registration)
└─ Fire `init` hook
   ├─ Register CPTs (`register_post_type`)
   ├─ Register Taxonomies (`register_taxonomy`)
   ├─ Register Shortcodes (`add_shortcode`)
   ├─ Register Menus & Widget Areas
   ├─ Load Translations (.mo language files)
   └─ Setup Rewrite Rules / Permalinks

QUERY (Stage 3 — Request Routing)
└─ Parse requested URL
   ├─ Fire `pre_get_posts` to modify main query
   └─ WP_Query determines content type:
       ├─ Home / Front Page
       ├─ Single Post / Page
       ├─ Category / Tag / Taxonomy
       ├─ CPT Archive
       ├─ Search Results
       └─ 404 Not Found

TEMPLATE (Stage 4 — Template Selection)
└─ Fire `template_redirect`
└─ Choose appropriate template via hierarchy:
   ├─ front-page.php
   ├─ home.php
   ├─ single.php / page.php
   ├─ archive.php / category.php / author.php / taxonomy.php
   └─ index.php (fallback)

ASSETS (Stage 5 — Asset Preparation & Loading)
└─ `wp_head()` runs
   └─ `wp_enqueue_scripts` fires  ✅
       ├─ Register / enqueue CSS & JS
       ├─ Manage dependencies & ordering
       ├─ Apply versioning / cache-busting
       └─ Conditional page-specific asset logic

RENDER (Stage 6 — Display HTML to Browser)
└─ Include theme template parts:
   ├─ header.php  → head & opening layout
   ├─ content templates
   ├─ sidebar.php (optional)
   └─ footer.php  → closing layout
└─ `wp_footer()` prints deferred scripts

SHUTDOWN (Stage 7 — Finalization)
└─ Send generated HTML to browser
└─ Fire `shutdown` hook
└─ Cleanup (memory release, DB close, cron triggers)


```