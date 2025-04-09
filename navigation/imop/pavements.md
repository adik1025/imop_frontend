---
layout: post
title: IMOP Pavement Data
search_exclude: true
permalink: /imop/pavements
menu: nav/imop.html
---

<head>
  <link rel="stylesheet" href="{{ site.baseurl }}/assets/css/pavements.css" />
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>CSV Upload</title>
  <style>
    body {
      font-family: sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 2rem;
    }
    input[type="file"] {
      margin: 1rem 0;
    }
    button {
      padding: 0.5rem 1rem;
      font-size: 1rem;
      cursor: pointer;
    }
    .status {
      margin-top: 1rem;
      color: green;
    }
  </style>
</head>

<main class="main-content" id="main-content">
    <div id="pavementCount"></div>
    <br>
</main>
  <h2>Upload CSV File</h2>
  <input type="file" id="csvFile" accept=".csv" />
  <button id="saveBtn">Save</button>
  <div class="status" id="statusMsg"></div>

<script type="module">

import { pythonURI, fetchOptions } from '{{ site.baseurl }}/assets/js/api/config.js';

const fileInput = document.getElementById('csvFile');
const saveBtn = document.getElementById('saveBtn');
const statusMsg = document.getElementById('statusMsg');

saveBtn.addEventListener('click', async () => {
    const file = fileInput.files[0];
    if (!file) {
        statusMsg.textContent = 'Please select a CSV file.';
        return;
    }

    console.log("SIGMA")
    console.log(file)

    const reader = new FileReader();

    reader.onload = async function(event) {
        const csvContent = event.target.result;
        const lines = csvContent.split('\n'); // Split the content by lines
        let formattedCsv = lines.join(',,'); // Join lines with double commas

        // Log the formatted CSV string
        console.log("Formatted CSV String: ", formattedCsv);

        const postData = {
            csv: formattedCsv
        };

        try {
            const response = await fetch(`${pythonURI}/api/pavement`, {
                ...fetchOptions,
                method: 'POST',
                body: JSON.stringify(postData)
            });

            if (!response.ok) throw new Error('Failed to upload file.');

            statusMsg.textContent = 'File uploaded successfully!';
        } catch (error) {
            statusMsg.style.color = 'red';
            statusMsg.textContent = 'Error uploading file: ' + error.message;
        }
    };

    reader.onerror = function(error) {
        statusMsg.style.color = 'red';
        statusMsg.textContent = 'Error reading file: ' + error.message;
    };

    reader.readAsText(file); // Read the file content as text
});

document.addEventListener("DOMContentLoaded", (event) => {
    fetchPavementData();
});

async function fetchPavementData() {
    try {
        const response = await fetch(`${pythonURI}/api/pavement`, {...fetchOptions});

        if (!response.ok) {
            throw new Error('Failed to fetch pavements: ' + response.statusText);
        }

        const data = await response.json();
        var pavementCount = data.length || 0;

        document.getElementById('pavementCount').innerHTML = `<h2>There are ${pavementCount} assessments of pavements in SD.</h2>`;

        const body = document.getElementById('main-content');

        data.forEach(item => {
            const card = document.createElement('div');
            card.className = 'card';
            const fetched_csv = item.cell;
            const csv_arr = JSON.stringify(fetched_csv.split(',,'));

            csv_arr.forEach(str => {
                const row = document.createElement('div');
                row.innerHTML = `
                    <p>${csv_arr}</p>
                `
                card.appendChild(csv_arr);
            });

            body.appendChild(card);
        });
    } catch (error) {
        console.error('Error fetching data:', error);
    }
}
</script>
