# reactTodoList
Created with CodeSandbox

This React todo list helped me to better learn and practice working with the component tree in React. 

A detailed walkthrough of the pertinent React code is given below. The project is split into 4 files and three components. The App() component as well as the ToDoItem() and InputArea() components which are all separate jsx files excepting index.js which only imports App.jsx.

The idea is to gain state over the input field as well as the array which holds the list items. To this the states were completed one at a time startiing with input state. InputArea() monitors the input field and the use of props allows for a connection between this component and the addItem function in the App.jsx component. onChange detects when the <input /> updates. When the button is clicked it calls the onAdd() function and passes in the current text inside the innput into inputText. This allows the inputText to get passed over inside the addItem function of App.jsx so it can be added inside the items array where all list-items are stored. 
```React
function InputArea(props) {
  const [inputText, setInputText] = useState("");

  function handleChange(event) {
    const newValue = event.target.value;
    setInputText(newValue);
  }

  return (
    <div className="form">
      <input onChange={handleChange} type="text" value={inputText} />
      <button
        onClick={() => {
          props.onAdd(inputText);
          setInputText("");
        }}
      >
        <span>Add</span>
      </button>
    </div>
  );
}
```

Props used inside the <ToDoItem /> link it with the ToDoItem() function inside of ToDoItem.jsx. This function generates the id for the list items which is required for the deleteItem() function in App.jsx.
```React

function ToDoItem(props) {
  return (
    <div
      onClick={() => {
        props.onChecked(props.id);
      }}
    >
      <li>{props.text}</li>
    </div>
  );
}
```

To hold list items and to monitor its state another destructuring was done inside of App().
```React
const [items, setItems] = useState([]);
```

The function addItem() inside of App.jsx takes in the inputText passed from InputArea.jsx and the function setItems() takes in the previous items of the items array and returns a new array with those previous items as well as the inputText just passed in, thus making an array with an index of +1 than before. 
```React
 function addItem(inputText) {
    setItems(prevItems => {
      return [...prevItems, inputText];
    });
  }
  ```
  
  The function deleteItem() takes in an id from ToDoItem() and the function setItems() takes the items array with all of the items inside of it and filters through it comparing the index numbers of each item in the array with the id number which was passed into the deleteItem() function. Since the index# and id#'s of the items match, the item in the array with the matching index# to the id# gets deleted.
  ```React
    function deleteItem(id) {
    setItems(prevItems => {
      return prevItems.filter((item, index) => {
        return index !== id;
      });
    });
  }
  ```
  
  When InputArea is called the prop addItem gets passed over into the InputArea(). The items [] is mapped over and each item is returned with the given set properties.
  ```React
   return (
    <div className="container">
      <div className="heading">
        <h1>To-Do List</h1>
      </div>
      <InputArea onAdd={addItem} />
      <div>
        <ul>
          {items.map((todoItem, index) => (
            <ToDoItem
              key={index}
              id={index}
              text={todoItem}
              onChecked={deleteItem}
            />
          ))}
        </ul>
      </div>
    </div>
  );
  ```
  
  ***End Walkthrough
  
