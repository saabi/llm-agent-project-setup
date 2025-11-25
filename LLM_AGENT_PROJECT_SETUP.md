# LLM Agent-Friendly Project Setup Guide

**Based on:** Iterative development across multiple projects, including the first migration project for LLM agent assistance (Classic ASP to SvelteKit + PostgreSQL)  
**Purpose:** Guide for structuring projects to maximize LLM agent effectiveness  
**Target Audience:** Project maintainers, technical leads, LLM agent operators

---

## Table of Contents

1. [Overview](#overview)
2. [Core Principles](#core-principles)
3. [Project Structure](#project-structure)
4. [Documentation Organization](#documentation-organization)
5. [New Project Setup](#new-project-setup)
6. [Legacy System Migration Setup](#legacy-system-migration-setup)
7. [Project Type Variations](#project-type-variations)
8. [LLM Agent Best Practices](#llm-agent-best-practices)
9. [Version Control Strategy](#version-control-strategy)
10. [Branch Management Strategies](#branch-management-strategies)
11. [Quality Assurance](#quality-assurance)
12. [Examples and Templates](#examples-and-templates)

**For Existing Projects:** See [LLM Agent Project Migration Guide](./LLM_AGENT_PROJECT_MIGRATION.md) for migrating existing projects to this structure.

---

## Overview

This guide documents best practices for structuring software projects to maximize effectiveness when working with LLM agents. The ideas and patterns presented here were developed iteratively across multiple projects, including the first migration project for LLM agent assistance (from Classic ASP to SvelteKit + PostgreSQL). The principles apply regardless of technology stack, though examples may reference SvelteKit, Next.js, or similar full-stack frameworks.

### Why This Matters

LLM agents excel when:
- **Context is clear:** Well-organized documentation with clear cross-references
- **Decisions are documented:** Rationale and alternatives are explained
- **Structure is consistent:** Predictable patterns across the codebase
- **Dependencies are explicit:** Clear relationships between components
- **History is preserved:** Git history tells a coherent story

### What This Guide Covers

- **New Projects:** Setting up greenfield projects for LLM agent collaboration
- **Legacy Migrations:** Documenting and migrating existing systems
- **Project Types:** Adaptations for libraries, websites, utilities
- **Best Practices:** Patterns that improve LLM agent effectiveness

---

## Core Principles

### 1. Explicit Over Implicit

**Bad:** "The schema follows standard patterns"  
**Good:** "All tenant tables include `team_id UUID NOT NULL REFERENCES teams(id)`. See [Multi-Tenancy Architecture](./docs/specs/MULTI_TENANCY.md) (or `./docs/new-system/...`) for details."

### 2. Document Decisions, Not Just Results

**Bad:** "We use PostgreSQL"  
**Good:** "We use PostgreSQL because: (1) Row-Level Security for multi-tenancy, (2) JSONB support for flexible schemas, (3) Strong ecosystem. See [Database Selection Analysis](./docs/decisions/database-selection.md)."

### 3. Cross-Reference Everything

**Bad:** Standalone documents  
**Good:** Documents that reference related docs, tickets, and code locations

### 4. Maintain Consistency

**Bad:** Mixed naming conventions, inconsistent patterns  
**Good:** Documented conventions enforced across the codebase

### 5. Preserve Context

**Bad:** "Fixed bug" commit message  
**Good:** "fix(auth): correct RLS policy syntax to include transaction-local parameter

The `current_setting()` call was missing the `true` parameter, which makes
the setting session-local instead of transaction-local. This could cause
security issues in concurrent transactions.

See: docs/tickets/phase-1/T-002B-tenant-schema-definition.md
Fixes: Issue #42"

---

## Project Structure

### Recommended Directory Layout

**Note:** Choose between single-repo or monorepo structure based on project needs. See [Monorepo vs Single-Repo](#monorepo-vs-single-repo) section below.

**Directory Naming:** For new system documentation, use `docs/specs/` for new projects (recommended) or `docs/new-system/` in migrations (when `old-system/` exists). Both names are acceptable and work equally well.

#### Option 1: Single-Repo Structure

Use for simple projects with a single application or service.

```
project-root/
├── docs/                          # All documentation
│   ├── old-system/                # Legacy system docs (if migrating)
│   ├── specs/                     # New system specs (recommended for new projects)
│   │   # OR new-system/            # Alternative name (also acceptable)
│   ├── migration/                 # Migration-specific docs
│   ├── tickets/                   # Project tickets
│   └── process/                   # Process documentation
├── src/                           # Source code
├── tests/                         # Test files
├── scripts/                       # Utility scripts
├── .cursorrules                   # Cursor/LLM agent rules
├── README.md                      # Project overview
└── CONTRIBUTING.md                # Contribution guidelines
```

#### Option 2: Monorepo Structure

Use for projects with multiple applications, services, or systems that share code or documentation.

**Pattern A: Full-Stack Application (Recommended for SvelteKit, Next.js, etc.)**
```
project-root/
├── docs/                          # Shared documentation
│   ├── old-system/                # Legacy system docs (if migrating)
│   ├── specs/                     # New system specs (recommended for new projects)
│   │   # OR new-system/            # Alternative name (also acceptable, especially in migrations)
│   ├── migration/                 # Migration-specific docs
│   ├── tickets/                   # Project tickets
│   └── process/                   # Process documentation
├── app/                           # Main full-stack application
│   ├── src/
│   │   ├── lib/
│   │   │   ├── components/        # Frontend components
│   │   │   └── server/            # Backend business layer & API
│   │   └── routes/                # SvelteKit/Next.js routes
│   ├── tests/
│   ├── package.json
│   └── README.md
├── admin/                         # Admin panel (optional separate app)
│   ├── src/
│   ├── tests/
│   └── package.json
├── shared/                        # Shared code/libraries
│   ├── types/
│   ├── utils/
│   └── package.json
├── scripts/                       # Shared utility scripts
│   └── migrate/                   # Migration scripts
├── .cursorrules                   # Cursor/LLM agent rules
├── README.md                      # Project overview
├── package.json                   # Root package.json (workspace config)
└── CONTRIBUTING.md                # Contribution guidelines
```

**Pattern B: Separate Frontend and Backend**
```
project-root/
├── docs/                          # Shared documentation
├── frontend/                      # Frontend application (React, Vue, etc.)
│   ├── src/
│   ├── tests/
│   ├── package.json
│   └── README.md
├── backend/                       # Backend API service
│   ├── src/
│   │   ├── routes/                # API routes
│   │   ├── services/              # Business logic
│   │   └── db/                    # Database layer
│   ├── tests/
│   ├── package.json
│   └── README.md
├── shared/                        # Shared code/libraries
│   ├── types/                     # Shared TypeScript types
│   └── utils/
├── scripts/
└── package.json                   # Workspace config
```

**Pattern C: Multiple Services (Microservices-style)**
```
project-root/
├── docs/
├── api/                           # Main API service
│   ├── src/
│   └── package.json
├── auth-service/                  # Authentication service
│   ├── src/
│   └── package.json
├── payment-service/               # Payment processing service
│   ├── src/
│   └── package.json
├── frontend/                      # Frontend application
│   ├── src/
│   └── package.json
├── shared/                        # Shared libraries
└── package.json
```

**Real-World Example (Full-Stack Application - Pattern A):**
```
project-name/
├── docs/                          # All documentation
│   ├── old-system/                # Legacy system docs (if migrating)
│   ├── specs/                     # New system specs (recommended for new projects)
│   │   # OR new-system/            # Alternative name (also acceptable, especially in migrations)
│   ├── migration/                 # Migration-specific docs
│   ├── tickets/                   # Project tickets
│   └── process/                   # Process documentation
├── app/                           # Full-stack application (SvelteKit, Next.js, etc.)
│   ├── src/
│   │   ├── lib/
│   │   │   ├── components/        # Frontend components
│   │   │   └── server/            # Backend business layer & API
│   │   ├── routes/                # Framework routes (pages + API)
│   │   └── app.d.ts               # TypeScript definitions
│   ├── tests/
│   ├── package.json
│   └── README.md
└── README.md
```

**Note:** 
- **Pattern A** (`app/`) is best for full-stack frameworks like SvelteKit, Next.js, Remix where frontend and backend are tightly integrated
- **Pattern B** (`frontend/` + `backend/`) is best when frontend and backend are separate applications (e.g., React SPA + Express API)
- **Pattern C** is for microservices architectures with multiple backend services
- Directory names should describe the primary purpose, even if they contain mixed code (e.g., `app/` contains both frontend and backend in a full-stack framework)

### Key Directories Explained

**`docs/`** (Root Documentation)
- **Location:** Always at project root, even in monorepos
- **Purpose:** Shared documentation across all apps/systems
- **Structure:** Organized by system (old/new), migration, tickets, process
- **Naming:** Use `specs/` for new projects (recommended), or `new-system/` for migrations (when `old-system/` exists)

**`docs/old-system/`** (Migration Projects Only)
- Complete specification of legacy system
- Schema analysis and documentation
- Feature catalog and business rules
- Unused code/object identification

**`docs/new-system/`** or **`docs/specs/`** (New System Specifications)
- Feature specifications
- Technical architecture
- Design decisions and rationale
- Naming conventions and standards
- **Note:** For new projects, `specs` is recommended and perfectly acceptable. For migration projects, either name works as long as old system docs are in `old-system/`.

**`docs/migration/`**
- Migration plan and strategy
- Comparison documents (old vs new)
- Legacy code snapshots for reference
- Migration scripts and tools

**`docs/tickets/`**
- Prioritized ticket lists by phase
- Individual ticket files with full details
- Dependency analysis documents
- Schema dependency tracking

**`docs/process/`**
- Commit message guidelines
- Ticket creation templates
- Code review process
- Development workflow

**Application Directories** (Monorepo)
- **`app/`, `frontend/`, `backend/`, `api/`, `admin/`, etc.:** Each application/service has its own directory
- **Structure:** Each app directory contains its own `src/`, `tests/`, `package.json`
- **Documentation:** App-specific docs can live in the app directory, but shared docs stay in root `docs/`
- **Naming Guidelines:**
  - **`app/`** - Full-stack application (SvelteKit, Next.js, Remix)
  - **`frontend/`** - Frontend-only application (React SPA, Vue app)
  - **`backend/`** or **`api/`** - Backend API service
  - **`admin/`** - Admin panel application
  - Use descriptive names that reflect primary purpose, even if they contain mixed code (e.g., `app/` contains both frontend and backend in a full-stack framework)

---

## Documentation Organization

### Documentation Hierarchy

1. **Executive/Overview Documents**
   - `README.md` - Project overview, quick start
   - `docs/migration/plan.md` - Migration strategy (if applicable)
   - `docs/specs/feature-specification.md` (or `docs/new-system/...`) - Complete feature catalog

2. **Architecture Documents**
   - `docs/specs/technical-architecture.md` (or `docs/new-system/...`) - System design
   - `docs/specs/MULTI_TENANCY.md` (or `docs/new-system/...`) - Architectural decisions
   - `docs/specs/BUSSINESS_LAYER.md` (or `docs/new-system/...`) - Business logic organization

3. **Analysis Documents**
   - `docs/migration/comparison/legacy-vs-new-differences.md` - Feature comparison
   - `docs/tickets/SCHEMA_DEPENDENCY_ANALYSIS.md` - Dependency tracking
   - `docs/CONSISTENCY_REVIEW.md` - Documentation consistency

4. **Process Documents**
   - `docs/process/COMMITS.md` - Commit message guidelines
   - `docs/process/TICKETS.md` - Ticket creation guidelines
   - `docs/process/CODE_REVIEW.md` - Review process

5. **Reference Documents**
   - `docs/specs/DEVELOPMENT_SETUP.md` (or `docs/new-system/...`) - Developer onboarding
   - `docs/specs/NAMING_CONVENTIONS.md` (or `docs/new-system/...`) - Coding standards
   - `docs/tickets/TICKET_CREATION_GUIDE.md` - Ticket templates

### Documentation Patterns

#### Pattern 1: Specification Documents

**Structure:**
```markdown
# Document Title

**Related Documents:**
- [Related Doc 1](./path/to/doc1.md)
- [Related Doc 2](./path/to/doc2.md)

---

## Table of Contents

1. [Section 1](#section-1)
2. [Section 2](#section-2)

---

## Section 1

Content with code examples, references, and cross-links.

**References:**
- [Implementation](./path/to/implementation.md)
- [Ticket T-001](./tickets/phase-1/T-001-task.md)
```

#### Pattern 2: Analysis Documents

**Structure:**
```markdown
# Analysis: [Topic]

**Date:** YYYY-MM-DD  
**Status:** Analysis Complete / In Progress

## Problem Statement

Clear description of what is being analyzed.

## Analysis

Detailed analysis with:
- Current state
- Issues found
- Recommendations

## Recommendations

1. **Recommendation 1**
   - Rationale
   - Impact
   - Implementation steps

## Action Items

- [ ] Action 1
- [ ] Action 2
```

#### Pattern 3: Decision Documents

**Structure:**
```markdown
# Decision: [Topic]

**Date:** YYYY-MM-DD  
**Status:** Approved / Pending

## Context

Why this decision is needed.

## Options Considered

### Option 1: [Name]
- Pros
- Cons
- Implementation complexity

### Option 2: [Name]
- Pros
- Cons
- Implementation complexity

## Decision

**Chosen:** Option X

**Rationale:**
- Reason 1
- Reason 2

## Implementation

Steps to implement the decision.

## References

- [Related Analysis](./analysis-doc.md)
- [Related Ticket](./tickets/T-XXX.md)
```

---

## New Project Setup

### Phase 1: Initial Structure

1. **Decide: Single-Repo or Monorepo?**
   - See [Monorepo vs Single-Repo](#monorepo-vs-single-repo) section
   - Start simple (single-repo) if unsure, can migrate later

2. **Create Documentation Skeleton**
   ```
   docs/                          # Always at root
   ├── specs/                     # Recommended for new projects
   │   # OR new-system/            # Also acceptable
   │   ├── feature-specification.md
   │   ├── technical-architecture.md
   │   └── development-setup.md
   ├── process/
   │   ├── COMMITS.md
   │   └── TICKETS.md
   └── tickets/
       └── README.md
   ```

3. **Create Application Structure**
   
   **Single-Repo:**
   ```
   src/
   tests/
   package.json
   ```
   
   **Monorepo (Full-Stack):**
   ```
   app/                   # Full-stack application (SvelteKit, Next.js)
   ├── src/
   ├── tests/
   └── package.json
   
   # Optional: Add more apps later
   # admin/
   # shared/
   
   package.json           # Root workspace config
   ```
   
   **Monorepo (Separate Frontend/Backend):**
   ```
   frontend/              # Frontend application
   ├── src/
   └── package.json
   
   backend/               # Backend API
   ├── src/
   └── package.json
   
   shared/                # Shared code
   └── package.json
   
   package.json           # Root workspace config
   ```

2. **Create Core Documents**
   - `README.md` - Project overview
   - `docs/specs/feature-specification.md` (or `docs/new-system/...`) - Feature catalog
   - `docs/specs/technical-architecture.md` (or `docs/new-system/...`) - System design
   - `docs/specs/development-setup.md` (or `docs/new-system/...`) - Developer guide
   - `docs/process/COMMITS.md` - Commit guidelines
   - `docs/process/TICKETS.md` - Ticket guidelines
   
   **Note:** Use `specs/` for new projects (recommended), or `new-system/` if you prefer. Both are acceptable.

3. **Set Up Version Control**
   - Initialize git repository
   - Create `.gitignore`
   - Set up branch protection (if using GitHub/GitLab)
   - Document branching strategy

4. **Create Cursor Rules** (`.cursorrules`)
   ```markdown
   # Project Rules for LLM Agents

   ## Documentation Standards
   - Always reference related documents
   - Use code references for existing code
   - Document decisions, not just results

   ## Commit Guidelines
   - Follow Conventional Commits
   - Reference tickets in commit messages
   - See: docs/process/COMMITS.md

   ## Code Standards
   - Follow naming conventions in docs/specs/NAMING_CONVENTIONS.md (or docs/new-system/...)
   - Write tests for new features
   - Update documentation when changing behavior
   ```

### Phase 2: Feature Specification

1. **Define Features**
   - List all features with descriptions
   - Categorize (core vs optional)
   - Define dependencies

2. **Document Data Models**
   - Database schema (if applicable)
   - API data structures
   - Type definitions

3. **Define Business Rules**
   - Validation rules
   - Business logic constraints
   - Permission requirements

### Phase 3: Technical Architecture

1. **Technology Stack**
   - Document chosen technologies
   - Explain rationale for each choice
   - Version requirements

2. **System Architecture**
   - High-level architecture diagram
   - Component relationships
   - Data flow

3. **Design Patterns**
   - Patterns used in the project
   - When to use each pattern
   - Examples

### Phase 4: Development Setup

1. **Environment Setup**
   - Prerequisites
   - Installation steps
   - Configuration

2. **Development Workflow**
   - Branching strategy
   - Testing approach
   - Code review process

3. **Naming Conventions**
   - Database naming
   - Code naming
   - File naming
   - API naming

---

## Legacy System Migration Setup

### Phase 1: Legacy System Analysis

1. **Document Legacy System**
   ```
   docs/old-system/
   ├── implementation-specification.md  # Complete system spec
   ├── schema-analysis.md               # Database schema
   ├── feature-catalog.md              # All features
   └── business-rules.md                # Business logic
   ```
   
   **Note:** When migrating, you can use either `docs/specs/` or `docs/new-system/` for the new system documentation. Both are acceptable as long as the old system docs are in `old-system/`.

2. **Create Legacy Code Snapshot**
   ```
   docs/migration/old_versions/
   ├── legacy-code/                     # Source code snapshot
   ├── legacy-schema.sql                # Database schema
   └── legacy-config/                   # Configuration files
   ```

3. **Identify Unused Code**
   - Document unused tables/views/procedures
   - Identify dead code paths
   - Note deprecated features

### Phase 2: Comparison and Gap Analysis

1. **Feature Comparison**
   ```
   docs/migration/comparison/
   ├── legacy-vs-new-differences.md     # Feature differences
   ├── schema-mapping.md                # Field mappings
   └── business-rule-changes.md         # Rule changes
   ```

2. **Gap Analysis**
   - Missing features in legacy
   - New features in target
   - Breaking changes

3. **Migration Strategy**
   - Phased approach
   - Data migration plan
   - Rollback strategy

### Phase 3: Migration Planning

1. **Migration Plan Document**
   - Phases and milestones
   - Dependencies
   - Risk mitigation

2. **Schema Dependency Analysis**
   - Table dependencies
   - Migration order
   - Critical path identification

3. **Ticket Creation**
   - Prioritized ticket list
   - Individual ticket files
   - Dependency tracking

### Phase 4: Consistency Review

1. **Documentation Review**
   - Cross-reference verification
   - Naming consistency
   - Terminology standardization

2. **Schema Review**
   - Table name consistency
   - Field name consistency
   - Type consistency

3. **Code Review Preparation**
   - Coding standards documented
   - Review checklist
   - Quality gates

---

## Project Type Variations

### Libraries

**Additional Documentation:**
- `docs/API.md` - Public API documentation
- `docs/EXAMPLES.md` - Usage examples
- `docs/CONTRIBUTING.md` - Contribution guidelines
- `docs/CHANGELOG.md` - Version history

**Key Differences:**
- Focus on API stability
- Versioning strategy
- Backward compatibility
- Public vs private APIs

**Example Structure:**
```
lib-project/
├── docs/
│   ├── API.md
│   ├── EXAMPLES.md
│   ├── MIGRATION_GUIDES/
│   │   ├── v1-to-v2.md
│   │   └── v2-to-v3.md
│   └── ARCHITECTURE.md
├── src/
│   ├── public/        # Public API
│   └── internal/      # Internal implementation
└── examples/
```

### Websites/Web Applications

**Additional Documentation:**
- `docs/ROUTES.md` - Route structure
- `docs/UI_COMPONENTS.md` - Component library
- `docs/DEPLOYMENT.md` - Deployment guide
- `docs/SEO.md` - SEO considerations (if public)

**Key Differences:**
- Route organization
- UI/UX specifications
- Performance requirements
- Accessibility requirements

**Example Structure (Single-Repo):**
```
web-app/
├── docs/
│   ├── ROUTES.md
│   ├── UI_COMPONENTS.md
│   ├── DEPLOYMENT.md
│   └── FEATURES/
│       ├── authentication.md
│       └── dashboard.md
├── src/
│   ├── routes/
│   ├── components/
│   └── lib/
├── static/
└── package.json
```

**Example Structure (Monorepo - Full-Stack App):**
```
web-app/
├── docs/               # Shared documentation
├── app/                # Main full-stack application (SvelteKit/Next.js)
│   ├── src/
│   │   ├── lib/
│   │   │   ├── components/  # Frontend
│   │   │   └── server/      # Backend
│   │   └── routes/
│   └── package.json
├── admin/              # Admin panel (optional separate app)
│   ├── src/
│   └── package.json
└── package.json        # Workspace config
```

**Example Structure (Monorepo - Separate Frontend/Backend):**
```
web-app/
├── docs/               # Shared documentation
├── frontend/           # Frontend application (React, Vue, etc.)
│   ├── src/
│   └── package.json
├── backend/            # Backend API service
│   ├── src/
│   │   ├── routes/
│   │   └── services/
│   └── package.json
├── shared/             # Shared code
│   └── package.json
└── package.json        # Workspace config
```

**Note:** 
- Use `app/` for full-stack frameworks (SvelteKit, Next.js, Remix) where frontend and backend are integrated
- Use `frontend/` + `backend/` when they are separate applications
- Directory names describe the primary purpose, not every file inside

### Utilities/CLI Tools

**Additional Documentation:**
- `docs/COMMANDS.md` - CLI command reference
- `docs/CONFIGURATION.md` - Configuration options
- `docs/INTEGRATION.md` - Integration examples
- `docs/TROUBLESHOOTING.md` - Common issues

**Key Differences:**
- Command-line interface design
- Configuration file formats
- Exit codes and error handling
- Logging strategy

**Example Structure:**
```
cli-tool/
├── docs/
│   ├── COMMANDS.md
│   ├── CONFIGURATION.md
│   ├── INTEGRATION.md
│   └── EXAMPLES.md
├── src/                # Single-repo: src at root
│   ├── commands/
│   ├── config/
│   └── utils/
├── man/                # Manual pages
└── package.json
```

**Or Monorepo (if CLI has multiple tools):**
```
cli-tool/
├── docs/
├── cli/                # Main CLI tool
├── lib/                # Shared library
├── plugins/            # Plugin system
└── package.json        # Workspace config
```

### SaaS Applications

**Additional Documentation:**
- `docs/BUSINESS_MODEL.md` - Pricing and business logic
- `docs/MULTI_TENANCY.md` - Multi-tenant architecture
- `docs/MODULE_SYSTEM.md` - Feature modules
- `docs/DEPLOYMENT.md` - Deployment and scaling

**Key Differences:**
- Multi-tenancy considerations
- Feature flag/module system
- Billing and subscription logic
- Data isolation and security

**Example Structure (Monorepo - Recommended for SaaS):**
```
saas-app/
├── docs/               # Shared documentation
│   ├── BUSINESS_MODEL.md
│   ├── MULTI_TENANCY.md
│   ├── MODULE_SYSTEM.md
│   └── SECURITY.md
├── app/                # Main application (full-stack SvelteKit/Next.js)
│   ├── src/
│   │   ├── lib/
│   │   │   ├── components/     # Frontend
│   │   │   └── server/         # Backend business layer
│   │   └── routes/             # SvelteKit/Next.js routes
│   └── package.json
├── admin/              # Admin panel (optional separate app)
│   ├── src/
│   └── package.json
├── api/                # Separate API service (optional, if needed)
│   ├── src/
│   └── package.json
├── shared/             # Shared code
│   ├── types/
│   └── utils/
└── package.json        # Workspace config
```

**Alternative (Separate Frontend/Backend for SaaS):**
```
saas-app/
├── docs/
├── frontend/           # Frontend application
│   ├── src/
│   └── package.json
├── backend/            # Backend API service
│   ├── src/
│   │   ├── routes/
│   │   ├── services/
│   │   └── db/
│   └── package.json
├── admin/              # Admin panel
│   └── package.json
└── package.json
```

**Note:** 
- Use `app/` for full-stack frameworks where frontend and backend are integrated
- Use `frontend/` + `backend/` when you need separate deployments or different tech stacks
- The directory name describes the primary purpose, not every file inside

---

## LLM Agent Best Practices

### 1. Document Everything Explicitly

**Bad:**
```markdown
## Authentication

We use Lucia Auth for authentication.
```

**Good:**
```markdown
## Authentication

**Library:** Lucia Auth (`lucia-auth`)

**Why:** 
- Session-based authentication (fits our use case)
- TypeScript-first design
- Drizzle ORM integration
- Uses Oslo.js internally for crypto operations

**Implementation:**
- See: `app/src/lib/server/auth.ts` (or `src/lib/server/auth.ts` in single-repo, or `backend/src/auth.ts` in separate backend)
- Configuration: `app/src/lib/server/auth.config.ts`
- Related: [Auth Setup Ticket](./tickets/phase-1/T-003-authentication-setup.md)

**Note:** In the examples above, `docs/specs/` or `docs/new-system/` can be used interchangeably. Use `specs/` for new projects (recommended), or `new-system/` in migrations (when `old-system/` exists).

**Dependencies:**
- Drizzle ORM (for session storage)
- PostgreSQL (for session table)
```

### 2. Use Code References

**Bad:**
```markdown
The `getUser` function handles user retrieval.
```

**Good:**
```markdown
The `getUser` function handles user retrieval:

```12:25:app/src/lib/server/auth.ts
export async function getUser(sessionId: string) {
  const session = await db.select()
    .from(sessions)
    .where(eq(sessions.id, sessionId))
    .limit(1);
  
  if (!session[0]) return null;
  
  return await db.select()
    .from(users)
    .where(eq(users.id, session[0].userId))
    .limit(1)
    .then(rows => rows[0] || null);
}
```
```

### 3. Cross-Reference Related Documents

**Bad:**
```markdown
## Database Schema

We use PostgreSQL with Drizzle ORM.
```

**Good:**
```markdown
## Database Schema

We use PostgreSQL with Drizzle ORM. See:
- [Database Setup](./tickets/phase-1/T-001-database-setup.md)
- [Schema Definition](./tickets/phase-1/T-002B-tenant-schema-definition.md)
- [Naming Conventions](./specs/DEVELOPMENT_SETUP.md#naming-conventions) (or `./new-system/...`)
- [Multi-Tenancy Architecture](./specs/MULTI_TENANCY.md) (or `./new-system/...`)
```

### 4. Document Decisions and Alternatives

**Bad:**
```markdown
## Installments

Installments are generated in the business layer.
```

**Good:**
```markdown
## Installments

Installments are generated in the business layer, not stored as database records.

**Decision:** See [Installments and Audit Approaches](./specs/CUOTAS_AND_AUDIT_APPROACHES.md#a-installments-approach) (or `./new-system/...`)

**Rationale:**
- Installments are calculated values, not stored data
- Always accurate (reflects current credit and payment state)
- Flexible (can add business logic)
- No storage overhead
- No synchronization issues

**Alternatives Considered:**
1. Physical table - Too much overhead, synchronization issues
2. Database view - Less flexible, harder to test
3. Materialized view - Can get out of sync

**Implementation:**
- Service: `app/src/lib/server/services/installments.ts` (or `src/lib/server/services/installments.ts` in single-repo, or `backend/src/services/installments.ts` in separate backend)
- Related: [Migration Scripts](./tickets/phase-1/T-027-migration-scripts.md)
```

### 5. Maintain Dependency Graphs

**Example:**
```markdown
## Ticket Dependencies

```
T-001 (Database Setup)
  └─> T-002 (Global Schema)
       ├─> T-002B (Complete Tenant Schema) ⚠️ CRITICAL
       │    └─> T-027 (Migration Scripts) ⚠️ CRITICAL
       ├─> T-003 (Auth Setup)
       │    └─> T-004 (Hooks)
       └─> T-005 (Team Management)
```

**Critical Path:** T-001 → T-002 → T-002B → T-027
```

### 6. Use Consistent Terminology

**Document conventions:**
```markdown
## Terminology Conventions

- **English everywhere:** Use English in code, database, and documentation
- **Localization:** UI can map to localized names via i18n
- **Legacy terms:** Document legacy terms with mappings

**Examples:**
- Database: `installments` (not `cuotas`)
- UI: Can display "Cuotas" in Spanish via i18n
- Legacy: `cuotas` → `installments` (migration mapping)
```

### 7. Create Analysis Documents

**For complex decisions:**
```markdown
# Analysis: [Topic]

## Problem Statement

Clear description of the problem.

## Options Considered

### Option 1: [Name]
- Pros
- Cons
- Implementation complexity

### Option 2: [Name]
- Pros
- Cons
- Implementation complexity

## Recommendation

**Chosen:** Option X

**Rationale:**
- Reason 1
- Reason 2

## Implementation Plan

1. Step 1
2. Step 2
```

### 8. Maintain Consistency Reviews

**Regular reviews:**
```markdown
# Consistency Review

**Date:** YYYY-MM-DD

## Issues Found

1. **Issue 1**
   - Location
   - Impact
   - Resolution

## Verification Checklist

- [ ] All naming conventions consistent
- [ ] All cross-references valid
- [ ] All decisions documented
```

---

## Version Control Strategy

### Commit Message Guidelines

**Format:** Conventional Commits
```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation changes
- `refactor` - Code refactoring
- `test` - Test changes
- `chore` - Maintenance tasks

**Examples:**
```
feat(auth): add password reset functionality

Implements password reset flow with email verification.
Uses Lucia Auth session invalidation for security.

See: docs/tickets/phase-1/T-015-password-reset.md
Closes: #42
```

```
docs(tickets): update T-002B with complete schema definition

Adds all missing tables identified in schema dependency analysis:
- expense_payments
- legal_cases
- interest_rates
- fee_rates
- configurations

See: docs/tickets/SCHEMA_DEPENDENCY_ANALYSIS.md
```

### Branching Strategy

**For New Projects:**
- `main` - Production-ready code
- `develop` - Integration branch
- `feature/*` - Feature branches
- `fix/*` - Bug fix branches

**For Migrations:**
- `main` - Current production
- `migration/*` - Migration phases
- `feature/*` - New features
- `legacy` - Legacy code snapshot

**Note:** See [Branch Management Strategies](#branch-management-strategies) section for detailed strategies including single vs multiple agent workflows, and [Main/Staging/Dev Workflows](#mainstagingdev-workflows) for production deployment strategies.

---

## Branch Management Strategies

### Overview

Branch management strategies vary significantly based on:
- **Number of agents:** Single agent vs multiple parallel agents
- **Work location:** Local only, remote (GitHub/GitLab), or hybrid
- **Coordination method:** Sequential work, parallel with coordination, or fully distributed
- **Ticketing system:** Local tickets, GitHub/GitLab issues, or hybrid

### Strategy Selection Matrix

| Scenario | Recommended Strategy | Key Considerations |
|----------|---------------------|-------------------|
| Single agent, local work | Simple Feature Branch | Linear workflow, no coordination needed |
| Single agent, remote work | Feature Branch + PR | Use GitHub/GitLab for backup and review |
| Multiple agents, single local copy | Sequential Work or Task Division | Need coordination, avoid conflicts |
| Multiple agents, multiple local copies | Distributed Feature Branches | Best for parallel work, merge via remote |
| Multiple agents, remote work | PR-Based Workflow | Full GitHub/GitLab integration |
| Hybrid (local + remote) | Feature Branch + Remote Sync | Local development, remote coordination |

---

### Strategy 1: Simple Feature Branch (Single Agent, Local)

**Best for:** Single developer/agent working locally on one machine

**Workflow:**
```
main
  └─> feature/T-001-database-setup
  └─> feature/T-002-schema
  └─> feature/T-003-auth
```

**Branch Naming:**
- `feature/T-XXX-task-name` - Feature branches
- `fix/T-XXX-bug-description` - Bug fixes
- `docs/description` - Documentation updates

**Process:**
1. Create branch from `main`: `git checkout -b feature/T-001-database-setup`
2. Work on ticket
3. Commit frequently with clear messages
4. When complete, merge to `main`: `git checkout main && git merge feature/T-001-database-setup`
5. Delete feature branch: `git branch -d feature/T-001-database-setup`

**Pros:**
- Simple and straightforward
- No coordination overhead
- Fast iteration
- Clean history

**Cons:**
- No backup until pushed
- No code review
- Single point of failure

**Recommendation for This Project:**
✅ **Good starting point** - Simple and effective for single agent work

---

### Strategy 2: Feature Branch + Remote Sync (Single Agent, Hybrid)

**Best for:** Single agent working locally but wants backup and optional remote access

**Workflow:**
```
main (local + remote)
  └─> feature/T-001-database-setup (local + remote)
```

**Process:**
1. Create branch locally: `git checkout -b feature/T-001-database-setup`
2. Work and commit locally
3. Push to remote regularly: `git push -u origin feature/T-001-database-setup`
4. When complete, merge locally and push: `git checkout main && git merge feature/T-001-database-setup && git push`
5. Delete remote branch: `git push origin --delete feature/T-001-database-setup`

**Pros:**
- Backup of work
- Can work from multiple machines
- Can create PRs for review if desired
- Still primarily local workflow

**Cons:**
- Requires remote repository setup
- Slightly more overhead

**Recommendation for This Project:**
✅ **Recommended** - Provides backup while maintaining local-first workflow

---

### Strategy 3: Sequential Agent Work (Multiple Agents, Single Local Copy)

**Best for:** Multiple agents but only one can work at a time on the same local copy

**Coordination Methods:**

#### Method A: Explicit Handoff
- Agent 1 completes ticket, commits, pushes
- Agent 2 pulls latest, works on next ticket
- Clear communication about current state

#### Method B: Lock File
- Create `.agent-lock` file when working
- Agents check for lock before starting
- Remove lock when done

**Workflow:**
```
main
  └─> feature/T-001 (Agent 1) → merge
  └─> feature/T-002 (Agent 2) → merge
  └─> feature/T-003 (Agent 1) → merge
```

**Process:**
1. Check for lock: `test -f .agent-lock && echo "Agent working" || echo "Available"`
2. Create lock: `touch .agent-lock && echo "agent-1" > .agent-lock`
3. Pull latest: `git pull origin main`
4. Create branch: `git checkout -b feature/T-XXX`
5. Work and commit
6. Merge and push: `git checkout main && git merge feature/T-XXX && git push`
7. Remove lock: `rm .agent-lock`

**Pros:**
- Works with single local copy
- No merge conflicts
- Clear ownership

**Cons:**
- No true parallelism
- Requires coordination
- Potential waiting time

**Recommendation for This Project:**
⚠️ **Possible but limiting** - Consider if you need true parallel work

---

### Strategy 4: Task Division (Multiple Agents, Single Local Copy)

**Best for:** Multiple agents working on different, non-conflicting areas

**Approach:**
- Divide work by area (frontend, backend, docs, tests)
- Each agent works on different tickets/areas
- Coordinate to avoid file conflicts
- Merge frequently

**Workflow:**
```
main
  ├─> feature/T-001-backend (Agent 1)
  └─> feature/T-002-frontend (Agent 2)
  └─> docs/T-003-docs (Agent 3)
```

**Coordination:**
- Use ticket system to assign work
- Document which files each agent is touching
- Merge in order (backend → frontend → docs)
- Test after each merge

**File Conflict Prevention:**
- Backend agent: `app/src/lib/server/**`
- Frontend agent: `app/src/lib/components/**`
- Docs agent: `docs/**`
- Tests: Coordinate or assign to specific agent

**Pros:**
- Some parallelism possible
- Works with single local copy
- Clear division of work

**Cons:**
- Requires careful coordination
- Still risk of conflicts
- Need to merge frequently

**Recommendation for This Project:**
✅ **Good option** - If you can divide work by area (backend/frontend/docs)

---

### Strategy 5: Distributed Feature Branches (Multiple Agents, Multiple Local Copies)

**Best for:** Multiple agents, each with their own local copy, working in parallel

**Workflow:**
```
Remote (GitHub/GitLab):
  main
    ├─> feature/T-001 (Agent 1's copy)
    ├─> feature/T-002 (Agent 2's copy)
    └─> feature/T-003 (Agent 3's copy)
```

**Process:**
1. Each agent clones repository: `git clone <repo-url>`
2. Each creates their own branch: `git checkout -b feature/T-XXX`
3. Work independently
4. Push to remote: `git push -u origin feature/T-XXX`
5. Create Pull Request (optional)
6. Merge via remote (or locally after pull)

**Coordination:**
- Use remote repository as coordination point
- Pull before starting: `git pull origin main`
- Push frequently: `git push origin feature/T-XXX`
- Resolve conflicts via remote or locally

**Pros:**
- True parallelism
- No local conflicts
- Each agent has full copy
- Can use PRs for review

**Cons:**
- Requires multiple local copies
- More disk space
- Need remote repository
- Merge conflicts possible

**Recommendation for This Project:**
✅ **Best for parallel work** - If you want multiple agents working simultaneously

---

### Strategy 6: PR-Based Workflow (Multiple Agents, Remote-First)

**Best for:** Teams using GitHub/GitLab issues and pull requests

**Workflow:**
```
GitHub/GitLab:
  main (protected)
    ├─> feature/T-001 (PR #1)
    ├─> feature/T-002 (PR #2)
    └─> feature/T-003 (PR #3)
```

**Process:**
1. Create issue/ticket in GitHub/GitLab
2. Create branch: `git checkout -b feature/T-001`
3. Work and commit
4. Push: `git push -u origin feature/T-001`
5. Create Pull Request
6. Review and merge via GitHub/GitLab UI
7. Delete branch after merge

**Integration:**
- Link commits to issues: `git commit -m "feat: add feature

Closes #42"`
- Use PR templates
- Require reviews (if team)
- Run CI/CD on PRs

**Pros:**
- Full GitHub/GitLab integration
- Code review built-in
- CI/CD integration
- Issue tracking integration
- Team collaboration

**Cons:**
- Requires remote repository
- More process overhead
- Less local-first

**Recommendation for This Project:**
⚠️ **Optional** - Can use for coordination even with local-first workflow

---

### Strategy 7: Hybrid Approach (Recommended for This Project)

**Best for:** Local-first development with remote coordination and optional parallel agents

**Workflow:**
```
Local (Primary):
  main
    └─> feature/T-001
    └─> feature/T-002

Remote (Backup + Coordination):
  main
    └─> feature/T-001 (synced)
    └─> feature/T-002 (synced)
```

**Process:**
1. **Primary work:** Local development on feature branches
2. **Regular sync:** Push to remote for backup: `git push origin feature/T-XXX`
3. **Coordination:** Use remote to coordinate if multiple agents
4. **Optional PRs:** Create PRs for important changes or reviews
5. **Merge locally:** Merge to main locally, then push

**For Multiple Agents:**
- Each agent has own local copy
- Work on different tickets/branches
- Push to remote regularly
- Pull before starting: `git pull origin main`
- Merge conflicts resolved locally or via PRs

**Configuration:**
```bash
# Set up remote
git remote add origin <repo-url>

# Regular sync
git push origin feature/T-XXX

# Before starting work
git pull origin main

# After completing work
git checkout main
git merge feature/T-XXX
git push origin main
```

**Pros:**
- Local-first workflow
- Backup and coordination
- Supports multiple agents
- Flexible (can use PRs when needed)
- Best of both worlds

**Cons:**
- Requires remote setup
- Need to sync regularly
- Slightly more complex

**Recommendation for This Project:**
✅ **Highly Recommended** - Balances local-first preference with coordination needs

---

### Branch Naming Conventions

**Consistent naming helps coordination:**

**Pattern:** `<type>/<ticket>-<description>`

**Types:**
- `feature/` - New features
- `fix/` - Bug fixes
- `docs/` - Documentation
- `refactor/` - Refactoring
- `test/` - Test additions
- `chore/` - Maintenance

**Examples:**
- `feature/T-001-database-setup`
- `fix/T-015-auth-bug`
- `docs/T-020-update-readme`
- `refactor/T-025-schema-cleanup`

**Benefits:**
- Easy to identify branch purpose
- Links to tickets
- Groups related branches
- Clear in remote repository

---

### Coordination Strategies for Multiple Agents

#### 1. Ticket-Based Division

**Approach:**
- Assign tickets to agents
- Each agent works on assigned tickets
- Clear ownership

**Implementation:**
- Use ticket system to assign
- Document assignments
- Update ticket status

#### 2. Area-Based Division

**Approach:**
- Divide by code area (backend, frontend, docs)
- Each agent owns an area
- Coordinate cross-area changes

**Implementation:**
- Document area ownership
- Coordinate via tickets or chat
- Review cross-area changes

#### 3. Time-Based Division

**Approach:**
- Agents work in shifts
- Hand off at specific times
- Clear handoff process

**Implementation:**
- Define shift schedule
- Document handoff checklist
- Use lock files or status

#### 4. Priority-Based Division

**Approach:**
- High-priority tickets to one agent
- Lower-priority to others
- Dynamic assignment

**Implementation:**
- Use ticket priorities
- Assign dynamically
- Reassign as needed

---

### Conflict Resolution Strategies

#### Prevention

1. **Clear division of work**
   - Assign tickets/areas
   - Document file ownership
   - Coordinate cross-cutting changes

2. **Frequent syncing**
   - Pull before starting: `git pull origin main`
   - Push frequently: `git push origin feature/T-XXX`
   - Merge small, incremental changes

3. **Communication**
   - Document what you're working on
   - Notify before touching shared files
   - Coordinate large refactors

#### Resolution

1. **Local resolution:**
   ```bash
   git pull origin main
   # Resolve conflicts
   git add .
   git commit -m "fix: resolve merge conflicts"
   git push
   ```

2. **Via PR:**
   - Create PR
   - GitHub/GitLab shows conflicts
   - Resolve in UI or locally
   - Update PR

3. **Rebase strategy:**
   ```bash
   git pull --rebase origin main
   # Resolve conflicts
   git rebase --continue
   ```

---

### Recommended Strategy for This Project

**Based on your preferences (local work, single copy, considering parallel agents):**

#### Phase 1: Single Agent (Current)

**Strategy:** Simple Feature Branch + Remote Sync

**Workflow:**
1. Work locally on feature branches
2. Push to remote regularly for backup
3. Merge locally, push to remote
4. Use remote as backup and coordination point

**Commands:**
```bash
# Start ticket
git checkout main
git pull origin main
git checkout -b feature/T-001-database-setup

# Work and commit
git add .
git commit -m "feat(db): add database setup

See: docs/tickets/phase-1/T-001-database-setup.md"

# Regular backup
git push -u origin feature/T-001-database-setup

# Complete ticket
git checkout main
git merge feature/T-001-database-setup
git push origin main
git branch -d feature/T-001-database-setup
git push origin --delete feature/T-001-database-setup
```

#### Phase 2: Multiple Agents (If Needed)

**Strategy:** Hybrid Approach with Task Division

**Workflow:**
1. Each agent has own local copy (or coordinate on single copy)
2. Divide work by area (backend, frontend, docs, tests)
3. Work on different tickets
4. Sync via remote regularly
5. Merge in coordination

**Coordination:**
- Use ticket system to assign work
- Document current work in tickets
- Pull before starting: `git pull origin main`
- Push frequently: `git push origin feature/T-XXX`
- Coordinate merges

**File Division Example:**
- Agent 1 (Backend): `app/src/lib/server/**`, `app/src/routes/api/**`
- Agent 2 (Frontend): `app/src/lib/components/**`, `app/src/routes/app/**`
- Agent 3 (Docs): `docs/**`
- Agent 4 (Tests): `app/tests/**`, `app/e2e/**`

**Lock File Approach (Single Copy):**
```bash
# Before starting work
if [ -f .agent-lock ]; then
  echo "Another agent is working. Current agent: $(cat .agent-lock)"
  exit 1
fi
echo "agent-1" > .agent-lock

# After completing work
rm .agent-lock
```

---

### GitHub/GitLab Integration (Optional)

Even with local-first workflow, GitHub/GitLab can help:

**Benefits:**
- Backup and sync
- Issue/ticket tracking
- Optional PRs for review
- CI/CD integration
- Team coordination

**Minimal Setup:**
- Create repository
- Push branches regularly
- Use issues for tickets (optional)
- Create PRs for important changes (optional)

**Full Integration:**
- Use GitHub/GitLab issues for all tickets
- Create PRs for all changes
- Require reviews
- Run CI/CD on PRs
- Use project boards

---

### Best Practices

1. **Always pull before starting:**
   ```bash
   git checkout main
   git pull origin main
   git checkout -b feature/T-XXX
   ```

2. **Commit frequently:**
   - Small, logical commits
   - Clear commit messages
   - Reference tickets

3. **Push regularly:**
   ```bash
   git push -u origin feature/T-XXX
   ```

4. **Test before merging:**
   ```bash
   npm test
   npm run build
   ```

5. **Clean up branches:**
   ```bash
   git branch -d feature/T-XXX
   git push origin --delete feature/T-XXX
   ```

6. **Document current work:**
   - Update ticket status
   - Document what you're working on
   - Notify team of changes

---

### Tools and Automation

#### Git Hooks

**Pre-commit hook:**
```bash
#!/bin/sh
# .git/hooks/pre-commit
npm run lint
npm run type-check
```

**Post-merge hook:**
```bash
#!/bin/sh
# .git/hooks/post-merge
npm install
```

#### Scripts

**Start ticket script:**
```bash
#!/bin/bash
# scripts/start-ticket.sh
TICKET=$1
git checkout main
git pull origin main
git checkout -b feature/$TICKET
echo "Started work on $TICKET"
```

**Complete ticket script:**
```bash
#!/bin/bash
# scripts/complete-ticket.sh
TICKET=$1
git checkout main
git merge feature/$TICKET
git push origin main
git branch -d feature/$TICKET
git push origin --delete feature/$TICKET
echo "Completed $TICKET"
```

---

### Summary

**For This Project (Local-First, Single Copy, Considering Parallel Agents):**

1. **Start with:** Simple Feature Branch + Remote Sync
2. **If adding agents:** Hybrid Approach with Task Division
3. **If true parallelism needed:** Distributed Feature Branches (multiple local copies)
4. **Optional:** GitHub/GitLab integration for coordination

**Key Recommendations:**
- ✅ Use feature branches for each ticket
- ✅ Push to remote regularly for backup
- ✅ Pull before starting new work
- ✅ Divide work by area if using multiple agents
- ✅ Use ticket system for coordination
- ✅ Test before merging
- ✅ Clean up branches after merge

### Git History Best Practices

1. **Chronological Commits**
   - Group related changes
   - Commit in logical order
   - Document why, not just what

2. **Non-Breaking Changes First**
   - Documentation before code
   - Schema before migration
   - Foundation before features

3. **Reference Tickets**
   - Include ticket numbers
   - Link to related docs
   - Explain context

---

## Main/Staging/Dev Workflows

### Overview

Different projects require different branch strategies for managing releases, testing, and production deployments. This section covers common patterns and how they work with LLM agent workflows.

### Workflow Patterns

#### Pattern 1: Simple Main Branch (GitHub Flow)

**Best for:** Continuous deployment, simple projects, single environment

**Structure:**
```
main (production)
  └─> feature/T-001
  └─> feature/T-002
```

**Workflow:**
1. Create feature branch from `main`
2. Develop and test
3. Merge to `main` (triggers deployment)
4. Deploy to production

**Pros:**
- Simple and straightforward
- Fast iteration
- Good for continuous deployment
- Easy to understand

**Cons:**
- No staging environment
- All changes go directly to production
- Requires high confidence in testing

**LLM Agent Considerations:**
- ✅ Simple for agents to understand
- ✅ Clear merge target
- ⚠️ Requires thorough testing before merge

---

#### Pattern 2: Main + Develop (Git Flow Simplified)

**Best for:** Projects needing integration testing, multiple environments

**Structure:**
```
main (production)
  └─> develop (integration/staging)
      └─> feature/T-001
      └─> feature/T-002
```

**Workflow:**
1. Create feature branch from `develop`
2. Develop and test locally
3. Merge to `develop` (deploys to staging)
4. Test in staging environment
5. Merge `develop` to `main` (deploys to production)

**Pros:**
- Staging environment for testing
- Can batch multiple features
- Safer than direct to production
- Good for scheduled releases

**Cons:**
- More complex than simple main
- Requires coordination for releases
- Can delay feature releases

**LLM Agent Considerations:**
- ✅ Clear separation of environments
- ✅ Agents merge to `develop` first
- ⚠️ Need to understand two merge targets
- ⚠️ Release process requires merging `develop` → `main`

---

#### Pattern 3: Main + Staging + Develop (Full Git Flow)

**Best for:** Complex projects, multiple environments, release management

**Structure:**
```
main (production)
  └─> staging (pre-production)
      └─> develop (integration)
          └─> feature/T-001
          └─> feature/T-002
```

**Workflow:**
1. Create feature branch from `develop`
2. Develop and test locally
3. Merge to `develop` (deploys to dev environment)
4. Test in dev environment
5. Merge `develop` to `staging` (deploys to staging)
6. Test in staging environment
7. Merge `staging` to `main` (deploys to production)

**Pros:**
- Multiple testing environments
- Very safe deployment process
- Good for complex projects
- Clear environment separation

**Cons:**
- Most complex workflow
- Slower release cycle
- Requires more coordination
- Can be overkill for simple projects

**LLM Agent Considerations:**
- ⚠️ Complex for agents to navigate
- ⚠️ Multiple merge targets
- ⚠️ Need clear documentation of workflow
- ✅ Very safe for production

---

#### Pattern 4: Main + Staging (Two-Branch)

**Best for:** Projects needing staging but not full Git Flow

**Structure:**
```
main (production)
  └─> staging (pre-production)
      └─> feature/T-001
      └─> feature/T-002
```

**Workflow:**
1. Create feature branch from `staging`
2. Develop and test locally
3. Merge to `staging` (deploys to staging)
4. Test in staging environment
5. Merge `staging` to `main` (deploys to production)

**Pros:**
- Simpler than full Git Flow
- Still has staging environment
- Good balance of safety and simplicity
- Clear two-stage process

**Cons:**
- No separate integration branch
- Features merge directly to staging
- Requires coordination for releases

**LLM Agent Considerations:**
- ✅ Simpler than three-branch flow
- ✅ Clear staging → main flow
- ✅ Good balance for agents

---

### Choosing the Right Pattern

**Use Simple Main (Pattern 1) if:**
- ✅ Continuous deployment
- ✅ High test coverage
- ✅ Simple project
- ✅ Fast iteration needed
- ✅ Single environment

**Use Main + Develop (Pattern 2) if:**
- ✅ Need staging environment
- ✅ Batch releases
- ✅ Integration testing needed
- ✅ Multiple features per release

**Use Main + Staging + Develop (Pattern 3) if:**
- ✅ Complex project
- ✅ Multiple environments
- ✅ Strict release process
- ✅ Enterprise-level safety

**Use Main + Staging (Pattern 4) if:**
- ✅ Need staging but not full Git Flow
- ✅ Simpler than three-branch
- ✅ Two-stage deployment
- ✅ Good balance

---

### Integration with Feature Branches

**All patterns use feature branches:**

```
main/staging/develop
  └─> feature/T-001
  └─> feature/T-002
```

**Key Points:**
- Feature branches always merge to integration branch first
- Integration branch (develop/staging) merges to production (main)
- Feature branches are temporary (deleted after merge)
- Integration branches are long-lived

---

### Release Process

#### Pattern 1: Simple Main (Continuous Deployment)

**Process:**
1. Feature complete → Merge to `main`
2. Automatic deployment to production
3. Monitor and fix issues

**No release branches needed** - each merge is a release

#### Pattern 2: Main + Develop (Scheduled Releases)

**Process:**
1. Features merge to `develop` continuously
2. When ready for release:
   - Create release branch: `release/v1.2.0` (optional)
   - Test and fix issues
   - Merge `develop` to `main`
   - Tag release: `git tag v1.2.0`
   - Deploy to production

**Release branches (optional):**
- Create from `develop` when ready to release
- Fix bugs and prepare release
- Merge to both `main` and `develop`
- Delete after merge

#### Pattern 3: Main + Staging + Develop (Staged Releases)

**Process:**
1. Features merge to `develop` continuously
2. Regular merges: `develop` → `staging`
3. When ready for release:
   - Test in staging
   - Create release branch: `release/v1.2.0` (optional)
   - Merge `staging` to `main`
   - Tag release: `git tag v1.2.0`
   - Deploy to production

#### Pattern 4: Main + Staging (Two-Stage Releases)

**Process:**
1. Features merge to `staging` continuously
2. When ready for release:
   - Test in staging
   - Merge `staging` to `main`
   - Tag release: `git tag v1.2.0`
   - Deploy to production

---

### Hotfix Process

**For all patterns with `main` branch:**

**Process:**
1. Create hotfix branch from `main`: `hotfix/critical-bug`
2. Fix the issue
3. Test thoroughly
4. Merge to `main` (and `develop`/`staging` if they exist)
5. Deploy to production immediately
6. Delete hotfix branch

**Example:**
```bash
# Create hotfix from main
git checkout main
git pull origin main
git checkout -b hotfix/critical-auth-bug

# Fix and commit
git add .
git commit -m "fix: critical authentication bug"
git push -u origin hotfix/critical-auth-bug

# Merge to main
git checkout main
git merge hotfix/critical-auth-bug
git push origin main

# Also merge to develop/staging if they exist
git checkout develop
git merge hotfix/critical-auth-bug
git push origin develop

# Delete hotfix branch
git branch -d hotfix/critical-auth-bug
git push origin --delete hotfix/critical-auth-bug
```

---

### LLM Agent Workflow Integration

#### For Single Agent

**Simple Main:**
1. Create feature branch from `main`
2. Work on ticket
3. Merge to `main`
4. Deploy

**Main + Develop:**
1. Create feature branch from `develop`
2. Work on ticket
3. Merge to `develop` (staging deployment)
4. When ready, merge `develop` to `main` (production)

**Main + Staging:**
1. Create feature branch from `staging`
2. Work on ticket
3. Merge to `staging` (staging deployment)
4. When ready, merge `staging` to `main` (production)

#### For Multiple Agents

**Coordination:**
- All agents create feature branches from same integration branch
- Merge to integration branch (develop/staging)
- Coordinate release merges (integration → main)
- Use remote repository for coordination

**Example:**
```
Agent 1: feature/T-001 → develop
Agent 2: feature/T-002 → develop
Agent 3: feature/T-003 → develop
→ When ready: develop → main (coordinated release)
```

---

### CI/CD Integration

**Branch Protection:**
- Protect `main` branch (require PR, reviews, tests)
- Protect `staging` branch (require PR, tests)
- Protect `develop` branch (optional, require tests)

**Automated Deployments:**
- `main` → Production environment
- `staging` → Staging environment
- `develop` → Dev environment

**Example GitHub Actions:**
```yaml
# Deploy to staging on develop merge
on:
  push:
    branches: [develop]
jobs:
  deploy-staging:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to staging
        run: ./scripts/deploy-staging.sh

# Deploy to production on main merge
on:
  push:
    branches: [main]
jobs:
  deploy-production:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to production
        run: ./scripts/deploy-production.sh
```

---

### Migration Projects

**For migration projects, consider:**

**Option A: Separate Migration Branch**
```
main (current production)
  └─> migration/phase-1
  └─> migration/phase-2
```

**Option B: Migration as Feature**
```
main
  └─> develop
      └─> migration/phase-1
      └─> migration/phase-2
```

**Option C: Parallel Tracks**
```
main (legacy production)
  └─> develop (new system development)
      └─> feature/T-001
```

**Recommendation:** Use Option B or C depending on whether you're maintaining legacy system during migration.

---

### Recommended Pattern for This Project

**Based on project characteristics (SaaS, migration, multi-tenant):**

**Recommended: Main + Staging (Pattern 4)**

**Structure:**
```
main (production)
  └─> staging (pre-production testing)
      └─> feature/T-001
      └─> feature/T-002
```

**Rationale:**
- ✅ Need staging for testing multi-tenant features
- ✅ Simpler than full Git Flow
- ✅ Clear two-stage process
- ✅ Good for migration project
- ✅ Easy for LLM agents to understand

**Workflow:**
1. Create feature branch from `staging`
2. Develop and test locally
3. Merge to `staging` (deploys to staging environment)
4. Test in staging (multi-tenant, permissions, etc.)
5. When ready, merge `staging` to `main` (deploys to production)

**Alternative: Main + Develop (Pattern 2)**
- If you want separate integration branch
- If you batch multiple features per release
- If you need more separation between development and staging

---

### Branch Naming for Integration Branches

**Standard Names:**
- `main` or `master` - Production
- `staging` - Pre-production
- `develop` or `dev` - Development/integration

**Avoid:**
- `production` (use `main`)
- `prod` (use `main`)
- `staging` and `stage` (pick one, use consistently)
- `development` (use `develop`)

**Recommendation:** Use `main`, `staging`, `develop` for consistency.

---

### Merge Strategies

When merging feature branches into main, staging, or dev branches, teams use different strategies:

- **Merge as-is:** Preserves all commits with a merge commit (complete history)
- **Squash and merge:** Combines all commits into one with comprehensive message (clean history)
- **Rebase and merge:** Replays commits linearly without merge commit (linear history, preserved commits)
- **Squash + Rebase:** Combines commits, then rebases (cleanest possible history)

**See:** [Branch Management Guide - Merge Strategies](./docs/process/BRANCH_MANAGEMENT.md#merge-strategies) for detailed comparison, pros/cons, and recommendations for each branch type.

**Recommendation:**
- **Feature → Dev:** Merge as-is (preserve development history)
- **Feature → Staging:** Squash and merge (clean history for testing)
- **Staging → Main:** Squash and merge (clean production history)

### Best Practices

1. **Protect Production Branch:**
   - Require PRs for `main`
   - Require reviews
   - Require passing tests
   - No direct pushes

2. **Regular Integration:**
   - Merge to integration branch frequently
   - Don't let feature branches get too far behind
   - Test in integration environment regularly

3. **Clear Release Process:**
   - Document release process
   - Use tags for releases: `git tag v1.2.0`
   - Document what's in each release

4. **Hotfix Process:**
   - Have clear hotfix process
   - Fix in hotfix branch
   - Merge to all relevant branches
   - Deploy quickly

5. **Environment Parity:**
   - Keep environments similar
   - Test in staging what you deploy to production
   - Document environment differences

6. **Consistent Merge Strategy:**
   - Choose merge strategy per branch type
   - Document your team's choice
   - Use consistently across the project
   - Update helper scripts to use chosen strategy

---

## Quality Assurance

### Documentation Quality Checks

1. **Completeness**
   - All features documented
   - All decisions explained
   - All dependencies listed

2. **Consistency**
   - Naming conventions followed
   - Terminology standardized
   - Cross-references valid

3. **Accuracy**
   - Code examples work
   - Links are valid
   - Information is current

### Code Quality Checks

1. **Standards Compliance**
   - Linting rules
   - Type checking
   - Formatting

2. **Test Coverage**
   - Unit tests
   - Integration tests
   - E2E tests

3. **Documentation Sync**
   - Code matches docs
   - Examples are current
   - APIs are documented

---

## Examples and Templates

### Template: Feature Specification

```markdown
# Feature: [Feature Name]

**Module:** [Module Name] (if applicable)  
**Priority:** [P0/P1/P2]  
**Status:** [Planned/In Progress/Complete]

## Description

Clear description of the feature.

## Requirements

- [ ] Requirement 1
- [ ] Requirement 2

## Technical Details

### Data Model
```typescript
interface FeatureData {
  // Type definition
}
```

### API Endpoints
- `GET /api/feature` - Description
- `POST /api/feature` - Description

### Components
- `FeatureComponent.svelte` - Description

## Business Rules

- Rule 1
- Rule 2

## Related Documents

- [Technical Architecture](./technical-architecture.md)
- [Ticket T-XXX](./tickets/phase-X/T-XXX-feature.md)
```

### Template: Migration Plan

```markdown
# Migration Plan: [Legacy System] → [New System]

## Executive Summary

High-level overview of migration.

## Migration Strategy

### Phases

1. **Phase 1: Foundation**
   - Tasks
   - Dependencies
   - Timeline

2. **Phase 2: Core Migration**
   - Tasks
   - Dependencies
   - Timeline

## Risk Mitigation

- Risk 1: Mitigation strategy
- Risk 2: Mitigation strategy

## Rollback Plan

Steps to rollback if needed.

## Success Criteria

- [ ] Criterion 1
- [ ] Criterion 2
```

### Template: Ticket

```markdown
# T-XXX: [Ticket Title]

**Priority:** [P0/P1/P2]  
**Type:** [Task/Feature/Bug]  
**Labels:** `phase-X`, `category`, `priority:high`

**Status:** [Open/In Progress/Review/Closed]

---

## Summary

One-sentence summary of the ticket.

## Background/Context

Why this ticket exists, what problem it solves.

**References:**
- [Related Doc](./path/to/doc.md)
- [Related Ticket](./T-YYY-related-ticket.md)

## Requirements

- [ ] Requirement 1
- [ ] Requirement 2

## Technical Details

Implementation details, code examples, etc.

## Acceptance Criteria

- [ ] Criterion 1
- [ ] Criterion 2

**Testing:**
- [ ] Unit test: Test case 1
- [ ] Integration test: Test case 2

## Additional Information

- **Dependencies:** T-XXX, T-YYY
- **Blocks:** T-ZZZ
- **Related Documentation:**
  - [Doc 1](./path/to/doc1.md)
  - [Doc 2](./path/to/doc2.md)

---

**Created:** YYYY-MM-DD  
**Estimated Effort:** X days  
**Assignee:** TBD
```

---

## Recommendations

### For Maximum LLM Agent Effectiveness

1. **Start with Documentation**
   - Document before coding
   - Update docs with code changes
   - Review docs regularly

2. **Use Consistent Patterns**
   - Same structure for similar documents
   - Same format for tickets
   - Same style for commit messages

3. **Maintain Cross-References**
   - Link related documents
   - Reference tickets in commits
   - Link code to documentation

4. **Document Decisions**
   - Why, not just what
   - Alternatives considered
   - Trade-offs made

5. **Regular Consistency Reviews**
   - Review documentation quarterly
   - Check cross-references
   - Update outdated information

6. **Version Control Best Practices**
   - Meaningful commit messages
   - Logical commit grouping
   - Reference tickets and docs

7. **Create Analysis Documents**
   - For complex decisions
   - For dependency analysis
   - For consistency reviews

8. **Use Templates**
   - Ticket templates
   - Document templates
   - Commit message templates

### Project-Specific Recommendations

**For Libraries:**
- Focus on API documentation
- Maintain changelog
- Document breaking changes
- Provide migration guides

**For Websites:**
- Document route structure
- Specify UI/UX requirements
- Document deployment process
- Include SEO considerations

**For Utilities:**
- Document all commands
- Provide usage examples
- Document configuration options
- Include troubleshooting guide

**For SaaS Applications:**
- Document multi-tenancy architecture
- Specify feature module system
- Document billing logic
- Include security considerations

---

## Conclusion

Structuring projects for LLM agent collaboration requires:
- **Clear organization** - Predictable structure
- **Explicit documentation** - No assumptions
- **Consistent patterns** - Easy to follow
- **Cross-references** - Easy navigation
- **Decision documentation** - Context preservation

Following these practices will significantly improve LLM agent effectiveness and project maintainability.

---

**Document Version:** 1.0  
**Last Updated:** 2025-01-XX  
**Based On:** Iterative development across multiple projects, including the first migration project for LLM agent assistance

