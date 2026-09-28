<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mente & Saber | Libros Digitales y Test Psicológicos</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#eef2ff',
                            100: '#e0e7ff',
                            500: '#6366f1',
                            600: '#4f46e5',
                            700: '#4338ca',
                            800: '#3730a3',
                            900: '#312e81',
                        },
                        yape: '#712885',
                        plin: '#00d2ff'
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        body { font-family: 'Inter', sans-serif; }
        .custom-scrollbar::-webkit-scrollbar {
            width: 6px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: #f1f1f1;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 4px;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }
        @keyframes pulse-subtle {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.85; }
        }
        .animate-pulse-subtle {
            animation: pulse-subtle 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 antialiased min-h-screen flex flex-col selection:bg-brand-500 selection:text-white">

    <!-- Header / Navbar -->
    <header class="sticky top-0 z-30 bg-white/90 backdrop-blur-md border-b border-slate-200 shadow-sm transition-all">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <!-- Logo -->
            <div class="flex items-center space-x-3 cursor-pointer" onclick="app.resetFilter()">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-brand-600 to-indigo-500 flex items-center justify-center text-white shadow-md shadow-brand-500/20">
                    <i class="fa-solid fa-brain text-xl"></i>
                </div>
                <div>
                    <span class="text-xl font-extrabold bg-gradient-to-r from-brand-700 to-indigo-600 bg-clip-text text-transparent">Mente & Saber</span>
                    <span class="text-xs font-semibold block text-slate-400 tracking-wider uppercase -mt-1">Recursos Digitales</span>
                </div>
            </div>

            <!-- Global Search & Actions -->
            <div class="flex items-center space-x-3">
                <button onclick="app.toggleCartModal()" class="relative p-2.5 rounded-xl text-slate-600 hover:text-brand-600 hover:bg-slate-100 transition duration-150" aria-label="Carrito de compras">
                    <i class="fa-solid fa-shopping-bag text-xl"></i>
                    <span id="cartCountBadge" class="absolute -top-1 -right-1 bg-brand-600 text-white text-xs font-bold rounded-full h-5 w-5 flex items-center justify-center shadow-md scale-0 transition-transform duration-200">0</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Hero Section -->
    <section class="bg-gradient-to-b from-brand-900 via-brand-800 to-slate-900 text-white py-12 px-4 sm:px-6 lg:px-8 relative overflow-hidden">
        <div class="absolute inset-0 opacity-10 bg-[radial-gradient(#fff_1px,transparent_1px)] [background-size:16px_16px]"></div>
        <div class="max-w-4xl mx-auto text-center relative z-10">
            <span class="inline-block px-3 py-1 rounded-full text-xs font-semibold bg-brand-500/30 text-brand-200 border border-brand-400/30 mb-3">
                <i class="fa-solid fa-bolt mr-1 text-yellow-300"></i> Descarga Automática e Inmediata
            </span>
            <h1 class="text-3xl sm:text-4xl md:text-5xl font-black tracking-tight mb-4 leading-tight">
                Librería & Evaluaciones Psicológicas Digitales
            </h1>
            <p class="text-slate-300 text-base sm:text-lg mb-8 max-w-2xl mx-auto">
                Adquiere test validados, manuales técnicos y libros especializados en formato PDF. Pagos 100% seguros con Yape, Plin y Tarjetas.
            </p>

            <!-- Search Bar -->
            <div class="max-w-xl mx-auto relative">
                <div class="relative flex items-center">
                    <i class="fa-solid fa-magnifying-glass absolute left-4 text-slate-400 text-lg"></i>
                    <input type="text" id="searchInput" onkeyup="app.handleSearch(event)" placeholder="Buscar por título, autor o clave (ej. Ansiedad, WISC, Clinica)..." class="w-full pl-11 pr-24 py-3.5 rounded-2xl bg-white text-slate-800 placeholder-slate-400 text-sm focus:outline-none focus:ring-4 focus:ring-brand-500/40 shadow-xl font-medium transition">
                    <button onclick="app.executeSearch()" class="absolute right-2 px-4 py-2 bg-brand-600 hover:bg-brand-700 text-white text-xs font-bold rounded-xl transition shadow">
                        Buscar
                    </button>
                </div>
            </div>
        </div>
    </section>

    <!-- Main Content Area -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 flex-grow w-full">

        <!-- Filters Bar -->
        <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 mb-8 pb-4 border-b border-slate-200">
            <!-- Categories -->
            <div class="flex items-center gap-2 overflow-x-auto w-full sm:w-auto pb-2 sm:pb-0 custom-scrollbar" id="categoryContainer">
                <!-- Dynamically populated -->
            </div>

            <!-- Sort option -->
            <div class="flex items-center space-x-2 text-xs font-medium text-slate-500 self-end sm:self-auto shrink-0">
                <span>Ordenar:</span>
                <select id="sortSelect" onchange="app.handleSort(this.value)" class="bg-white border border-slate-200 rounded-lg px-2.5 py-1.5 text-xs text-slate-700 font-semibold focus:outline-none focus:border-brand-500 shadow-sm cursor-pointer">
                    <option value="featured">Destacados</option>
                    <option value="price-low">Precio: Menor a Mayor</option>
                    <option value="price-high">Precio: Mayor a Menor</option>
                    <option value="title">Nombre A-Z</option>
                </select>
            </div>
        </div>

        <!-- Products Grid Container -->
        <div id="productsGrid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
            <!-- Products injected by JS -->
        </div>

        <!-- Empty state -->
        <div id="emptyState" class="hidden text-center py-16 bg-white rounded-3xl border border-dashed border-slate-200 my-8">
            <i class="fa-solid fa-folder-open text-5xl text-slate-300 mb-4"></i>
            <h3 class="text-lg font-bold text-slate-700 mb-1">No se encontraron productos</h3>
            <p class="text-sm text-slate-500 max-w-md mx-auto mb-4">Intenta cambiar los términos de búsqueda o filtro de categorías.</p>
            <button onclick="app.resetFilter()" class="px-4 py-2 bg-brand-100 text-brand-700 font-bold text-xs rounded-xl hover:bg-brand-200 transition">
                Restablecer Filtros
            </button>
        </div>
    </main>

    <!-- Footer -->
    <footer class="bg-slate-900 text-slate-400 border-t border-slate-800 text-sm mt-12 py-8">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex flex-col md:flex-row justify-between items-center gap-4 text-center md:text-left">
            <div>
                <span class="font-bold text-slate-200 text-base">Mente & Saber Digital</span>
                <p class="text-xs text-slate-500 mt-1">Plataforma de distribución de recursos psicométricos y bibliográficos.</p>
            </div>
            <div class="flex items-center space-x-6 text-xs">
                <span class="flex items-center space-x-1 text-slate-300"><i class="fa-solid fa-shield-halved text-emerald-400"></i> <span>Garantía de Entrega</span></span>
                <span class="flex items-center space-x-1 text-slate-300"><i class="fa-solid fa-bolt text-amber-400"></i> <span>Descarga Inmediata</span></span>
                <span class="flex items-center space-x-1 text-slate-300"><i class="fa-solid fa-lock text-brand-400"></i> <span>Pago Seguro</span></span>
            </div>
            <p class="text-xs text-slate-500">&copy; 2026 Mente & Saber. Todos los derechos reservados.</p>
        </div>
    </footer>

    <!-- PRODUCT DETAIL MODAL -->
    <div id="detailModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center p-4 opacity-0 pointer-events-none transition-opacity duration-300">
        <div class="bg-white rounded-3xl max-w-2xl w-full max-h-[90vh] overflow-hidden shadow-2xl flex flex-col transform scale-95 transition-transform duration-300" id="detailModalContent">
            <!-- Modal Header -->
            <div class="relative bg-gradient-to-r from-brand-900 to-indigo-900 p-6 text-white flex justify-between items-start">
                <button onclick="app.closeDetailModal()" class="absolute top-4 right-4 bg-white/10 hover:bg-white/20 text-white rounded-full w-8 h-8 flex items-center justify-center transition">
                    <i class="fa-solid fa-xmark"></i>
                </button>
                <div class="pr-8">
                    <span id="modalCategory" class="inline-block px-2.5 py-0.5 rounded-full text-xs font-semibold bg-white/20 text-brand-100 uppercase tracking-wider mb-2">Categoría</span>
                    <h2 id="modalTitle" class="text-xl sm:text-2xl font-black leading-tight">Título del Recurso</h2>
                    <p id="modalAuthor" class="text-xs text-brand-200 mt-1"><i class="fa-solid fa-user-pen mr-1"></i> Autor / Editorial</p>
                </div>
            </div>

            <!-- Modal Body -->
            <div class="p-6 overflow-y-auto custom-scrollbar flex-grow space-y-6">
                <!-- Badges / Tech details -->
                <div class="grid grid-cols-3 gap-3 bg-slate-50 p-3 rounded-2xl border border-slate-100 text-center">
                    <div>
                        <span class="text-[10px] text-slate-400 uppercase font-bold block">Formato</span>
                        <span id="modalFormat" class="text-xs font-bold text-slate-700"><i class="fa-regular fa-file-pdf text-red-500 mr-1"></i> PDF</span>
                    </div>
                    <div>
                        <span class="text-[10px] text-slate-400 uppercase font-bold block">Páginas / Contenido</span>
                        <span id="modalPages" class="text-xs font-bold text-slate-700">120 Pág.</span>
                    </div>
                    <div>
                        <span class="text-[10px] text-slate-400 uppercase font-bold block">Tamaño</span>
                        <span id="modalSize" class="text-xs font-bold text-slate-700">14.5 MB</span>
                    </div>
                </div>

                <!-- Description -->
                <div>
                    <h4 class="text-xs font-bold text-slate-400 uppercase tracking-wider mb-2">Descripción General</h4>
                    <p id="modalDescription" class="text-sm text-slate-600 leading-relaxed">Descripción completa del producto digital...</p>
                </div>

                <!-- Syllabus / Contents Preview -->
                <div>
                    <h4 class="text-xs font-bold text-slate-400 uppercase tracking-wider mb-2">Contenido e Incluye</h4>
                    <ul id="modalSyllabus" class="space-y-1.5 text-xs text-slate-600">
                        <!-- Injected by JS -->
                    </ul>
                </div>
            </div>

            <!-- Modal Footer -->
            <div class="p-4 bg-slate-50 border-t border-slate-100 flex items-center justify-between">
                <div>
                    <span class="text-xs text-slate-400 font-medium block">Precio Final</span>
                    <span id="modalPrice" class="text-2xl font-black text-slate-900">S/ 0.00</span>
                </div>
                <button id="modalAddBtn" onclick="" class="px-6 py-3 bg-brand-600 hover:bg-brand-700 text-white font-bold text-sm rounded-xl shadow-lg shadow-brand-500/20 transition flex items-center space-x-2">
                    <i class="fa-solid fa-cart-plus"></i>
                    <span>Agregar al Carrito</span>
                </button>
            </div>
        </div>
    </div>

    <!-- CART MODAL / DRAWER -->
    <div id="cartModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 opacity-0 pointer-events-none transition-opacity duration-300">
        <div class="absolute right-0 top-0 bottom-0 w-full max-w-md bg-white shadow-2xl flex flex-col transform translate-x-full transition-transform duration-300 ease-out" id="cartContent">
            <!-- Cart Header -->
            <div class="p-5 border-b border-slate-100 flex items-center justify-between bg-slate-50">
                <div class="flex items-center space-x-2">
                    <i class="fa-solid fa-shopping-bag text-brand-600 text-lg"></i>
                    <h3 class="font-bold text-slate-800 text-base">Carrito de Compras</h3>
                </div>
                <button onclick="app.toggleCartModal()" class="text-slate-400 hover:text-slate-600 p-1 rounded-lg">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>

            <!-- Cart Items List -->
            <div id="cartItemsList" class="flex-grow p-4 overflow-y-auto custom-scrollbar space-y-3">
                <!-- Cart items JS injected -->
            </div>

            <!-- Empty Cart View -->
            <div id="cartEmptyView" class="hidden flex-grow flex flex-col items-center justify-center p-6 text-center">
                <div class="w-16 h-16 rounded-full bg-slate-100 flex items-center justify-center text-slate-400 mb-3">
                    <i class="fa-solid fa-cart-arrow-down text-2xl"></i>
                </div>
                <p class="font-bold text-slate-700 text-sm">Tu carrito está vacío</p>
                <p class="text-xs text-slate-400 mt-1 max-w-xs">Explora el catálogo y añade los test o libros que necesitas.</p>
            </div>

            <!-- Cart Footer -->
            <div class="p-5 border-t border-slate-100 bg-slate-50 space-y-4">
                <div class="space-y-1.5 text-xs text-slate-600">
                    <div class="flex justify-between">
                        <span>Subtotal</span>
                        <span id="cartSubtotal" class="font-semibold">S/ 0.00</span>
                    </div>
                    <div class="flex justify-between text-emerald-600 font-medium">
                        <span>Envío Digital (Inmediato)</span>
                        <span>GRATIS</span>
                    </div>
                    <div class="flex justify-between text-base font-black text-slate-900 pt-2 border-t border-slate-200">
                        <span>Total a pagar</span>
                        <span id="cartTotal" class="text-brand-600">S/ 0.00</span>
                    </div>
                </div>

                <button id="cartCheckoutBtn" onclick="app.goToCheckout()" class="w-full py-3.5 bg-brand-600 hover:bg-brand-700 text-white font-bold text-sm rounded-xl shadow-lg shadow-brand-500/20 transition flex items-center justify-center space-x-2">
                    <span>Procesar Pago</span>
                    <i class="fa-solid fa-arrow-right"></i>
                </button>
            </div>
        </div>
    </div>

    <!-- CHECKOUT MODAL -->
    <div id="checkoutModal" class="fixed inset-0 bg-slate-900/70 backdrop-blur-sm z-50 flex items-center justify-center p-3 sm:p-4 opacity-0 pointer-events-none transition-opacity duration-300">
        <div class="bg-white rounded-3xl max-w-3xl w-full max-h-[92vh] overflow-hidden shadow-2xl flex flex-col transform scale-95 transition-transform duration-300" id="checkoutModalContent">
            <!-- Checkout Header -->
            <div class="bg-slate-900 px-6 py-4 text-white flex items-center justify-between border-b border-slate-800">
                <div class="flex items-center space-x-2">
                    <i class="fa-solid fa-lock text-emerald-400"></i>
                    <h3 class="font-bold text-base">Finalizar Compra y Pago Seguro</h3>
                </div>
                <button onclick="app.closeCheckoutModal()" class="text-slate-400 hover:text-white p-1">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>

            <!-- Checkout Grid -->
            <div class="p-6 overflow-y-auto custom-scrollbar flex-grow grid grid-cols-1 md:grid-cols-12 gap-6">
                <!-- Left Column: User Details & Payment Method Selection -->
                <div class="md:col-span-7 space-y-6">
                    <!-- Step 1: User Details -->
                    <div>
                        <h4 class="text-xs font-bold text-brand-600 uppercase tracking-wider mb-3 flex items-center">
                            <span class="w-5 h-5 rounded-full bg-brand-100 text-brand-700 inline-flex items-center justify-center text-[10px] mr-2">1</span>
                            Datos del Comprador
                        </h4>
                        <div class="space-y-3">
                            <div>
                                <label class="block text-xs font-semibold text-slate-600 mb-1">Nombre Completo *</label>
                                <input type="text" id="custName" placeholder="Ej. Ana María Torres" class="w-full px-3.5 py-2.5 text-xs rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-brand-500 font-medium">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-slate-600 mb-1">Correo Electrónico (Donde llegará el recurso) *</label>
                                <input type="email" id="custEmail" placeholder="ejemplo@correo.com" class="w-full px-3.5 py-2.5 text-xs rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-brand-500 font-medium">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-slate-600 mb-1">Teléfono / WhatsApp *</label>
                                <input type="tel" id="custPhone" placeholder="987 654 321" class="w-full px-3.5 py-2.5 text-xs rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-brand-500 font-medium">
                            </div>
                        </div>
                    </div>

                    <!-- Step 2: Payment Method Choice -->
                    <div>
                        <h4 class="text-xs font-bold text-brand-600 uppercase tracking-wider mb-3 flex items-center">
                            <span class="w-5 h-5 rounded-full bg-brand-100 text-brand-700 inline-flex items-center justify-center text-[10px] mr-2">2</span>
                            Método de Pago
                        </h4>
                        
                        <div class="grid grid-cols-2 gap-3 mb-4">
                            <label class="border-2 border-slate-200 rounded-2xl p-3 flex flex-col items-center justify-center cursor-pointer hover:border-brand-500 transition has-[:checked]:border-brand-600 has-[:checked]:bg-brand-50/50">
                                <input type="radio" name="payMethod" value="yape" checked onchange="app.switchPayMethod('yape')" class="sr-only">
                                <div class="w-8 h-8 rounded-lg bg-yape text-white font-black text-xs flex items-center justify-center mb-1">Y/P</div>
                                <span class="text-xs font-bold text-slate-800">Yape / Plin</span>
                                <span class="text-[10px] text-slate-400">Transferencia Móvil</span>
                            </label>

                            <label class="border-2 border-slate-200 rounded-2xl p-3 flex flex-col items-center justify-center cursor-pointer hover:border-brand-500 transition has-[:checked]:border-brand-600 has-[:checked]:bg-brand-50/50">
                                <input type="radio" name="payMethod" value="card" onchange="app.switchPayMethod('card')" class="sr-only">
                                <div class="text-slate-700 text-lg mb-1"><i class="fa-solid fa-credit-card"></i></div>
                                <span class="text-xs font-bold text-slate-800">Tarjeta Crédito/Débito</span>
                                <span class="text-[10px] text-slate-400">Visa / Mastercard</span>
                            </label>
                        </div>

                        <!-- Dynamic Payment Details Panel -->
                        <!-- Option A: Yape / Plin -->
                        <div id="payPanelYape" class="bg-slate-50 border border-slate-200 rounded-2xl p-4 space-y-4">
                            <div class="flex items-center justify-between pb-3 border-b border-slate-200">
                                <div>
                                    <span class="text-xs font-bold text-slate-700 block">Escanear QR o Usar Número</span>
                                    <span class="text-[11px] text-slate-500">Mente & Saber Digital E.I.R.L.</span>
                                </div>
                                <span class="px-2 py-1 bg-purple-100 text-yape font-black text-xs rounded-md">Yape / Plin</span>
                            </div>

                            <div class="flex flex-col sm:flex-row items-center gap-4">
                                <!-- Generated Dynamic QR -->
                                <div class="bg-white p-2.5 rounded-xl border border-slate-200 shadow-sm text-center shrink-0">
                                    <div class="w-28 h-28 bg-slate-900 rounded-lg flex flex-col items-center justify-center text-white relative overflow-hidden">
                                        <!-- Simulated QR Pattern -->
                                        <div class="absolute inset-2 grid grid-cols-5 gap-1 opacity-80">
                                            <div class="bg-white"></div><div class="bg-white"></div><div class="bg-slate-900"></div><div class="bg-white"></div><div class="bg-white"></div>
                                            <div class="bg-white"></div><div class="bg-slate-900"></div><div class="bg-white"></div><div class="bg-slate-900"></div><div class="bg-white"></div>
                                            <div class="bg-slate-900"></div><div class="bg-white"></div><div class="bg-white"></div><div class="bg-white"></div><div class="bg-slate-900"></div>
                                            <div class="bg-white"></div><div class="bg-slate-900"></div><div class="bg-white"></div><div class="bg-slate-900"></div><div class="bg-white"></div>
                                            <div class="bg-white"></div><div class="bg-white"></div><div class="bg-slate-900"></div><div class="bg-white"></div><div class="bg-white"></div>
                                        </div>
                                        <div class="z-10 bg-yape px-1.5 py-0.5 rounded text-[9px] font-bold">YAPE ME</div>
                                    </div>
                                    <span class="text-[10px] text-slate-400 mt-1 block">QR Oficial</span>
                                </div>

                                <div class="space-y-2 text-xs flex-grow w-full">
                                    <div class="bg-white p-2.5 rounded-xl border border-slate-200 flex items-center justify-between">
                                        <div>
                                            <span class="text-[10px] text-slate-400 block font-bold">NÚMERO DE CELULAR</span>
                                            <span class="font-extrabold text-slate-800 text-sm">987 123 456</span>
                                        </div>
                                        <button onclick="app.copyToClipboard('987123456')" class="px-2.5 py-1 bg-slate-100 hover:bg-slate-200 text-slate-700 rounded-lg text-[11px] font-bold transition">
                                            <i class="fa-regular fa-copy mr-1"></i> Copiar
                                        </button>
                                    </div>
                                    <p class="text-[11px] text-slate-500 leading-tight">
                                        Realiza el pago exacto de <strong id="checkoutYapeAmount" class="text-brand-700">S/ 0.00</strong> e ingresa el número de operación abajo para validar.
                                    </p>
                                </div>
                            </div>

                            <!-- Operation Number input -->
                            <div class="pt-2">
                                <label class="block text-xs font-bold text-slate-700 mb-1">Nº de Operación / Comprobante (Yape/Plin) *</label>
                                <input type="text" id="opNumber" placeholder="Ej. 8493021" class="w-full px-3.5 py-2.5 text-xs rounded-xl border border-slate-300 focus:outline-none focus:ring-2 focus:ring-brand-500 bg-white font-mono font-bold">
                            </div>
                        </div>

                        <!-- Option B: Credit Card -->
                        <div id="payPanelCard" class="hidden bg-slate-50 border border-slate-200 rounded-2xl p-4 space-y-3">
                            <div class="flex justify-between items-center pb-2 border-b border-slate-200">
                                <span class="text-xs font-bold text-slate-700">Tarjeta de Crédito o Débito</span>
                                <div class="flex space-x-1 text-slate-400 text-base">
                                    <i class="fa-brands fa-cc-visa"></i>
                                    <i class="fa-brands fa-cc-mastercard"></i>
                                </div>
                            </div>

                            <div>
                                <label class="block text-[11px] font-semibold text-slate-600 mb-1">Número de Tarjeta</label>
                                <input type="text" placeholder="4557 0000 0000 0000" maxlength="19" class="w-full px-3 py-2 text-xs rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-brand-500 bg-white font-mono">
                            </div>

                            <div class="grid grid-cols-2 gap-3">
                                <div>
                                    <label class="block text-[11px] font-semibold text-slate-600 mb-1">Vencimiento (MM/AA)</label>
                                    <input type="text" placeholder="12/28" maxlength="5" class="w-full px-3 py-2 text-xs rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-brand-500 bg-white text-center font-mono">
                                </div>
                                <div>
                                    <label class="block text-[11px] font-semibold text-slate-600 mb-1">CVV</label>
                                    <input type="password" placeholder="123" maxlength="4" class="w-full px-3 py-2 text-xs rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-brand-500 bg-white text-center font-mono">
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Right Column: Order Summary -->
                <div class="md:col-span-5 bg-slate-50 p-4 rounded-2xl border border-slate-200 flex flex-col justify-between h-full">
                    <div>
                        <h4 class="text-xs font-bold text-slate-700 uppercase tracking-wider mb-3">Resumen de la Orden</h4>
                        <div id="checkoutSummaryList" class="space-y-2 mb-4 max-h-48 overflow-y-auto custom-scrollbar pr-1">
                            <!-- Items inserted by JS -->
                        </div>

                        <div class="border-t border-slate-200 pt-3 space-y-1.5 text-xs">
                            <div class="flex justify-between text-slate-600">
                                <span>Subtotal</span>
                                <span id="checkoutSubtotal">S/ 0.00</span>
                            </div>
                            <div class="flex justify-between text-emerald-600 font-medium">
                                <span>Descuento Digital</span>
                                <span>S/ 0.00</span>
                            </div>
                            <div class="flex justify-between text-base font-black text-slate-900 pt-2 border-t border-slate-200">
                                <span>Total Final</span>
                                <span id="checkoutTotal" class="text-brand-600">S/ 0.00</span>
                            </div>
                        </div>
                    </div>

                    <!-- Action Button -->
                    <div class="pt-6">
                        <button onclick="app.processPaymentVerification()" class="w-full py-3.5 bg-emerald-600 hover:bg-emerald-700 text-white font-extrabold text-sm rounded-xl shadow-lg shadow-emerald-600/20 transition flex items-center justify-center space-x-2">
                            <i class="fa-solid fa-shield-check"></i>
                            <span>Verificar y Descargar Recurso</span>
                        </button>
                        <p class="text-[10px] text-slate-400 text-center mt-2">
                            <i class="fa-solid fa-lock mr-1"></i> Transacción cifrada. Recibirás tu enlace de descarga en pantalla e email inmediatamente.
                        </p>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- VERIFICATION LOADING MODAL -->
    <div id="verifyModal" class="fixed inset-0 bg-slate-900/80 backdrop-blur-md z-50 flex items-center justify-center p-4 opacity-0 pointer-events-none transition-opacity duration-300">
        <div class="bg-white rounded-3xl max-w-sm w-full p-8 text-center shadow-2xl">
            <div class="relative w-20 h-20 mx-auto mb-6 flex items-center justify-center">
                <div class="absolute inset-0 border-4 border-brand-200 border-t-brand-600 rounded-full animate-spin"></div>
                <i class="fa-solid fa-receipt text-2xl text-brand-600 animate-pulse"></i>
            </div>
            <h3 class="text-lg font-extrabold text-slate-800 mb-2">Comprobando Pago...</h3>
            <p class="text-xs text-slate-500 mb-4">Verificando número de operación con el servidor bancario y generando enlaces de descarga segura.</p>
            <div class="w-full bg-slate-100 rounded-full h-2 overflow-hidden">
                <div id="verifyProgressBar" class="bg-brand-600 h-full w-0 transition-all duration-300"></div>
            </div>
        </div>
    </div>

    <!-- SUCCESS & IMMEDIATE DOWNLOAD MODAL -->
    <div id="successModal" class="fixed inset-0 bg-slate-900/80 backdrop-blur-md z-50 flex items-center justify-center p-4 opacity-0 pointer-events-none transition-opacity duration-300">
        <div class="bg-white rounded-3xl max-w-xl w-full max-h-[90vh] overflow-y-auto custom-scrollbar shadow-2xl p-6 sm:p-8 text-center relative">
            
            <!-- Success Badge -->
            <div class="w-16 h-16 bg-emerald-100 text-emerald-600 rounded-full flex items-center justify-center text-3xl mx-auto mb-4 shadow-lg shadow-emerald-500/20">
                <i class="fa-solid fa-check animate-bounce"></i>
            </div>

            <h2 class="text-2xl font-black text-slate-900 mb-1">¡Pago Confirmado Con Éxito!</h2>
            <p class="text-xs text-slate-500 mb-6">Tu comprobante ha sido verificado. Puedes descargar tus materiales de inmediato.</p>

            <!-- Notification banner -->
            <div class="bg-emerald-50 border border-emerald-200 rounded-2xl p-3.5 mb-6 text-left flex items-start space-x-3">
                <i class="fa-solid fa-envelope-circle-check text-emerald-600 text-xl shrink-0 mt-0.5"></i>
                <div>
                    <span class="text-xs font-bold text-emerald-900 block">Copia enviada al correo:</span>
                    <span id="successEmailDisplay" class="text-xs font-semibold text-emerald-700">correo@ejemplo.com</span>
                    <p class="text-[11px] text-emerald-600/90 mt-0.5">También hemos enviado los enlaces directos y el recibo PDF a tu casilla de entrada.</p>
                </div>
            </div>

            <!-- Download List Section -->
            <div class="text-left mb-6">
                <h4 class="text-xs font-bold text-slate-400 uppercase tracking-wider mb-3">Tus Archivos Listos para Descargar</h4>
                <div id="downloadsList" class="space-y-3">
                    <!-- Download items JS injected -->
                </div>
            </div>

            <!-- Receipt Info Box -->
            <div class="bg-slate-50 rounded-2xl p-4 border border-slate-200 text-left text-xs space-y-2 mb-6">
                <div class="flex justify-between">
                    <span class="text-slate-400">Nº de Orden:</span>
                    <span id="receiptOrderNum" class="font-mono font-bold text-slate-700">MS-98402</span>
                </div>
                <div class="flex justify-between">
                    <span class="text-slate-400">Fecha / Hora:</span>
                    <span id="receiptDate" class="font-medium text-slate-700">28/09/2026</span>
                </div>
                <div class="flex justify-between border-t border-slate-200 pt-2 font-bold text-slate-800">
                    <span>Monto Total Pagado:</span>
                    <span id="receiptTotalAmount" class="text-brand-600">S/ 0.00</span>
                </div>
            </div>

            <!-- Actions -->
            <button onclick="app.closeSuccessModal()" class="w-full py-3 bg-slate-900 hover:bg-slate-800 text-white font-bold text-xs rounded-xl transition">
                Volver al Catálogo
            </button>
        </div>
    </div>

    <script>
        /**
         * Digital Products Database (Psychological Tests & Books)
         */
        const PRODUCTS_DATA = [
            {
                id: 'prod-1',
                title: 'Test de Ansiedad e Inventario de Beck (BAI)',
                category: 'Test Psicológicos',
                price: 29.00,
                rating: 4.9,
                author: 'Aaron T. Beck (Adaptado)',
                pages: 'Manual + Protocolos (PDF)',
                size: '8.4 MB',
                format: 'PDF Editable + Clave',
                image: 'https://images.unsplash.com/photo-1544716278-ca5e3f4abd8c?auto=format&fit=crop&w=600&q=80',
                featured: true,
                description: 'Instrumento de autoinforme de 21 ítems diseñado para medir la gravedad de la sintomatología ansiosa en adultos y adolescentes. Incluye baremos actualizados y plantilla de corrección automática en Excel.',
                syllabus: [
                    'Manual de aplicación e interpretación clínica',
                    'Protocolo de preguntas listo para imprimir',
                    'Plantilla de calificación automatizada (Excel)',
                    'Informe tipo editable en formato Word'
                ]
            },
            {
                id: 'prod-2',
                title: 'Manual de Terapia Cognitivo-Conductual Práctica',
                category: 'Manuales',
                price: 45.00,
                rating: 4.8,
                author: 'Dr. Roberto Mendoza',
                pages: '310 Páginas',
                size: '22.1 MB',
                format: 'Ebook PDF',
                image: 'https://images.unsplash.com/photo-1532012197267-da84d127e765?auto=format&fit=crop&w=600&q=80',
                featured: true,
                description: 'Guía clínica paso a paso con técnicas avanzadas de reestructuración cognitiva, registro de pensamientos automáticos y hojas de trabajo para pacientes en consulta psicológica.',
                syllabus: [
                    'Capítulo 1: Conceptualización de casos complejos',
                    'Capítulo 2: Protocolos de intervención por trastorno',
                    'Anexo: 30 Fichas de trabajo para el paciente',
                    'Guía de tareas intersesión'
                ]
            },
            {
                id: 'prod-3',
                title: 'Evaluación Neuropsicológica Integral Infantil (ENI)',
                category: 'Evaluaciones',
                price: 59.00,
                rating: 5.0,
                author: 'Instituto Neuropsicológico',
                pages: 'Kit Completo (PDF)',
                size: '45.0 MB',
                format: 'PDF / Material Gráfico',
                image: 'https://images.unsplash.com/photo-1503676260728-1c00da094a0b?auto=format&fit=crop&w=600&q=80',
                featured: true,
                description: 'Batería para evaluar funciones cognitivas en niños de 5 a 16 años. Evalúa atención, memoria, lenguaje, habilidades visuoespaciales y funciones ejecutivas.',
                syllabus: [
                    'Manual de aplicación y normas de puntuación',
                    'Cuadernillos de estímulos en alta resolución',
                    'Hojas de anotación por rangos de edad',
                    'Programa de interpretación de perfil'
                ]
            },
            {
                id: 'prod-4',
                title: 'Guía Práctica del DSM-5-TR para Clínicos',
                category: 'Libros',
                price: 39.00,
                rating: 4.7,
                author: 'Dra. Claudia Salazar',
                pages: '240 Páginas',
                size: '15.8 MB',
                format: 'PDF Interactivo',
                image: 'https://images.unsplash.com/photo-1497633762265-9d179a990aa6?auto=format&fit=crop&w=600&q=80',
                featured: false,
                description: 'Resumen estructurado con esquemas de diagnóstico diferencial, criterios de clasificación actualizados y casos clínicos resueltos.',
                syllabus: [
                    'Criterios diagnósticos resumidos',
                    'Algoritmos de decisión diferencial',
                    'Novedades de la revisión de texto (DSM-5-TR)',
                    'Casos de estudio comentados'
                ]
            },
            {
                id: 'prod-5',
                title: 'Test de Inteligencia Emocional e Inteligencia Social',
                category: 'Test Psicológicos',
                price: 35.00,
                rating: 4.9,
                author: 'Eq-Test Lab',
                pages: 'Batería Digital',
                size: '11.2 MB',
                format: 'PDF + Excel Autocalificable',
                image: 'https://images.unsplash.com/photo-1516321318423-f06f85e504b3?auto=format&fit=crop&w=600&q=80',
                featured: false,
                description: 'Evaluación multidimensional de las competencias emocionales en el ámbito personal y organizacional. Mide intrapersonal, interpersonal, adaptabilidad y manejo del estrés.',
                syllabus: [
                    'Cuestionario digital de 60 ítems',
                    'Plantilla de resultados gráficos automáticos',
                    'Manual de interpretación de competencias',
                    'Guía para la elaboración de informe ejecutivo'
                ]
            },
            {
                id: 'prod-6',
                title: 'Manual de Intervención en Crisis y Primeros Auxilios Psicológicos',
                category: 'Manuales',
                price: 28.00,
                rating: 4.6,
                author: 'Red de Salud Mental',
                pages: '180 Páginas',
                size: '9.5 MB',
                format: 'Ebook PDF',
                image: 'https://images.unsplash.com/photo-1576091160399-112ba8d25d1d?auto=format&fit=crop&w=600&q=80',
                featured: false,
                description: 'Protocolo de actuación urgente ante eventos traumáticos, duelo agudo e emergencias comunitarias. Ideal para psicólogos, médicos y personal de socorro.',
                syllabus: [
                    'Protocolo ABCDE de primeros auxilios psicológicos',
                    'Manejo de estados de agitación e hiperactivación',
                    'Formularios de triaje psicológico',
                    'Material de autoayuda para entregar a sobrevivientes'
                ]
            }
        ];

        /**
         * Main Application Controller Object
         */
        const app = {
            products: [...PRODUCTS_DATA],
            filteredProducts: [...PRODUCTS_DATA],
            cart: [],
            activeCategory: 'Todos',
            currentPayMethod: 'yape',
            
            init() {
                this.renderCategories();
                this.renderProducts();
                this.updateCartUI();
            },

            // --- CATEGORIES & FILTERS ---
            getCategories() {
                const categories = ['Todos', ...new Set(PRODUCTS_DATA.map(p => p.category))];
                return categories;
            },

            renderCategories() {
                const container = document.getElementById('categoryContainer');
                const categories = this.getCategories();

                container.innerHTML = categories.map(cat => {
                    const isActive = cat === this.activeCategory;
                    return `
                        <button onclick="app.setCategory('${cat}')" 
                                class="px-4 py-2 rounded-xl text-xs font-bold transition shrink-0 ${
                                    isActive 
                                    ? 'bg-brand-600 text-white shadow-md shadow-brand-500/20' 
                                    : 'bg-white text-slate-600 hover:bg-slate-100 border border-slate-200'
                                }">
                            ${cat}
                        </button>
                    `;
                }).join('');
            },

            setCategory(cat) {
                this.activeCategory = cat;
                this.renderCategories();
                this.applyFilters();
            },

            handleSearch(e) {
                if (e.key === 'Enter') {
                    this.applyFilters();
                }
            },

            executeSearch() {
                this.applyFilters();
            },

            handleSort(sortType) {
                if (sortType === 'price-low') {
                    this.filteredProducts.sort((a, b) => a.price - b.price);
                } else if (sortType === 'price-high') {
                    this.filteredProducts.sort((a, b) => b.price - a.price);
                } else if (sortType === 'title') {
                    this.filteredProducts.sort((a, b) => a.title.localeCompare(b.title));
                } else {
                    // Featured / Default
                    this.filteredProducts.sort((a, b) => (b.featured ? 1 : 0) - (a.featured ? 1 : 0));
                }
                this.renderProducts();
            },

            applyFilters() {
                const query = document.getElementById('searchInput').value.toLowerCase().trim();

                this.filteredProducts = PRODUCTS_DATA.filter(p => {
                    const matchesCategory = this.activeCategory === 'Todos' || p.category === this.activeCategory;
                    const matchesQuery = p.title.toLowerCase().includes(query) || 
                                         p.description.toLowerCase().includes(query) || 
                                         p.author.toLowerCase().includes(query);
                    return matchesCategory && matchesQuery;
                });

                const sortSelect = document.getElementById('sortSelect');
                this.handleSort(sortSelect.value);
            },

            resetFilter() {
                document.getElementById('searchInput').value = '';
                this.activeCategory = 'Todos';
                this.renderCategories();
                this.filteredProducts = [...PRODUCTS_DATA];
                this.renderProducts();
            },

            // --- CATALOG RENDERING ---
            renderProducts() {
                const grid = document.getElementById('productsGrid');
                const emptyState = document.getElementById('emptyState');

                if (this.filteredProducts.length === 0) {
                    grid.innerHTML = '';
                    emptyState.classList.remove('hidden');
                    return;
                }

                emptyState.classList.add('hidden');
                grid.innerHTML = this.filteredProducts.map(p => `
                    <div class="bg-white rounded-3xl border border-slate-200/80 overflow-hidden shadow-sm hover:shadow-xl transition-all duration-300 flex flex-col group">
                        <!-- Image & Badge Container -->
                        <div class="relative h-48 overflow-hidden bg-slate-100">
                            <img src="${p.image}" alt="${p.title}" class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500" onerror="this.src='https://placehold.co/600x400/312e81/ffffff?text=Recurso+Digital'">
                            <span class="absolute top-3 left-3 bg-slate-900/80 backdrop-blur-md text-white text-[10px] font-extrabold px-2.5 py-1 rounded-full uppercase tracking-wider">
                                ${p.category}
                            </span>
                            <span class="absolute top-3 right-3 bg-emerald-500 text-white text-[10px] font-black px-2 py-0.5 rounded-md shadow">
                                PDF
                            </span>
                        </div>

                        <!-- Card Body -->
                        <div class="p-5 flex-grow flex flex-col justify-between space-y-3">
                            <div>
                                <div class="flex items-center justify-between text-xs text-slate-400 mb-1">
                                    <span><i class="fa-solid fa-user-pen mr-1"></i> ${p.author}</span>
                                    <span class="text-amber-500 font-bold"><i class="fa-solid fa-star"></i> ${p.rating}</span>
                                </div>
                                <h3 onclick="app.openDetailModal('${p.id}')" class="font-bold text-slate-800 text-base leading-snug line-clamp-2 hover:text-brand-600 transition cursor-pointer">
                                    ${p.title}
                                </h3>
                                <p class="text-xs text-slate-500 mt-2 line-clamp-2 leading-relaxed">
                                    ${p.description}
                                </p>
                            </div>

                            <div class="pt-2 border-t border-slate-100 flex items-center justify-between">
                                <div>
                                    <span class="text-[10px] text-slate-400 uppercase font-bold block">Precio Digital</span>
                                    <span class="text-lg font-black text-slate-900">S/ ${p.price.toFixed(2)}</span>
                                </div>
                                <div class="flex items-center space-x-1">
                                    <button onclick="app.openDetailModal('${p.id}')" class="p-2.5 rounded-xl bg-slate-100 hover:bg-slate-200 text-slate-600 transition" title="Ver Detalle">
                                        <i class="fa-solid fa-eye text-xs"></i>
                                    </button>
                                    <button onclick="app.addToCart('${p.id}')" class="px-3.5 py-2.5 rounded-xl bg-brand-600 hover:bg-brand-700 text-white font-bold text-xs shadow-md shadow-brand-500/20 transition flex items-center space-x-1.5">
                                        <i class="fa-solid fa-cart-plus"></i>
                                        <span>Comprar</span>
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                `).join('');
            },

            // --- DETAIL MODAL ---
            openDetailModal(productId) {
                const p = PRODUCTS_DATA.find(item => item.id === productId);
                if (!p) return;

                document.getElementById('modalCategory').textContent = p.category;
                document.getElementById('modalTitle').textContent = p.title;
                document.getElementById('modalAuthor').textContent = p.author;
                document.getElementById('modalFormat').innerHTML = `<i class="fa-regular fa-file-pdf text-red-500 mr-1"></i> ${p.format}`;
                document.getElementById('modalPages').textContent = p.pages;
                document.getElementById('modalSize').textContent = p.size;
                document.getElementById('modalDescription').textContent = p.description;
                document.getElementById('modalPrice').textContent = `S/ ${p.price.toFixed(2)}`;

                const syllabusList = document.getElementById('modalSyllabus');
                syllabusList.innerHTML = p.syllabus.map(item => `
                    <li class="flex items-center space-x-2">
                        <i class="fa-solid fa-circle-check text-emerald-500 text-xs"></i>
                        <span>${item}</span>
                    </li>
                `).join('');

                const addBtn = document.getElementById('modalAddBtn');
                addBtn.onclick = () => {
                    this.addToCart(p.id);
                    this.closeDetailModal();
                    this.toggleCartModal(true);
                };

                const modal = document.getElementById('detailModal');
                const content = document.getElementById('detailModalContent');
                modal.classList.remove('opacity-0', 'pointer-events-none');
                content.classList.remove('scale-95');
                content.classList.add('scale-100');
            },

            closeDetailModal() {
                const modal = document.getElementById('detailModal');
                const content = document.getElementById('detailModalContent');
                content.classList.remove('scale-100');
                content.classList.add('scale-95');
                modal.classList.add('opacity-0', 'pointer-events-none');
            },

            // --- CART LOGIC ---
            addToCart(productId) {
                const p = PRODUCTS_DATA.find(item => item.id === productId);
                if (!p) return;

                const existing = this.cart.find(item => item.id === productId);
                if (!existing) {
                    this.cart.push({ ...p, quantity: 1 });
                    this.showNotification(`"${p.title.substring(0, 25)}..." añadido al carrito`);
                } else {
                    this.showNotification('El recurso digital ya está en tu carrito');
                }
                this.updateCartUI();
            },

            removeFromCart(productId) {
                this.cart = this.cart.filter(item => item.id !== productId);
                this.updateCartUI();
            },

            updateCartUI() {
                const badge = document.getElementById('cartCountBadge');
                const count = this.cart.length;
                badge.textContent = count;
                if (count > 0) {
                    badge.classList.remove('scale-0');
                    badge.classList.add('scale-100');
                } else {
                    badge.classList.remove('scale-100');
                    badge.classList.add('scale-0');
                }

                const list = document.getElementById('cartItemsList');
                const emptyView = document.getElementById('cartEmptyView');
                const checkoutBtn = document.getElementById('cartCheckoutBtn');

                if (count === 0) {
                    list.innerHTML = '';
                    emptyView.classList.remove('hidden');
                    checkoutBtn.disabled = true;
                    checkoutBtn.classList.add('opacity-50', 'cursor-not-allowed');
                } else {
                    emptyView.classList.add('hidden');
                    checkoutBtn.disabled = false;
                    checkoutBtn.classList.remove('opacity-50', 'cursor-not-allowed');

                    list.innerHTML = this.cart.map(item => `
                        <div class="flex items-center justify-between bg-slate-50 p-3 rounded-2xl border border-slate-200/80">
                            <div class="flex items-center space-x-3 pr-2">
                                <div class="w-12 h-12 rounded-xl bg-slate-200 overflow-hidden shrink-0">
                                    <img src="${item.image}" class="w-full h-full object-cover">
                                </div>
                                <div>
                                    <h4 class="font-bold text-slate-800 text-xs line-clamp-1">${item.title}</h4>
                                    <span class="text-[10px] text-slate-400 font-semibold uppercase">${item.category}</span>
                                    <span class="font-extrabold text-slate-900 text-xs block mt-0.5">S/ ${item.price.toFixed(2)}</span>
                                </div>
                            </div>
                            <button onclick="app.removeFromCart('${item.id}')" class="text-slate-400 hover:text-red-500 p-2 transition shrink-0">
                                <i class="fa-solid fa-trash-can text-sm"></i>
                            </button>
                        </div>
                    `).join('');
                }

                const total = this.cart.reduce((sum, i) => sum + i.price, 0);
                document.getElementById('cartSubtotal').textContent = `S/ ${total.toFixed(2)}`;
                document.getElementById('cartTotal').textContent = `S/ ${total.toFixed(2)}`;
            },

            toggleCartModal(forceOpen = false) {
                const modal = document.getElementById('cartModal');
                const content = document.getElementById('cartContent');

                if (forceOpen || modal.classList.contains('opacity-0')) {
                    modal.classList.remove('opacity-0', 'pointer-events-none');
                    content.classList.remove('translate-x-full');
                } else {
                    content.classList.add('translate-x-full');
                    modal.classList.add('opacity-0', 'pointer-events-none');
                }
            },

            // --- CHECKOUT & PAYMENT SWITCHING ---
            goToCheckout() {
                if (this.cart.length === 0) return;
                this.toggleCartModal(false);

                const total = this.cart.reduce((sum, i) => sum + i.price, 0);
                document.getElementById('checkoutYapeAmount').textContent = `S/ ${total.toFixed(2)}`;
                document.getElementById('checkoutSubtotal').textContent = `S/ ${total.toFixed(2)}`;
                document.getElementById('checkoutTotal').textContent = `S/ ${total.toFixed(2)}`;

                // Populate Summary
                const summaryList = document.getElementById('checkoutSummaryList');
                summaryList.innerHTML = this.cart.map(item => `
                    <div class="flex justify-between items-center text-xs pb-1 border-b border-slate-100">
                        <span class="text-slate-700 font-medium line-clamp-1 pr-2">${item.title}</span>
                        <span class="font-bold text-slate-900 shrink-0">S/ ${item.price.toFixed(2)}</span>
                    </div>
                `).join('');

                const modal = document.getElementById('checkoutModal');
                const content = document.getElementById('checkoutModalContent');
                modal.classList.remove('opacity-0', 'pointer-events-none');
                content.classList.remove('scale-95');
                content.classList.add('scale-100');
            },

            closeCheckoutModal() {
                const modal = document.getElementById('checkoutModal');
                const content = document.getElementById('checkoutModalContent');
                content.classList.remove('scale-100');
                content.classList.add('scale-95');
                modal.classList.add('opacity-0', 'pointer-events-none');
            },

            switchPayMethod(method) {
                this.currentPayMethod = method;
                const yapePanel = document.getElementById('payPanelYape');
                const cardPanel = document.getElementById('payPanelCard');

                if (method === 'yape') {
                    yapePanel.classList.remove('hidden');
                    cardPanel.classList.add('hidden');
                } else {
                    yapePanel.classList.add('hidden');
                    cardPanel.classList.remove('hidden');
                }
            },

            // --- PAYMENT VERIFICATION & AUTOMATIC DOWNLOAD SIMULATION ---
            processPaymentVerification() {
                const name = document.getElementById('custName').value.trim();
                const email = document.getElementById('custEmail').value.trim();
                const phone = document.getElementById('custPhone').value.trim();
                const opNum = document.getElementById('opNumber').value.trim();

                if (!name || !email || !phone) {
                    alert('Por favor, completa tus datos personales (Nombre, Correo y Teléfono).');
                    return;
                }

                if (this.currentPayMethod === 'yape' && !opNum) {
                    alert('Por favor ingresa el número de operación de Yape/Plin para comprobar la transferencia.');
                    return;
                }

                // Close checkout, open verification loader
                this.closeCheckoutModal();

                const verifyModal = document.getElementById('verifyModal');
                const progressBar = document.getElementById('verifyProgressBar');
                verifyModal.classList.remove('opacity-0', 'pointer-events-none');

                let progress = 0;
                const interval = setInterval(() => {
                    progress += 25;
                    progressBar.style.width = `${progress}%`;

                    if (progress >= 100) {
                        clearInterval(interval);
                        setTimeout(() => {
                            verifyModal.classList.add('opacity-0', 'pointer-events-none');
                            this.showSuccessAndDownloads(email);
                        }, 500);
                    }
                }, 400);
            },

            showSuccessAndDownloads(email) {
                document.getElementById('successEmailDisplay').textContent = email;
                
                const total = this.cart.reduce((sum, i) => sum + i.price, 0);
                document.getElementById('receiptTotalAmount').textContent = `S/ ${total.toFixed(2)}`;
                document.getElementById('receiptOrderNum').textContent = `MS-${Math.floor(10000 + Math.random() * 90000)}`;
                
                const now = new Date();
                document.getElementById('receiptDate').textContent = `${now.toLocaleDateString('es-PE')} ${now.toLocaleTimeString('es-PE', {hour: '2-digit', minute:'2-digit'})}`;

                // Populate Download Links
                const downloadsList = document.getElementById('downloadsList');
                downloadsList.innerHTML = this.cart.map(item => `
                    <div class="bg-brand-50 border border-brand-200 rounded-2xl p-4 flex flex-col sm:flex-row items-start sm:items-center justify-between gap-3">
                        <div class="flex items-center space-x-3">
                            <div class="w-10 h-10 rounded-xl bg-brand-600 text-white flex items-center justify-center shrink-0 shadow">
                                <i class="fa-regular fa-file-pdf text-lg"></i>
                            </div>
                            <div>
                                <h5 class="font-bold text-slate-800 text-xs">${item.title}</h5>
                                <span class="text-[10px] text-brand-700 font-semibold">${item.format} • ${item.size}</span>
                            </div>
                        </div>
                        <button onclick="app.triggerDownload('${item.title}')" class="w-full sm:w-auto px-4 py-2 bg-brand-600 hover:bg-brand-700 text-white font-extrabold text-xs rounded-xl shadow-md transition flex items-center justify-center space-x-1.5 shrink-0">
                            <i class="fa-solid fa-download"></i>
                            <span>Descargar PDF</span>
                        </button>
                    </div>
                `).join('');

                const successModal = document.getElementById('successModal');
                successModal.classList.remove('opacity-0', 'pointer-events-none');

                // Clear cart
                this.cart = [];
                this.updateCartUI();
            },

            triggerDownload(title) {
                // Generate a dummy blob text for instant real file download simulation
                const dummyContent = `%PDF-1.4 Mock Document\nTitle: ${title}\nMente & Saber - Recursos Digitales\nGracias por tu compra. Este es un archivo PDF de prueba demostrativo.`;
                const blob = new Blob([dummyContent], { type: 'application/pdf' });
                const url = window.URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.href = url;
                a.download = `${title.replace(/[^a-zA-Z0-9]/g, '_')}_MenteSaber.pdf`;
                document.body.appendChild(a);
                a.click();
                document.body.removeChild(a);
                window.URL.revokeObjectURL(url);

                this.showNotification('¡Descarga iniciada automáticamente!');
            },

            closeSuccessModal() {
                const modal = document.getElementById('successModal');
                modal.classList.add('opacity-0', 'pointer-events-none');
            },

            // Helper Utility
            copyToClipboard(text) {
                const input = document.createElement('input');
                input.value = text;
                document.body.appendChild(input);
                input.select();
                document.execCommand('copy');
                document.body.removeChild(input);
                this.showNotification('Número de Yape copiado al portapapeles');
            },

            showNotification(msg) {
                const toast = document.createElement('div');
                toast.className = 'fixed bottom-5 right-5 bg-slate-900 text-white px-4 py-3 rounded-2xl shadow-2xl z-50 text-xs font-bold flex items-center space-x-2 transition-all duration-300 transform translate-y-10 opacity-0';
                toast.innerHTML = `<i class="fa-solid fa-circle-check text-emerald-400 text-sm"></i> <span>${msg}</span>`;
                document.body.appendChild(toast);

                setTimeout(() => {
                    toast.classList.remove('translate-y-10', 'opacity-0');
                }, 50);

                setTimeout(() => {
                    toast.classList.add('translate-y-10', 'opacity-0');
                    setTimeout(() => toast.remove(), 300);
                }, 3000);
            }
        };

        // Initialize on DOM Ready
        window.addEventListener('DOMContentLoaded', () => {
            app.init();
        });
    </script>
</body>
</html>
