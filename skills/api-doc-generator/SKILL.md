---
name: api-doc-generator
description: Automatic API documentation generator that creates comprehensive OpenAPI/Swagger specifications, interactive documentation, and code examples. Use when users need to document APIs, create API specs, generate Swagger/OpenAPI docs, or request "document this API", "create API documentation", "make OpenAPI spec", or upload API code asking for documentation.
---

# API Documentation Generator

Automatically generate comprehensive, standards-compliant API documentation from code or descriptions.

## Documentation Formats

### Primary: OpenAPI 3.0 Specification

Generate complete OpenAPI 3.0 YAML/JSON including:
- Endpoint paths and methods
- Request/response schemas
- Authentication requirements
- Error responses
- Example payloads

### Secondary: Interactive Markdown

Human-readable documentation with:
- Quick start guide
- Authentication guide
- Endpoint reference
- Code examples in multiple languages
- Common error codes

## Generation Process

### 1. API Discovery

Extract from code or descriptions:
- **Endpoints**: Methods (GET, POST, etc.) and paths
- **Parameters**: Path, query, header, body parameters
- **Responses**: Success/error status codes and schemas
- **Authentication**: API keys, OAuth, JWT, etc.
- **Data models**: Request/response object structures

### 2. Schema Generation

For each endpoint:

```yaml
paths:
  /users/{id}:
    get:
      summary: Get user by ID
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: integer
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '404':
          description: User not found
      security:
        - bearerAuth: []
```

### 3. Documentation Structure

**OpenAPI Spec (api-spec.yaml)**
```yaml
openapi: 3.0.0
info:
  title: [API Name]
  version: 1.0.0
  description: [API Description]
servers:
  - url: https://api.example.com/v1
paths:
  [Generated paths]
components:
  schemas:
    [Data models]
  securitySchemes:
    [Auth methods]
```

**Interactive Markdown (API-DOCS.md)**
```markdown
# [API Name] Documentation

## Quick Start
[Authentication setup]
[First API call example]

## Authentication
[Detailed auth guide]

## Endpoints

### [Endpoint Name]
- **URL**: `[METHOD] /path`
- **Description**: [What it does]
- **Request**: [Parameters and body]
- **Response**: [Success response]
- **Errors**: [Error codes]
- **Example**:
  ```bash
  curl -X GET "https://api.example.com/users/123" \
    -H "Authorization: Bearer YOUR_TOKEN"
  ```
```

## Language-Specific Extraction

**Python (Flask/FastAPI)**
```python
# Extract from decorators and type hints
@app.route('/users/<int:user_id>', methods=['GET'])
def get_user(user_id: int) -> User:
    """Get user by ID."""
```

**Node.js (Express)**
```javascript
// Extract from route definitions
router.get('/users/:id', async (req, res) => {
  // Implementation
});
```

**Java (Spring Boot)**
```java
// Extract from annotations
@GetMapping("/users/{id}")
public ResponseEntity<User> getUser(@PathVariable Long id) {
    // Implementation
}
```

**Go**
```go
// Extract from handler signatures and comments
// GET /users/{id} - Get user by ID
func GetUser(w http.ResponseWriter, r *http.Request) {
    // Implementation
}
```

## Output Deliverables

1. **OpenAPI Specification** (YAML)
   - Complete, valid OpenAPI 3.0 spec
   - Importable to Swagger UI, Postman, etc.

2. **Markdown Documentation** 
   - Human-readable API guide
   - Code examples in curl, Python, JavaScript, Go

3. **Postman Collection** (JSON) - Optional
   - Ready-to-import Postman collection
   - Pre-configured requests

## Best Practices

### Complete Information
- Include all HTTP methods per endpoint
- Document all possible response codes
- Provide example requests/responses
- Specify required vs optional parameters

### Clear Descriptions
- Use action verbs in summaries ("Get user", "Create order")
- Explain business logic and constraints
- Document rate limits and pagination
- Include deprecation notices if applicable

### Schema Design
- Use $ref for reusable components
- Define clear, typed schemas
- Include field descriptions
- Show enum values for restricted fields

### Examples
- Provide realistic example data
- Show both success and error responses
- Include multiple use cases
- Demonstrate pagination, filtering, sorting

## Example Workflows

<example>
User: "Document this Flask API"
```python
@app.route('/api/users', methods=['POST'])
def create_user():
    data = request.get_json()
    user = User(name=data['name'], email=data['email'])
    db.session.add(user)
    db.session.commit()
    return jsonify(user.to_dict()), 201
```

Output: Generate complete OpenAPI spec with:
- POST /api/users endpoint
- Request schema with name/email fields
- 201 success response with User schema
- 400 bad request for validation errors
- Authentication if detected in code
- Interactive markdown documentation
</example>

<example>
User: "I have a REST API with these endpoints: GET /products, POST /products, GET /products/{id}, PUT /products/{id}, DELETE /products/{id}. Products have id, name, price, and description. Create documentation."

Output: Generate:
1. Full OpenAPI spec with all 5 endpoints
2. Product schema definition
3. Markdown docs with curl examples
4. Optional Postman collection
</example>

## Validation

Ensure generated specs:
- Pass OpenAPI validation (use Swagger Editor)
- Include all required fields
- Have consistent naming conventions
- Follow REST best practices
- Are version-controlled ready

## Interactive Features

When generating Markdown docs, include:
- Table of contents with anchor links
- Collapsible sections for long responses
- Syntax-highlighted code blocks
- Try-it-out curl examples
- Error troubleshooting guide
