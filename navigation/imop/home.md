---
layout: post
title: IMOP Home
search_exclude: true
permalink: /imop/
menu: nav/imop.html
---

<style>
    body {
        font-family: Arial, sans-serif;
        margin: 0;
        padding: 0;
        background-color: #ddd;
    }

    .container {
        display: flex;
        height: 100vh;
    }

    .sidebar {
        width: 200px;
        background-color: #666;
        padding: 20px;
        display: flex;
        flex-direction: column;
        gap: 10px;
    }

    .sidebar button {
        background-color: #444;
        color: white;
        border: none;
        padding: 10px;
        text-align: left;
        width: 100%;
        cursor: pointer;
        border-radius: 5px;
    }

    .sidebar button:hover {
        background-color: #555;
    }

    .main {
        flex: 1;
        background-color: #aaa;
        padding: 40px;
        display: flex;
        align-items: center;
    }

    .profile-container {
        display: flex;
        align-items: center;
        gap: 20px;
    }

    .profile-container img {
        width: 100px;
        height: 100px;
        border-radius: 50%;
        border: 3px solid white;
    }

    .profile-container h1 {
        font-size: 24px;
        color: white;
        font-weight: bold;
    }

    .header {
        background-color: #0a5404;
        color: white;
        padding: 10px 20px;
        display: flex;
        align-items: center;
        font-size: 20px;
        font-weight: bold;
    }
</style>
<div class="header">SD IMOP</div>
<div class="container">
    <div class="main">
        <div class="profile-container">
            <img src="bob_the_builder.jpg" alt="Profile Picture">
            <h1>Bob the Builder</h1>
        </div>
    </div>
</div>
