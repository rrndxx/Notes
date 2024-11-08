# 1. Firebase Authentication
### Firebase Authentication allows you to handle user authentication using various providers, such as email/password, Google, and more.

## a. Register a New User (Email/Password)
### Use this function to create a new user account with email and password.

```javascript
// Register a new user with email and password
import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";
import app from './firebase-config';

const auth = getAuth(app);

const registerUser = (email, password) => {
  createUserWithEmailAndPassword(auth, email, password)
    .then((userCredential) => {
      const user = userCredential.user;
      console.log("User registered:", user);
    })
    .catch((error) => {
      const errorCode = error.code;
      const errorMessage = error.message;
      console.error("Error registering user:", errorCode, errorMessage);
    });
};
```

## b. Sign In Existing User (Email/Password)
### Use this function to sign in an existing user with email and password.

```javascript
// Sign in with email and password
import { getAuth, signInWithEmailAndPassword } from "firebase/auth";
import app from './firebase-config';

const auth = getAuth(app);

const signInUser = (email, password) => {
  signInWithEmailAndPassword(auth, email, password)
    .then((userCredential) => {
      const user = userCredential.user;
      console.log("User signed in:", user);
    })
    .catch((error) => {
      const errorCode = error.code;
      const errorMessage = error.message;
      console.error("Error signing in user:", errorCode, errorMessage);
    });
};
```

## c. Sign Out User
### Use this function to sign out the currently authenticated user.

```javascript
// Sign out the current user
import { getAuth, signOut } from "firebase/auth";
import app from './firebase-config';

const auth = getAuth(app);

const signOutUser = () => {
  signOut(auth)
    .then(() => {
      console.log("User signed out successfully");
    })
    .catch((error) => {
      console.error("Error signing out:", error);
    });
};
```
---

# 2.  Firebase Firestore (NoSQL Database)
### Firestore is Firebase's flexible, scalable NoSQL cloud database for storing and syncing data.

## a. Add Data to Firestore
### This function adds a new document to a specified Firestore collection.

```javascript
// Add data to Firestore
import { getFirestore, collection, addDoc } from "firebase/firestore";
import app from './firebase-config';

const db = getFirestore(app);

const addData = async (collectionName, data) => {
  try {
    const docRef = await addDoc(collection(db, collectionName), data);
    console.log("Document written with ID:", docRef.id);
  } catch (e) {
    console.error("Error adding document:", e);
  }
};
```

## b.  Retrieve Data from Firestore
### This function retrieves all documents from a Firestore collection.

```javascript
// Retrieve data from Firestore
import { getFirestore, collection, getDocs } from "firebase/firestore";
import app from './firebase-config';

const db = getFirestore(app);

const getData = async (collectionName) => {
  const querySnapshot = await getDocs(collection(db, collectionName));
  querySnapshot.forEach((doc) => {
    console.log(doc.id, " => ", doc.data());
  });
};
```

## c. Query Data from Firestore
### This function demonstrates querying data based on a condition (e.g., users above a certain age).

```javascript
// Query data from Firestore
import { getFirestore, collection, query, where, getDocs } from "firebase/firestore";
import app from './firebase-config';

const db = getFirestore(app);

const queryData = async () => {
  const q = query(collection(db, "users"), where("age", ">", 18));
  const querySnapshot = await getDocs(q);
  querySnapshot.forEach((doc) => {
    console.log(doc.id, " => ", doc.data());
  });
};
```
---

# 3. Firebase Realtime Database
### Firebase Realtime Database allows you to store and sync data in real-time.

## a. Write Data to Realtime Database
### This function writes data to a specific path in the Realtime Database.

```javascript
// Write data to Realtime Database
import { getDatabase, ref, set } from "firebase/database";
import app from './firebase-config';

const db = getDatabase(app);

const writeData = (path, data) => {
  const reference = ref(db, path);
  set(reference, data)
    .then(() => {
      console.log("Data written to Realtime Database");
    })
    .catch((error) => {
      console.error("Error writing data:", error);
    });
};
```

## b. Read Data from Realtime Database
### This function reads data from a specific path in the Realtime Database.

```javascript
// Read data from Realtime Database
import { getDatabase, ref, get } from "firebase/database";
import app from './firebase-config';

const db = getDatabase(app);

const readData = async (path) => {
  const reference = ref(db, path);
  try {
    const snapshot = await get(reference);
    if (snapshot.exists()) {
      console.log("Data: ", snapshot.val());
    } else {
      console.log("No data available");
    }
  } catch (error) {
    console.error("Error reading data:", error);
  }
};
```
---

# 4. Firebase Storage (File Uploads)
### Firebase Storage allows you to upload and store files such as images, videos, and other media.

## a. Upload a File to Firebase Storage
### This function uploads a file to Firebase Storage.

```javascript
// Upload file to Firebase Storage
import { getStorage, ref, uploadBytes } from "firebase/storage";
import app from './firebase-config';

const storage = getStorage(app);

const uploadFile = (file, filePath) => {
  const storageRef = ref(storage, filePath);
  uploadBytes(storageRef, file)
    .then((snapshot) => {
      console.log("File uploaded successfully:", snapshot);
    })
    .catch((error) => {
      console.error("Error uploading file:", error);
    });
};
```

## b. Download a File from Firebase Storage
### This function retrieves a file's URL from Firebase Storage.

```javascript
// Download file URL from Firebase Storage
import { getStorage, ref, getDownloadURL } from "firebase/storage";
import app from './firebase-config';

const storage = getStorage(app);

const downloadFile = async (filePath) => {
  const storageRef = ref(storage, filePath);
  try {
    const url = await getDownloadURL(storageRef);
    console.log("File URL:", url);
  } catch (error) {
    console.error("Error downloading file:", error);
  }
};
```
---

# Conclusion

These are some of the most common Firebase code snippets and logic for Firebase services such as Authentication, Firestore, Realtime Database, and Firebase Storage. The code is structured in a way to be easy to integrate into your JavaScript projects. Be sure to use Firebase SDK's latest version and follow security rules to protect sensitive information, such as API keys, during development.

Keep this document as a reference whenever you need to integrate Firebase services into your project!



















