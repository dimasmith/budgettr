# Project Context

## Purpose
Budgettr is a simple desktop application designed to help users track their personal finances. The application provides an intuitive interface for managing budgets, expenses, and financial data.

## Tech Stack

### Frontend
- **React 19.1.0** - UI framework with React.StrictMode enabled
- **TypeScript 5.8.3** - Type-safe JavaScript with strict mode enabled
- **Vite 7.0.4** - Build tool and dev server (runs on port 1420)
- **Material-UI (MUI) 7.3.5** - React component library for UI
- **Emotion** - CSS-in-JS styling solution (used by MUI)

### Backend/Desktop
- **Tauri 2** - Desktop application framework (Rust-based)
- **Rust 2021 Edition** - Backend logic and native capabilities
- **Tauri Plugins**: opener (for external links/files)

### Development Tools
- **pnpm** - Package manager
- **dotenvy** - Environment variable management in Rust

## Project Conventions

### Code Style
- **TypeScript Configuration**:
  - Target: ES2020
  - Strict mode enabled (`strict: true`)
  - `noUnusedLocals` and `noUnusedParameters` enabled
  - `noFallthroughCasesInSwitch` enabled
  - JSX: react-jsx
  - Module resolution: bundler
  
- **File Naming**:
  - React components: PascalCase (e.g., `App.tsx`)
  - TypeScript files: camelCase with `.tsx` for components, `.ts` for utilities
  - Rust files: snake_case (e.g., `main.rs`, `lib.rs`)

- **Code Organization**:
  - Frontend source: `src/`
  - Backend source: `src-tauri/src/`
  - Build outputs: `src-tauri/target/`
  - OpenSpec documentation: `openspec/`

### Architecture Patterns
- **Desktop Application Architecture**:
  - Tauri provides the native desktop shell and Rust backend
  - React frontend communicates with Rust backend via Tauri commands
  - Vite handles frontend bundling and HMR during development
  
- **Component Structure**:
  - React functional components with hooks
  - Material-UI components for consistent UI/UX
  - Emotion for styling (via MUI integration)

- **Backend Commands**:
  - Tauri commands decorated with `#[tauri::command]`
  - Registered in the command handler in `lib.rs`
  - Example: `greet` command demonstrates Rust ↔ Frontend communication

### Testing Strategy
- **Frontend Testing**: Vitest with React Testing Library (see ADR 001)
- TypeScript strict mode provides compile-time type safety
- Rust's type system provides additional safety in the backend

### Git Workflow
- Version format: `0.1.0-dev` (following semantic versioning)
- Private repository (`private: true` in package.json)

## Domain Context
- **Personal Finance Domain**: The application deals with budgets, expenses, income tracking, and financial calculations
- **Desktop-First**: Built as a native desktop application rather than a web app
- **Privacy-Focused**: Running locally on user's machine with potential for local data storage

## Important Constraints
- **Port Requirements**: Vite dev server must run on port 1420 (strict), HMR on 1421
- **Rust Compilation**: Changes to Rust code require full rebuild of Tauri app
- **Platform Targeting**: Desktop platforms (Windows, macOS, Linux)
- **React 19**: Using latest React version - ensure compatibility with libraries
- **TypeScript Strict Mode**: All code must pass strict TypeScript checks

## External Dependencies
- **Tauri Core**: Desktop application framework and IPC layer
- **Material-UI**: UI component library with theming system
- **Vite**: Development server and build system
- **Rust Toolchain**: Required for building Tauri backend
- **Node.js/pnpm**: Required for frontend development
