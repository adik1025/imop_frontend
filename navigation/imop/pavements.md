---
layout: page
title: IMOP Pavement Data
search_exclude: true
permalink: /pavements
menu: nav/imop.html
---

<main class="max-w-5xl mx-auto px-6 py-16" id="main-content">
  <h1 class="text-3xl font-bold text-white mb-8">Upload Pavement Assessment CSV</h1>

  <!-- File Upload UI -->
  <div class="flex flex-col sm:flex-row gap-4 items-start sm:items-center mb-8">
    <input type="file" id="csvFile" accept=".csv" class="text-sm bg-neutral-800 text-gray-300 px-4 py-2 rounded-md border border-white/10 shadow-inner" />
    <button id="saveBtn" class="bg-accent hover:bg-purple-600 text-white font-medium px-6 py-2 rounded-md transition shadow-md">Save</button>
  </div>

  <div class="text-sm text-gray-400 mb-6" id="statusMsg"></div>
  <div id="pavementCount" class="text-lg font-medium text-white mb-8"></div>
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

    const postData = { csv: formattedCsv };

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
    if (!response.ok) throw new Error('Failed to fetch pavements: ' + response.statusText);

    const data = await response.json();
    const pavementCount = data.length || 0;
    document.getElementById('pavementCount').innerHTML = `<h2 class="text-white text-2xl font-semibold">There are ${pavementCount} pavement assessments in San Diego.</h2>`;

    const body = document.getElementById('main-content');

    data.forEach(item => {
      const card = document.createElement('div');
      card.className = 'bg-neutralCard border border-white/10 rounded-xl p-6 mb-6 shadow-md transition hover:border-accent';

      const cardHeader = document.createElement('div');
      cardHeader.innerHTML = `<h3 class="text-lg font-semibold text-accent mb-3">Pavement ID: ${item.id}</h3>`;
      card.appendChild(cardHeader);

      const fetched_csv = item.cell;
      const csv_arr = fetched_csv.split(',,');
      const linesToShow = 10;
      const linesDisplayed = csv_arr.slice(0, linesToShow);

      linesDisplayed.forEach(str => {
        const row = document.createElement('div');
        row.innerHTML = `<p class="text-sm text-gray-300 mb-1 border-b border-white/5 pb-1">${str.replace(/,,/g, ' | ')}</p>`;
        card.appendChild(row);
      });

      if (csv_arr.length > linesToShow) {
        const moreIndicator = document.createElement('div');
        moreIndicator.innerHTML = '<p class="text-xs italic text-gray-500 mt-2">Data has been truncated...</p>';
        card.appendChild(moreIndicator);

        const btnGroup = document.createElement('div');
        btnGroup.className = 'flex flex-wrap gap-3 mt-4';

        const downloadButton = document.createElement('button');
        downloadButton.className = 'px-4 py-2 bg-accent text-white rounded-md text-sm font-medium hover:bg-purple-600';
        downloadButton.textContent = 'Download CSV';
        downloadButton.onclick = function () {
          const blob = new Blob([fetched_csv], { type: 'text/csv' });
          const url = URL.createObjectURL(blob);
          const link = document.createElement('a');
          link.href = url;
          link.download = 'pavement_data.csv';
          link.click();
        };
        btnGroup.appendChild(downloadButton);

        const deleteButton = document.createElement('button');
        deleteButton.className = 'px-4 py-2 bg-red-600 hover:bg-red-700 text-white rounded-md text-sm font-medium';
        deleteButton.textContent = 'Delete';
        deleteButton.onclick = function () {
          deleteItem(item.id);
        };
        btnGroup.appendChild(deleteButton);

        card.appendChild(btnGroup);
      }

      body.appendChild(card);
    });
  } catch (error) {
    console.error('Error fetching data:', error);
  }
}

async function deleteItem(id) {
  const deleteData = { id };
  try {
    const response = await fetch(`${pythonURI}/api/pavement`, {
      ...fetchOptions,
      method: 'DELETE',
      body: JSON.stringify(deleteData)
    });

    if (!response.ok) throw new Error(`HTTP error! Status: ${response.status}`);
    const data = await response.json();
    console.log('Delete response:', data);
  } catch (error) {
    console.error("Error deleting data:", error);
  }
}
</script>