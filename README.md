# React Developer Roadmap Plugin

**Complete React ecosystem learning path based on roadmap.sh/react**

Master modern React development from fundamentals to advanced patterns with expert guidance, practical examples, and production-ready skills.

## 🎯 Overview

This plugin provides a comprehensive React learning journey with:

- **7 Specialized Agents** - Expert guidance for each React skill level
- **7 Skill Modules** - Hands-on React patterns with code examples
- **4 Learning Commands** - Navigate your React learning path
- **Production-Ready Focus** - Real-world patterns and best practices

## 🚀 Quick Start

### Installation

```bash
# In Claude Code
/plugin add https://github.com/pluginagentmarketplace/custom-plugin-react
```

Or load locally:
```bash
# From plugin directory
/plugin load ./react-developer-roadmap
```

### First Steps

1. **Start Learning React**
   ```
   /learn-react
   ```
   Get personalized React learning path based on your level

2. **Explore React Roadmap**
   ```
   /explore-react-roadmap
   ```
   Navigate the complete React ecosystem

3. **Assess Your React Skills**
   ```
   /assess-react-skills
   ```
   Evaluate your React knowledge and get recommendations

4. **Find React Resources**
   ```
   /find-react-resources
   ```
   Discover official docs, tutorials, and community resources

## 🏗️ Plugin Architecture

### Agents (7 React Experts)

| Agent | Focus | Best For |
|-------|-------|----------|
| **React Fundamentals** | JSX, Components, Props, State | Beginners starting with React |
| **Hooks & Patterns** | useState, useEffect, Custom Hooks | Understanding modern React |
| **Component Architecture** | Composition, HOCs, Render Props | Building scalable component systems |
| **State Management** | Context, Redux, Zustand | Managing application state |
| **Routing & Navigation** | React Router v6, Navigation patterns | Building multi-page apps |
| **Performance Optimization** | Memoization, Code Splitting, Profiling | Optimizing React apps |
| **Testing & Deployment** | React Testing Library, Jest, CI/CD | Production deployment |

### Skills (7 Practical Modules)

Each skill includes:
- **Code examples and patterns**
- **Best practices**
- **Common mistakes to avoid**
- **Practice exercises**
- **Official documentation links**

**Available Skills:**
- `react-hooks-patterns` - Master all React Hooks
- `component-library` - Build reusable component libraries
- `redux-state-management` - Redux Toolkit & RTK Query
- `react-router` - Client-side routing with React Router v6
- `react-testing-library` - Write maintainable React tests
- `next-js-framework` - Server-side rendering with Next.js
- `react-performance` - Performance optimization techniques

### Commands (4 Learning Tools)

```
/learn-react              → Get personalized React learning path
/explore-react-roadmap    → Browse complete React ecosystem
/assess-react-skills      → Evaluate your React knowledge
/find-react-resources     → Discover React learning resources
```

## 📚 Learning Path

### Complete React Journey (4-8 months)

#### Beginner Track (Weeks 1-7)
- **Weeks 1-3**: React Fundamentals
  - JSX and components
  - Props and state
  - Event handling
  - Lists and conditional rendering

- **Weeks 4-7**: Hooks & Patterns
  - useState and useEffect
  - useContext for state sharing
  - Custom hooks
  - Performance hooks (useMemo, useCallback)

#### Intermediate Track (Weeks 8-16)
- **Weeks 8-11**: Component Architecture
  - Composition patterns
  - Higher-Order Components
  - Render props
  - Compound components

- **Weeks 12-16**: State Management
  - Context API deep dive
  - Redux Toolkit
  - RTK Query for API calls
  - Zustand and modern alternatives

#### Advanced Track (Weeks 17-23)
- **Weeks 17-19**: Routing & Navigation
  - React Router v6
  - Nested routes
  - Protected routes
  - Code splitting

- **Weeks 20-23**: Performance Optimization
  - React DevTools Profiler
  - Memoization strategies
  - List virtualization
  - Bundle optimization

#### Production Track (Weeks 24-28)
- **Weeks 24-28**: Testing & Deployment
  - React Testing Library
  - Jest configuration
  - E2E testing with Cypress
  - CI/CD pipelines
  - Production deployment

## 🛠️ React Technology Stack

### Core React
- **React 18+** - Latest features including Concurrent Mode
- **JSX** - JavaScript XML syntax
- **Components** - Functional components with Hooks
- **Props** - Component communication
- **State** - Local and global state management

### State Management
- **Context API** - Built-in state sharing
- **Redux Toolkit** - Predictable state container
- **RTK Query** - Powerful data fetching
- **Zustand** - Lightweight state management
- **Jotai** - Atomic state management

### Routing
- **React Router v6** - Declarative routing
- **TanStack Router** - Type-safe routing
- **Next.js Routing** - File-based routing

### Testing
- **React Testing Library** - User-centric testing
- **Jest** - JavaScript testing framework
- **Cypress** - E2E testing
- **Playwright** - Cross-browser testing

### Build Tools
- **Vite** - Next generation build tool
- **Create React App** - Official React starter
- **Next.js** - Production framework
- **Webpack** - Module bundler

### UI & Styling
- **Tailwind CSS** - Utility-first CSS
- **styled-components** - CSS-in-JS
- **Emotion** - CSS-in-JS library
- **Chakra UI** - Component library
- **Material-UI** - React components

## 🎓 How to Use

### For React Beginners
1. Start with `/learn-react`
2. Begin with React Fundamentals agent
3. Follow the beginner track (Weeks 1-7)
4. Build practice projects
5. Progress to Hooks & Patterns

### For JavaScript Developers
1. Run `/assess-react-skills` to evaluate current level
2. Start with Hooks & Patterns if comfortable with JS
3. Focus on React-specific patterns
4. Build real-world projects
5. Learn state management

### For Experienced React Developers
1. Choose specific advanced agents
2. Dive into Performance Optimization
3. Master Testing & Deployment
4. Learn Next.js for SSR
5. Build production-ready applications

### For Full-Stack Developers
1. Explore React + Next.js combination
2. Integrate with backend APIs
3. Master state management
4. Implement authentication flows
5. Deploy to production

## 📖 Documentation Structure

```
react-developer-roadmap/
├── .claude-plugin/
│   └── plugin.json                         # Plugin manifest
├── agents/                                 # 7 React agents
│   ├── 01-react-fundamentals.md
│   ├── 02-hooks-patterns.md
│   ├── 03-component-architecture.md
│   ├── 04-state-management.md
│   ├── 05-routing-navigation.md
│   ├── 06-performance-optimization.md
│   └── 07-testing-deployment.md
├── skills/                                 # 7 Skill modules
│   ├── react-hooks-patterns/SKILL.md
│   ├── component-library/SKILL.md
│   ├── redux-state-management/SKILL.md
│   ├── react-router/SKILL.md
│   ├── react-testing-library/SKILL.md
│   ├── next-js-framework/SKILL.md
│   └── react-performance/SKILL.md
├── commands/                               # 4 Slash commands
│   ├── learn-path.md
│   ├── explore-roadmap.md
│   ├── assess-skills.md
│   └── find-resources.md
├── hooks/hooks.json                        # Automation config
├── README.md                               # This file
└── LICENSE
```

## 🎯 Key Features

✅ **Modern React Focus** - React 18+ with Hooks and latest patterns

✅ **Production-Ready** - Real-world patterns used in production

✅ **Comprehensive Coverage** - From basics to advanced optimization

✅ **Hands-On Learning** - Code examples for every concept

✅ **Best Practices** - Industry-standard patterns and conventions

✅ **Testing Focused** - Learn to write maintainable tests

✅ **Performance First** - Optimization from the start

✅ **Official Roadmap** - Based on roadmap.sh/react

## 🌟 React Ecosystem Coverage

### Core React Concepts
- JSX and Components
- Props and State
- Event Handling
- Conditional Rendering
- Lists and Keys
- Forms and Controlled Components

### React Hooks
- useState for state management
- useEffect for side effects
- useContext for context consumption
- useReducer for complex state
- useCallback for function memoization
- useMemo for value memoization
- useRef for DOM access and mutable values
- Custom Hooks for reusable logic

### State Management Solutions
- Context API and useContext
- Redux Toolkit (modern Redux)
- RTK Query (API state)
- Zustand (lightweight)
- Jotai (atomic state)
- Recoil (Facebook's solution)

### Routing & Navigation
- React Router v6
- Nested Routes
- Protected Routes
- Dynamic Routing
- Code Splitting Routes
- Navigation Patterns

### Testing Strategies
- React Testing Library
- Jest Configuration
- Component Testing
- Hooks Testing
- Integration Testing
- E2E Testing with Cypress

### Performance Optimization
- React.memo
- useMemo and useCallback
- Code Splitting
- Lazy Loading
- List Virtualization
- Bundle Optimization
- React DevTools Profiler

### Production Deployment
- Build Optimization
- Environment Variables
- CI/CD Pipelines
- Vercel/Netlify Deployment
- Docker Containerization
- Monitoring and Logging

## 📝 Learning Philosophy

This plugin follows React best practices:

1. **Hooks-First Approach** - Modern functional components
2. **Composition over Inheritance** - Build with composition
3. **Accessibility** - WCAG-compliant components
4. **Performance** - Optimize from the start
5. **Testing** - Write tests that resemble user behavior
6. **TypeScript Ready** - Prepare for type safety

## 🔧 Practice Projects

### Beginner Projects
- Todo List with local state
- Counter with useState
- Simple Form Validation
- Product Card Grid

### Intermediate Projects
- Shopping Cart with Context API
- Blog with React Router
- Weather App with API integration
- Authentication Flow

### Advanced Projects
- E-commerce Dashboard with Redux
- Social Media Feed with Infinite Scroll
- Real-time Chat with WebSockets
- Full-Stack App with Next.js

## 📞 Resources & Community

### Official Documentation
- [React Official Docs](https://react.dev) - New React docs
- [React Router](https://reactrouter.com) - Routing documentation
- [Redux Toolkit](https://redux-toolkit.js.org) - State management
- [Next.js](https://nextjs.org) - Production framework

### Learning Resources
- [React Tutorial](https://react.dev/learn) - Official tutorial
- [React Patterns](https://reactpatterns.com) - Common patterns
- [Kent C. Dodds Blog](https://kentcdodds.com/blog) - React insights

### Community
- [Reactiflux Discord](https://www.reactiflux.com) - React community
- [React Reddit](https://reddit.com/r/reactjs) - Discussions
- [Stack Overflow](https://stackoverflow.com/questions/tagged/reactjs) - Q&A

## 🤝 Contributing

This plugin is based on [roadmap.sh/react](https://roadmap.sh/react). You can:

- Report issues or suggestions
- Share React resources
- Contribute examples
- Help other learners

## 📄 License

MIT License

## 🙏 Acknowledgments

Built on the excellent [roadmap.sh](https://roadmap.sh) project by [@kamranahmedse](https://github.com/kamranahmedse)

---

**Ready to master React?**

```
/learn-react
```

Happy React learning! ⚛️

## 🎯 Specialization Tracks

### Single Page Applications (SPA)
- Client-side routing
- State management
- API integration
- Performance optimization

### Server-Side Rendering (SSR)
- Next.js framework
- Data fetching strategies
- SEO optimization
- Performance benefits

### Static Site Generation (SSG)
- Static site generators
- Build-time rendering
- Content management
- Deployment strategies

### Progressive Web Apps (PWA)
- Service workers
- Offline functionality
- Push notifications
- App-like experience

---

**Difficulty**: Beginner to Advanced
**Estimated Learning Time**: 4-8 months
**Source**: [roadmap.sh/react](https://roadmap.sh/react)
**Last Updated**: 2025-11-20
