## 1.1 Definitions – Essential WordPress Terms

#### **Overview**
This section defines the foundational concepts required to understand WordPress structure, workflow, and terminology. These topics recur throughout development, documentation, debugging, architecture decisions, and professional communication.

|Topic|Brief|
|---|---|
|**[[WordPress Core]]**|The central framework controlling system execution, routing, permissions, and APIs, enabling themes and plugins to function reliably without modifying core files.|
|**[[Theme]]**|Controls layout, visual presentation, and interface structure, separating display from logic to ensure maintainability and flexible customization.|
|**[[Plugin]]**|Extends or modifies functionality using hooks and APIs, allowing modular feature development without altering themes or WordPress core.|
|**[[Posts vs Pages]]**|Posts represent chronological, regularly updated content; pages serve stable, evergreen information like About or Contact. Both use shared architecture but different purposes.|
|**[[Custom Post Types (CPT)]]**|Enables structured content models beyond posts and pages, supporting application-level entities like products, events, or listings for scalable custom systems.|
|**[[Taxonomies]]**|Provide structured categorization and relationships between content types, improving navigation, filtering, and semantic organization across complex data environments.|
|**[[Template Hierarchy]]**|A rule-based file selection system determining which theme template renders a view based on context, ensuring predictable display behavior.|
|**[[The Loop]]**|The mechanism that iterates through query results and outputs post content, transforming database responses into formatted HTML through theme templates.|
|**[[Hooks (Actions & Filters)]]**|Extension points that allow developers to insert or modify functionality without editing core, supporting safe customization and modular architecture.|
|**[[wp-admin / Dashboard]]**|Secure backend interface for managing content, users, plugins, themes, and system settings through centralized administrative tools.|
|**[[Database (MySQL)]]**|Stores site content, users, metadata, and configuration, forming the persistent layer powering dynamic rendering and application logic.|
|**[[REST API]]**|Provides JSON-based programmatic access to WordPress data, enabling integrations with mobile apps, SPAs, and headless architectures.|
