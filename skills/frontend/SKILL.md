---
name: frontend-development
description: Master React, Vue, Angular, TypeScript, HTML5, CSS3, and modern frontend development practices. Use when building user interfaces, styling applications, managing component state, or working with frontend frameworks.
---

# Frontend Development

## Quick Start

### Setting up a React project with TypeScript:

```bash
npx create-react-app my-app --template typescript
cd my-app
npm start
```

### Basic React component with hooks:

```typescript
import React, { useState, useEffect } from 'react';

interface Props {
  title: string;
}

const MyComponent: React.FC<Props> = ({ title }) => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log('Component mounted');
    return () => console.log('Component unmounted');
  }, []);

  return (
    <div>
      <h1>{title}</h1>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
};

export default MyComponent;
```

### CSS Module example:

```css
/* styles.module.css */
.container {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  padding: 2rem;
}

.button {
  padding: 0.5rem 1rem;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.button:hover {
  background-color: #0056b3;
}
```

## Core Concepts

**Component Architecture**
- Functional components with hooks (preferred)
- Props for passing data down
- State management with useState, useReducer
- Context API for shared state
- Custom hooks for reusable logic

**State Management**
- Local state with useState
- Global state with Context API
- Redux for complex applications
- Zustand or Jotai for lightweight alternatives

**Performance Optimization**
- Code splitting with React.lazy and Suspense
- Memoization with React.memo, useMemo, useCallback
- Virtual lists for large datasets
- Image optimization and lazy loading

## Best Practices

1. **Component Design**
   - Keep components small and focused
   - Use composition over inheritance
   - Lift state up when needed
   - Avoid prop drilling with Context API

2. **Performance**
   - Profile before optimizing
   - Use React DevTools Profiler
   - Implement code splitting
   - Optimize bundle size

3. **Testing**
   - Unit tests with Jest
   - Component testing with React Testing Library
   - E2E testing with Cypress or Playwright
   - Aim for 80%+ coverage

4. **Accessibility**
   - Semantic HTML
   - ARIA labels and roles
   - Keyboard navigation
   - Color contrast compliance

## Common Patterns

**Custom Hook for API calls:**
```typescript
function useFetch<T>(url: string) {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    fetch(url)
      .then(r => r.json())
      .then(data => {
        setData(data);
        setLoading(false);
      })
      .catch(err => {
        setError(err);
        setLoading(false);
      });
  }, [url]);

  return { data, loading, error };
}
```

## Resources

- **React**: react.dev (official docs)
- **TypeScript**: typescriptlang.org
- **Testing**: testing-library.com
- **Styling**: tailwindcss.com, styled-components
- **Performance**: web.dev/performance
