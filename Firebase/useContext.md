# 1. Firebase Setup and Installation
## First, install Firebase:

```bash
npm install firebase
```
Then, create a Firebase configuration file:

src/firebase-config.js
```javascript
// src/firebase-config.js
import { initializeApp } from "firebase/app";

// Firebase configuration from Firebase Console
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT_ID.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID",
  measurementId: "YOUR_MEASUREMENT_ID",
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);

export default app;
```
# 2. Create AuthContext for Global Authentication State
## We'll use useContext and useState to share authentication state across the app.

src/contexts/AuthContext.js
```javascript
// src/contexts/AuthContext.js
import React, { createContext, useState, useEffect } from "react";
import { getAuth, onAuthStateChanged } from "firebase/auth";
import app from "../firebase-config";

// Initialize Firebase Authentication
const auth = getAuth(app);

// Create a context
export const AuthContext = createContext();

// Create a provider component
export const AuthProvider = ({ children }) => {
  const [user, setUser] = useState(null);

  // Monitor authentication state
  useEffect(() => {
    const unsubscribe = onAuthStateChanged(auth, (user) => {
      setUser(user);
    });

    return () => unsubscribe(); // Cleanup listener
  }, []);

  return (
    <AuthContext.Provider value={{ user }}>
      {children}
    </AuthContext.Provider>
  );
};
```
# 3. Firebase Authentication Logic (auth.js)
## This file contains the functions for user registration, login, and logout.

src/auth.js
```javascript
// src/auth.js
import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, signOut } from "firebase/auth";
import app from "./firebase-config";

const auth = getAuth(app);

// Register user
export const registerUser = async (email, password) => {
  return createUserWithEmailAndPassword(auth, email, password);
};

// Login user
export const loginUser = async (email, password) => {
  return signInWithEmailAndPassword(auth, email, password);
};

// Logout user
export const logoutUser = async () => {
  return signOut(auth);
};
```
# 4. App Structure with AuthContext and Components
src/index.js
## Wrap your entire app with the AuthProvider to provide the authentication state globally.

```javascript
// src/index.js
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import { AuthProvider } from "./contexts/AuthContext"; // Import AuthProvider

ReactDOM.render(
  <AuthProvider>
    <App />
  </AuthProvider>,
  document.getElementById("root")
);
```
src/App.js
## Use the AuthContext to show different views based on whether the user is authenticated.

```javascript
// src/App.js
import React, { useContext } from "react";
import { AuthContext } from "./contexts/AuthContext";
import LoginPage from "./components/LoginPage";
import ProtectedPage from "./components/ProtectedPage";

const App = () => {
  const { user } = useContext(AuthContext);

  return (
    <div>
      {!user ? (
        <LoginPage /> // Show LoginPage if not authenticated
      ) : (
        <ProtectedPage /> // Show ProtectedPage if authenticated
      )}
    </div>
  );
};

export default App;
```
# 5. LoginPage Component
## This component will handle both login and registration functionality.

src/components/LoginPage.js
```javascript
// src/components/LoginPage.js
import React, { useState, useContext } from "react";
import { loginUser, registerUser } from "../auth";
import { AuthContext } from "../contexts/AuthContext";

const LoginPage = () => {
  const { user } = useContext(AuthContext); // Consume the user state from AuthContext
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [error, setError] = useState("");

  const handleLogin = async () => {
    try {
      await loginUser(email, password);
      // Redirect or update UI after successful login
    } catch (err) {
      setError(err.message);
    }
  };

  const handleRegister = async () => {
    try {
      await registerUser(email, password);
      // Redirect or update UI after successful registration
    } catch (err) {
      setError(err.message);
    }
  };

  if (user) {
    return <p>Welcome, {user.email}!</p>; // Display user info if logged in
  }

  return (
    <div>
      <h2>Login</h2>
      <input
        type="email"
        placeholder="Email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      <input
        type="password"
        placeholder="Password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      />
      <button onClick={handleLogin}>Login</button>
      <button onClick={handleRegister}>Register</button>
      {error && <p>{error}</p>}
    </div>
  );
};

export default LoginPage;
```
# 6. ProtectedPage Component
## This page will be shown only if the user is authenticated.

src/components/ProtectedPage.js
```javascript
// src/components/ProtectedPage.js
import React, { useContext } from "react";
import { AuthContext } from "../contexts/AuthContext";

const ProtectedPage = () => {
  const { user } = useContext(AuthContext); // Get user from context

  if (!user) {
    return <p>You must be logged in to view this page.</p>;
  }

  return (
    <div>
      <h2>Protected Content</h2>
      <p>This is a protected page only visible to logged-in users!</p>
    </div>
  );
};

export default ProtectedPage;
```
# 7. Logout Button
## This button will allow the user to log out.

src/components/LogoutButton.js
```javascript
// src/components/LogoutButton.js
import React from "react";
import { logoutUser } from "../auth";

const LogoutButton = () => {
  const handleLogout = async () => {
    try {
      await logoutUser();
      // Redirect or update UI after successful logout
    } catch (err) {
      console.error("Logout failed:", err.message);
    }
  };

  return <button onClick={handleLogout}>Logout</button>;
};

export default LogoutButton;
```
# 8. File Directory Structure
## Here's the directory structure for the app:

```lua
src/
|-- auth.js                  # Firebase Authentication logic
|-- contexts/
|   |-- AuthContext.js        # Authentication context provider
|-- components/
|   |-- LoginPage.js          # Login and Registration components
|   |-- ProtectedPage.js      # Protected page for logged-in users
|   |-- LogoutButton.js       # Button to logout user
|-- firebase-config.js       # Firebase config file
|-- App.js                   # Main application file
|-- index.js                 # Entry point for the React app
```
# Conclusion
## This solution integrates Firebase Authentication with React's useContext API to share the authentication state globally across components. By wrapping your app in the AuthProvider, you can easily check whether a user is logged in or not and conditionally render the appropriate components.
