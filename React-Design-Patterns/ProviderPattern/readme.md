# Provider Pattern (Context API)

## Overview

The Provider Pattern uses **React Context** to share data across the component tree
without prop drilling.

It is commonly used for **app-level or cross-cutting concerns** where many components
need access to the same data.

---

## Core Idea

- A Provider owns shared state
- Consumers read data via context
- Data flows implicitly through the tree
- Eliminates prop drilling for global concerns

---

## Common Use Cases

- Theme management (dark / light mode)
- Feature flags
- Internationalization (i18n)
- User/session data
- App-wide configuration

---

## Minimal Example

```tsx
const ThemeContext = createContext(null);

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  return <ThemeContext.Provider value={{ theme, setTheme }}>{children}</ThemeContext.Provider>;
}
```

```tsx
function Button() {
  const { theme } = useContext(ThemeContext);
  return <button data-theme={theme} />;
}
```

## Core Rules & Responsibilities

- Keep context scope as small as possible
- One context = one responsibility
- Do not treat context as a global store
- Pass only relevant data, not everything
- Prefer multiple small contexts over one large context
- Avoid frequent updates in context whenever possible

---

## Common Pitfalls

- Overloading context with unrelated data
- Passing entire objects when only part is needed
- Using context for frequently changing values
- Mixing unrelated concerns in a single provider

---

## Performance Notes

- Any context value change re-renders all consumers
- Frequent updates can cause performance issues
- Split contexts to reduce unnecessary re-renders
- Memoize provider values when needed

```ts
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  const value = useMemo(() => ({ theme, setTheme }), [theme]);

  return <ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>;
}
```

## When NOT to Use

- Local component state
- High-frequency updates (e.g., animations, input typing)
- Simple parent–child data passing

---

## Rule of Thumb

- Use context for global configuration, not dynamic data
- Keep providers narrow and focused
- If updates are frequent, reconsider the pattern

## Example

## Build an Auth Context & Provider

### Requirements

- Hold `user` and `isAuthenticated`
- Expose `login()` and `logout()`
- Consume in `Navbar` and `Dashboard`

### Auth Context

```tsx
const AuthContext = createContext(null);

function AuthProvider({ children }) {
  const [user, setUser] = useState(null);

  const login = (userData) => setUser(userData);
  const logout = () => setUser(null);

  const value = {
    user,
    isAuthenticated: !!user,
    login,
    logout,
  };

  return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
}
```

## Consuming Auth Context

```tsx
function Navbar() {
  const { isAuthenticated, logout } = useContext(AuthContext);

  return isAuthenticated ? <button onClick={logout}>Logout</button> : null;
}
```

```tsx
function Dashboard() {
  const { user } = useContext(AuthContext);
  return <div>Welcome {user?.name}</div>;
}
```

## Build a Language Context

## Requirements

- Store selected language
- Toggle language
- Update text dynamically

## Language Context

```tsx
const LanguageContext = createContext(null);

function LanguageProvider({ children }) {
  const [lang, setLang] = useState('en');

  const toggleLanguage = () => setLang((prev) => (prev === 'en' ? 'fr' : 'en'));

  const value = { lang, toggleLanguage };

  return <LanguageContext.Provider value={value}>{children}</LanguageContext.Provider>;
}
```

## Consuming Language Context

```tsx
function Greeting() {
  const { lang } = useContext(LanguageContext);

  return <p>{lang === 'en' ? 'Hello' : 'Bonjour'}</p>;
}
```

## Combine Them (Clean Composition)

## App-Level Composition

```tsx
function AppProviders({ children }) {
  return (
    <AuthProvider>
      <LanguageProvider>{children}</LanguageProvider>
    </AuthProvider>
  );
}
```

```tsx
function App() {
  return (
    <AppProviders>
      <Navbar />
      <Greeting />
      <Dashboard />
    </AppProviders>
  );
}
```

## Key Takeaways

- Each context has a single responsibility
- Providers are small and focused
- Context is used for cross-cutting concerns
- Composition keeps providers readable and scalable

## Rule of Thumb

> Use multiple small providers instead of one large global context.

<!-- https://github.com/tapascript/15-days-of-react-design-patterns/tree/main/day-07/provider-pattern/src -->
