---
layout: post
title: IMOP Pavement Data
search_exclude: true
permalink: /imop/pavements
menu: nav/imop.html
---


<main class="main-content" id="main-content">
    <div id="pavementCount"></div>
    <br>
</main>

<script type="module">

import { pythonURI, fetchOptions } from '{{ site.baseurl }}/assets/js/api/config.js';

document.addEventListener("DOMContentLoaded", (event) => {
    fetchLikedHotels();
});

async function fetchLikedHotels() {
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
            card.innerHTML = `
                <h2>${item.seg_id}</h2>
                <p>${item.pci} | ${item.pci_desc}</p>
            `;

            body.appendChild(card);

        });
    } catch (error) {
        console.error('Error fetching data:', error);
    }
}

// function handleKeyPress(event, id) {
//     if (event.key === 'Enter') {
//         event.preventDefault();
//         const newRating = event.target.textContent;
//         putHotelData(id, newRating);
//     }
// }

// async function putHotelData(id, newRating) {
    
//     const putData = {
//         id: id,
//         rating: parseInt(newRating)
//     };

//     try {
//         const response = await fetch(`${pythonURI}/api/hotel`, {
//             ...fetchOptions,
//             method: 'PUT',
//             body: JSON.stringify(putData)
//         });

//         if (!response.ok) {
//             throw new Error(`HTTP error! Status: ${response.status}`);
//         }

//         const data = await response.json();
//         console.log('Put response:', data);
//     } catch (error) {
//         console.error("Error putting data:", error);
//     }
// }

// async function deleteHotel(id) {

//     const deleteData = {
//         id: id,
//     };

//     try {
//         const response = await fetch(`${pythonURI}/api/hotel`, {
//             ...fetchOptions,
//             method: 'DELETE',
//             body: JSON.stringify(deleteData)
//         });

//         if (!response.ok) {
//             throw new Error(`HTTP error! Status: ${response.status}`);
//         }

//         const data = await response.json();
//         console.log('Delete response:', data);
//     } catch (error) {
//         console.error("Error deleting data:", error);
//     }
// }


</script>