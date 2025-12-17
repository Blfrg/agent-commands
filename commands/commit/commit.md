# Create multiple atomic git commits by intelligently grouping related changes
<!-- markdownlint-disable-file MD029 MD013 -->

## Core Philosophy: Granular, Ordered, Meaningful History

**Default stance**: When in doubt, **split** into separate commits. Prefer granular commits that tell a clear story.

**Test-Driven Development**: ALL changes follow TDD workflow—tests commit BEFORE implementation (red → green).

**Path-based separation**: Different directories/modules = different micro-intents = separate commits.

**Logical chunks within files**: Use partial staging (`git add -p`) to commit distinct changes within the same file separately.

**Priority-aware ordering**: Critical fixes before features, dependencies before dependents, foundation before extensions.

**Linking without merging**: Related commits share issue references but remain separate. Only the actual implementation uses closing keywords.

## User Input: Issue/PR References

Parse text after `/commit` for issue references to apply to commit footers.

**Examples**: `/commit fixes #123` → Final implementation commit gets `Fixes #123`, others get `Related to #123`

**Closing keywords** (use ONLY on implementation commit): `close[s|d]`, `fix[es|ed]`, `resolve[s|d]`

**Non-closing references** (use on all other commits): `related to`, `refs`, `see`, `see also`, `#123`

## Process

1. **Assess changes**:
   - `git status` → check for renamed entries (file moves)
   - `git diff --summary` → operations overview
   - `git diff` → detailed changes for understanding logical boundaries
   - If >500 lines, provide summary of commit strategy before starting
   - If no changes, report "Working directory clean" and stop

2. **Identify logical commit boundaries** (favor separation):

   Analyze changes to find atomic commit units:

   **File-level boundaries**:
   - Different paths/modules: `src/auth/` vs `src/api/` vs `src/dashboard/`
   - Different files serving different micro-intents

   **Within-file boundaries** (use `git add -p` for partial staging):
   - Distinct function/method changes in same file
   - Multiple unrelated changes to same file (security fix + feature addition)
   - Different logical purposes within same file

   **Example - same file, multiple logical changes**:

```typescript
   // src/auth/session.ts has:
   // - Fixed security vulnerability in token validation
   // - Added new session refresh feature
   // - Updated error messages

   # Should be 3 separate commit series (6 commits total with TDD):

   # Critical security fix (highest priority):
   test(auth): add token validation security tests
   fix(auth): patch token validation vulnerability

   # Feature (after security is patched):
   test(auth): add session refresh tests
   feat(auth): add session refresh capability

   # Chore (lowest priority):
   test(auth): add error message tests
   chore(auth): improve session error messages
```

3. **Determine macro priority groups**:

   Before ordering individual commits, classify all changes into priority tiers. Process in this order:

   **Priority 1 - Critical (commit first)**:
   - **Security fixes**: Vulnerabilities, exploits, authentication issues
   - **Critical bugs**: Data corruption, crashes, system failures
   - **Data integrity**: Migration fixes, data loss prevention

   **Priority 2 - Breaking changes**:
   - **API changes**: Breaking contracts, signature changes
   - **Schema changes**: Database migrations that break compatibility
   - **Config format**: Breaking configuration changes

   **Priority 3 - Features**:
   - **New capabilities**: User-facing features, new functionality
   - **Enhancements**: Improvements to existing features

   **Priority 4 - Bug fixes** (non-critical):
   - **Functional bugs**: Logic errors, incorrect behavior
   - **UI bugs**: Display issues, formatting problems

   **Priority 5 - Improvements**:
   - **Refactoring**: Code structure improvements, no behavior change
   - **Performance**: Optimizations, efficiency improvements

   **Priority 6 - Supporting**:
   - **Documentation**: README, API docs, guides
   - **Chores**: Formatting, linting, dependency updates, config
   - **Build/CI**: Build scripts, CI configuration

   **Example - mixed priority changes**:

```bash
   # Changes present:
   src/auth/token.ts          # Security vulnerability fix
   src/api/users.ts           # New feature: user search
   src/components/Button.tsx  # UI bug: alignment issue
   docs/api.md                # Documentation update

   # Commit order (with TDD):
   1. test(auth): add token validation security tests (Priority 1)
   2. fix(auth): patch token signature bypass vulnerability (Priority 1)
   3. test(api): add user search endpoint tests (Priority 3)
   4. feat(api): add user search endpoint (Priority 3)
   5. test(components): add button alignment tests (Priority 4)
   6. fix(components): correct button vertical alignment (Priority 4)
   7. docs(api): add user search endpoint documentation (Priority 6)
```

4. **Determine dependency order within priority groups**:

   Within each priority tier, order commits by dependencies:

   **TDD ordering (always)**:
   - Test first (red state) → Implementation second (green state)

   **Dependency types**:
   - **Foundation before dependent**: Types/utilities before features that use them
   - **Infrastructure before usage**: Database migrations before API endpoints
   - **Configuration before features**: Config changes before code that uses them
   - **Core before periphery**: Main logic before UI polish

   **Example - Priority 3 (Features) with dependencies**:

```bash
   # Feature: Add user profile system
   # Files changed:
   migrations/002_add_profiles.sql
   src/types/profile.ts
   src/db/profiles.ts
   src/api/profiles.ts

   # Commit order (respecting dependencies + TDD):
   1. feat(db): add user profiles table migration
      # Foundation: schema first

   2. feat(types): add Profile type definitions
      # Foundation: types needed by all layers

   3. test(db): add profile repository tests
      # TDD: test first

   4. feat(db): add profile database operations
      Fixes #123
      # TDD: implementation after test

   5. test(api): add profile endpoint tests
      Related to #123
      # TDD: test first

   6. feat(api): add profile CRUD endpoints
      Related to #123
      # TDD: implementation after test
```

   **Handle circular dependencies pragmatically**:

- If truly circular, commit together with note in body
- Don't get stuck—some changes must be distributed together
- Document dependency in commit message: "Requires abc1234"

5. **Assign issue references** (precision matters):

   For commits related to user-specified issue:

   **Implementation commit** (completes the feature/fix):
   - Use **closing keyword**: `Fixes #123` or `Closes #123` or `Resolves #123`
   - Choose the commit that actually implements the solution

   **All other commits** (tests, docs, dependencies, supporting):
   - Use **non-closing reference**: `Related to #123` or `Refs #123`

   **Multiple priority groups for same issue**:

```bash
   # Issue #123: "Add OAuth with security hardening"

   # Priority 1: Security (if security aspect is critical)
   test(auth): add OAuth token security tests
   Related to #123

   fix(auth): harden OAuth token validation
   Related to #123

   # Priority 3: Feature (main implementation)
   test(auth): add OAuth provider tests
   Related to #123

   feat(auth): add OAuth provider support
   Fixes #123  ← Closing keyword here (completes the issue)

   # Priority 6: Documentation
   docs(auth): add OAuth setup guide
   Related to #123
```

6. **Compose commit message**:

   **Structure**: `<type>(<scope>): <micro-intent description>`

   **Type** (primary change nature):
   - `test`: Test changes (always before implementation)
   - `feat`: New capability or enhancement
   - `fix`: Bug fix (critical or regular)
   - `docs`: Documentation only
   - `refactor`: Code restructure, no behavior change
   - `perf`: Performance improvement
   - `style`: Formatting, whitespace
   - `build`: Build system, dependencies
   - `ci`: CI/CD configuration
   - `chore`: Maintenance, tooling
   - `move`: File relocation/rename

   **Scope** (functional area):
   - Module/feature: `auth`, `api`, `dashboard`, `billing`
   - Component: `Button`, `DataTable`
   - Use path structure: `src/auth/*` → scope is `auth`

   **Subject line** (max 72 chars):
   - Clear micro-intent: "add OAuth provider validation"
   - Imperative mood: "add" not "adds" or "added"
   - Action-oriented: "add", "fix", "patch", "update", "remove"

   **Body** (when valuable):
   - Security context for fixes: "Patches CVE-2024-1234" or "Prevents token replay"
   - Breaking change explanation: What breaks and migration path
   - Dependencies: "Requires abc1234" or "Builds on xyz5678"
   - TDD state for tests: "Tests fail until implementation in next commit"
   - Complex reasoning: Why this approach over alternatives

   **Footer**:
   - Issue reference (closing or non-closing)
   - Breaking changes: `BREAKING CHANGE: description`

   **Template**:

```text
   <type>(<scope>): <micro-intent max 72 chars>

   [Optional body: security context, dependencies, TDD state, why/how]
   [Wrap at 72 characters]

   [Related to #123 | Refs #123 | Fixes #123]
   [BREAKING CHANGE: description]
```

7. **Stage and commit** (in priority + dependency order):

   Process each priority tier in order (Critical → Breaking → Feature → Fix → Improvement → Supporting).

   Within each priority tier, process in dependency order (foundation → dependent).

   Within each logical unit, follow TDD (test → implementation).

   For each commit:

   a. **Stage**:
      - **Full files**: `git add <paths>`
      - **Partial files** (multiple logical changes in one file):
        - `git add -p <path>` → interactively select hunks
        - If interactive fails: `git add <path>` then `git reset -p <path>` to unstage
      - **File moves**: `git add <old-path> <new-path>` together

   b. **Verify staging**:
      - `git diff --staged` → review what will be committed
      - Confirm ONLY changes for this micro-intent are staged
      - For moves: `git diff --staged --summary` must show `rename`

   c. **Commit** with proper quoting:
      - **Breaking changes** (with exclamation): MUST use single quotes
        - Example: git commit -m 'feat(api)!: change endpoint signature'
      - **Normal commits**: either quote style
        - Example: git commit -m "test(api): add endpoint tests"
      - **Multi-line**:
        - Example: git commit -m 'type(scope): subject' -m 'Body' -m 'Related to #123'

   d. **Confirm**: `git log --oneline -1` → verify commit created

8. **Continue**: Repeat step 7 for all priority tiers and their commits

9. **Summary**:
   - `git status` → verify working directory clean
   - `git log --oneline -15 --name-status` → show commit series
   - Highlight commit organization: priority groups, TDD pairs, dependencies

## Partial File Staging

**When multiple logical changes exist in same file**:

Use `git add -p <path>` for interactive hunk selection:

```bash
git add -p src/auth/session.ts

# For each hunk:
# y - stage this hunk
# n - don't stage this hunk
# s - split into smaller hunks
# e - manually edit hunk
# q - quit staging

# Commit staged hunks:
git commit -m "test(auth): add security validation tests"

# Continue with remaining changes:
git add -p src/auth/session.ts
git commit -m "fix(auth): patch security vulnerability"
```

**Fallback if interactive fails**:

```bash
git add src/auth/session.ts
git reset -p src/auth/session.ts  # Unstage unwanted hunks
git commit -m "fix(auth): patch vulnerability"
```

**Decision making**:

- Different functions/methods → separate commits
- Security fix + feature → separate (different priorities)
- Related changes serving one micro-intent → same commit

## File Move Detection & Handling

**Staging moves correctly**:

- `git add <old-path> <new-path>` → auto-detects rename if >50% similar
- Multiple related moves → group in one `move` type commit
- Never stage delete OR create separately

**Verification**: `git diff --staged --summary` must show `rename old => new (X%)`

**If rename not detected** (similarity <50%):

- Lower threshold: `git -c diff.renameLimit=1000 diff --find-renames=30%`
- If still fails: commit as move with note "Heavy modification, history preserved via git log --follow"

## Autonomous Decision Making

**Claude decides automatically**:

**Split into separate commits when**:

- Different paths/modules
- Different logical changes within same file
- Different priority tiers (security ≠ feature)
- Can describe separately without "and"

**Use partial staging when**:

- Same file has security fix + feature
- Same file has multiple unrelated function changes
- Same file has fix + refactor

**Commit together only when**:

- Generated/mechanical pairs (file + immediate export)
- Changes that would break atomically (rename + all references)
- Single logical micro-intent across tightly coupled files

**Handle edge cases decisively**:

- **Debug code found**: Remove it, don't commit it, note removal
- **TODOs added**: Remove or commit separately as `chore` with low priority
- **Console.log/debugger**: Remove before committing
- **Test `.only` or `.skip`**: Remove before committing
- **Circular dependencies**: Commit in best-effort order, document in message
- **Build breaks at commit point**: Acceptable if TDD red state, fix if avoidable
- **Unclear priority**: Use best judgment based on impact/risk

**Don't ask, decide**:

- Trust expertise on commit boundaries
- Trust expertise on priority classification
- Trust expertise on dependency ordering
- Make principled decisions favoring quality and clarity

## Error Handling

**Graceful degradation**:

- `git add -p` fails → fall back to full file, note in commit body
- Merge conflicts → stop, alert user with clear message
- Hooks fail → show error, auto-retry with `--no-verify` if non-critical
- Rename not detected → commit with move note, suggest `git log --follow`
- Partial staging too complex → fall back to full files with detailed commit messages

## Project Conventions

Check in order, apply automatically:

1. `.gitmessage` template → use as commit message template
2. `CONTRIBUTING.md` or `CLAUDE.md` → follow commit guidelines
3. Recent commits: `git log --oneline -30 --name-status`
   - Learn commit granularity patterns
   - Learn scope naming conventions
   - Learn TDD pair frequency
   - Learn priority handling approach
4. Default: Conventional Commits with TDD, priority-ordered, granular micro-intents

## Git History Quality Principles

**Each commit should**:

1. Have single, clear purpose (micro-intent)
2. Follow TDD (test before implementation)
3. Respect priority ordering (critical before features)
4. Be traversable (working or acceptable state at each point)
5. Be cherry-pickable independently (when possible)
6. Reference issues precisely (closing vs non-closing)

**Exemplary commit series**:

```bash
# Issue #123: "Add OAuth with rate limiting"
# Mixed priorities: feature + security enhancement

# Priority 1: Critical (rate limiting is security concern)
abc1234 test(auth): add rate limiting tests for OAuth endpoints
        Related to #123

def5678 fix(auth): add rate limiting to prevent OAuth abuse
        Related to #123

# Priority 3: Feature (main OAuth functionality)
ghi9012 feat(db): add OAuth providers table migration
        Related to #123

jkl3456 feat(types): add OAuth provider type definitions
        Related to #123

mno7890 test(auth): add OAuth provider validation tests
        Related to #123

pqr1234 feat(auth): add OAuth provider validation
        Fixes #123

stu5678 test(api): add OAuth token endpoint tests
        Related to #123

vwx9012 feat(api): add OAuth token endpoints
        Related to #123

# Priority 6: Documentation
yz01234 docs(auth): add OAuth setup and rate limit guide
        Related to #123
```

**Why this is excellent**:

- ✅ Security/rate limiting handled first (highest priority)
- ✅ Feature implementation follows (appropriate priority)
- ✅ Documentation last (lowest priority)
- ✅ All commits follow TDD (test before implementation)
- ✅ Dependencies respected (migration → types → validation → endpoints)
- ✅ Only main implementation uses `Fixes` (precise closure)
- ✅ Each commit focused and traversable
- ✅ Can cherry-pick security fix independently
- ✅ Can bisect to find which component caused issues

**Anti-pattern**:

```bash
abc1234 feat(auth): add OAuth with rate limiting, tests, and docs
        Fixes #123
```

**Why this fails**:

- ❌ Can't see TDD workflow
- ❌ Security and feature mixed (wrong priority handling)
- ❌ Can't cherry-pick just rate limiting
- ❌ Can't bisect which part failed
- ❌ `git blame` loses granularity

**When in doubt, split**. Multiple focused commits in priority order beat one
large commit.
