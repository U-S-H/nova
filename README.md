<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | Institutional Mining</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --bg: #F8FAFC; --card: #FFFFFF; }
        body { background: var(--bg); font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; overflow-x: hidden; }
        .premium-card { background: var(--card); border: 1px solid #e2e8f0; border-radius: 28px; transition: 0.3s; box-shadow: 0 4px 20px rgba(0,0,0,0.02); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 16px; font-weight: 800; transition: 0.3s; text-align: center; cursor: pointer; }
        .screen { display: none; }
        .active-screen { display: block; animation: slideUp 0.6s cubic-bezier(0.16, 1, 0.3, 1); }
        @keyframes slideUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(20px); border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 15px 5px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 8px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: var(--gold); }
    </style>
</head>
<body>

    <div id="auth-screen" class="active-screen">
        <section class="p-8 text-center bg-white min-h-screen flex flex-col justify-center">
            <h1 class="text-5xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[8px] text-slate-400 font-bold uppercase tracking-[8px] mt-2 mb-8">Quantum Enterprise</p>
            
            <div class="bg-slate-50 p-5 rounded-2xl mb-6 text-left border border-slate-100">
                <h3 class="text-[10px] font-black uppercase mb-1">Company Profile</h3>
                <p class="text-[9px] text-slate-500 leading-tight">Nova Corp is a premier cloud mining provider. We leverage high-efficiency ASIC hardware to deliver stable daily rewards. Join our global network today.</p>
            </div>

            <div class="space-y-3 max-w-xs mx-auto w-full">
                <input type="text" id="u-id" placeholder="Username" class="w-full p-4 border border-slate-100 rounded-2xl outline-none focus:border-[#D4AF37]">
                <input type="password" id="u-pw" placeholder="Password" class="w-full p-4 border border-slate-100 rounded-2xl outline-none focus:border-[#D4AF37]">
                <button onclick="handleAuth()" class="w-full btn-gold py-4 uppercase text-xs">Access Terminal</button>
                <p onclick="toggleMode()" id="mode-text" class="text-[9px] text-slate-400 font-bold uppercase mt-4 cursor-pointer underline">Create Account</p>
            </div>
            
            <div class="mt-8 flex justify-center gap-6">
                <a id="link-wa" href="#" class="text-slate-400 hover:text-green-500"><i class="fa-brands fa-whatsapp text-2xl"></i></a>
                <a id="link-tg" href="#" class="text-slate-400 hover:text-blue-500"><i class="fa-brands fa-telegram text-2xl"></i></a>
            </div>
        </section>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white border-b border-slate-100 sticky top-0 z-[100]">
            <h2 id="logo-main" class="font-black text-2xl tracking-tighter cursor-pointer">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="flex flex-col items-end">
                <span id="user-tag" class="text-[10px] font-black text-slate-900 uppercase">@USER</span>
                <span id="user-status" class="text-[8px] font-bold text-green-500 uppercase">Status: Active</span>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6 pb-24">
            <div class="bg-slate-900 rounded-[30px] p-8 mb-6 text-white shadow-xl">
                <p class="text-[8px] text-slate-400 font-bold uppercase tracking-widest mb-1">Portfolio Balance</p>
                <h1 id="b-main" class="text-5xl font-black tracking-tighter mb-8">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                    <div><p class="text-[7px] text-slate-500 uppercase font-bold">Daily Yield</p><p id="b-daily" class="text-lg font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[7px] text-slate-500 uppercase font-bold">Total Profits</p><p id="b-total" class="text-lg font-black text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>
            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('modal-dep')" class="premium-card p-5 flex flex-col items-center gap-2"><i class="fa-solid fa-plus-circle text-[#D4AF37]"></i><span class="text-[9px] font-black">DEPOSIT</span></button>
                <button onclick="openModal('modal-wd')" class="premium-card p-5 flex flex-col items-center gap-2"><i class="fa-solid fa-arrow-up-right-from-square text-slate-400"></i><span class="text-[9px] font-black">WITHDRAW</span></button>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6 pb-24">
            <h2 class="text-xl font-black mb-6 text-center">QUANTUM <span class="text-[#D4AF37]">HARDWARE</span></h2>
            <div id="nodes-grid" class="space-y-4"></div>
        </div>

        <div id="page-admin" class="screen px-4 pt-6 pb-24">
            <h2 class="text-xl font-black mb-4 text-red-600 uppercase">Admin Terminal</h2>
            
            <div class="premium-card p-5 mb-4 bg-slate-900 text-white">
                <h3 class="text-[10px] font-bold mb-3 uppercase text-slate-400">Global Configuration</h3>
                <div class="grid grid-cols-2 gap-2 mb-3">
                    <input type="number" id="cfg-min-dep" placeholder="Min Dep" class="bg-slate-800 p-3 rounded-xl text-[10px] outline-none">
                    <input type="number" id="cfg-min-wd" placeholder="Min WD" class="bg-slate-800 p-3 rounded-xl text-[10px] outline-none">
                </div>
                <input type="number" id="cfg-tax" placeholder="Tax Percentage %" class="w-full bg-slate-800 p-3 rounded-xl text-[10px] outline-none mb-3">
                <button onclick="saveGlobalConfig()" class="w-full btn-gold py-2 text-[10px]">Update Global Rules</button>
            </div>

            <div class="premium-card p-5 mb-4">
                <h3 class="text-[10px] font-bold mb-3 uppercase">Wallet Management</h3>
                <input type="text" id="cfg-ep" placeholder="EasyPaisa No" class="w-full border p-3 rounded-xl text-[10px] mb-2">
                <input type="text" id="cfg-jc" placeholder="JazzCash No" class="w-full border p-3 rounded-xl text-[10px] mb-2">
                <input type="text" id="cfg-usdt" placeholder="USDT Address" class="w-full border p-3 rounded-xl text-[10px] mb-3">
                <button onclick="saveWallets()" class="w-full btn-gold py-2 text-[10px]">Save Addresses</button>
            </div>

            <div class="premium-card p-5 mb-4">
                <h3 class="text-[10px] font-bold mb-3 uppercase">User Control</h3>
                <input type="text" id="adm-u-target" placeholder="Target Username" class="w-full border p-3 rounded-xl text-[10px] mb-2">
                <div class="flex gap-2">
                    <button onclick="userAction('ban')" class="flex-1 bg-red-500 text-white py-2 rounded-xl text-[10px] font-bold">BAN</button>
                    <button onclick="userAction('unban')" class="flex-1 bg-green-500 text-white py-2 rounded-xl text-[10px] font-bold">UNBAN</button>
                    <button onclick="userAction('reset')" class="flex-1 bg-blue-500 text-white py-2 rounded-xl text-[10px] font-bold">RESET PW</button>
                </div>
            </div>

            <h3 class="text-[10px] font-bold mb-3 uppercase">Pending Requests</h3>
            <div id="adm-pending" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-house-chimney text-lg"></i><span>HOME</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip text-lg"></i><span>NODES</span></div>
            <div id="nav-admin-btn" onclick="nav('page-admin', this)" class="nav-item text-red-500" style="display: none;"><i class="fa-solid fa-shield-halved text-lg"></i><span>ADMIN</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-lg text-red-400"></i><span>LOGOUT</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-6"><h2 class="font-black">FUNDING</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-x"></i></div>
        <div class="space-y-3">
            <div id="dep-info" class="p-4 bg-slate-50 rounded-xl text-center mb-4">
                <p id="disp-method" class="text-[8px] font-bold text-slate-400 uppercase"></p>
                <p id="disp-addr" class="text-xs font-black"></p>
            </div>
            <div class="grid grid-cols-3 gap-2">
                <button onclick="selDep('EP')" class="border p-2 rounded-lg text-[8px] font-bold">EASYPAISA</button>
                <button onclick="selDep('JC')" class="border p-2 rounded-lg text-[8px] font-bold">JAZZCASH</button>
                <button onclick="selDep('USDT')" class="border p-2 rounded-lg text-[8px] font-bold">USDT</button>
            </div>
            <input type="number" id="in-amt" placeholder="Amount" class="w-full p-4 border rounded-xl outline-none">
            <input type="text" id="in-txid" placeholder="Transaction ID" class="w-full p-4 border rounded-xl outline-none">
            <button onclick="submitFin('Deposit')" class="w-full btn-gold py-4 text-xs">SUBMIT</button>
        </div>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-white z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-6"><h2 class="font-black">WITHDRAW</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-x"></i></div>
        <div class="space-y-3">
            <input type="number" id="out-amt" placeholder="Amount" class="w-full p-4 border rounded-xl outline-none">
            <input type="text" id="out-addr" placeholder="Account/Wallet Address" class="w-full p-4 border rounded-xl outline-none">
            <button onclick="submitFin('Withdraw')" class="w-full btn-gold py-4 text-xs">REQUEST PAYOUT</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);

        let currentUser = localStorage.getItem('nova_session');
        let adminClicks = 0;
        let config = { min_dep: 10, min_wd: 5, tax: 2, ep: "03000000000", jc: "03000000000", usdt: "0xAddress" };

        const nodesData = [
            { id: 1, name: "Core S1", cost: 10, yield: 0.9, color: "#94a3b8" },
            { id: 2, name: "Alpha V2", cost: 20, yield: 2.0, color: "#10b981" },
            { id: 3, name: "Rig X-50", cost: 50, yield: 5.5, color: "#d97706" },
            { id: 4, name: "Titan 100", cost: 100, yield: 12.0, color: "#3b82f6" },
            { id: 5, name: "Ultra 250", cost: 250, yield: 32.0, color: "#f59e0b" }
        ];
        for(let i=6; i<=21; i++) nodesData.push({ id: i, name: `Master Node V${i}`, cost: 250 + (i*100), yield: 30 + (i*15), color: "#000" });

        // Hidden Admin Trigger
        document.getElementById('logo-main').onclick = () => {
            adminClicks++;
            if(adminClicks >= 5) {
                const pin = prompt("Admin Pin:");
                if(pin === "nov786") {
                    document.getElementById('nav-admin-btn').style.display = "flex";
                    alert("Admin Unlocked!");
                }
                adminClicks = 0;
            }
        };

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return;
            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(s.exists()) {
                if(s.data().status === 'Banned') return alert("Account Banned!");
                if(s.data().password === pw) { localStorage.setItem('nova_session', id); location.reload(); }
                else alert("Wrong Password");
            } else {
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
                document.getElementById('b-main').innerText = "$" + (u.balance || 0).toFixed(2);
                document.getElementById('b-daily').innerText = "+$" + (u.daily || 0).toFixed(2);
                document.getElementById('b-total').innerText = "$" + (u.total_profit || 0).toFixed(2);
                renderNodes(u.active_nodes || []);
            });
            onSnapshot(doc(db, "config", "global"), d => { if(d.exists()) config = d.data(); });
            loadAdminRequests();
        }

        function renderNodes(active) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            nodesData.forEach(n => {
                const isRun = active.find(a => a.nodeId === n.id);
                grid.innerHTML += `<div class="premium-card p-4 border-l-4" style="border-left-color: ${n.color}">
                    <div class="flex justify-between items-center mb-3"><h4 class="font-black text-xs">${n.name}</h4><span class="text-green-500 font-bold text-[10px]">+$${n.yield}/D</span></div>
                    ${isRun ? `<span class="text-[8px] font-black text-blue-500">ACTIVE MINING</span>` : `<button onclick="buyNode(${n.id}, ${n.cost}, ${n.yield})" class="btn-gold px-4 py-2 text-[8px]">ACTIVATE $${n.cost}</button>`}
                </div>`;
            });
        }

        window.selDep = (t) => {
            document.getElementById('disp-method').innerText = t;
            document.getElementById('disp-addr').innerText = config[t.toLowerCase()] || "Contact Support";
        };

        window.submitFin = async (type) => {
            const amt = parseFloat(document.getElementById(type === 'Deposit' ? 'in-amt' : 'out-amt').value);
            if(type === 'Deposit' && amt < config.min_dep) return alert("Min Deposit: " + config.min_dep);
            if(type === 'Withdraw' && amt < config.min_wd) return alert("Min Withdraw: " + config.min_wd);
            
            const ref = document.getElementById(type === 'Deposit' ? 'in-txid' : 'out-addr').value;
            await addDoc(collection(db, "logs"), { user: currentUser, type, amount: amt, ref, status: 'Pending', time: Date.now() });
            alert("Request Sent!"); closeModal(type === 'Deposit' ? 'modal-dep' : 'modal-wd');
        };

        // Admin Functions
        async function loadAdminRequests() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const d = document.getElementById('adm-pending'); d.innerHTML = "";
                s.forEach(req => {
                    const r = req.data();
                    d.innerHTML += `<div class="premium-card p-3 text-[9px]">
                        <p><b>User:</b> ${r.user} | <b>${r.type}:</b> $${r.amount}</p>
                        <p class="mb-2">Ref: ${r.ref}</p>
                        <button onclick="approve('${req.id}', '${r.user}', ${r.amount}, '${r.type}')" class="bg-green-500 text-white px-3 py-1 rounded">APPROVE</button>
                    </div>`;
                });
            });
        }

        window.saveGlobalConfig = async () => {
            const data = { min_dep: parseFloat(document.getElementById('cfg-min-dep').value), min_wd: parseFloat(document.getElementById('cfg-min-wd').value), tax: parseFloat(document.getElementById('cfg-tax').value) };
            await updateDoc(doc(db, "config", "global"), data); alert("Config Updated");
        };

        window.saveWallets = async () => {
            const data = { ep: document.getElementById('cfg-ep').value, jc: document.getElementById('cfg-jc').value, usdt: document.getElementById('cfg-usdt').value };
            await updateDoc(doc(db, "config", "global"), data); alert("Wallets Saved");
        };

        window.userAction = async (act) => {
            const u = document.getElementById('adm-u-target').value; if(!u) return;
            const ref = doc(db, "users", u);
            if(act === 'ban') await updateDoc(ref, { status: 'Banned' });
            if(act === 'unban') await updateDoc(ref, { status: 'Active' });
            if(act === 'reset') await updateDoc(ref, { password: '123' });
            alert("Action Done");
        };

        window.approve = async (lid, uid, amt, type) => {
            const uRef = doc(db, "users", uid); const u = await getDoc(uRef);
            if(type === 'Deposit') await updateDoc(uRef, { balance: (u.data().balance || 0) + amt });
            if(type === 'Withdraw') {
                const finalAmt = amt - (amt * (config.tax / 100));
                await updateDoc(uRef, { balance: (u.data().balance || 0) - amt });
            }
            await updateDoc(doc(db, "logs", lid), { status: 'Approved' });
        };

        window.nav = (id, el) => { document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen')); document.getElementById(id).classList.add('active-screen'); document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active'); };
        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.logout = () => { localStorage.clear(); location.reload(); };
        if(currentUser) initApp(currentUser);
    </script>
</body>
</html>
