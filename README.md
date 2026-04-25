<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | PRO VERSION</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root { --gold: #D4AF37; --dark: #0f172a; }
        body { background: #ffffff; font-family: 'Inter', sans-serif; overflow-x: hidden; }
        /* Screens management */
        .screen { display: none; min-height: 100vh; width: 100%; background: white; }
        .active-screen { display: block !important; }
        .glass-card { background: rgba(255, 255, 255, 0.8); backdrop-filter: blur(10px); border: 1px solid #f1f5f9; border-radius: 24px; }
        .btn-main { background: var(--gold); color: white; border-radius: 14px; font-weight: 700; transition: 0.3s; }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: white; border-top: 1px solid #f1f5f9; display: flex; justify-content: space-around; padding: 15px; z-index: 1000; }
        input { border: 1.5px solid #f1f5f9; border-radius: 14px; padding: 12px; outline: none; transition: 0.2s; }
        input:focus { border-color: var(--gold); }
    </style>
</head>
<body>

    <div id="auth-screen" class="screen active-screen flex flex-col justify-center items-center p-8">
        <div class="mb-12 text-center">
            <h1 class="text-5xl font-black tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[10px] text-slate-400 font-bold uppercase tracking-[0.3em]">Institutional Grade</p>
        </div>
        <div class="w-full max-w-sm space-y-4">
            <input type="text" id="u-id" placeholder="Username" class="w-full bg-slate-50">
            <input type="password" id="u-pw" placeholder="Password" class="w-full bg-slate-50">
            <button onclick="handleAuth()" class="w-full btn-main py-4 uppercase text-xs tracking-widest shadow-lg shadow-gold/20">Authorize</button>
            <p onclick="toggleAuth()" id="auth-toggle-btn" class="text-center text-[11px] font-bold text-slate-400 cursor-pointer underline">NEW ACCOUNT? REGISTER</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-6 flex justify-between items-center sticky top-0 bg-white/80 backdrop-blur-md z-[100]">
            <h2 id="admin-trigger" class="font-black text-2xl select-none">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="flex items-center gap-2">
                <span id="display-name" class="text-[10px] font-bold text-slate-500 bg-slate-100 px-3 py-1 rounded-full">@USER</span>
            </div>
        </header>

        <div id="scr-dash" class="screen px-5 pt-4 pb-32">
            <div class="bg-slate-950 rounded-[32px] p-8 mb-6 text-white relative overflow-hidden shadow-2xl">
                <div class="relative z-10">
                    <p class="text-[9px] text-slate-400 font-bold uppercase tracking-widest mb-2">Portfolio Balance</p>
                    <h1 id="user-bal" class="text-5xl font-black mb-10">$0.00</h1>
                    <div class="flex justify-between border-t border-white/10 pt-6">
                        <div><p class="text-[8px] text-slate-500 uppercase font-bold">24h Earnings</p><p id="user-daily" class="text-xl font-bold text-green-400">+$0.00</p></div>
                        <div class="text-right"><p class="text-[8px] text-slate-500 uppercase font-bold">Total Profit</p><p id="user-total" class="text-xl font-bold text-[#D4AF37]">$0.00</p></div>
                    </div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-8">
                <button onclick="openModal('dep-modal')" class="glass-card p-6 flex flex-col items-center gap-3"><div class="w-10 h-10 rounded-full bg-green-50 flex items-center justify-center text-green-600"><i class="fa-solid fa-arrow-down"></i></div><span class="text-[11px] font-black">DEPOSIT</span></button>
                <button onclick="openModal('wd-modal')" class="glass-card p-6 flex flex-col items-center gap-3"><div class="w-10 h-10 rounded-full bg-red-50 flex items-center justify-center text-red-600"><i class="fa-solid fa-arrow-up"></i></div><span class="text-[11px] font-black">WITHDRAW</span></button>
            </div>
        </div>

        <div id="scr-nodes" class="screen px-5 pt-4 pb-32">
            <h3 class="font-black text-xl mb-6 px-2 text-slate-800 underline decoration-gold decoration-4">Mining Nodes</h3>
            <div id="nodes-container" class="space-y-4"></div>
        </div>

        <div id="scr-admin" class="screen px-5 pt-4 pb-32">
            <div class="flex justify-between items-center mb-6">
                <h3 class="font-black text-xl text-red-600 uppercase italic">Admin Authority</h3>
                <span class="bg-red-100 text-red-600 text-[9px] px-2 py-1 rounded font-bold uppercase">Live Logs</span>
            </div>
            <div id="admin-logs" class="space-y-4"></div>
        </div>

        <nav class="nav-bar">
            <button onclick="changeScreen('scr-dash')" class="flex flex-col items-center gap-1 text-[10px] font-bold text-slate-400"><i class="fa-solid fa-grid-2"></i>HOME</button>
            <button onclick="changeScreen('scr-nodes')" class="flex flex-col items-center gap-1 text-[10px] font-bold text-slate-400"><i class="fa-solid fa-microchip"></i>NODES</button>
            <button id="admin-nav" onclick="changeScreen('scr-admin')" class="hidden flex flex-col items-center gap-1 text-[10px] font-bold text-red-500"><i class="fa-solid fa-user-shield"></i>ADMIN</button>
        </nav>
    </div>

    <div id="dep-modal" class="fixed inset-0 bg-white z-[2000] p-8 hidden animate-slide-up">
        <div class="flex justify-between items-center mb-10"><h2 class="font-black text-3xl">DEPOSIT</h2><i onclick="closeModal('dep-modal')" class="fa-solid fa-circle-xmark text-2xl text-slate-300"></i></div>
        <div class="space-y-6">
            <div class="bg-slate-50 p-6 rounded-2xl border border-dashed border-slate-300 text-center">
                <p class="text-[10px] font-bold text-slate-400 mb-2">USDT (TRC20) ADDRESS</p>
                <p class="text-xs font-black break-all">TJqWvX9xxxxxxxxxxxxxxxxxxxxxx</p>
            </div>
            <input type="number" id="d-amt" placeholder="Amount in USD" class="w-full">
            <input type="text" id="d-id" placeholder="Transaction Hash / ID" class="w-full">
            <button onclick="submitReq('Deposit')" class="w-full btn-main py-5 shadow-xl shadow-gold/30">CONFIRM PAYMENT</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo",
            authDomain: "home-94d45.firebaseapp.com",
            projectId: "home-94d45",
            appId: "1:964390949419:web:589840cb91b7e42ecd506e"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        
        let currentUser = localStorage.getItem('nova_user');
        let authMode = 'login';
        let adminTaps = 0;

        // --- CORE NAVIGATION (NO OVERLAP BUG) ---
        window.changeScreen = (id) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
        };

        // --- AUTH SYSTEM ---
        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("Fill all fields");

            const ref = doc(db, "users", id);
            const snap = await getDoc(ref);

            if(authMode === 'login') {
                if(snap.exists() && snap.data().password === pw) {
                    localStorage.setItem('nova_user', id);
                    location.reload();
                } else alert("Invalid Credentials");
            } else {
                if(snap.exists()) return alert("Username taken");
                await setDoc(ref, { password: pw, balance: 0, daily: 0, total_profit: 0, nodes: [] });
                localStorage.setItem('nova_user', id);
                location.reload();
            }
        };

        // --- DASHBOARD & NODES ---
        function loadDashboard(id) {
            document.getElementById('auth-screen').classList.remove('active-screen');
            document.getElementById('app').classList.remove('hidden');
            changeScreen('scr-dash');
            document.getElementById('display-name').innerText = `@${id}`;

            onSnapshot(doc(db, "users", id), d => {
                const u = d.data();
                document.getElementById('user-bal').innerText = `$${u.balance.toFixed(2)}`;
                document.getElementById('user-daily').innerText = `+$${u.daily.toFixed(2)}`;
                document.getElementById('user-total').innerText = `$${u.total_profit.toFixed(2)}`;
                renderNodes(u.nodes || []);
            });
        }

        function renderNodes(owned) {
            const container = document.getElementById('nodes-container');
            container.innerHTML = "";
            for(let i=1; i<=21; i++) {
                const price = 50 * i;
                const daily = 2 * i;
                const hasNode = owned.includes(i);
                container.innerHTML += `
                    <div class="glass-card p-5 flex justify-between items-center">
                        <div>
                            <h4 class="font-black text-xs uppercase tracking-tighter">Nova Node V${i}</h4>
                            <p class="text-[9px] text-slate-400 font-bold">Yield: $${daily}/day</p>
                        </div>
                        <button onclick="${hasNode ? '' : `buyNode(${i}, ${price}, ${daily})`}" 
                                class="px-5 py-2 text-[10px] font-black rounded-xl transition ${hasNode ? 'bg-green-50 text-green-600' : 'btn-main shadow-lg shadow-gold/20'}">
                            ${hasNode ? 'ACTIVE' : `BUY $${price}`}
                        </button>
                    </div>`;
            }
        }

        window.buyNode = async (num, price, yieldAmt) => {
            const ref = doc(db, "users", currentUser);
            const snap = await getDoc(ref);
            if(snap.data().balance < price) return alert("Low Balance!");
            
            await updateDoc(ref, {
                balance: snap.data().balance - price,
                nodes: [...(snap.data().nodes || []), num],
                daily: (snap.data().daily || 0) + yieldAmt
            });
            alert("Node Successfully Deployed!");
        };

        // --- ADMIN TRIGGER ---
        document.getElementById('admin-trigger').onclick = () => {
            adminTaps++;
            if(adminTaps === 4) {
                if(prompt("Security Key:") === "nova786") {
                    document.getElementById('admin-nav').classList.remove('hidden');
                    loadAdminPanel();
                    alert("Admin Authority Granted");
                }
                adminTaps = 0;
            }
        };

        function loadAdminPanel() {
            onSnapshot(query(collection(db, "requests"), where("status", "==", "Pending")), snap => {
                const list = document.getElementById('admin-logs');
                list.innerHTML = "";
                snap.forEach(req => {
                    const data = req.data();
                    list.innerHTML += `
                        <div class="glass-card p-5 border-l-4 border-l-gold">
                            <div class="flex justify-between mb-3">
                                <span class="text-[9px] font-black text-slate-400 uppercase">@${data.user} - ${data.type}</span>
                                <span class="text-[10px] font-bold text-gold">$${data.amount}</span>
                            </div>
                            <p class="text-[9px] mb-4 text-slate-500 font-mono">Ref: ${data.ref}</p>
                            <div class="flex gap-2">
                                <button onclick="processReq('${req.id}', '${data.user}', ${data.amount}, 'Approve')" class="flex-1 bg-slate-900 text-white py-3 rounded-xl text-[10px] font-bold">APPROVE</button>
                                <button onclick="processReq('${req.id}', '${data.user}', ${data.amount}, 'Reject')" class="flex-1 bg-slate-100 text-slate-400 py-3 rounded-xl text-[10px] font-bold">REJECT</button>
                            </div>
                        </div>`;
                });
            });
        }

        window.processReq = async (reqId, uId, amt, action) => {
            if(action === 'Approve') {
                const uRef = doc(db, "users", uId);
                const uSnap = await getDoc(uRef);
                await updateDoc(uRef, { balance: (uSnap.data().balance || 0) + amt });
            }
            await updateDoc(doc(db, "requests", reqId), { status: 'Handled' });
            alert(`Request ${action}ed`);
        };

        window.submitReq = async (type) => {
            const amt = parseFloat(document.getElementById('d-amt').value);
            const ref = document.getElementById('d-id').value;
            if(!amt || !ref) return alert("Fill data");
            await addDoc(collection(db, "requests"), { user: currentUser, type, amount: amt, ref, status: 'Pending' });
            alert("Sent! Wait for approval.");
            closeModal('dep-modal');
        };

        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.toggleAuth = () => {
            authMode = authMode === 'login' ? 'reg' : 'login';
            document.getElementById('auth-toggle-btn').innerText = authMode === 'login' ? 'NEW ACCOUNT? REGISTER' : 'ALREADY HAVE ACCOUNT? LOGIN';
        };

        if(currentUser) loadDashboard(currentUser);
    </script>
</body>
</html>
