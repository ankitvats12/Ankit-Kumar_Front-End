# Question 1
##  Explain what the simple List component does.

# Answer 1
This simple `List` component is a React component that displays a list of items with the ability to select one item at a time. 
`SingleListItem` and `ListComponent` are its `two` sub-components. 
`SingleListItem` creates a `single list` item with the following attributes:
- `onClickHandler`: a `function` that is run when the `item is clicked`. 
- `index`: an `integer` that denotes the index of the `item in the list`.
- `text`: a `string` that represents the text that will be `displayed in the item`.

The `ListComponent` produces the list by `traversing` the array of `items` that is provided as a parameter. Each item has a `"SingleListItem"` component that is rendered and is given the required attributes. Additionally, it uses the `useState` hook to manage the item index that is `currently chosen` and `useEffect` to reset the selected index to `null` if the `items` prop `changes`.

The `List` component also `exports` the `ListComponent` wrapped in a `memo` `HOC (High Order Component)` to improve `efficiency` by preventing pointless `re-renders`. Utilising the `PropTypes` library, it additionally specifies the `anticipated` types for the `props`.


# Question 2
## What problems / warnings are there with code?

# Answer 2
There are a few problems / warnings with this code:

1. In the `SingleListItem` component, the `onClickHandler` prop is defined as a required function, but it is not actually a function. Instead, it is defined as an arrow function that calls `handleClick` with the `index` argument. To fix this, you can change the prop type to `PropTypes.oneOfType([PropTypes.func, PropTypes.bool])`.
2. In the `SingleListItem` component, the `isSelected` prop is passed to the `style` object as a boolean value (`true` or `false`). However, the `backgroundColor` style property expects a string value, so this will result in a warning. To fix this, you can change the prop type to `PropTypes.oneOfType([PropTypes.bool, PropTypes.string])` and pass a string value (e.g. `'green'` or `'red'`) to the `style` object.

3. In the `WrappedListComponent` component, the `items` prop is defined as an array of objects with a `text` property. However, the `PropTypes.array` function does not exist; it should be `PropTypes.arrayOf(PropTypes.shape({ text: PropTypes.string.isRequired }))`.
4. In the `WrappedListComponent` component, the `items` prop is defined with a default value of `null`, but it should be an empty array (`[]`) instead.


# Question 3
## Please `fix`, `optimize`, and/or `modify` the component as much as you think is necessary.

# Answer 3

```JSX
import React, { useState, useCallback } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const SingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={() => onClickHandler(index)}
    >
      {text}
    </li>
  );
};

SingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

// List Component
const List = ({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState(null);

  const handleClick = useCallback((index) => {
    setSelectedIndex(index);
  }, []);

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map(({ text }, index) => (
        <SingleListItem
          key={index}
          onClickHandler={() => handleClick(index)}
          text={text}
          index={index}
          isSelected={selectedIndex === index}
        />
      ))}
    </ul>
  );
};

List.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({ text: PropTypes.string.isRequired })),
};

List.defaultProps = {
  items: [],
};

export default List;
```
