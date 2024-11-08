Firebase Setup & Initialization
javascript
Copy code
// Initialize Firebase
import { initializeApp } from "firebase/app";

// Firebase configuration
const firebaseConfig = {
  apiKey: "your-api-key",
  authDomain: "your-auth-domain",
  projectId: "your-project-id",
  storageBucket: "your-storage-bucket",
  messagingSenderId: "your-sender-id",
  appId: "your-app-id",
  measurementId: "your-measurement-id"
};

// Initialize Firebase App
const app = initializeApp(firebaseConfig);
initializeApp initializes the Firebase app with your project’s configuration object.
The firebaseConfig object contains your Firebase project's credentials, like apiKey, authDomain, etc.
Firebase Authentication
Sign Up User
javascript
Copy code
import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";

// Get Firebase Authentication instance
const auth = getAuth();

// Sign up with email and password
const signUp = async (email, password) => {
  try {
    const userCredential = await createUserWithEmailAndPassword(auth, email, password);
    // User signed up successfully
    console.log("User Signed Up:", userCredential.user);
  } catch (error) {
    // Handle errors during sign-up
    console.error("Error signing up:", error.message);
  }
};
getAuth() gets the instance of Firebase Authentication for managing users.
createUserWithEmailAndPassword(auth, email, password) creates a new user with the given email and password.
Sign In User
javascript
Copy code
import { getAuth, signInWithEmailAndPassword } from "firebase/auth";

// Sign in with email and password
const signIn = async (email, password) => {
  try {
    const userCredential = await signInWithEmailAndPassword(auth, email, password);
    // User signed in successfully
    console.log("User Signed In:", userCredential.user);
  } catch (error) {
    // Handle sign-in errors
    console.error("Error signing in:", error.message);
  }
};
signInWithEmailAndPassword(auth, email, password) signs in a user with the provided credentials.
Sign Out User
javascript
Copy code
import { getAuth, signOut } from "firebase/auth";

// Sign out the current user
const logOut = async () => {
  try {
    await signOut(auth);
    console.log("User signed out successfully");
  } catch (error) {
    // Handle sign-out errors
    console.error("Error signing out:", error.message);
  }
};
signOut(auth) signs out the currently authenticated user.
Firebase Firestore (Real-time Database)
Add Document to Firestore
javascript
Copy code
import { getFirestore, collection, addDoc } from "firebase/firestore";

// Get Firestore database instance
const db = getFirestore();

// Add document to collection
const addDocument = async (data) => {
  try {
    const docRef = await addDoc(collection(db, "users"), data);
    console.log("Document written with ID:", docRef.id);
  } catch (error) {
    // Handle errors while adding the document
    console.error("Error adding document:", error);
  }
};
getFirestore() gets an instance of Firestore.
collection(db, "users") refers to the "users" collection in the Firestore database.
addDoc(collection, data) adds a new document with the provided data.
Get Document from Firestore
javascript
Copy code
import { getDoc, doc } from "firebase/firestore";

// Get document by ID
const getDocument = async (docId) => {
  try {
    const docRef = doc(db, "users", docId);  // Reference to document
    const docSnap = await getDoc(docRef);   // Get document snapshot

    if (docSnap.exists()) {
      console.log("Document data:", docSnap.data());
    } else {
      console.log("No such document!");
    }
  } catch (error) {
    // Handle errors during document retrieval
    console.error("Error getting document:", error);
  }
};
doc(db, "users", docId) gets a reference to a specific document in the Firestore.
getDoc(docRef) retrieves the document snapshot, which you can check to see if it exists.
Update Document in Firestore
javascript
Copy code
import { updateDoc, doc } from "firebase/firestore";

// Update document fields
const updateDocument = async (docId, updatedData) => {
  try {
    const docRef = doc(db, "users", docId);  // Reference to the document
    await updateDoc(docRef, updatedData);   // Update the document with new data
    console.log("Document updated successfully");
  } catch (error) {
    // Handle errors during document update
    console.error("Error updating document:", error);
  }
};
updateDoc(docRef, updatedData) updates a document with new data.
Delete Document from Firestore
javascript
Copy code
import { deleteDoc, doc } from "firebase/firestore";

// Delete document
const deleteDocument = async (docId) => {
  try {
    const docRef = doc(db, "users", docId);  // Reference to the document
    await deleteDoc(docRef);                 // Delete the document
    console.log("Document deleted successfully");
  } catch (error) {
    // Handle errors during document deletion
    console.error("Error deleting document:", error);
  }
};
deleteDoc(docRef) deletes a document from Firestore.
Firebase Storage (File Upload & Download)
Upload File to Firebase Storage
javascript
Copy code
import { getStorage, ref, uploadBytes } from "firebase/storage";

// Get Storage instance
const storage = getStorage();

// Upload file to Firebase Storage
const uploadFile = async (file) => {
  try {
    const storageRef = ref(storage, "uploads/" + file.name);  // Reference to the storage location
    await uploadBytes(storageRef, file);   // Upload the file to Firebase Storage
    console.log("File uploaded successfully");
  } catch (error) {
    // Handle errors during file upload
    console.error("Error uploading file:", error);
  }
};
ref(storage, "uploads/") creates a reference to the storage location where the file will be uploaded.
uploadBytes(storageRef, file) uploads the file to the specified reference in Firebase Storage.
Download File from Firebase Storage
javascript
Copy code
import { getDownloadURL, ref } from "firebase/storage";

// Get file URL
const getFileUrl = async (fileName) => {
  try {
    const fileRef = ref(storage, "uploads/" + fileName);  // Reference to the file in storage
    const url = await getDownloadURL(fileRef);           // Get the download URL
    console.log("File URL:", url);
  } catch (error) {
    // Handle errors during file retrieval
    console.error("Error getting file URL:", error);
  }
};
getDownloadURL(fileRef) retrieves the file's download URL from Firebase Storage.
Firebase Cloud Functions
Firebase Cloud Function to Handle HTTP Request
javascript
Copy code
const functions = require("firebase-functions");

exports.helloWorld = functions.https.onRequest((req, res) => {
  res.send("Hello from Firebase!");
});
functions.https.onRequest sets up an HTTP-triggered Cloud Function.
res.send() sends a response to the client.
Firebase Realtime Database (Alternative to Firestore)
Write Data to Realtime Database
javascript
Copy code
import { getDatabase, ref, set } from "firebase/database";

// Get Realtime Database instance
const db = getDatabase();

// Write data to the database
const writeUserData = async (userId, name, email) => {
  try {
    await set(ref(db, "users/" + userId), {
      username: name,
      email: email
    });
    console.log("User data written to Realtime Database");
  } catch (error) {
    console.error("Error writing data:", error);
  }
};
set(ref(db, "users/" + userId), data) writes data to a specific location in the Realtime Database.
Firebase Firestore Security Rules (Sample)
json
Copy code
{
  "rules": {
    "users": {
      "$userId": {
        ".read": "$userId === auth.uid",  // Only allow users to read their own data
        ".write": "$userId === auth.uid"  // Only allow users to write their own data
      }
    }
  }
}
This security rule ensures that users can only read and write their own data in the Firestore "users" collection, using auth.uid to identify the authenticated user.
Full README Example
md
Copy code
# Firebase Common Code Snippets

This document provides common Firebase code snippets and explanations for various operations like authentication, Firestore, Firebase Storage, and Cloud Functions. Each snippet is explained with detailed comments on what each line of code does.

## Firebase Setup

To initialize Firebase in your app, you need to configure your Firebase project and initialize it using the following code:

```javascript
import { initializeApp } from "firebase/app";

const firebaseConfig = {
  apiKey: "your-api-key",
  authDomain: "your-auth-domain",
  projectId: "your-project-id",
  storageBucket: "your-storage-bucket",
  messagingSenderId: "your-sender-id",
  appId: "your-app-id",
  measurementId: "your-measurement-id"
};

const app = initializeApp(firebaseConfig);
Firebase Authentication
Sign Up User
javascript
Copy code
import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";

const auth = getAuth();

const signUp = async (email, password) => {
  try {
    const userCredential = await createUserWithEmailAndPassword(auth, email, password);
    console.log("User Signed Up:", userCredential.user);
  } catch (error) {
    console.error("Error signing up:", error.message);
  }
};
Sign In User
javascript
Copy code
import { getAuth, signInWithEmailAndPassword } from "firebase/auth";

const signIn = async (email, password) => {
  try {
    const userCredential = await signInWithEmailAndPassword(auth, email, password);
    console.log("User Signed In:", userCredential.user);
  } catch (error) {
    console.error("Error signing in:", error.message);
  }
};
Sign Out User
javascript
Copy code
import { getAuth, signOut } from "firebase/auth";

const logOut = async () => {
  try {
    await signOut(auth);
    console.log("User signed out successfully");
  } catch (error) {
    console.error("Error signing out:", error.message);
  }
};
Firebase Firestore
Add Document
javascript
Copy code
import { getFirestore, collection, addDoc } from "firebase/firestore";

const db = getFirestore();

const addDocument = async (data) => {
  try {
    const docRef = await addDoc(collection(db, "users"), data);
    console.log("Document written with ID:", docRef.id);
  } catch (error) {
    console.error("Error adding document:", error);
  }
};
Firebase Storage
Upload File
javascript
Copy code
import { getStorage, ref, uploadBytes } from "firebase/storage";

const storage = getStorage();

const uploadFile = async (file) => {
  try {
    const storageRef = ref(storage, "uploads/" + file.name);
    await uploadBytes(storageRef, file);
    console.log("File uploaded successfully");
  } catch (error) {
    console.error("Error uploading file:", error);
  }
};
Firebase Cloud Functions
HTTP Function Example
javascript
Copy code
const functions = require("firebase-functions");

exports.helloWorld = functions.https.onRequest((req, res) => {
  res.send("Hello from Firebase!");
});
Firebase Realtime Database
Write Data Example
javascript
Copy code
import { getDatabase, ref, set } from "firebase/database";

const db = getDatabase();

const writeUserData = async (userId, name, email) => {
  try {
    await set(ref(db, "users/" + userId), {
      username: name,
      email: email
    });
    console.log("User data written to Realtime Database");
  } catch (error) {
    console.error("Error writing data:", error);
  }
};
Firebase Firestore Security Rules
json
Copy code
{
  "rules": {
    "users": {
      "$userId": {
        ".read": "$userId === auth.uid",
        ".write": "$userId === auth.uid"
      }
    }
  }
}
