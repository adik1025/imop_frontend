---
layout: page 
title: Login
permalink: /login
search_exclude: true
show_reading_time: false 
---

<div class="max-w-5xl mx-auto px-6 py-20">
  <div class="grid md:grid-cols-2 gap-10">

    <!-- Login Card -->
    <div class="bg-neutralCard border border-white/10 p-8 rounded-2xl shadow-md">
      <h2 class="text-2xl font-bold text-white mb-6">User Login</h2>
      <form id="pythonForm" onsubmit="pythonLogin(); return false" class="space-y-4">
        <div>
          <label class="block text-sm text-gray-400 mb-1">GitHub ID</label>
          <input type="text" id="uid" required class="w-full p-3 rounded-lg bg-neutral-800 text-white focus:outline-none focus:ring-2 focus:ring-accent" />
        </div>
        <div>
          <label class="block text-sm text-gray-400 mb-1">Password</label>
          <input type="password" id="password" required class="w-full p-3 rounded-lg bg-neutral-800 text-white focus:outline-none focus:ring-2 focus:ring-accent" />
        </div>
        <p id="message" class="text-red-400 text-sm"></p>
        <button type="submit" class="w-full py-3 rounded-lg bg-accent text-white hover:bg-white hover:text-accent transition font-semibold">
          Login
        </button>
      </form>
    </div>

    <!-- Signup Card -->
    <div class="bg-neutralCard border border-white/10 p-8 rounded-2xl shadow-md">
      <h2 class="text-2xl font-bold text-white mb-6">Sign Up</h2>
      <form id="signupForm" onsubmit="signup(); return false" class="space-y-4">
        <div>
          <label class="block text-sm text-gray-400 mb-1">Name</label>
          <input type="text" id="name" required class="w-full p-3 rounded-lg bg-neutral-800 text-white focus:outline-none focus:ring-2 focus:ring-accent" />
        </div>
        <div>
          <label class="block text-sm text-gray-400 mb-1">GitHub ID</label>
          <input type="text" id="signupUid" required class="w-full p-3 rounded-lg bg-neutral-800 text-white focus:outline-none focus:ring-2 focus:ring-accent" />
        </div>
        <div>
          <label class="block text-sm text-gray-400 mb-1">Password</label>
          <input type="password" id="signupPassword" required class="w-full p-3 rounded-lg bg-neutral-800 text-white focus:outline-none focus:ring-2 focus:ring-accent" />
        </div>
        <p id="signupMessage" class="text-green-400 text-sm"></p>
        <button type="submit" class="w-full py-3 rounded-lg bg-accent text-white hover:bg-white hover:text-accent transition font-semibold">
          Sign Up
        </button>
      </form>
    </div>

  </div>
</div>

<script type="module">
import { login, pythonURI, fetchOptions } from '{{site.baseurl}}/assets/js/api/config.js';

window.pythonLogin = function () {
  const options = {
    URL: `${pythonURI}/api/authenticate`,
    callback: pythonDatabase,
    message: "message",
    method: "POST",
    cache: "no-cache",
    body: {
      uid: document.getElementById("uid").value,
      password: document.getElementById("password").value,
    },
  };
  login(options);
};

window.signup = function () {
  const signupButton = document.querySelector("#signupForm button");
  signupButton.disabled = true;
  signupButton.classList.add("opacity-50");

  const signupOptions = {
    URL: `${pythonURI}/api/user`,
    method: "POST",
    cache: "no-cache",
    body: {
      name: document.getElementById("name").value,
      uid: document.getElementById("signupUid").value,
      password: document.getElementById("signupPassword").value,
    },
  };

  fetch(signupOptions.URL, {
    method: signupOptions.method,
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(signupOptions.body),
  })
    .then((response) => {
      if (!response.ok) throw new Error(`Signup failed: ${response.status}`);
      return response.json();
    })
    .then(() => {
      document.getElementById("signupMessage").textContent = "Signup successful!";
    })
    .catch((error) => {
      document.getElementById("signupMessage").textContent = `Signup Error: ${error.message}`;
      signupButton.disabled = false;
      signupButton.classList.remove("opacity-50");
    });
};

function pythonDatabase() {
  const URL = `${pythonURI}/api/id`;

  fetch(URL, fetchOptions)
    .then((response) => {
      if (!response.ok) throw new Error(`Flask server response: ${response.status}`);
      return response.json();
    })
    .then(() => {
      window.location.href = '{{site.baseurl}}/profile';
    })
    .catch((error) => {
      document.getElementById("message").textContent = `Login Error: ${error.message}`;
    });
}

window.onload = function () {
  pythonDatabase();
};
</script>