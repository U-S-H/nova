<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | Final Master</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --bg: #F8FAFC; }
        body { background: var(--bg); font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; -webkit-tap-highlight-color: transparent; }
        .premium-card { background: white; border: 1px solid #e2e8f0; border-radius: 24px; box-shadow: 0 4px 20px rgba(0,0,0,0.02); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 14px; font-weight: 800; border: none; }
        .screen { display: none; padding-bottom: 120px; animation: fadeIn 0.4s ease; }
        .active-screen { display: block !important; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(15px); border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 15px 5px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 9px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: var(--gold); }
        input, select { background: #f1f5f9 !important; border: 1px solid #e2e8f0 !important; border-radius: 12px !important; outline: none; }
    </style>
</head>
<body>

    <div id="auth-screen" class="flex flex-col justify-center items-center min-h-screen p-8 text-center active-screen">
        <h1 id="logo-auth" class="text-4xl font-black tracking-tighter mb-1 select-none">NO<span class="text-[#D4AF37]">VA</span></h1>
        <p class="text-[9px] text-slate-400 font-bold uppercase tracking-[0.3em] mb-12">Quantum Asset Management</p>
        <div class="space-y-4 w-full max-w-xs">
            <input type="text" id="u-id" placeholder="Username" class="w-full p-4">
            <input type="password" id="u-pw" placeholder="Password" class="w-full p-4">
            <button onclick="handleAuth()" class="w-full btn-gold py-4 uppercase text-xs tracking-widest mt-2">Authorize</button>
            <p onclick="toggleAuth()" id="auth-btn-text" class="text-[10px] text-slate-400 font-bold uppercase cursor-pointer underline mt-6">Create New Account</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center sticky top-0 bg-white/80 backdrop-blur-md z-[100] border-b">
            <h2 id="logo-main" class="font-black text-2xl tracking-tighter cursor-pointer">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="text-right"><p id="user-display" class="text-[10px] font-black text-slate-600 uppercase">@USER</p></div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6">
            <div class="bg-slate-900 rounded-[30px] p-8 mb-6 text-white shadow-xl relative overflow-hidden">
                <p class="text-[8px] text-slate-400 font-bold uppercase tracking-widest mb-1">Portfolio Balance</p>
                <h1 id="b-main" class="text-5xl font-black tracking-tighter mb-8">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                    <div><p class="text-[7px] text-slate-400 font-bold uppercase">Daily Yield</p><p id="b-daily" class="text-lg font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[7px] text-slate-400 font-bold uppercase">Total Profit</p><p id="b-total" class="text-lg font-black text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>
            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('modal-dep')" class="premium-card p-6 flex flex-col items-center gap-2"><i class="fa-solid fa-wallet text-green-600"></i><span class="text-[10px] font-black">DEPOSIT</span></button>
                <button onclick="openModal('modal-wd')" class="premium-card p-6 flex flex-col items-center gap-2"><i class="fa-solid fa-hand-holding-dollar text-red-600"></i><span class="text-[10px] font-black">WITHDRAW</span></button>
            </div>
            <div class="premium-card p-4"><p class="text-[9px] font-bold text-slate-400 uppercase mb-2">Referral Link</p><input type="text" id="ref-link" readonly class="w-full p-2 text-[9px] border-none"></div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6"><h2 class="text-xl font-black mb-6 uppercase">Hardware <span class="text-[#D4AF37]">Inventory</span></h2><div id="nodes-grid" class="space-y-4"></div></div>

        <div id="page-logs" class="screen px-4 pt-6"><h2 class="text-xl font-black mb-6 uppercase">System <span class="text-slate-400">Ledger</span></h2><div id="logs-list" class="space-y-3"></div></div>

        <div id="page-admin" class="screen px-4 pt-6"><h2 class="text-xl font-black mb-6 text-red-600 uppercase">Network Control</h2><div id="adm-pending-list" class="space-y-4"></div></div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-house"></i><span>HOME</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip"></i><span>NODES</span></div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-clock-rotate-left"></i><span>LOGS</span></div>
            <div id="nav-admin-btn" onclick="nav('page-admin', this)" class="nav-item text-red-600 hidden"><i class="fa-solid fa-shield-halved"></i><span>ADMIN</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-10"><h2 class="font-black text-2xl">DEPOSIT</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-xl"></i></div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="w-full p-4 mb-4">
        <input type="text" id="dep-ref" placeholder="Transaction Hash / ID" class="w-full p-4 mb-8">
        <button onclick="submitFin('Deposit')" class="w-full btn-gold py-4 uppercase text-xs">Submit Request</button>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-white z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-10"><h2 class="font-black text-2xl text-red-600">WITHDRAW</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark text-xl"></i></div>
        <input type="number" id="wd-amt" placeholder="Amount ($)" class="w-full p-4 mb-4">
        <input type="text" id="wd-addr" placeholder="Wallet Address" class="w-full p-4 mb-8">
        <button onclick="submitFin('Withdraw')" class="w-full btn-gold py-4 uppercase text-xs text-white">Request Payout</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);

        let user = localStorage.getItem('nova_session');
        let tapCount = 0; let mode = 'login';

        // 1. Secret Admin Toggle (4 Taps)
        document.getElementById('logo-main').onclick = () => {
            tapCount++; if(tapCount === 4) {
                if(prompt("Admin Key:") === "nov786") {
                    document.getElementById('nav-admin-btn').classList.remove('hidden');
                    loadAdminPanel(); alert("Admin Unlocked");
                }
                tapCount = 0;
            }
        };

        // 2. Auth Logic
        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("Fill data");
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(mode === 'login') {
                if(snap.exists() && snap.data().password === pw) { localStorage.setItem('nova_session', id); location.reload(); }
                else alert("Invalid credentials");
            } else {
                if(snap.exists()) return alert("Username taken");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0, nodes: [] });
                localStorage.setItem('nova_session', id); location.reload();
            }
        };

        // 3. User & Admin Functions
        function initApp(id) {
            document.getElementById('user-display').innerText = `@${id}`;
            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u) return;
                document.getElementById('b-main').innerText = `$${u.balance.toFixed(2)}`;
                document.getElementById('b-daily').innerText = `+$${u.daily.toFixed(2)}`;
                document.getElementById('b-total').innerText = `$${u.total_profit.toFixed(2)}`;
                renderNodes(u.nodes || []);
            });
            onSnapshot(query(collection(db, "logs"), where("user", "==", id), orderBy("time", "desc")), s => {
                const list = document.getElementById('logs-list'); list.innerHTML = "";
                s.forEach(d => {
                    const l = d.data();
                    list.innerHTML += `<div class="premium-card p-4 flex justify-between items-center text-[10px]">
                        <div><b>${l.type}</b><br><span class="text-slate-400">${new Date(l.time).toLocaleTimeString()}</span></div>
                        <div class="text-right"><b>$${l.amount}</b><br><span class="${l.status==='Pending'?'text-orange-500':'text-green-500'} font-bold">${l.status}</span></div>
                    </div>`;
                });
            });
        }

        function renderNodes(active) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            for(let i=1; i<=21; i++) {
                const cost = 10 + (i*15); const yieldV = 0.5 + (i*1.1); const isOwned = active.includes(i);
                grid.innerHTML += `<div class="premium-card p-5 flex justify-between items-center">
                    <div><h4 class="font-black text-xs uppercase">Node V${i}</h4><p class="text-[9px] text-slate-400">Profit: $${yieldV.toFixed(1)}/day</p></div>
                    <button onclick="${isOwned?'':`buyNode(${i},${cost},${yieldV})`}" class="px-4 py-2 text-[8px] font-black rounded-lg ${isOwned?'text-green-500 bg-green-50':'btn-gold'}">
                        ${isOwned ? 'ACTIVE' : `BUY $${cost}`}
                    </button>
                </div>`;
            }
        }

        window.buyNode = async (id, cost, y) => {
            const uRef = doc(db, "users", user); const snap = await getDoc(uRef);
            if(snap.data().balance < cost) return alert("Low Balance");
            await updateDoc(uRef, { balance: snap.data().balance - cost, nodes: [...(snap.data().nodes || []), id], daily: (snap.data().daily || 0) + y });
            alert("Deployed!");
        };

        window.submitFin = async (type) => {
            const amt = parseFloat(document.getElementById(type==='Deposit'?'dep-amt':'wd-amt').value);
            const ref = document.getElementById(type==='Deposit'?'dep-ref':'wd-addr').value;
            if(!amt || !ref) return alert("Fill data");
            await addDoc(collection(db, "logs"), { user, type, amount: amt, ref, status: 'Pending', time: Date.now() });
            alert("Submitted!"); closeModal(type==='Deposit'?'modal-dep':'modal-wd');
        };

        // ADMIN CORE LOGIC
        function loadAdminPanel() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const container = document.getElementById('adm-pending-list'); container.innerHTML = "";
                s.forEach(docSnap => {
                    const l = docSnap.data(); const dId = docSnap.id;
                    container.innerHTML += `<div class="premium-card p-5 border-l-4 border-red-500">
                        <p class="text-[9px] font-black uppercase text-slate-500">${l.type} - @${l.user}</p>
                        <h3 class="text-xl font-black mb-3">$${l.amount}</h3>
                        <div class="flex gap-2">
                            <button onclick="processReq('${dId}','${l.user}',${l.amount},'${l.type}','Approve')" class="flex-1 bg-green-500 text-white py-2 rounded-lg text-[9px] font-bold">APPROVE</button>
                            <button onclick="processReq('${dId}','${l.user}',${l.amount},'${l.type}','Reject')" class="flex-1 bg-slate-100 text-slate-400 py-2 rounded-lg text-[9px] font-bold">REJECT</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.processReq = async (dId, uId, amt, type, action) => {
            const uRef = doc(db, "users", uId); const lRef = doc(db, "logs", dId);
            if(action === 'Approve' && type === 'Deposit') {
                const uSnap = await getDoc(uRef);
                await updateDoc(uRef, { balance: (uSnap.data().balance || 0) + amt });
            }
            await updateDoc(lRef, { status: action === 'Approve' ? 'Completed' : 'Rejected' });
            alert(`Request ${action}d`);
        };

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            el.classList.add('active');
        };
        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.toggleAuth = () => { mode = mode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = mode === 'login' ? "Create New Account" : "Back to Login"; };
        if(user) { document.getElementById('auth-screen').classList.remove('active-screen'); document.getElementById('app').classList.remove('hidden'); initApp(user); }
    </script>
</body>
</html>
