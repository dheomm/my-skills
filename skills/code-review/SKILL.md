---
name: code-review
description: Comprehensive code review skill for analyzing code quality, security vulnerabilities, performance issues, and best practices. Use when users request code review, code analysis, security audit, performance optimization suggestions, or ask to improve/refactor existing code. Triggers include phrases like "review this code", "check my code", "is this code good", "find bugs", "optimize this", or when code files are uploaded for feedback.
---

# Code Review Skill

Systematic code review framework covering quality, security, performance, and maintainability.

## Review Process

### 1. Initial Assessment
- Language and framework identification
- Code purpose and context understanding
- Complexity evaluation

### 2. Multi-Dimensional Analysis

Execute reviews in this order, focusing on issues found:

**Security (CRITICAL)**
- Input validation and sanitization
- Authentication/authorization flaws
- Sensitive data exposure
- Injection vulnerabilities (SQL, XSS, Command)
- Dependency vulnerabilities
- Cryptographic weaknesses

**Correctness**
- Logic errors and edge cases
- Null/undefined handling
- Type safety issues
- Off-by-one errors
- Race conditions

**Performance**
- Algorithm complexity (O(n) analysis)
- Unnecessary loops or operations
- Memory leaks
- Database query optimization (N+1 queries)
- Caching opportunities

**Best Practices**
- Code organization and modularity
- Naming conventions
- Error handling
- Documentation and comments
- DRY (Don't Repeat Yourself)
- SOLID principles adherence

**Maintainability**
- Code readability
- Test coverage
- Magic numbers/strings
- Technical debt

### 3. Output Format

Structure review as:

```markdown
## 🔴 Critical Issues
[Security vulnerabilities and breaking bugs]

## 🟡 Important Improvements
[Performance issues and significant code quality problems]

## 🟢 Suggestions
[Best practices and minor improvements]

## ✅ Strengths
[What the code does well]

## 📝 Refactored Code
[Improved version with fixes applied]
```

## Language-Specific Considerations

**Python**
- PEP 8 compliance
- Type hints usage
- Context managers for resources
- List comprehensions vs loops

**JavaScript/TypeScript**
- async/await vs promises
- TypeScript strict mode
- ESLint rules
- Bundle size implications

**Java**
- Exception handling patterns
- Stream API usage
- Memory management
- Concurrency issues

**Go**
- Error handling idioms
- Goroutine leaks
- Interface design
- defer usage

**Rust**
- Ownership and borrowing
- Unsafe code justification
- Error handling (Result/Option)
- Lifetime annotations

## Review Depth

Adjust detail based on code size:
- **< 50 lines**: Comprehensive line-by-line review
- **50-200 lines**: Focus on critical issues and patterns
- **> 200 lines**: High-level architecture + critical issues only

## Examples

<example>
User: "Review this Python function"
```python
def get_user(id):
    result = db.execute(f"SELECT * FROM users WHERE id = {id}")
    return result[0]
```

Response:
## 🔴 Critical Issues

1. **SQL Injection Vulnerability**: The query uses string formatting with user input, allowing SQL injection attacks
2. **No Error Handling**: IndexError if no results found

## 📝 Refactored Code

```python
def get_user(user_id: int) -> Optional[Dict]:
    """Retrieve user by ID with proper error handling."""
    try:
        result = db.execute(
            "SELECT * FROM users WHERE id = ?", 
            (user_id,)
        )
        return result[0] if result else None
    except DatabaseError as e:
        logger.error(f"Database error fetching user {user_id}: {e}")
        return None
```
</example>

## Best Practices

- Always provide actionable feedback with specific fixes
- Explain WHY issues matter, not just WHAT is wrong
- Balance criticism with recognition of good patterns
- Prioritize security and correctness over style
- Include working refactored code when possible
- Consider the context: production code vs prototype
- Reference official documentation for recommendations
