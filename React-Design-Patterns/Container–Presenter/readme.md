# Container–Presenter Design Pattern

## Overview

The Container–Presenter pattern separates **data and business logic** from **UI rendering**.

It improves readability, testability, and scalability for data-heavy React components.

---

## Core Idea

Split a component into two parts:

### Container (Smart Component)

- Handles data fetching
- Manages state and side effects
- Prepares data for presentation

### Presenter (Dumb Component)

- Receives data via props
- Renders JSX only
- Contains no business logic

---

## When to Use

- Data-heavy components
- Pages that fetch from APIs
- Components with multiple UI states
- Complex data transformations before rendering

---

## When NOT to Use

- Small or purely presentational components
- Simple UI with minimal logic
- Deeply nested components that cause prop drilling

---

## Core Rules & Responsibilities

- Container owns all state, side effects, and business logic
- Container handles API calls, data preparation, and actions
- Presenter renders JSX only and remains declarative
- No API calls or side effects inside presenters
- No JSX decision-making inside the container
- Child presenters are small, reusable, and isolated
- Clear separation of concerns ensures testability and scalability

---

## Example: Product List

This example demonstrates the Container–Presenter pattern using a product listing feature.

---

## Responsibilities

### ProductListContainer

- Fetch products from API
- Manage cart state
- Handle errors
- Pass prepared data and handlers to presenter

### ProductListPresenter

- Render product list
- Handle sort/filter UI events
- Trigger add-to-cart actions via callbacks

### Smaller Presenters

- ProductCard
- SortFilterControls
- CartSummary

---

## ProductListContainer (Logic Only)

```tsx
function ProductListContainer() {
  const [products, setProducts] = useState([]);
  const [cart, setCart] = useState([]);
  const [sortBy, setSortBy] = useState('name');

  useEffect(() => {
    fetchProducts().then(setProducts);
  }, []);

  const addToCart = (product) => {
    setCart((prev) => [...prev, product]);
  };

  const sortedProducts = sort(products, sortBy);

  return (
    <ProductListPresenter products={sortedProducts} cart={cart} onAddToCart={addToCart} onSortChange={setSortBy} />
  );
}
```

## ProductListPresenter (JSX Only)

```tsx
function ProductListPresenter({ products, cart, onAddToCart, onSortChange }) {
  return (
    <>
      <SortFilterControls onSortChange={onSortChange} />
      <CartSummary cart={cart} />

      {products.map((p) => (
        <ProductCard key={p.id} product={p} onAddToCart={onAddToCart} />
      ))}
    </>
  );
}
```

<!-- https://github.com/tapascript/15-days-of-react-design-patterns/tree/main/day-01/container-presenter-pattern/src -->
