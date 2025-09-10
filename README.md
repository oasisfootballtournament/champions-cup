<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Oasis Champions Cup 2025</title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      margin: 0; padding: 0;
      background: linear-gradient(135deg, #1e3c72, #2a5298);
      color: #333;
    }
    header {
      text-align: center;
      padding: 40px 20px;
      color: #fff;
    }
    header h1 {
      font-size: 2.5rem;
      margin-bottom: 10px;
    }
    header p {
      font-size: 1.2rem;
    }
    main {
      max-width: 1000px;
      margin: 30px auto;
      background: #fff;
      border-radius: 12px;
      box-shadow: 0 6px 18px rgba(0,0,0,0.2);
      padding: 30px;
    }
    h2 {
      color: #1e3c72;
      border-bottom: 2px solid #ddd;
      padding-bottom: 8px;
      margin-top: 30px;
    }
    ul {
      line-height: 1.8;
      font-size: 1.05rem;
    }
    form {
      margin-top: 20px;
      display: flex;
      flex-direction: column;
    }
    input, button {
      margin: 8px 0;
      padding: 12px;
      font-size: 1rem;
      border-radius: 6px;
      border: 1px solid #ccc;
    }
    button {
      background: #1e3c72;
      color: #fff;
      border: none;
      cursor: pointer;
      transition: 0.3s;
    }
    button:hover {
      background: #2a5298;
    }
    .qr-container {
      text-align: center;
      margin-top: 20px;
    }
    #qrcode {
      margin-top: 10px;
    }
    .admin-section {
      margin-top: 50px;
      padding: 20px;
      border: 1px solid #ddd;
      border-radius: 8px;
      background: #f9f9f9;
    }
    .suspicious { color: red; font-weight: bold; }
    .valid { color: green; font-weight: bold; }
    footer {
      text-align: center;
      padding: 20px;
      margin-top: 30px;
      color: #fff;
      background: rgba(0,0,0,0.2);
    }
  </style>
</head>
<body>

<header>
  <h1>🏆 Oasis Champions Cup 2025 🏆</h1>
  <p>September 20–21, 2025 • Near Mathamangalam Govt. School</p>
</header>

<main>
  <h2>Eligibility</h2>
  <ul>
    <li>Participants must be born on or after <b>January 1, 2011</b>.</li>
    <li>Valid ID required at registration.</li>
    <li>Open to all teams (boys & girls).</li>
  </ul>

  <h2>Prizes</h2>
  <ul>
    <li>🥇 First Prize: ₹1800 + Trophy</li>
    <li>🥈 Runner-up: ₹800 + Trophy</li>
    <li>💰 Entry Fee: ₹300 per team</li>
  </ul>

  <h2>Register Your Team</h2>
  <form id="regForm">
    <input type="text" id="teamName" placeholder="Team Name" required>
    <input type="text" id="captainName" placeholder="Captain Name" required>
    <input type="number" id="players" placeholder="Number of Players (5-15)" min="5" max="15" required>
    <button type="submit">Register Team</button>
  </form>

  <div class="qr-container">
    <h2>Scan to Visit Website</h2>
    <canvas id="qrcode"></canvas>
  </div>

  <div class="admin-section">
    <h2>Admin Login</h2>
    <input type="password" id="adminPass" placeholder="Enter Password">
    <button onclick="checkAdmin()">Login</button>

    <div id="adminPanel" class="hidden">
      <h3>All Registrations</h3>
      <ul id="regList"></ul>
    </div>
  </div>
</main>

<footer>
  <p>© 2025 Oasis Mathamangalam • Organized by Oasis Jr Committee</p>
</footer>

<script src="https://cdnjs.cloudflare.com/ajax/libs/qrious/4.0.2/qrious.min.js"></script>
<script>
  // Generate QR Code
  const websiteURL = window.location.href;
  new QRious({
    element: document.getElementById("qrcode"),
    value: websiteURL,
    size: 200
  });

  // Load registrations
  function loadRegistrations() {
    const regList = document.getElementById("regList");
    regList.innerHTML = "";
    const regs = JSON.parse(localStorage.getItem("registrations")) || [];
    regs.forEach((r, i) => {
      const li = document.createElement("li");
      li.innerHTML = `${i+1}. <b>${r.team}</b> (Captain: ${r.captain}, Players: ${r.players}) 
        - <span class="${r.status === 'Suspicious' ? 'suspicious' : 'valid'}">${r.status}</span>`;
      regList.appendChild(li);
    });
  }

  // AI-like check for suspicious entries
  function isSuspicious(team, captain, players) {
    if (team.length < 3) return true;
    if (/[^a-zA-Z\s]/.test(captain)) return true;
    if (players < 5 || players > 15) return true;

    const regs = JSON.parse(localStorage.getItem("registrations")) || [];
    if (regs.some(r => r.team.toLowerCase() === team.toLowerCase())) return true;

    return false;
  }

  // Handle registration form
  document.getElementById("regForm").addEventListener("submit", e => {
    e.preventDefault();
    const team = document.getElementById("teamName").value.trim();
    const captain = document.getElementById("captainName").value.trim();
    const players = parseInt(document.getElementById("players").value);

    let status = isSuspicious(team, captain, players) ? "Suspicious" : "Valid";

    const regs = JSON.parse(localStorage.getItem("registrations")) || [];
    regs.push({ team, captain, players, status });
    localStorage.setItem("registrations", JSON.stringify(regs));

    alert(status === "Valid" ? "✅ Team registered successfully!" : "⚠️ Registration flagged as suspicious!");
    document.getElementById("regForm").reset();

    if (!document.getElementById("adminPanel").classList.contains("hidden")) {
      loadRegistrations();
    }
  });

  // Admin login
  function checkAdmin() {
    const pass = document.getElementById("adminPass").value;
    if (pass === "occ2k25") {
      document.getElementById("adminPanel").classList.remove("hidden");
      loadRegistrations();
    } else {
      alert("❌ Wrong password!");
    }
  }
</script>

</body>
</html>
