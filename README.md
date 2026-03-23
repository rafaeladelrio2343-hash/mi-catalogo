# mi-catalogo
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>K&Y Angelic - Catálogo Digital</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&family=Playfair+Display:wght@700&display=swap');
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f9fafb;
        }

        .brand-font {
            font-family: 'Playfair Display', serif;
        }

        .product-card {
            transition: transform 0.2s, box-shadow 0.2s;
        }

        .product-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
        }

        .cart-badge {
            position: absolute;
            top: -8px;
            right: -8px;
            background: #ef4444;
            color: white;
            border-radius: 50%;
            padding: 2px 6px;
            font-size: 0.75rem;
            font-weight: bold;
        }

        /* Ocultar scrollbar pero permitir funcionalidad */
        .no-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .no-scrollbar {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }
    </style>
</head>
<body class="text-gray-800">

    <!-- Navbar -->
    <nav class="sticky top-0 z-50 bg-white border-b border-gray-100 px-4 py-3 shadow-sm">
        <div class="max-w-6xl mx-auto flex justify-between items-center">
            <h1 class="text-2xl font-bold brand-font text-indigo-900 cursor-pointer" onclick="location.reload()">K&Y <span class="text-amber-600">Angelic</span></h1>
            <div class="flex items-center gap-4">
                <button onclick="openAdmin()" class="text-gray-400 hover:text-indigo-600 transition-colors" title="Administrar productos">
                    <i class="fas fa-cog text-xl"></i>
                </button>
                <div class="relative cursor-pointer bg-gray-50 p-2 rounded-full hover:bg-gray-100 transition-colors" onclick="toggleCart()">
                    <i class="fas fa-shopping-bag text-xl text-gray-700"></i>
                    <span id="cart-count" class="cart-badge">0</span>
                </div>
            </div>
        </div>
    </nav>

    <!-- Header -->
    <header class="bg-indigo-900 text-white py-12 px-4 relative overflow-hidden">
        <div class="absolute top-0 right-0 w-64 h-64 bg-indigo-800 rounded-full -mr-32 -mt-32 opacity-50"></div>
        <div class="max-w-4xl mx-auto text-center relative z-10">
            <h2 class="text-4xl md:text-5xl font-bold brand-font mb-4">K&Y Angelic</h2>
            <p class="text-indigo-200 mb-8 text-lg">Tu estilo, tu esencia. Explora nuestra nueva colección.</p>
            
            <div class="relative max-w-xl mx-auto">
                <input type="text" id="search-input" placeholder="¿Buscas algo especial?" 
                       class="w-full pl-12 pr-4 py-4 rounded-xl text-gray-900 shadow-lg focus:outline-none focus:ring-2 focus:ring-amber-400">
                <i class="fas fa-search absolute left-4 top-1/2 -translate-y-1/2 text-gray-400"></i>
            </div>
        </div>
    </header>

    <!-- Categorías -->
    <section class="max-w-6xl mx-auto px-4 py-8">
        <div class="flex flex-wrap gap-3 justify-center mb-10" id="category-filters">
            <button onclick="filterCategory('all')" class="category-btn active bg-indigo-900 text-white px-6 py-2 rounded-full font-medium shadow-md transition-all">Todos</button>
            <button onclick="filterCategory('Dama')" class="category-btn bg-white border border-gray-200 px-6 py-2 rounded-full font-medium hover:bg-gray-50 transition-all">Dama</button>
            <button onclick="filterCategory('Caballero')" class="category-btn bg-white border border-gray-200 px-6 py-2 rounded-full font-medium hover:bg-gray-50 transition-all">Caballero</button>
            <button onclick="filterCategory('Balaclavas')" class="category-btn bg-white border border-gray-200 px-6 py-2 rounded-full font-medium hover:bg-gray-50 transition-all">Balaclavas</button>
        </div>

        <div id="product-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8">
            <!-- Los productos se cargan aquí -->
        </div>
    </section>

    <!-- Modal Admin (Editar Productos) -->
    <div id="admin-modal" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-[70] hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl w-full max-w-2xl max-h-[90vh] overflow-hidden flex flex-col shadow-2xl">
            <div class="p-6 border-b flex justify-between items-center bg-indigo-900 text-white">
                <h3 class="text-xl font-bold brand-font">Gestión de Catálogo</h3>
                <button onclick="closeAdmin()" class="hover:rotate-90 transition-transform">
                    <i class="fas fa-times text-xl"></i>
                </button>
            </div>
            <div class="p-6 overflow-y-auto flex-grow space-y-6 no-scrollbar">
                <!-- Formulario Nuevo Producto -->
                <div class="bg-gray-50 p-4 rounded-xl border border-dashed border-indigo-200">
                    <h4 class="font-bold mb-4 text-indigo-900">Añadir Nuevo Producto</h4>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <input type="text" id="new-name" placeholder="Nombre del producto" class="p-2 border rounded-lg focus:ring-2 focus:ring-indigo-400 outline-none">
                        <input type="number" id="new-price" placeholder="Precio ($)" class="p-2 border rounded-lg focus:ring-2 focus:ring-indigo-400 outline-none">
                        <select id="new-category" class="p-2 border rounded-lg focus:ring-2 focus:ring-indigo-400 outline-none">
                            <option value="Dama">Dama</option>
                            <option value="Caballero">Caballero</option>
                            <option value="Balaclavas">Balaclavas</option>
                        </select>
                        <input type="text" id="new-image" placeholder="URL de la imagen (ej. de ImgBB)" class="p-2 border rounded-lg focus:ring-2 focus:ring-indigo-400 outline-none">
                        <textarea id="new-desc" placeholder="Descripción corta" class="p-2 border rounded-lg md:col-span-2 focus:ring-2 focus:ring-indigo-400 outline-none"></textarea>
                    </div>
                    <button onclick="saveNewProduct()" class="mt-4 w-full bg-indigo-600 text-white py-3 rounded-lg font-bold hover:bg-indigo-700 transition-colors shadow-lg">
                        Agregar al Catálogo
                    </button>
                </div>

                <!-- Lista de productos actuales para borrar -->
                <div>
                    <h4 class="font-bold mb-4 text-gray-700">Productos en Inventario</h4>
                    <div id="admin-product-list" class="space-y-2">
                        <!-- Items aquí -->
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Carrito -->
    <div id="cart-sidebar" class="fixed inset-y-0 right-0 w-full md:w-96 bg-white shadow-2xl z-[60] transform translate-x-full transition-transform duration-300 ease-in-out">
        <div class="flex flex-col h-full">
            <div class="p-6 border-b flex justify-between items-center bg-white">
                <div>
                    <h3 class="text-xl font-bold text-gray-900 brand-font">Tu Carrito</h3>
                    <p class="text-xs text-gray-500">K&Y Angelic Collection</p>
                </div>
                <button onclick="toggleCart()" class="text-gray-400 hover:text-gray-600 p-2">
                    <i class="fas fa-times text-2xl"></i>
                </button>
            </div>
            <div id="cart-items" class="flex-grow overflow-y-auto p-6 space-y-4 no-scrollbar">
                <!-- Items del carrito -->
            </div>
            <div class="p-6 border-t bg-gray-50">
                <div class="flex justify-between items-center mb-6">
                    <span class="text-gray-600 font-medium">Total:</span>
                    <span id="cart-total" class="font-bold text-2xl text-indigo-900">$0.00</span>
                </div>
                <button onclick="sendWhatsApp()" class="w-full bg-green-500 hover:bg-green-600 text-white font-bold py-4 rounded-xl flex items-center justify-center gap-3 transition-all shadow-lg hover:shadow-green-200">
                    <i class="fab fa-whatsapp text-2xl"></i>
                    Confirmar por WhatsApp
                </button>
            </div>
        </div>
    </div>

    <div id="cart-backdrop" onclick="toggleCart()" class="fixed inset-0 bg-black/40 backdrop-blur-sm z-[55] hidden transition-opacity"></div>

    <script>
        // Configuración inicial
        const WHATSAPP_NUMBER = "584242670186";
        const ADMIN_PASSWORD = "admin123"; 
        
        // Productos iniciales si el almacenamiento local está vacío
        const initialProducts = [
            { id: 101, name: "Balaclava Tactical Pro", category: "Balaclavas", price: 15.00, description: "Diseño ergonómico y transpirable.", image: "https://images.unsplash.com/photo-1503944583220-79d8926ad5e2?w=500&q=80" },
            { id: 102, name: "Vestido Gala Angelic", category: "Dama", price: 45.00, description: "Elegancia exclusiva para momentos únicos.", image: "https://images.unsplash.com/photo-1595777457583-95e059d581b8?w=500&q=80" },
            { id: 103, name: "Chaqueta Urban Style", category: "Caballero", price: 55.00, description: "Resistente y con estilo contemporáneo.", image: "https://images.unsplash.com/photo-1520975954732-35dd22299614?w=500&q=80" }
        ];

        let products = JSON.parse(localStorage.getItem('angelic_products')) || initialProducts;
        let cart = [];

        // Renderizar Catálogo
        function renderProducts(items) {
            const grid = document.getElementById('product-grid');
            grid.innerHTML = '';
            
            if (items.length === 0) {
                grid.innerHTML = `<div class="col-span-full text-center py-20 text-gray-400">No se encontraron productos en esta categoría.</div>`;
                return;
            }

            items.forEach(product => {
                grid.innerHTML += `
                    <div class="product-card bg-white rounded-2xl overflow-hidden shadow-sm border border-gray-100 group">
                        <div class="relative overflow-hidden h-64">
                            <img src="${product.image}" alt="${product.name}" class="w-full h-full object-cover transition-transform duration-500 group-hover:scale-110" 
                                 onerror="this.src='https://via.placeholder.com/500x300?text=K%26Y+Angelic'">
                            <div class="absolute top-3 left-3">
                                <span class="bg-white/90 backdrop-blur-sm text-indigo-900 px-3 py-1 rounded-full text-xs font-semibold shadow-sm">${product.category}</span>
                            </div>
                        </div>
                        <div class="p-5">
                            <h3 class="font-bold text-lg mb-1 brand-font text-gray-800">${product.name}</h3>
                            <p class="text-gray-500 text-sm mb-4 line-clamp-2">${product.description}</p>
                            <div class="flex justify-between items-center">
                                <span class="text-xl font-bold text-amber-600">$${parseFloat(product.price).toFixed(2)}</span>
                                <button onclick="addToCart(${product.id})" class="bg-indigo-900 hover:bg-indigo-800 text-white w-10 h-10 rounded-xl flex items-center justify-center transition-all shadow-md active:scale-95">
                                    <i class="fas fa-cart-plus"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                `;
            });
        }

        // --- FUNCIONES DE ADMINISTRACIÓN ---
        function openAdmin() {
            const pass = prompt("Introduce la contraseña para editar el catálogo:");
            if (pass === ADMIN_PASSWORD) {
                document.getElementById('admin-modal').classList.remove('hidden');
                renderAdminList();
            } else if (pass !== null) {
                alert("Contraseña incorrecta.");
            }
        }

        function closeAdmin() {
            document.getElementById('admin-modal').classList.add('hidden');
        }

        function renderAdminList() {
            const list = document.getElementById('admin-product-list');
            list.innerHTML = '';
            products.forEach(p => {
                list.innerHTML += `
                    <div class="flex items-center justify-between p-3 bg-white border rounded-lg shadow-sm">
                        <div class="flex items-center gap-3">
                            <img src="${p.image}" class="w-12 h-12 object-cover rounded" onerror="this.src='https://via.placeholder.com/50?text=Error'">
                            <div>
                                <p class="text-sm font-bold text-gray-800">${p.name}</p>
                                <p class="text-xs text-gray-500">${p.category} • $${p.price}</p>
                            </div>
                        </div>
                        <button onclick="deleteProduct(${p.id})" class="text-red-400 hover:text-red-600 p-2 transition-colors">
                            <i class="fas fa-trash-alt"></i>
                        </button>
                    </div>
                `;
            });
        }

        function saveNewProduct() {
            const name = document.getElementById('new-name').value;
            const price = document.getElementById('new-price').value;
            const category = document.getElementById('new-category').value;
            const image = document.getElementById('new-image').value;
            const desc = document.getElementById('new-desc').value;

            if (!name || !price || !image) {
                alert("Completa al menos el nombre, precio y enlace de imagen.");
                return;
            }

            const newProd = {
                id: Date.now(),
                name,
                price: parseFloat(price),
                category,
                image,
                description: desc || "Nueva pieza exclusiva de K&Y Angelic."
            };

            products.unshift(newProd); // Agregar al inicio
            saveToStorage();
            renderAdminList();
            renderProducts(products);
            
            // Limpiar campos
            document.getElementById('new-name').value = '';
            document.getElementById('new-price').value = '';
            document.getElementById('new-image').value = '';
            document.getElementById('new-desc').value = '';
        }

        function deleteProduct(id) {
            if (confirm("¿Estás seguro de eliminar este producto del inventario?")) {
                products = products.filter(p => p.id !== id);
                saveToStorage();
                renderAdminList();
                renderProducts(products);
            }
        }

        function saveToStorage() {
            localStorage.setItem('angelic_products', JSON.stringify(products));
        }

        // --- FILTROS Y BÚSQUEDA ---
        function filterCategory(category) {
            const buttons = document.querySelectorAll('.category-btn');
            buttons.forEach(btn => {
                btn.classList.remove('bg-indigo-900', 'text-white', 'active', 'shadow-md');
                btn.classList.add('bg-white', 'text-gray-800', 'border-gray-200');
            });
            event.target.classList.add('bg-indigo-900', 'text-white', 'active', 'shadow-md');

            if (category === 'all') {
                renderProducts(products);
            } else {
                renderProducts(products.filter(p => p.category === category));
            }
        }

        document.getElementById('search-input').addEventListener('input', (e) => {
            const term = e.target.value.toLowerCase();
            const filtered = products.filter(p => 
                p.name.toLowerCase().includes(term) || p.description.toLowerCase().includes(term)
            );
            renderProducts(filtered);
        });

        // --- CARRITO ---
        function toggleCart() {
            document.getElementById('cart-sidebar').classList.toggle('translate-x-full');
            document.getElementById('cart-sidebar').classList.toggle('translate-x-0');
            document.getElementById('cart-backdrop').classList.toggle('hidden');
        }

        function addToCart(productId) {
            const product = products.find(p => p.id === productId);
            const existing = cart.find(item => item.id === productId);
            if (existing) {
                existing.quantity++;
            } else {
                cart.push({ ...product, quantity: 1 });
            }
            updateCartUI();
        }

        function removeFromCart(productId) {
            cart = cart.filter(item => item.id !== productId);
            updateCartUI();
        }

        function updateCartUI() {
            const container = document.getElementById('cart-items');
            const totalDisplay = document.getElementById('cart-total');
            const countDisplay = document.getElementById('cart-count');
            
            container.innerHTML = '';
            let total = 0;
            let count = 0;

            if (cart.length === 0) {
                container.innerHTML = `<div class="text-center py-10 text-gray-400">Carrito vacío</div>`;
            } else {
                cart.forEach(item => {
                    total += item.price * item.quantity;
                    count += item.quantity;
                    container.innerHTML += `
                        <div class="flex items-center gap-4 bg-gray-50 p-3 rounded-xl border border-gray-100">
                            <img src="${item.image}" class="w-16 h-16 object-cover rounded-lg" onerror="this.src='https://via.placeholder.com/60'">
                            <div class="flex-grow">
                                <h4 class="font-bold text-sm text-gray-800">${item.name}</h4>
                                <p class="text-xs text-indigo-600 font-semibold">${item.quantity} x $${item.price.toFixed(2)}</p>
                            </div>
                            <button onclick="removeFromCart(${item.id})" class="text-gray-300 hover:text-red-500">
                                <i class="fas fa-trash text-lg"></i>
                            </button>
                        </div>
                    `;
                });
            }
            countDisplay.innerText = count;
            totalDisplay.innerText = `$${total.toFixed(2)}`;
        }

        function sendWhatsApp() {
            if (cart.length === 0) return alert("Tu carrito está vacío.");
            
            let msg = "✨ *K&Y Angelic - Nuevo Pedido* ✨\n\n";
            cart.forEach(i => {
                msg += `🔹 *${i.name}*\n   Cant: ${i.quantity} • Subtotal: $${(i.price*i.quantity).toFixed(2)}\n\n`;
            });
            msg += `━━━━━━━━━━━━━━━\n`;
            msg += `💰 *TOTAL A PAGAR: ${document.getElementById('cart-total').innerText}*\n\n`;
            msg += `¿Me podrían indicar los métodos de pago disponibles? Gracias.`;
            
            window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(msg)}`, '_blank');
        }

        // Carga inicial
        window.onload = () => renderProducts(products);
    </script>
</body>
</html>
