```
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


