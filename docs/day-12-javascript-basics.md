---
title: Day 12 — JavaScript Basics
parent: Week 3 — Building and Scripting
nav_order: 2
---

# Day 12 — JavaScript Basics in the Browser

**Syllabus:** Develop a web application using an appropriate scripting language
**Outcomes:** SE-12-02
**Textbook:** Section 12.5

---

## Key Terms

**JavaScript** — A scripting language that runs in the browser, enabling interactive and dynamic web pages. Unlike Python, JavaScript runs client-side by default.

**Variable** — A named container for storing a value. Use `const` for values that won't change, `let` for values that will.

**Function** — A reusable block of code. Defined with `function` keyword or arrow syntax.

**Event** — A browser signal that something happened (click, keypress, form submit). JavaScript listens for events and responds.

**console.log()** — Outputs a value to the browser DevTools console. The JavaScript equivalent of Python's `print()`.

---

## Theory

### JavaScript vs Python

| Concept | Python | JavaScript |
|---|---|---|
| Variable | `name = "Alex"` | `const name = "Alex"` |
| Print | `print(name)` | `console.log(name)` |
| Function | `def greet():` | `function greet() {}` |
| Boolean | `True / False` | `true / false` |
| None | `None` | `null` |
| List | `[1, 2, 3]` | `[1, 2, 3]` (array) |
| Dictionary | `{"key": "value"}` | `{key: "value"}` (object) |
| f-string | `f"Hi {name}"` | `` `Hi ${name}` `` |
| Comment | `# comment` | `// comment` |

### Adding JavaScript to HTML

```html
<!-- At the bottom of <body> — ensures HTML loads first -->
<script src="script.js"></script>

<!-- Or inline (small scripts only) -->
<script>
    console.log("Hello from JavaScript");
</script>
```

### Variables and Data Types

```javascript
const name = "Alex";          // string — won't change
let score = 0;                // number — will change
let active = true;            // boolean
let nothing = null;           // null

// Template literals (like f-strings)
console.log(`Hello ${name}, your score is ${score}`);

// Arrays
const subjects = ["Maths", "Software Engineering", "Physics"];
console.log(subjects[0]);     // "Maths"
console.log(subjects.length); // 3

// Objects (like Python dictionaries)
const student = {
    name: "Alex",
    year: 12,
    score: 85
};
console.log(student.name);    // "Alex"
```

### Functions

```javascript
// Standard function
function greet(name) {
    return `Hello, ${name}!`;
}

// Arrow function (common in modern JS)
const double = (n) => n * 2;

console.log(greet("Alex"));   // "Hello, Alex!"
console.log(double(5));       // 10
```

### Conditionals and Loops

```javascript
// If/else
if (score >= 90) {
    console.log("Excellent");
} else if (score >= 50) {
    console.log("Pass");
} else {
    console.log("Fail");
}

// For loop
for (let i = 0; i < 5; i++) {
    console.log(i);
}

// Array forEach (like Python's for-in)
subjects.forEach((subject) => {
    console.log(subject);
});

// Array map — transforms each item
const upper = subjects.map((s) => s.toUpperCase());
```

### Events

```javascript
// Run code when the page has fully loaded
document.addEventListener("DOMContentLoaded", () => {
    console.log("Page ready");
});

// Button click
const btn = document.getElementById("myButton");
btn.addEventListener("click", () => {
    console.log("Button clicked");
});
```

---

## Objective Response Questions

**1.** Which JavaScript keyword should be used to declare a variable whose value will change during program execution?

- A) `const`
- B) `var`
- C) `let`
- D) `static`

**Answer: C** — `let` declares a reassignable variable. `const` is for values that will not change.

---

**2.** What is the output of the following code?
```javascript
const nums = [10, 20, 30];
console.log(nums[1] * 2);
```

- A) 20
- B) 40
- C) 10
- D) 60

**Answer: B** — `nums[1]` is `20`. `20 * 2 = 40`.

---

**3.** A developer wants to run a function only after the user clicks a button. Which approach is correct?

- A) Call the function at the top of the script file
- B) Use `document.addEventListener("DOMContentLoaded", ...)` on the button
- C) Use `btn.addEventListener("click", ...)` on the button element
- D) Use `setInterval()` to check if the button was clicked

**Answer: C**

---

## Try It

Create `script.js` and link it to your `index.html`:

1. Declare an array of five items relevant to your page topic
2. Use `forEach` to log each item to the console
3. Write a function that takes a number and returns whether it is even or odd
4. Test it with three different values using `console.log`
5. Open the Console panel in DevTools and verify the output
