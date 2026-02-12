---
name: harmonyos-review
description: HarmonyOS code review skill for auditing ArkTS projects against official Huawei development guidelines and security best practices. Use when reviewing HarmonyOS applications for: (1) Security compliance (hardcoded credentials, encryption, input validation), (2) ArkTS language standards (hilog usage, type safety, magic numbers), (3) Component lifecycle management (resource cleanup, event subscription handling), (4) State management (V1/V2 decorator consistency), (5) Database operations (ResultSet handling, transaction management, encryption), (6) Permission management (official permission patterns), (7) Performance issues (async forEach, resource leaks), (8) API version compatibility, (9) Kit usage best practices. Generates comprehensive markdown reports with prioritized fix recommendations.
---

# HarmonyOS Code Review

## Overview

This skill audits HarmonyOS ArkTS projects against official Huawei development guidelines and generates prioritized fix reports.

**Review Standards Based On:**
- Official HarmonyOS version compatibility requirements
- Huawei application development guidelines
- Official API reference patterns
- Best practices from Huawei documentation

## Review Process

### 1. Quick Scan (First Pass)

Run these checks in parallel to identify critical issues:

```bash
# Check for hardcoded credentials
grep -r "password\|secret\|key\|token" --include="*.json5" --include="*.ets"

# Check for console instead of hilog
grep -r "console\." --include="*.ets" | grep -v "hilog"

# Check for async forEach issues
grep -r "forEach.*await" --include="*.ets"

# Check target API version
grep -r "compileSdkVersion\|targetSdkVersion" --include="*.json5"

# Check for deprecated API usage patterns
grep -r "@Deprecated\|deprecated" --include="*.ets"
```

### 2. Deep Code Analysis

For each category, read relevant files and apply the checklist from [references/checklist.md](references/checklist.md).

### 3. Generate Report

Create markdown report with:
- Executive summary (issue counts by priority)
- Detailed findings per category
- Prioritized fix recommendations with time estimates
- Overall grade (A-F)

## Category Checklist

See [references/checklist.md](references/checklist.md) for complete review criteria organized by category:

- Security (credentials, encryption, input validation)
- ArkTS Language (hilog, type safety, magic numbers)
- Component Lifecycle (resource cleanup, subscriptions)
- State Management (decorator versions, async race conditions)
- Database (ResultSet, transactions, encryption)
- Permissions (request timing, declarations, official patterns)
- Performance (async patterns, blocking operations)
- Code Quality (function length, duplication, tests)

## Official Documentation References

See [references/official-docs.md](references/official-docs.md) for Huawei official guidelines:

- Version Compatibility (API levels, deprecation policy)
- Kit Usage (Ability Kit, ArkUI, Network Kit, etc.)
- API Reference Patterns (error handling, permissions, SysCap)
- Best Practices (official Huawei recommendations)

## Issue Priority

- **Critical (Red)**: Blocks release, must fix immediately
- **High (Orange)**: Should fix soon, affects quality
- **Medium (Yellow)**: Consider fixing, technical debt
- **Low (White)**: Optional optimization

## Report Template

Use [references/report-template.md](references/report-template.md) as the base structure.

## Exit Criteria

Review complete when:
- All categories in checklist reviewed
- Markdown report generated at `docs/YYYY-MM-DD-review.md`
- Critical and high issues have clear fix suggestions with file:line references
