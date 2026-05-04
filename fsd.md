#Pro1
console.log("Hello World");
let num1 = 5; 
let num2 = 10; 
let sum = num1 + num2; 
console.log("The sum is: " + sum);
let cities = ["New York", "London", "Paris", "Tokyo", "Sydney"];
console.log("Total number of cities: " + cities.length);
cities.push("Berlin");
console.log("Cities after adding a new one: " + cities);
cities.shift();
console.log("Cities after removing the first one: " + cities);
let indexOfCity = cities.indexOf("Paris");
console.log("Index of Paris: " + indexOfCity);


#Pro2
var ipstring = "Welcome to JavaScript Lab";
var ipstringlen=ipstring.length;
console.log("String length is "+ipstringlen);
var pos=ipstring.indexOf('JavaScript');
console.log(pos);
var newstr=ipstring.substr(pos,10);
console.log(newstr);
var strrep=ipstring.replace("Lab","Class");
console.log(strrep);
function isPalindrome(str) {
 let rev = str.split("").reverse().join("");
 if (rev == str) {
 return true
 }
 return false
}
let str1 = "racecar";
let str2 = "nitin";
let str3 = "Rama";
console.log(isPalindrome(str1));
console.log(isPalindrome(str2));
console.log(isPalindrome(str3));


#Pro3
let student = {
 name: "John Doe",
 grade: 85,
 subjects: ["Math", "Science", "History"],
 displayInfo: function() {
 console.log(`Name: ${this.name}`);
 console.log(`Grade: ${this.grade}`);
 console.log(`Subjects: ${this.subjects.join(", ")}`);
 }
};
if (student.grade >= 60) {
 student.passed = true;
}
else {
 student.passed = false;
}
for (let key in student) {
if(typeof student[key]!=='function'){
console.log(`${key}: ${student[key]}`);}}


#Pro4
<!DOCTYPE html>
<html lang="en">
<head>
 <meta charset="UTF-8">
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 <title>Event Listeners</title>
 <style>
 img {
 width: 300px;
 height: 200px;
 border: 5px solid black;
 position:center;
 transition: border 0.3s ease;
 }
button {
 padding: 5px 10px;
 background: #000;
 color: #fff;
 border-radius: 5px;
 cursor: pointer;
 }
 </style>
</head>
<body>
 <!-- Button -->
 <button id="myButton">Click Me</button>
 <!-- Image -->
 <img id="myImage" src="https://i.pinimg.com/736x/6e/b9/a8/6eb9a8b43a13cf33dae7d2fcd83daf8a.jpg" alt="Sample Image">
 <script>
 // Button click event
 const button = document.getElementById('myButton');
 button.addEventListener('click', () => {
    window.alert('Button clicked!');
 });
 // Image mouseover event
 const image = document.getElementById('myImage');
 image.addEventListener('mouseover', () => {
 image.style.border = '5px solid blue';
 });
 // Reset border when mouse leaves image
 image.addEventListener('mouseout', () => {
 image.style.border = 'none';
 });
 // Keypress event listener
 document.addEventListener('keydown', (event) => {
 window.alert('Key pressed:'+ event.key);
 });
 </script>
</body>
</html>



#prog5
Step1: npx create-react-app issue-tracker.
Step 2: cd issue-tracker
Step 3: npm start
#app.js
import React from 'react';
import './App.css';
 const issues = [
 {
 id: 1,
 title: 'Bug in authentication',
 description: 'User unable to log in with correct credentials.',
 status: 'Open'
 },
 {
 id: 2,
 title: 'UI layout misalignment',
 description: 'The navigation bar is not aligned on mobile screens.',
 status: 'Closed'
 },
 {
 id: 3,
 title: 'Server downtime',
 description: 'The server crashed during high traffic.',
 status: 'Open'
 }
];
const IssueList = () => {
 return (
<div>
 <h2>Issue Tracker</h2>
 <ul>
 {issues.map((issue) => (
  <li key={issue.id}>
 <h3>{issue.title}</h3>
 <p>{issue.description}</p>
 <span>Status: <strong>{issue.status}</strong></span>
 </li>
 ))}
 </ul>
 </div>
 );
};
function App() {
 return (
 <div className="App">
 <IssueList />
 </div>
 );
}
export default App;
#app.css
.App {
 font-family: Arial, sans-serif;
 margin: 20px;
}
h2 {
 text-align: center;
 color: #4CAF50;
}
ul {
 list-style-type: none;
 padding: 0;
}
li {
 margin: 20px;
 padding: 15px;
 background-color: #f9f9f9;
 border: 1px solid #ddd;
 border-radius: 5px;
 }
h3 {
 margin: 0;
 color: #333;
}
span {
 display: block;
 margin-top: 10px;
 font-weight: bold;
}


#prog6
Step1: npx create-react-app counter.
Step 2: cd counter
Step 3: npm start
#app.js
import React, { useState, useEffect } from 'react';
const Counter = () => {
 // Initialize state variable count to 0
 const [count, setCount] = useState(0);
 // Simulate fetching initial data using useEffect
 useEffect(() => {
 const fetchData = async () => {
 try {
 // Simulating an API call (could fetch data and update count)
 const initialCount = await new Promise((resolve, reject) => {
 setTimeout(() => {
 resolve(0); // Assume the initial count is fetched as 0
 }, 1000); // Simulating a delay for data fetching
 });
 // Update the state with the fetched count
 setCount(initialCount);
 } catch (error) {
 console.error("Error fetching data:", error);
 }
 };
 fetchData(); // Fetch data when the component mounts
 }, []); // Empty dependency array ensures this runs only once on mount
  // Handle increment action
 const increment = () => setCount(count + 1);
 // Handle decrement action
 const decrement = () => setCount(count - 1);
 // Handle doubling the count value
 const double = () => setCount(count * 2);
 // Handle resetting the count to 0
 const reset = () => setCount(0);
 return (
 <div>
 <h1>Counter: {count}</h1>
 <div>
 <button onClick={increment}>Increment</button>
 <button onClick={decrement}>Decrement</button>
 <button onClick={double}>Double</button>
 <button onClick={reset}>Reset</button>
 </div>
 <div>
 {/* Additional messages to demonstrate edge cases */}
 {count < 0 && <p>The count cannot be less than 0!</p>}
 {count === 0 && <p>Count is at the initial value!</p>}
 </div>
 </div>
 );
};
 export default Counter;


 #prog7
 Step 1: Initialize a Node.js Project
 Open integrated vscode terminal. Execute the below commands:
➢ mkdir express-api
➢ cd express-api
➢ npm init -y
This creates a package.json file with default values.
Step 2: Install Express:
Install express.js by executing below command:
➢ npm install express
This installs Express and adds it to your package.json.
Step 3: Create server.js

const express = require('express');
const app = express();
// Middleware to log requests to the console
app.use((req, res, next) => {
 console.log(`${req.method} request made to: ${req.url}`);
 next();
});
// Middleware to parse JSON payloads
app.use(express.json());
// Root endpoint to return "Hello, Express!"
app.get('/', (req, res) => {
 res.send('Hello, Express!');
});
// Sample in-memory data for products
let products = [
 { id: 1, name: 'Laptop', price: 1000 },
 { id: 2, name: 'Phone', price: 500 }
];
// GET /products: Returns a list of all products
app.get('/products', (req, res) => {
 res.json(products);
});
// POST /products: Adds a new product
app.post('/products', (req, res) => {
 const { name, price } = req.body;
 if (!name || !price) {
 return res.status(400).json({ message: 'Name and price are required' });
 }
 const newProduct = { id: products.length + 1, name, price };
 products.push(newProduct);
 res.status(201).json(newProduct);
});
// GET /products/:id: Returns details of a specific product
app.get('/products/:id', (req, res) => {
 const product = products.find(p => p.id === parseInt(req.params.id));
 if (!product) {
 return res.status(404).json({ message: 'Product not found' });
 }
 res.json(product);
});
// PUT /products/:id: Updates an existing product
app.put('/products/:id', (req, res) => {
 const product = products.find(p => p.id === parseInt(req.params.id));
 if (!product) {
 return res.status(404).json({ message: 'Product not found' });
 }
 const { name, price } = req.body;
 if (!name || !price) {
 return res.status(400).json({ message: 'Name and price are required' });
 }
 product.name = name;
 product.price = price;
 res.json(product);
});
// DELETE /products/:id: Deletes a product
app.delete('/products/:id', (req, res) => {
 const productIndex = products.findIndex(p => p.id === parseInt(req.params.id));
 if (productIndex === -1) {
 return res.status(404).json({ message: 'Product not found' });
 }
 products.splice(productIndex, 1);
 res.status(204).send();
});
// Start the server on port 3000
const PORT = 8080;
app.listen(PORT, () => {
 console.log(`Server is running on http://localhost:${PORT}`);
});

