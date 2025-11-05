# Onlineconsultation
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Online Medical System — Virtual Consultations</title>
  <style>
    :root {
      --primary: #0077ff;
      --secondary: #0054b3;
      --bg: #f1f5f9;
      --card: #ffffff;
      --text: #111827;
      --muted: #6b7280;
      --success: #16a34a;
      --danger: #dc2626;
    }

    [data-theme="dark"] {
      --primary: #60a5fa;
      --secondary: #1d4ed8;
      --bg: #0f172a;
      --card: #1e293b;
      --text: #e2e8f0;
      --muted: #94a3b8;
    }

    * {
      box-sizing: border-box;
    }

    body {
      font-family: "Segoe UI", sans-serif;
      margin: 0;
      background: var(--bg);
      color: var(--text);
      transition: background 0.3s, color 0.3s;
    }

    header {
      background: var(--primary);
      color: white;
      padding: 25px;
      text-align: center;
    }

    nav {
      background: var(--secondary);
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      padding: 10px;
      gap: 10px;
    }

    nav a {
      color: white;
      text-decoration: none;
      padding: 8px 16px;
      border-radius: 6px;
      transition: background 0.3s;
    }

    nav a.active, nav a:hover {
      background: rgba(255, 255, 255, 0.25);
    }

    main {
      padding: 25px;
      display: grid;
      gap: 25px;
      max-width: 950px;
      margin: auto;
    }

    .card {
      background: var(--card);
      border-radius: 12px;
      padding: 20px;
      box-shadow: 0 3px 10px rgba(0,0,0,0.1);
    }

    h2 {
      color: var(--primary);
      margin-top: 0;
    }

    form {
      display: grid;
      gap: 12px;
      margin-top: 10px;
    }

    input, select, textarea, button {
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 6px;
      font-size: 1em;
      width: 100%;
    }

    button {
      background: var(--primary);
      color: white;
      border: none;
      cursor: pointer;
      font-weight: 600;
      transition: background 0.3s;
    }

    button:hover {
      background: var(--secondary);
    }

    .btn-emergency {
      background: var(--danger);
      font-weight: bold;
    }

    .btn-premium {
      background: var(--success);
    }

    .dark-toggle {
      position: fixed;
      top: 15px;
      right: 15px;
      background: var(--card);
      color: var(--text);
      border: none;
      padding: 8px 14px;
      border-radius: 20px;
      cursor: pointer;
      box-shadow: 0 2px 6px rgba(0,0,0,0.2);
    }

    footer {
      text-align: center;
      background: var(--primary);
      color: white;
      padding: 10px;
      margin-top: 40px;
    }

    /* Modal */
    .modal {
      display: none;
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,0.6);
      justify-content: center;
      align-items: center;
      z-index: 999;
    }

    .modal-content {
      background: var(--card);
      padding: 25px;
      border-radius: 12px;
      width: 90%;
      max-width: 500px;
      animation: pop 0.3s ease;
    }

    @keyframes pop {
      from {transform: scale(0.9); opacity: 0;}
      to {transform: scale(1); opacity: 1;}
    }

    .close {
      float: right;
      font-size: 20px;
      cursor: pointer;
    }
  </style>
</head>

<body>
  <header>
    <h1>Online Medical System — Virtual Consultations</h1>
    <p>Book appointments, access e-prescriptions, and get instant care</p>
  </header>

  <button class="dark-toggle" id="themeToggle">🌙 Dark Mode</button>

  <nav>
    <a href="#admin">Admin</a>
    <a href="#doctor">Doctor</a>
    <a href="#patient">Patient</a>
    <a href="#pharmacist">Pharmacist</a>
    <a href="#premium">Premium</a>
  </nav>

  <main>
    <!-- Admin -->
    <section id="admin" class="card">
      <h2>Admin Panel</h2>
      <p>Manage platform settings, oversee users, and ensure system security.</p>
      <form>
        <label>Search User:</label>
        <input type="text" id="searchUser" placeholder="Enter user name or ID">
        <button type="button" onclick="searchUser()">Search</button>
      </form>
      <p id="adminResult" style="color:var(--muted)"></p>
    </section>

    <!-- Doctor -->
    <section id="doctor" class="card">
      <h2>Doctor Dashboard</h2>
      <p>Conduct virtual consultations, manage e-prescriptions, and access records.</p>
      <button onclick="openModal('consultationModal')">🩺 Start Consultation</button>
      <button onclick="openVideo()">🎥 Video Consultation</button>
    </section>

    <!-- Patient -->
    <section id="patient" class="card">
      <h2>Patient Portal</h2>
      <p>Book virtual appointments, view records, or request urgent care.</p>
      <form id="appointmentForm">
        <label>Select Doctor:</label>
        <select id="doctorSelect">
          <option>Dr. Smith — Cardiologist</option>
          <option>Dr. Johnson — Dermatologist</option>
          <option>Dr. Patel — General Physician</option>
        </select>

        <label>Date & Time:</label>
        <input type="datetime-local" id="appointmentDate">

        <button type="submit">Book Appointment</button>
        <button type="button" class="btn-emergency" onclick="emergencyCall()">🚨 Emergency Service</button>
      </form>
    </section>

    <!-- Pharmacist -->
    <section id="pharmacist" class="card">
      <h2>Pharmacist Panel</h2>
      <p>Manage e-prescriptions, track medicine orders, and verify requests.</p>
      <form>
        <label>Prescription ID:</label>
        <input type="text" id="prescriptionId" placeholder="Enter prescription ID">
        <label>Status:</label>
        <select id="orderStatus">
          <option>Pending</option>
          <option>Processing</option>
          <option>Delivered</option>
        </select>
        <button type="button" onclick="updateOrder()">Update</button>
      </form>
    </section>

    <!-- Premium Features -->
    <section id="premium" class="card">
      <h2>Premium Membership</h2>
      <p>Unlock 24/7 doctor access, instant video consultations, and priority booking.</p>
      <ul>
        <li>🕒 24/7 doctor availability</li>
        <li>💬 Chat with specialists anytime</li>
        <li>⚡ Priority scheduling & faster reports</li>
      </ul>
      <button class="btn-premium" onclick="upgrade()">Upgrade to Premium</button>
    </section>
  </main>

  <!-- Doctor Modal -->
  <div id="consultationModal" class="modal">
    <div class="modal-content">
      <span class="close" onclick="closeModal('consultationModal')">&times;</span>
      <h3>Virtual Consultation</h3>
      <form id="consultForm">
        <label>Patient Name:</label>
        <input type="text" id="patientName" required>
        <label>Diagnosis:</label>
        <textarea id="diagnosis" required></textarea>
        <label>E-Prescription:</label>
        <textarea id="prescription" required></textarea>
        <button type="submit">Save & Send</button>
      </form>
    </div>
  </div>

  <footer>
    <p>© 2025 Online Medical System | Designed by Lokesh</p>
  </footer>

  <script>
    // Theme Toggle
    const themeToggle = document.getElementById('themeToggle');
    themeToggle.addEventListener('click', () => {
      const dark = document.body.getAttribute('data-theme');
      if (dark) {
        document.body.removeAttribute('data-theme');
        themeToggle.textContent = '🌙 Dark Mode';
      } else {
        document.body.setAttribute('data-theme', 'dark');
        themeToggle.textContent = '☀️ Light Mode';
      }
    });

    // Admin search simulation
    function searchUser() {
      const name = document.getElementById('searchUser').value.trim();
      const result = document.getElementById('adminResult');
      if (name) result.textContent = `User "${name}" found ✅ (Active)`;
      else result.textContent = "Enter a valid user name.";
    }

    // Appointment Booking
    document.getElementById("appointmentForm").addEventListener("submit", (e) => {
      e.preventDefault();
      const doctor = document.getElementById("doctorSelect").value;
      const date = document.getElementById("appointmentDate").value;
      if (!date) return alert("Please select a valid date and time!");
      alert(`✅ Appointment booked with ${doctor} on ${date}`);
    });

    // Emergency Service
    function emergencyCall() {
      alert("🚨 Emergency request sent! Nearest hospital and ambulance notified.");
    }

    // Premium Upgrade
    function upgrade() {
      alert("🌟 Premium Activated! You now have 24/7 doctor access and instant booking.");
    }

    // Pharmacist order update
    function updateOrder() {
      const id = document.getElementById("prescriptionId").value;
      const status = document.getElementById("orderStatus").value;
      if (!id) return alert("Enter a valid Prescription ID!");
      alert(`Prescription ${id} updated to "${status}"`);
    }

    // Video Consultation Simulation
    function openVideo() {
      window.open("https://meet.google.com", "_blank");
      alert("🎥 Opening secure video consultation in new tab...");
    }

    // Modal Controls
    function openModal(id) {
      document.getElementById(id).style.display = "flex";
    }
    function closeModal(id) {
      document.getElementById(id).style.display = "none";
    }

    document.getElementById("consultForm").addEventListener("submit", (e) => {
      e.preventDefault();
      const name = document.getElementById("patientName").value;
      closeModal('consultationModal');
      alert(`🩺 Consultation saved for ${name}. E-prescription sent.`);
    });
  </script>
</body>
</html>
