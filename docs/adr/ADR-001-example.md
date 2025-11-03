# ADR-001: Use Mercurius over Apollo for GraphQL

**Status**: Accepted
**Date**: 2025-01-01
**Authors**: metaGOTHIC Team

## Context

The metaGOTHIC framework requires a GraphQL server implementation for its API layer. We need to choose between different GraphQL server libraries for Node.js.

**Requirements**:
- High performance (target: <10ms p95 latency for simple queries)
- Federation support for microservices architecture
- Subscription support for real-time features
- TypeScript support
- Active maintenance and community
- Integration with our Fastify-based services

**Constraints**:
- Must work with Fastify (our chosen HTTP framework)
- Need to support 1000+ concurrent connections
- Budget constraints favor open-source solutions
- Team has prior Node.js/TypeScript experience

## Decision

We will use **Mercurius** as our GraphQL server implementation.

## Rationale

### Options Considered

#### Option 1: Apollo Server

**Description**: Popular GraphQL server from Apollo GraphQL. Market leader with largest community.

**Pros**:
- Largest ecosystem and community
- Extensive documentation and examples
- Many plugins and integrations available
- Team has prior experience
- Strong commercial support available

**Cons**:
- Performance overhead (Express-based, even with Fastify adapter)
- Heavier weight (~500KB vs ~50KB for Mercurius)
- Federation v2 requires Apollo paid plan
- Slower query execution (benchmarks show 2-3x slower than Mercurius)
- More complex setup for subscriptions

**Benchmarks**: ~1500 req/sec for simple queries

#### Option 2: Mercurius *(Selected)*

**Description**: High-performance GraphQL adapter for Fastify, built with performance as primary goal.

**Pros**:
- **Performance**: 3x faster than Apollo Server (benchmarks: ~4500 req/sec)
- **Fastify Integration**: Native Fastify plugin, zero overhead
- **Federation**: Free, built-in Federation v1 support
- **Subscriptions**: WebSocket support built-in
- **Lightweight**: Minimal dependencies, small bundle size
- **TypeScript**: Full TypeScript support
- **JIT Compilation**: Query compilation for even better performance
- **Caching**: Built-in response caching via @mercuriusjs/cache

**Cons**:
- Smaller community than Apollo
- Less third-party tooling
- Federation v2 not yet supported (v1 sufficient for our needs)
- Team learning curve (no prior experience)

**Benchmarks**: ~4500 req/sec for simple queries

**Why Selected**:
- **Performance is critical**: Our target of <10ms p95 latency requires the best performance
- **Fastify alignment**: We're already committed to Fastify, Mercurius is purpose-built for it
- **Cost savings**: Free federation vs paid Apollo federation
- **Sufficient features**: Federation v1 meets our microservices needs
- **Growing adoption**: Increasing community momentum

#### Option 3: GraphQL Yoga

**Description**: Batteries-included GraphQL server built on top of Envelop plugin system.

**Pros**:
- Flexible plugin system
- Works with multiple HTTP frameworks
- Good TypeScript support
- Federation support via plugins

**Cons**:
- Performance between Apollo and Mercurius
- More complex plugin configuration
- Less Fastify-optimized than Mercurius
- Smaller community than Apollo

**Benchmarks**: ~2500 req/sec for simple queries

## Consequences

### Positive Consequences

- **Better Performance**: 3x improvement in query throughput enables handling more load with fewer servers
- **Lower Costs**: Reduced server costs due to higher efficiency; no federation licensing costs
- **Simplified Stack**: Single framework (Fastify) for both HTTP and GraphQL reduces complexity
- **Future-Proof**: JIT compilation and caching give headroom for growth

### Negative Consequences

- **Learning Curve**: Team needs to learn Mercurius-specific patterns (estimated 1-2 weeks)
- **Smaller Ecosystem**: Fewer third-party plugins; may need to build custom solutions
- **Documentation Gap**: Less Stack Overflow content; reliance on official docs
- **Migration Risk**: If we need to switch later, migration effort would be significant

### Neutral/Unknown

- **Community Growth**: Mercurius community is growing but future trajectory uncertain
- **Federation v2**: Timeline for Federation v2 support unclear; may need workarounds if required
- **Enterprise Support**: No commercial support option if needed in future

## Implementation

### Action Items

- [x] Evaluate Mercurius in prototype (completed)
- [x] Benchmark against Apollo Server (completed)
- [ ] Create Mercurius integration in graphql-toolkit package
- [ ] Document Mercurius patterns in CLAUDE.md files
- [ ] Create example implementations
- [ ] Train team on Mercurius (workshop session)
- [ ] Migrate existing GraphQL services

### Migration Strategy

**Phase 1: Foundation** (Week 1-2)
1. Create graphql-toolkit package with Mercurius integration
2. Document common patterns and utilities
3. Build example service

**Phase 2: Migration** (Week 3-4)
1. Migrate claude-service to Mercurius
2. Migrate git-service to Mercurius
3. Migrate github-adapter to Mercurius

**Phase 3: Optimization** (Week 5-6)
1. Implement JIT compilation
2. Add response caching
3. Optimize federation query planning

### Validation

Success criteria after 3 months:

- **Performance**: p95 latency < 10ms for simple queries ✓ (Target met)
- **Reliability**: 99.9% uptime for GraphQL services
- **Developer Productivity**: <1 day to create new GraphQL service
- **Cost**: 30% reduction in server costs vs Apollo baseline

**Actual Results** (to be updated):
- Performance: TBD
- Reliability: TBD
- Productivity: TBD
- Cost: TBD

## References

- [Mercurius Documentation](https://mercurius.dev/)
- [Benchmark Results](https://github.com/mercurius-js/mercurius/blob/master/benchmarks/README.md)
- [Fastify Performance](https://www.fastify.io/benchmarks/)
- [Federation Comparison](./ADR-001-federation-comparison.md) *(hypothetical)*

## Notes

### Review Schedule

This decision should be reviewed: **July 2025** (6 months after adoption)

Review criteria:
- Has Federation v2 become necessary?
- Has community/ecosystem grown as expected?
- Are performance targets being met?
- Has Apollo Server improved significantly?

### Discussion

Key points from team discussion:

- Consensus that performance is our top priority
- Acknowledgment of learning curve but confidence in team's ability
- Agreement that Federation v1 is sufficient for current architecture
- Decision to create comprehensive documentation to mitigate ecosystem gap

**Decision Date**: January 1, 2025
**Team Vote**: 4 in favor, 0 against, 1 abstain (absent)

---

*This is an example ADR to demonstrate format and style*
