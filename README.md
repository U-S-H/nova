<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | FINAL ULTIMATE</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0F172A; }
        body { background: #F8FAFC; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; overflow-x: hidden; }
        .premium-card { background: white; border: 1px solid #e2e8f0; border-radius: 28px; box-shadow: 0 4px 25px rgba(0,0,0,0.03); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 16px; font-weight: 800; cursor: pointer; transition: 0.3s; border: none; }
        .btn-gold:active { transform: scale(0.95); }
        .screen { display: none; min-height: 100vh; width: 100%; }
        .active-screen { display: block; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.96); backdrop-filter: blur(15px); border-top: 1px solid #f1f5f9; display: flex; justify-content: space-around; padding: 12px 0; z-index: 1000; padding-bottom: calc(12px + env(safe-area-inset-bottom)); }
        .nav-item { color: #94a3b8; font-size: 9px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 5px; transition: 0.3s; }
        .nav-item.active { color: var(--gold); }
        input { border: 1.5px solid #f1f5f9 !important; border-radius: 16px !important; padding: 14px !important; transition: 0.3s; }
        input:focus { border-color: var(--gold) !important; background: white !important; }
    </style>
</head>
<body>

    <div id="auth-screen" class="screen active-screen flex flex-col justify-center items-center p-8 bg-white">
        <div class="text-center mb-12">
            <h1 class="text-5xl font-black italic tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[10px] text-slate-400 font-bold uppercase tracking-[0.3em] mt-2">Institutional Terminal</p>
        </div>
        <div class="w-full max-w-xs space-y-4">
            <input type="text" id="u-id" placeholder="Username" class="w-full bg-slate-50 outline-none">
            <input type="password" id="u-pw" placeholder="Password" class="w-full bg-slate-50 outline-none">
            <input type="text" id="u-ref" placeholder="Referral ID (Optional)" class="w-full bg-slate-50 outline-none">
            <button onclick="handleAuth()" class="w-full btn-gold py-5 uppercase text-xs tracking-widest shadow-lg shadow-gold/20">Access Terminal</button>
            <p onclick="toggleAuth()" id="auth-btn-text" class="text-center text-[11px] font-bold text-slate-400 cursor-pointer underline">New? Create Operational Account</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white/80 backdrop-blur-md border-b sticky top-0 z-[100]">
            <h2 id="logo-main" class="font-black text-2xl tracking-tighter select-none">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="text-right">
                <p id="user-display" class="text-[10px] font-black uppercase text-slate-900">@USER</p>
                <p class="text-[8px] font-bold text-green-500 flex items-center justify-end gap-1"><span class="w-1.5 h-1.5 bg-green-500 rounded-full animate-pulse"></span> ONLINE</p>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6 pb-32">
            <div class="bg-slate-950 rounded-[35px] p-8 mb-6 text-white shadow-2xl relative overflow-hidden">
                <p class="text-[9px] text-slate-500 font-bold uppercase tracking-widest mb-1">Portfolio Balance</p>
                <h1 id="b-main" class="text-5xl font-black tracking-tighter mb-10">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                    <div><p class="text-[7px] text-slate-500 font-bold uppercase">Today's Yield</p><p id="b-daily" class="text-xl font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[7px] text-slate-500 font-bold uppercase">Total Profit</p><p id="b-total" class="text-xl font-black text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('modal-dep')" class="premium-card p-6 flex flex-col items-center gap-3"><i class="fa-solid fa-plus-circle text-[#D4AF37] text-2xl"></i><span class="text-[10px] font-black uppercase">Deposit</span></button>
                <button onclick="openModal('modal-wd')" class="premium-card p-6 flex flex-col items-center gap-3"><i class="fa-solid fa-wallet text-slate-400 text-2xl"></i><span class="text-[10px] font-black uppercase">Withdraw</span></button>
            </div>

            <div class="premium-card p-5 mb-6 border-dashed border-2 border-gold/20">
                <h3 class="text-[10px] font-black uppercase mb-3">Redeem Promo</h3>
                <div class="flex gap-2">
                    <input type="text" id="promo-input" placeholder="Code" class="flex-1 bg-slate-50 text-xs font-bold uppercase border-none">
                    <button onclick="applyPromo()" class="btn-gold px-6 text-[10px]">CLAIM</button>
                </div>
            </div>

            <div class="premium-card p-5">
                <h3 class="text-[10px] font-black uppercase mb-2">Invite Friends</h3>
                <div class="flex items-center justify-between bg-slate-50 p-3 rounded-2xl">
                    <p id="ref-link-display" class="text-[10px] font-bold text-slate-400 truncate mr-4 italic">Loading Link...</p>
                    <i onclick="copyRef()" class="fa-solid fa-copy text-gold cursor-pointer p-2"></i>
                </div>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6 pb-32">
            <div class="flex justify-between items-center mb-6 px-2">
                <h3 class="font-black text-xl italic uppercase">Mining Nodes</h3>
                <span class="bg-red-50 text-red-500 text-[8px] font-black px-2 py-1 rounded-full uppercase">Live Nodes: 21</span>
            </div>
            <div id="nodes-grid" class="space-y-4"></div>
        </div>

        <div id="page-team" class="screen px-4 pt-6 pb-32">
            <h3 class="font-black text-xl italic uppercase mb-6 px-2">My Network</h3>
            <div class="grid grid-cols-2 gap-4 mb-6">
                <div class="premium-card p-5 text-center"><p class="text-[8px] font-bold text-slate-400 uppercase">Team Size</p><h2 id="team-count" class="text-2xl font-black">0</h2></div>
                <div class="premium-card p-5 text-center"><p class="text-[8px] font-bold text-slate-400 uppercase">Comm. Earned</p><h2 id="team-comm" class="text-2xl font-black text-green-500">$0.00</h2></div>
            </div>
            <div class="premium-card p-5">
                <h4 class="text-[10px] font-black uppercase mb-4 border-b pb-2">Downline Members</h4>
                <div id="team-list" class="space-y-3"></div>
            </div>
        </div>

        <div id="page-admin" class="screen px-4 pt-6 pb-32">
            <h2 class="text-2xl font-black mb-6 text-red-600 uppercase italic">Command Center</h2>
            
            <div class="premium-card p-6 mb-6 bg-slate-950 text-white">
                <h3 class="text-[10px] font-bold text-slate-400 uppercase mb-4">Promo Generator</h3>
                <div class="flex gap-2 mb-4">
                    <input type="text" id="adm-p-code" placeholder="CODE" class="flex-1 p-3 rounded-xl text-black text-xs font-bold uppercase">
                    <input type="number" id="adm-p-val" placeholder="$" class="w-20 p-3 rounded-xl text-black text-xs font-bold">
                </div>
                <button onclick="createPromo()" class="w-full btn-gold py-4 text-[10px] uppercase">Generate Reward Code</button>
            </div>

            <div class="premium-card p-5 mb-6">
                <h3 class="text-[10px] font-black uppercase mb-4">Master User List</h3>
                <div id="adm-user-list" class="space-y-3 max-h-80 overflow-y-auto"></div>
            </div>

            <h3 class="text-[10px] font-bold uppercase mb-3 text-slate-400">Approval Queue</h3>
            <div id="adm-pending-list" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-house-chimney text-lg"></i><span>HOME</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip text-lg"></i><span>NODES</span></div>
            <div onclick="nav('page-team', this)" class="nav-item"><i class="fa-solid fa-users text-lg"></i><span>TEAM</span></div>
            <div onclick="nav('page-admin', this)" id="nav-admin-btn" class="nav-item text-red-600" style="display:none;"><i class="fa-solid fa-shield-halved text-lg"></i><span>ADMIN</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-red-400 text-lg"></i><span>EXIT</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden animate-slide-up">
        <div class="flex justify-between items-center mb-8"><h2 class="font-black text-2xl uppercase italic">Funding</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-circle-xmark text-2xl text-slate-200"></i></div>
        <div class="grid grid-cols-3 gap-2 mb-6">
            <button onclick="showWay('EP')" class="border p-4 rounded-2xl text-[9px] font-black uppercase">EasyPaisa</button>
            <button onclick="showWay('JC')" class="border p-4 rounded-2xl text-[9px] font-black uppercase">JazzCash</button>
            <button onclick="showWay('USDT')" class="border p-4 rounded-2xl text-[9px] font-black uppercase">USDT</button>
        </div>
        <div id="dep-info" class="bg-slate-50 p-6 rounded-3xl text-center mb-8 hidden border border-dashed"><p id="dep-addr" class="text-xs font-bold text-slate-600"></p></div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="w-full bg-slate-50 mb-4 outline-none border-none">
        <input type="text" id="dep-txid" placeholder="Transaction ID" class="w-full bg-slate-50 mb-8 outline-none border-none">
        <button onclick="handleFinance('Deposit')" class="w-full btn-gold py-5 shadow-2xl shadow-gold/30 uppercase text-xs">Confirm Payment</button>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-white z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-8"><h2 class="font-black text-2xl uppercase italic">Withdraw</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-circle-xmark text-2xl text-slate-200"></i></div>
        <input type="number" id="wd-amt" placeholder="Amount ($)" class="w-full bg-slate-50 mb-4 outline-none border-none">
        <input type="text" id="wd-addr" placeholder="Wallet Address / Number" class="w-full bg-slate-50 mb-8 outline-none border-none">
        <button onclick="handleFinance('Withdraw')" class="w-full btn-gold py-5 uppercase text-xs">Request Payout</button>
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
        for(let i=1; i<=21; i++) nodes.push({id:i, n:`Titanium Node V${i}`, c:50+(i*80), y:4+(i*10)});

        // Admin Secret Trigger
        document.getElementById('logo-main').onclick = () => {
            admClicks++;
            if(admClicks >= 5) {
                if(prompt("Security Authorization:") === "nov786") {
                    document.getElementById('nav-admin-btn').style.display = "flex";
                    loadMasterUsers(); alert("Access Granted!");
                }
                admClicks = 0;
            }
        };

        // CORE AUTH
        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            const refBy = document.getElementById('u-ref').value.toLowerCase().trim() || 'None';
            if(!id || !pw) return alert("Credentials required");
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);

            if(mode === 'login') {
                if(snap.exists() && snap.data().password === pw) {
                    localStorage.setItem('nova_session', id); location.reload();
                } else alert("Invalid Access");
            } else {
                if(snap.exists()) return alert("Username unavailable");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0, active_nodes: [], refBy: refBy, commission: 0 });
                localStorage.setItem('nova_session', id); location.reload();
            }
        };

        function initApp(id) {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('ref-link-display').innerText = window.location.origin + window.location.pathname + "?ref=" + id;
            
            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u) return;
                document.getElementById('user-display').innerText = `@${id}`;
                document.getElementById('b-main').innerText = `$${(u.balance || 0).toFixed(2)}`;
                document.getElementById('b-daily').innerText = `+$${(u.daily || 0).toFixed(2)}`;
                document.getElementById('b-total').innerText = `$${(u.total_profit || 0).toFixed(2)}`;
                document.getElementById('team-comm').innerText = `$${(u.commission || 0).toFixed(2)}`;
                renderNodes(u.active_nodes || []);
            });
            onSnapshot(doc(db, "config", "master"), d => { if(d.exists()) sys = d.data(); });
            loadTeamData(id); loadAdminReqs();
        }

        // PROMO
        window.applyPromo = async () => {
            const code = document.getElementById('promo-input').value.toUpperCase().trim();
            const pRef = doc(db, "promos", code); const snap = await getDoc(pRef);
            if(snap.exists()) {
                const val = snap.data().value;
                const uRef = doc(db, "users", user);
                const uSnap = await getDoc(uRef);
                await updateDoc(uRef, { balance: uSnap.data().balance + val });
                await deleteDoc(pRef); alert(`Bonus $${val} Applied!`);
            } else alert("Invalid Code");
        };

        window.createPromo = async () => {
            const code = document.getElementById('adm-p-code').value.toUpperCase().trim();
            const val = parseFloat(document.getElementById('adm-p-val').value);
            if(!code || !val) return;
            await setDoc(doc(db, "promos", code), { value: val });
            alert("Promo Active!");
        };

        // NODES
        function renderNodes(active) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            nodes.forEach(n => {
                const isRun = active.find(a => a.nodeId === n.id);
                grid.innerHTML += `<div class="premium-card p-6 flex justify-between items-center">
                    <div><h4 class="font-black text-[11px] uppercase">${n.n}</h4><p class="text-[9px] text-slate-400">$${n.c} • Yield: $${n.y}/day</p></div>
                    ${isRun ? `<span class="text-green-500 font-black text-[10px] animate-pulse">MINING</span>` : `<button onclick="buyNode(${n.id}, ${n.c}, ${n.y})" class="btn-gold px-6 py-2 text-[10px]">DEPLOY</button>`}
                </div>`;
            });
        }

        window.buyNode = async (id, cost, y) => {
            const uRef = doc(db, "users", user); const snap = await getDoc(uRef);
            if(snap.data().balance < cost) return alert("Insufficient Liquidity");
            await updateDoc(uRef, { balance: snap.data().balance - cost, active_nodes: [...(snap.data().active_nodes || []), { nodeId: id, yield: y }], daily: (snap.data().daily || 0) + y });
            alert("Hardware Online!");
        };

        // TEAM
        async function loadTeamData(id) {
            const q = query(collection(db, "users"), where("refBy", "==", id));
            onSnapshot(q, snap => {
                document.getElementById('team-count').innerText = snap.size;
                const list = document.getElementById('team-list'); list.innerHTML = "";
                snap.forEach(d => {
                    list.innerHTML += `<div class="flex justify-between p-3 border rounded-xl text-[10px] font-bold"><span>@${d.id}</span><span class="text-green-500 uppercase">Deployed</span></div>`;
                });
            });
        }

        // ADMIN OPS
        async function loadMasterUsers() {
            onSnapshot(collection(db, "users"), q => {
                const box = document.getElementById('adm-user-list'); box.innerHTML = "";
                q.forEach(uDoc => {
                    const u = uDoc.data();
                    box.innerHTML += `<div class="p-4 border rounded-2xl flex justify-between items-center bg-slate-50 text-[10px]">
                        <div><b>@${uDoc.id}</b><br>Bal: $${u.balance.toFixed(2)}</div>
                        <div class="flex gap-2">
                            <button onclick="editBal('${uDoc.id}')" class="bg-black text-white px-3 py-1 rounded-lg">EDIT</button>
                            <button onclick="delUser('${uDoc.id}')" class="text-red-500 font-bold p-1">DEL</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.editBal = async (id) => {
            const v = parseFloat(prompt("Set New Balance:"));
            if(!isNaN(v)) await updateDoc(doc(db, "users", id), { balance: v });
        };

        async function loadAdminReqs() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const list = document.getElementById('adm-pending-list'); list.innerHTML = "";
                s.forEach(ds => {
                    const l = ds.data();
                    list.innerHTML += `<div class="premium-card p-4 text-[9px] flex justify-between items-center border-l-4 border-l-gold">
                        <div><b>${l.user}</b>: $${l.amount} (${l.type})<br>Ref: ${l.ref}</div>
                        <div class="flex gap-2">
                             <button onclick="approve('${ds.id}', '${l.user}', ${l.amount}, '${l.type}')" class="bg-slate-900 text-white px-4 py-2 rounded-xl font-bold uppercase">Approve</button>
                             <button onclick="rejectReq('${ds.id}')" class="text-red-600 font-bold">X</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.approve = async (lid, uid, amt, type) => {
            const uRef = doc(db, "users", uid); const s = await getDoc(uRef);
            if(type === 'Deposit') await updateDoc(uRef, { balance: (s.data().balance || 0) + amt });
            if(type === 'Withdraw') await updateDoc(uRef, { balance: (s.data().balance || 0) - amt });
            await updateDoc(doc(db, "logs", lid), { status: 'Approved' });
            alert("Action Confirmed!");
        };

        // UTILS
        window.handleFinance = async (type) => {
            const amt = parseFloat(document.getElementById(type === 'Deposit' ? 'dep-amt' : 'wd-amt').value);
            const ref = document.getElementById(type === 'Deposit' ? 'dep-txid' : 'wd-addr').value;
            if(!amt || !ref) return alert("Missing Info");
            await addDoc(collection(db, "logs"), { user, type, amount: amt, ref, status: 'Pending' });
            alert("Forwarded to Admin!"); closeModal(type === 'Deposit' ? 'modal-dep' : 'modal-wd');
        };

        window.copyRef = () => { navigator.clipboard.writeText(document.getElementById('ref-link-display').innerText); alert("Link Copied!"); };
        window.nav = (id, el) => { document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen')); document.getElementById(id).classList.add('active-screen'); document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active'); };
        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.logout = () => { localStorage.clear(); location.reload(); };
        window.toggleAuth = () => { mode = mode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = mode === 'login' ? "New? Create Operational Account" : "Return to Login Console"; };
        window.showWay = (t) => { document.getElementById('dep-info').classList.remove('hidden'); document.getElementById('dep-addr').innerText = t + ": " + (sys[t.toLowerCase()] || "Contact Specialist"); };

        if(user) initApp(user);
    </script>
</body>
</html>
