---
layout: page
title:
search_exclude: true
permalink: /schedule 
menu: nav/imop.html
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Schedule</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background: #f4f4f9;
        }

        .calendar {
            width: 350px;
            background: #ffffff;
            border-radius: 10px;
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
            overflow: hidden;
        }

        .calendar-header {
            background: #007bff;
            color: #ffffff;
            text-align: center;
            padding: 20px;
            font-size: 1.5em;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .calendar-header button {
            background: none;
            border: none;
            color: #ffffff;
            font-size: 1.5em;
            cursor: pointer;
        }

        .calendar-days {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            text-align: center;
            padding: 10px;
            background: #f8f9fa;
            font-weight: bold;
        }

        .calendar-body {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            padding: 10px;
        }

        .calendar-body div {
            width: 40px;
            height: 40px;
            margin: auto;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            border-radius: 50%;
            font-size: 0.9em;
            font-weight: bold;
            color: #333; /* Set a default dark color for text */
        }

        .calendar-body div:hover {
            background: #007bff;
            color: #ffffff;
        }

        .calendar-body .today {
            background: #28a745;
            color: #ffffff;
        }

        .calendar-body .disabled {
            color: #d3d3d3;
            cursor: not-allowed;
        }
    </style>
</head>
<body>
    <div class="calendar">
        <div class="calendar-header">
            <button id="prevMonth">←</button>
            <h2 id="monthYear"></h2>
            <button id="nextMonth">→</button>
        </div>
        <div class="calendar-days">
            <div>Sun</div>
            <div>Mon</div>
            <div>Tue</div>
            <div>Wed</div>
            <div>Thu</div>
            <div>Fri</div>
            <div>Sat</div>
        </div>
        <div class="calendar-body" id="calendarDays"></div>
    </div>

    <script>
        const calendarDays = document.getElementById('calendarDays');
        const monthYear = document.getElementById('monthYear');
        const prevMonth = document.getElementById('prevMonth');
        const nextMonth = document.getElementById('nextMonth');

        let currentDate = new Date();

        function renderCalendar() {
            const year = currentDate.getFullYear();
            const month = currentDate.getMonth();
            const firstDay = new Date(year, month, 1).getDay();
            const lastDate = new Date(year, month + 1, 0).getDate();

            // Set header
            monthYear.innerText = currentDate.toLocaleString('default', { month: 'long', year: 'numeric' });

            // Clear previous days
            calendarDays.innerHTML = '';

            // Add empty spaces before the first day
            for (let i = 0; i < firstDay; i++) {
                const emptyDiv = document.createElement('div');
                emptyDiv.classList.add('disabled');
                calendarDays.appendChild(emptyDiv);
            }

            // Add days
            for (let day = 1; day <= lastDate; day++) {
                const dayDiv = document.createElement('div');
                dayDiv.innerText = day;

                const today = new Date();
                if (year === today.getFullYear() && month === today.getMonth() && day === today.getDate()) {
                    dayDiv.classList.add('today');
                }

                dayDiv.addEventListener('click', () => {
                    alert(`You selected ${day}/${month + 1}/${year}`);
                });

                calendarDays.appendChild(dayDiv);
            }
        }

        prevMonth.addEventListener('click', () => {
            currentDate.setMonth(currentDate.getMonth() - 1);
            renderCalendar();
        });

        nextMonth.addEventListener('click', () => {
            currentDate.setMonth(currentDate.getMonth() + 1);
            renderCalendar();
        });

        renderCalendar();
    </script>
</body>
</html>