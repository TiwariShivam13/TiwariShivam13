React + Vite Complete Developer Revision Guide

This document is your end-to-end reference for building production-grade React apps with Vite. Use it as a checklist, cheat sheet, and template when starting or revising a project.


---

1) Create a React App with Vite

npm create vite@latest my-app
cd my-app
npm install
npm run dev

Entry files:

main.jsx → mounts React to the DOM
App.jsx → root component

---

2) Recommended Base Folder Structure

src/
  assets/
  components/
    Navbar.jsx
    Footer.jsx
    Loader.jsx
    index.js            // barrel exports
  pages/
    Home.jsx
    About.jsx
    Login.jsx
    Dashboard.jsx
    index.js            // barrel exports
  hooks/
    useAuth.js
    useFetch.js
    index.js
  store/
    index.js            // Redux store setup
    slices/
      authSlice.js
      uiSlice.js
  services/
    api.js              // axios/fetch wrappers
  layouts/
    MainLayout.jsx
  App.jsx
  main.jsx

---

3) Components Basics

Component = reusable UI function

File names in PascalCase

function Navbar() {
  return <div>Navbar</div>;
}
export default Navbar;

Use it:

import Navbar from "./components/Navbar";

function App() {
  return <Navbar />;
}

---

4) Barrel Files (index.js) for Clean Imports

src/components/index.js

export { default as Navbar } from "./Navbar";
export { default as Footer } from "./Footer";
export { default as Loader } from "./Loader";

Import:

import { Navbar, Footer } from "./components";

Why:

Cleaner imports
Easier refactor
Scales well

---

5) Props (Passing Data)

Parent → Child data flow.

function Card({ title }) {
  return <h2>{title}</h2>;
}

function App() {
  return <Card title="Hello" />;
}

---

6) State with useState

State = component memory

import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  );
}

Rule: Changing state ⇒ UI re-renders.

---

7) Side Effects with useEffect

Used for:

API calls
subscriptions
timers


import { useEffect } from "react";

useEffect(() => {
  // runs on mount
}, []);

Patterns:

[] → run once on mount
[dep] → run when dep changes

---

8) Other Important Hooks (You Should Know)

useRef
Store mutable value or access DOM


const inputRef = useRef();
<input ref={inputRef} />

useContext

Access global data without prop drilling


useMemo / useCallback

Performance optimization (avoid unnecessary recalculations / re-creations)


Custom Hooks

Reuse logic

function useFetch(url) { /* ... */ }


---

9) Routing (React Router)

Install:

npm install react-router-dom

App.jsx:

import { BrowserRouter, Routes, Route } from "react-router-dom";
import { Navbar } from "./components";
import { Home, About, Login, Dashboard } from "./pages";

function App() {
  return (
    <BrowserRouter>
      <Navbar />
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/login" element={<Login />} />
        <Route path="/dashboard" element={<Dashboard />} />
      </Routes>
    </BrowserRouter>
  );
}

export default App;

Notes:

Still a SPA

URL decides which component renders

---

10) Layout Pattern

layouts/MainLayout.jsx

import { Navbar, Footer } from "../components";
import { Outlet } from "react-router-dom";

export default function MainLayout() {
  return (
    <>
      <Navbar />
      <Outlet />
      <Footer />
    </>
  );
}

---

11) Redux Toolkit (Global State Management)

Install:

npm install @reduxjs/toolkit react-redux

Store Setup: src/store/index.js

import { configureStore } from "@reduxjs/toolkit";
import authReducer from "./slices/authSlice";

export const store = configureStore({
  reducer: {
    auth: authReducer,
  },
});

Slice Example: authSlice.js

import { createSlice } from "@reduxjs/toolkit";

const authSlice = createSlice({
  name: "auth",
  initialState: { user: null },
  reducers: {
    setUser: (state, action) => {
      state.user = action.payload;
    },
    logout: (state) => {
      state.user = null;
    },
  },
});

export const { setUser, logout } = authSlice.actions;
export default authSlice.reducer;

Provide Store in main.jsx

import { Provider } from "react-redux";
import { store } from "./store";

root.render(
  <Provider store={store}>
    <App />
  </Provider>
);

Use in Component

import { useSelector, useDispatch } from "react-redux";
import { setUser } from "../store/slices/authSlice";

const user = useSelector(state => state.auth.user);
const dispatch = useDispatch();

dispatch(setUser({ name: "Shivam" }));


---

12) API Layer (services/api.js)

export async function fetchUsers() {
  const res = await fetch("/api/users");
  return res.json();
}

Use in component with useEffect.


---

13) Import Rules

Default export:

export default Navbar;
import Navbar from "./Navbar";

Named export (barrel):

import { Navbar } from "./components";


---

14) Common Mistakes

❌ Forgetting to export component

❌ Wrong import path

❌ Using {} for default export

❌ Not wrapping app with BrowserRouter

❌ Mutating state directly in React (outside Redux Toolkit)



---

15) Mental Model (Must Remember)

UI = function(state, props)

App = tree of components

Hooks = give power to function components

Redux = global state

Router = switches pages

Barrel files = only for clean imports


---

16) When Project Grows

Use components/, pages/, hooks/, services/, store/

Add path aliases (@/components)

Split Redux into slices

Add lazy loading for routes

---

17) Pre-Project Checklist

[ ] Folder structure ready

[ ] Router configured

[ ] Store (Redux) configured (if needed)

[ ] Barrel files added

[ ] Layout decided

[ ] API layer created

---

18) One-Line Memory Cheats

useState = memory

useEffect = side effects

props = data from parent

Redux = global memory

Router = page switcher

Components = UI blocks

---