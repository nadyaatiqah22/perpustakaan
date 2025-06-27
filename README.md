<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Login/Register</title>
  <head>
  <meta charset="UTF-8" />
  <title>Login/Register</title>
  <link rel="stylesheet" href="style.css" />
</head>

</head>

<body>
  <h2>Form Login / Register</h2>
  <form>
    <input type="text" id="name" placeholder="Nama (untuk Register)" /><br/>
    <input type="email" id="email" placeholder="Email" required /><br/>
    <input type="password" id="password" placeholder="Password" required /><br/>
    <button type="button" onclick="register()">Register</button>
    <button type="button" onclick="login()">Login</button>
  </form>

  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.8.1/firebase-app.js";
    import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.8.1/firebase-auth.js";
    import { getFirestore, doc, setDoc } from "https://www.gstatic.com/firebasejs/11.8.1/firebase-firestore.js";

    // Firebase config
    const firebaseConfig = {
      apiKey: "AIzaSyAHD4kQzHQZuHOzQNS76ef2XGgZGzwhzhg",
      authDomain: "ecommerce-web-35f10.firebaseapp.com",
      projectId: "ecommerce-web-35f10",
      storageBucket: "ecommerce-web-35f10.appspot.com",
      messagingSenderId: "618504801619",
      appId: "1:618504801619:web:2934145fc56353f9e6a5a6",
      measurementId: "G-2MV1SM5RCH"
    };

    // Inisialisasi Firebase
    const app = initializeApp(firebaseConfig);
    const auth = getAuth(app);
    const db = getFirestore(app);

    // Cek status login
    onAuthStateChanged(auth, (user) => {
      if (user) {
        // Kalau sudah login, langsung ke dashboard
        window.location.href = "dashboard.html";
      } else {
        // Kosongkan input setiap kali halaman dibuka
        document.getElementById("name").value = "";
        document.getElementById("email").value = "";
        document.getElementById("password").value = "";
      }
    });

    // Fungsi register
    window.register = async function () {
      const name = document.getElementById("name").value;
      const email = document.getElementById("email").value;
      const password = document.getElementById("password").value;

      try {
        const userCredential = await createUserWithEmailAndPassword(auth, email, password);
        const user = userCredential.user;
        await setDoc(doc(db, "users", user.uid), {
          name,
          email,
          role: "customer"
        });
        alert("Berhasil daftar!");
      } catch (error) {
        alert("Error: " + error.message);
      }
    };

    // Fungsi login
    window.login = async function () {
      const email = document.getElementById("email").value;
      const password = document.getElementById("password").value;

      try {
        const userCredential = await signInWithEmailAndPassword(auth, email, password);
        alert("Login berhasil!");
        window.location.href = "dashboard.html";
      } catch (error) {
        alert("Login gagal: " + error.message);
      }
    };
  </script>
</body>
</html>
