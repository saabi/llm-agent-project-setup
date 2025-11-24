# Ticket Composition Guidelines

This document outlines best practices for creating and managing tickets in our issue tracking system. Well-written tickets improve clarity, reduce back-and-forth, and help ensure work is completed correctly.

## Table of Contents

1. [Ticket Structure](#ticket-structure)
2. [Ticket Types](#ticket-types)
3. [Title Guidelines](#title-guidelines)
4. [Description Template](#description-template)
5. [Acceptance Criteria](#acceptance-criteria)
6. [Labels and Categorization](#labels-and-categorization)
7. [Priority and Severity](#priority-and-severity)
8. [Dependencies and Relationships](#dependencies-and-relationships)
9. [Examples](#examples)
10. [Common Pitfalls](#common-pitfalls)

---

## Ticket Structure

Every ticket should include:

1. **Clear, descriptive title**
2. **Summary/Overview** - Brief context (2-3 sentences)
3. **Detailed Description** - What, why, and context
4. **Acceptance Criteria** - Measurable conditions for completion
5. **Technical Details** (if applicable) - Implementation notes, constraints
6. **Related Information** - Links, references, screenshots
7. **Labels/Tags** - Categorization for filtering and organization

---

## Ticket Types

### Feature Request

A new capability or enhancement to the system.

**When to use:**
- Adding new functionality
- Enhancing existing features
- User-facing improvements

**Required fields:**
- User story or use case
- Business value/justification
- Mockups or wireframes (if UI-related)
- Dependencies on other features

### Bug Report

A defect or unexpected behavior in the system.

**When to use:**
- Something doesn't work as expected
- Error messages or crashes
- Incorrect data or calculations
- UI/UX issues

**Required fields:**
- Steps to reproduce
- Expected vs. actual behavior
- Environment details (browser, OS, version)
- Error messages or logs
- Screenshots or screen recordings

### Technical Debt

Code quality improvements, refactoring, or infrastructure work.

**When to use:**
- Refactoring existing code
- Improving test coverage
- Updating dependencies
- Performance optimizations
- Code cleanup

**Required fields:**
- Current state description
- Proposed improvement
- Impact assessment
- Risk level

### Documentation

Documentation updates or creation.

**When to use:**
- API documentation
- User guides
- Architecture documentation
- Process documentation

**Required fields:**
- Target audience
- Content outline
- Related code/features

### Task

General work items that don't fit other categories.

**When to use:**
- Configuration changes
- Data migration
- Setup tasks
- Administrative work

---

## Title Guidelines

### Good Titles

- **Specific and descriptive:**
  - ✅ "Add JWT token refresh mechanism to authentication flow"
  - ✅ "Fix null pointer exception in user service when user not found"
  - ✅ "Refactor database query builder to improve testability"

- **Include context:**
  - ✅ "Payment processing: Handle expired credit cards gracefully"
  - ✅ "API: Add pagination to credit listing endpoint"

- **Action-oriented:**
  - ✅ "Implement password reset functionality"
  - ✅ "Add validation for client CUIT field"

### Bad Titles

- ❌ "Bug fix"
- ❌ "Update code"
- ❌ "Issue with payments"
- ❌ "WIP"
- ❌ "asdf"
- ❌ "Fix the thing" (too vague)

### Title Format

```
<Type>: <Brief Description> [<Scope>]
```

Examples:
- `Feature: Add bulk payment import [Payments]`
- `Bug: Payment calculation incorrect for partial payments [Finance]`
- `Refactor: Extract query builder module [Database]`

---

## Description Template

Use this template as a starting point for your tickets:

```markdown
## Summary

[2-3 sentence overview of what this ticket is about]

## Background/Context

[Why is this needed? What problem does it solve?]

## Requirements

[What needs to be done? Be specific and detailed.]

### User Story (if applicable)
As a [user type]
I want [goal/desire]
So that [benefit/value]

## Technical Details

[Implementation notes, technical constraints, architecture decisions]

## Acceptance Criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Additional Information

- Related tickets: #[123], #[456]
- Dependencies: #[789]
- Blocked by: #[101]
- Screenshots: [links]
- Mockups: [links]
- Documentation: [links]
```

---

## Acceptance Criteria

Acceptance criteria define the conditions that must be met for a ticket to be considered complete. They should be:

### Characteristics of Good Acceptance Criteria

1. **Specific and measurable:**
   - ✅ "User can reset password by clicking 'Forgot Password' link"
   - ❌ "Password reset should work"

2. **Testable:**
   - ✅ "System sends email with reset link within 60 seconds"
   - ❌ "Email should be sent quickly"

3. **User-focused:**
   - ✅ "User receives confirmation message after successful password reset"
   - ❌ "Backend should return 200 status code"

4. **Complete:**
   - Cover all scenarios (happy path, edge cases, error cases)
   - Include validation requirements
   - Specify error handling

### Example Acceptance Criteria

**Feature: Password Reset**

- [ ] User can request password reset from login page
- [ ] System sends email with reset link to registered email address
- [ ] Reset link expires after 24 hours
- [ ] User can set new password using valid reset link
- [ ] System validates new password meets requirements (min 8 chars, etc.)
- [ ] User receives confirmation email after successful password reset
- [ ] Invalid or expired reset links show appropriate error message
- [ ] User cannot reuse previous password
- [ ] Password reset link can only be used once

---

## Labels and Categorization

Use labels to categorize and filter tickets effectively.

### Suggested Label Categories

**By Component/Area:**
- `auth` - Authentication and authorization
- `api` - API endpoints and services
- `ui` - User interface components
- `db` - Database and data layer
- `migration` - Migration-related work
- `config` - Configuration and setup
- `docs` - Documentation

**By Type:**
- `feature` - New features
- `bug` - Bug fixes
- `refactor` - Code refactoring
- `technical-debt` - Technical debt
- `documentation` - Documentation updates

**By Priority:**
- `priority:critical` - Blocks other work or production
- `priority:high` - Important, should be addressed soon
- `priority:medium` - Normal priority
- `priority:low` - Nice to have, can wait

**By Status:**
- `needs-triage` - Requires review/prioritization
- `needs-design` - Requires design work
- `needs-approval` - Requires stakeholder approval
- `blocked` - Blocked by other work
- `ready-for-review` - Ready for code review

**By Effort (optional):**
- `effort:small` - < 1 day
- `effort:medium` - 1-3 days
- `effort:large` - 3-5 days
- `effort:xlarge` - > 5 days (consider breaking down)

---

## Priority and Severity

### Priority Levels

**Critical (P0)**
- Production system down or severely impacted
- Security vulnerabilities
- Data loss or corruption
- Blocks all other work

**High (P1)**
- Major feature broken
- Significant user impact
- Blocks specific workflows
- Performance issues affecting many users

**Medium (P2)**
- Minor feature issues
- Workarounds available
- Affects some users
- Non-critical bugs

**Low (P3)**
- Cosmetic issues
- Nice-to-have improvements
- Edge cases with minimal impact
- Documentation improvements

### Severity Levels

**Severity** describes the impact of a bug, while **Priority** describes how urgently it should be addressed.

**Severity: Critical**
- System crash or data loss
- Complete feature failure
- Security breach

**Severity: High**
- Major feature partially broken
- Significant data issues
- Performance degradation

**Severity: Medium**
- Minor feature issues
- Partial functionality loss
- Workarounds exist

**Severity: Low**
- Cosmetic issues
- Minor inconveniences
- Edge cases

---

## Dependencies and Relationships

### Linking Related Tickets

**Blocks / Blocked By:**
- Use when one ticket must be completed before another can start
- Example: "Database migration" blocks "New feature using migrated schema"

**Relates To:**
- Use for tickets that are related but not dependent
- Example: "API endpoint" relates to "Frontend component using API"

**Duplicates:**
- Use when multiple tickets describe the same issue
- Close duplicates and link to the primary ticket

**Part Of / Epic:**
- Use for grouping related tickets
- Example: "User authentication" epic contains multiple tickets

### Example Dependency Chain

```
Epic: User Authentication
├── Ticket #1: Database schema for users (blocks #2, #3)
├── Ticket #2: User registration API (blocked by #1, blocks #4)
├── Ticket #3: User login API (blocked by #1, blocks #4)
└── Ticket #4: Login UI component (blocked by #2, #3)
```

---

## Examples

### Example 1: Feature Request

**Title:** `Feature: Add bulk payment import functionality [Payments]`

**Labels:** `feature`, `payments`, `priority:high`, `effort:medium`

**Description:**

```markdown
## Summary

Add ability to import multiple payments at once via CSV file upload. This will
significantly reduce time spent on data entry for large batches of payments.

## Background/Context

Currently, payments must be entered one at a time through the UI. For clients
with many payments per month, this is time-consuming and error-prone. A bulk
import feature would improve efficiency and reduce manual errors.

## Requirements

- User can upload a CSV file containing payment data
- System validates CSV format and data
- User can preview imported payments before confirming
- System processes payments in batch
- User receives summary of successful and failed imports
- Failed imports show specific error messages

### User Story
As a finance administrator
I want to import multiple payments from a CSV file
So that I can process monthly payment batches efficiently

## Technical Details

- CSV format: `credit_id,amount,payment_date,reference_number`
- Maximum file size: 10MB
- Maximum rows per import: 1000
- Validation: Check credit exists, amount is positive, date is valid
- Use database transaction for batch processing
- Create audit log entry for each import

## Acceptance Criteria

- [ ] User can navigate to "Bulk Import" page from payments menu
- [ ] User can select and upload CSV file
- [ ] System validates CSV format (headers, data types)
- [ ] System displays preview table with parsed data
- [ ] Preview shows validation errors for invalid rows
- [ ] User can confirm or cancel import
- [ ] System processes valid payments in single transaction
- [ ] User sees success message with count of imported payments
- [ ] Failed rows are displayed with specific error messages
- [ ] User can download error report for failed rows
- [ ] Import is logged in audit trail
- [ ] CSV template can be downloaded from import page

## Additional Information

- Related tickets: #[123] (Payment validation improvements)
- Dependencies: #[456] (CSV parsing utility)
- Mockups: [link to Figma]
- CSV format specification: [link to docs]
```

### Example 2: Bug Report

**Title:** `Bug: Payment calculation incorrect for partial payments [Finance]`

**Labels:** `bug`, `finance`, `priority:high`, `severity:high`

**Description:**

```markdown
## Summary

When recording a partial payment, the system incorrectly calculates the
remaining balance, resulting in negative balances in some cases.

## Background/Context

Users have reported that after recording partial payments, the credit balance
shows incorrect values. This affects financial reporting and client statements.

## Steps to Reproduce

1. Navigate to a credit with balance of $10,000
2. Record a payment of $3,000
3. View the credit details
4. Observe the remaining balance

**Expected Behavior:**
- Remaining balance should be $7,000

**Actual Behavior:**
- Remaining balance shows $13,000 (incorrect calculation)

## Environment

- Browser: Chrome 120.0.6099.109
- OS: Windows 11
- User Role: Finance Manager
- Credit ID: 12345 (for testing)

## Error Details

No error messages displayed, but calculation is incorrect.

## Additional Information

- First reported: 2024-01-15
- Affected credits: All credits with partial payments
- Related tickets: #[789] (Payment calculation refactor)
- Screenshots: [link to screenshots]
```

### Example 3: Technical Debt

**Title:** `Refactor: Extract database query builder to separate module [Database]`

**Labels:** `refactor`, `technical-debt`, `database`, `priority:medium`, `effort:small`

**Description:**

```markdown
## Summary

Extract the database query builder logic into a separate, reusable module to
improve code organization and testability.

## Background/Context

Query building logic is currently scattered across multiple service files,
making it difficult to test and maintain. Centralizing this logic will improve
code quality and reduce duplication.

## Current State

- Query building code exists in 5+ service files
- Similar query patterns repeated across files
- Difficult to unit test query building logic
- Changes to query structure require updates in multiple places

## Proposed Solution

- Create `src/lib/db/query-builder.ts` module
- Extract common query building patterns
- Update services to use new query builder
- Add unit tests for query builder

## Impact Assessment

**Benefits:**
- Improved code organization
- Better testability
- Reduced code duplication
- Easier to maintain and extend

**Risks:**
- Low risk - refactoring existing code
- Requires updating multiple files
- Need to ensure no functional changes

## Acceptance Criteria

- [ ] Query builder module created with common patterns
- [ ] All services updated to use new query builder
- [ ] Unit tests added for query builder (>80% coverage)
- [ ] Integration tests pass
- [ ] No functional changes to existing behavior
- [ ] Code review completed

## Additional Information

- Related tickets: #[234] (Database layer improvements)
- Estimated effort: 2-3 days
```

---

## Common Pitfalls

### ❌ Avoid These Mistakes

1. **Vague or unclear descriptions**
   - ❌ "Fix the bug"
   - ✅ "Fix null pointer exception when user ID is missing"

2. **Missing context**
   - ❌ "Add new field"
   - ✅ "Add 'phone_number' field to user registration form for SMS notifications"

3. **No acceptance criteria**
   - ❌ "Implement feature X"
   - ✅ Include specific, testable acceptance criteria

4. **Too large or too small**
   - ❌ "Build entire authentication system" (too large - break down)
   - ❌ "Change button color" (too small - combine with related work)

5. **Missing reproduction steps for bugs**
   - ❌ "Payment doesn't work"
   - ✅ Include step-by-step reproduction instructions

6. **No links or references**
   - ❌ "Related to that other ticket"
   - ✅ Link to related tickets, documentation, or resources

7. **Unclear priority**
   - ❌ "This is urgent" (without context)
   - ✅ Explain why it's urgent and what the impact is

8. **Missing technical details**
   - ❌ "Add API endpoint"
   - ✅ Include request/response formats, authentication, error handling

### ✅ Best Practices

1. **Be specific and detailed**
2. **Include context and background**
3. **Write clear acceptance criteria**
4. **Link related tickets and resources**
5. **Add screenshots, mockups, or examples**
6. **Keep tickets focused (one thing per ticket)**
7. **Update tickets as work progresses**
8. **Close tickets with clear resolution notes**

---

## Ticket Lifecycle

### States

1. **Open/New** - Ticket created, awaiting triage
2. **Triage** - Being reviewed and prioritized
3. **In Progress** - Work has started
4. **In Review** - Code/implementation review
5. **Testing** - Being tested
6. **Done/Closed** - Completed and verified
7. **Blocked** - Cannot proceed due to dependency
8. **Won't Fix** - Decided not to implement

### When to Update Tickets

- **When starting work:** Move to "In Progress", add estimate
- **When blocked:** Move to "Blocked", explain why
- **When unblocked:** Move back to appropriate state
- **When scope changes:** Update description and acceptance criteria
- **When completed:** Add resolution notes, link to PR/commit

---

## Additional Resources

- [How to Write a Good Bug Report](https://www.chiark.greenend.org.uk/~sgtatham/bugs.html)
- [User Story Mapping](https://www.jpattonassociates.com/user-story-mapping/)
- [Acceptance Criteria Best Practices](https://www.agilealliance.org/glossary/acceptance-criteria/)

---

## Questions?

If you're unsure about how to write a ticket, ask a team member or refer to existing tickets in the repository for examples.

