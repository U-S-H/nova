<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Coin Project - Professional Dashboard</title>
    <style>
        :root {
            --primary-color: #2563eb;
            --success-color: #22c55e;
            --danger-color: #ef4444;
            --bg-color: #f8fafc;
            --card-bg: #ffffff;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            margin: 0;
            padding-bottom: 70px; /* Space for bottom nav */
            color: #1e293b;
        }

        /* --- Header & Balance Section --- */
        .header {
            background: linear-gradient(135deg, var(--primary-color), #1d4ed8);
            color: white;
            padding: 30px 20px;
            text-align: center;
            border-radius: 0 0 25px 25px;
        }

        .balance-card h1 { font-size: 2.5rem; margin: 10px 0; }
        .stats-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            padding: 20px;
            margin-top: -30px;
        }

        .stat-box {
            background: var(--card-bg);
            padding: 15px;
            border-radius: 15px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.05);
            text-align: center;
        }

        /* --- Admin & User Sections --- */
        .section-title { padding: 0 20px; margin-top: 25px; font-size: 1.1rem; font-weight: bold; }
        
        .card {
            background: var(--card-bg);
            margin: 10px 20px;
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.02);
            border: 1px solid #e2e8f0;
        }

        /* --- Professional Nodes --- */
        .node-container { display: flex; overflow-x: auto; padding: 10px 20px; gap: 15px; }
        .node-card {
            min-width: 140px;
            background: var(--card-bg);
            padding: 15px;
            border-radius: 12px;
            text-align: center;
            border: 1px solid #e2e8f0;
        }

        /* --- Buttons --- */
        .btn {
            width: 100%;
            padding: 12px;
            border: none;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            margin-top: 10px;
        }
        .btn-deposit { background: var(--primary-color); color: white; }
        .btn-reject { background: var(--danger-color); color: white; }

        /* --- Bottom Navigation (Fixed Logout) --- */
        .nav-bar {
            position: fixed;
            bottom: 0;
            width: 100%;
            background: white;
            display: flex;
            justify-content: space-around;
            padding: 15px 0;
            box-shadow: 0 -2px 10px rgba(0,0,0,0.05);
            z-index: 1000;
        }
        .nav-item { font-size: 1.5rem; cursor: pointer; color: #64748b; }
        .nav-item.active { color: var(--primary-color); }

        /* --- Timer & Promo --- */
        .countdown { color: var(--danger-color); font-weight: bold; }
    </style>
</head>
<body>

    <div class="header">
        <p>Total Balance</p>
        <h1 id="user-balance">$5,240.00</h1>
        <p>Username: <span id="display-username">User_786</span></p>
    </div>

    <div class="stats-grid">
        <div class="stat-box">
            <small>Daily Profit</small>
            <div style="color: var(--success-color); font-weight: bold;">+$12.50</div>
        </div>
        <div class="stat-box">
            <small>Total Profit</small>
            <div style="font-weight: bold;">$450.00</div>
        </div>
    </div>

    <div class="section-title">Active Nodes</div>
    <div class="node-container">
        <div class="node-card">🔌<br><b>Micro</b></div>
        <div class="node-card">🏭<br><b>Industrial</b></div>
        <div class="node-card">🚀<br><b>Pro V3</b></div>
    </div>

    <div class="section-title">Update Deposit</div>
    <div class="card">
        <input type="number" id="deposit-amount" placeholder="Enter Amount" style="width:90%; padding:10px; border:1px solid #ddd; border-radius:5px;">
        <button class="btn btn-deposit" onclick="updateDeposit()">Update System</button>
    </div>

    <div class="section-title">Referral & Promo</div>
    <div class="card">
        <p><small>Your Link:</small><br><code id="ref-link">https://coin.io/ref/user786</code></p>
        <p>Next Reward in: <span class="countdown" id="timer">05:59:00</span></p>
        <input type="text" placeholder="Promo Code" style="width:60%; padding:8px; border:1px solid #ddd; border-radius:5px;">
        <button style="padding:8px; background:#333; color:white; border-radius:5px; border:none;">Apply</button>
    </div>

    <div class="section-title" style="color: var(--danger-color);">Admin Control</div>
    <div class="card" style="border: 1px solid var(--danger-color);">
        <p>Pending Request #882</p>
        <button class="btn btn-reject" onclick="rejectAction()">REJECT REQUEST</button>
        <button class="btn" style="background:#eee; margin-top:5px;" onclick="sendNotification()">Send Notification</button>
    </div>

    <div class="nav-bar">
        <div class="nav-item active">🏠</div>
        <div class="nav-item">📊</div>
        <div class="nav-item">👥</div>
        <div class="nav-item" title="Settings">⚙️</div>
        <div class="nav-item" onclick="handleLogout()" title="Logout" style="color: var(--danger-color);">🔒</div>
    </div>

    <script>
        // Logout Logic
        function handleLogout() {
            if(confirm("Sweetie, are you sure you want to logout?")) {
                alert("Logging out...");
                // Clear sessions here
                window.location.reload();
            }
        }

        // Reject Logic
        function rejectAction() {
            alert("Request Rejected. Notification sent to user.");
        }

        // Deposit Logic
        function updateDeposit() {
            const amount = document.getElementById('deposit-amount').value;
            if(amount) {
                alert("Deposit system updated for: $" + amount);
            }
        }

        // Simple Countdown
        let time = 360; // minutes
        setInterval(() => {
            if(time > 0) time--;
            // Timer formatting logic can be added here
        }, 1000);

        function sendNotification() {
            alert("Admin Notification: Message sent to all team members!");
        }
    </script>
</body>
</html>
