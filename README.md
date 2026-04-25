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
        body { background: #F8FAFC; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; }
        .premium-card { background: white; border: 1px solid #e2e8f0; border-radius: 24px; box-shadow: 0 4px 20px rgba(0,0,0,0.02); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 14px; font-weight: 800; cursor: pointer; border: none; }
        .screen { display: none; }
        .active-screen { display: block; animation: slideUp 0.4s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.95); backdrop-filter: blur(10px); border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 12px 5px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 8px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: var(--gold); }
        .timer-badge { background: #fee2e2; color: #ef4444; padding: 2px 8px; border-radius: 8px; font-size: 9px; font-weight: 800; }
    </style>
</head>
<body>

    <div id="auth-screen" class="active-screen">
        <section class="p-8 text-center bg-white min-h-screen flex flex-col justify-center">
            <h1 class="text-5xl font-black text-slate-900 tracking-tighter italic">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[10px] text-slate-400 font-bold uppercase tracking-widest mt-2">Institutional Terminal</p>
            <div class="space-y-4 max-w-xs mx-auto w-full mt-10">
                <input type="text" id="u-id" placeholder="Username" class="w-full p-4 rounded-2xl outline-none border bg-slate-50">
                <input type="password" id="u-pw" placeholder="Password" class="w-full p-4 rounded-2xl outline-none border bg-slate-50">
                <input type="text" id="u-ref" placeholder="Referral ID (Optional)" class="w-full p-4 rounded-2xl outline-none border bg-slate-50">
                <button onclick="handleAuth()" class="w-full btn-gold py-4 uppercase text-xs tracking-widest shadow-lg shadow-gold/20">Authorize Access</button>
                <p onclick="toggleAuth()" id="auth-btn-text" class="text-[10px] text-slate-400 font-bold uppercase cursor-pointer underline">Create Operational Account</p>
            </div>
        </section>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white border-b sticky top-0 z-[100]">
            <h2 id="logo-main" class="font-black text-2xl tracking-tighter select-none cursor-pointer">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="flex items-center gap-3">
                <div class="text-right">
                    <p id="user-display" class="text-[10px] font-black uppercase">@USER</p>
                    <div class="flex items-center justify-end gap-1"><span class="w-1.5 h-1.5 bg-green-500 rounded-full animate-pulse"></span><span class="text-[8px] font-bold text-slate-400 uppercase">Live</span></div>
                </div>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6 pb-32">
            <div class="bg-slate-900 rounded-[32px] p-8 mb-6 text-white shadow-2xl relative overflow-hidden">
                <div class="absolute top-0 right-0 p-4 opacity-10 text-5xl italic font-black">NV</div>
                <p class="text-[9px] text-slate-500 font-bold uppercase tracking-widest mb-1">Portfolio Value</p>
                <h1 id="b-main" class="text-5xl font-black tracking-tighter mb-8">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                    <div><p class="text-[7px] text-slate-500 font-bold uppercase">24h Yield</p><p id="b-daily" class="text-xl font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[7px] text-slate-500 font-bold uppercase">Cycle Profit</p><p id="b-total" class="text-xl font-black text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('modal-dep')" class="premium-card p-6 flex flex-col items-center gap-2"><i class="fa-solid fa-plus-circle text-[#D4AF37] text-xl"></i><span class="text-[10px] font-black uppercase">Deposit</span></button>
                <button onclick="openModal('modal-wd')" class="premium-card p-6 flex flex-col items-center gap-2"><i class="fa-solid fa-arrow-up-right-from-square text-slate-400 text-xl"></i><span class="text-[10px] font-black uppercase">Withdraw</span></button>
            </div>

            <div class="premium-card p-5 mb-6 border-dashed border-2 border-gold/30">
                <h3 class="text-[10px] font-black uppercase mb-3">Redeem Promo Code</h3>
                <div class="flex gap-2">
                    <input type="text" id="promo-input" placeholder="Enter Code" class="flex-1 p-3 rounded-xl border-none bg-slate-50 text-xs font-bold uppercase">
                    <button onclick="applyPromo()" class="btn-gold px-6 text-[10px]">APPLY</button>
                </div>
            </div>

            <div class="premium-card p-5">
                <h3 class="text-[10px] font-black uppercase mb-2">Your Affiliate Link</h3>
                <div class="flex items-center justify-between bg-slate-50 p-3 rounded-xl border border-dashed">
                    <p id="ref-link-display" class="text-[10px] font-bold text-slate-400 truncate mr-4">Loading...</p>
                    <i onclick="copyRef()" class="fa-solid fa-copy text-gold cursor-pointer"></i>
                </div>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6 pb-32">
            <div class="flex justify-between items-center mb-6 px-2">
                <h3 class="font-black text-xl italic uppercase">Hardware Nodes</h3>
                <div id="next-cycle" class="timer-badge">Next Yield: 12:45:01</div>
            </div>
            <div id="nodes-grid" class="space-y-4"></div>
        </div>

        <div id="page-team" class="screen px-4 pt-6 pb-32">
            <h3 class="font-black text-xl italic uppercase mb-6 px-2">Affiliate Network</h3>
            <div class="grid grid-cols-2 gap-4 mb-6 text-center">
                <div class="premium-card p-5">
                    <p class="text-[8px] font-bold text-slate-400 uppercase">Team Size</p>
                    <h2 id="team-count" class="text-2xl font-black">0</h2>
                </div>
                <div class="premium-card p-5">
                    <p class="text-[8px] font-bold text-slate-400 uppercase">Commissions</p>
                    <h2 id="team-comm" class="text-2xl font-black text-green-500">$0.00</h2>
                </div>
            </div>
            <div class="premium-card p-5">
                <h4 class="text-[10px] font-black uppercase mb-4 border-b pb-2">Active Referrals</h4>
                <div id="team-list" class="space-y-3 text-[10px] font-bold">
                    <p class="text-slate-400 text-center py-4 italic">No subordinates yet</p>
                </div>
            </div>
        </div>

        <div id="page-admin" class="screen px-4 pt-6 pb-32">
            <h2 class="text-2xl font-black mb-6 text-red-600 uppercase italic">Command Center</h2>
            
            <div class="premium-card p-5 mb-6 bg-slate-900 text-white">
                <h3 class="text-[10px] font-bold text-slate-400 uppercase mb-4">Promo Code Generator</h3>
                <div class="flex gap-2 mb-4">
                    <input type="text" id="adm-p-code" placeholder="CODE (e.g. GIFT)" class="flex-1 p-3 rounded-xl text-black text-xs uppercase font-bold">
                    <input type="number" id="adm-p-val" placeholder="Value $" class="w-24 p-3 rounded-xl text-black text-xs font-bold">
                </div>
                <button onclick="createPromo()" class="w-full btn-gold py-3 text-[10px] uppercase tracking-widest">Activate Promo Code</button>
            </div>

            <div class="premium-card p-5 mb-6">
                <h3 class="text-[10px] font-black uppercase mb-4">User Management</h3>
                <div id="adm-user-list" class="space-y-3 max-h-64 overflow-y-auto"></div>
            </div>

            <h3 class="text-[10px] font-bold uppercase mb-3 text-slate-400">Financial Requests</h3>
            <div id="adm-pending-list" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-layer-group text-lg"></i><span>HOME</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip text-lg"></i><span>NODES</span></div>
            <div onclick="nav('page-team', this)" class="nav-item"><i class="fa-solid fa-users-viewfinder text-lg"></i><span>TEAM</span></div>
            <div onclick="nav('page-admin', this)" id="nav-admin-btn" class="nav-item text-red-600" style="display:none;"><i class="fa-solid fa-terminal text-lg"></i><span>ADMIN</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-red-400 text-lg"></i><span>EXIT</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-8"><h2 class="font-black text-xl">FUNDING</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-xl"></i></div>
        <div class="grid grid-cols-3 gap-2 mb-6">
            <button onclick="showWay('EP')" class="border p-4 rounded-2xl text-[9px] font-black uppercase">EasyPaisa</button>
            <button onclick="showWay('JC')" class="border p-4 rounded-2xl text-[9px] font-black uppercase">JazzCash</button>
            <button onclick="showWay('USDT')" class="border p-4 rounded-2xl text-[9px] font-black uppercase">USDT</button>
        </div>
        <div id="dep-info" class="bg-slate-100 p-6 rounded-3xl text-center mb-8 hidden font-black text-xs text-slate-600"><p id="dep-addr"></p></div>
        <input type="number" id="dep-amt" placeholder="Deposit Amount ($)" class="w-full p-5 bg-slate-50 border-none rounded-2xl mb-4 outline-none">
        <input type="text" id="dep-txid" placeholder="Transaction Hash / ID" class="w-full p-5 bg-slate-50 border-none rounded-2xl mb-8 outline-none">
        <button onclick="handleFinance('Deposit')" class="w-full btn-gold py-5 shadow-xl shadow-gold/30 uppercase text-xs tracking-tighter">Confirm Transaction</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, getDocs, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);

        let user = localStorage.getItem('nova_session');
        let mode = 'login'; let admClicks = 0;
        let sys = { ep: "Not Set", jc: "Not Set", usdt: "Not Set", tax: 0 };

        const nodes = [];
        for(let i=1; i<=21; i++) nodes.push({id:i, n:`Industrial Node V${i}`, c:50+(i*75), y:5+(i*12)});

        // Admin Secret
        document.getElementById('logo-main').onclick = () => {
            admClicks++;
            if(admClicks >= 5) {
                if(prompt("Admin Access Pin:") === "nov786") {
                    document.getElementById('nav-admin-btn').style.display = "flex";
                    loadMasterUsers(); alert("Authorized!");
                }
                admClicks = 0;
            }
        };

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            const refBy = document.getElementById('u-ref').value.toLowerCase().trim();
            if(!id || !pw) return alert("All fields required");
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);

            if(mode === 'login') {
                if(snap.exists() && snap.data().password === pw) {
                    localStorage.setItem('nova_session', id); location.reload();
                } else alert("Invalid ID or Password");
            } else {
                if(snap.exists()) return alert("Identifier Taken");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0, active_nodes: [], refBy: refBy || 'None', commission: 0 });
                localStorage.setItem('nova_session', id); location.reload();
            }
        };

        function initApp(id) {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('ref-link-display').innerText = window.location.origin + "?ref=" + id;
            
            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u) return;
                document.getElementById('user-display').innerText = `@${id}`;
                document.getElementById('b-main').innerText = `$${(u.balance || 0).toFixed(2)}`;
                document.getElementById('b-daily').innerText = `+$${(u.daily || 0).toFixed(2)}`;
                document.getElementById('b-total').innerText = `$${(u.total_profit || 0).toFixed(2)}`;
                document.getElementById('team-comm').innerText = `$${(u.commission || 0).toFixed(2)}`;
                renderNodes(u.active_nodes || []);
            });
            loadTeamData(id); loadAdminReqs();
        }

        // TIMER LOGIC
        setInterval(() => {
            const now = new Date();
            const h = 23 - now.getHours();
            const m = 59 - now.getMinutes();
            const s = 59 - now.getSeconds();
            document.getElementById('next-cycle').innerText = `Next Yield: ${h}:${m}:${s}`;
        }, 1000);

        // PROMO SYSTEM
        window.applyPromo = async () => {
            const code = document.getElementById('promo-input').value.toUpperCase().trim();
            const pRef = doc(db, "promos", code); const snap = await getDoc(pRef);
            if(snap.exists()) {
                const val = snap.data().value;
                const uRef = doc(db, "users", user);
                const uSnap = await getDoc(uRef);
                await updateDoc(uRef, { balance: uSnap.data().balance + val });
                await deleteDoc(pRef); alert(`$${val} Added Successfully!`);
            } else alert("Invalid or Expired Code");
        };

        window.createPromo = async () => {
            const code = document.getElementById('adm-p-code').value.toUpperCase().trim();
            const val = parseFloat(document.getElementById('adm-p-val').value);
            if(!code || !val) return;
            await setDoc(doc(db, "promos", code), { value: val });
            alert("Promo Code Live!");
        };

        // TEAM & REFERRAL
        async function loadTeamData(id) {
            const q = query(collection(db, "users"), where("refBy", "==", id));
            const snap = await getDocs(q);
            document.getElementById('team-count').innerText = snap.size;
            const list = document.getElementById('team-list'); list.innerHTML = "";
            snap.forEach(d => {
                list.innerHTML += `<div class="flex justify-between p-2 border-b"><span>@${d.id}</span><span class="text-green-500 uppercase">Active</span></div>`;
            });
        }

        window.copyRef = () => {
            navigator.clipboard.writeText(window.location.origin + "?ref=" + user);
            alert("Referral link copied!");
        };

        // RENDER NODES
        function renderNodes(active) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            nodes.forEach(n => {
                const isRun = active.find(a => a.nodeId === n.id);
                grid.innerHTML += `<div class="premium-card p-5 flex justify-between items-center">
                    <div><h4 class="font-black text-[11px] uppercase">${n.n}</h4><p class="text-[9px] text-slate-400">$${n.c} • Yield: $${n.y}/d</p></div>
                    ${isRun ? `<span class="text-green-500 font-black text-[9px] tracking-widest">MINING...</span>` : `<button onclick="buyNode(${n.id}, ${n.c}, ${n.y})" class="btn-gold px-6 py-2 text-[10px]">DEPLOY</button>`}
                </div>`;
            });
        }

        // ADMIN TOOLS
        async function loadMasterUsers() {
            const q = await getDocs(collection(db, "users"));
            const box = document.getElementById('adm-user-list'); box.innerHTML = "";
            q.forEach(uDoc => {
                const u = uDoc.data();
                box.innerHTML += `<div class="p-4 border rounded-2xl flex justify-between bg-slate-50 text-[10px]">
                    <div><b>@${uDoc.id}</b><br>Bal: $${u.balance.toFixed(2)}</div>
                    <div class="flex gap-2">
                        <button onclick="editBal('${uDoc.id}')" class="bg-slate-800 text-white px-3 py-1 rounded-lg">EDIT</button>
                        <button onclick="delUser('${uDoc.id}')" class="text-red-500">DEL</button>
                    </div>
                </div>`;
            });
        }

        window.editBal = async (id) => {
            const newVal = parseFloat(prompt("Enter New Balance:"));
            if(!isNaN(newVal)) await updateDoc(doc(db, "users", id), { balance: newVal });
            loadMasterUsers();
        };

        // UTILS
        window.nav = (id, el) => { document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen')); document.getElementById(id).classList.add('active-screen'); document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active'); };
        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.logout = () => { localStorage.clear(); location.reload(); };
        window.toggleAuth = () => { mode = mode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = mode === 'login' ? "Create Operational Account" : "Access Console"; };
        window.showWay = (t) => { document.getElementById('dep-info').classList.remove('hidden'); document.getElementById('dep-addr').innerText = t + ": " + (sys[t.toLowerCase()] || "Admin Config Needed"); };

        if(user) initApp(user);
    </script>
</body>
</html>
