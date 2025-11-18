---
name: frontend-development
description: Master React, Vue, Angular, TypeScript, HTML5, CSS3, performance optimization, accessibility, and testing. Build scalable component systems with modern frameworks and best practices.
---

# Frontend Development Skills

## Advanced React Patterns

### Custom Hooks for State Logic Reuse
```typescript
// useFetch with proper error handling and caching
function useFetch<T>(url: string, options?: RequestInit) {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);
  const cacheRef = useRef<Map<string, T>>(new Map());

  useEffect(() => {
    if (cacheRef.current.has(url)) {
      setData(cacheRef.current.get(url)!);
      setLoading(false);
      return;
    }

    let isMounted = true;

    (async () => {
      try {
        const response = await fetch(url, { ...options });
        if (!response.ok) throw new Error(`HTTP ${response.status}`);
        const result = await response.json();
        
        if (isMounted) {
          cacheRef.current.set(url, result);
          setData(result);
          setError(null);
        }
      } catch (err) {
        if (isMounted) {
          setError(err instanceof Error ? err : new Error(String(err)));
        }
      } finally {
        if (isMounted) setLoading(false);
      }
    })();

    return () => { isMounted = false; };
  }, [url, options]);

  return { data, loading, error };
}
```

### Higher-Order Components (HOCs)
```typescript
function withAuth<P extends object>(
  Component: React.ComponentType<P>
): React.FC<Omit<P, 'user'>> {
  return (props) => {
    const user = useAuth();
    if (!user) return <Navigate to="/login" />;
    return <Component {...(props as P)} user={user} />;
  };
}
```

### Compound Components
```typescript
const Form = ({ children }) => {
  const [errors, setErrors] = useState({});
  return (
    <FormContext.Provider value={{ errors, setErrors }}>
      {children}
    </FormContext.Provider>
  );
};

Form.Input = ({ name, ...props }) => {
  const { errors } = useContext(FormContext);
  return (
    <>
      <input name={name} {...props} />
      {errors[name] && <span className="error">{errors[name]}</span>}
    </>
  );
};
```

## Performance Optimization

### Code Splitting & Lazy Loading
```typescript
const DashboardModule = lazy(() =>
  import('./pages/Dashboard').then(m => ({ default: m.Dashboard }))
);

<Suspense fallback={<LoadingSpinner />}>
  <DashboardModule />
</Suspense>
```

### Memoization Strategies
```typescript
// Prevent unnecessary re-renders
const ExpensiveComponent = memo(
  ({ data, onFilter }) => {
    return <ComplexRendering data={data} />;
  },
  (prev, next) => prev.data === next.data
);

// Memoize callbacks
const handleFilter = useCallback((filter) => {
  setFilters(f => ({ ...f, ...filter }));
}, []);

// Memoize computed values
const filtered = useMemo(() =>
  items.filter(item => item.active),
  [items]
);
```

## CSS Architecture

### CSS Modules
```css
/* Button.module.css */
.button {
  padding: var(--spacing-md);
  border-radius: var(--radius-sm);
  transition: all 200ms ease;
}

.button:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.primary {
  background: var(--color-primary);
  color: white;
}
```

### CSS-in-JS Best Practices
```typescript
const buttonStyles = css`
  padding: ${spacing.md};
  background: ${colors.primary};
  
  &:hover {
    background: ${colors.primaryDark};
  }
  
  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
`;
```

## Testing Strategies

### React Testing Library (Behavioral Testing)
```typescript
import { render, screen, userEvent } from '@testing-library/react';

test('adds item to list', async () => {
  render(<TodoApp />);
  const input = screen.getByRole('textbox');
  const button = screen.getByRole('button', { name: /add/i });
  
  await userEvent.type(input, 'New task');
  await userEvent.click(button);
  
  expect(screen.getByText('New task')).toBeInTheDocument();
});
```

### E2E Testing with Cypress
```typescript
describe('Login Flow', () => {
  it('logs in successfully', () => {
    cy.visit('/login');
    cy.get('[data-testid="email"]').type('user@example.com');
    cy.get('[data-testid="password"]').type('password');
    cy.get('button[type="submit"]').click();
    cy.url().should('include', '/dashboard');
  });
});
```

## Accessibility (a11y)

### Semantic HTML & ARIA
```typescript
<nav aria-label="Main navigation">
  <ul>
    <li><Link aria-current="page" to="/">Home</Link></li>
    <li><Link to="/about">About</Link></li>
  </ul>
</nav>

<button aria-label="Toggle dark mode">🌙</button>
<div aria-live="polite" aria-atomic="true">
  {statusMessage}
</div>
```

### Color Contrast & Keyboard Navigation
```typescript
// Ensure 4.5:1 contrast for normal text, 3:1 for large text
// Test with Lighthouse, axe DevTools

const Focusable = styled.button`
  &:focus {
    outline: 2px solid ${colors.primary};
    outline-offset: 2px;
  }
`;
```

## Browser APIs

### Intersection Observer (Lazy Loading)
```typescript
const useIntersection = (ref: RefObject<HTMLElement>) => {
  const [isVisible, setIsVisible] = useState(false);

  useEffect(() => {
    const observer = new IntersectionObserver(([entry]) => {
      setIsVisible(entry.isIntersecting);
    });

    if (ref.current) observer.observe(ref.current);
    return () => observer.disconnect();
  }, [ref]);

  return isVisible;
};
```

### Service Workers & Offline Support
```typescript
// Register service worker for offline capability
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js').then(reg => {
    console.log('SW registered');
  });
}
```

## TypeScript Mastery

### Advanced Types
```typescript
// Discriminated unions for type-safe state
type Action =
  | { type: 'LOAD'; data: User[] }
  | { type: 'ERROR'; error: string }
  | { type: 'RESET' };

// Generic constraints
function merge<T extends { id: string }>(a: T, b: T): T {
  return { ...a, ...b };
}

// Utility types
type ReadonlyUser = Readonly<User>;
type PartialUser = Partial<User>;
type UserKeys = keyof User;
```

## Web Performance

### Metrics & Monitoring
```typescript
// Monitor Web Vitals
import { getCLS, getFID, getFCP, getLCP, getTTFB } from 'web-vitals';

getCLS(console.log);
getFID(console.log);
getFCP(console.log);
getLCP(console.log);
getTTFB(console.log);

// Custom performance marks
performance.mark('expensive-operation-start');
// ... expensive work ...
performance.mark('expensive-operation-end');
performance.measure('expensive-operation', 'start', 'end');
```

## Best Practices

✅ Keep components small and focused
✅ Use composition over inheritance
✅ Optimize images (WebP, srcset)
✅ Implement code splitting
✅ Test behavior, not implementation
✅ Use TypeScript strict mode
✅ Monitor Core Web Vitals
✅ Implement error boundaries
✅ Use React DevTools Profiler
✅ Keep bundle size in check

