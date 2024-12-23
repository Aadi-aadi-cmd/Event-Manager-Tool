# Event-Manager-Tool
Code Part:-

HTML part
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Event Management Dashboard</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>

<div class="container">
  <h1>Event Management Dashboard</h1>

  <h2>Event Management</h2>
  <button id="addEventBtn">Add Event</button>
  <div id="eventList"></div>

  <h2>Attendee Management</h2>
  <button id="addAttendeeBtn">Add Attendee</button>
  <table id="attendeeTable" border="1">
    <thead>
      <tr>
        <th>Name</th>
        <th>Email</th>
        <th>Actions</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <h2>Task Tracker</h2>
  <div id="taskContainer"></div>
</div>

<!-- Modals -->
<div id="addEventModal" class="modal">
  <form id="eventForm">
    <h3>Add/Edit Event</h3>
    <input type="text" id="eventName" placeholder="Event Name" required />
    <textarea id="eventDescription" placeholder="Event Description" required></textarea>
    <input type="text" id="eventLocation" placeholder="Location" required />
    <input type="date" id="eventDate" required />
    <button type="submit">Save</button>
    <button type="button" id="closeEventModal">Cancel</button>
  </form>
</div>

<div id="addAttendeeModal" class="modal">
  <form id="attendeeForm">
    <h3>Add Attendee</h3>
    <input type="text" id="attendeeName" placeholder="Name" required />
    <input type="email" id="attendeeEmail" placeholder="Email" required />
    <button type="submit">Save</button>
    <button type="button" id="closeAttendeeModal">Cancel</button>
  </form>
</div>

<script src="script.js"></script>
</body>
</html>


Style.css
/* General Styling */
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
}

.container {
    padding: 20px;
}

h1 {
    color: #333;
}

button {
    padding: 10px 15px;
    background-color: #007bff;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #0056b3;
}

.modal {
    display: none;
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: #fff;
    padding: 20px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.modal form {
    display: flex;
    flex-direction: column;
}

.modal input, .modal textarea {
    margin: 10px 0;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

.event-card {
    border: 1px solid #ccc;
    border-radius: 5px;
    padding: 15px;
    margin: 10px 0;
}

@media (max-width: 768px) {
    .container {
        padding: 10px;
    }
}


script.js
let events = [];
let attendees = [];
let tasks = [];

// Event Management
const eventList = document.getElementById('eventList');
const addEventModal = document.getElementById('addEventModal');
const eventForm = document.getElementById('eventForm');

document.getElementById('addEventBtn').addEventListener('click', () => {
    addEventModal.style.display = 'block';
});

document.getElementById('closeEventModal').addEventListener('click', () => {
    addEventModal.style.display = 'none';
});

eventForm.addEventListener('submit', (e) => {
    e.preventDefault();
    const name = document.getElementById('eventName').value;
    const description = document.getElementById('eventDescription').value;
    const location = document.getElementById('eventLocation').value;
    const date = document.getElementById('eventDate').value;

    events.push({ name, description, location, date });
    displayEvents();
    addEventModal.style.display = 'none';
});

function displayEvents() {
    eventList.innerHTML = '';
    events.forEach((event, index) => {
        const eventCard = document.createElement('div');
        eventCard.className = 'event-card';
        eventCard.innerHTML = `
            <h3>${event.name}</h3>
            <p>${event.description}</p>
            <p>${event.location}</p>
            <p>${event.date}</p>
            <button onclick="deleteEvent(${index})">Delete</button>
        `;
        eventList.appendChild(eventCard);
    });
}

function deleteEvent(index) {
    events.splice(index, 1);
    displayEvents();
}

// Add additional JavaScript here for attendees and tasks



eventmanagement.js
import React, { useState } from "react";

function EventManagement() {
  const [events, setEvents] = useState([]);
  const [form, setForm] = useState({ name: "", description: "", location: "", date: "" });

  const handleChange = (e) => {
    setForm({ ...form, [e.target.name]: e.target.value });
  };

  const addEvent = () => {
    setEvents([...events, form]);
    setForm({ name: "", description: "", location: "", date: "" });
  };

  return (
    <div>
      <h2>Event Management</h2>
      <div>
        <input type="text" name="name" value={form.name} onChange={handleChange} placeholder="Event Name" />
        <input type="text" name="description" value={form.description} onChange={handleChange} placeholder="Description" />
        <input type="text" name="location" value={form.location} onChange={handleChange} placeholder="Location" />
        <input type="date" name="date" value={form.date} onChange={handleChange} />
        <button onClick={addEvent}>Add Event</button>
      </div>
      <ul>
        {events.map((event, index) => (
          <li key={index}>
            <strong>{event.name}</strong> - {event.description} ({event.location}) on {event.date}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default EventManagement;



Attendeemanagement.js
import React, { useState } from "react";

function AttendeeManagement() {
  const [attendees, setAttendees] = useState([]);
  const [attendee, setAttendee] = useState({ name: "", email: "" });

  const handleChange = (e) => {
    setAttendee({ ...attendee, [e.target.name]: e.target.value });
  };

  const addAttendee = () => {
    setAttendees([...attendees, attendee]);
    setAttendee({ name: "", email: "" });
  };

  return (
    <div>
      <h2>Attendee Management</h2>
      <div>
        <input type="text" name="name" value={attendee.name} onChange={handleChange} placeholder="Name" />
        <input type="email" name="email" value={attendee.email} onChange={handleChange} placeholder="Email" />
        <button onClick={addAttendee}>Add Attendee</button>
      </div>
      <ul>
        {attendees.map((attendee, index) => (
          <li key={index}>
            {attendee.name} ({attendee.email})
          </li>
        ))}
      </ul>
    </div>
  );
}

export default AttendeeManagement;


Tasktracker.js
import React, { useState } from "react";

function TaskTracker() {
  const [tasks, setTasks] = useState([]);
  const [task, setTask] = useState({ name: "", deadline: "", status: "Pending" });

  const handleChange = (e) => {
    setTask({ ...task, [e.target.name]: e.target.value });
  };

  const addTask = () => {
    setTasks([...tasks, task]);
    setTask({ name: "", deadline: "", status: "Pending" });
  };

  const updateStatus = (index) => {
    const updatedTasks = tasks.map((task, i) =>
      i === index ? { ...task, status: task.status === "Pending" ? "Completed" : "Pending" } : task
    );
    setTasks(updatedTasks);
  };

  return (
    <div>
      <h2>Task Tracker</h2>
      <div>
        <input type="text" name="name" value={task.name} onChange={handleChange} placeholder="Task Name" />
        <input type="date" name="deadline" value={task.deadline} onChange={handleChange} />
        <button onClick={addTask}>Add Task</button>
      </div>
      <ul>
        {tasks.map((task, index) => (
          <li key={index}>
            {task.name} (Deadline: {task.deadline}) - Status: {task.status}{" "}
            <button onClick={() => updateStatus(index)}>Toggle Status</button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default TaskTracker;


App.js
import React from "react";
import EventManagement from "./components/EventManagement";
import AttendeeManagement from "./components/AttendeeManagement";
import TaskTracker from "./components/TaskTracker";

function App() {
  return (
    <div>
      <h1>Event Management Dashboard</h1>
      <EventManagement />
      <AttendeeManagement />
      <TaskTracker />
    </div>
  );
}

export default App;


