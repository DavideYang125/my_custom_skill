---
name: auto-review
description: Iterative code review cycle. Use when the user says "继续 review", "auto review", "继续审查", or asks to continue a review cycle. Automates reading fix summaries, verifying fixes in actual code, tracking unresolved issues, and writing the next review document.
---

# Auto Review

Iterative code review cycle: read fix summary → verify code → find new issues → write next review.

## Workflow

### 1. Determine current round
- Scan the `review/` directory in the current project for the highest-numbered `第N次Review.md`
- Set current round to N
- If no review files exist, tell the user to create the first review manually or provide the initial commit range

### 2. Check if fix summary is ready
- Check for `review/第N次Review修复总结.md`
- If not found, tell the user: `第N次Review修复总结.md has not been created yet. Please fix the issues and write the fix summary first.`
- If the file exists but was modified less than 15 seconds ago, wait until 15 seconds have passed since last modification (indicates writing is still in progress)

### 3. Execute review

#### 3.1 Read fix summary
- Fully read `review/第N次Review修复总结.md`
- Record all fixed and unfixed issues

#### 3.2 Verify fixed code
- Read each actual code file listed in the fix summary
- Verify each fix is correctly implemented
- Flag incomplete fixes or new issues introduced by fixes
- Reference specific file paths and line numbers

#### 3.3 Check unfixed issues
- For issues marked "unfixed" in previous rounds, check if they still exist in current code
- Update assessment if situation has changed

#### 3.4 Find new issues
- Read through all modified code files to find new bugs, logic errors, security issues
- Check for missed fixes or inconsistencies

### 4. Write output
- Write results to `review/第N+1次Review.md`
- Follow consistent format:
  - **一、上一次 Review 修复验证** (verified fixes / incomplete fixes)
  - **二、新发现的问题** (categorized by severity)
  - **三、未修复问题追踪表** (tracking table)
  - **四、总结** (summary with prioritized fix suggestions)

### 5. Guidelines
- Always read actual code — do not rely solely on fix summary descriptions
- Cite specific file paths and line numbers for every finding
- Severity levels: 🔴 高 (high) / ⚠️ 中 (medium) / ⚠️ 低 (low)
- Be objective: mark fixes as ✅ when correct; do not fabricate issues
- If code quality is already good with no new issues, state the positive conclusion directly — do not force findings
- Track unresolved issues across rounds in a table format
