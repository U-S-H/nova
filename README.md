<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | Master Terminal V2</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0F172A; }
        body { background: #F8FAFC; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; -webkit-tap-highlight-color: transparent; }
        .premium-card { background: white; border: 1px solid #e2e8f0; border-radius: 24px; box-shadow: 0 4px 20px rgba(0,0,0,0.02); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 14px; font-weight: 800; cursor: pointer; border: none; transition: 0.3s; }
        .btn-gold:active { transform: scale(0.95); }
        .screen { display: none; }
        .active-screen { display: block; animation: slideUp 0.4s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.9); backdrop-filter: blur(10px); border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 15px 5px; z-index: 1000; transition: transform 0.3s; }
        .nav-hidden { transform: translateY(100%); }
        .nav-item { color: #94a3b8; font-size: 8px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: var(--gold); }
        input { border: 1px solid #e2e8f0 !important; font-size: 14px !important; }
    </style>
</head>
<body>

    <div id="auth-screen" class="active-screen">
        <section class="p-8 text-center bg-white min-h-screen flex flex-col justify-center">
            <h1 class="text-5xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <div class="space-y-4 max-w-xs mx-auto w-full mt-10">
                <input type="text" id="u-id" placeholder="Username" class="w-full p-4 rounded-2xl outline-none">
                <input type="password" id="u-pw" placeholder="Password" class="w-full p-4 rounded-2xl outline-none">
                <button onclick="handleAuth()" class="w-full btn-gold py-4 uppercase text-xs">Login / Join</button>
                <p onclick="toggleAuth()" id="auth-btn-text" class="text-[10px] text-slate-400 font-bold uppercase cursor-pointer underline mt-4">Create New Account</p>
            </div>
        </section>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white border-b sticky top-0 z-[100]">
            <h2 id="logo-main" class="font-black text-2xl tracking-tighter cursor-pointer select-none">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="text-right">
                <p id="user-display" class="text-[10px] font-black uppercase">@USER</p>
                <p class="text-[8px] font-bold text-green-500 uppercase">System Active</p>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6 pb-24">
            <div class="bg-slate-900 rounded-[32px] p-8 mb-6 text-white shadow-xl relative overflow-hidden">
                <p class="text-[8px] text-slate-500 font-bold uppercase tracking-widest mb-1">Current Balance</p>
                <h1 id="b-main" class="text-5xl font-black tracking-tighter mb-8">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                    <div><p class="text-[7px] text-slate-500 font-bold uppercase">Today Profit</p><p id="b-daily" class="text-lg font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[7px] text-slate-500 font-bold uppercase">Total Earned</p><p id="b-total" class="text-lg font-black text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('modal-dep')" class="premium-card p-6 flex flex-col items-center gap-2"><i class="fa-solid fa-circle-plus text-[#D4AF37]"></i><span class="text-[10px] font-black">DEPOSIT</span></button>
                <button onclick="openModal('modal-wd')" class="premium-card p-6 flex flex-col items-center gap-2"><i class="fa-solid fa-wallet text-slate-400"></i><span class="text-[10px] font-black">WITHDRAW</span></button>
            </div>

            <div class="premium-card p-6 border-dashed border-2 border-[#D4AF37] bg-amber-50/30">
                <div class="flex justify-between items-center">
                    <div>
                        <p class="text-[8px] font-black uppercase text-slate-400">Invite & Earn</p>
                        <h3 class="text-xs font-black">Referral Link</h3>
                    </div>
                    <button onclick="copyRef()" class="btn-gold px-4 py-2 text-[8px]">COPY</button>
                </div>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6 pb-24"><div id="nodes-grid" class="space-y-4"></div></div>
        <div id="page-logs" class="screen px-4 pt-6 pb-24"><div id="logs-list" class="space-y-3"></div></div>

        <div id="page-admin" class="screen px-4 pt-6 pb-24">
            <h2 class="text-xl font-black mb-6 text-red-600 uppercase">Master Admin</h2>
            <div class="premium-card p-5 mb-6 bg-slate-900 text-white">
                <h3 class="text-[10px] font-bold text-slate-400 uppercase mb-4">Master Config</h3>
                <input type="text" id="adm-ep" placeholder="EasyPaisa No" class="w-full p-3 rounded-xl text-black mb-2">
                <input type="text" id="adm-jc" placeholder="JazzCash No" class="w-full p-3 rounded-xl text-black mb-2">
                <input type="text" id="adm-usdt" placeholder="USDT Address" class="w-full p-3 rounded-xl text-black mb-2">
                <input type="number" id="adm-tax" placeholder="Tax %" class="w-full p-3 rounded-xl text-black mb-4">
                <button onclick="saveAdminSettings()" class="w-full btn-gold py-3 text-xs">Update Settings</button>
            </div>
            <div id="adm-pending-list" class="space-y-3 mb-6"></div>
            <div id="adm-user-list" class="space-y-2 max-h-60 overflow-y-auto"></div>
        </div>

        <nav class="nav-bar" id="bottom-nav">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-house"></i><span>HOME</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-server"></i><span>NODES</span></div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-list-ul"></i><span>LOGS</span></div>
            <div id="nav-admin-btn" onclick="nav('page-admin', this)" class="nav-item text-red-600" style="display: none;"><i class="fa-solid fa-lock"></i><span>ADMIN</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-red-400"></i><span>EXIT</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-8"><h2 class="font-black text-xl text-slate-900 tracking-tighter">DEPOSIT FUNDS</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-2xl"></i></div>
        <div class="grid grid-cols-3 gap-2 mb-6">
            <button onclick="showWay('EP')" class="border p-3 rounded-xl text-[9px] font-black uppercase">EasyPaisa</button>
            <button onclick="showWay('JC')" class="border p-3 rounded-xl text-[9px] font-black uppercase">JazzCash</button>
            <button onclick="showWay('USDT')" class="border p-3 rounded-xl text-[9px] font-black uppercase">USDT</button>
        </div>
        <div id="dep-info" class="bg-slate-100 p-6 rounded-2xl text-center mb-8 hidden"><p id="dep-addr" class="text-xs font-black break-all text-slate-600"></p></div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="w-full p-4 border rounded-2xl mb-4">
        <input type="text" id="dep-txid" placeholder="Transaction ID" class="w-full p-4 border rounded-2xl mb-8">
        <button onclick="handleFinance('Deposit')" class="w-full btn-gold py-4">SUBMIT DEPOSIT</button>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-white z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-8"><h2 class="font-black text-xl tracking-tighter">WITHDRAW</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark text-2xl"></i></div>
        <input type="number" id="wd-amt" placeholder="Amount ($)" class="w-full p-4 border rounded-2xl mb-4">
        <input type="text" id="wd-addr" placeholder="Account Number / Wallet" class="w-full p-4 border rounded-2xl mb-8">
        <button onclick="handleFinance('Withdraw')" class="w-full btn-gold py-4">REQUEST PAYOUT</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, getDocs, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);

        let user = localStorage.getItem('nova_session');
        let mode = 'login'; let admClicks = 0;
        let sys = { ep: "Not Set", jc: "Not Set", usdt: "Not Set", tax: 0 };

        const nodes = [{id:1, n:"Micro Node", c:10, y:0.9}];
        for(let i=2; i<=21; i++) nodes.push({id:i, n:`Industrial Node V${i}`, c:20+(i*50), y:2+(i*6)});

        // Admin Secret Trigger
        document.getElementById('logo-main').onclick = () => {
            admClicks++;
            if(admClicks >= 5) {
                if(prompt("Enter Admin Pin:") === "nov786") {
                    document.getElementById('nav-admin-btn').style.display = "flex";
                    loadMasterUsers();
                    alert("Admin Access Granted!");
                }
                admClicks = 0;
            }
        };

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("Fields required");
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(mode === 'login') {
                if(snap.exists() && snap.data().password === pw) {
                    localStorage.setItem('nova_session', id); location.reload();
                } else alert("Wrong credentials");
            } else {
                if(snap.exists()) return alert("Username taken");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0, active_nodes: [], status: 'Active' });
                localStorage.setItem('nova_session', id); location.reload();
            }
        };

        window.copyRef = () => {
            const link = `${window.location.origin}${window.location.pathname}?ref=${user}`;
            navigator.clipboard.writeText(link);
            alert("Referral link copied sweetie!");
        };

        async function loadMasterUsers() {
            const q = await getDocs(collection(db, "users"));
            const box = document.getElementById('adm-user-list');
            box.innerHTML = `<h3 class="text-[10px] font-black uppercase mb-4 text-slate-400">Total Users</h3>`;
            q.forEach(uDoc => {
                const u = uDoc.data();
                box.innerHTML += `<div class="p-3 border rounded-xl flex justify-between bg-slate-50 text-[10px] mb-2">
                    <div><b>@${uDoc.id}</b><br>PW: ${u.password}</div>
                    <div class="text-right">Bal: $${(u.balance || 0).toFixed(2)}<br><span class="text-red-600 font-bold cursor-pointer" onclick="delUser('${uDoc.id}')">DELETE</span></div>
                </div>`;
            });
        }

        window.saveAdminSettings = async () => {
            const up = { ep: document.getElementById('adm-ep').value, jc: document.getElementById('adm-jc').value, usdt: document.getElementById('adm-usdt').value, tax: parseFloat(document.getElementById('adm-tax').value) || 0 };
            await setDoc(doc(db, "config", "master"), up, { merge: true });
            alert("Master settings updated!");
        };

        function initApp(id) {
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
            loadAdminReqs(); loadUserLogs(id);
        }

        function renderNodes(active) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = `<h2 class="text-xl font-black mb-4 uppercase tracking-tighter">Hardware Fleet</h2>`;
            nodes.forEach(n => {
                const isRun = active.find(a => a.nodeId === n.id);
                grid.innerHTML += `<div class="premium-card p-5 flex justify-between items-center bg-white border-l-4 ${isRun?'border-green-500':'border-slate-200'}">
                    <div><h4 class="font-black text-xs uppercase">${n.n}</h4><p class="text-[9px] text-slate-400">$${n.c} • Yield: $${n.y}/d</p></div>
                    ${isRun ? `<span class="text-green-500 font-black text-[9px] tracking-widest animate-pulse">MINING...</span>` : `<button onclick="buyNode(${n.id}, ${n.c}, ${n.y})" class="btn-gold px-6 py-2 text-[10px]">BUY</button>`}
                </div>`;
            });
        }

        window.buyNode = async (id, cost, y) => {
            const uRef = doc(db, "users", user); const snap = await getDoc(uRef);
            if(snap.data().balance < cost) return alert("Insufficient balance sweetie");
            await updateDoc(uRef, { balance: snap.data().balance - cost, active_nodes: [...(snap.data().active_nodes || []), { nodeId: id, yield: y }], daily: (snap.data().daily || 0) + y });
            alert("Hardware Activated Successfully!");
        };

        window.showWay = (t) => {
            document.getElementById('dep-info').classList.remove('hidden');
            document.getElementById('dep-addr').innerText = t + ": " + (sys[t.toLowerCase()] || "Contact Support");
        };

        window.handleFinance = async (type) => {
            const amt = parseFloat(document.getElementById(type === 'Deposit' ? 'dep-amt' : 'wd-amt').value);
            const ref = document.getElementById(type === 'Deposit' ? 'dep-txid' : 'wd-addr').value;
            if(!amt || !ref) return alert("Fill all details");
            await addDoc(collection(db, "logs"), { user: user, type, amount: amt, ref, status: 'Pending', time: Date.now() });
            alert("Request sent to admin!"); closeModal(type === 'Deposit' ? 'modal-dep' : 'modal-wd');
        };

        async function loadAdminReqs() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const list = document.getElementById('adm-pending-list'); 
                list.innerHTML = `<h3 class="text-[10px] font-black uppercase mb-3 text-slate-400">Approval Requests</h3>`;
                s.forEach(ds => {
                    const l = ds.data();
                    list.innerHTML += `<div class="premium-card p-4 text-[9px] flex flex-col gap-3 bg-white border-l-4 border-orange-400 mb-2">
                        <div class="flex justify-between items-start">
                            <div><b>@${l.user}</b>: $${l.amount} (${l.type})<br><span class="text-slate-400">Ref: ${l.ref}</span></div>
                            <div class="text-[7px] font-bold text-slate-300">${new Date(l.time).toLocaleTimeString()}</div>
                        </div>
                        <div class="flex gap-2">
                            <button onclick="approve('${ds.id}', '${l.user}', ${l.amount}, '${l.type}')" class="flex-1 bg-green-600 text-white py-2 rounded-xl font-bold uppercase">Approve</button>
                            <button onclick="rejectReq('${ds.id}')" class="flex-1 bg-red-600 text-white py-2 rounded-xl font-bold uppercase">Reject</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.approve = async (lid, uid, amt, type) => {
            const uRef = doc(db, "users", uid); const s = await getDoc(uRef);
            if(type === 'Deposit') await updateDoc(uRef, { balance: (s.data().balance || 0) + amt });
            if(type === 'Withdraw') {
                const finalAmt = amt + (amt * (sys.tax / 100));
                if(s.data().balance < finalAmt) return alert("User has low balance now!");
                await updateDoc(uRef, { balance: (s.data().balance || 0) - finalAmt });
            }
            await updateDoc(doc(db, "logs", lid), { status: 'Approved' });
            alert("Transaction Approved!");
        };

        window.rejectReq = async (lid) => {
            if(confirm("Reject this transaction?")) {
                await updateDoc(doc(db, "logs", lid), { status: 'Rejected' });
                alert("Transaction Rejected!");
            }
        };

        function loadUserLogs(id) {
            onSnapshot(query(collection(db, "logs"), where("user", "==", id)), s => {
                const list = document.getElementById('logs-list'); 
                list.innerHTML = `<h2 class="text-xl font-black mb-4 uppercase tracking-tighter">Activity Logs</h2>`;
                s.forEach(d => {
                    const l = d.data();
                    const stColor = l.status === 'Approved' ? 'text-green-500' : (l.status === 'Rejected' ? 'text-red-500' : 'text-orange-500');
                    list.innerHTML += `<div class="premium-card p-4 flex justify-between items-center text-[10px] mb-2">
                        <div><b class="uppercase">${l.type}</b><br><span class="text-slate-400 font-bold text-[8px]">${new Date(l.time).toLocaleDateString()}</span></div>
                        <div class="text-right"><b>$${l.amount}</b><br><span class="${stColor} font-black uppercase text-[8px]">${l.status}</span></div>
                    </div>`;
                });
            });
        }

        // UI Utility Functions
        window.delUser = async (id) => { if(confirm(`Permanently delete @${id}?`)) await deleteDoc(doc(db, "users", id)); loadMasterUsers(); };
        window.nav = (id, el) => { document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen')); document.getElementById(id).classList.add('active-screen'); document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active'); window.scrollTo(0,0); };
        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.logout = () => { if(confirm("Logout sweetie?")) { localStorage.clear(); location.reload(); } };
        window.toggleAuth = () => { mode = mode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = mode === 'login' ? "Create New Account" : "Login Instead"; };

        // Scroll Hide Nav Logic
        let lastScroll = 0;
        window.addEventListener("scroll", () => {
            const currentScroll = window.pageYOffset;
            const nav = document.getElementById("bottom-nav");
            if (currentScroll > lastScroll && currentScroll > 100) nav.classList.add("nav-hidden");
            else nav.classList.remove("nav-hidden");
            lastScroll = currentScroll;
        });

        if(user) initApp(user);
    </script>
</body>
</html>
