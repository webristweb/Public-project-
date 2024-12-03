# Public-project-
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Attendance Record System</title>
  
 <style>
 
 /* styles.css */
body {
  font-family: Arial, sans-serif;
  background: linear-gradient(to right, #6a11cb, #2575fc);
  color: white;
  margin: 0;
  padding: 0;
}

.container {
  max-width: 800px;
  margin: 50px auto;
  padding: 20px;
  background: #ffffff;
  color: #000;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
}

h1 {
  text-align: center;
}

form, .search-bar {
  display: flex;
  justify-content: space-between;
  gap: 10px;
  margin-bottom: 20px;
}

form input, form select, form button, .search-bar input {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 16px;
  outline: none;
}

form button {
  background-color: #2575fc;
  color: white;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s;
}

form button:hover {
  background-color: #6a11cb;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

thead {
  background-color: #2575fc;
  color: white;
}

thead th {
  padding: 10px;
  text-align: left;
}

tbody tr:nth-child(even) {
  background-color: #f4f4f4;
}

tbody tr:nth-child(odd) {
  background-color: #ddd;
}

tbody td {
  padding: 10px;
}

tbody td button {
  background-color: #ff4d4d;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
  border-radius: 3px;
  transition: background-color 0.3s;
}

tbody td button:hover {
  background-color: #e60000;
}

 
 </style
  
</head>
<body>
  <div class="container">
    <h1>Attendance Record System</h1>
    <form id="attendance-form">
      <input type="text" id="name" placeholder="Enter Name" required>
      <input type="date" id="date" required>
      <select id="status" required>
        <option value="" disabled selected>Select Status</option>
        <option value="Present">Present</option>
        <option value="Absent">Absent</option>
      </select>
      <button type="submit">Add Record</button>
    </form>
    <div class="search-bar">
      <input type="text" id="search" placeholder="Search by Name...">
    </div>
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Date</th>
          <th>Status</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody id="records">
        <!-- Dynamic rows will appear here -->
      </tbody>
    </table>
  </div>
  
  <script>
  
  // script.js
document.addEventListener("DOMContentLoaded", () => {
  const form = document.getElementById("attendance-form");
  const recordsTable = document.getElementById("records");
  const search = document.getElementById("search");
  
  // Load records from localStorage
  const loadRecords = () => {
    const data = JSON.parse(localStorage.getItem("attendanceRecords")) || [];
    data.forEach(record => addRecordToTable(record));
  };
  
  const addRecordToTable = (record) => {
    const row = document.createElement("tr");
    row.innerHTML = `
      <td>${record.name}</td>
      <td>${record.date}</td>
      <td>${record.status}</td>
      <td><button onclick="deleteRecord(this)">Delete</button></td>
    `;
    recordsTable.appendChild(row);
  };
  
  const saveRecord = (record) => {
    const records = JSON.parse(localStorage.getItem("attendanceRecords")) || [];
    records.push(record);
    localStorage.setItem("attendanceRecords", JSON.stringify(records));
  };
  
  form.addEventListener("submit", (e) => {
    e.preventDefault();
    const name = document.getElementById("name").value;
    const date = document.getElementById("date").value;
    const status = document.getElementById("status").value;
    const record = { name, date, status };
    addRecordToTable(record);
    saveRecord(record);
    form.reset();
  });

  window.deleteRecord = (btn) => {
    const row = btn.parentElement.parentElement;
    const name = row.children[0].textContent;
    const date = row.children[1].textContent;
    row.remove();
    const records = JSON.parse(localStorage.getItem("attendanceRecords")) || [];
    const updatedRecords = records.filter(record => !(record.name === name && record.date === date));
    localStorage.setItem("attendanceRecords", JSON.stringify(updatedRecords));
  };

  search.addEventListener("input", () => {
    const filter = search.value.toLowerCase();
    const rows = recordsTable.querySelectorAll("tr");
    rows.forEach(row => {
      const name = row.children[0].textContent.toLowerCase();
      row.style.display = name.includes(filter) ? "" : "none";
    });
  });

  loadRecords();
});

  
  </script>
</body>
</html>
