# metaGOTHIC Development Guide

Comprehensive guide for developing with the metaGOTHIC framework.

## Getting Started

### Prerequisites

- Node.js 18+ and npm
- Git 2.30+
- GitHub account with access to ChaseNoCapDev organization

### Initial Setup

```bash
# Clone with all submodules
git clone --recurse-submodules https://github.com/ChaseNoCapDev/meta-gothic-framework.git
cd meta-gothic-framework

# Or initialize submodules after cloning
git submodule update --init --recursive

# Install dependencies
npm install

# Build all packages
npm run build
```

## Working with Submodules

### Understanding the Structure

metaGOTHIC uses Git submodules for package management:

```
meta-gothic-framework/           # Meta repository
├── packages/
│   ├── cache/                  # Submodule → github.com/ChaseNoCapDev/cache
│   ├── event-system/           # Submodule → github.com/ChaseNoCapDev/event-system
│   └── ...                     # 13 total package submodules
└── docs/                       # Framework-level documentation
```

**Important**: Each package is its own git repository tracked as a submodule.

### Updating Submodules

```bash
# Update all submodules to latest from remote
git submodule update --recursive --remote

# Update specific submodule
cd packages/cache
git pull origin main
cd ../..

# Commit submodule reference update
git add packages/cache
git commit -m "Update cache submodule to latest version"
```

### Making Changes to Packages

```bash
# Navigate to package
cd packages/event-system

# Create feature branch
git checkout -b feat/new-feature

# Make changes, commit
git add src/
git commit -m "feat: add new event handler"

# Push to package repository
git push origin feat/new-feature

# Return to root and commit submodule pointer update
cd ../..
git add packages/event-system
git commit -m "Update event-system submodule with new feature"
```

### Hierarchical Commit Strategy

**Critical**: Always commit in this order:

1. **Commit in submodule first** (package repository)
2. **Push submodule changes**
3. **Commit submodule reference in parent** (framework repository)
4. **Push parent changes**

```bash
# ✅ CORRECT
cd packages/cache
git add .
git commit -m "feat: add new cache strategy"
git push

cd ../..
git add packages/cache
git commit -m "Update cache submodule with new strategy"
git push

# ❌ WRONG - Don't commit parent first!
git add .
git commit -m "Update everything"  # Creates dirty submodule state
```

**Helper Scripts**:

```bash
# Use npm scripts for proper commit order
npm run git:commit "feat: your message"  # Commits submodules, then parent
npm run git:push                        # Pushes in correct order
npm run git:sync "feat: your message"   # Commit + push together
```

## Building and Testing

### Building Packages

```bash
# Build all packages
npm run build

# Build specific package
cd packages/graphql-toolkit
npm run build

# Watch mode for development
npm run build:watch
```

### Running Tests

```bash
# Run all tests
npm test

# Run tests with coverage
npm run test:coverage

# Run tests for specific package
cd packages/sdlc-engine
npm test

# Watch mode
npm run test:watch
```

### Linting and Formatting

```bash
# Lint all packages
npm run lint

# Fix linting issues
npm run lint:fix

# Format code with Prettier
npm run format
```

## Package Development

### Creating a New Package

1. **Create repository** in ChaseNoCapDev organization
2. **Initialize package**:

```bash
cd packages
git submodule add https://github.com/ChaseNoCapDev/new-package.git new-package
cd new-package

# Set up package structure
npm init -y
mkdir -p src tests docs

# Create package.json, tsconfig.json, etc.
```

3. **Add to monorepo**:

```bash
cd ../..
git add .gitmodules packages/new-package
git commit -m "Add new-package submodule"
git push
```

### Package Structure

Standard package layout:

```
package-name/
├── src/
│   ├── interfaces/          # TypeScript interfaces
│   ├── implementations/     # Concrete implementations
│   ├── types/              # Type definitions
│   ├── utils/              # Utilities
│   └── index.ts            # Public API
├── tests/
│   ├── unit/               # Unit tests
│   └── integration/        # Integration tests
├── docs/
│   └── examples/           # Usage examples
├── CLAUDE.md               # AI development context
├── README.md               # User documentation
├── package.json
├── tsconfig.json
└── vitest.config.ts
```

### Documentation Requirements

Every package must have:

1. **CLAUDE.md**: AI/Claude development context
   - Package purpose and responsibilities
   - Architecture and design patterns
   - Integration examples
   - Common tasks and patterns

2. **README.md**: User-facing documentation
   - Installation instructions
   - Quick start guide
   - API reference
   - Examples

3. **Inline Documentation**: JSDoc comments for public APIs

### Testing Standards

- **Minimum 80% code coverage** (target 90%+)
- Unit tests for all public APIs
- Integration tests for complex interactions
- Use Vitest for testing framework

```typescript
// Example test structure
describe('MemoryCache', () => {
  let cache: ICache;

  beforeEach(() => {
    cache = new MemoryCache();
  });

  describe('set/get', () => {
    it('should store and retrieve values', async () => {
      await cache.set('key', 'value');
      const result = await cache.get('key');
      expect(result).toBe('value');
    });

    it('should respect TTL expiration', async () => {
      await cache.set('key', 'value', 100);
      await sleep(150);
      const result = await cache.get('key');
      expect(result).toBeNull();
    });
  });
});
```

## Coding Standards

### TypeScript Guidelines

- Use strict mode: `"strict": true`
- Explicit return types on functions
- Avoid `any` type (use `unknown` if needed)
- Use interfaces over types for public APIs
- Leverage generics for reusable components

```typescript
// ✅ Good
export async function fetchUser(id: string): Promise<User | null> {
  const user = await db.findOne({ id });
  return user ?? null;
}

// ❌ Bad
export async function fetchUser(id) {
  return await db.findOne({ id });
}
```

### Naming Conventions

- **Interfaces**: `IServiceName`, `ICache`, `ILogger`
- **Classes**: `ServiceName`, `MemoryCache`, `ConsoleLogger`
- **Types**: `PascalCase` for type names
- **Functions**: `camelCase` for function names
- **Constants**: `UPPER_SNAKE_CASE` for constants
- **Files**: Match class/interface name (`MemoryCache.ts`)

### Dependency Injection

Use Inversify for DI:

```typescript
import { injectable, inject } from 'inversify';
import { TYPES } from './types';

@injectable()
export class UserService implements IUserService {
  constructor(
    @inject(TYPES.IDatabase) private database: IDatabase,
    @inject(TYPES.ILogger) private logger: ILogger,
    @inject(TYPES.ICache) private cache: ICache
  ) {}

  async getUser(id: string): Promise<User> {
    const cached = await this.cache.get<User>(`user:${id}`);
    if (cached) return cached;

    const user = await this.database.findUser(id);
    await this.cache.set(`user:${id}`, user, 300000);
    return user;
  }
}
```

## GraphQL Development

### Using Mercurius

metaGOTHIC uses Mercurius (not Apollo) for GraphQL:

```typescript
import Fastify from 'fastify';
import mercurius from 'mercurius';

const app = Fastify();

await app.register(mercurius, {
  schema,
  resolvers,
  graphiql: true
});

await app.listen({ port: 3000 });
```

### Federation Patterns

See [graphql-toolkit documentation](../packages/graphql-toolkit/README.md) for federation setup.

## Common Tasks

### Adding a New Feature

1. Create feature branch in package repository
2. Implement feature with tests
3. Update documentation (CLAUDE.md, README.md)
4. Run tests and linting
5. Create pull request
6. After merge, update submodule reference in framework

### Fixing a Bug

1. Create bug fix branch
2. Write failing test that reproduces bug
3. Implement fix
4. Verify test passes
5. Update changelog
6. Create pull request

### Releasing a Package

```bash
# In package directory
npm version patch  # or minor, major
npm publish

# Update package version in dependent packages
# Update submodule reference in framework
```

## Troubleshooting

### Submodule Issues

**Problem**: Submodule is "dirty" or shows modified files

```bash
# Option 1: Discard changes in submodule
cd packages/problem-package
git reset --hard HEAD
git clean -fd

# Option 2: Commit changes
git add .
git commit -m "Update package"
git push
```

**Problem**: Submodule is detached HEAD

```bash
cd packages/problem-package
git checkout main
git pull
cd ../..
git add packages/problem-package
git commit -m "Update submodule pointer to latest main"
```

### Build Issues

**Problem**: TypeScript compilation errors

```bash
# Clean build
npm run clean
npm install
npm run build
```

**Problem**: Circular dependencies

- Review import structure
- Use interfaces to break cycles
- Refactor into separate packages if needed

### Test Failures

```bash
# Run tests in watch mode to debug
npm run test:watch

# Run specific test file
npm test -- tests/unit/MemoryCache.test.ts

# Debug with VSCode
# Set breakpoint, use "Debug Test" in VSCode
```

## Best Practices

### Performance

- Use caching aggressively
- Implement DataLoader for GraphQL N+1 issues
- Profile before optimizing
- Monitor metrics in production

### Security

- Never commit secrets or tokens
- Use environment variables for configuration
- Validate all inputs
- Sanitize error messages in production

### Error Handling

```typescript
// ✅ Good: Structured errors with context
throw new ApplicationError('User not found', {
  code: 'USER_NOT_FOUND',
  userId,
  statusCode: 404
});

// ❌ Bad: Generic errors
throw new Error('Not found');
```

## Resources

- **Architecture**: See `/docs/architecture-reference.md`
- **ADRs**: See `/docs/adr/` for architectural decisions
- **CLAUDE.md**: Check each package's CLAUDE.md for specific guidance
- **GitHub**: [metaGOTHIC Framework](https://github.com/ChaseNoCapDev/meta-gothic-framework)

---

*Part of the metaGOTHIC Framework - AI-Guided Opinionated TypeScript Framework*
