# Angular 2 & TypeScript Training

## Trainers
- Roope Hakulinen
- Henri Ihalainen

---

## Background
- From Gofore Oy
- Working on EU funding applying & managing system for Ministry of Employment and the Economy (Työ- ja Elinkeinoministeriö)
- Used Angular 2 from its first beta (beginning of the year)

---

# Slides

[angular2-training.herokuapp.com](http://angular2-training.herokuapp.com/)

---

# Agenda

## Day 1
### Morning
- Introduction to SPAs
- TypeScript & Tooling
- Angular 2 Fundamentals
### Afternoon
- Angular 2 Advanced Topics

## Day 2
### Morning
- Reactive programming with Angular 2
### Afternoon
- Testing

---

# Introduction to SPA & Angular

---

# Agenda

1. What is a SPA?
2. Real-life examples
3. Technical overview

---

# What Is a SPA?

- "A single-page application (SPA) is a web application or web site that fits on a single web page with the goal of providing a more fluid user experience similar to a desktop application." - Wikipedia
- Browser fetches executable code that makes asynchronous calls for actual data to be shown
- Data is visualized and/or manipulated and stored back on server asynchronously


---

# Technical Overview

- Initial page load fetches index HTML and JavaScript
- JavaScript fetches the actual data via AJAX (Asynchronous JavaScript and XML). Data is usually JSON (or XML)
- JavaScript renders the data on templates and binds event handlers for clicks, keyboard input etc.

---

# Real-life Example
- [Google search](http://www.google.com)

---

# Angular 1

- MVC framework with dependency injection
- Emphasis on testability (decouples DOM manipulation from app logic)
- Two-way data binding

---

# Angular 2

- Universal apps
- Web workers
- Migration from 1
- Any rendering target (browser, mobile, desktop)
