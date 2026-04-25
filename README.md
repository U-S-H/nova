<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | Unlimited</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root { --gold: #D4AF37; --bg: #ffffff; }
        body { background: var(--bg); font-family: sans-serif; overflow-x: hidden; }
        .screen { display: none; min-height: 100vh; width: 100%; position: absolute; top: 0; left: 0; background: white; }
        .active-screen { display: block !important; z-index: 50; }
        .premium-card { background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 20px; }
        .btn-gold { background: #D4AF37; color: white; border-radius: 12px; font-weight: 800; }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: white; border-top: 1px solid #eee; display: flex; justify-content: space-around; padding: 12px; z-index: 1000; }
    </style>
</head>
<body>

    <div id="auth-screen" class="screen active-screen flex flex-col justify-center items-center p-8 text-center">
        <h1 class="text-4xl font-black mb-1">NO<span class="text-[#D4AF37]">VA</span></h1>
        <p class="text-[10px] text-slate-400 uppercase tracking-widest mb-10">Global Asset Management</p>
        <div class="space-y-4 w-full max-w-xs">
            <input type="text" id="u-id" placeholder="Username" class="w-full p-4 bg-slate-100 rounded-xl outline-none">
            <input type="password" id="u-pw" placeholder="Password" class="w-full p-4 bg-slate-100 rounded-xl outline-none">
            <button onclick="handleAuth()" class="w-full btn-gold py-4 uppercase text-xs tracking-widest">Authorize Access</button>
            <p onclick="toggleAuth()" id="auth-btn-text" class="text-[10px] text-slate-400 font-bold underline cursor-pointer">CREATE NEW ACCOUNT</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center sticky top-0 bg-white/90 backdrop-blur-md z-[60] border-b">
            <h2 id="logo-main" class="font-black text-2xl select-none">NO<span class="text-[#D4AF37]">VA</span></h2>
            <p id="user-display" class="text-[10px] font-bold text-slate-500 uppercase">@USER</p>
        </header>

        <div id="page-dash" class="screen px-4 pt-6">
            <div class="bg-slate-900 rounded-[25px] p-8 mb-6 text-white shadow-2xl">
                <p class="text-[8px] text-slate-400 font-bold uppercase mb-1">Portfolio Balance</p>
                <h1 id="b-main" class="text-5xl font-black mb-8">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                    <div><p class="text-[7px] text-slate-400 uppercase">Daily Yield</p><p id="b-daily" class="text-lg font-bold text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[7px] text-slate-400 uppercase">Total Profit</p><p id="b-total" class="text-lg font-bold text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>
            <div class="grid grid-cols-2 gap-4">
                <button onclick="openModal('modal-dep')" class="premium-card p-6 flex flex-col items-center gap-2"><i class="fa-solid fa-plus text-green-600"></i><span class="text-[10px] font-black">DEPOSIT</span></button>
                <button onclick="openModal('modal-wd')" class="premium-card p-6 flex flex-col items-center gap-2"><i class="fa-solid fa-minus text-red-600"></i><span class="text-[10px] font-black">WITHDRAW</span></button>
            </div>
        </div>

        <div id="page-nodes" class="screen px-4 pt-6 pb-32"><div id="nodes-grid" class="space-y-4"></div></div>

        <div id="page-admin" class="screen px-4 pt-6 pb-32"><h2 class="text-red-600 font-black mb-4">ADMIN CONTROL</h2><div id="adm-list" class="space-y-4"></div></div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash')" class="flex flex-col items-center gap-1 text-[9px] font-bold text-slate-400"><i class="fa-solid fa-house"></i>HOME</div>
            <div onclick="nav('page-nodes')" class="flex flex-col items-center gap-1 text-[9px] font-bold text-slate-400"><i class="fa-solid fa-microchip"></i>NODES</div>
            <div id="nav-admin-btn" onclick="nav('page-admin')" class="hidden flex flex-col items-center gap-1 text-[9px] font-bold text-red-600"><i class="fa-solid fa-shield"></i>ADMIN</div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-10"><h2 class="font-black text-2xl">DEPOSIT</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-2xl"></i></div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="w-full p-4 bg-slate-100 rounded-xl mb-4 outline-none">
        <input type="text" id="dep-ref" placeholder="Transaction ID" class="w-full p-4 bg-slate-100 rounded-xl mb-8 outline-none">
        <button onclick="submitFin('Deposit')" class="w-full btn-gold py-4">SUBMIT</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo",
            authDomain: "home-94d45.firebaseapp.com",
            projectId: "home-94d45",
            appId: "1:964390949419:web:589840cb91b7e42ecd506e"
        };

        const app = initializeApp(firebaseConfig); const db = getFirestore(app);
        let user = localStorage.getItem('nova_session');
        let mode = 'login'; let tapCount = 0;

        // SCREEN CONTROLLER (BUGS FIXED)
        window.nav = (id) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
        };

        // AUTH LOGIC
        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("Fill data");
            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(mode === 'login') {
                if(s.exists() && s.data().password === pw) { localStorage.setItem('nova_session', id); location.reload(); }
                else alert("Access Denied");
            } else {
                if(s.exists()) return alert("User exists");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0, nodes: [] });
                localStorage.setItem('nova_session', id); location.reload();
            }
        };

        // ADMIN TRIGGER
        document.getElementById('logo-main').onclick = () => {
            tapCount++; if(tapCount === 4) {
                if(prompt("Key:") === "nov786") {
                    document.getElementById('nav-admin-btn').classList.remove('hidden');
                    loadAdmin(); alert("Admin Active");
                } tapCount = 0;
            }
        };

        function initApp(id) {
            document.getElementById('auth-screen').classList.remove('active-screen');
            document.getElementById('app').classList.remove('hidden');
            nav('page-dash');
            document.getElementById('user-display').innerText = `@${id}`;
            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u) return;
                document.getElementById('b-main').innerText = `$${u.balance.toFixed(2)}`;
                document.getElementById('b-daily').innerText = `+$${u.daily.toFixed(2)}`;
                document.getElementById('b-total').innerText = `$${u.total_profit.toFixed(2)}`;
                renderNodes(u.nodes || []);
            });
        }

        function renderNodes(active) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = "";
            for(let i=1; i<=21; i++) {
                const cost = 25+(i*25); const y = 1.5+(i*1.5); const isOwned = active.includes(i);
                grid.innerHTML += `<div class="premium-card p-5 flex justify-between items-center">
                    <div><h4 class="font-black text-xs">QUANTUM V${i}</h4><p class="text-[9px] text-slate-400">Profit: $${y}/day</p></div>
                    <button onclick="${isOwned?'':`buyNode(${i},${cost},${y})`}" class="px-4 py-2 text-[9px] font-bold rounded-lg ${isOwned?'text-green-600 bg-green-50':'btn-gold'}">
                        ${isOwned ? 'DEPLOYED' : `BUY $${cost}`}
                    </button>
                </div>`;
            }
        }

        window.buyNode = async (i, c, y) => {
            const uRef = doc(db, "users", user); const s = await getDoc(uRef);
            if(s.data().balance < c) return alert("Insufficient Balance");
            await updateDoc(uRef, { balance: s.data().balance - c, nodes: [...(s.data().nodes || []), i], daily: (s.data().daily || 0) + y });
            alert("Node Deployed!");
        };

        window.submitFin = async (type) => {
            const amt = parseFloat(document.getElementById('dep-amt').value);
            const ref = document.getElementById('dep-ref').value;
            await addDoc(collection(db, "logs"), { user, type, amount: amt, ref, status: 'Pending', time: Date.now() });
            alert("Request Sent!"); closeModal('modal-dep');
        };

        function loadAdmin() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const list = document.getElementById('adm-list'); list.innerHTML = "";
                s.forEach(docSnap => {
                    const l = docSnap.data(); const dId = docSnap.id;
                    list.innerHTML += `<div class="premium-card p-5">
                        <p class="text-[9px] font-bold uppercase">${l.type} - @${l.user}</p>
                        <h3 class="text-xl font-black mb-4">$${l.amount}</h3>
                        <div class="flex gap-2">
                            <button onclick="approve('${dId}','${l.user}',${l.amount})" class="flex-1 bg-green-600 text-white py-2 rounded-lg text-[10px] font-bold">APPROVE</button>
                            <button onclick="reject('${dId}')" class="flex-1 bg-slate-200 text-slate-500 py-2 rounded-lg text-[10px] font-bold">REJECT</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.approve = async (dId, uId, amt) => {
            const uRef = doc(db, "users", uId); const uS = await getDoc(uRef);
            await updateDoc(uRef, { balance: (uS.data().balance || 0) + amt });
            await updateDoc(doc(db, "logs", dId), { status: 'Completed' });
            alert("Done!");
        };

        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.toggleAuth = () => { mode = mode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = mode === 'login' ? "CREATE NEW ACCOUNT" : "BACK TO LOGIN"; };

        if(user) initApp(user);
    </script>
</body>
</html>
