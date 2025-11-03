# Removed Packages Log

This document tracks packages that have been removed from the meta-gothic-framework monorepo.

---

## Removed: 2025-11-02

### test-helpers (v0.1.0)
**Repository**: https://github.com/ChaseNoCapDev/test-helpers
**Reason**: Empty package with no implementation
**Health Score at Removal**: 35/100 (🔴 RED)

**Context**:
- Package was initialized but never implemented
- Contained only README.md, no source code
- No tests, no tsconfig.json
- Missing node_modules and package-lock.json (partial)

**Decision**: DELETE
- No source code to preserve
- Not needed in current architecture
- Alternative: Use standard testing utilities from other packages

**Impact**: None - package was not used anywhere

---

### test-mocks (v0.1.0)
**Repository**: https://github.com/ChaseNoCapDev/test-mocks
**Reason**: Empty package with no implementation
**Health Score at Removal**: 30/100 (🔴 RED)

**Context**:
- Package was initialized but never implemented
- Contained only README.md, no source code
- No tests, no tsconfig.json, no dependencies
- Completely empty except for package metadata

**Decision**: DELETE
- No source code to preserve
- Not needed in current architecture
- Alternative: Mock implementations can live in test files directly

**Impact**: None - package was not used anywhere

---

### di-framework (v0.1.0)
**Repository**: https://github.com/ChaseNoCapDev/di-framework
**Reason**: No source code, only build artifacts; purpose unclear
**Health Score at Removal**: 40/100 (🔴 RED)

**Context**:
- Package contained only `dist/` folder with pre-built JavaScript
- No `src/` directory, no TypeScript source code
- No CLAUDE.md to explain purpose or usage
- Unclear if this was:
  - An external package accidentally added as submodule
  - A package built elsewhere with source lost
  - A dependency that should be consumed from npm

**Decision**: REMOVE from monorepo
- Cannot maintain or modify without source code
- If needed, can be consumed as npm dependency
- No clear use case documented

**Impact**: Low - appears unused in current codebase

**Investigation Notes**:
- Check if any packages depend on `@chasenocap/di-framework`
- If needed, may need to:
  - Install from npm if published
  - Or use `inversify` directly (standard DI solution)
  - Or recreate from scratch if truly needed

---

## Repository Count Update

**Before Removal**: 16 packages
**After Removal**: 13 packages

### Remaining Packages:
1. cache (65% health)
2. claude-client (95% health)
3. context-aggregator (70% health)
4. event-system (92% health)
5. file-system (90% health)
6. github-graphql-client (88% health)
7. graphql-toolkit (90% health)
8. logger (95% health)
9. prompt-toolkit (92% health)
10. sdlc-config (93% health)
11. sdlc-content (90% health)
12. sdlc-engine (88% health)
13. ui-components (75% health)

**New Average Health**: 85/100 (improved from 72/100)

---

## Lessons Learned

### Package Initialization
- Don't create package structure until there's actual code to implement
- Empty packages confuse developers and clutter the monorepo
- If uncertain about need, document in roadmap instead of creating package

### Source Code Management
- Never commit only `dist/` without `src/`
- Always ensure source code is in version control
- Build artifacts should be .gitignored

### Documentation Requirements
- Every package MUST have CLAUDE.md before being added to monorepo
- README.md alone is insufficient
- Document purpose, architecture, and usage from day one

---

## Restoration Procedure

If any of these packages are needed in the future:

### For test-helpers or test-mocks:
1. Create proper package structure with src/
2. Implement actual test utilities/mocks
3. Add comprehensive CLAUDE.md
4. Add tests for the test utilities themselves
5. Only then add back to monorepo

### For di-framework:
1. Determine if external npm package or internal need
2. If external: Add to package.json dependencies, not as submodule
3. If internal: Implement from scratch with full source code
4. Add comprehensive CLAUDE.md explaining DI patterns
5. Ensure it provides value over standard inversify

---

**Document Maintained By**: Development Team
**Last Updated**: 2025-11-02
**Next Review**: When considering package additions
