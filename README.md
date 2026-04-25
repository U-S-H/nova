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
        body { background: #F8FAFC; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; -webkit-tap-highlight-color: transparent; overflow-x: hidden; }
        .glass-card { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(10px); border: 1px solid #e2e8f0; border-radius: 32px; box-shadow: 0 10px 40px rgba(0,0,0,0.02); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 18px; font-weight: 800; border: none; cursor: pointer; transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1); }
        .btn-gold:active { transform: scale(0.95); opacity: 0.9; }
        .screen { display: none; min-height: 100vh; width: 100%; padding-bottom: 110px; }
        .active-screen { display: block; animation: slideIn 0.5s ease-out; }
        @keyframes slideIn { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.9); backdrop-filter: blur(25px); border-top: 1px solid #f1f5f9; display: flex; justify-content: space-around; padding: 18px 0; z-index: 1000; border-radius: 30px 30px 0 0; box-shadow: 0 -10px 40px rgba(0,0,0,0.03); }
        .nav-item { color: #94a3b8; font-size: 8px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 5px; transition: 0.3s; }
        .nav-item.active { color: var(--gold); transform: translateY(-5px); }
        .node-img { width: 100%; height: 150px; object-fit: cover; border-bottom: 1px solid #f1f5f9; }
        .mining-pulse { position: absolute; top: 15px; right: 15px; width: 10px; height: 10px; background: #22c55e; border-radius: 50%; box-shadow: 0 0 15px #22c55e; animation: pulse 1.5s infinite; }
        @keyframes pulse { 0% { transform: scale(1); opacity: 1; } 100% { transform: scale(3); opacity: 0; } }
    </style>
</head>
<body>

    <div id="app">
        <header class="p-6 flex justify-between items-center bg-white/80 border-b sticky top-0 z-[100] backdrop-blur-md">
            <h2 id="logo-adm" class="font-black text-2xl italic tracking-tighter cursor-pointer select-none">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="flex items-center gap-3 bg-slate-50 px-4 py-2 rounded-2xl border border-slate-100">
                <div class="text-right">
                    <p id="u-tag" class="text-[9px] font-black text-slate-400 uppercase">@SESSION_ID</p>
                    <p id="b-header" class="text-xs font-black text-slate-900">$0.00</p>
                </div>
            </div>
        </header>

        <div id="scr-home" class="screen active-screen px-4 pt-6">
            <div class="bg-slate-950 rounded-[40px] p-10 text-white mb-8 relative shadow-2xl overflow-hidden">
                <div class="absolute -bottom-10 -right-10 w-40 h-40 bg-gold/10 rounded-full blur-3xl"></div>
                <p class="text-[10px] text-slate-500 font-bold uppercase tracking-[0.2em] mb-2">Portfolio Balance</p>
                <h1 id="b-main" class="text-5xl font-black tracking-tighter mb-10">$0.00</h1>
                <div class="grid grid-cols-2 gap-6 border-t border-white/5 pt-8">
                    <div><p class="text-[8px] text-slate-500 uppercase font-black">24h Profit</p><p id="b-yield" class="text-2xl font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[8px] text-slate-500 uppercase font-black">Total Yield</p><p id="b-total" class="text-2xl font-black text-gold">$0.00</p></div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('mod-dep')" class="glass-card p-7 flex flex-col items-center gap-3"><i class="fa-solid fa-bolt-lightning text-gold text-2xl"></i><span class="text-[10px] font-black uppercase">Recharge</span></button>
                <button onclick="nav('scr-hist', document.getElementById('nav-hist'))" class="glass-card p-7 flex flex-col items-center gap-3"><i class="fa-solid fa-receipt text-slate-300 text-2xl"></i><span class="text-[10px] font-black uppercase">Records</span></button>
            </div>

            <div onclick="nav('scr-about', null)" class="glass-card p-5 flex items-center justify-between cursor-pointer">
                <div class="flex items-center gap-4">
                    <div class="w-10 h-10 bg-gold/10 rounded-xl flex items-center justify-center"><i class="fa-solid fa-building-shield text-gold"></i></div>
                    <div><h4 class="text-xs font-black uppercase">Company Profile</h4><p class="text-[9px] font-bold text-slate-400">Security & Infrastructure Details</p></div>
                </div>
                <i class="fa-solid fa-chevron-right text-slate-200 text-xs"></i>
            </div>
        </div>

        <div id="scr-nodes" class="screen px-4 pt-6">
            <div class="flex justify-between items-center mb-8 px-2">
                <h3 class="font-black text-2xl italic uppercase">Terminal Nodes</h3>
                <span class="bg-red-50 text-red-500 text-[9px] font-black px-3 py-1.5 rounded-full">LIVE NODES: 21</span>
            </div>
            <div id="nodes-grid" class="space-y-6"></div>
        </div>

        <div id="scr-hist" class="screen px-4 pt-6">
            <h3 class="font-black text-2xl italic uppercase mb-8 px-2">Transaction History</h3>
            <div id="hist-list" class="space-y-4"></div>
        </div>

        <div id="scr-about" class="screen px-4 pt-6">
            <div class="glass-card p-8 mb-6 border-gold/20">
                <h3 class="font-black text-3xl mb-4 italic tracking-tighter text-slate-950">NO<span class="text-gold">VA</span> CORP</h3>
                <p class="text-xs text-slate-500 font-medium leading-relaxed">
                    Nova Corp provides professional cloud mining solutions. Our distributed terminal network ensures consistent uptime and institutional-grade yields for digital asset management.
                </p>
            </div>
            <div class="glass-card p-8 text-center bg-slate-950 text-white border-none shadow-gold/10">
                <i class="fa-solid fa-headset text-gold text-4xl mb-4"></i>
                <h4 class="font-black text-lg mb-2">Official Helpdesk</h4>
                <p class="text-[10px] text-slate-500 mb-8 font-bold uppercase tracking-widest">Available 24/7 for support</p>
                <div class="space-y-3">
                    <a href="https://wa.me/YOUR_NUM" class="w-full btn-gold py-4 block text-[11px] tracking-widest uppercase">WhatsApp Support</a>
                    <a href="https://t.me/YOUR_TELE" class="w-full border border-slate-700 rounded-2xl py-4 block text-[11px] font-black uppercase">Telegram Channel</a>
                </div>
            </div>
        </div>

        <div id="scr-admin" class="screen px-4 pt-6">
            <h2 class="text-3xl font-black mb-8 text-red-600 uppercase italic px-2">System Core</h2>
            <div class="glass-card p-6 bg-slate-900 text-white mb-6 border-none">
                <h4 class="text-[10px] font-black text-slate-500 uppercase mb-6 tracking-[0.2em]">Pending Requests</h4>
                <div id="adm-reqs" class="space-y-4"></div>
            </div>
            <div class="glass-card p-6 border-none shadow-xl">
                <h4 class="text-[10px] font-black uppercase mb-6 text-slate-400">Manage Operators</h4>
                <div id="adm-users" class="space-y-4"></div>
            </div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('scr-home', this)" class="nav-item active"><i class="fa-solid fa-house-user text-xl"></i><span>HOME</span></div>
            <div onclick="nav('scr-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip text-xl"></i><span>MINING</span></div>
            <div id="nav-hist" onclick="nav('scr-hist', this)" class="nav-item"><i class="fa-solid fa-receipt text-xl"></i><span>LOGS</span></div>
            <div onclick="nav('scr-about', this)" class="nav-item"><i class="fa-solid fa-shield-halved text-xl"></i><span>SECURITY</span></div>
            <div id="nav-adm" onclick="nav('scr-admin', this)" class="nav-item text-red-500" style="display:none;"><i class="fa-solid fa-user-gear text-xl"></i><span>ADMIN</span></div>
        </nav>
    </div>

    <div id="mod-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden flex flex-col">
        <div class="flex justify-between items-center mb-12"><h2 class="font-black text-2xl italic uppercase tracking-tighter">Terminal Funding</h2><i onclick="closeModal('mod-dep')" class="fa-solid fa-circle-xmark text-3xl text-slate-200"></i></div>
        <div class="space-y-6">
            <div class="space-y-2">
                <label class="text-[10px] font-black uppercase text-slate-400 ml-2">Amount ($)</label>
                <input type="number" id="d-amt" placeholder="0.00" class="w-full p-5 bg-slate-50 rounded-[22px] outline-none font-black text-xl border-none">
            </div>
            <div class="space-y-2">
                <label class="text-[10px] font-black uppercase text-slate-400 ml-2">Transaction Hash / Proof ID</label>
                <input type="text" id="d-tx" placeholder="Enter Ref ID" class="w-full p-5 bg-slate-50 rounded-[22px] outline-none font-bold border-none">
            </div>
            <button onclick="handleRequest('Deposit')" class="w-full btn-gold py-6 shadow-2xl shadow-gold/30 mt-8 text-xs uppercase tracking-widest">Initiate Deposit</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, onSnapshot, collection, addDoc, query, where, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const fb = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(fb); const db = getFirestore(app);

        let user = localStorage.getItem('nova_user') || 'dev_user';
        let admClicks = 0;

        const nodes = [];
        for(let i=1; i<=21; i++) {
            nodes.push({ id:i, n:`NOVA-CORE V${i}`, p:10+(i*12), y:1+(i*1.8), img:`https://images.unsplash.com/photo-1639322537228-f710d846310a?auto=format&fit=crop&w=600` });
        }

        // God Mode Logic
        document.getElementById('logo-adm').onclick = () => {
            admClicks++;
            if(admClicks >= 5) {
                if(prompt("System Access Key:") === "nov786") {
                    document.getElementById('nav-adm').style.display = "flex";
                    loadAdminSuite(); alert("God Mode: ENABLED");
                }
                admClicks = 0;
            }
        };

        // User Data Sync
        onSnapshot(doc(db, "users", user), snap => {
            const u = snap.data(); if(!u) return;
            document.getElementById('u-tag').innerText = `@${user}`;
            const bal = `$${(u.balance || 0).toFixed(2)}`;
            document.getElementById('b-main').innerText = bal;
            document.getElementById('b-header').innerText = bal;
            document.getElementById('b-yield').innerText = `+$${(u.daily || 0).toFixed(2)}`;
            document.getElementById('b-total').innerText = `$${(u.total_profit || 0).toFixed(2)}`;
            renderNodes(u.active_nodes || []);
        });

        // Records Sync
        onSnapshot(query(collection(db, "logs"), where("user", "==", user)), snap => {
            const list = document.getElementById('hist-list'); list.innerHTML = "";
            snap.forEach(d => {
                const l = d.data();
                list.innerHTML += `<div class="glass-card p-6 flex justify-between items-center border-l-4 ${l.status==='Approved'?'border-l-green-500':'border-l-gold'}">
                    <div><p class="text-[9px] font-black uppercase text-slate-400">${l.type}</p><p class="text-xs font-bold">${l.ref}</p></div>
                    <div class="text-right"><p class="font-black text-sm">$${l.amount}</p><p class="text-[8px] font-black uppercase ${l.status==='Pending'?'text-orange-500':'text-green-500'}">${l.status}</p></div>
                </div>`;
            });
        });

        function renderNodes(active) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            nodes.forEach(n => {
                const isRun = active.includes(n.id);
                grid.innerHTML += `<div class="glass-card overflow-hidden relative">
                    ${isRun ? '<div class="mining-pulse"></div>' : ''}
                    <img src="${n.img}" class="node-img">
                    <div class="p-6 flex justify-between items-center">
                        <div><h4 class="font-black text-[11px] uppercase">${n.n}</h4><p class="text-[10px] font-bold text-green-500">Yield: $${n.y.toFixed(1)} / day</p></div>
                        <button onclick="${isRun?'':`buyNode(${n.id},${n.p},${n.y})`}" class="px-7 py-2.5 text-[10px] ${isRun?'text-green-500 font-black':'btn-gold'}">
                            ${isRun?'● ONLINE':'$'+n.p}
                        </button>
                    </div>
                </div>`;
            });
        }

        window.buyNode = async (id, price, yieldVal) => {
            const ref = doc(db, "users", user); const s = await getDoc(ref);
            if(s.data().balance < price) return alert("Insufficient Terminal Balance");
            await updateDoc(ref, { 
                balance: s.data().balance - price, 
                active_nodes: [...(s.data().active_nodes || []), id], 
                daily: (s.data().daily || 0) + yieldVal 
            });
            alert("Node Successfully Synchronized!");
        };

        window.handleRequest = async (type) => {
            const amount = parseFloat(document.getElementById('d-amt').value);
            const ref = document.getElementById('d-tx').value;
            if(!amount || !ref) return alert("Invalid Parameters");
            await addDoc(collection(db, "logs"), { user, type, amount, ref, status: "Pending" });
            alert("Terminal Request Transmitted!"); closeModal('mod-dep');
        };

        // ADMIN POWER SUITE
        function loadAdminSuite() {
            onSnapshot(collection(db, "users"), s => {
                const list = document.getElementById('adm-users'); list.innerHTML = "";
                s.forEach(d => {
                    list.innerHTML += `<div class="p-4 border-b flex justify-between items-center text-xs">
                        <div><b>@${d.id}</b><br>Bal: $${d.data().balance.toFixed(2)}</div>
                        <div class="flex gap-3"><button onclick="editBal('${d.id}')" class="text-blue-500 font-black">EDIT</button><button onclick="delUser('${d.id}')" class="text-red-500 font-black">DEL</button></div>
                    </div>`;
                });
            });
            onSnapshot(query(collection(db, "logs"), where("status","==","Pending")), s => {
                const list = document.getElementById('adm-reqs'); list.innerHTML = "";
                s.forEach(d => {
                    const l = d.data();
                    list.innerHTML += `<div class="bg-white/5 p-4 rounded-2xl flex justify-between items-center text-[10px]">
                        <div><b>${l.user}</b>: $${l.amount}<br>${l.ref}</div>
                        <button onclick="approveReq('${d.id}','${l.user}',${l.amount})" class="bg-green-500 text-white px-5 py-2 rounded-xl font-black">APPROVE</button>
                    </div>`;
                });
            });
        }

        window.editBal = async (id) => { const v = prompt("Enter New Balance:"); if(v) await updateDoc(doc(db,"users",id), {balance: parseFloat(v)}); };
        window.delUser = async (id) => { if(confirm("Terminate User Account?")) await deleteDoc(doc(db,"users",id)); };
        window.approveReq = async (logId, userId, amt) => {
            const ur = doc(db, "users", userId); const us = await getDoc(ur);
            await updateDoc(ur, { balance: (us.data().balance || 0) + amt });
            await updateDoc(doc(db, "logs", logId), { status: "Approved" });
        };

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            if(el) { document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active'); }
        };
        window.openModal = (id) => { document.getElementById(id).classList.remove('hidden'); document.getElementById(id).style.display = 'flex'; };
        window.closeModal = (id) => { document.getElementById(id).classList.add('hidden'); document.getElementById(id).style.display = 'none'; };
    </script>
</body>
</html>
