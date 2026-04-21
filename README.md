<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Institutional Asset Management</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap" rel="stylesheet">
    <style>
        :root { --primary: #020617; --accent: #10B981; --card-bg: #0F172A; --text-main: #F8FAFC; --text-dim: #94A3B8; --gold: #F59E0B; --danger: #EF4444; --elite: #8B5CF6; }
        body { margin: 0; font-family: 'Outfit', sans-serif; background: var(--primary); color: var(--text-main); overflow-x: hidden; padding-bottom: 90px; }
        
        .glass-card { background: var(--card-bg); border: 1px solid rgba(255,255,255,0.05); border-radius: 28px; padding: 20px; margin: 15px; }
        .btn-prime { width: 100%; padding: 16px; background: var(--accent); color: white; border: none; border-radius: 18px; font-weight: 900; cursor: pointer; transition: 0.3s; }
        .btn-prime:active { transform: scale(0.95); }
        
        input, select { width: 100%; padding: 15px; background: #020617; border: 1px solid #334155; color: #fff; border-radius: 16px; box-sizing: border-box; margin-bottom: 12px; outline: none; font-family: 'Outfit'; }
        
        .app-header { padding: 15px 20px; background: rgba(15, 23, 42, 0.8); backdrop-filter: blur(20px); border-bottom: 1px solid rgba(255,255,255,0.05); display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; }
        .bottom-nav { position: fixed; bottom: 0; width: 100%; background: rgba(15, 23, 42, 0.95); display: flex; justify-content: space-around; padding: 15px 0; border-top: 1px solid #1e293b; z-index: 1000; }
        .nav-item { text-align: center; color: var(--text-dim); font-size: 10px; flex: 1; cursor: pointer; }
        .nav-item.active { color: var(--accent); }
        .nav-item i { font-size: 1.4rem; margin-bottom: 3px; }

        .screen { display: none; animation: slideUp 0.4s ease; }
        .screen.active { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } }

        .node-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; padding: 15px; }
        .node-box { background: var(--card-bg); border-radius: 20px; padding: 15px; border: 1px solid rgba(255,255,255,0.03); text-align: center; }
        .node-img { width: 100%; height: 80px; border-radius: 12px; object-fit: cover; margin-bottom: 8px; }

        #admin-panel { background: #000; min-height: 100vh; padding: 20px; display: none; color: white; position: fixed; inset: 0; z-index: 9999; overflow-y: auto; }
        .admin-section { background: #111; border-radius: 20px; padding: 15px; margin-bottom: 15px; border: 1px solid #222; }
    </style>
</head>
<body>

    <div id="auth-screen" style="min-height:100vh; display:flex; flex-direction:column; align-items:center; justify-content:center; background: radial-gradient(circle at top right, #1e1b4b, #020617); padding:20px;">
        <i class="fa-solid fa-gem" style="font-size: 4rem; color: var(--accent); margin-bottom: 20px;"></i>
        <div class="glass-card" style="width:100%; max-width:380px;">
            <h2 id="auth-title" style="text-align:center; margin:0 0 20px 0;">Login to Nova</h2>
            <input type="text" id="username" placeholder="Username">
            <input type="password" id="password" placeholder="Passkey">
            <button class="btn-prime" onclick="handleAuth()">LOGIN</button>
            <p onclick="toggleAuth()" id="toggleTxt" style="text-align:center; font-size:12px; color:var(--accent); cursor:pointer; margin-top:20px;">Create New Identity</p>
        </div>
    </div>

    <div id="app-main" style="display:none;">
        <div class="app-header">
            <div id="userDisp" style="background:var(--accent); color:white; padding:7px 16px; border-radius:12px; font-weight:900; font-size:11px;">OPERATOR</div>
            <div style="display:flex; gap:12px; align-items:center;">
                <button onclick="claimBonus()" style="background:var(--gold); border:none; border-radius:8px; padding:5px 10px; font-size:10px; font-weight:900; color:white;">DAILY BONUS</button>
                <i class="fa-solid fa-power-off" onclick="logout()" style="color:var(--danger); font-size:1.2rem; cursor:pointer;"></i>
            </div>
        </div>

        <div id="home" class="screen active">
            <div class="glass-card" style="background: linear-gradient(135deg, #10B981, #064e3b);">
                <small style="font-weight:900; opacity:0.8;">PORTFOLIO VALUE</small>
                <h1 style="font-size:3rem; margin:10px 0;" id="mainBal">$0.00</h1>
                <div style="display:grid; grid-template-columns:1fr 1fr; border-top:1px solid rgba(255,255,255,0.1); padding-top:10px;">
                    <div><small>DAILY</small><br><b id="dayYield">+$0.00</b></div>
                    <div><small>TOTAL</small><br><b id="totProfit">$0.00</b></div>
                </div>
            </div>
            <h3 style="padding:0 20px;">Cluster Nodes</h3>
            <div id="nodeGrid" class="node-grid"></div>
        </div>

        <div id="wallet" class="screen">
            <div class="glass-card">
                <h3>Deposit Assets</h3>
                <p style="font-size:10px; color:var(--text-dim); margin-bottom:10px;">Send USDT/TRX to: <b>TAvfQQ18hXHdVCHbExV4yUPvQ4XkNgfKsJ</b></p>
                <input type="number" id="depAmt" placeholder="Amount ($)">
                <input type="text" id="tid" placeholder="Transaction ID (TID)">
                <input type="file" id="proof" style="font-size:11px;" accept="image/*">
                <button class="btn-prime" onclick="submitDep()" style="margin-top:10px;">SUBMIT PROOF</button>
            </div>
            <h3 style="padding:0 20px;">Active Subscriptions</h3>
            <div id="history-list" style="padding:0 15px;"></div>
        </div>

        <div id="info" class="screen">
            <div class="glass-card">
                <h3>Nova Protocol</h3>
                <p style="font-size:12px; opacity:0.7; line-height:1.6;">Official registration SG-944. All assets are 100% insured by the Liquidity Protection Fund (LPF). Active nodes operate on a high-frequency 30-day cycle with automated daily yield distribution.</p>
            </div>
        </div>

        <div class="bottom-nav">
            <div class="nav-item active" onclick="navTo('home', this)"><i class="fa-solid fa-house"></i><br>HOME</div>
            <div class="nav-item" onclick="navTo('wallet', this)"><i class="fa-solid fa-clock-rotate-left"></i><br>HISTORY</div>
            <div class="nav-item" onclick="navTo('info', this)"><i class="fa-solid fa-shield-check"></i><br>TRUST</div>
        </div>
    </div>

    <div id="admin-panel">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:20px;">
            <h2>GOD MODE</h2>
            <button onclick="document.getElementById('admin-panel').style.display='none'" style="background:var(--danger); border:none; padding:10px 20px; color:white; border-radius:10px; font-weight:900;">CLOSE</button>
        </div>
        <div class="admin-section">
            <h4>Adjust User Assets</h4>
            <input id="adj-u" placeholder="Target Username">
            <input id="adj-a" type="number" placeholder="Amount (+ or -)">
            <button class="btn-prime" onclick="adjustBalance()">EXECUTE UPDATE</button>
        </div>
        <h4 style="margin-left:10px;">Pending Transactions</h4>
        <div id="adm-pending-list"></div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, collection, getDocs, updateDoc, addDoc, query, where, onSnapshot } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let currentUser = null; 
        let balance = 0; 
        let isLogin = true;

        window.toggleAuth = () => {
            isLogin = !isLogin;
            document.getElementById('auth-title').innerText = isLogin ? "Login to Nova" : "Register Identity";
            document.getElementById('toggleTxt').innerText = isLogin ? "Create New Identity" : "Already have account? Login";
        };

        window.handleAuth = async () => {
            const u = document.getElementById('username').value.toLowerCase().trim();
            const p = document.getElementById('password').value;
            if(!u || !p) return alert("Fill all fields!");
            
            const ref = doc(db, "users", u);
            const snap = await getDoc(ref);
            
            if(isLogin) {
                if(snap.exists() && snap.data().password === p) {
                    sessionStorage.setItem("nova_user", u);
                    init();
                } else alert("Invalid Access Credentials!");
            } else {
                if(snap.exists()) return alert("Operator ID already exists!");
                await setDoc(ref, { password: p, balance: 0, dailyProfit: 0, totalProfit: 0, lastBonus: "" });
                alert("Identity Registered! Now Login.");
                isLogin = true; toggleAuth();
            }
        };

        async function init() {
            currentUser = sessionStorage.getItem("nova_user");
            if(!currentUser) return;

            document.getElementById('auth-screen').style.display='none';
            document.getElementById('app-main').style.display='block';
            document.getElementById('userDisp').innerText = currentUser.toUpperCase();

            // Real-time listener for balance
            onSnapshot(doc(db, "users", currentUser), (doc) => {
                if (doc.exists()) {
                    const data = doc.data();
                    balance = data.balance || 0;
                    document.getElementById('mainBal').innerText = "$" + balance.toFixed(2);
                    document.getElementById('dayYield').innerText = "+$" + (data.dailyProfit || 0).toFixed(2);
                    document.getElementById('totProfit').innerText = "$" + (data.totalProfit || 0).toFixed(2);
                }
            });

            renderNodes();
            loadHistory();
        }

        function renderNodes() {
            const grid = document.getElementById('nodeGrid'); 
            grid.innerHTML = "";
            const images = [
                "1639762681485-074b7f938ba0", "1558494949-ef010cbdcc51", "1518770660439-4636190af475",
                "1550751827-4bd374c3f58b", "1640343136701-d051ff221245", "1607799279861-4dd421837ad1"
            ];

            for(let i=1; i<=20; i++) {
                const cost = i * 25;
                const daily = (cost * 0.08).toFixed(2);
                const randImg = images[i % images.length];
                grid.innerHTML += `
                <div class="node-box">
                    <img src="https://images.unsplash.com/photo-${randImg}?w=200" class="node-img">
                    <div style="font-size:11px; font-weight:900; color:var(--text-dim);">NOVA-NODE ${i}</div>
                    <div style="color:var(--accent); font-weight:900; margin:5px 0;">$${cost}</div>
                    <button onclick="buyNode(${cost}, ${daily})" style="width:100%; border:none; background:var(--accent); color:white; border-radius:10px; padding:8px; font-size:10px; font-weight:700; cursor:pointer;">ACTIVATE</button>
                </div>`;
            }
        }

        window.buyNode = async (cost, daily) => {
            if(balance < cost) return alert("Insufficient Balance! Please deposit assets.");
            if(confirm(`Activate Cluster for $${cost}?`)) {
                const exp = new Date(); exp.setDate(exp.getDate() + 30);
                await updateDoc(doc(db, "users", currentUser), { balance: balance - cost });
                await addDoc(collection(db, "active_nodes"), { 
                    user: currentUser, cost, daily: parseFloat(daily), expires: exp.toISOString(), status: "Active", time: Date.now()
                });
                alert("Protocol Activated Successfully!");
                loadHistory();
            }
        };

        window.loadHistory = async () => {
            const q = query(collection(db, "active_nodes"), where("user", "==", currentUser));
            const snap = await getDocs(q);
            const list = document.getElementById('history-list'); 
            list.innerHTML = "";
            snap.forEach(d => {
                const node = d.data();
                list.innerHTML += `
                <div class="glass-card" style="margin:10px 0; border-left:5px solid var(--accent); padding:15px;">
                    <div style="display:flex; justify-content:space-between;">
                        <b>NODE $${node.cost}</b>
                        <span style="color:var(--accent); font-size:10px;">${node.status}</span>
                    </div>
                    <p style="font-size:10px; margin:5px 0; color:var(--text-dim);">Daily Yield: $${node.daily} | Cycle: 30 Days</p>
                </div>`;
            });
        }

        window.claimBonus = async () => {
            const t = new Date().toDateString();
            const ref = doc(db, "users", currentUser);
            const snap = await getDoc(ref);
            if(snap.data().lastBonus === t) return alert("Bonus already claimed for today!");
            await updateDoc(ref, { balance: balance + 0.50, lastBonus: t });
            alert("Daily Bonus $0.50 Credited!");
        };

        window.submitDep = async () => {
            const amt = document.getElementById('depAmt').value;
            const tid = document.getElementById('tid').value;
            const file = document.getElementById('proof').files[0];
            if(!amt || !tid || !file) return alert("All details + proof image required!");

            const reader = new FileReader();
            reader.onloadend = async () => {
                await addDoc(collection(db, "transactions"), {
                    user: currentUser, amount: parseFloat(amt), tid, proof: reader.result, status: "Pending", time: Date.now()
                });
                alert("Proof Broadcasted! Awaiting manual audit.");
                document.getElementById('depAmt').value = ""; document.getElementById('tid').value = "";
            };
            reader.readAsDataURL(file);
        };

        window.navTo = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
            el.classList.add('active');
        };

        window.logout = () => { sessionStorage.clear(); location.reload(); };

        // --- ADMIN GOD MODE ---
        document.getElementById('userDisp').onclick = () => {
            window.ac = (window.ac || 0) + 1;
            if(window.ac === 5) {
                const p = prompt("ACCESS KEY:");
                if(p === "nova_boss_786") {
                    document.getElementById('admin-panel').style.display='block';
                    loadAdmin();
                } window.ac = 0;
            }
        };

        window.loadAdmin = () => {
            onSnapshot(collection(db, "transactions"), (snap) => {
                const list = document.getElementById('adm-pending-list');
                list.innerHTML = "";
                snap.forEach(d => {
                    const t = d.data();
                    if(t.status === "Pending") {
                        list.innerHTML += `
                        <div class="admin-section" style="font-size:12px;">
                            <p><b>User:</b> ${t.user} | <b>Amt:</b> $${t.amount}</p>
                            <p><b>TID:</b> ${t.tid}</p>
                            <img src="${t.proof}" style="width:100%; border-radius:10px; margin:10px 0;">
                            <div style="display:flex; gap:10px;">
                                <button onclick="admTx('${d.id}','${t.user}',${t.amount},'Approved')" style="background:var(--accent); flex:1; border:none; color:white; padding:10px; border-radius:8px;">APPROVE</button>
                                <button onclick="admTx('${d.id}','${t.user}',0,'Rejected')" style="background:var(--danger); flex:1; border:none; color:white; padding:10px; border-radius:8px;">REJECT</button>
                            </div>
                        </div>`;
                    }
                });
            });
        };

        window.admTx = async (docId, user, amt, status) => {
            await updateDoc(doc(db, "transactions", docId), { status });
            if(status === 'Approved') {
                const r = doc(db, "users", user);
                const s = await getDoc(r);
                await updateDoc(r, { balance: (s.data().balance || 0) + amt });
            }
            alert("Transaction " + status);
        };

        window.adjustBalance = async () => {
            const u = document.getElementById('adj-u').value.trim().toLowerCase();
            const a = parseFloat(document.getElementById('adj-a').value);
            const r = doc(db, "users", u);
            const s = await getDoc(r);
            if(s.exists()){ 
                await updateDoc(r, { balance: (s.data().balance || 0) + a }); 
                alert("Assets Synchronized!");
            } else alert("User not found!");
        };

        if(sessionStorage.getItem("nova_user")) init();
    </script>
</body>
</html>
