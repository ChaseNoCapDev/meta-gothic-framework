# Low Priority Tasks

**Purpose**: Track documentation and feature enhancements that are deferred to future iterations.

**Status**: These items are acknowledged but not critical for current development.

---

## Package: github-graphql-client

### Metrics Collection Documentation

**Priority**: LOW
**Effort**: MEDIUM
**Impact**: MEDIUM

**Description**:
The `MetricsCollector` class is implemented but lacks comprehensive documentation. While the class works, detailed usage examples and integration guidance would be helpful for future developers.

**What's Needed**:
- MetricsCollector API documentation
- Examples of collecting and exporting metrics
- Integration patterns with monitoring systems (Datadog, Prometheus, etc.)
- Metric definitions and their meanings

**Timeline**: Future iteration (Q2 2026 or later)

**Notes**: Functionality exists and works. Documentation would be "nice to have" but not blocking current development.

---

## Package: logger

### Log Aggregation Patterns

**Priority**: LOW
**Effort**: LOW
**Impact**: LOW

**Description**:
Winston-based logger with daily rotation is functional. However, guidance on centralized logging and aggregation for production environments would be beneficial.

**What's Needed**:
- Integration examples with ELK Stack
- Datadog transport configuration
- CloudWatch integration
- Production best practices
- Custom transport examples

**Timeline**: When production deployment is planned

**Notes**: Package works well for development. Production logging patterns can be documented when needed for actual deployment.

---

## Package: prompt-toolkit

### YAML Template Support

**Priority**: LOW
**Effort**: HIGH
**Impact**: MEDIUM

**Description**:
Currently listed as "future enhancement" in CLAUDE.md. XML templates work well, but YAML alternative would provide more familiar syntax for some developers.

**What's Needed**:
- YAML parser implementation
- Template converter (XML ↔ YAML)
- YAML schema validation
- Updated documentation
- Migration guide

**Timeline**: Not scheduled (XML templates are sufficient)

**Notes**: XML templates are working and comprehensive. YAML would be syntactic sugar, not essential functionality.

**Considerations**:
- XML has better tooling for complex templates
- YAML might be easier for simple templates
- Would add maintenance burden (two template formats)
- Consider carefully before implementing

---

## General Documentation

### Performance Documentation

**Priority**: LOW
**Effort**: LOW to MEDIUM (depending on benchmarking)
**Impact**: LOW

**Affected Packages**:
- file-system: File size limits, memory usage for large files
- cache: Cache performance characteristics
- context-aggregator: Loading strategy performance
- Others as identified

**What's Needed**:
- Benchmark suite for performance testing
- Performance characteristics documentation
- Memory profiling data
- Best practices for high-load scenarios

**Timeline**: When performance becomes a concern

**Notes**: Current implementations are functional. Performance documentation becomes critical only when:
- Production deployment happens
- Performance issues are reported
- High-load scenarios are encountered

---

## Root Framework

### CI/CD Pipeline Documentation

**Priority**: LOW
**Effort**: MEDIUM
**Impact**: MEDIUM

**Description**:
No CI/CD pipeline currently exists. Documentation would be premature until pipeline is implemented.

**What's Needed** (when CI/CD is implemented):
- Build pipeline overview
- Test automation setup
- Deployment process
- Release workflow
- Badge configuration

**Timeline**: When CI/CD is actually set up

**Notes**: Don't document what doesn't exist. Implement CI/CD first, then document it.

---

## Implementation Guidelines

**When to Move from Low Priority:**

Items should be promoted to active development when:
1. **User Request**: Actual users need the feature/documentation
2. **Production Need**: Deployment requires the documentation
3. **Blocking Issue**: Lack of docs is causing problems
4. **Available Resources**: Time and people available to do quality work

**When NOT to Implement:**

- "Nice to have" documentation for unused features
- Premature optimization documentation
- Features that might never be needed
- Documentation for documentation's sake

**Review Schedule:**

Review this file quarterly to:
- Assess if any items have become higher priority
- Remove items that are no longer relevant
- Add new low-priority items as identified

---

## Notes on Deferred Decisions

### test-helpers and test-mocks

**Decision**: DELETE (completed)
**Rationale**: Empty packages with no implementation. Not needed in current architecture.

### di-framework

**Decision**: REMOVE from monorepo
**Rationale**: No source code, only dist artifacts. Either external package or incorrectly added.
**Action**: Document in removal log, investigate if it's needed as npm dependency.

---

**Last Updated**: 2025-11-02
**Next Review**: 2026-02-01 (Q1 2026)
