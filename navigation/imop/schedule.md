---
layout: page
title: Schedule
permalink: /schedule
show_reading_time: false
menu: nav/imop.html
search_exclude: true
---


<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Maintenance Schedule</title>
</head>
<body class="bg-gray-900 text-gray-100">
  <main class="max-w-5xl mx-auto px-6 py-16" id="main-content">
    <!-- Title -->
    <h1 class="text-3xl font-bold text-white mb-8">Manage repair schedule and Calendar</h1>
    <!-- Event Form -->
    <div class="bg-gray-800 p-6 rounded-md shadow-lg mb-8">
      <h2 class="text-2xl font-semibold text-white mb-4">Maintenance Registration Form</h2>
      <form id="eventForm" class="space-y-4">
        <div>
          <label for="clubName" class="block text-sm font-medium text-gray-300 mb-1">WO_ID</label>
          <input type="text" id="clubName" placeholder="Enter the IAMFLOC (Location)" class="w-full px-4 py-2 text-sm bg-neutral-800 text-gray-300 rounded-md border border-white/10 shadow-inner" required>
        </div>
        <div>
          <label for="eventDescription" class="block text-sm font-medium text-gray-300 mb-1">IAMFLOC</label>
          <input type="text" id="eventDescription" placeholder="Type the WO_ID (Work Order ID)" class="w-full px-4 py-2 text-sm bg-neutral-800 text-gray-300 rounded-md border border-white/10 shadow-inner" required>
        </div>
        <div>
          <label for="eventDate" class="block text-sm font-medium text-gray-300 mb-1">Schedule Date</label>
          <input type="date" id="eventDate" class="w-full px-4 py-2 text-sm bg-neutral-800 text-gray-300 rounded-md border border-white/10 shadow-inner" required>
        </div>
        <button type="submit" id="submitBtn" class="bg-accent hover:bg-purple-600 text-white font-medium px-6 py-2 rounded-md transition shadow-md">Schedule</button>
      </form>
    </div>


    <!-- Event List -->
    <div id="eventListContainer" class="bg-gray-800 p-6 rounded-md shadow-lg mb-8">
      <h2 class="text-2xl font-semibold text-white mb-4">Maintenance List</h2>
      <div id="events" class="space-y-4">
        <!-- New events will appear here -->
      </div>
    </div>


    <!-- Calendar -->
    <div class="bg-gray-800 p-6 rounded-md shadow-lg">
      <h2 class="text-2xl font-semibold text-white mb-4">Calendar</h2>
      <div class="flex items-center justify-between mb-6">
        <button id="prevMonthBtn" class="bg-gray-700 hover:bg-gray-600 text-white font-medium py-1 px-3 rounded-md">Previous</button>
        <span id="currentMonth" class="text-lg font-bold text-white"></span>
        <button id="nextMonthBtn" class="bg-gray-700 hover:bg-gray-600 text-white font-medium py-1 px-3 rounded-md">Next</button>
      </div>
      <div id="calendar" class="grid grid-cols-7 gap-2 bg-gray-700 p-5 rounded-md"></div>
      <div id="eventDetails" class="mt-6">
        <h3 class="text-lg font-medium text-white mb-2">Maintenance on <span id="selectedDate" class="font-semibold"></span></h3>
        <div id="eventsOnDate" class="space-y-2">
          <!-- Selected date events will appear here -->
        </div>
      </div>
    </div>
  </main>


  <script type="module">
    import { pythonURI } from "{{site.baseurl}}/assets/js/api/config.js";


    let events = [];
    let currentYear = new Date().getFullYear();
    let currentMonth = new Date().getMonth();


    document.addEventListener('DOMContentLoaded', () => {
      const eventForm = document.getElementById('eventForm');
      const calendar = document.getElementById('calendar');
      const selectedDateText = document.getElementById('selectedDate');
      const eventsOnDate = document.getElementById('eventsOnDate');
      const prevMonthBtn = document.getElementById('prevMonthBtn');
      const nextMonthBtn = document.getElementById('nextMonthBtn');
      const currentMonthText = document.getElementById('currentMonth');


      eventForm.addEventListener('submit', async function (e) {
        e.preventDefault();
        const clubName = document.getElementById('clubName').value.trim();
        const eventDescription = document.getElementById('eventDescription').value.trim();
        const eventDate = document.getElementById('eventDate').value;


        if (clubName && eventDescription && eventDate) {
          const payload = {
            title: clubName,
            description: eventDescription,
            date: eventDate,
          };
          try {
            await fetch(`${pythonURI}/api/event`, {
              method: 'POST',
              headers: { 'Content-Type': 'application/json' },
              body: JSON.stringify(payload),
            });
            await fetchAndDisplayEvents();
            alert('Maintenance has been scheduled!');
          } catch (error) {
            alert(`Failed to schedule maintenance: ${error.message}`);
          }
        } else {
          alert('Please fill out all fields!');
        }
      });


      async function fetchAndDisplayEvents() {
        try {
          const response = await fetch(`${pythonURI}/api/event`, { method: 'GET' });
          events = await response.json();
          renderEventList();
          initializeCalendar(currentYear, currentMonth);
        } catch (error) {
          alert(`Failed to fetch events: ${error.message}`);
        }
      }


      function renderEventList() {
        const eventList = document.getElementById('events');
        eventList.innerHTML = ''; // Clear existing events


        events.forEach(event => {
          const eventBox = document.createElement('div');
          eventBox.classList.add('bg-gray-700', 'p-4', 'rounded-md', 'shadow-md', 'space-y-2');
          eventBox.innerHTML = `
            <p class="text-sm text-gray-300"><strong>WO_ID:</strong> ${event.title}</p>
            <p class="text-sm text-gray-300"><strong>IAMFLOC:</strong> ${event.description}</p>
            <p class="text-sm text-gray-300"><strong>Date:</strong> ${event.date}</p>
          `;


          const deleteBtn = document.createElement('button');
          deleteBtn.classList.add('bg-accent', 'hover:bg-purple-600', 'text-white', 'font-medium', 'px-4', 'py-2', 'rounded-md');
          deleteBtn.textContent = 'Delete';
          deleteBtn.addEventListener('click', async () => {
            const confirmed = confirm('Are you sure you want to cancel this maintenance?');
            if (confirmed) {
              await deleteEvent(event.id);
            }
          });


          eventBox.appendChild(deleteBtn);
          eventList.appendChild(eventBox);
        });
      }


      async function deleteEvent(eventId) {
        try {
          const response = await fetch(`${pythonURI}/api/event`, {
            method: 'DELETE',
            headers: {
              'Content-Type': 'application/json',
            },
            credentials: 'include',
            body: JSON.stringify({ id: eventId })
          });


          if (response.ok) {
            alert('Maintenance schedule cancelled!');
            await fetchAndDisplayEvents(); // Refresh the event list dynamically
          } else {
            const error = await response.json();
            console.error(`Error: ${error.message}`);
            alert('Failed to cancel the schedule. Please try again.');
          }
        } catch (error) {
          console.error('An error occurred while canceling the schedule:', error);
          alert('Failed to cancel the event. Please try again.');
        }
      }


      function initializeCalendar(year, month) {
        const daysInMonth = new Date(year, month + 1, 0).getDate();
        const firstDayOfMonth = new Date(year, month, 1).getDay();
        calendar.innerHTML = '';


        // Add empty divs for days before the first day of the month
        for (let i = 0; i < firstDayOfMonth; i++) {
          const emptyDiv = document.createElement('div');
          calendar.appendChild(emptyDiv);
        }


        // Add days with event indicators
        for (let day = 1; day <= daysInMonth; day++) {
          const date = `${year}-${String(month + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`;
          const dayDiv = document.createElement('div');
          dayDiv.textContent = day;
          dayDiv.classList.add(
            'text-center',
            'bg-gray-600',
            'hover:bg-gray-500',
            'text-white',
            'font-medium',
            'py-2',
            'rounded-md',
            'cursor-pointer'
          );


          // Check if this day has any events
          const hasEvents = events.some(event => event.date === date);
          if (hasEvents) {
            dayDiv.classList.add('bg-purple-500'); // Highlight the day with a purple background
          }


          dayDiv.addEventListener('click', () => {
            selectedDateText.textContent = date;
            showEventsOnDate(date);
          });


          calendar.appendChild(dayDiv);
        }


        currentMonthText.textContent = `${
          [
            'January',
            'February',
            'March',
            'April',
            'May',
            'June',
            'July',
            'August',
            'September',
            'October',
            'November',
            'December',
          ][month]
        } ${year}`;
      }


      function showEventsOnDate(date) {
        eventsOnDate.innerHTML = '';
        const eventsOnSelectedDate = events.filter(event => event.date === date);
        if (eventsOnSelectedDate.length === 0) {
          eventsOnDate.innerHTML = '<p class="text-white">No maintenance for this date.</p>';
        } else {
          eventsOnSelectedDate.forEach(event => {
            const eventDiv = document.createElement('div');
            eventDiv.innerHTML = `
              <p class="text-sm text-gray-300"><strong>WO_ID:</strong>${event.title}</p>
              <p class="text-sm text-gray-300"><strong>IAMFLOC:</strong> ${event.description}</p>
            `;
            eventsOnDate.appendChild(eventDiv);
          });
        }
      }


      prevMonthBtn.addEventListener('click', () => {
        currentMonth -= 1;
        if (currentMonth < 0) {
          currentMonth = 11;
          currentYear -= 1;
        }
        initializeCalendar(currentYear, currentMonth);
      });


      nextMonthBtn.addEventListener('click', () => {
        currentMonth += 1;
        if (currentMonth > 11) {
          currentMonth = 0;
          currentYear += 1;
        }
        initializeCalendar(currentYear, currentMonth);
      });


      fetchAndDisplayEvents();
    });
  </script>
</body>
</html>