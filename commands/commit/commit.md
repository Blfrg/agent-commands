# Create multiple atomic git commits by intelligently grouping related changes

## User Input Handling

If the user provides additional text after the command (e.g., `/commit fixes #123`
or `/commit closes #456, related to #789`), treat this as issue/PR reference
information that should be included in commit message footers.

**Examples**:

- `/commit fixes #123` → Add `Fixes #123` to commit footer
- `/commit closes #456` → Add `Closes #456` to commit footer
- `/commit related to #789` → Add `Related to #789` to commit footer
- `/commit #123 #456` → Add references for both issues

**Supported keywords** (case-insensitive):

- **Auto-closing**
  ([GitHub](https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue)/[GitLab](https://docs.gitlab.com/ee/user/project/issues/managing_issues.html#closing-issues-automatically)):
  `close`, `closes`, `closed`, `fix`, `fixes`, `fixed`, `resolve`, `resolves`,
  `resolved`
- **Non-closing references**: `related to`, `refs`, `see also`, or just `#123`

Keywords can be followed by optional colons (e.g., `Fixes: #123` or `Fixes #123`).

## Initial Assessment

1. Run `git status` to see all changes
2. If no changes found, report "Working directory clean" and stop
3. Run `git diff` to review all unstaged changes
4. If diff is large (>500 lines), inform user and ask if they want a summary
   first

## Change Analysis & Grouping

Analyze changes to identify logical commit groups using this priority order:

1. **Critical fixes**: Security patches, crash fixes, data corruption fixes
2. **Breaking changes**: API changes, schema modifications, config format
   changes
3. **Features**: New functionality (group tightly related files)
4. **Bug fixes**: Non-critical fixes (keep separate from features)
5. **Refactoring**: Code structure improvements (no behavior change)
6. **Tests**: Test additions/updates (can combine with their feature)
7. **Docs**: Documentation updates
8. **Chores**: Formatting, dependencies, config tweaks

## Commit Creation Process

For each logical group, iterate:

### a. Stage Changes

- Use `git add -p <file>` when a file contains mixed changes
- Use `git add <file1> <file2>` when entire files belong together
- Use `git add -i` if you need to stage many partial changes interactively
- If unsure about grouping, describe the changes and ask for guidance

### b. Verify Staging

- Run `git diff --staged` to review what will be committed
- Run `git diff` to see what remains unstaged
- If staged changes seem unrelated, unstage with `git reset` and regroup

### c. Craft Commit Message

**CRITICAL**: Always wrap commit messages in single quotes when they contain
exclamation marks

Format: `<type>(<scope>)<breaking>: <description>`

**Types**: feat, fix, docs, style, refactor, perf, test, build, ci, chore  
**Scope examples**: api, auth, ui, parser, cli, config (optional but
recommended)  
**Breaking change marker**: Use exclamation mark before colon for breaking
changes

**Message structure template** (do not copy literally - adapt content):

```text
<type>(<scope>): <description up to 72 chars>

[Optional body explaining what and why, not how]
[Wrap at 72 characters]

[Fixes #123]
[Closes #456]
[Related to #789]
[BREAKING CHANGE: Description of what breaks and how to migrate]
```

**Issue/PR references**: If the user provided issue or PR references when
invoking the command, include them in the commit footer. Apply the reference to
the most relevant commit(s). If multiple commits are created and the reference
applies to all of them, include it in the final commit that completes the work.

### d. Execute Commit

<!-- markdownlint-disable-next-line MD036-->
**⚠️ BASH SAFETY CRITICAL ⚠️**

For standard commits:

```bash
git commit -m 'type(scope): description'
```

For breaking changes (note the single quotes are MANDATORY):

```bash
# Single quote usage is REQUIRED to prevent bash history expansion
git commit -m 'feat(api)!: change endpoint structure'

# For multi-line commits with body and/or footer:
git commit -m 'feat!: redesign API' -m 'BREAKING CHANGE: endpoints moved from /v1 to /v2'

# For commits with issue references:
git commit -m 'fix(auth): resolve token expiration' -m 'Fixes #123'
```

<!-- markdownlint-disable-next-line MD036-->
**Never use double quotes with exclamation marks in git commands**

### e. Verify Commit

- Run `git log --oneline -1` to show the commit
- If commit message is wrong, use `git commit --amend` to fix it

## Completion

1. Run `git status` to check for remaining changes
2. If changes remain, continue with next logical group
3. When complete, run `git log --oneline -10` to show all commits created
4. Report: "Created N atomic commits" with a summary list

## Decision Guidelines

**Ask user when**:

- Unsure how to group related changes
- Found potential issues (e.g., debug code, console.logs, TODO comments)
- Changes span many files (>10) in different modules
- Commit would be very large (>200 lines changed)

**Proceed automatically when**:

- Changes clearly belong to one feature/fix
- Following obvious patterns from existing commits
- Simple typo or formatting fixes

## Error Handling

- If `git add -p` fails: Fall back to full file staging and explain
- If merge conflicts detected: Stop and alert user
- If hooks fail: Show error and ask whether to bypass with `--no-verify`
- If working on wrong branch: Alert user before committing

## Project Conventions

Check in order of priority:

1. `.gitmessage` template
2. `CONTRIBUTING.md` or `CLAUDE.md`
3. Recent commit history with `git log --oneline -20`
4. If none found, use [Conventional Commits](https://www.conventionalcommits.org/)
   as described above

## Breaking Change Commit Examples

When you need to create a breaking change commit, ALWAYS construct it like this:

- Regular feature: `git commit -m 'feat(auth): add OAuth support'`
- Breaking feature:
  `git commit -m 'feat(auth)!: require API keys for all endpoints'`
- Breaking refactor: `git commit -m 'refactor!: rename all API methods'`

**Final reminder**: The exclamation mark in commit messages MUST be within
single quotes to avoid bash interpreting it as history expansion
