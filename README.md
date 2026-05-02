<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Blakis Élec - Matériel Professionnel</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        .page { display: none; }
        .active { display: block; }
        .elec-blue { background-color: #1e3a8a; }
        .elec-yellow { background-color: #fbbf24; }
    </style>
</head>
<body class="bg-gray-100 pb-24">

    <div id="auth" class="page active fixed inset-0 bg-white z-[100] p-8 flex flex-col justify-center">
        <div class="text-center mb-8">
            <div class="w-20 h-20 elec-blue rounded-full mx-auto flex items-center justify-center text-white text-4xl mb-4">
                <i class="fa-solid fa-plug"></i>
            </div>
            <h1 class="text-3xl font-black text-blue-900">BLAKIS ÉLEC</h1>
            <p class="text-gray-500">Votre matériel électrique à Bondoukou</p>
        </div>
        <input type="text" id="nom" placeholder="Votre Nom" class="w-full p-4 border-2 rounded-xl mb-4 outline-none focus:border-blue-600">
        <button onclick="entrer()" class="w-full elec-blue text-white font-bold p-4 rounded-xl shadow-lg">ENTRER DANS LA BOUTIQUE</button>
    </div>

    <header id="head" class="hidden elec-blue p-4 sticky top-0 z-50 flex justify-between items-center shadow-lg">
        <h1 class="text-white font-bold tracking-tighter">BLAKIS <span class="text-yellow-400">ÉLEC</span></h1>
        <div class="relative" onclick="nav('cart-page')">
            <i class="fa-solid fa-cart-shopping text-white text-xl"></i>
            <span id="cart-count" class="absolute -top-2 -right-2 bg-yellow-400 text-blue-900 text-[10px] font-bold rounded-full h-5 w-5 flex items-center justify-center">0</span>
        </div>
    </header>

    <main id="home" class="page p-3">
        <div class="grid grid-cols-2 gap-3" id="product-list">
            </div>
    </main>

    <main id="cart-page" class="page p-4">
        <h2 class="text-xl font-bold mb-4">Mon Panier</h2>
        <div id="cart-items" class="space-y-3 mb-6">
            </div>
        <div class="bg-white p-4 rounded-xl shadow-md">
            <div class="flex justify-between font-bold text-lg mb-4">
                <span>Total :</span>
                <span id="total-price">0 F</span>
            </div>
            <button onclick="commander()" class="w-full bg-green-600 text-white font-bold p-4 rounded-xl">COMMANDER SUR WHATSAPP</button>
        </div>
    </main>

    <nav id="navbar" class="hidden fixed bottom-0 w-full bg-white border-t flex justify-around p-3 z-50">
        <button onclick="nav('home')" class="flex flex-col items-center text-blue-900">
            <i class="fa-solid fa-house"></i><span class="text-[10px] font-bold">Accueil</span>
        </button>
        <button onclick="nav('cart-page')" class="flex flex-col items-center text-gray-400">
            <i class="fa-solid fa-basket-shopping"></i><span class="text-[10px] font-bold">Panier</span>
        </button>
    </nav>

    <script>
        const products = [
            { id: 1, name: "Câble 1.5mm (100m)", price: 18500, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR6A7j7W6H" },
            { id: 2, name: "Câble 2.5mm (100m)", price: 28000, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTe2-p-Y7U" },
            { id: 3, name: "Disjoncteur 16A", price: 3500, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQYv7fG" },
            { id: 4, name: "Prise Legrand", price: 2500, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR9X5z" },
            { id: 5, name: "Ampoule LED 12W", price: 1500, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT6-6e" },
            { id: 6, name: "Interrupteur simple", price: 1200, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRqZ" },
            { id: 7, name: "Coffret 8 Modules", price: 8500, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT5e" },
            { id: 8, name: "Douille B22", price: 500, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTz" },
            { id: 9, name: "Pince Coupante Pro", price: 6500, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTy" },
            { id: 10, name: "Multimètre Digital", price: 12000, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTu" },
            // Ajoute d'autres ici jusqu'à 20
        ];

        let cart = [];

        function renderProducts() {
            const list = document.getElementById('product-list');
            list.innerHTML = products.map(p => `
                <div class="bg-white p-2 rounded-xl shadow-sm border border-gray-100">
                    <img src="${p.img}" class="w-full h-24 object-cover rounded-lg">
                    <h3 class="text-[11px] font-bold mt-2 h-8 leading-tight">${p.name}</h3>
                    <p class="text-blue-700 font-black text-sm">${p.price} F</p>
                    <button onclick="addToCart(${p.id})" class="w-full elec-blue text-white text-[10px] py-2 mt-2 rounded-lg uppercase font-bold">Ajouter</button>
                </div>
            `).join('');
        }

        function addToCart(id) {
            const p = products.find(x => x.id === id);
            cart.push(p);
            updateCart();
        }

        function updateCart() {
            document.getElementById('cart-count').innerText = cart.length;
            const items = document.getElementById('cart-items');
            items.innerHTML = cart.map((item, index) => `
                <div class="flex justify-between bg-white p-3 rounded-lg shadow-sm">
                    <span class="text-sm font-bold">${item.name}</span>
                    <span class="text-blue-600 font-bold">${item.price} F</span>
                </div>
            `).join('');
            const total = cart.reduce((sum, item) => sum + item.price, 0);
            document.getElementById('total-price').innerText = total + " F";
        }

        function commander() {
            const total = document.getElementById('total-price').innerText;
            const message = `Bonjour Blakis Élec, je souhaite commander : ${cart.length} articles pour un total de ${total}.`;
            window.location.href = `https://wa.me/2250000000000?text=${encodeURIComponent(message)}`;
        }

        function entrer() {
            if(document.getElementById('nom').value.length < 2) return;
            document.getElementById('auth').classList.remove('active');
            document.getElementById('head').classList.remove('hidden');
            document.getElementById('navbar').classList.remove('hidden');
            document.getElementById('home').classList.add('active');
            renderProducts();
        }

        function nav(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
        }
    </script>
</body>
</html>
