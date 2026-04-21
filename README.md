<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Institutional Terminal</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --accent: #00FFA3; --bg: #050505; --card: #0f0f0f; --text: #FFFFFF; --danger: #ff4d4d; }
        body { margin: 0; font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: var(--text); overflow-x: hidden; -webkit-tap-highlight-color: transparent; }
        .glass { background: rgba(15, 15, 15, 0.98); border: 1px solid rgba(212, 175, 55, 0.2); border-radius: 30px; padding: 25px; backdrop-filter: blur(20px); }
        .input-nova { width: 100%; padding: 18px; background: #000; border: 1px solid #222; color: #fff; border-radius: 18px; margin-bottom: 12px; outline: none; box-sizing: border-box; }
        .btn-nova { background: linear-gradient(135deg, var(--gold), #F2CC60); color: #000; border: none; padding: 18px; border-radius: 18px; font-weight: 800; width: 100%; cursor: pointer; }
        
        #auth-screen { height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center; padding: 30px; }
        .screen { display: none; padding-bottom: 120px; animation: fadeIn 0.3s; }
        .active-screen { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        .navbar { position: fixed; bottom: 0; width: 100%; background: #000; display: flex; justify-content: space-around; padding: 15px 0; border-top: 1px solid #222; }
        .nav-link { color: #555; text-decoration: none; font-size: 10px; text-align: center; }
        .nav-link.active { color: var(--gold); }

        #admin-panel { display: none; position: fixed; inset: 0; background: #000; z-index: 9999; padding: 20px; overflow-y: auto; }
        .adm-card { background: #111; border: 1px solid var(--gold); border-radius: 20px; padding: 15px; margin-bottom: 10px; }
        .history-item { background: #111; padding: 15px; border-radius: 15px; margin-bottom: 10px; border-left: 4px solid var(--gold); }
    </style>
</head>
<body>

    <div id="auth-screen">
        <h1 id="logo-trigger" onclick="handleLogoTaps()" style="font-size:3rem; font-weight:900;">NOVA<span style="color:var(--gold)">.</span></h1>
        <div class="glass" style="width:100%; max-width:400px;">
            <input type="text" id="u-id" placeholder="Account ID" class="input-nova">
            <input type="password" id="u-pass" placeholder="Passkey" class="input-nova">
            <input type="text" id="admin-key" style="display:none; border-color:var(--gold);" placeholder="Admin Key" class="input-nova">
            <button class="btn-nova" onclick="processAuth()">ENTER</button>
            <p onclick="toggleReg()" id="toggle-text" style="text-align:center; font-size:12px; margin-top:15px; color:var(--gold);">New? Create Account</p>
        </div>
    </div>

    <div id="admin-panel">
        <div style="display:flex; justify-content:space-between;">
            <h2 style="color:var(--gold);">ADMIN MODE</h2>
            <button onclick="location.reload()" style="background:var(--danger); color:white; border:none; border-radius:10px; padding:0 15px;">EXIT</button>
        </div>
        <div id="admin-requests"></div>
    </div>

    <div id="app-main" style="display:none;">
        <header style="padding:20px; display:flex; justify-content:space-between;">
            <div style="font-weight:800;">ID: <span id="user-display">...</span></div>
            <div onclick="secureLogout()"><i class="fa-solid fa-power-off" style="color:var(--danger);"></i></div>
        </header>

        <div id="dash" class="screen active-screen">
            <div style="padding:15px;">
                <div class="glass">
                    <small style="color:#666;">BALANCE</small>
                    <h1 id="bal-main">$0.00</h1>
                    <div style="display:flex; gap:20px;">
                        <div><small>PROFIT</small><br><b id="v-daily" style="color:var(--accent);">+$0.00</b></div>
                        <div><small>TOTAL</small><br><b id="v-total">$0.00</b></div>
                    </div>
                </div>
            </div>
        </div>

        <div id="finance" class="screen">
            <div style="padding:15px;">
                <div class="glass">
                    <h3>FINANCE HUB</h3>
                    <input type="number" id="f-amt" placeholder="Amount ($)" class="input-nova">
                    <button class="btn-nova" onclick="submitFin('Deposit')">DEPOSIT</button>
                    <button class="btn-nova" style="background:#222; color:white; margin-top:10px;" onclick="submitFin('Withdrawal')">WITHDRAW</button>
                </div>
                <h4 style="margin-top:20px;">ACTIVITY HISTORY</h4>
                <div id="history-list"></div>
            </div>
        </div>
    </div>

    <nav class="navbar" id="nav-bar" style="display:none;">
        <a href="#" class="nav-link active" onclick="nav('dash', this)">DASHBOARD</a>
        <a href="#" class="nav-link" onclick="nav('finance', this)">FINANCE</a>
    </nav>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo",
            authDomain: "home-94d45.firebaseapp.com",
            projectId: "home-94d45",
            appId: "1:964390949419:web:589840cb91b7e42ecd506e"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let user = null; let isReg = false; let taps = 0;

        window.handleLogoTaps = () => { taps++; if(taps >= 6) document.getElementById('admin-key').style.display='block'; };
        window.toggleReg = () => { isReg = !isReg; document.getElementById('toggle-text').innerText = isReg ? "Back to Login" : "New? Create Account"; };

        window.processAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            const ak = document.getElementById('admin-key').value;

            if(ak === "nov786") { document.getElementById('admin-panel').style.display='block'; loadAdmin(); return; }
            if(!id || !pw) return alert("Fill all fields");

            const ref = doc(db, "users", id);
            const snap = await getDoc(ref);

            if(isReg) {
                if(snap.exists()) return alert("User already exists");
                await setDoc(ref, { password: pw, balance: 0, daily: 0, total: 0 });
                boot(id);
            } else {
                if(snap.exists() && snap.data().password === pw) boot(id);
                else alert("Invalid login");
            }
        };

        function boot(id) {
            user = id; sessionStorage.setItem("nova_u", id);
            document.getElementById('auth-screen').style.display='none';
            document.getElementById('app-main').style.display='block';
            document.getElementById('nav-bar').style.display='flex';
            document.getElementById('user-display').innerText = id;

            onSnapshot(doc(db, "users", id), (d) => {
                const data = d.data();
                document.getElementById('bal-main').innerText = "$" + data.balance.toFixed(2);
                document.getElementById('v-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
            });
            loadHistory();
        }

        window.submitFin = async (type) => {
            const amt = document.getElementById('f-amt').value;
            if(!amt) return;
            await addDoc(collection(db, "logs"), { user, amount: parseFloat(amt), type, status: 'Pending', time: Date.now() });
            alert(type + " requested!");
        };

        function loadHistory() {
            const q = query(collection(db, "logs"), where("user", "==", user), orderBy("time", "desc"));
            onSnapshot(q, (snap) => {
                const l = document.getElementById('history-list'); l.innerHTML = "";
                snap.forEach(d => {
                    const t = d.data();
                    l.innerHTML += `<div class="history-item"><b>${t.type}</b>: $${t.amount} <span style="float:right">${t.status}</span></div>`;
                });
            });
        }

        function loadAdmin() {
            const q = query(collection(db, "logs"), where("status", "==", "Pending"), orderBy("time", "asc"));
            onSnapshot(q, (snap) => {
                const l = document.getElementById('admin-requests'); l.innerHTML = "";
                snap.forEach(d => {
                    const r = d.data();
                    l.innerHTML += `<div class="adm-card">
                        <p>${r.user} | ${r.type}: $${r.amount}</p>
                        <button onclick="adminAction('${d.id}','${r.user}',${r.amount},'${r.type}','Verified')" style="background:var(--accent); border:none; padding:10px; border-radius:10px;">APPROVE</button>
                        <button onclick="adminAction('${d.id}','${r.user}',${r.amount},'${r.type}','Rejected')" style="background:var(--danger); color:white; border:none; padding:10px; border-radius:10px;">REJECT</button>
                    </div>`;
                });
            });
        }

        window.adminAction = async (id, u, amt, t, st) => {
            if(st === 'Verified') {
                const r = doc(db, "users", u); const s = await getDoc(r);
                let nb = t === 'Deposit' ? (s.data().balance + amt) : (s.data().balance - amt);
                await updateDoc(r, { balance: nb });
            }
            await updateDoc(doc(db, "logs", id), { status: st });
        };

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.style.display='none');
            document.getElementById(id).style.display='block';
            document.querySelectorAll('.nav-link').forEach(l => l.classList.remove('active'));
            el.classList.add('active');
        };

        window.secureLogout = () => { sessionStorage.clear(); location.reload(); };
        if(sessionStorage.getItem("nova_u")) boot(sessionStorage.getItem("nova_u"));
    </script>
</body>
</html>
