# Branch Management Strategy for This Project

**Project:** Escriba Finanzas  
**Workflow Preference:** Local-first, single local copy  
**Agent Setup:** Currently single agent, considering multiple parallel agents

---

## Recommended Strategy

### Branch Structure

**Recommended: Main + Staging (Two-Branch Pattern)**

```
main (production)
  └─> staging (pre-production testing)
      └─> feature/T-001-database-setup
      └─> feature/T-002-schema
      └─> feature/T-003-auth
```

**Rationale:**
- Need staging environment for testing multi-tenant features
- Simpler than full Git Flow (main + staging + develop)
- Clear two-stage deployment process
- Good for migration project
- Easy for LLM agents to understand

**Alternative:** If you prefer simpler workflow, use just `main` (Simple Main pattern) and add `staging` later when needed.

### Phase 1: Single Agent (Current)

**Strategy:** Feature Branch + Remote Sync with Staging

**Workflow:**
```
main (production, local + remote)
  └─> staging (pre-production, local + remote)
      └─> feature/T-001-database-setup (local + remote)
      └─> feature/T-002-schema (local + remote)
      └─> feature/T-003-auth (local + remote)
```

**Process:**
1. Start ticket: Create feature branch from main
2. Work locally: Make changes, commit frequently
3. Regular backup: Push to remote regularly
4. Complete ticket: Merge to main, push, cleanup

**Commands:**
```bash
# Start new ticket (from staging)
git checkout staging
git pull origin staging
git checkout -b feature/T-001-database-setup

# Work and commit
git add .
git commit -m "feat(db): add database setup

See: docs/tickets/phase-1/T-001-database-setup.md"

# Regular backup (do this frequently)
git push -u origin feature/T-001-database-setup

# Complete ticket (merge to staging)
git checkout staging
git merge feature/T-001-database-setup
git push origin staging
git branch -d feature/T-001-database-setup
git push origin --delete feature/T-001-database-setup

# When ready for production (after testing in staging)
git checkout main
git pull origin main
git merge staging
git push origin main
git tag v1.0.0  # Tag release
git push origin v1.0.0
```

**Note:** Use helper scripts (see below) which handle this workflow automatically.

---

### Phase 2: Multiple Agents (If Needed)

**Strategy:** Hybrid Approach with Task Division

**Option A: Single Local Copy (Sequential)**
- Agents work one at a time
- Use lock file for coordination
- Pull before starting, push when done

**Option B: Multiple Local Copies (Parallel)**
- Each agent has own local copy
- Work on different tickets/areas
- Sync via remote repository
- Merge conflicts resolved via remote or locally

**Recommended:** Option B (Multiple Local Copies) for true parallelism

**Work Division:**
- **Backend Agent:** `app/src/lib/server/**`, `app/src/routes/api/**`
- **Frontend Agent:** `app/src/lib/components/**`, `app/src/routes/app/**`
- **Docs Agent:** `docs/**`
- **Tests Agent:** `app/tests/**`, `app/e2e/**`

**Coordination:**
- Use ticket system to assign work
- Document current work in tickets
- Pull before starting: `git pull origin main`
- Push frequently: `git push origin feature/T-XXX`
- Coordinate merges to avoid conflicts

### Release Process

**Staging → Main (Production Release):**

1. Test thoroughly in staging environment
2. When ready for production:
   ```bash
   git checkout main
   git pull origin main
   git merge staging
   git push origin main
   git tag v1.0.0
   git push origin v1.0.0
   ```

**Hotfix Process:**

1. Create hotfix from main:
   ```bash
   git checkout main
   git pull origin main
   git checkout -b hotfix/critical-bug
   ```

2. Fix and test
3. Merge to both main and staging:
   ```bash
   git checkout main
   git merge hotfix/critical-bug
   git push origin main
   
   git checkout staging
   git merge hotfix/critical-bug
   git push origin staging
   ```

4. Delete hotfix branch

---

## Branch Naming Convention

**Pattern:** `<type>/<ticket>-<description>`

**Examples:**
- `feature/T-001-database-setup`
- `fix/T-015-auth-bug`
- `docs/T-020-update-readme`
- `refactor/T-025-schema-cleanup`

---

## Best Practices

1. **Always pull before starting:**
   ```bash
   git checkout main
   git pull origin main
   ```

2. **Commit frequently:**
   - Small, logical commits
   - Clear commit messages
   - Reference tickets

3. **Push regularly:**
   - Push after each logical unit of work
   - Push before ending work session
   - Push before switching tickets

4. **Test before merging:**
   ```bash
   cd app
   npm test
   npm run build
   ```

5. **Clean up branches:**
   - Delete local branch after merge
   - Delete remote branch after merge

---

## Helper Scripts

**Available Scripts:**
- `scripts/git/start-ticket.sh` - Start work on new ticket (from staging or main)
- `scripts/git/complete-ticket.sh` - Complete and merge ticket (to staging or main)
- `scripts/git/sync-branch.sh` - Sync current branch with remote
- `scripts/git/check-status.sh` - Check current git status
- `scripts/git/release-to-production.sh` - Release staging to production (with optional version tag)

**Usage Examples:**
```bash
# Start ticket (creates branch from staging)
./scripts/git/start-ticket.sh T-001-database-setup

# Complete ticket (merges to staging)
./scripts/git/complete-ticket.sh T-001-database-setup

# Release staging to production
./scripts/git/release-to-production.sh v1.0.0
```

---

**Related:**
- [LLM Agent Project Setup Guide](../LLM_AGENT_PROJECT_SETUP.md#branch-management-strategies)
- [LLM Agent Project Migration Guide](../LLM_AGENT_PROJECT_MIGRATION.md#branch-management-during-migration)

