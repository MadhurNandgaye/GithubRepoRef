
Certainly! Here's the code with the changes highlighted for clarity:

### Before Changes:

```javascript
// State Management
const [showCategoryA, setShowCategoryA] = useState(false); // Toggle for Category A visibility

// ...

// JSX Rendering
<button onClick={toggleCategoryAVisibility}>
  Toggle Category A Visibility
</button>

{showCategoryA && (
  <div>
    <h2>Category A Items:</h2>
    <ul>{renderListItems(filteredItems.filter(item => item.category === 'A'))}</ul>
  </div>
)}
```

### After Changes:

```javascript
// State Management
const [categoryVisibility, setCategoryVisibility] = useState({
  A: false,
  B: false,
  C: false
});

// ...

// Updated Toggle Function
const toggleCategoryVisibility = useCallback((category) => {
  setCategoryVisibility(prev => ({
    ...prev,
    [category]: !prev[category]
  }));
}, []);

// ...

// JSX Rendering
{Object.keys(categoryVisibility).map(category => (
  <div key={category}>
    <h2>{`Category ${category} Items:`}</h2>
    <button onClick={() => toggleCategoryVisibility(category)}>
      Toggle Category {category} Visibility
    </button>
    {categoryVisibility[category] ? (
      <ul>{renderListItems(filteredItems.filter(item => item.category === category))}</ul>
    ) : null}
  </div>
))}
```

The changes made involve transitioning from a singular toggle for Category A to a dynamic approach that handles toggling for all categories (`A`, `B`, and `C`) based on their respective visibility in the `categoryVisibility` state.





Certainly! Here's the refactored code using the ternary operator for dynamic category visibility:

```javascript
import React, { useState, useEffect, useCallback, useMemo } from 'react';

const ListComponent = () => {
  
  // State Management with useState
  const [categoryVisibility, setCategoryVisibility] = useState({
    A: false,
    B: false,
    C: false
  });

  const [asyncData, setAsyncData] = useState([]);
  const [searchTerm, setSearchTerm] = useState('');
  const [selectedCategory, setSelectedCategory] = useState('All');
  const [sortType, setSortType] = useState('name');
  const [showDeepCopied, setShowDeepCopied] = useState(false);

  // Side Effect with useEffect for data fetching
  useEffect(() => {
    const fetchData = async () => {
      setTimeout(() => {
        setAsyncData([
          { id: 5, name: "Async of Item 1", category: "A" },
          { id: 6, name: "Async of Item 2", category: "B" },
          { id: 7, name: "Async of Item 3", category: "A" },
          { id: 8, name: "Async of Item 4", category: "C" },
          { id: 9, name: "Async of Item 5", category: "A" },
        ]);
      }, 2000);
    };

    fetchData();
  }, []);

  const deepCopiedData = useMemo(() => [...asyncData], [asyncData]);

  const filteredItems = useMemo(() => {
    let filtered = asyncData.filter((item) => {
      return (item.name.toLowerCase().includes(searchTerm.toLowerCase()) || searchTerm === '') &&
             (selectedCategory === 'All' || item.category === selectedCategory);
    });

    if (sortType === 'name') {
      filtered.sort((a, b) => a.name.localeCompare(b.name));
    } else if (sortType === 'category') {
      filtered.sort((a, b) => a.category.localeCompare(b.category));
    }

    return filtered;
  }, [asyncData, searchTerm, sortType, selectedCategory]);

  const renderListItems = useCallback((itemList) => {
    return itemList.map((item) => (
      <li key={item.id}>{item.name} - {item.category}</li>
    ));
  }, []);

  const toggleCategoryVisibility = useCallback((category) => {
    setCategoryVisibility(prev => ({
      ...prev,
      [category]: !prev[category]
    }));
  }, []);

  return (
    <div>
      <h2>List of Items:</h2>

      <input 
        type="text" 
        placeholder="Search by name..." 
        value={searchTerm} 
        onChange={(e) => setSearchTerm(e.target.value)} 
      />

      <select value={selectedCategory} onChange={(e) => setSelectedCategory(e.target.value)}>
        <option value="All">All Categories</option>
        <option value="A">Category A</option>
        <option value="B">Category B</option>
        <option value="C">Category C</option>
      </select>

      <select value={sortType} onChange={(e) => setSortType(e.target.value)}>
        <option value="name">Sort by Name</option>
        <option value="category">Sort by Category</option>
      </select>

      <ul>{renderListItems(filteredItems)}</ul>

      <button onClick={() => setShowDeepCopied(prev => !prev)}>
        Toggle Deep Copied Data
      </button>

      {showDeepCopied && (
        <div>
          <h2>Deep Copied Data:</h2>
          <pre>{JSON.stringify(deepCopiedData, null, 2)}</pre>
        </div>
      )}

      <h2>Find Example:</h2>
      <p>Item with id 6: {filteredItems.find((item) => item.id === 6)?.name ?? "Not found"}</p>

      <h2>Filter Example:</h2>
      {Object.keys(categoryVisibility).map(category => (
        <div key={category}>
          <h2>{`Category ${category} Items:`}</h2>
          <button onClick={() => toggleCategoryVisibility(category)}>
            Toggle Category {category} Visibility
          </button>
          {categoryVisibility[category] ? (
            <ul>{renderListItems(filteredItems.filter(item => item.category === category))}</ul>
          ) : null}
        </div>
      ))}

      <hr />
    </div>
  );
};

export default ListComponent;
```

This refactored code uses the ternary operator for conditional rendering, making it more dynamic and concise.



Project Code
```jsx
import React, { useState, useEffect, useCallback, useMemo } from 'react';

const ListComponent = () => {
  
  // State Management with useState
  const [showCategoryA, setShowCategoryA] = useState(false); // Toggle for Category A visibility
  const [asyncData, setAsyncData] = useState([]); // Asynchronous data fetched from API
  const [searchTerm, setSearchTerm] = useState(''); // Search term for filtering items
  const [selectedCategory, setSelectedCategory] = useState('All'); // Selected category for filtering
  const [sortType, setSortType] = useState('name'); // Sorting type (by name or category)
  const [showDeepCopied, setShowDeepCopied] = useState(false); // Toggle for displaying deep copied data

  // Side Effect with useEffect for data fetching
  useEffect(() => {
    const fetchData = async () => {
      setTimeout(() => {
        setAsyncData([
          { id: 5, name: "Async of Item 1", category: "A" },
          { id: 6, name: "Async of Item 2", category: "B" },
          { id: 7, name: "Async of Item 3", category: "A" },
          { id: 8, name: "Async of Item 4", category: "C" },
          { id: 9, name: "Async of Item 5", category: "A" },
        ]);
      }, 2000);
    };

    fetchData();
  }, []);

  // Memoization with useMemo for deep copying data
  const deepCopiedData = useMemo(() => [...asyncData], [asyncData]);

  // Memoization for derived data with useMemo
  const filteredItems = useMemo(() => {
    let filtered = asyncData.filter((item) => {
      return (item.name.toLowerCase().includes(searchTerm.toLowerCase()) || searchTerm === '') &&
             (selectedCategory === 'All' || item.category === selectedCategory);
    });

    if (sortType === 'name') {
      filtered.sort((a, b) => a.name.localeCompare(b.name));
    } else if (sortType === 'category') {
      filtered.sort((a, b) => a.category.localeCompare(b.category));
    }

    return filtered;
  }, [asyncData, searchTerm, sortType, selectedCategory]);

  // Memoization with useCallback for rendering list items
  const renderListItems = useCallback((itemList) => {
    return itemList.map((item) => (
      <li key={item.id}>{item.name} - {item.category}</li>
    ));
  }, []);

  // Memoization with useCallback for toggling visibility
  const toggleCategoryAVisibility = useCallback(() => {
    setShowCategoryA(prev => !prev);
  }, []);

  return (
    <div>
      {/* JSX rendering with React components and state */}
      <h2>List of Items:</h2>

      {/* Search input */}
      <input 
        type="text" 
        placeholder="Search by name..." 
        value={searchTerm} 
        onChange={(e) => setSearchTerm(e.target.value)} 
      />

      {/* Dropdown to select category */}
      <select value={selectedCategory} onChange={(e) => setSelectedCategory(e.target.value)}>
        <option value="All">All Categories</option>
        <option value="A">Category A</option>
        <option value="B">Category B</option>
        <option value="C">Category C</option>
      </select>

      {/* Dropdown to select sorting type */}
      <select value={sortType} onChange={(e) => setSortType(e.target.value)}>
        <option value="name">Sort by Name</option>
        <option value="category">Sort by Category</option>
      </select>

      {/* Display filtered and sorted items */}
      <ul>{renderListItems(filteredItems)}</ul>

      {/* Button to toggle deep copied data */}
      <button onClick={() => setShowDeepCopied(prev => !prev)}>
        Toggle Deep Copied Data
      </button>

      {/* Conditional rendering with React components */}
      {showDeepCopied && (
        <div>
          <h2>Deep Copied Data:</h2>
          <pre>{JSON.stringify(deepCopiedData, null, 2)}</pre>
        </div>
      )}

      {/* Find Example */}
      <h2>Find Example:</h2>
      <p>Item with id 6: {filteredItems.find((item) => item.id === 6)?.name ?? "Not found"}</p>

      {/* Filter Example */}
      <h2>Filter Example:</h2>
      <button onClick={toggleCategoryAVisibility}>
        Toggle Category A Visibility
      </button>
      
      {/* Conditional rendering for Category A */}
      {showCategoryA && (
        <div>
          <h2>Category A Items:</h2>
          <ul>{renderListItems(filteredItems.filter(item => item.category === 'A'))}</ul>
        </div>
      )}

      {/* Horizontal separator */}
      <hr />
    </div>
  );
};

export default ListComponent;
```

Hierarchical tree diagram to illustrate how each function and hook in the `ListComponent` is connected:

```
ListComponent
|
|-- useState
|   |-- showCategoryA
|   |-- asyncData
|   |-- searchTerm
|   |-- selectedCategory
|   |-- sortType
|   |-- showDeepCopied
|
|-- useEffect (Fetch Data)
|   |-- fetchData
|
|-- useMemo (Deep Copy)
|   |-- deepCopiedData
|   |   |-- asyncData
|
|-- useMemo (Filtered Items)
|   |-- filteredItems
|   |   |-- asyncData
|   |   |-- searchTerm
|   |   |-- sortType
|   |   |-- selectedCategory
|
|-- useCallback (Render List Items)
|   |-- renderListItems
|
|-- useCallback (Toggle Category A Visibility)
|   |-- toggleCategoryAVisibility
|
|-- JSX Rendering
    |-- Input (Search)
    |   |-- searchTerm
    |
    |-- Dropdown (Category Selection)
    |   |-- selectedCategory
    |
    |-- Dropdown (Sort Type)
    |   |-- sortType
    |
    |-- List Rendering (Filtered Items)
    |   |-- filteredItems
    |   |-- renderListItems
    |
    |-- Button (Toggle Deep Copied Data)
    |   |-- showDeepCopied
    |
    |-- Conditional Rendering (Deep Copied Data)
    |   |-- showDeepCopied
    |   |-- deepCopiedData
    |
    |-- Conditional Rendering (Category A)
        |-- showCategoryA
        |-- filteredItems
        |-- renderListItems
```


Walk through the `ListComponent` step by step, explaining its functionality:


### **Component Initialization and State Setup**:
1.You're initializing various states using `useState` to manage the component's behavior.

```javascript
const [showCategoryA, setShowCategoryA] = useState(false);
const [asyncData, setAsyncData] = useState([]);
const [searchTerm, setSearchTerm] = useState('');
const [selectedCategory, setSelectedCategory] = useState('All');
const [sortType, setSortType] = useState('name');
const [showDeepCopied, setShowDeepCopied] = useState(false);
```

### **Data Fetching with `useEffect`**:
2.After the component mounts, a delay of 2 seconds simulates asynchronous data fetching. After the delay, the fetched data is set into the `asyncData` state.

```javascript
useEffect(() => {
  const fetchData = async () => {
    setTimeout(() => {
      setAsyncData([
        { id: 5, name: "Async of Item 1", category: "A" },
        { id: 6, name: "Async of Item 2", category: "B" },
        { id: 7, name: "Async of Item 3", category: "A" },
        { id: 8, name: "Async of Item 4", category: "C" },
        { id: 9, name: "Async of Item 5", category: "A" },
      ]);
    }, 2000);
  };

  fetchData();
}, []);
```


### **Deep Copying Data with `useMemo`**:
3.You create a deep copy of `asyncData` to ensure immutability.

```javascript
const deepCopiedData = useMemo(() => [...asyncData], [asyncData]);
```

### **Filtering and Sorting Data with `useMemo`**:
4.Based on the current state values, you filter and sort the items in `asyncData` and store them in `filteredItems`.

```javascript
const filteredItems = useMemo(() => {
  let filtered = asyncData.filter((item) => {
    return (item.name.toLowerCase().includes(searchTerm.toLowerCase()) || searchTerm === '') &&
           (selectedCategory === 'All' || item.category === selectedCategory);
  });

  if (sortType === 'name') {
    filtered.sort((a, b) => a.name.localeCompare(b.name));
  } else if (sortType === 'category') {
    filtered.sort((a, b) => a.category.localeCompare(b.category));
  }

  return filtered;
}, [asyncData, searchTerm, sortType, selectedCategory]);
```

### **Rendering List Items with `useCallback`**:
5.You memoize the function to render list items to optimize performance.

```javascript
const renderListItems = useCallback((itemList) => {
  return itemList.map((item) => (
    <li key={item.id}>{item.name} - {item.category}</li>
  ));
}, []);
```

### **Toggling Category A Visibility with `useCallback`**:
6.Another memoized function to toggle the visibility of Category A items.

```javascript
const toggleCategoryAVisibility = useCallback(() => {
  setShowCategoryA(prev => !prev);
}, []);
```

### **JSX Rendering**:
7.Finally, you render the UI based on the component's state and user interactions. This includes:
- Search input.
- Dropdowns for category selection and sorting type.
- A button to toggle deep-copied data.
- Lists to display items.
- Conditional renderings for deep-copied data and Category A items.

The logic revolves around state management, asynchronous data fetching, data manipulation (filtering and sorting), and rendering the UI accordingly, while the data provides a mock list of items with their respective details.
In summary, the `ListComponent` manages a list of items fetched asynchronously. It allows users to search, filter, and sort the items. Additionally, users can view a deep-copied version of the data and toggle the visibility of items belonging to Category A.


Break down the code step-by-step and explain its functionality:

### 1. **Import Statements**:
```javascript
import React, { useState, useEffect, useCallback, useMemo } from 'react';
```
- **Explanation**: Imports necessary React hooks and functions for this component.

### 2. **Component State Setup**:
```javascript
const [showCategoryA, setShowCategoryA] = useState(false);
const [asyncData, setAsyncData] = useState([]);
const [searchTerm, setSearchTerm] = useState('');
const [selectedCategory, setSelectedCategory] = useState('All');
const [sortType, setSortType] = useState('name');
const [showDeepCopied, setShowDeepCopied] = useState(false);
```
- **Explanation**:
  - Various state variables are initialized using `useState`.
  - These states manage different aspects of the component, such as visibility toggles, fetched data, search terms, and sorting options.

### 3. **Data Fetching with `useEffect`**:
```javascript
useEffect(() => {
  // ... asynchronous data fetching logic
}, []);
```
- **Explanation**:
  - When the component mounts, the `useEffect` hook triggers the `fetchData` function after a 2-second delay (simulated).
  - The fetched data is then set into the `asyncData` state.

### 4. **Deep Copying Data with `useMemo`**:
```javascript
const deepCopiedData = useMemo(() => [...asyncData], [asyncData]);
```
- **Explanation**:
  - `useMemo` creates a memoized value `deepCopiedData` which is a deep copy of `asyncData`.
  - This ensures that `deepCopiedData` doesn't change unless `asyncData` changes, optimizing performance.

### 5. **Filtering and Sorting Data with `useMemo`**:
```javascript
const filteredItems = useMemo(() => {
  // ... filtering and sorting logic
}, [asyncData, searchTerm, sortType, selectedCategory]);
```
- **Explanation**:
  - `filteredItems` is derived from `asyncData`, filtered based on search terms, category, and sorted based on the selected sorting type.

### 6. **Rendering List Items with `useCallback`**:
```javascript
const renderListItems = useCallback((itemList) => {
  // ... rendering logic
}, []);
```
- **Explanation**:
  - `renderListItems` is a memoized callback function that renders a list of items.
  - It ensures that the function reference remains consistent across renders unless its dependencies change.

### 7. **Toggling Category A Visibility**:
```javascript
const toggleCategoryAVisibility = useCallback(() => {
  setShowCategoryA(prev => !prev);
}, []);
```
- **Explanation**:
  - The `toggleCategoryAVisibility` function toggles the visibility of Category A items.

### 8. **JSX Rendering**:
- The remaining portion of the code renders the JSX elements based on the component's state and user interactions. This includes:
  - Search input for filtering items.
  - Dropdowns for selecting a category and sorting type.
  - List items displayed based on the filtered and sorted data.
  - Buttons for toggling deep copied data and Category A visibility.
  - Conditional rendering of Category A items based on the `showCategoryA` state.

### Conclusion:
This component, `ListComponent`, showcases how to manage state using React's hooks (`useState`, `useEffect`, `useCallback`, `useMemo`). It fetches and displays mock data, provides filtering and sorting functionalities, and demonstrates the use of memoization for optimizing performance.

Below is a documentation for the given `ListComponent`:
---

# ListComponent Documentation

## Overview

`ListComponent` is a React component designed to display a list of items fetched asynchronously and provides various functionalities like filtering, sorting, and toggling visibility based on categories.

## Structure

The component is structured with the following sections:

1. **State Management**: Using `useState` for managing component states.
2. **Side Effects**: Utilizing `useEffect` for side effects like data fetching.
3. **Memoization**: Employing `useMemo` for optimized rendering.
4. **Rendering**: Rendering JSX elements based on the component states.

## State Management

### States:

1. `showCategoryA`: Controls the visibility of items under Category A.
2. `asyncData`: Stores asynchronously fetched data.
3. `searchTerm`: Holds the search string for filtering.
4. `selectedCategory`: Manages the selected category for filtering.
5. `sortType`: Determines the type of sorting.
6. `showDeepCopied`: Toggles the visibility of deep copied data.

## Functionalities

### Data Fetching

- Uses `useEffect` to fetch asynchronous data after 2 seconds (simulated delay).

### Filtering and Sorting

- Filters items based on search term and selected category.
- Sorts filtered items either by name or category.

### Memoization

- Memoizes deep copied data using `useMemo`.
- Memoizes filtered items based on various dependencies.

### Rendering

- Dynamically renders list items, toggles, and other UI elements based on state values.

## Components and UI Elements

1. **Search Input**: For entering search terms.
2. **Dropdowns**: 
    - To select a category.
    - To select sorting type.
3. **List**: Displays filtered and sorted items.
4. **Buttons**: 
    - Toggle deep copied data.
    - Toggle Category A visibility.

## Examples

- **Find Example**: Demonstrates finding an item by its ID.
- **Filter Example**: Toggles the visibility of items in Category A.

---

This documentation provides a concise overview of the `ListComponent`. Depending on the project's complexity and requirements, you might want to expand on specific sections, add screenshots, or provide examples of usage.
