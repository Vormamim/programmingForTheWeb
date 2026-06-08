# Day 13 — JavaScript: DOM Manipulation and Events

**Syllabus:** Develop a web application using an appropriate scripting language
**Outcomes:** SE-12-02
**Textbook:** Section 12.5

---

## Key Terms

**DOM (Document Object Model)** — The browser's in-memory tree representation of an HTML page. JavaScript reads and modifies the DOM to change what the user sees — without reloading the page.

**Element** — A node in the DOM corresponding to an HTML tag.

**Event listener** — A function registered to run when a specific event occurs on an element.

**innerHTML** — A property that gets or sets the HTML content inside an element.

**classList** — A property that provides methods to add, remove, and toggle CSS classes on an element.

---

## Theory

### Selecting Elements

```javascript
// By ID — returns one element
const heading = document.getElementById("main-heading");

// By CSS selector — returns first match
const btn = document.querySelector(".submit-btn");

// By CSS selector — returns all matches (NodeList)
const cards = document.querySelectorAll(".card");
```

### Reading and Changing Content

```javascript
const heading = document.getElementById("main-heading");

// Read content
console.log(heading.textContent);  // plain text
console.log(heading.innerHTML);    // HTML including tags

// Change content
heading.textContent = "Updated Heading";
heading.innerHTML = "<em>Updated</em> Heading";
```

### Changing Styles and Classes

```javascript
const box = document.querySelector(".box");

// Inline style
box.style.backgroundColor = "navy";
box.style.fontSize = "20px";

// CSS classes (preferred — keeps style in CSS)
box.classList.add("active");
box.classList.remove("hidden");
box.classList.toggle("open");       // adds if absent, removes if present
box.classList.contains("active");   // true or false
```

### Creating and Inserting Elements

```javascript
// Create a new element
const item = document.createElement("li");
item.textContent = "New item";

// Add to the page
const list = document.getElementById("my-list");
list.appendChild(item);

// Remove an element
item.remove();
```

### Common Events

```javascript
const btn = document.getElementById("btn");
const input = document.getElementById("search");

// Click
btn.addEventListener("click", () => {
    console.log("Clicked");
});

// Input — fires on every keystroke
input.addEventListener("input", (e) => {
    console.log(e.target.value);    // current value of the input
});

// Form submit
const form = document.getElementById("my-form");
form.addEventListener("submit", (e) => {
    e.preventDefault();             // stop page reload
    console.log("Form submitted");
});

// Page loaded
document.addEventListener("DOMContentLoaded", () => {
    console.log("DOM ready");
});
```

### Practical Example — Show/Hide Toggle

```html
<button id="toggle-btn">Show Details</button>
<div id="details" class="hidden">
    <p>Here are the details...</p>
</div>
```

```css
.hidden { display: none; }
```

```javascript
const btn = document.getElementById("toggle-btn");
const details = document.getElementById("details");

btn.addEventListener("click", () => {
    details.classList.toggle("hidden");
    btn.textContent = details.classList.contains("hidden")
        ? "Show Details"
        : "Hide Details";
});
```

### Practical Example — Live List Builder

```html
<input type="text" id="item-input" placeholder="Add item">
<button id="add-btn">Add</button>
<ul id="item-list"></ul>
```

```javascript
document.getElementById("add-btn").addEventListener("click", () => {
    const input = document.getElementById("item-input");
    const value = input.value.trim();

    if (value === "") return;

    const li = document.createElement("li");
    li.textContent = value;
    document.getElementById("item-list").appendChild(li);

    input.value = "";
    input.focus();
});
```

---

## Objective Response Questions

**1.** A developer calls `e.preventDefault()` inside a form submit event listener. What does this do?

- A) Clears all form fields
- B) Prevents the form from sending data to the server
- C) Stops the page from reloading on submit
- D) Validates the form inputs automatically

**Answer: C** — `preventDefault()` stops the browser's default behaviour. For forms, the default is to submit and reload the page.

---

**2.** What is the difference between `textContent` and `innerHTML`?

- A) `textContent` works only on headings; `innerHTML` works on all elements
- B) `textContent` sets plain text; `innerHTML` parses and renders HTML tags
- C) `textContent` is read-only; `innerHTML` can be written to
- D) There is no practical difference

**Answer: B** — Setting `innerHTML` to `"<em>hello</em>"` renders italic text. Setting `textContent` to the same string displays the literal characters `<em>hello</em>`.

---

**3.** Which method adds a CSS class to an element without removing existing classes?

- A) `element.className = "new-class"`
- B) `element.classList.toggle("new-class")`
- C) `element.classList.add("new-class")`
- D) `element.setAttribute("class", "new-class")`

**Answer: C** — `classList.add()` appends the class. Option A replaces all classes. Option D replaces all classes via the attribute.

---

## Try It

Add to your `index.html` and `script.js`:

1. A button that toggles a "dark mode" class on the `<body>` — write CSS for `.dark-mode` that changes background and text colour
2. A text input and Add button that appends list items to a `<ul>` (use the live list builder pattern above)
3. A character counter that updates live as the user types in a `<textarea>` — display the count below the textarea
4. Prevent the default submit behaviour on any form on your page and log the input values to the console instead
