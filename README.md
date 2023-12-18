# GithubRepoRef

import React, { useState, useEffect } from 'react';

const ListComponent = () => {
  // State to manage the visibility of Category A items
  const [showCategoryA, setShowCategoryA] = useState(false);

  // State to store asynchronously fetched data
  const [asyncData, setAsyncData] = useState([]);

  // Simulate asynchronous data fetching
  useEffect(() => {
    const fetchData = async () => {
      // Simulate an API call or any asynchronous operation
      // In this example, we're using a simple setTimeout
      setTimeout(() => {
        setAsyncData([
          { id: 5, name: "Async of Item 1", category: "A" },
          { id: 6, name: "Async of Item 2", category: "B" },
          { id: 7, name: "Async of Item 3", category: "A" },
        ]);
      }, 3000); // Simulating a 1-second delay
    };

    // Call the fetchData function when the component mounts
    fetchData();
  }, []); // The empty dependency array ensures useEffect runs only once on mount

  // Find a specific item by ID
  const specificItem = asyncData.find((item) => item.id === 6);

  // Filter items belonging to Category A
  const categoryAItems = asyncData.filter((item) => item.category === 'A');

  // Function to render list items
  const renderListItems = (itemList) => {
    return itemList.map((item) => (
      <li key={item.id}>{item.name}</li>
    ));
  };

  // Callback function to toggle visibility of Category A items
  const toggleCategoryAVisibility = () => {
    setShowCategoryA(!showCategoryA);
  };

  return (
    <div>
      {/* Display the list of all items */}
      <h2>List of Items:</h2>
      <ul>{renderListItems(asyncData)}</ul>

      {/* Example of finding an item by ID */}
      <h2>Find Example:</h2>
      <p>Item with id 6: {specificItem ? specificItem.name : "Not found"}</p>

      {/* Example of filtering items based on a condition */}
      <h2>Filter Example:</h2>
      {/* Button to toggle visibility of Category A items */}
      <button onClick={toggleCategoryAVisibility}>
        Toggle Category A Visibility
      </button>
      
      {/* Display Category A items based on visibility */}
      {showCategoryA && (
        <div>
          <h2>Category A Items:</h2>
          <ul>{renderListItems(categoryAItems)}</ul>
        </div>
      )}

      {/* Horizontal line for separation */}
      <hr />
    </div>
  );
};

export default ListComponent;
