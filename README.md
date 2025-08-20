# Rezox
Big ShopStyle 
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ShopStyle - Boutique en ligne</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .product-card:hover { transform: translateY(-4px); }
        .cart-badge { animation: bounce 0.5s ease-in-out; }
        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
            40% { transform: translateY(-10px); }
            60% { transform: translateY(-5px); }
        }
    </style>
</head>
<body class="bg-gray-50">
    <!-- Header -->
    <header class="bg-white shadow-sm sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between items-center h-16">
                <div class="flex items-center">
                    <h1 class="text-2xl font-bold text-indigo-600">🛍️ ShopStyle</h1>
                </div>
                <nav class="hidden md:flex space-x-8">
                    <a href="#" class="text-gray-700 hover:text-indigo-600 font-medium">Accueil</a>
                    <a href="#products" class="text-gray-700 hover:text-indigo-600 font-medium">Produits</a>
                    <a href="#" class="text-gray-700 hover:text-indigo-600 font-medium">À propos</a>
                    <a href="#" class="text-gray-700 hover:text-indigo-600 font-medium">Contact</a>
                </nav>
                <div class="flex items-center space-x-4">
                    <button onclick="toggleCart()" class="relative p-2 text-gray-700 hover:text-indigo-600">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4m0 0L7 13m0 0l-1.5 6M7 13l-1.5 6m0 0h9m-9 0V19a2 2 0 002 2h7a2 2 0 002-2v-.5"></path>
                        </svg>
                        <span id="cart-count" class="absolute -top-1 -right-1 bg-red-500 text-white text-xs rounded-full h-5 w-5 flex items-center justify-center cart-badge hidden">0</span>
                    </button>
                </div>
            </div>
        </div>
    </header>

    <!-- Hero Section -->
    <section class="bg-gradient-to-r from-indigo-600 to-purple-600 text-white py-20">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
            <h2 class="text-4xl md:text-6xl font-bold mb-6">Découvrez notre collection</h2>
            <p class="text-xl mb-8 opacity-90">Des produits de qualité à des prix exceptionnels</p>
            <button onclick="document.getElementById('products').scrollIntoView({behavior: 'smooth'})" class="bg-white text-indigo-600 px-8 py-3 rounded-full font-semibold hover:bg-gray-100 transition duration-300">
                Voir les produits
            </button>
        </div>
    </section>

    <!-- Products Section -->
    <section id="products" class="py-16">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <h3 class="text-3xl font-bold text-center mb-12 text-gray-900">Nos produits populaires</h3>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8" id="products-grid">
                <!-- Products will be loaded here -->
            </div>
        </div>
    </section>

    <!-- Cart Sidebar -->
    <div id="cart-sidebar" class="fixed right-0 top-0 h-full w-96 bg-white shadow-2xl transform translate-x-full transition-transform duration-300 z-50">
        <div class="p-6 border-b">
            <div class="flex justify-between items-center">
                <h3 class="text-xl font-semibold">Panier</h3>
                <button onclick="toggleCart()" class="text-gray-500 hover:text-gray-700">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                    </svg>
                </button>
            </div>
        </div>
        <div class="flex-1 overflow-y-auto p-6">
            <div id="cart-items">
                <p class="text-gray-500 text-center py-8">Votre panier est vide</p>
            </div>
        </div>
        <div class="border-t p-6">
            <div class="flex justify-between items-center mb-4">
                <span class="text-lg font-semibold">Total:</span>
                <span id="cart-total" class="text-xl font-bold text-indigo-600">0,00 €</span>
            </div>
            <button id="checkout-btn" class="w-full bg-indigo-600 text-white py-3 rounded-lg font-semibold hover:bg-indigo-700 transition duration-300 disabled:opacity-50 disabled:cursor-not-allowed" disabled onclick="showCheckoutDemo()">
                Procéder au paiement
            </button>
        </div>
    </div>

    <!-- Cart Overlay -->
    <div id="cart-overlay" class="fixed inset-0 bg-black bg-opacity-50 z-40 hidden" onclick="toggleCart()"></div>

    <!-- Demo Modal -->
    <div id="demo-modal" class="fixed inset-0 bg-black bg-opacity-50 z-60 hidden flex items-center justify-center">
        <div class="bg-white rounded-lg p-8 max-w-md mx-4">
            <h3 class="text-xl font-semibold mb-4">🛍️ Démo du site</h3>
            <p class="text-gray-600 mb-6">Ceci est une démonstration. Dans un vrai site e-commerce, cette page redirigerait vers un système de paiement sécurisé.</p>
            <button onclick="closeDemoModal()" class="w-full bg-indigo-600 text-white py-2 rounded-lg hover:bg-indigo-700 transition duration-300">
                Compris
            </button>
        </div>
    </div>

    <script>
        // Sample products data
        const products = [
            { id: 1, name: "Casque Audio Premium", price: 89.99, image: "🎧", description: "Son haute qualité" },
            { id: 2, name: "Montre Connectée", price: 199.99, image: "⌚", description: "Suivi fitness avancé" },
            { id: 3, name: "Sac à Dos Design", price: 49.99, image: "🎒", description: "Élégant et pratique" },
            { id: 4, name: "Lunettes de Soleil", price: 79.99, image: "🕶️", description: "Protection UV totale" },
            { id: 5, name: "Chaussures Sport", price: 129.99, image: "👟", description: "Confort optimal" },
            { id: 6, name: "Parfum Luxe", price: 159.99, image: "🌸", description: "Fragrance exclusive" }
        ];

        let cart = [];

        // Load products
        function loadProducts() {
            const grid = document.getElementById('products-grid');
            grid.innerHTML = products.map(product => `
                <div class="product-card bg-white rounded-xl shadow-md overflow-hidden transition-all duration-300 hover:shadow-xl">
                    <div class="p-6 text-center">
                        <div class="text-6xl mb-4">${product.image}</div>
                        <h4 class="text-xl font-semibold mb-2">${product.name}</h4>
                        <p class="text-gray-600 mb-4">${product.description}</p>
                        <div class="flex justify-between items-center">
                            <span class="text-2xl font-bold text-indigo-600">${product.price.toFixed(2)} €</span>
                            <button onclick="addToCart(${product.id})" class="bg-indigo-600 text-white px-6 py-2 rounded-lg hover:bg-indigo-700 transition duration-300">
                                Ajouter
                            </button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        // Add to cart
        function addToCart(productId) {
            const product = products.find(p => p.id === productId);
            const existingItem = cart.find(item => item.id === productId);
            
            if (existingItem) {
                existingItem.quantity += 1;
            } else {
                cart.push({ ...product, quantity: 1 });
            }
            
            updateCartUI();
        }

        // Remove from cart
        function removeFromCart(productId) {
            cart = cart.filter(item => item.id !== productId);
            updateCartUI();
        }

        // Update quantity
        function updateQuantity(productId, change) {
            const item = cart.find(item => item.id === productId);
            if (item) {
                item.quantity += change;
                if (item.quantity <= 0) {
                    removeFromCart(productId);
                } else {
                    updateCartUI();
                }
            }
        }

        // Update cart UI
        function updateCartUI() {
            const cartCount = document.getElementById('cart-count');
            const cartItems = document.getElementById('cart-items');
            const cartTotal = document.getElementById('cart-total');
            const checkoutBtn = document.getElementById('checkout-btn');
            
            const totalItems = cart.reduce((sum, item) => sum + item.quantity, 0);
            const totalPrice = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            
            // Update cart count
            if (totalItems > 0) {
                cartCount.textContent = totalItems;
                cartCount.classList.remove('hidden');
                cartCount.classList.add('cart-badge');
            } else {
                cartCount.classList.add('hidden');
            }
            
            // Update cart items
            if (cart.length === 0) {
                cartItems.innerHTML = '<p class="text-gray-500 text-center py-8">Votre panier est vide</p>';
                checkoutBtn.disabled = true;
            } else {
                cartItems.innerHTML = cart.map(item => `
                    <div class="flex items-center justify-between py-4 border-b">
                        <div class="flex items-center space-x-3">
                            <span class="text-2xl">${item.image}</span>
                            <div>
                                <h4 class="font-medium">${item.name}</h4>
                                <p class="text-sm text-gray-500">${item.price.toFixed(2)} €</p>
                            </div>
                        </div>
                        <div class="flex items-center space-x-2">
                            <button onclick="updateQuantity(${item.id}, -1)" class="w-8 h-8 rounded-full bg-gray-200 flex items-center justify-center hover:bg-gray-300">-</button>
                            <span class="w-8 text-center">${item.quantity}</span>
                            <button onclick="updateQuantity(${item.id}, 1)" class="w-8 h-8 rounded-full bg-gray-200 flex items-center justify-center hover:bg-gray-300">+</button>
                            <button onclick="removeFromCart(${item.id})" class="ml-2 text-red-500 hover:text-red-700">
                                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"></path>
                                </svg>
                            </button>
                        </div>
                    </div>
                `).join('');
                checkoutBtn.disabled = false;
            }
            
            // Update total
            cartTotal.textContent = `${totalPrice.toFixed(2)} €`;
        }

        // Toggle cart
        function toggleCart() {
            const sidebar = document.getElementById('cart-sidebar');
            const overlay = document.getElementById('cart-overlay');
            
            if (sidebar.classList.contains('translate-x-full')) {
                sidebar.classList.remove('translate-x-full');
                overlay.classList.remove('hidden');
            } else {
                sidebar.classList.add('translate-x-full');
                overlay.classList.add('hidden');
            }
        }

        // Show checkout demo
        function showCheckoutDemo() {
            document.getElementById('demo-modal').classList.remove('hidden');
        }

        // Close demo modal
        function closeDemoModal() {
            document.getElementById('demo-modal').classList.add('hidden');
        }

        // Initialize
        document.addEventListener('DOMContentLoaded', function() {
            loadProducts();
        });
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'97229ba8231b807e',t:'MTc1NTcwMDQzMC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
