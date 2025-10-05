<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>College Event Management Portal</title>
<style>
body {
  font-family: "Segoe UI", sans-serif;
  background: linear-gradient(135deg, #1abc9c, #3498db);
  color: #2c3e50;
  margin: 0;
  padding: 20px;
}
.container {
  max-width: 1000px;
  margin: auto;
  background: white;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.2);
}
h1, h2 {
  text-align: center;
  color: #16a085;
}
form {
  margin-bottom: 20px;
}
input, select, textarea, button {
  width: 100%;
  padding: 10px;
  margin: 5px 0 15px;
  border: 1px solid #ccc;
  border-radius: 8px;
}
button {
  background: #16a085;
  color: white;
  border: none;
  cursor: pointer;
  font-weight: bold;
}
button:hover {
  background: #13856b;
}
table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
}
th, td {
  border: 1px solid #ccc;
  padding: 8px;
  text-align: center;
}
th {
  background: #f5f5f5;
}
.print-btn {
  background: #2c3e50;
  color: white;
  padding: 8px 15px;
  border-radius: 6px;
  float: right;
  cursor: pointer;
}
.print-btn:hover {
  background: #1a252f;
}
</style>
</head>
<body>
<div class="container">
  <h1>🎓 College Event Management Portal</h1>

  <!-- Admin Section -->
  <h2>Admin - Publish New Event</h2>
  <form id="eventForm">
    <input type="text" id="eventName" placeholder="Enter Event Name" required>
    <textarea id="eventDesc" placeholder="Enter Event Description" required></textarea>
    <button type="submit">Publish Event</button>
  </form>

  <hr>

  <!-- Student Section -->
  <h2>Student - Register for Event</h2>
  <form id="registerForm">
    <input type="text" id="studentName" placeholder="Enter Your Name" required>
    <select id="department" required>
      <option value="">Select Department</option>
      <option>ECE</option>
      <option>CSE</option>
      <option>EEE</option>
      <option>MECH</option>
      <option>CIVIL</option>
      <option>AIDS</option>
    </select>
    <select id="selectEvent" required>
      <option value="">Select Event</option>
    </select>
    <button type="submit">Register</button>
  </form>

  <hr>

  <!-- Report Section -->
  <h2>📊 Reports</h2>
  <button class="print-btn" onclick="window.print()">🖨️ Print Report</button>

  <h3>Department-wise Registration Count</h3>
  <table id="deptTable">
    <thead><tr><th>Department</th><th>Count</th></tr></thead>
    <tbody></tbody>
  </table>

  <h3>Event-wise Registration Count</h3>
  <table id="eventTable">
    <thead><tr><th>Event</th><th>Count</th></tr></thead>
    <tbody></tbody>
  </table>

  <h3>Department-wise Student List</h3>
  <div id="deptLists"></div>
</div>

<script>
let events = [];
let registrations = [];

const eventForm = document.getElementById("eventForm");
const registerForm = document.getElementById("registerForm");
const selectEvent = document.getElementById("selectEvent");
const deptTableBody = document.querySelector("#deptTable tbody");
const eventTableBody = document.querySelector("#eventTable tbody");
const deptLists = document.getElementById("deptLists");

// Add event
eventForm.addEventListener("submit", e => {
  e.preventDefault();
  const name = document.getElementById("eventName").value.trim();
  const desc = document.getElementById("eventDesc").value.trim();
  if (name && desc) {
    events.push({ name, desc });
    updateEventDropdown();
    alert("✅ Event Published Successfully!");
    eventForm.reset();
  }
});

// Update dropdown
function updateEventDropdown() {
  selectEvent.innerHTML = '<option value="">Select Event</option>';
  events.forEach(e => {
    const opt = document.createElement("option");
    opt.value = e.name;
    opt.textContent = e.name;
    selectEvent.appendChild(opt);
  });
}

// Register student
registerForm.addEventListener("submit", e => {
  e.preventDefault();
  const name = document.getElementById("studentName").value.trim();
  const dept = document.getElementById("department").value;
  const event = document.getElementById("selectEvent").value;
  if (name && dept && event) {
    registrations.push({ name, dept, event });
    alert("🎉 Registration Successful!");
    registerForm.reset();
    updateReports();
  }
});

// Update reports
function updateReports() {
  const deptCount = {};
  const eventCount = {};
  const deptWise = {};

  registrations.forEach(r => {
    deptCount[r.dept] = (deptCount[r.dept] || 0) + 1;
    eventCount[r.event] = (eventCount[r.event] || 0) + 1;
    if (!deptWise[r.dept]) deptWise[r.dept] = [];
    deptWise[r.dept].push(r.name + " (" + r.event + ")");
  });

  // Dept count
  deptTableBody.innerHTML = "";
  Object.keys(deptCount).forEach(d => {
    deptTableBody.innerHTML += `<tr><td>${d}</td><td>${deptCount[d]}</td></tr>`;
  });

  // Event count
  eventTableBody.innerHTML = "";
  Object.keys(eventCount).forEach(e => {
    eventTableBody.innerHTML += `<tr><td>${e}</td><td>${eventCount[e]}</td></tr>`;
  });

  // Dept-wise list
  deptLists.innerHTML = "";
  Object.keys(deptWise).forEach(d => {
    const list = deptWise[d].map(n => `<li>${n}</li>`).join("");
    deptLists.innerHTML += `<h4>${d}</h4><ul>${list}</ul>`;
  });
}
</script>
</body>
</html>
