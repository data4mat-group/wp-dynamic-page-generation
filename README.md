# Dynamic Content Template Engine for WordPress

## Overview
This project provides a reusable and scalable system for dynamically generating page content in WordPress using predefined templates and structured database-driven data.

Instead of creating and maintaining large numbers of static pages, this solution enables a single template (or a small set of templates) to render content dynamically based on request parameters. The system is designed to handle hierarchical data structures such as product catalogs with multiple levels (e.g., categories, subcategories, and individual products).

The goal is to reduce complexity, improve maintainability, and enable non-developers to manage content through structured input interfaces.

---

## Key Features
- Dynamic page rendering using reusable templates  
- Support for hierarchical data (classes → subclasses → products)  
- Clean URL structure mapped to database-driven content  
- Centralized data management (no need to create multiple pages manually)  
- Scalable architecture suitable for large catalogs  
- Compatible with standard WordPress environments  
- Designed for reuse across multiple projects  

---

## Use Case Example

### Product Structure
The system supports multi-level navigation such as:

- Product Classes (e.g., Electronics, Furniture)
  - Sub-Classes (e.g., Phones, Laptops)
    - Products (e.g., iPhone, Samsung Galaxy)

### Flow
1. User visits `/products`  
   - Displays all product classes  

2. User selects a class → `/products/{class}`  
   - Displays sub-classes for that class  

3. User selects a sub-class → `/products/{class}/{subclass}`  
   - Displays products within that sub-class  

4. User selects a product → `/products/{class}/{subclass}/{product}`  
   - Displays dynamically generated product details  

---

## Architecture

### Core Components
- **Template Engine**  
  A predefined WordPress template responsible for rendering all levels of content dynamically.

- **Routing Layer**  
  Maps incoming URLs to the correct data context (class, subclass, or product).

- **Data Layer**  
  Retrieves structured data from the database (custom tables or WordPress post types).

- **Controller Logic**  
  Determines what type of content to render based on the current request.

---

## Data Handling

The system can be implemented using one of the following approaches:

### Option 1: Custom Post Types (Recommended for WordPress-native setup)
- Product Classes → Taxonomies  
- Sub-Classes → Hierarchical Taxonomies  
- Products → Custom Post Type  

### Option 2: Custom Database Tables
- Full control over schema and relationships  
- Suitable for high-performance or complex data models  

---

## URL Structure

The system relies on clean and predictable URLs:

```
/products
/products/{class}
/products/{class}/{subclass}
/products/{class}/{subclass}/{product}
```

Rewrite rules or custom routing logic are required to map these URLs to the template.

---

## Template Behavior

The template dynamically determines what to render based on the request:

- No parameters → show all classes  
- Class parameter → show sub-classes  
- Sub-class parameter → show products  
- Product parameter → show product details  

---

## Content Management

Content can be managed through:

- WordPress admin interface (via custom post types and taxonomies)  
- Custom admin UI for structured data entry  
- External integrations via REST API (optional)  

This enables non-technical users to maintain and update content without modifying templates or code.

---

## Installation (Planned)

1. Install as a WordPress plugin or include in theme  
2. Register custom post types / database schema  
3. Configure rewrite rules  
4. Assign template to the desired route/page  
5. Populate initial data  

---

## Future Enhancements

- Admin UI for managing hierarchical data  
- REST API integration for external systems  
- Caching layer for improved performance  
- SEO optimization (dynamic meta tags, schema markup)  
- Multi-language support  
- Role-based access control for content management  

---

## Design Principles

- Minimal reliance on third-party plugins  
- Clear separation of data, logic, and presentation  
- No vendor lock-in — easily transferable between developers  
- Performance-conscious architecture  
- Maintainability over shortcuts  

---

## Status
Initial project setup. Core architecture and implementation in progress.

---

## License

This project is licensed under the Apache License 2.0.

You are free to use, modify, and distribute this software for both commercial and non-commercial purposes, provided that the terms of the license are respected.

See the LICENSE file for full details.

---

## Attribution

This project was originally developed by Data4Mat AB.

In accordance with the Apache License 2.0:

- The LICENSE file must be included in all distributions
- Any existing copyright notices must be preserved
- The NOTICE file (if present) must be included

While not required, attribution to Data4Mat AB in user-facing applications is appreciated where applicable.