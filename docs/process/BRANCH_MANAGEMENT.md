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

## Merge Strategies

Different teams and projects prefer different merge strategies when integrating feature branches into main, staging, or dev branches. This section covers the most common approaches and when to use each.

### Overview

**Merge strategies determine:**
- How feature branch commits appear in the target branch history
- Whether individual commits are preserved or combined
- The linearity and cleanliness of the git history
- How easy it is to track changes and revert work

**Key considerations:**
- **History preservation:** Do you want to see all intermediate commits?
- **History cleanliness:** Do you prefer a linear, clean history?
- **Revert granularity:** Do you need to revert individual commits or entire features?
- **Team preference:** What does your team find most useful?

---

### Strategy 1: Merge as-is (Regular Merge)

**Description:** Preserves all commits from the feature branch with a merge commit.

**How it works:**
- Creates a merge commit that combines both branches
- Preserves all individual commits from the feature branch
- Shows the branching structure in history

**Commands:**
```bash
# Merge feature branch to staging/main
git checkout staging  # or main
git pull origin staging
git merge feature/T-001-database-setup
git push origin staging
```

**Git History:**
```
*   Merge commit (feature → staging)
|\
| * Commit 3 (feature branch)
| * Commit 2 (feature branch)
| * Commit 1 (feature branch)
|/
* Previous commit (staging)
```

**Pros:**
- ✅ Preserves complete history of development
- ✅ Easy to see what commits belong to which feature
- ✅ Can revert individual commits if needed
- ✅ Shows branching structure clearly
- ✅ Simple and straightforward

**Cons:**
- ❌ Creates merge commits (can clutter history)
- ❌ Non-linear history (can be harder to follow)
- ❌ Shows all intermediate commits (including "WIP" or "fix typo" commits)

**Best for:**
- Teams that value complete history
- Projects where you need to track individual commits
- When working with multiple agents in parallel
- When you want to preserve the development process

**Recommended for:**
- **Dev branch:** ✅ Good - preserves development history
- **Staging branch:** ⚠️ Depends on team preference
- **Main branch:** ⚠️ Can create cluttered history

---

### Strategy 2: Squash and Merge

**Description:** Combines all feature branch commits into a single commit with a comprehensive message.

**How it works:**
- Squashes all commits from the feature branch into one
- Creates a single commit with a well-crafted message
- Maintains linear history (no merge commit)

**Commands:**
```bash
# Squash merge feature branch to staging/main
git checkout staging  # or main
git pull origin staging
git merge --squash feature/T-001-database-setup
git commit -m "feat(db): add database setup

Complete database setup implementation including:
- PostgreSQL configuration
- Connection pooling
- Migration system setup
- Environment variable management

See: docs/tickets/phase-1/T-001-database-setup.md
Closes: #42"
git push origin staging
```

**Git History:**
```
* Single comprehensive commit (squashed feature)
* Previous commit (staging)
```

**Pros:**
- ✅ Clean, linear history
- ✅ One commit per feature (easy to understand)
- ✅ Comprehensive commit message summarizes entire feature
- ✅ Easy to revert entire feature (single commit)
- ✅ No merge commits

**Cons:**
- ❌ Loses individual commit history
- ❌ Can't revert individual commits from feature
- ❌ Requires writing comprehensive commit message
- ❌ Loses development process details

**Best for:**
- Teams that prefer clean, linear history
- Projects where features are atomic units
- When you want one commit per feature/ticket
- Teams that value simplicity in history

**Recommended for:**
- **Dev branch:** ⚠️ Depends on preference (loses development history)
- **Staging branch:** ✅ Good - clean history for testing
- **Main branch:** ✅ Excellent - clean production history

---

### Strategy 3: Rebase and Merge (Fast-Forward)

**Description:** Replays feature branch commits on top of target branch, then fast-forwards.

**How it works:**
- Rebases feature branch onto target branch
- Replays commits in order
- Creates linear history without merge commit
- Preserves individual commits

**Commands:**
```bash
# Rebase feature branch, then merge
git checkout feature/T-001-database-setup
git rebase staging  # or main
# Resolve any conflicts if needed
git checkout staging
git merge feature/T-001-database-setup  # Fast-forward merge
git push origin staging
```

**Alternative (using pull with rebase):**
```bash
git checkout staging
git pull --rebase origin staging
git merge --ff-only feature/T-001-database-setup
git push origin staging
```

**Git History:**
```
* Commit 3 (feature branch, rebased)
* Commit 2 (feature branch, rebased)
* Commit 1 (feature branch, rebased)
* Previous commit (staging)
```

**Pros:**
- ✅ Linear, clean history
- ✅ Preserves individual commits
- ✅ No merge commits
- ✅ Easy to follow development flow
- ✅ Can revert individual commits

**Cons:**
- ❌ Rewrites commit history (can be problematic if already pushed)
- ❌ Requires resolving conflicts during rebase
- ❌ More complex than regular merge
- ❌ Can be confusing if feature branch was shared

**Best for:**
- Teams that want linear history but preserve commits
- Single-agent workflows (or coordinated multi-agent)
- When feature branches aren't shared
- Projects where clean history is important

**Recommended for:**
- **Dev branch:** ✅ Good - linear history with preserved commits
- **Staging branch:** ✅ Good - clean history for testing
- **Main branch:** ✅ Excellent - clean production history

**⚠️ Important:** Only rebase if the feature branch hasn't been shared/pushed, or coordinate with team if it has been.

---

### Strategy 4: Squash, Rebase, then Merge

**Description:** Combines commits into one, rebases onto target, then merges.

**How it works:**
- First, squash commits on feature branch
- Then rebase onto target branch
- Finally, fast-forward merge
- Results in single commit with linear history

**Commands:**
```bash
# Interactive rebase to squash commits
git checkout feature/T-001-database-setup
git rebase -i staging  # or main
# In editor: change 'pick' to 'squash' for commits to combine
# Write comprehensive commit message

# Then merge (will be fast-forward)
git checkout staging
git merge feature/T-001-database-setup
git push origin staging
```

**Git History:**
```
* Single comprehensive commit (squashed and rebased)
* Previous commit (staging)
```

**Pros:**
- ✅ Cleanest possible history
- ✅ Linear history
- ✅ One commit per feature
- ✅ Comprehensive commit message
- ✅ No merge commits

**Cons:**
- ❌ Most complex process
- ❌ Loses individual commit history
- ❌ Requires interactive rebase
- ❌ Time-consuming for large feature branches

**Best for:**
- Teams that want the cleanest possible history
- Projects where features are always atomic
- When you have time to craft comprehensive messages
- Perfectionist teams

**Recommended for:**
- **Dev branch:** ⚠️ Usually overkill
- **Staging branch:** ✅ Good if you want cleanest history
- **Main branch:** ✅ Excellent for production history

---

### Strategy Comparison Matrix

| Strategy | History Type | Preserves Commits | Merge Commit | Complexity | Revert Granularity |
|----------|-------------|-------------------|--------------|------------|-------------------|
| Merge as-is | Non-linear | ✅ Yes | ✅ Yes | Low | Individual commits |
| Squash and Merge | Linear | ❌ No | ❌ No | Medium | Entire feature |
| Rebase and Merge | Linear | ✅ Yes | ❌ No | Medium-High | Individual commits |
| Squash + Rebase | Linear | ❌ No | ❌ No | High | Entire feature |

---

### Recommended Strategies by Branch

#### For Main Branch (Production)

**Recommended: Squash and Merge** or **Rebase and Merge**

**Rationale:**
- Clean, linear history is important for production
- Easy to understand what changed in each release
- Simple to revert if needed
- Professional appearance

**Example:**
```bash
# Squash merge to main
git checkout main
git pull origin main
git merge --squash feature/T-001-database-setup
git commit -m "feat(db): add database setup

[Comprehensive message describing entire feature]

See: docs/tickets/phase-1/T-001-database-setup.md"
git push origin main
```

#### For Staging Branch

**Recommended: Squash and Merge** or **Rebase and Merge**

**Rationale:**
- Clean history helps with testing and debugging
- One commit per feature makes it easy to test
- Can still see what's being tested

**Example:**
```bash
# Same as main, but to staging
git checkout staging
git pull origin staging
git merge --squash feature/T-001-database-setup
git commit -m "feat(db): add database setup

[Comprehensive message]"
git push origin staging
```

#### For Dev Branch

**Recommended: Merge as-is** or **Rebase and Merge**

**Rationale:**
- Development branch can preserve more history
- Useful to see development process
- Can help with debugging during development
- Less critical to have perfect history

**Example:**
```bash
# Regular merge to dev (preserves history)
git checkout develop
git pull origin develop
git merge feature/T-001-database-setup
git push origin develop
```

---

### Choosing a Strategy

**Use Merge as-is if:**
- ✅ You want complete development history
- ✅ Multiple people work on the same feature branch
- ✅ You need to track individual commits
- ✅ You're working on a dev/integration branch

**Use Squash and Merge if:**
- ✅ You want clean, linear history
- ✅ Features are atomic units
- ✅ You prefer one commit per feature
- ✅ You're merging to staging or main

**Use Rebase and Merge if:**
- ✅ You want linear history AND preserved commits
- ✅ Feature branch hasn't been shared
- ✅ You're comfortable with rebase
- ✅ You want clean history without losing commit details

**Use Squash + Rebase if:**
- ✅ You want the absolute cleanest history
- ✅ You have time for comprehensive commit messages
- ✅ Features are always atomic
- ✅ You're merging to main/production

---

### Team Agreement

**Important:** Choose one strategy (or strategy per branch) and document it clearly:

1. **Document your choice** in this file or project README
2. **Update helper scripts** to use the chosen strategy
3. **Train team members** on the chosen approach
4. **Be consistent** - don't mix strategies randomly

**Example team agreement:**
```markdown
## Merge Strategy

- **Feature → Dev:** Merge as-is (preserve development history)
- **Feature → Staging:** Squash and merge (clean history for testing)
- **Staging → Main:** Squash and merge (clean production history)
- **Hotfix → Main:** Merge as-is (preserve hotfix details)
```

---

### Helper Scripts with Merge Strategies

Update your helper scripts to support different merge strategies:

**Example: `scripts/git/complete-ticket.sh`**
```bash
#!/bin/bash
# Usage: ./complete-ticket.sh T-001-database-setup [merge-strategy]
# Strategies: merge, squash, rebase, squash-rebase

TICKET=$1
STRATEGY=${2:-squash}  # Default to squash
TARGET=${3:-staging}   # Default to staging

git checkout $TARGET
git pull origin $TARGET

case $STRATEGY in
  merge)
    git merge feature/$TICKET
    ;;
  squash)
    git merge --squash feature/$TICKET
    # Prompt for commit message or use template
    git commit -m "feat: [comprehensive message]"
    ;;
  rebase)
    git checkout feature/$TICKET
    git rebase $TARGET
    git checkout $TARGET
    git merge --ff-only feature/$TICKET
    ;;
  squash-rebase)
    git checkout feature/$TICKET
    git rebase -i $TARGET  # Interactive squash
    git checkout $TARGET
    git merge --ff-only feature/$TICKET
    ;;
esac

git push origin $TARGET
git branch -d feature/$TICKET
git push origin --delete feature/$TICKET
```

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

## Branch Cleanup and Deletion

### Should Feature Branches Be Deleted After Merge?

**Short answer: Yes, feature/ticket branches should be deleted after being successfully merged or rebased into main, staging, or dev branches.**

### Why Delete Feature Branches?

**Benefits of deleting:**
- ✅ **Cleaner repository:** Reduces clutter in branch lists
- ✅ **Prevents confusion:** Avoids accidentally working on old branches
- ✅ **Clear status:** Only active branches remain visible
- ✅ **Reduces mistakes:** Can't accidentally merge from old branches
- ✅ **Better organization:** Easier to see what's currently in progress

**History is preserved:**
- ⚠️ **Don't worry:** Deleting a branch doesn't delete commits
- ✅ **Commits remain:** All commits are preserved in the target branch
- ✅ **Can recover:** Branch can be recreated from commit hash if needed
- ✅ **History intact:** Git history shows all merged work

### When to Delete

**Delete immediately after successful merge:**
- ✅ After merging to target branch (main/staging/dev)
- ✅ After verifying merge was successful
- ✅ After pushing merged changes to remote
- ✅ Before starting next ticket/feature

**Don't delete if:**
- ❌ Merge failed or has conflicts (resolve first)
- ❌ Need to reference branch for debugging (delete after issue resolved)
- ❌ Branch contains unmerged work (merge first)
- ❌ Team policy requires keeping branches for a period (follow policy)

### How to Delete Branches

#### Delete Local Branch

**After successful merge:**
```bash
# Delete local branch (safe - only if merged)
git branch -d feature/T-001-database-setup

# Force delete if needed (use with caution)
git branch -D feature/T-001-database-setup
```

**Difference:**
- `-d` (safe delete): Only deletes if branch is fully merged
- `-D` (force delete): Deletes even if not merged (use carefully)

#### Delete Remote Branch

**After successful merge:**
```bash
# Delete remote branch
git push origin --delete feature/T-001-database-setup

# Alternative syntax
git push origin :feature/T-001-database-setup
```

#### Complete Cleanup Workflow

**After merging feature branch:**
```bash
# 1. Merge feature branch (using your chosen strategy)
git checkout staging
git pull origin staging
git merge --squash feature/T-001-database-setup
git commit -m "feat: comprehensive commit message"
git push origin staging

# 2. Delete local branch
git branch -d feature/T-001-database-setup

# 3. Delete remote branch
git push origin --delete feature/T-001-database-setup

# 4. Verify cleanup
git branch -a  # Should not show feature/T-001-database-setup
```

### Cleanup by Merge Strategy

#### After Regular Merge

```bash
git checkout staging
git merge feature/T-001-database-setup
git push origin staging

# Cleanup
git branch -d feature/T-001-database-setup
git push origin --delete feature/T-001-database-setup
```

#### After Squash Merge

```bash
git checkout staging
git merge --squash feature/T-001-database-setup
git commit -m "feat: comprehensive message"
git push origin staging

# Cleanup (same as regular merge)
git branch -d feature/T-001-database-setup
git push origin --delete feature/T-001-database-setup
```

#### After Rebase and Merge

```bash
# Rebase feature branch
git checkout feature/T-001-database-setup
git rebase staging
git checkout staging
git merge --ff-only feature/T-001-database-setup
git push origin staging

# Cleanup (same as regular merge)
git branch -d feature/T-001-database-setup
git push origin --delete feature/T-001-database-setup
```

### Cleanup for Different Branch Types

#### Feature Branches → Dev/Staging/Main

**Always delete after merge:**
- ✅ Feature branches are temporary
- ✅ Work is preserved in target branch
- ✅ Keeps repository clean

#### Hotfix Branches → Main

**Always delete after merge:**
- ✅ Hotfix branches are temporary
- ✅ Fix is preserved in main (and staging/dev)
- ✅ No need to keep hotfix branch

#### Release Branches → Main

**Delete after release:**
- ✅ Release branches are temporary
- ✅ Release is tagged and preserved
- ✅ Delete after successful release

#### Long-Lived Branches

**Don't delete:**
- ❌ `main` - Production branch
- ❌ `staging` - Pre-production branch
- ❌ `develop` - Development branch
- ❌ `legacy` - Legacy code snapshot (if applicable)

### Automated Cleanup

#### Update Helper Scripts

**Example: `scripts/git/complete-ticket.sh`**
```bash
#!/bin/bash
# Complete ticket and cleanup branches

TICKET=$1
TARGET=${2:-staging}
STRATEGY=${3:-squash}

# Merge using chosen strategy
git checkout $TARGET
git pull origin $TARGET

case $STRATEGY in
  merge)
    git merge feature/$TICKET
    ;;
  squash)
    git merge --squash feature/$TICKET
    git commit -m "feat: [comprehensive message]"
    ;;
  # ... other strategies
esac

git push origin $TARGET

# Cleanup: Delete local and remote branches
echo "Cleaning up branches..."
git branch -d feature/$TICKET
git push origin --delete feature/$TICKET

echo "✅ Ticket $TICKET completed and branches cleaned up"
```

#### GitHub/GitLab Auto-Delete

**Configure repository settings:**
- **GitHub:** Settings → Branches → "Automatically delete head branches"
- **GitLab:** Settings → Repository → "Delete source branch" option

**Benefits:**
- ✅ Automatic cleanup after PR/MR merge
- ✅ No manual deletion needed
- ✅ Consistent across team

### Recovery: What If I Need the Branch Again?

**You can recover a deleted branch if needed:**

```bash
# Find the merge commit
git log --oneline --all | grep "feature/T-001"

# Recreate branch from merge commit
git checkout -b feature/T-001-database-setup <merge-commit-hash>

# Or recreate from any commit in the branch
git checkout -b feature/T-001-database-setup <commit-hash>
```

**Note:** Since commits are preserved in the target branch, you can always recreate the branch if needed. However, this is rarely necessary.

### Best Practices Summary

1. **Always delete feature branches after merge:**
   - ✅ Local branch: `git branch -d feature/T-XXX`
   - ✅ Remote branch: `git push origin --delete feature/T-XXX`

2. **Delete immediately after successful merge:**
   - ✅ Don't wait - delete right after merge
   - ✅ Verify merge success first
   - ✅ Then cleanup branches

3. **Automate when possible:**
   - ✅ Use helper scripts
   - ✅ Enable auto-delete in GitHub/GitLab
   - ✅ Make cleanup part of merge workflow

4. **Exception: Keep if needed temporarily:**
   - ⚠️ Only keep if actively debugging merge issues
   - ⚠️ Delete as soon as issue is resolved
   - ⚠️ Don't accumulate old branches

5. **Verify before deleting:**
   - ✅ Confirm merge was successful
   - ✅ Verify changes are in target branch
   - ✅ Check that commits are preserved

### Common Mistakes to Avoid

**❌ Don't:**
- Keep feature branches "just in case" (commits are preserved)
- Delete before verifying merge success
- Force delete merged branches (use `-d` for safety)
- Forget to delete remote branches (causes clutter)

**✅ Do:**
- Delete immediately after successful merge
- Delete both local and remote branches
- Use safe delete (`-d`) to prevent mistakes
- Make cleanup part of your workflow

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

