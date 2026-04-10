# Data4Mat Dynamic Content Template Engine for WordPress

## Purpose

This project implements a structured content engine inside WordPress that separates:

- Data (custom SQL schema)
- Business logic (plugin services)
- Presentation (block-based templates)

The system allows developers and editors to define structured content types and render them using dynamically populated block-based templates.

This is not a traditional WordPress content model. WordPress is used primarily as:

- Editor (Gutenberg)
- Template storage (block content)
- Runtime environment

All structured data is managed by the plugin.

---

## Architectural Overview

The system is built as a layered architecture:
<code>
Admin UI (WordPress)
│
▼
Block-Based Templates (Gutenberg)
│
▼
Rendering Layer (Context + Template Engine)
│
▼
Application / Domain Layer (Services)
│
▼
Data Access Layer (Repositories)
│
▼
Database (Custom Tables)
</code>


Each layer has a strict responsibility and must not leak concerns across boundaries.

---

## Core Concepts

### Content Type
Defines a structured entity (e.g. Product).

### Field
Defines a property of a content type.

### Record
An instance of a content type.

### Template
A block-based layout stored as a WordPress post (`d4m_template`).

### Render Context
The runtime container holding the current record and its resolved values.

### Block
A dynamic Gutenberg block that reads data from the render context.

---

## Data Model (V1)

Custom tables:

- `wp_d4m_content_types`
- `wp_d4m_fields`
- `wp_d4m_records`
- `wp_d4m_record_values`
- `wp_d4m_template_assignments`

### Notes

- EAV-style storage is used for flexibility
- Schema is controlled entirely by the plugin
- No reliance on `wp_posts` for structured data

---

## Module Structure

src/
Core/
Database/
Domain/
Repository/
Service/
Rendering/
Routing/
Blocks/
Admin/
Support/

---

### Responsibilities

#### Core
- Plugin bootstrap
- Service container / dependency wiring

#### Database
- Migrations
- Schema management

#### Domain
- Entity definitions
- Business models

#### Repository
- All database access
- No business logic

#### Service
- Application logic
- Orchestration
- Validation

#### Rendering
- RenderContext
- TemplateRenderer
- Value resolution

#### Routing
- URL mapping
- Request resolution

#### Blocks
- Gutenberg block registration
- Server-side rendering callbacks

#### Admin
- WP admin pages
- Data management UI

#### Support
- Utilities (validation, sanitization, helpers)

---

## Rendering Pipeline
Request → Route Resolver → Record Lookup → Template Lookup
→ RenderContext निर्माण → Block Rendering → HTML Output

---


### Steps

1. Resolve request to a content type + record (via slug or ID)
2. Load record from repository
3. Resolve assigned template
4. Build RenderContext
5. Render template (block content)
6. Dynamic blocks resolve values via context
7. Output final HTML

---

## Block System

Blocks are the only layer allowed to interact with templates.

### Rules

- Blocks must NOT query the database directly
- Blocks must use services (via context) to access data
- Blocks must be stateless
- All data comes from RenderContext

### V1 Blocks

- Record Title
- Record Field
- Record Image
- Record Rich Text

Each block:
- Accepts configuration (field key, format, etc.)
- Resolves value through FieldValueService
- Outputs sanitized HTML

---

## Template System

- Templates are stored as `d4m_template` post type
- Built using Gutenberg editor
- Assigned per content type

### Constraints

- Templates define structure only
- No hardcoded content
- All dynamic values must come from blocks

---

## Routing

V1 uses explicit routing:
/products/{slug}/


Routing layer must:

- Parse request
- Resolve content type
- Resolve record
- Pass control to rendering pipeline

No reliance on default WP template hierarchy.

---

## Admin Responsibilities

Admin UI must support:

- Content type creation
- Field definition
- Record CRUD
- Template assignment

Admin UI must NOT contain business logic.

---

## Design Constraints

- Strict separation of concerns
- No direct SQL outside repositories
- No rendering logic in services
- No business logic in blocks
- No dependency on WP post meta for core data
- No cross-layer shortcuts

---

## Scope (V1)

Implemented:

- Single content type (Product)
- Basic field types
- Core dynamic blocks
- Template assignment
- End-to-end rendering pipeline

Not included (yet):

- Relationships
- Repeaters
- Conditional blocks
- Query blocks
- Multi-template inheritance
- API layer

---

## Development Order

1. Database schema + migrations
2. Content type + field system
3. Record CRUD
4. Template post type
5. RenderContext + renderer
6. Basic blocks
7. Routing
8. Template assignment

Do not change this order.

---

## Future Work

- Multiple content types
- Field types expansion
- Relationships and joins
- Conditional rendering blocks
- Query/listing blocks
- REST API layer
- Caching strategy
- Performance tuning

---

## Status

Architecture defined.  
Implementation in progress.

---

## License

Apache License 2.0.

---

## Attribution

Developed by Data4Mat AB.