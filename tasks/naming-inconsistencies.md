# Naming Inconsistencies Analysis

**Purpose**: Document naming inconsistencies across the meta-gothic-framework to establish consistent patterns.

**How to Use**:
1. Review each inconsistency below
2. Fill in the `<!-- EDIT BELOW -->` sections with your decisions
3. Copy the entire edited document back as a prompt: "Implement the naming decisions in this document: [paste]"

---

## Issue #1: Interface Naming Convention - "I" Prefix

**Current State:**
- Most packages use `I` prefix: `ILogger`, `IFileSystem`, `ICache`, `IEventBus`
- Some interfaces may not follow this pattern consistently
- Industry debate: TypeScript style guide discourages `I` prefix, but C#/Java developers prefer it

**Observed in Packages:**
- ✓ logger: `ILogger`
- ✓ file-system: `IFileSystem`
- ✓ cache: Uses `I` prefix (inferred from patterns)
- ✓ event-system: `IEventBus`
- ✓ claude-client: `IClaudeClient`
- All documented packages follow this pattern

**Decision Required:**
<!-- EDIT BELOW: Choose the standard -->
**Selected Convention:**
[ ] Keep `I` prefix for all interfaces (e.g., `ILogger`, `ICache`)
[ ] Remove `I` prefix, use plain names (e.g., `Logger`, `Cache`)
[ ] Hybrid: Use `I` for public APIs, plain for internal (specify criteria: _______________)

**Rationale:** _______________

**Apply To:**
[ ] All packages globally
[ ] Only new packages
[ ] Gradually refactor existing packages
[ ] Other: _______________

---

## Issue #2: Factory Function Naming

**Current State:**
- Most packages use `create` prefix: `createLogger()`, `createContextAggregator()`, `createPromptToolkit()`
- Inconsistency in what's returned: Some return container, some return service directly
- No clear pattern for async vs sync factories

**Examples:**
- logger: `createLogger()` → returns `ILogger` (sync)
- prompt-toolkit: `createPromptToolkit()` → returns `IResult<{parser, registry, constructor}>` (async)
- context-aggregator: `createContextAggregator()` → returns `Promise<IContextAggregator>` (async)
- github-graphql-client: `createGitHubContainer()` → returns Container (sync), separate `getService()`

**Decision Required:**
<!-- EDIT BELOW: Standardize factory patterns -->
**Factory Naming Standard:**
[ ] Always use `create{ServiceName}()` (e.g., `createLogger()`)
[ ] Use `create{ServiceName}Container()` when returning DI container
[ ] Use `build{ServiceName}()` for complex construction
[ ] Other pattern: _______________

**Return Type Standard:**
[ ] Always return service interface directly (e.g., `ILogger`)
[ ] Always return `{service, container}` object
[ ] Always return DI container, use `container.get()` to extract
[ ] Context-dependent (specify rules: _______________)

**Async/Sync Convention:**
[ ] All factories should be async (return Promise)
[ ] Sync for simple, async for complex (define threshold: _______________)
[ ] Keep current mixed approach
[ ] Other: _______________

---

## Issue #3: Service Class Naming Suffix

**Current State:**
- Varied suffixes across packages: Service, Manager, Client, Handler, Processor, Engine
- No clear distinction when to use which suffix

**Examples:**
- `ClaudeClient` - External API interaction
- `PhaseManager` - Manages phase lifecycle
- `StateMachine` - Engine for workflow
- `ContentLoader` - Loads content
- `TemplateRenderer` - Renders templates
- `EventBus` - Event distribution
- `TaskExecutor` - Executes tasks
- `ContextAggregator` - Aggregates context

**Decision Required:**
<!-- EDIT BELOW: Define suffix usage -->
**Suffix Standards:**

**Client** - Use for:
[ ] External API/service interactions
[ ] Other: _______________

**Manager** - Use for:
[ ] Lifecycle management of entities
[ ] Coordination of multiple services
[ ] Other: _______________

**Engine** - Use for:
[ ] Core processing/state machines
[ ] Complex algorithmic work
[ ] Other: _______________

**Handler** - Use for:
[ ] Event/request processing
[ ] Single-responsibility processors
[ ] Other: _______________

**Processor** - Use for:
[ ] Data transformation pipelines
[ ] Stream processing
[ ] Other: _______________

**Service** - Use for:
[ ] General business logic
[ ] Orchestration layer
[ ] Other: _______________

**Executor** - Use for:
[ ] Task/command execution
[ ] Worker patterns
[ ] Other: _______________

**[No Suffix]** - Use for:
[ ] Simple utilities (e.g., `EventBus`, `Logger`)
[ ] Domain models
[ ] Other: _______________

**Custom Rules:** _______________

---

## Issue #4: Package Naming Convention

**Current State:**
- All packages use kebab-case: `claude-client`, `context-aggregator`, `file-system`
- All use singular nouns except utilities
- Scope: `@chasenocap/` for all packages

**Inconsistencies Identified:**
- `test-helpers` vs `test-mocks` - Plural helpers, plural mocks (consistent)
- `ui-components` - Plural (makes sense for collection)
- Most others singular: `logger` not `loggers`, `cache` not `caches`

**Decision Required:**
<!-- EDIT BELOW: Confirm conventions -->
**Package Name Format:**
[ ] Always singular (e.g., `component`, `helper`)
[ ] Plural for collections/sets (e.g., `components`, `helpers`)
[ ] **Current approach is correct** (mostly singular, plural for collections)

**Scope/Organization:**
[ ] Keep `@chasenocap/` for all
[ ] Split into multiple scopes (specify: _______________)
[ ] Other: _______________

**Special Cases:** _______________

---

## Issue #5: Directory Structure Terminology

**Current State:**
- Most packages use: `src/interfaces/`, `src/implementations/`, `src/types/`, `src/utils/`
- Some variations: `src/strategies/`, `src/factories/`, `src/schemas/`
- Inconsistent use of `src/services/` vs implementations

**Examples:**
- prompt-toolkit: `implementations/XmlTemplateParser.ts`
- context-aggregator: `implementations/ContextAggregator.ts`
- graphql-toolkit: `implementations/SchemaComposer.ts`
- Some might use `services/` folder

**Decision Required:**
<!-- EDIT BELOW: Standardize folder structure -->
**Standard Folder Names:**

**Interface definitions:**
[ ] `src/interfaces/` (current standard)
[ ] `src/contracts/`
[ ] Other: _______________

**Concrete implementations:**
[ ] `src/implementations/` (current standard)
[ ] `src/services/`
[ ] Both (use when: _______________)
[ ] Other: _______________

**Type definitions:**
[ ] `src/types/` (current)
[ ] `src/models/`
[ ] `src/dto/` (for data transfer objects specifically)
[ ] Other: _______________

**Utilities/Helpers:**
[ ] `src/utils/` (current)
[ ] `src/helpers/`
[ ] `src/common/`
[ ] Other: _______________

**Additional Standard Folders:**
[ ] `src/strategies/` for strategy pattern implementations
[ ] `src/factories/` for factory functions
[ ] `src/decorators/` for TypeScript decorators
[ ] `src/middleware/` for middleware/plugins
[ ] Other: _______________

---

## Issue #6: Test File Naming and Location

**Current State:**
- Most packages use separate `tests/` directory
- event-system uses co-located tests (src/EventBus.test.ts)
- Inconsistent test file naming: `.test.ts` vs `.spec.ts`

**Observed Patterns:**
- Separate `tests/` directory: claude-client, context-aggregator, file-system, etc.
- Co-located in `src/`: event-system
- Test naming appears to be `.test.ts` primarily

**Decision Required:**
<!-- EDIT BELOW: Standardize test patterns -->
**Test Location:**
[ ] Separate `tests/` directory mirroring `src/` structure
[ ] Co-located with source files (e.g., `Component.ts` + `Component.test.ts`)
[ ] Hybrid (unit tests co-located, integration tests separate)
[ ] Other: _______________

**Test File Naming:**
[ ] `*.test.ts` (Jest convention)
[ ] `*.spec.ts` (Angular/Jasmine convention)
[ ] `*.test.tsx` for React components
[ ] Keep current mixed approach
[ ] Other: _______________

**Test Organization:**
[ ] Flat structure in `tests/`
[ ] Mirror `src/` folder structure exactly
[ ] Organize by type: `tests/unit/`, `tests/integration/`, `tests/e2e/`
[ ] Other: _______________

---

## Issue #7: Configuration File Naming

**Current State:**
- All packages have `tsconfig.json`
- Most have `package.json` with scripts
- Root has `.gitignore`, `.npmrc.template`
- Docs in `docs/` folder (root) vs package-level docs

**Inconsistencies:**
- Some packages missing `tsconfig.json`: cache, di-framework, test-helpers, test-mocks
- Some missing README: (most have it)
- Inconsistent use of CLAUDE.md (some packages don't have it)

**Decision Required:**
<!-- EDIT BELOW: Required files for all packages -->
**Required Files (All Packages Must Have):**
[ ] `package.json` ✓ (all have)
[ ] `tsconfig.json` ✓ (add to missing packages)
[ ] `README.md` ✓ (standardize format)
[ ] `CLAUDE.md` ✓ (add to all packages)
[ ] `.gitignore` (specify: _______________)
[ ] `CHANGELOG.md` (if versioned independently)
[ ] `LICENSE` (root only or per-package: _______________)
[ ] Other: _______________

**Optional Files (Use When Appropriate):**
[ ] `.npmrc.template` (for auth)
[ ] `.npmignore` (publishing control)
[ ] `jest.config.js` (if custom test config needed)
[ ] `.eslintrc.js` (if custom lint rules)
[ ] Other: _______________

---

## Issue #8: Export Pattern Naming

**Current State:**
- Most packages have `src/index.ts` as main entry point
- Inconsistent re-export patterns
- Some use barrel exports, some selective exports

**Examples:**
- Some packages export everything: `export * from './interfaces'`
- Some selective: `export { ILogger, createLogger } from './factories'`
- Injection tokens exported differently: `TYPES`, `TOKENS`, `INJECTION_TOKENS`

**Decision Required:**
<!-- EDIT BELOW: Standardize exports -->
**Main Entry Point:**
[ ] Always `src/index.ts` (current standard)
[ ] Package-specific main file
[ ] Other: _______________

**Barrel Export Style:**
[ ] Export all: `export * from './module'`
[ ] Selective named exports: `export { Class1, Class2 }`
[ ] Hybrid (explain: _______________)

**Injection Token Naming:**
[ ] `TYPES` (following inversify convention)
[ ] `TOKENS`
[ ] `INJECTION_TOKENS`
[ ] `{PACKAGE_NAME}_TYPES` (e.g., `LOGGER_TYPES`, `CACHE_TYPES`)
[ ] Other: _______________

**Internal vs Public Exports:**
[ ] Mark internal with `@internal` JSDoc
[ ] Use separate `internal.ts` file
[ ] Prefix with underscore: `_InternalClass`
[ ] No internal exports (all public)
[ ] Other: _______________

---

## Issue #9: Error Class Naming

**Current State:**
- Some packages have custom errors: `FileSystemError`, `PhaseExecutionError`
- Inconsistent error naming patterns
- Not all packages define custom errors

**Decision Required:**
<!-- EDIT BELOW: Standardize error classes -->
**Error Naming Pattern:**
[ ] `{Domain}Error` (e.g., `FileSystemError`, `ValidationError`)
[ ] `{Action}Error` (e.g., `ReadFileError`, `ParseError`)
[ ] `{Package}{Type}Error` (e.g., `LoggerConfigurationError`)
[ ] Other: _______________

**Error Organization:**
[ ] All errors in `src/errors/` folder
[ ] Co-located with related code
[ ] Single `src/errors.ts` file
[ ] Other: _______________

**When to Create Custom Errors:**
[ ] Always create custom error types
[ ] Only for domain-specific errors with additional context
[ ] Never, use standard Error with custom messages
[ ] Other: _______________

---

## Issue #10: Documentation File Terminology

**Current State:**
- Root CLAUDE.md: Framework-level context
- Package CLAUDE.md: Package-specific context
- README.md: User-facing documentation
- docs/: Additional documentation (ADRs, guides)

**Inconsistencies:**
- Not all packages have CLAUDE.md (missing: cache, di-framework, test-helpers, test-mocks, ui-components)
- No standard template for CLAUDE.md
- README.md format varies

**Decision Required:**
<!-- EDIT BELOW: Documentation standards -->
**Required Documentation per Package:**
[ ] CLAUDE.md - AI development context ✓
[ ] README.md - User guide ✓
[ ] CHANGELOG.md - Version history
[ ] API.md - API reference
[ ] CONTRIBUTING.md (root only)
[ ] Other: _______________

**CLAUDE.md Standard Sections (Define Required Sections):**
[ ] Package Identity (name, purpose, status)
[ ] Single Responsibility (what it does/doesn't do)
[ ] Architecture (core components)
[ ] Usage Patterns (code examples)
[ ] Testing Strategy
[ ] Integration Points
[ ] Performance Considerations
[ ] Security Considerations
[ ] Future Enhancements
[ ] Custom sections: _______________

**README.md Standard Sections:**
[ ] Installation
[ ] Quick Start
[ ] API Overview
[ ] Examples
[ ] Contributing
[ ] License
[ ] Other: _______________

---

## Summary: Apply Changes

**After filling in all decisions above, use this section to confirm scope:**

**Packages to Update:**
[ ] All 16 packages
[ ] Only packages missing standards (specify: _______________)
[ ] Prioritize by health score (RED first, then YELLOW, then GREEN)
[ ] Manual selection (list: _______________)

**Implementation Approach:**
[ ] Automated script/migration
[ ] Manual updates package-by-package
[ ] Gradual refactoring over time
[ ] Other: _______________

**Breaking Changes:**
[ ] Accept breaking changes, bump major versions
[ ] Maintain backward compatibility where possible
[ ] Create migration guide for breaking changes
[ ] Other: _______________

---

## Ready to Implement

**When you've filled in all decisions, copy this entire document and use this prompt:**

```
"Implement the naming standardization decisions in the attached document.
For each inconsistency, apply the selected convention across all specified packages.
Create migration scripts where needed and update all documentation.

[PASTE EDITED DOCUMENT HERE]"
```
