# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Development
npm run start:dev       # Run with watch mode
npm run start:debug     # Run with debugger attached

# Build
npm run build           # Compile TypeScript to dist/

# Testing
npm run test            # Unit tests (*.spec.ts)
npm run test:watch      # Unit tests in watch mode
npm run test:cov        # Unit tests with coverage
npm run test:e2e        # End-to-end tests

# Code quality
npm run lint            # ESLint with auto-fix
npm run format          # Prettier formatting
```

To run a single test file:
```bash
npx jest src/app.controller.spec.ts
```

## Architecture

NestJS 11 REST API for product CRUD. Data is stored **in-memory** (no database) — state resets on server restart.

**Request flow**: `ProductsController` (route handling) → `ProductsService` (business logic + in-memory store) → response

**Key patterns**:
- Global `ValidationPipe` (whitelist + transform) applied in `main.ts` — DTOs use `class-validator` decorators for input validation
- `UpdateProductDto` is a `PartialType` of `CreateProductDto` (all fields optional)
- `ProductsService` throws `NotFoundException` for missing products; NestJS converts this to a 404 response automatically

**Modules**: `AppModule` imports `ProductsModule`. Adding new features means creating a new module and importing it into `AppModule`.

## CI/CD

Two GitHub Actions workflows in `.github/workflows/`:
- `claude.yml` — Claude responds to `@claude` mentions in issues/PRs
- `claude-code-review.yml` — Automatic code review on every PR via Claude
