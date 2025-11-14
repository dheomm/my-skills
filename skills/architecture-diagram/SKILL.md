---
name: architecture-diagram
description: System architecture visualization tool that creates diagrams using Mermaid syntax following C4 model principles (Context, Container, Component, Code). Use when users request architecture diagrams, system design visualization, "draw architecture", "visualize system", "create diagram", or when discussing system design, microservices, or technical architecture that would benefit from visual representation.
---

# Architecture Diagram Generator

Create comprehensive system architecture diagrams using Mermaid syntax and C4 model principles.

## Diagram Types

### 1. System Context (C4 Level 1)
Shows system and its external dependencies

```mermaid
C4Context
    title System Context Diagram

    Person(user, "User", "End user of the system")
    System(system, "Your System", "Core application")
    System_Ext(external, "External API", "Third-party service")
    
    Rel(user, system, "Uses")
    Rel(system, external, "Calls", "HTTPS")
```

### 2. Container Diagram (C4 Level 2)
Shows high-level technology choices

```mermaid
C4Container
    title Container Diagram

    Person(user, "User")
    
    Container(web, "Web Application", "React", "SPA")
    Container(api, "API", "Node.js", "REST API")
    ContainerDb(db, "Database", "PostgreSQL", "User data")
    Container(cache, "Cache", "Redis", "Session storage")
    
    Rel(user, web, "Uses", "HTTPS")
    Rel(web, api, "Calls", "HTTPS/JSON")
    Rel(api, db, "Reads/Writes", "SQL")
    Rel(api, cache, "Reads/Writes", "Redis Protocol")
```

### 3. Component Diagram (C4 Level 3)
Shows internal components within a container

```mermaid
C4Component
    title API Component Diagram

    Component(controller, "API Controller", "Express", "HTTP endpoints")
    Component(service, "Business Logic", "Service Layer", "Core logic")
    Component(repo, "Repository", "Data Access", "DB queries")
    ComponentDb(db, "Database", "PostgreSQL")
    
    Rel(controller, service, "Calls")
    Rel(service, repo, "Uses")
    Rel(repo, db, "Queries", "SQL")
```

### 4. Sequence Diagram
Shows interaction flow over time

```mermaid
sequenceDiagram
    participant User
    participant Frontend
    participant API
    participant Database
    
    User->>Frontend: Submit form
    Frontend->>API: POST /users
    API->>Database: INSERT user
    Database-->>API: Success
    API-->>Frontend: 201 Created
    Frontend-->>User: Show confirmation
```

### 5. Entity Relationship Diagram
Shows database schema

```mermaid
erDiagram
    USER ||--o{ ORDER : places
    USER {
        int id PK
        string email
        string name
    }
    ORDER ||--|{ ORDER_ITEM : contains
    ORDER {
        int id PK
        int user_id FK
        datetime created_at
    }
    ORDER_ITEM {
        int id PK
        int order_id FK
        int product_id FK
        int quantity
    }
    PRODUCT ||--o{ ORDER_ITEM : "ordered in"
    PRODUCT {
        int id PK
        string name
        decimal price
    }
```

### 6. Deployment Diagram
Shows infrastructure and deployment

```mermaid
graph TB
    subgraph "Cloud Provider"
        subgraph "Load Balancer"
            LB[Load Balancer]
        end
        
        subgraph "Application Servers"
            APP1[App Server 1]
            APP2[App Server 2]
        end
        
        subgraph "Database Cluster"
            DB1[(Primary DB)]
            DB2[(Replica DB)]
        end
        
        subgraph "Cache Layer"
            CACHE[Redis Cluster]
        end
    end
    
    USER[Users] --> LB
    LB --> APP1
    LB --> APP2
    APP1 --> DB1
    APP2 --> DB1
    DB1 -.replicate.-> DB2
    APP1 --> CACHE
    APP2 --> CACHE
```

### 7. Flowchart
Shows process or decision flow

```mermaid
flowchart TD
    Start([User Login]) --> Input[Enter Credentials]
    Input --> Validate{Valid?}
    Validate -->|Yes| CheckMFA{MFA Enabled?}
    Validate -->|No| Error[Show Error]
    CheckMFA -->|Yes| MFA[Request MFA Code]
    CheckMFA -->|No| Success[Login Success]
    MFA --> VerifyMFA{Code Valid?}
    VerifyMFA -->|Yes| Success
    VerifyMFA -->|No| Error
    Error --> Input
    Success --> End([Dashboard])
```

## Generation Process

### 1. Understand Requirements

Ask clarifying questions:
- What system/component needs visualization?
- What level of detail is needed?
- Who is the audience? (Developers, architects, stakeholders)
- What specific aspects to highlight? (Data flow, deployment, interactions)

### 2. Select Appropriate Diagram Type

| Use Case | Diagram Type |
|----------|--------------|
| High-level system overview | System Context |
| Technology stack | Container Diagram |
| Internal architecture | Component Diagram |
| Request/response flow | Sequence Diagram |
| Database design | ER Diagram |
| Infrastructure | Deployment Diagram |
| Business process | Flowchart |

### 3. Create Diagram

Structure with:
- **Clear title**: Describes what the diagram shows
- **Consistent naming**: Use meaningful, consistent names
- **Proper grouping**: Use subgraphs for logical grouping
- **Relationship labels**: Annotate connections with protocols/data
- **Legend** (if needed): Explain symbols or colors

### 4. Provide Context

Accompany diagrams with:
- Written explanation of key components
- Design decisions and rationale
- Technology choices and why
- Known limitations or future improvements
- Links to related diagrams

## Best Practices

### Clarity
- Keep diagrams focused on one aspect
- Limit to 7-10 main components per diagram
- Use consistent direction (top-to-bottom or left-to-right)
- Avoid crossing lines when possible

### Completeness
- Include all critical components
- Show external dependencies
- Document data flow direction
- Specify protocols and data formats

### Hierarchy
- Start with high-level (Context)
- Drill down to detail (Container → Component)
- Create separate diagrams for complex subsystems
- Link related diagrams together

### Annotations
- Label relationships with protocols
- Add notes for important decisions
- Include version numbers
- Date diagrams for historical reference

## Architecture Patterns

### Microservices
```mermaid
graph TB
    API[API Gateway]
    
    subgraph "Services"
        US[User Service]
        OS[Order Service]
        PS[Product Service]
        NS[Notification Service]
    end
    
    subgraph "Data Stores"
        UDB[(User DB)]
        ODB[(Order DB)]
        PDB[(Product DB)]
    end
    
    MSG[Message Queue]
    
    API --> US
    API --> OS
    API --> PS
    US --> UDB
    OS --> ODB
    PS --> PDB
    OS --> MSG
    MSG --> NS
```

### Event-Driven
```mermaid
sequenceDiagram
    participant Order Service
    participant Event Bus
    participant Inventory Service
    participant Notification Service
    
    Order Service->>Event Bus: OrderCreated Event
    Event Bus->>Inventory Service: Consume Event
    Event Bus->>Notification Service: Consume Event
    Inventory Service->>Event Bus: InventoryReserved Event
    Notification Service->>Event Bus: EmailSent Event
```

### Layered Architecture
```mermaid
graph TB
    subgraph "Presentation Layer"
        UI[Web UI]
        API2[REST API]
    end
    
    subgraph "Business Logic Layer"
        SVC[Service Layer]
        DOMAIN[Domain Models]
    end
    
    subgraph "Data Access Layer"
        REPO[Repository]
        ORM[ORM]
    end
    
    subgraph "Infrastructure"
        DB[(Database)]
        CACHE[(Cache)]
    end
    
    UI --> API2
    API2 --> SVC
    SVC --> DOMAIN
    SVC --> REPO
    REPO --> ORM
    ORM --> DB
    SVC --> CACHE
```

## Output Format

For each diagram request, provide:

1. **Mermaid Diagram Code**
   - Properly formatted and renderable
   - Includes title and clear labels

2. **Diagram Explanation**
   - What each component does
   - How components interact
   - Key design decisions

3. **Technology Details** (if applicable)
   - Specific technologies used
   - Communication protocols
   - Data formats

4. **Related Diagrams**
   - Links to complementary views
   - Drill-down diagrams for complex parts

## Examples

<example>
User: "Create an architecture diagram for an e-commerce system with React frontend, Node.js backend, PostgreSQL database, and Redis cache"

Output: Container-level C4 diagram showing:
- User interacting with React SPA
- React calling Node.js REST API
- API connecting to PostgreSQL and Redis
- External payment gateway integration
- Proper protocol labels (HTTPS, SQL, Redis protocol)
- Accompanied by explanation of each component
</example>

<example>
User: "Show me the login flow"

Output: Sequence diagram showing:
- User submitting credentials
- Frontend calling authentication API
- API validating against database
- JWT token generation
- Token returned to frontend
- Frontend storing in localStorage
- Subsequent requests with token
</example>

## Validation

Ensure diagrams:
- Render correctly in Mermaid
- Are not too complex (split if needed)
- Follow consistent styling
- Include all mentioned components
- Show accurate relationships
- Are appropriately detailed for audience

## Advanced Features

### Multiple View Support
Create comprehensive documentation with:
- System Context (external view)
- Container Diagram (technology stack)
- Component Diagram (internal structure)
- Sequence Diagrams (key flows)
- Deployment Diagram (infrastructure)

### Interactive Elements
- Clickable nodes linking to detailed docs
- Collapsible subgraphs for complexity management
- Color coding for component types
- Icons for technology identification
