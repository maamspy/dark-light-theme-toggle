
# Theme Changing Dark/Light Toggle (React with Tailwind + Vite)

This example uses the following versions:

- React: **19.1.0**
- React Icons: **5.5.0**
- TailwindCSS: **4.1.8**
- Vite: **6.3.5**
- React Router: **7.6.2**

---

## 📁 File: `src/Contexts/ThemeProvider.jsx`

```jsx
import React, { createContext, useContext, useEffect, useState } from "react";

const ThemeContext = createContext();

export const ThemeProvider = ({ children }) => {
  const [isDark, setIsDark] = useState(false);
  const [isInitialized, setIsInitialized] = useState(false);

  useEffect(() => {
    const stored = localStorage.getItem("theme");
    const prefersDark = window.matchMedia("(prefers-color-scheme: dark)").matches;
    const dark = stored === "dark" || (!stored && prefersDark);
    setIsDark(dark);
    document.documentElement.classList.toggle("dark", dark);
    setIsInitialized(true);
  }, []);

  const toggleTheme = () => {
    const newTheme = !isDark;
    setIsDark(newTheme);
    document.documentElement.classList.toggle("dark", newTheme);
    localStorage.setItem("theme", newTheme ? "dark" : "light");
  };

  return (
    <ThemeContext.Provider value={{ isDark, toggleTheme, isInitialized }}>
      {children}
    </ThemeContext.Provider>
  );
};

export const useTheme = () => useContext(ThemeContext);
```

---

## 📁 File: `src/Components/Navbar.jsx`

```jsx
import { useTheme } from "../Contexts/ThemeProvider";
import { MdDarkMode, MdLightMode } from "react-icons/md";

const Navbar = () => {
  const { isDark, toggleTheme, isInitialized } = useTheme();

  return (
    <nav className="w-full bg-white dark:bg-gray-900 shadow px-6 py-4 flex items-center justify-between">
      <button
        onClick={toggleTheme}
        className="text-4xl text-gray-800 dark:text-white"
        aria-label="Toggle theme"
        disabled={!isInitialized}
      >
        {isInitialized ? (
          isDark ? <MdDarkMode /> : <MdLightMode />
        ) : (
          <span style={{ width: "1.5em", height: "1.5em", display: "inline-block" }} />
        )}
      </button>
    </nav>
  );
};

export default Navbar;
```

---

## 📁 File: `src/Pages/HomePage.jsx`

```jsx
import React from "react";
import Navbar from "../Components/Navbar";

const HomePage = () => {
  return (
    <div>
      <Navbar />
      <div className="min-h-screen flex flex-col items-center justify-center p-4">
        <div className="bg-white dark:bg-gray-800 rounded-lg px-6 py-8 ring shadow-xl ring-gray-900/5">
          <h3 className="text-gray-900 dark:text-white mt-5 text-base font-medium tracking-tight">
            Writes upside-down
          </h3>
          <p className="text-gray-500 dark:text-gray-400 mt-2 text-sm">
            The Zero Gravity Pen can be used to write in any orientation,
            including upside-down.
          </p>
        </div>
      </div>
    </div>
  );
};

export default HomePage;
```

