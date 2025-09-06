<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Student Result System</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
      transition: background 0.3s, color 0.3s;
    }
    h1 {
      text-align: center;
      margin-bottom: 20px;
    }
    .container {
      max-width: 800px;
      margin: auto;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.2);
    }
    label {
      display: block;
      margin-top: 10px;
    }
    input {
      padding: 8px;
      margin-top: 5px;
      width: 100%;
      border-radius: 6px;
      border: 1px solid #ccc;
    }
    button {
      margin-top: 15px;
      padding: 10px 20px;
      border: none;
      background: #007bff;
      color: white;
      border-radius: 6px;
      cursor: pointer;
    }
    button:hover { background: #0056b3; }
    .dark-mode {
      background: #1e1e1e;
      color: #f5f5f5;
    }
    .dark-mode input {
      background: #333;
      color: #fff;
      border: 1px solid #777;
    }
    table {
      margin-top: 20px;
      border-collapse: collapse;
      width: 100%;
    }
    th, td {
      padding: 10px;
      border: 1px solid #ddd;
      text-align: center;
    }
    th { background: #007bff; color: white; }
    .dark-mode th { background: #333; }
    .rank {
      font-weight: bold;
      color: green;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>📊 Student Result System</h1>

    <!-- Input Form -->
    <label>Student Name:</label>
    <input type="text" id="name">

    <label>Marks (comma separated e.g. 78,89,90):</label>
    <input type="text" id="marks">

    <button onclick="addStudent()">Add & Show Result</button>
    <button onclick="toggleMode()">🌙 Dark/Light Mode</button>
    <button onclick="downloadPDF()">📥 Download PDF</button>

    <!-- Results Table -->
    <table id="resultTable">
      <thead>
        <tr>
          <th>Rank 🏆</th>
          <th>Name</th>
          <th>Total</th>
          <th>Percentage</th>
          <th>Grade</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
  </div>

  <script>
    let students = [];

    function calculateGrade(percentage) {
      if (percentage >= 90) return "A+";
      if (percentage >= 75) return "A";
      if (percentage >= 60) return "B";
      if (percentage >= 45) return "C";
      if (percentage >= 33) return "D";
      return "F";
    }

    function addStudent() {
      const name = document.getElementById("name").value.trim();
      const marksInput = document.getElementById("marks").value.trim();

      if (!name || !marksInput) {
        alert("Please enter name and marks!");
        return;
      }

      const marksArray = marksInput.split(",").map(m => parseInt(m.trim()));
      if (marksArray.some(isNaN)) {
        alert("Enter valid numeric marks!");
        return;
      }

      const total = marksArray.reduce((a, b) => a + b, 0);
      const percentage = (total / (marksArray.length * 100)) * 100;
      const grade = calculateGrade(percentage);

      students.push({ name, total, percentage, grade });

      students.sort((a, b) => b.percentage - a.percentage);

      showTable();
      document.getElementById("name").value = "";
      document.getElementById("marks").value = "";
    }

    function showTable() {
      const tbody = document.querySelector("#resultTable tbody");
      tbody.innerHTML = "";

      students.forEach((s, index) => {
        let row = `<tr>
          <td class="rank">${index + 1}</td>
          <td>${s.name}</td>
          <td>${s.total}</td>
          <td>${s.percentage.toFixed(2)}%</td>
          <td>${s.grade}</td>
        </tr>`;
        tbody.innerHTML += row;
      });
    }

    function toggleMode() {
      document.body.classList.toggle("dark-mode");
    }

    function downloadPDF() {
      window.print();
    }
  </script>
</body>
</html># Student-Result-Management
