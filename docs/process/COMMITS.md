# Commit Guidelines

This document outlines the commit message format and guidelines for this repository.

## Commit Message Format

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification for our commit messages. This format provides a clear structure and enables automated tooling.

### Format Structure

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Components

#### Type (Required)

The type indicates the nature of the change. Use one of the following:

- **feat**: A new feature
- **fix**: A bug fix
- **docs**: Documentation only changes
- **style**: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc.)
- **refactor**: A code change that neither fixes a bug nor adds a feature
- **perf**: A code change that improves performance
- **test**: Adding missing tests or correcting existing tests
- **build**: Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm)
- **ci**: Changes to CI configuration files and scripts
- **chore**: Other changes that don't modify src or test files
- **revert**: Reverts a previous commit

#### Scope (Optional)

The scope specifies the area of the codebase affected. Examples:

- `auth`: Authentication-related changes
- `api`: API changes
- `ui`: User interface changes
- `db`: Database changes
- `migration`: Migration-related changes
- `config`: Configuration changes

#### Subject (Required)

- Use imperative, present tense: "add feature" not "added feature" or "adds feature"
- Don't capitalize the first letter
- No period (.) at the end
- Maximum 72 characters
- Be concise and descriptive

#### Body (Optional)

- Provide additional context about the change
- Explain the "what" and "why" vs. the "how"
- Wrap at 72 characters
- Use blank line to separate from subject
- Can include multiple paragraphs

#### Footer (Optional)

- Reference issue numbers: `Closes #123`, `Fixes #456`
- Breaking changes: `BREAKING CHANGE: <description>`
- Co-authors: `Co-authored-by: Name <email>`

## Examples

### Good Commit Messages

```
feat(auth): add JWT token refresh mechanism

Implement automatic token refresh when access token expires.
This improves user experience by reducing forced logouts.

Closes #123
```

```
fix(api): resolve null pointer exception in user service

The getUserById method was throwing NPE when user not found.
Added null check and proper error handling.

Fixes #456
```

```
docs(migration): update migration plan with new requirements

Added section about multi-tenancy considerations and updated
timeline based on stakeholder feedback.
```

```
refactor(db): extract query builder to separate module

Improves code organization and makes database queries more
testable. No functional changes.
```

```
feat(ui)!: redesign user dashboard layout

BREAKING CHANGE: Dashboard component props have changed.
Old layout components are no longer supported.

Migration guide available in docs/migration/ui-dashboard.md
```

### Bad Commit Messages

```
❌ fix bug
❌ updated files
❌ WIP
❌ asdf
❌ fix: Fixed the thing that was broken
❌ feat: add new feature (too vague)
❌ docs: update readme (missing context)
```

## Commit Guidelines

### 1. Atomic Commits

- Make one logical change per commit
- Don't mix unrelated changes
- If you need to make multiple changes, use multiple commits

**Good:**
```
feat(auth): add password reset functionality
fix(api): handle expired tokens gracefully
```

**Bad:**
```
fix: various bugs and added new feature
```

### 2. Commit Frequency

- Commit early and often
- Don't wait until the end of the day to commit
- Each commit should represent a working state (code compiles, tests pass)

### 3. Commit Size

- Keep commits focused and reasonably sized
- Large features should be broken into smaller, logical commits
- Aim for commits that can be reviewed in 5-10 minutes

### 4. Testing

- Ensure code compiles before committing
- Run relevant tests before committing
- Don't commit broken code (use `WIP:` prefix if necessary, but avoid in main branches)

### 5. Review Your Changes

- Review your changes with `git diff` before committing
- Ensure you're not committing temporary files, debug code, or secrets
- Check that you're committing the right files

## Branch Naming

When creating branches, use descriptive names that follow this pattern:

```
<type>/<short-description>
```

Examples:
- `feat/user-authentication`
- `fix/login-error-handling`
- `docs/api-documentation`
- `refactor/database-queries`

## Commit Workflow

1. **Before committing:**
   - Review your changes: `git status` and `git diff`
   - Ensure code compiles and tests pass
   - Check for any temporary files or debug code

2. **Writing the commit message:**
   - Use the format specified above
   - Be clear and descriptive
   - Reference related issues

3. **After committing:**
   - Verify your commit: `git log -1`
   - Push to remote when ready

## Tools and Automation

### Commit Message Templates

You can set up a Git commit message template:

```bash
git config commit.template .gitmessage
```

Create a `.gitmessage` file in the repository root with:

```
# <type>(<scope>): <subject>
#
# <body>
#
# <footer>
```

### Pre-commit Hooks

Consider using tools like:
- [commitlint](https://commitlint.js.org/) - Lint commit messages
- [husky](https://typicode.github.io/husky/) - Git hooks made easy
- [lint-staged](https://github.com/okonet/lint-staged) - Run linters on staged files

## Additional Resources

- [Conventional Commits Specification](https://www.conventionalcommits.org/)
- [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/)
- [Semantic Versioning](https://semver.org/) (for versioning based on commit types)

## Questions?

If you're unsure about how to format a commit message, ask a team member or refer to recent commits in the repository for examples.

