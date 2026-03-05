<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>WellTrack BMI - SanJoseLitexSeniorHighSchool</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body {
      font-family: 'Poppins', Arial, sans-serif;
      background: linear-gradient(to right, #ff9ec4, #ff9ec4);
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      margin: 0;
      padding: 20px;
    }

    .card {
      background: rgba(255,255,255,0.95);
      padding: 40px 30px;
      border-radius: 20px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.2);
      text-align: center;
      width: 95%;
      max-width: 950px;
      border: 2px solid #000;
      animation: fadeIn 0.6s ease-in-out;
      max-height: 90vh;
      overflow-y: auto;
      position: relative;
    }

    h1 { color: hotpink; font-size: 28px; margin-bottom: 5px; text-shadow: 1px 1px 2px #00000070; }
    h2 { color: hotpink; font-size: 22px; margin-bottom: 20px; text-shadow: 1px 1px 2px #00000050; }

    .logo-left, .logo-right {
      width: 60px; height: 60px; border-radius: 10px;
      position: absolute; top: 15px;
    }
    .logo-left { left: 15px; }
    .logo-right { right: 15px; }

    input {
      width: 90%; max-width: 400px; padding: 12px; margin: 10px 0;
      border-radius: 10px; border: 2px solid #000;
      background-color: #ffe6f5; font-size: 16px; box-sizing: border-box; outline: none;
    }

    .password-container {
      position: relative; width: 90%; max-width: 400px; margin: 10px auto;
    }
    .password-container input { width: 100%; padding-right: 40px; }

    .toggle-eye {
      position: absolute; right: 12px; top: 50%;
      transform: translateY(-50%); cursor: pointer; font-size: 18px; color: #555;
    }

    button {
      padding: 10px 20px; border-radius: 10px; background-color: hotpink;
      color: white; font-size: 14px; font-weight: 600; border: none;
      cursor: pointer; margin: 5px; transition: 0.3s; box-shadow: 2px 2px 5px rgba(0,0,0,0.3);
    }
    button:hover { background-color: #ff5ca1; transform: scale(1.05); }

    table { width: 100%; border-collapse: collapse; margin-top: 15px; background: white; }
    th, td { border: 1px solid #ccc; padding: 10px; text-align: center; font-size: 13px; }
    th { background-color: hotpink; color: white; }

    .chart-box { width: 100%; max-width: 700px; margin: 20px auto; }

    @keyframes fadeIn { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }
  </style>
</head>
<body>

<div id="frontPage" class="card">
  <img src="https://upload.wikimedia.org/wikipedia/en/c/c3/BMI_Logo_blue_spark.svg" class="logo-left">
  <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQIIgIbE3prBazJq_qX0sFnyRAEi7oWAZyfBp7ujJBIRw&s=10" class="logo-right">
  <br><br>
  <h1>Welcome to WellTrack BMI</h1>
  <h2>SanJoseLitexSeniorHighSchool</h2>
  <button onclick="showPage('loginPage')">Login / Sign Up</button>
  <button onclick="viewDashboard(false, false, true)" style="background:#555">Public Dashboard</button>
</div>

<div id="loginPage" class="card" style="display:none;">
  <h2>Login</h2>
  <input type="text" id="loginUser" placeholder="Surname-Section">
  <div class="password-container">
    <input type="password" id="loginPass" placeholder="Password">
    <span class="toggle-eye" onclick="togglePass('loginPass')">👁️</span>
  </div>
  <button onclick="login()">Login</button>
  <p onclick="showPage('signupPage')" style="cursor:pointer; color:#5e00ff; text-decoration:underline;">Create Account</p>
  <button onclick="showPage('frontPage')" style="background:#555">Back</button>
</div>

<div id="signupPage" class="card" style="display:none;">
  <h2>Sign Up</h2>
  <input type="text" id="signupUser" placeholder="Surname-Section">
  <div class="password-container">
    <input type="password" id="signupPass" placeholder="Password">
    <span class="toggle-eye" onclick="togglePass('signupPass')">👁️</span>
  </div>
  <button onclick="signup()">Sign Up</button>
  <button onclick="showPage('loginPage')">Back to Login</button>
</div>

<div id="bmiPage" class="card" style="display:none;">
  <img src="https://upload.wikimedia.org/wikipedia/en/c/c3/BMI_Logo_blue_spark.svg" class="logo-left">
  <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQIIgIbE3prBazJq_qX0sFnyRAEi7oWAZyfBp7ujJBIRw&s=10" class="logo-right">
  <h1>WellTrack BMI</h1>
  <p>Logged in: <strong id="displayUser"></strong></p>
  <input type="number" id="height" placeholder="Height (cm)">
  <input type="number" id="weight" placeholder="Weight (kg)">
  <br>
  <button onclick="calculateBMI()">Calculate & Save</button>
  <h3 id="result"></h3>
  <hr>
  <button onclick="showAnalytics()" style="background:#00bcd4;">My Progress Chart</button>
  <button onclick="viewDashboard(false, true)" style="background:#ba68c8;">Manage My Records</button>
  <button onclick="viewDashboard(false, false, true)" style="background:#ff9ec4;">Public Dashboard</button>
  <button id="adminBtn" style="display:none; background:#5e00ff;" onclick="viewDashboard(true, false)">Admin Panel</button>
  <button onclick="logout()" style="background:#555">Logout</button>
</div>

<div id="dashboardPage" class="card" style="display:none;">
  <h2 id="dashTitle">BMI Records</h2>
  <table>
    <thead>
      <tr>
        <th>Date</th>
        <th>Surname</th>
        <th>Section</th>
        <th>BMI</th>
        <th>Category</th>
        <th id="actionCol" style="display:none;">Actions</th>
      </tr>
    </thead>
    <tbody id="tableBody"></tbody>
  </table>
  <br>
  <button onclick="currentUser ? showPage('bmiPage') : showPage('frontPage')">Back</button>
</div>

<div id="analyticsPage" class="card" style="display:none;">
  <h2>Your BMI Trend</h2>
  <div class="chart-box">
    <canvas id="bmiChart"></canvas>
  </div>
  <button onclick="showPage('bmiPage')">Back</button>
</div>

<script>
let users = JSON.parse(localStorage.getItem("users")) || { "admin": "admin123" };
let bmiRecords = JSON.parse(localStorage.getItem("bmiRecords")) || [];
let currentUser = "";
let myChart = null;

function showPage(id) {
  document.querySelectorAll('.card').forEach(c => c.style.display = 'none');
  document.getElementById(id).style.display = 'block';
}

function togglePass(id) {
  const x = document.getElementById(id);
  x.type = x.type === "password" ? "text" : "password";
}

function signup() {
  let u = document.getElementById('signupUser').value.trim();
  let p = document.getElementById('signupPass').value.trim();
  if(!u.includes('-')) { alert("Use format: Surname-Section"); return; }
  users[u] = p;
  localStorage.setItem("users", JSON.stringify(users));
  alert("Account Created!"); showPage('loginPage');
}

function login() {
  let u = document.getElementById('loginUser').value.trim();
  let p = document.getElementById('loginPass').value.trim();
  if(users[u] === p) {
    currentUser = u;
    document.getElementById('displayUser').innerText = u;
    document.getElementById('adminBtn').style.display = (u === "admin") ? "inline-block" : "none";
    showPage('bmiPage');
  } else { alert("Invalid login"); }
}

function getCategory(bmi) {
  if(bmi < 18.5) return "Underweight";
  if(bmi < 24.9) return "Normal";
  if(bmi < 29.9) return "Overweight";
  return "Obese";
}

function calculateBMI() {
  let h = parseFloat(document.getElementById('height').value);
  let w = parseFloat(document.getElementById('weight').value);
  if(!h || !w || h <= 0 || w <= 0) { alert("Enter valid height and weight!"); return; }
  
  let bmi = (w / ((h/100)**2)).toFixed(2);
  let cat = getCategory(bmi);
  let parts = currentUser.split("-");

  bmiRecords.push({
    owner: currentUser,
    date: new Date().toLocaleDateString(),
    surname: parts[0] || "Guest", 
    section: parts[1] || "N/A",
    height: h, weight: w, bmi: bmi, category: cat
  });

  localStorage.setItem("bmiRecords", JSON.stringify(bmiRecords));
  document.getElementById('result').innerText = `BMI: ${bmi} (${cat}) - Saved!`;
}

function viewDashboard(isAdmin, isUserOnly, isPublic=false) {
  if(isPublic) currentUser = "";  // Reset for public
  showPage('dashboardPage');
  document.getElementById('dashTitle').innerText = isUserOnly ? "My Personal Records" : (isPublic ? "Public Dashboard" : "Records Dashboard");
  document.getElementById('actionCol').style.display = (isAdmin || isUserOnly) ? "table-cell" : "none";

  const tbody = document.getElementById('tableBody');
  tbody.innerHTML = "";

  bmiRecords.forEach((r, i) => {
    if(isUserOnly && r.owner !== currentUser) return;

    tbody.innerHTML += `<tr>
      <td>${r.date || 'N/A'}</td>
      <td>${r.surname}</td>
      <td>${r.section}</td>
      <td>${r.bmi}</td>
      <td>${r.category}</td>
      ${(isAdmin || (isUserOnly && r.owner === currentUser)) ? `<td>
        <button onclick="updateRec(${i}, ${isUserOnly})" style="background:orange; padding:5px; font-size:10px;">Edit</button>
        <button onclick="deleteRec(${i}, ${isUserOnly})" style="background:red; padding:5px; font-size:10px;">X</button>
      </td>` : ""}
    </tr>`;
  });
}

function updateRec(i, isUserView) {
  let r = bmiRecords[i];
  let nSec = prompt("Update Section:", r.section);
  let nH = prompt("Update Height (cm):", r.height);
  let nW = prompt("Update Weight (kg):", r.weight);
  
  if(nSec && nH && nW) {
    let nBmi = (parseFloat(nW) / ((parseFloat(nH)/100)**2)).toFixed(2);
    bmiRecords[i] = { ...r, section: nSec, height: nH, weight: nW, bmi: nBmi, category: getCategory(nBmi) };
    localStorage.setItem("bmiRecords", JSON.stringify(bmiRecords));
    viewDashboard(currentUser === "admin", isUserView);
  }
}

function deleteRec(i, isUserView) {
  if(confirm("Delete this entry?")) {
    bmiRecords.splice(i, 1);
    localStorage.setItem("bmiRecords", JSON.stringify(bmiRecords));
    viewDashboard(currentUser === "admin", isUserView);
  }
}

function showAnalytics() {
  showPage('analyticsPage');
  const userData = bmiRecords.filter(r => r.owner === currentUser);
  const labels = userData.map(r => r.date);
  const bmis = userData.map(r => r.bmi);

  const ctx = document.getElementById('bmiChart').getContext('2d');
  if(myChart) myChart.destroy();

  myChart = new Chart(ctx, {
    type: 'line',
    data: {
      labels: labels,
      datasets: [{
        label: 'My BMI Trend',
        data: bmis,
        borderColor: 'hotpink',
        backgroundColor: 'rgba(255, 105, 180, 0.2)',
        borderWidth: 3,
        pointBackgroundColor: 'hotpink',
        fill: true,
        tension: 0.3
      }]
    },
    options: {
      responsive: true,
      scales: { y: { suggestedMin: 15, suggestedMax: 35 } }
    }
  });
}

function logout() { currentUser = ""; showPage('frontPage'); }
</script>

</body>
</html>