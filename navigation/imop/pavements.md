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
    <h2>Upload CSV File</h2>
    <input type="file" id="csvFile" accept=".csv" />
    <button id="saveBtn">Save</button>
    <div class="status" id="statusMsg"></div>
    <div id="pavementCount"></div>
    <br>
</main>

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

    const reader = new FileReader();

    reader.onload = async function(event) {
        const csvContent = event.target.result;
        const lines = csvContent.split('\n');
        let formattedCsv = lines.join(' | ');

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

    reader.readAsText(file);
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

        document.getElementById('pavementCount').innerHTML = `<h2 class="count-heading">There are ${pavementCount} assessments of pavements in SD.</h2>`;

        const body = document.getElementById('main-content');

        data.forEach(item => {
            const card = document.createElement('div');
            card.className = 'card';

            const cardHeader = document.createElement('div');
            cardHeader.className = 'card-header';
            cardHeader.innerHTML = `<h3 class="card-title">Pavement ID: ${item.id}</h3>`;
            card.appendChild(cardHeader);

            const fetched_csv = item.cell;
            const csv_arr = fetched_csv.split(',,');

            const linesToShow = 10;
            const linesDisplayed = csv_arr.slice(0, linesToShow);

            linesDisplayed.forEach(str => {
                const row = document.createElement('div');
                row.className = 'csv-row';
                row.innerHTML = `<p>${str.replace(/,,/g, ' | ')}</p>`;
                card.appendChild(row);
            });

            if (csv_arr.length > linesToShow) {
                const moreIndicator = document.createElement('div');
                moreIndicator.className = 'more-indicator';
                moreIndicator.innerHTML = '<p class="truncate-text">Data has been truncated</p>';
                card.appendChild(moreIndicator);

                const downloadButton = document.createElement('button');
                downloadButton.className = 'btn download-btn';
                downloadButton.textContent = 'Download Full CSV';
                downloadButton.onclick = function() {
                    const blob = new Blob([fetched_csv], { type: 'text/csv' });
                    const url = URL.createObjectURL(blob);
                    const link = document.createElement('a');
                    link.href = url;
                    link.download = 'pavement_data.csv';
                    link.click();
                };
                card.appendChild(downloadButton);

                const deleteButton = document.createElement('button');
                deleteButton.className = 'btn delete-btn';
                deleteButton.textContent = 'Delete';
                deleteButton.onclick = function() {
                    deleteItem(item.id);
                };
                card.appendChild(deleteButton);
            }

            body.appendChild(card);
        });
    } catch (error) {
        console.error('Error fetching data:', error);
    }
}

async function deleteItem(id) {
    const deleteData = {
        id: id,
    };

    try {
        const response = await fetch(`${pythonURI}/api/pavement`, {
            ...fetchOptions,
            method: 'DELETE',
            body: JSON.stringify(deleteData)
        });

        if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
        }

        const data = await response.json();
        console.log('Delete response:', data);
    } catch (error) {
        console.error("Error deleting data:", error);
    }
}
</script>
