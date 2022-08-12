---
title: "Firebase - How to use Cloud Firestore in our application"
description: ""
date: 2022-07-20
draft: false
categories: 
- Firebase
tags:
- Firestore
cover:
    image: "cover.jpg"
    relative: true
---


### What is the Cloud Firestore

Cloud Firestore Database of Google Cloud, provide to mobile, Web, and sever development. Through realtime listeners and offers offline support so we can build responsive apps without network latency or Internet connectivity. 

### step1: create project and application

Create a project and add application as needed then register applications.

### step2: add the Firebase SDK to our project in development.

First, we install the latest version of Firebase. 

```
npm install firebase
```

Next, initialize Firebase and add the SDK that we obtained after registration to our project under development.

```
// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";

// Your web app's Firebase configuration
// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const firebaseConfig = {
	//...
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);
```

### step3: added Cloud Firestore features

Add Cloud Firestore functionality and create a Database.
 Cloud Firestore provides production and test modes to start our Database：

- **test mode：** A suit suitable for mobile and web client libraries. but  in this mode, anyone can read and overwrite the data. After testing, remember to save the data add protection.
- **locked mode：** It is suitable for server client library with C#, Go, Java, Node.js, PHP, Python, or Ruby. Reject all reads and writes for mobile and web clients.

After selecting a mode, specify where to store Could Firebase data and enable the database.

### step4: initialization Cloud **Firestore**

Continue where we initialized firebase, we also initialize Cloud Firestroe.

```
import { initializeApp } from 'firebase/app';
import { getFirestore } from 'firebase/firestore';

const firebaseConfig = {
  //...
};

const app = initializeApp(firebaseConfig);

// Initialize Cloud Firestore and get a reference to the service
const db = getFirestore(app);
```

### step5: add the data we want to store

In Could Firestore, our data will be stored in Documents in json format, a data corresponds to a Documents, and these Documents will be stored in a Collections.
We can add Data through `setDoc()` or `addDoc()` method.

- **`addDoc()`**: Using `addDoc()` method Firestore will automatically create an ID for each Documents.

```
import { collection, addDoc } from "firebase/firestore"; 

// Add a new document with a generated id.
const docRef = await addDoc(collection(db, "user"), {
  name: "Naomi",
  country: "Japan"
});
console.log("Document written with ID: ", docRef.id);
```
    


- **`setDoc()`**: Using the `setDoc()` method we need to specify an ID for each Documents. `setDoc()` is more like updating the current Documents, comparing whether the  current data exists through the Document ID. if it exists, it overwrites the data of the Document. If is doesn’t exist, it adds the Document and data.
      
```
import { doc, setDoc } from "firebase/firestore"; 

// Add a new document in collection "user"
// Specifies the ID of the document to create
await setDoc(doc(db, "user", "user-id"), data);
```
  

In addition, if we only want to update the data of some fields in existing Data, we can call **`updateDoc()`** like this :

```
import { doc, updateDoc } from "firebase/firestore";

const washingtonRef = doc(db, "user", "user-id");

// Set the "country" field of the user 'America'
await updateDoc(washingtonRef, {
  country: "America"
});
```

### step7: read data stored in firebase

We can call `getDoc()` or `onSnapshot()` to get data in Cloud Firestore, if we just want to get data once, we can call `getDoc()` to get the data, or if we want to receive data changes in  realtime, we can set a listener use `onSnapshot()`.

- **`getDoc()`**: This method is for us to take the initiative to get data from Firestore, unless we call it again to get the new data, we will not know any changes to the date on the Firestore.
    
**get a document**
  
```
import { doc, getDoc } from "firebase/firestore";

const docSnap = await getDoc(doc(db, "user", "user-id"););

if (docSnap.exists()) {
  console.log("Document data:", docSnap.data());
} else {
  // doc.data() will be undefined in this case
  console.log("No such document!");
}
```
      
**get multiple document from a collection**
  
```
import { collection, query, where, getDocs } from "firebase/firestore";

const querySnapshot = await getDocs(collection(db, "user"));
querySnapshot.forEach((doc) => {
  // doc.data() is never undefined for query doc snapshots
  console.log(doc.id, " => ", doc.data());
});
```
        
If we just want to query the documents that  meet a certain condition we can use `where()` like this: 
  
```
import { collection, query, where, getDocs } from "firebase/firestore";

const q = query(collection(db, "user"), where("field", "==", "content"));

const querySnapshot = await getDocs(q);
querySnapshot.forEach((doc) => {
  // doc.data() is never undefined for query doc snapshots
  console.log(doc.id, " => ", doc.data());
});
```
        

- **`onSnapshot()`**: We can receive data changes on Firestore in realtime  through this method. When an initial call using the callback will create a document snapshot immediately with the current data of the single document.Then each time the data changes, another call updates the snapshot.

**Listen to a document**


```
import { doc, onSnapshot } from "firebase/firestore";

const unsub = onSnapshot(doc(db, "user", "user-id"), (doc) => {
    console.log("Current data: ", doc.data());
});
```
**Listen to multiple documents in a collection**
        
```
import { collection, query, where, onSnapshot } from "firebase/firestore";

const query = query(collection(db, "user"));
const unsubscribe = onSnapshot(query, (querySnapshot) => {
  const cities = [];
  querySnapshot.forEach((doc) => {
      cities.push(doc.data().name);
  });
  console.log("Current data: ", cities.join(", "));
});
```
        
Similarly, if we just want to query the documents that  meet a certain condition we can use `where()` like this: 

```
import { collection, query, where, onSnapshot } from "firebase/firestore";

const query = query(collection(db, "user"),where("field", "==", "content"));
const unsubscribe = onSnapshot(q, (querySnapshot) => {);
const unsubscribe = onSnapshot(query, (querySnapshot) => {
  const cities = [];
  querySnapshot.forEach((doc) => {
      cities.push(doc.data().name);
  });
  console.log("Current field in content: ", cities.join(", "));
});
```

**Detach a listener**


```
import { collection, onSnapshot } from "firebase/firestore";

const unsubscribe = onSnapshot(collection(db, "user"), () => {
  // Respond to data
  // ...
});

// Later ...

// Stop listening to changes
unsubscribe();
```

Finally, remember detach the listener through unsubscribe when we no longer need to listen our data.
    

### Conclusion

Thanks for reading, If you have suggestions or want to discuss, you can contact me!