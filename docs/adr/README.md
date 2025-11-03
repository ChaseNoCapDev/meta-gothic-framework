# Architecture Decision Records (ADRs)

This directory contains Architecture Decision Records (ADRs) for the metaGOTHIC framework.

## What is an ADR?

An Architecture Decision Record (ADR) documents an architectural decision made along with its context and consequences. ADRs help teams:

- Understand why decisions were made
- Onboard new team members
- Revisit decisions when context changes
- Learn from past experiences

## ADR Format

We use a structured format based on Michael Nygard's ADR template:

1. **Title**: Descriptive title (e.g., "Use Mercurius instead of Apollo")
2. **Status**: Proposed → Accepted → (Deprecated | Superseded)
3. **Context**: The situation requiring a decision
4. **Decision**: What was decided
5. **Rationale**: Why this decision was made (options considered)
6. **Consequences**: Positive, negative, and neutral outcomes

See [TEMPLATE.md](./TEMPLATE.md) for the full template.

## Creating an ADR

### 1. Identify the Need

Create an ADR when making decisions about:

- **Architecture**: System structure, component boundaries
- **Technology**: Framework, library, or tool selection
- **Patterns**: Design patterns, coding standards
- **Infrastructure**: Deployment, hosting, CI/CD
- **Process**: Development workflow, testing strategy

### 2. Use the Template

```bash
# Copy template
cp docs/adr/TEMPLATE.md docs/adr/ADR-XXX-your-decision.md

# Fill in all sections
# - Keep it concise but complete
# - Include enough context for future readers
# - Document alternatives considered
```

### 3. Number Sequentially

ADRs are numbered sequentially (ADR-001, ADR-002, etc.) in the order they are created, not by importance.

### 4. Get Review

- Share with team for feedback
- Discuss alternatives and tradeoffs
- Update based on discussion
- Mark as "Accepted" when consensus reached

### 5. Commit to Repository

```bash
git add docs/adr/ADR-XXX-your-decision.md
git commit -m "docs: add ADR-XXX for [decision]"
```

## ADR Status Lifecycle

```
┌─────────┐
│Proposed │  Initial draft, under discussion
└────┬────┘
     │
     ↓
┌─────────┐
│Accepted │  Decision approved and implemented
└────┬────┘
     │
     ├─→ ┌─────────────┐
     │   │ Deprecated  │  No longer recommended, but not replaced
     │   └─────────────┘
     │
     └─→ ┌─────────────┐
         │ Superseded  │  Replaced by a newer ADR
         └─────────────┘
```

## Index of ADRs

### Active Decisions

| ADR | Title | Status | Date |
|-----|-------|--------|------|
| [ADR-001](./ADR-001-example.md) | Use Mercurius over Apollo for GraphQL | Accepted | 2025-01-01 |
| *Add your ADRs here* | | | |

### Superseded Decisions

| ADR | Title | Superseded By | Date |
|-----|-------|---------------|------|
| *None yet* | | | |

## Best Practices

### When to Create an ADR

**DO create an ADR when**:
- Choosing between multiple viable options
- Making a decision with long-term implications
- Changing a previously decided architecture
- Making a decision that affects multiple teams/packages

**DON'T create an ADR when**:
- Implementing a feature (use regular documentation)
- Following an existing pattern
- Making a trivial decision
- Documenting a requirement (not a decision)

### Writing Guidelines

**DO**:
- Write for your future self and new team members
- Include enough context to understand the decision later
- Document alternatives seriously considered
- Be honest about tradeoffs and negative consequences
- Keep it concise (1-3 pages maximum)

**DON'T**:
- Write a novel (be concise)
- Skip the "why" (context and rationale are critical)
- Forget to update status when decisions change
- Leave template sections empty (delete if not applicable)

### Maintaining ADRs

- **Review periodically**: Set review dates for significant decisions
- **Update status**: Mark as Deprecated/Superseded when appropriate
- **Link related ADRs**: Cross-reference when decisions build on each other
- **Keep them immutable**: Don't edit after acceptance (create new ADR instead)

## Example ADR

See [ADR-001-example.md](./ADR-001-example.md) for a sample ADR.

## References

- [Michael Nygard's ADR Article](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions)
- [ADR GitHub Organization](https://adr.github.io/)
- [When to Use ADRs](https://github.com/joelparkerhenderson/architecture-decision-record)

---

*Part of the metaGOTHIC Framework - AI-Guided Opinionated TypeScript Framework*
