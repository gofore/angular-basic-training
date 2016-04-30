# Introduction to SPA

---

# Agenda

1. What is a SPA?
2. Real-life examples
3. Technical overview

---

# What is a SPA?

- "A single-page application (SPA) is a web application or web site that fits on a single web page with the goal of providing a more fluid user experience similar to a desktop application." - Wikipedia
- Browser fetches executable code that makes asynchronous calls for actual data to be shown
- Data is visualized and/or manipulated and stored back on server asynchronously

---

# Real-life examples

- [Google search](http://www.google.com)
-

---

# Technical overview

- Initial page load fetches index HTML and JavaScript
- JavaScript fetches the actual data via AJAX (Asynchronous JavaScript and XML). Data is usually JSON (or XML)
- JavaScript renders the data on templates and binds event handlers for clicks, keyboard input etc.
