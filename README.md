<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | ALL-IN-ONE TERMINAL</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0F172A; }
        body { background: #F8FAFC; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; -webkit-tap-highlight-color: transparent; }
        .glass-card { background: white; border: 1px solid #e2e8f0; border-radius: 32px; box-shadow: 0 4px 25px rgba(0,0,0,0.02); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 18px; font-weight: 800; cursor: pointer; border: none; transition: 0.3s; }
        .screen { display: none; min-height: 100vh; width: 100%; padding-bottom: 130px; }
        .active-screen { display: block; animation: fadeIn 0.4s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 20px; left: 5%; width: 90%; background: #0F172A; border-radius: 30px; display: flex; justify-content: space-around; padding: 18px 0; z-index: 1000; box-shadow: 0 15px 35px rgba(0,0,0,0.2); }
        .nav-item { color: #64748b; font-size: 8px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 5px; text-transform: uppercase; }
        .nav-item.active { color: var(--gold); }
        .ticker-wrap { background: #0F172A; color: var(--gold); padding: 8px 0; font-size: 10px; font-weight: 900; overflow: hidden; border-bottom: 2px solid var(--gold); }
        .ticker-move { display: inline-block; white-space: nowrap; animation: ticker 35s linear infinite; }
        @keyframes ticker { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }
        .history-item { border-bottom: 1px solid #f1f5f9; padding: 15px 0; }
        .node-img { width: 100%; height: 160px; object-fit: cover; border-radius: 24px; margin-bottom: 15px; }
    </style>
</head>
<body>

    <div id="auth-screen" class="active-screen min-h-screen flex flex-col justify-center p-10 bg-white">
        <div class="text-center mb-12">
            <h1 class="text-7xl font-black italic tracking-tighter">NO<span class="text-gold">VA</span></h1>
            <p class="text-[10px] font-bold text-slate-400 tracking-[0.6em] mt-3 uppercase">Global Infrastructure</p>
        </div>
        <div class="space-y-4 max-w-sm mx-auto w-full">
            <input type="text" id="u-id" placeholder="Username" class="w-full p-5 rounded-3xl bg-slate-50 border-none font-bold outline-none">
            <input type="password" id="u-pw" placeholder="Password" class="w-full p-5 rounded-3xl bg-slate-50 border-none font-bold outline-none">
            <input type="text" id="u-ref" placeholder="Referral Code (Optional)" class="w-full p-5 rounded-3xl bg-slate-50 border-none font-bold text-xs">
            <button onclick="handleAuth()" class="w-full btn-gold py-5 shadow-2xl text-xs tracking-widest uppercase">Sync Terminal</button>
            <p onclick="toggleAuth()" id="auth-btn-text" class="text-[10px] text-center text-slate-400 font-black uppercase cursor-pointer underline">New User? Register</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <div class="ticker-wrap"><div class="ticker-move">SYSTEM: ONLINE • ALL WITHDRAWALS AUTOMATED • NEW QUANTUM NODES DEPLOYED • WORLDWIDE ACCESS ENABLED • </div></div>
        
        <header class="p-6 flex justify-between items-center bg-white sticky top-0 z-[100] border-b">
            <div class="flex items-center gap-4">
                <h2 id="logo-main" class="font-black text-2xl italic tracking-tighter cursor-pointer">NO<span class="text-gold">VA</span></h2>
                <i onclick="logout()" class="fa-solid fa-power-off text-red-500 bg-red-50 p-3 rounded-full text-sm"></i>
            </div>
            <div class="text-right">
                <p id="user-tag" class="text-[8px] font-black uppercase text-slate-400">@USER</p>
                <p id="b-header" class="text-xl font-black text-slate-950 tracking-tighter">$0.00</p>
            </div>
        </header>

        <div id="scr-dash" class="screen active-screen px-6 pt-6">
            <div class="bg-slate-950 rounded-[45px] p-10 text-white mb-8 shadow-2xl border-b-[10px] border-gold relative">
                <p class="text-[9px] text-slate-500 uppercase font-black">Current Liquidity</p>
                <h1 id="b-main" class="text-5xl font-black mb-10 tracking-tighter">$0.00</h1>
                <div class="flex justify-between items-end border-t border-white/10 pt-6">
                    <div><p class="text-[8px] text-slate-500 uppercase font-black">Daily Profit</p><p id="b-daily" class="text-2xl font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[8px] text-slate-500 uppercase font-black">Total Payout</p><p id="b-total-p" class="text-2xl font-black text-gold">$0.00</p></div>
                </div>
            </div>

            <div class="grid grid-cols-3 gap-4 mb-8">
                <div onclick="openModal('mod-dep')" class="glass-card p-6 text-center"><i class="fa-solid fa-plus text-gold text-2xl mb-2"></i><p class="text-[8px] font-black uppercase">Deposit</p></div>
                <div onclick="openModal('mod-wd')" class="glass-card p-6 text-center"><i class="fa-solid fa-arrow-up text-slate-400 text-2xl mb-2"></i><p class="text-[8px] font-black uppercase">Withdraw</p></div>
                <div onclick="claimDaily()" class="glass-card p-6 text-center"><i class="fa-solid fa-gift text-blue-500 text-2xl mb-2"></i><p class="text-[8px] font-black uppercase">Bonus</p></div>
            </div>

            <div class="space-y-4">
                <div onclick="nav('scr-hist')" class="glass-card p-6 flex justify-between items-center"><div class="flex items-center gap-4"><i class="fa-solid fa-receipt text-gold"></i><span class="text-xs font-black uppercase">Transaction History</span></div><i class="fa-solid fa-chevron-right text-slate-200"></i></div>
                <div onclick="nav('scr-about')" class="glass-card p-6 flex justify-between items-center"><div class="flex items-center gap-4"><i class="fa-solid fa-building text-gold"></i><span class="text-xs font-black uppercase">About Company</span></div><i class="fa-solid fa-chevron-right text-slate-200"></i></div>
            </div>
        </div>

        <div id="scr-nodes" class="screen px-6 pt-8">
            <h3 class="font-black text-2xl mb-8 uppercase italic">Mining Nodes</h3>
            <div id="nodes-grid" class="space-y-8"></div>
        </div>

        <div id="scr-team" class="screen px-6 pt-8">
            <h3 class="font-black text-2xl mb-8 uppercase italic">My Network</h3>
            <div class="grid grid-cols-2 gap-4 mb-8">
                <div class="glass-card p-8 bg-slate-900 text-white text-center"><h5 id="team-count" class="text-4xl font-black text-gold">0</h5><p class="text-[8px] font-black uppercase mt-2">Team Size</p></div>
                <div class="glass-card p-8 text-center"><h5 id="team-comm" class="text-4xl font-black">$0</h5><p class="text-[8px] font-black text-slate-400 uppercase mt-2">Team Profit</p></div>
            </div>
            <div id="team-list" class="space-y-3"></div>
        </div>

        <div id="scr-hist" class="screen px-6 pt-8">
            <h3 class="font-black text-2xl mb-8 uppercase italic">Logs</h3>
            <div id="hist-list" class="glass-card p-6 min-h-[400px] space-y-2"></div>
        </div>

        <div id="scr-about" class="screen px-6 pt-8">
            <h3 class="font-black text-2xl mb-8 uppercase italic">Nova Corp</h3>
            <div class="glass-card p-8 space-y-6">
                <img src="https://images.unsplash.com/photo-1551288049-bbbda536339a?w=600" class="rounded-3xl w-full h-44 object-cover">
                <h4 class="font-black text-lg uppercase">Global Finance Protocol</h4>
                <p class="text-[11px] text-slate-500 font-bold leading-relaxed">Nova Corp is a decentralized asset management firm specializing in cloud-based quantum mining. We provide stable daily returns through automated liquidity pools.</p>
                <div class="bg-slate-50 p-6 rounded-2xl space-y-3 text-[10px] font-black uppercase">
                    <div class="flex justify-between"><span>Reg No:</span><span>UK-99281-B</span></div>
                    <div class="flex justify-between"><span>Support:</span><span>24/7 Global</span></div>
                    <div class="flex justify-between text-green-600"><span>Security:</span><span>SSL Encrypted</span></div>
                </div>
            </div>
        </div>

        <div id="scr-admin" class="screen px-6 pt-8">
            <h3 class="text-2xl font-black mb-8 text-red-600 uppercase italic">Admin Master</h3>
            
            <div class="glass-card p-8 bg-slate-950 text-white mb-8">
                <p class="text-[10px] font-black text-slate-500 mb-5 uppercase text-center">System Settings</p>
                <div class="space-y-4">
                    <input type="text" id="adm-ep" placeholder="EasyPaisa Number" class="w-full p-4 rounded-xl text-black font-bold">
                    <input type="text" id="adm-jc" placeholder="JazzCash Number" class="w-full p-4 rounded-xl text-black font-bold">
                    <input type="text" id="adm-bin" placeholder="Binance Pay ID" class="w-full p-4 rounded-xl text-black font-bold">
                    <button onclick="saveAdminSettings()" class="w-full btn-gold py-4 text-xs uppercase">Update Payment Details</button>
                </div>
            </div>

            <h4 class="text-[10px] font-black uppercase text-slate-400 mb-4 ml-2">Pending Requests</h4>
            <div id="adm-pending" class="space-y-4 mb-10"></div>

            <h4 class="text-[10px] font-black uppercase text-slate-400 mb-4 ml-2">User Directory</h4>
            <div id="adm-users" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('scr-dash', this)" class="nav-item active"><i class="fa-solid fa-house-chimney text-2xl"></i><span>Dashboard</span></div>
            <div onclick="nav('scr-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip text-2xl"></i><span>Nodes</span></div>
            <div onclick="nav('scr-team', this)" class="nav-item"><i class="fa-solid fa-users text-2xl"></i><span>Team</span></div>
            <div id="nav-adm" onclick="nav('scr-admin', this)" class="nav-item text-red-500" style="display:none;"><i class="fa-solid fa-lock text-2xl"></i><span>Master</span></div>
        </nav>
    </div>

    <div id="mod-dep" class="fixed inset-0 bg-white z-[2000] p-10 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-10"><h2 class="font-black text-2xl uppercase italic">Add Funds</h2><i onclick="closeModal('mod-dep')" class="fa-solid fa-xmark text-4xl text-slate-200"></i></div>
        <div class="grid grid-cols-3 gap-3 mb-8">
            <button onclick="showWay('ep')" class="glass-card p-4 font-black text-[8px] uppercase">EasyPaisa</button>
            <button onclick="showWay('jc')" class="glass-card p-4 font-black text-[8px] uppercase">JazzCash</button>
            <button onclick="showWay('bin')" class="glass-card p-4 font-black text-[8px] uppercase">Binance</button>
        </div>
        <div id="way-info" class="bg-slate-950 text-gold p-10 rounded-[40px] mb-8 hidden text-center"><p id="way-num" class="text-xl font-black">0000000000</p></div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="w-full p-5 bg-slate-50 rounded-2xl mb-4 font-bold outline-none border-none">
        <input type="text" id="dep-tx" placeholder="Transaction ID" class="w-full p-5 bg-slate-50 rounded-2xl mb-10 font-bold outline-none border-none">
        <button onclick="handleFinance('Deposit')" class="w-full btn-gold py-6 uppercase text-xs">Submit Deposit</button>
    </div>

    <div id="mod-wd" class="fixed inset-0 bg-white z-[2000] p-10 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-10"><h2 class="font-black text-2xl uppercase italic">Withdraw</h2><i onclick="closeModal('mod-wd')" class="fa-solid fa-xmark text-4xl text-slate-200"></i></div>
        <select id="wd-method" class="w-full p-5 bg-slate-50 rounded-2xl mb-4 font-bold border-none outline-none appearance-none"><option value="EasyPaisa">EasyPaisa</option><option value="JazzCash">JazzCash</option><option value="Binance USDT">Binance</option></select>
        <input type="number" id="wd-amt" placeholder="Amount ($)" class="w-full p-5 bg-slate-50 rounded-2xl mb-4 font-bold outline-none border-none">
        <input type="text" id="wd-acc" placeholder="Account Number / Wallet" class="w-full p-5 bg-slate-50 rounded-2xl mb-10 font-bold outline-none border-none">
        <button onclick="handleFinance('Withdraw')" class="w-full btn-gold py-6 uppercase text-xs">Request Payout</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const fbCfg = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(fbCfg); const db = getFirestore(app);

        let user = localStorage.getItem('nova_user');
        let sys = { ep: "03279177196", jc: "03279177196", bin: "PAY-ID-88221" };
        let mode = 'login'; let tapCount = 0;

        const nodes = [];
        for(let i=0; i<21; i++) { nodes.push({ id: i+1, n: `NODE V${i+1}`, c: 20 + (i * 25), y: 1.8 + (i * 2.2), img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?w=400" }); }

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            const ref = document.getElementById('u-ref').value.trim() || 'none';
            if(!id || !pw) return alert("Fields required.");
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(mode === 'login') {
                if(snap.exists() && snap.data().pw === pw) {
                    if(snap.data().status === 'Banned') return alert("Access Blocked.");
                    localStorage.setItem('nova_user', id); location.reload();
                } else alert("Invalid credentials.");
            } else {
                if(snap.exists()) return alert("Username taken.");
                await setDoc(uRef, { bal: 0, tp: 0, pw: pw, daily: 0, team_bal: 0, refBy: ref, active: [], lastClaim: 0, status: 'Active' });
                localStorage.setItem('nova_user', id); location.reload();
            }
        };

        window.logout = () => { if(confirm("Logout?")) { localStorage.clear(); location.reload(); } };

        function initApp(id) {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            
            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u || u.status === 'Banned') { localStorage.clear(); location.reload(); }
                document.getElementById('user-tag').innerText = `@${id}`;
                document.getElementById('b-main').innerText = `$${u.bal.toFixed(2)}`;
                document.getElementById('b-header').innerText = `$${u.bal.toFixed(2)}`;
                document.getElementById('b-daily').innerText = `+$${u.daily.toFixed(2)}`;
                document.getElementById('b-total-p').innerText = `$${(u.tp || 0).toFixed(2)}`;
                renderNodes(u.active || []);
            });

            onSnapshot(query(collection(db, "logs"), where("user", "==", id)), s => {
                const hList = document.getElementById('hist-list'); hList.innerHTML = "";
                if(s.empty) hList.innerHTML = '<p class="text-center text-slate-300 py-10 font-black uppercase text-[10px]">No logs</p>';
                s.forEach(doc => {
                    const l = doc.data();
                    hList.innerHTML += `<div class="history-item flex justify-between items-center">
                        <div><p class="text-[9px] font-black uppercase">${l.type}</p><p class="text-[8px] text-slate-400 font-bold">${new Date(l.timestamp).toLocaleDateString()}</p></div>
                        <div class="text-right"><p class="text-xs font-black ${l.type==='Deposit'?'text-green-600':'text-red-500'}">$${l.amount}</p><p class="text-[8px] font-black uppercase ${l.status==='Pending'?'text-orange-500':'text-blue-500'}">${l.status}</p></div>
                    </div>`;
                });
            });

            onSnapshot(query(collection(db, "users"), where("refBy", "==", id)), s => {
                document.getElementById('team-count').innerText = s.size;
                const tl = document.getElementById('team-list'); tl.innerHTML = "";
                s.forEach(doc => { tl.innerHTML += `<div class="glass-card p-5 flex justify-between items-center text-[10px]"><b>@${doc.id}</b><span class="text-green-500 font-black italic uppercase">Verified</span></div>`; });
            });

            onSnapshot(doc(db, "config", "master"), d => { if(d.exists()) sys = d.data(); });
        }

        function renderNodes(active) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            nodes.forEach(n => {
                const isRun = active.includes(n.id);
                grid.innerHTML += `<div class="glass-card p-6 overflow-hidden">
                    <img src="${n.img}" class="node-img">
                    <div class="flex justify-between items-center">
                        <div><h4 class="font-black text-xs uppercase">${n.n}</h4><p class="text-[10px] text-green-600 font-black">Profit: $${n.y.toFixed(2)}/hr</p></div>
                        ${isRun ? `<span class="bg-green-50 text-green-600 px-4 py-2 rounded-full text-[9px] font-black">RUNNING</span>` : `<button onclick="buyNode(${n.id}, ${n.c}, ${n.y})" class="btn-gold px-6 py-3 text-[9px] uppercase">$${n.c}</button>`}
                    </div>
                </div>`;
            });
        }

        window.buyNode = async (id, c, y) => {
            const uRef = doc(db, "users", user); const s = await getDoc(uRef);
            if(s.data().bal < c) return alert("Insufficient funds.");
            await updateDoc(uRef, { bal: s.data().bal - c, active: [...s.data().active, id], daily: (s.data().daily || 0) + y });
            alert("Activation Success!");
        };

        window.claimDaily = async () => {
            const uRef = doc(db, "users", user); const s = await getDoc(uRef);
            if(Date.now() - (s.data().lastClaim || 0) < 86400000) return alert("Next claim in 24h.");
            await updateDoc(uRef, { bal: s.data().bal + 0.50, tp: (s.data().tp || 0) + 0.50, lastClaim: Date.now() });
            alert("Reward Received!");
        };

        window.handleFinance = async (type) => {
            const amt = parseFloat(document.getElementById(type === 'Deposit' ? 'dep-amt' : 'wd-amt').value);
            const ref = document.getElementById(type === 'Deposit' ? 'dep-tx' : 'wd-acc').value;
            if(!amt || !ref) return alert("Input required.");
            await addDoc(collection(db, "logs"), { user, type, amount: amt, ref, status: 'Pending', timestamp: Date.now(), method: type === 'Withdraw' ? document.getElementById('wd-method').value : 'Direct' });
            alert("Broadcasted to Master Node."); closeModal('mod-dep'); closeModal('mod-wd');
        };

        document.getElementById('logo-main').onclick = () => { tapCount++; if(tapCount >= 5) { if(prompt("Security Key:") === "nov786") { document.getElementById('nav-adm').style.display="flex"; loadMaster(); } tapCount=0; } };

        function loadMaster() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const al = document.getElementById('adm-pending'); al.innerHTML = "";
                s.forEach(d => {
                    al.innerHTML += `<div class="glass-card p-5 text-[10px] flex justify-between items-center border-l-8 border-gold">
                        <div><b>@${d.data().user}</b> [${d.data().type}]<br>$${d.data().amount}<br>${d.data().ref}</div>
                        <div class="flex gap-2">
                            <button onclick="approveAction('${d.id}', '${d.data().user}', ${d.data().amount}, '${d.data().type}')" class="bg-green-500 text-white p-3 rounded-xl"><i class="fa-solid fa-check"></i></button>
                            <button onclick="deleteDoc(doc(db, 'logs', '${d.id}'))" class="bg-red-500 text-white p-3 rounded-xl"><i class="fa-solid fa-trash"></i></button>
                        </div>
                    </div>`;
                });
            });
            onSnapshot(collection(db, "users"), s => {
                const ul = document.getElementById('adm-users'); ul.innerHTML = "";
                s.forEach(d => {
                    ul.innerHTML += `<div class="glass-card p-4 flex justify-between items-center text-[10px]">
                        <div><b>@${d.id}</b> | Bal: $${d.data().bal.toFixed(2)}</div>
                        <button onclick="updateStatus('${d.id}', 'Banned')" class="bg-red-50 text-red-600 px-3 py-1 rounded-full font-black">BAN</button>
                    </div>`;
                });
            });
        }

        window.approveAction = async (lid, uid, amt, type) => {
            const uRef = doc(db, "users", uid); const us = await getDoc(uRef);
            if(type === 'Deposit') await updateDoc(uRef, { bal: (us.data().bal || 0) + amt });
            if(type === 'Withdraw') await updateDoc(uRef, { bal: (us.data().bal || 0) - amt });
            await updateDoc(doc(db, "logs", lid), { status: 'Approved' }); alert("Protocol Updated.");
        };

        window.updateStatus = async (uid, s) => { await updateDoc(doc(db, "users", uid), { status: s }); };
        window.saveAdminSettings = async () => {
            await setDoc(doc(db, "config", "master"), { ep: document.getElementById('adm-ep').value, jc: document.getElementById('adm-jc').value, bin: document.getElementById('adm-bin').value }, { merge: true });
            alert("Payment Config Updated.");
        };

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            if(el) { document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active'); }
        };
        window.showWay = (t) => { document.getElementById('way-info').classList.remove('hidden'); document.getElementById('way-num').innerText = sys[t] || "Scanning..."; };
        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.toggleAuth = () => { mode = mode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = mode === 'login' ? "New User? Register" : "Already User? Login"; };

        if(user) initApp(user);
    </script>
</body>
</html>
