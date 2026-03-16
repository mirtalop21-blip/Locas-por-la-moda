<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Locas por la Moda | Indumentaria Femenina</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;700&display=swap');
        body { font-family: 'Montserrat', sans-serif; }
        .cart-badge { position: absolute; top: -5px; right: -10px; background: #ff4757; color: white; border-radius: 50%; padding: 2px 6px; font-size: 10px; }
    </style>
</head>
<body class="bg-gray-50">

    <nav class="bg-white shadow-sm sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 h-20 flex justify-between items-center">
            <h1 class="text-2xl font-bold tracking-tighter text-gray-800 italic">LOCAS <span class="text-pink-500">X LA MODA</span></h1>
            <div class="relative cursor-pointer" onclick="toggleCart()">
                <i class="fas fa-shopping-bag text-2xl text-gray-700"></i>
                <span id="cart-count" class="cart-badge">0</span>
            </div>
        </div>
    </nav>

    <header class="bg-pink-50 py-16 text-center">
        <h2 class="text-3xl font-light text-gray-800">Nueva Colección <span class="font-bold">2026</span></h2>
        <p class="text-gray-500 mt-2 uppercase tracking-widest text-sm">Indumentaria Femenina de Diseño</p>
    </header>

    <main class="max-w-7xl mx-auto px-4 py-12">
        <div id="product-grid" class="grid grid-cols-2 md:grid-cols-4 gap-6">
            </div>
    </main>

    <div id="cart-modal" class="fixed inset-0 bg-black/50 z-[100] hidden flex justify-end">
        <div class="bg-white w-full max-w-md h-full p-8 shadow-2xl flex flex-col">
            <div class="flex justify-between items-center mb-8">
                <h3 class="text-xl font-bold">Tu Pedido</h3>
                <button onclick="toggleCart()" class="text-2xl">&times;</button>
            </div>
            <div id="cart-items" class="flex-1 overflow-y-auto space-y-4">
                </div>
            <div class="border-t pt-6 mt-6">
                <div class="flex justify-between text-xl font-bold mb-6">
                    <span>Total:</span>
                    <span id="cart-total">$0</span>
                </div>
                <button onclick="sendWhatsApp()" class="w-full bg-green-500 text-white py-4 rounded-xl font-bold flex items-center justify-center gap-2 hover:bg-green-600 transition-all">
                    <i class="fab fa-whatsapp text-xl"></i> ENVIAR PEDIDO POR WHATSAPP
                </button>
            </div>
        </div>
    </div>

    <script>
        // CONFIGURACIÓN DE TU MARCA
        const WHATSAPP_NUMBER = "5491166860054"; // Tu número de la tarjeta
        
        const products = [
            { id: 1, name: "Tapado Camel Soft", price: 45000, img: "https://images.unsplash.com/photo-1539533018447-63fcce2678e3?q=80&w=400" },
            { id: 2, name: "Jean Mom Celeste", price: 28000, img: "https://images.unsplash.com/photo-1541099649105-f69ad21f3246?q=80&w=400" },
            { id: 3, name: "Blusa Romántica", price: 18500, img: "https://images.unsplash.com/photo-1551163943-3f6a855d1153?q=80&w=400" },
            { id: 4, name: "Saco de Hilo", price: 22000, img: "https://images.unsplash.com/photo-1620799140408-edc6dcb6d633?q=80&w=400" }
        ];

        let cart = [];

        function renderProducts() {
            const grid = document.getElementById('product-grid');
            grid.innerHTML = products.map(p => `
                <div class="bg-white rounded-2xl overflow-hidden shadow-sm hover:shadow-md transition-shadow">
                    <img src="${p.img}" class="w-full h-64 object-cover">
                    <div class="p-4 text-center">
                        <h4 class="font-bold text-gray-800 text-sm mb-1">${p.name}</h4>
                        <p class="text-pink-500 font-bold mb-4">$${p.price.toLocaleString()}</p>
                        <button onclick="addToCart(${p.id})" class="w-full border border-pink-500 text-pink-500 py-2 rounded-lg text-xs font-bold hover:bg-pink-500 hover:text-white transition-all">
                            AGREGAR
                        </button>
                    </div>
                </div>
            `).join('');
        }

        function addToCart(id) {
            const product = products.find(p => p.id === id);
            cart.push(product);
            updateCart();
        }

        function updateCart() {
            document.getElementById('cart-count').innerText = cart.length;
            const itemsDiv = document.getElementById('cart-items');
            itemsDiv.innerHTML = cart.map((item, index) => `
                <div class="flex justify-between items-center border-b pb-2">
                    <div>
                        <p class="font-bold text-sm">${item.name}</p>
                        <p class="text-xs text-gray-500">$${item.price.toLocaleString()}</p>
                    </div>
                    <button onclick="removeFromCart(${index})" class="text-red-400 text-xs">Eliminar</button>
                </div>
            `).join('');
            
            const total = cart.reduce((sum, item) => sum + item.price, 0);
            document.getElementById('cart-total').innerText = `$${total.toLocaleString()}`;
        }

        function removeFromCart(index) {
            cart.splice(index, 1);
            updateCart();
        }

        function toggleCart() {
            document.getElementById('cart-modal').classList.toggle('hidden');
        }

        function sendWhatsApp() {
            if (cart.length === 0) return alert("Tu carrito está vacío");
            
            let message = "¡Hola Locas por la Moda! ✨ Quiero realizar el siguiente pedido:%0A%0A";
            cart.forEach(item => {
                message += `- ${item.name} ($${item.price.toLocaleString()})%0A`;
            });
            const total = cart.reduce((sum, item) => sum + item.price, 0);
            message += `%0A*TOTAL: $${total.toLocaleString()}*%0A%0A¿Cómo podemos coordinar el pago y el envío?`;
            
            window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=${message}`, '_blank');
        }

        renderProducts();
    </script>
</body>
</html> Locas-por-la-moda
