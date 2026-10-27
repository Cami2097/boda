# boda<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nuestra Boda - Camila & Daniel</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;0,600;1,400&family=Montserrat:wght@300;400;500;600&family=Great+Vibes&display=swap" rel="stylesheet">

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        champagne: '#F3E5D8',
                        gold: '#D4AF37',
                        goldDark: '#AA820A',
                        eucalyptus: '#3D5A4E',
                        eucalyptusLight: '#658376',
                        blush: '#FDF7F5',
                        roseGold: '#C58F87',
                        darkText: '#2C3531'
                    },
                    fontFamily: {
                        serif: ['"Cormorant Garamond"', 'serif'],
                        sans: ['Montserrat', 'sans-serif'],
                        script: ['"Great Vibes"', 'cursive']
                    }
                }
            }
        }
    </script>
    <style>
        html {
            scroll-behavior: smooth;
        }
        .bg-pattern {
            background-color: #fdfbf7;
            background-image: radial-gradient(#658376 0.4px, transparent 0.4px), radial-gradient(#658376 0.4px, #fdfbf7 0.4px);
            background-size: 24px 24px;
            background-position: 0 0, 12px 12px;
        }
        .gold-gradient-text {
            background: linear-gradient(135deg, #B8860B, #D4AF37, #AA820A, #E6C687);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        .glass-card {
            background: rgba(255, 255, 255, 0.90);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(212, 175, 55, 0.25);
        }
        .timeline-line::before {
            content: '';
            position: absolute;
            top: 0;
            bottom: 0;
            left: 50%;
            width: 2px;
            background: #D4AF37;
            transform: translateX(-50%);
        }
    </style>
</head>
<body class="font-sans text-darkText bg-pattern leading-relaxed overflow-x-hidden">

    <!-- BOTONES FLOTANTES: MÚSICA & EDITOR -->
    <div class="fixed top-4 right-4 z-50 flex flex-col gap-3">
        <button id="musicToggle" class="w-12 h-12 rounded-full bg-eucalyptus text-white shadow-xl flex items-center justify-center hover:bg-eucalyptusLight transition-all duration-300 group" title="Activar/Pausar Música">
            <i id="musicIcon" class="fas fa-music text-lg group-hover:scale-110"></i>
        </button>
        <button id="openConfigBtn" class="w-12 h-12 rounded-full bg-gold text-white shadow-xl flex items-center justify-center hover:bg-goldDark transition-all duration-300" title="Editar Contenidos">
            <i class="fas fa-sliders-h text-lg"></i>
        </button>
    </div>

    <!-- MODAL INICIAL DE BIENVENIDA & SELECCIÓN DE PASES PARA INVITADOS -->
    <div id="guestGateModal" class="fixed inset-0 z-50 bg-black/75 backdrop-blur-md flex items-center justify-center p-4">
        <div class="glass-card max-w-md w-full rounded-3xl p-6 md:p-8 text-center shadow-2xl relative border-2 border-gold/40">
            <div class="w-16 h-16 mx-auto mb-4 rounded-full bg-eucalyptus/10 flex items-center justify-center text-eucalyptus">
                <i class="fas fa-envelope-open text-2xl"></i>
            </div>
            <p class="text-xs uppercase tracking-[0.25em] text-goldDark font-semibold mb-1">Invitación Oficial</p>
            <h2 class="font-serif text-3xl font-bold text-eucalyptus mb-6">Camila & Daniel</h2>

            <!-- Opción 1: Lista Predefinida -->
            <div class="mb-4 text-left">
                <label class="block text-xs font-semibold uppercase tracking-wider text-gray-500 mb-2">Selecciona tu nombre en la lista:</label>
                <select id="guestDropdown" class="w-full p-3.5 rounded-xl border border-gold/40 bg-white text-sm focus:ring-2 focus:ring-eucalyptus outline-none shadow-sm">
                    <!-- Dinámico vía JS -->
                </select>
            </div>

            <div class="flex items-center my-4">
                <div class="flex-grow border-t border-gray-300"></div>
                <span class="px-3 text-[11px] text-gray-400 font-semibold uppercase">O ingresa manualmente</span>
                <div class="flex-grow border-t border-gray-300"></div>
            </div>

            <!-- Opción 2: Ingreso Manual -->
            <div class="space-y-3 text-left">
                <input type="text" id="customGuestName" placeholder="Tu Nombre / Nombre de Familia" class="w-full p-3 rounded-xl border border-gray-300 text-sm focus:ring-2 focus:ring-eucalyptus outline-none">
                <input type="number" id="customGuestPasses" min="1" max="10" placeholder="Número de pases asignados (Ej. 2)" class="w-full p-3 rounded-xl border border-gray-300 text-sm focus:ring-2 focus:ring-eucalyptus outline-none">
            </div>

            <button id="enterInvitationBtn" class="w-full mt-6 py-4 bg-gradient-to-r from-eucalyptus to-eucalyptusLight text-white font-semibold rounded-xl shadow-lg hover:shadow-2xl hover:opacity-95 transition-all text-xs uppercase tracking-widest">
                Abrir Invitación
            </button>
        </div>
    </div>

    <!-- CONTENIDO PRINCIPAL DE LA INVITACIÓN -->
    <div id="mainContent" class="hidden">
        
        <!-- PORTADA / HERO SECTION -->
        <header class="min-h-screen flex flex-col justify-between items-center text-center px-4 py-12 relative overflow-hidden bg-cover bg-center" style="background-image: linear-gradient(rgba(253,251,247,0.90), rgba(253,251,247,0.80)), url('https://images.unsplash.com/photo-1519741497674-611481863552?auto=format&fit=crop&q=80');">
            
            <div class="mt-8">
                <p class="uppercase tracking-[0.35em] text-xs font-semibold text-eucalyptus mb-2">¡Nos Casamos!</p>
                <div class="w-16 h-0.5 bg-gold mx-auto mb-4"></div>
                <p class="font-serif italic text-gray-600 text-sm md:text-base max-w-lg mx-auto">
                    "Las muchas aguas no podrán apagar el amor, ni lo ahogarán los ríos."
                    <br><span class="text-xs font-sans not-italic font-semibold text-goldDark">— Cantares 8:7</span>
                </p>
            </div>

            <!-- NOMBRES & PASE PERSONALIZADO -->
            <div class="my-auto py-6">
                <h1 class="font-script text-6xl md:text-8xl gold-gradient-text drop-shadow-sm py-2">Camila & Daniel</h1>
                
                <!-- Tarjeta con Pase del Invitado -->
                <div class="mt-6 inline-block glass-card px-8 py-4 rounded-full shadow-lg border border-gold/40">
                    <p class="text-[10px] uppercase tracking-widest text-gray-400 font-semibold">Invitación Reservada Especialmente Para:</p>
                    <p id="displayGuestName" class="font-serif text-2xl md:text-3xl text-eucalyptus font-bold">Invitado de Honor</p>
                    <div class="flex items-center justify-center gap-2 text-xs text-goldDark mt-1">
                        <i class="fas fa-ticket-alt"></i>
                        <span id="displayGuestPasses" class="font-semibold">1 Pase VIP</span>
                    </div>
                </div>
            </div>

            <!-- FECHA & LUGAR -->
            <div class="mb-4">
                <p class="font-serif text-3xl md:text-4xl font-semibold tracking-widest text-darkText">06 • 03 • 2027</p>
                <p class="text-xs uppercase tracking-widest text-eucalyptus font-bold mt-1">Quito - Ecuador</p>
                
                <a href="#padres" class="inline-block mt-8 text-eucalyptus animate-bounce">
                    <i class="fas fa-chevron-down text-xl"></i>
                </a>
            </div>
        </header>

        <!-- SECCIÓN NUESTROS PADRES -->
        <section id="padres" class="py-16 px-4 max-w-4xl mx-auto text-center">
            <div class="glass-card p-8 md:p-12 rounded-3xl shadow-xl border border-gold/30">
                <p class="font-script text-4xl text-goldDark mb-1">Con la bendición de Dios y</p>
                <h2 class="font-serif text-2xl md:text-3xl font-bold text-eucalyptus mb-8 uppercase tracking-wider">Nuestros Padres</h2>
                
                <div class="grid md:grid-cols-2 gap-8 text-center">
                    <!-- Padres Novia -->
                    <div class="p-6 rounded-2xl bg-blush border border-roseGold/20">
                        <i class="fas fa-heart text-roseGold text-lg mb-2"></i>
                        <h3 class="font-serif text-sm font-semibold text-eucalyptus uppercase tracking-widest mb-3">Padres de la Novia</h3>
                        <p class="font-serif text-xl font-bold text-darkText">Lucía Portilla</p>
                        <p class="text-xs text-goldDark font-serif italic my-1">y</p>
                        <p class="font-serif text-xl font-bold text-darkText">Ricardo Palacios</p>
                    </div>
                    
                    <!-- Padres Novio -->
                    <div class="p-6 rounded-2xl bg-blush border border-roseGold/20">
                        <i class="fas fa-heart text-roseGold text-lg mb-2"></i>
                        <h3 class="font-serif text-sm font-semibold text-eucalyptus uppercase tracking-widest mb-3">Padres del Novio</h3>
                        <p class="font-serif text-xl font-bold text-darkText">Grace Andrade</p>
                        <p class="text-xs text-goldDark font-serif italic my-1">e</p>
                        <p class="font-serif text-xl font-bold text-darkText">Santiago González</p>
                    </div>
                </div>

                <div class="mt-8 pt-6 border-t border-gold/20">
                    <p class="font-serif italic text-gray-600 text-sm">
                        "Mejor son dos que uno... porque si cayeren, el uno levantará a su compañero."
                        <span class="block text-xs font-sans not-italic text-goldDark mt-1">— Eclesiastés 4:9-10</span>
                    </p>
                </div>
            </div>
        </section>

        <!-- CUENTA REGRESIVA EN TIEMPO REAL -->
        <section class="py-14 bg-eucalyptus text-white text-center px-4 relative">
            <div class="max-w-4xl mx-auto">
                <h3 class="font-serif text-2xl md:text-3xl tracking-widest uppercase mb-2">Faltan Muy Pocos Días</h3>
                <p class="text-xs font-light text-champagne uppercase tracking-widest mb-8">Para nuestro "Sí, Acepto"</p>
                
                <div class="grid grid-cols-2 sm:grid-cols-4 gap-4 max-w-2xl mx-auto">
                    <div class="bg-white/10 backdrop-blur-md p-4 rounded-2xl border border-white/20">
                        <span id="cd-days" class="block font-serif text-4xl md:text-5xl font-bold text-champagne">00</span>
                        <span class="text-[10px] uppercase tracking-widest text-white/80">Días</span>
                    </div>
                    <div class="bg-white/10 backdrop-blur-md p-4 rounded-2xl border border-white/20">
                        <span id="cd-hours" class="block font-serif text-4xl md:text-5xl font-bold text-champagne">00</span>
                        <span class="text-[10px] uppercase tracking-widest text-white/80">Horas</span>
                    </div>
                    <div class="bg-white/10 backdrop-blur-md p-4 rounded-2xl border border-white/20">
                        <span id="cd-minutes" class="block font-serif text-4xl md:text-5xl font-bold text-champagne">00</span>
                        <span class="text-[10px] uppercase tracking-widest text-white/80">Minutos</span>
                    </div>
                    <div class="bg-white/10 backdrop-blur-md p-4 rounded-2xl border border-white/20">
                        <span id="cd-seconds" class="block font-serif text-4xl md:text-5xl font-bold text-champagne">00</span>
                        <span class="text-[10px] uppercase tracking-widest text-white/80">Segundos</span>
                    </div>
                </div>
            </div>
        </section>

        <!-- ITINERARIO ACTUALIZADO CON LOS HORARIOS SOLICITADOS -->
        <section class="py-16 px-4 max-w-4xl mx-auto">
            <div class="text-center mb-12">
                <p class="font-script text-4xl text-goldDark">Cronograma del Día</p>
                <h2 class="font-serif text-3xl font-bold text-eucalyptus uppercase tracking-wider">Itinerario de la Boda</h2>
                <div class="w-16 h-0.5 bg-gold mx-auto mt-2"></div>
            </div>

            <div class="grid md:grid-cols-2 gap-8">
                <!-- Evento 1: Registro Civil -->
                <div class="glass-card p-8 rounded-3xl border-l-4 border-l-gold shadow-lg relative hover:shadow-xl transition-all">
                    <div class="flex items-center gap-4 mb-4">
                        <div class="w-12 h-12 rounded-full bg-champagne flex items-center justify-center text-goldDark text-xl font-bold">
                            <i class="fas fa-pen-fancy"></i>
                        </div>
                        <div>
                            <span class="text-xs font-bold text-goldDark uppercase tracking-widest block">16:00 PM (4:00 PM)</span>
                            <h3 class="font-serif text-2xl font-bold text-eucalyptus">Matrimonio Civil</h3>
                        </div>
                    </div>
                    <p class="text-sm text-gray-600 mb-4">Ceremonia oficial de unión civil de Camila y Daniel.</p>
                    <div class="text-xs text-gray-500 font-medium border-t border-gray-100 pt-3">
                        <i class="fas fa-map-marker-alt text-eucalyptus mr-1"></i> Registro Civil (Quito, Ecuador)
                    </div>
                </div>

                <!-- Evento 2: Recepción La Capricciosa -->
                <div class="glass-card p-8 rounded-3xl border-l-4 border-l-eucalyptus shadow-lg relative hover:shadow-xl transition-all">
                    <div class="flex items-center gap-4 mb-4">
                        <div class="w-12 h-12 rounded-full bg-eucalyptus/10 flex items-center justify-center text-eucalyptus text-xl font-bold">
                            <i class="fas fa-glass-cheers"></i>
                        </div>
                        <div>
                            <span class="text-xs font-bold text-eucalyptus uppercase tracking-widest block">18:00 PM (6:00 PM)</span>
                            <h3 class="font-serif text-2xl font-bold text-eucalyptus">Recepción & Fiesta</h3>
                        </div>
                    </div>
                    <p class="text-sm text-gray-600 mb-4">Brindis de bienvenida, banquete gourmet y fiesta de celebración.</p>
                    <div class="text-xs text-gray-500 font-medium border-t border-gray-100 pt-3">
                        <i class="fas fa-map-marker-alt text-eucalyptus mr-1"></i> La Capricciosa (Quito, Ecuador)
                    </div>
                </div>
            </div>
        </section>

        <!-- UBICACIÓN Y MAPAS INTERACTIVOS -->
        <section class="py-16 bg-blush/60 px-4">
            <div class="max-w-4xl mx-auto text-center">
                <h2 class="font-serif text-3xl font-bold text-eucalyptus uppercase tracking-wider mb-2">Ubicaciones del Evento</h2>
                <p class="text-sm text-gray-600 mb-8">Te esperamos en nuestros dos puntos de encuentro en Quito:</p>

                <div class="grid md:grid-cols-2 gap-8">
                    <!-- Tarjeta Lugar 1: Registro Civil -->
                    <div class="glass-card p-6 rounded-3xl shadow-md text-left flex flex-col justify-between">
                        <div>
                            <span class="text-[10px] font-bold uppercase tracking-widest text-goldDark">Ceremonia Civil - 4:00 PM</span>
                            <h3 class="font-serif text-xl font-bold text-eucalyptus mt-1">Registro Civil Quito</h3>
                            <p class="text-xs text-gray-500 mt-2 mb-4">Sede oficial de enlace matrimonial civil.</p>
                        </div>
                        <div class="flex gap-2">
                            <a href="https://maps.google.com/?q=Registro+Civil+Quito" target="_blank" class="flex-1 py-2.5 bg-eucalyptus text-white text-center rounded-xl text-xs font-semibold uppercase tracking-wider hover:bg-eucalyptusLight transition-all">
                                <i class="fas fa-map-marked-alt mr-1"></i> Google Maps
                            </a>
                        </div>
                    </div>

                    <!-- Tarjeta Lugar 2: La Capricciosa -->
                    <div class="glass-card p-6 rounded-3xl shadow-md text-left flex flex-col justify-between border-2 border-gold/30">
                        <div>
                            <span class="text-[10px] font-bold uppercase tracking-widest text-goldDark">Recepción & Celebración - 6:00 PM</span>
                            <h3 class="font-serif text-xl font-bold text-eucalyptus mt-1">La Capricciosa</h3>
                            <p class="text-xs text-gray-500 mt-2 mb-4">Quito, Ecuador.</p>
                        </div>
                        <div class="flex gap-2">
                            <a href="https://maps.google.com/?q=La+Capricciosa+Quito" target="_blank" class="flex-1 py-2.5 bg-gold text-white text-center rounded-xl text-xs font-semibold uppercase tracking-wider hover:bg-goldDark transition-all">
                                <i class="fas fa-map-marked-alt mr-1"></i> Google Maps
                            </a>
                            <a href="https://waze.com/ul?q=La%20Capricciosa%20Quito" target="_blank" class="flex-1 py-2.5 bg-blue-600 text-white text-center rounded-xl text-xs font-semibold uppercase tracking-wider hover:bg-blue-500 transition-all">
                                <i class="fab fa-waze mr-1"></i> Waze
                            </a>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- CÓDIGO DE VESTIMENTA -->
        <section class="py-16 px-4 max-w-3xl mx-auto text-center">
            <div class="glass-card p-8 md:p-12 rounded-3xl shadow-lg border border-gold/30">
                <i class="fas fa-user-tie text-3xl text-goldDark mb-3"></i>
                <h2 class="font-serif text-3xl font-bold text-eucalyptus uppercase tracking-wider mb-2">Código de Vestimenta</h2>
                <p class="font-serif text-2xl text-goldDark font-semibold mb-4">Formal / Etiqueta</p>
                <p class="text-xs text-gray-600 max-w-md mx-auto mb-6">Queremos que luzcas radiante en nuestra gran noche. Agradecemos reservar los tonos Blancos y Marfil para la novia.</p>
                
                <div class="flex justify-center items-center gap-3">
                    <span class="w-8 h-8 rounded-full bg-[#3D5A4E] shadow" title="Verde Eucalipto"></span>
                    <span class="w-8 h-8 rounded-full bg-[#D4AF37] shadow" title="Dorado"></span>
                    <span class="w-8 h-8 rounded-full bg-[#F3E5D8] shadow" title="Champagne"></span>
                    <span class="w-8 h-8 rounded-full bg-[#2C3531] shadow" title="Negro / Oscuro"></span>
                </div>
            </div>
        </section>

        <!-- MESA DE REGALOS / TRANSFERENCIAS BANCARIAS -->
        <section class="py-16 bg-eucalyptus/5 px-4">
            <div class="max-w-3xl mx-auto text-center">
                <div class="glass-card p-8 md:p-12 rounded-3xl shadow-xl border border-gold/40 relative">
                    <i class="fas fa-gift text-4xl text-goldDark mb-3"></i>
                    <h2 class="font-serif text-3xl font-bold text-eucalyptus uppercase tracking-wider mb-2">Mesa de Regalos</h2>
                    <p class="font-serif italic text-gray-600 text-sm mb-6">
                        "El amor es el mayor regalo de todos. Tu presencia es nuestro mejor obsequio, pero si deseas hacernos un detalle en efectivo, te dejamos los datos de nuestra cuenta:"
                    </p>

                    <!-- Datos Bancarios Editables -->
                    <div class="bg-white p-6 rounded-2xl border border-gold/30 shadow-inner max-w-md mx-auto text-left space-y-2 text-xs">
                        <div class="flex justify-between border-b pb-2">
                            <span class="font-semibold text-gray-400">BANCO:</span>
                            <span id="bankNameDisplay" class="font-bold text-eucalyptus">Banco Pichincha</span>
                        </div>
                        <div class="flex justify-between border-b pb-2">
                            <span class="font-semibold text-gray-400">TIPO DE CUENTA:</span>
                            <span id="accountTypeDisplay" class="font-bold text-darkText">Ahorros</span>
                        </div>
                        <div class="flex justify-between border-b pb-2">
                            <span class="font-semibold text-gray-400">TITULAR:</span>
                            <span id="accountHolderDisplay" class="font-bold text-darkText">Camila Palacios</span>
                        </div>
                        <div class="flex justify-between border-b pb-2">
                            <span class="font-semibold text-gray-400">C.I. / RUC:</span>
                            <span id="accountIDDisplay" class="font-bold text-darkText">1712345678</span>
                        </div>
                        <div class="flex justify-between items-center pt-2">
                            <span class="font-semibold text-gray-400">N° DE CUENTA:</span>
                            <span id="accountNumDisplay" class="font-mono font-bold text-goldDark text-base">2200112233</span>
                        </div>
                    </div>

                    <button id="copyAccountBtn" class="mt-6 px-8 py-3 bg-gold text-white rounded-xl shadow-md hover:bg-goldDark transition-all text-xs font-semibold uppercase tracking-wider inline-flex items-center gap-2">
                        <i class="fas fa-copy"></i> Copiar Número de Cuenta
                    </button>
                    <p id="copyAlert" class="text-xs text-eucalyptus font-semibold mt-2 hidden">¡Número de cuenta copiado al portapapeles!</p>
                </div>
            </div>
        </section>

        <!-- SECCIÓN RSVP / CONFIRMACIÓN DE ASISTENCIA -->
        <section class="py-16 px-4 max-w-2xl mx-auto">
            <div class="glass-card p-8 md:p-12 rounded-3xl shadow-xl border border-gold/30 text-center">
                <i class="fab fa-whatsapp text-4xl text-eucalyptus mb-3"></i>
                <h2 class="font-serif text-3xl font-bold text-eucalyptus uppercase tracking-wider mb-2">Confirmación de Asistencia</h2>
                <p class="text-xs text-gray-600 mb-6">Por favor confirma tu asistencia antes del 01 de Febrero de 2027.</p>

                <form id="rsvpForm" class="space-y-4 text-left">
                    <div>
                        <label class="block text-xs font-semibold uppercase text-gray-500 mb-1">Nombre Completo:</label>
                        <input type="text" id="rsvpName" required class="w-full p-3 rounded-xl border border-gray-300 text-sm outline-none focus:ring-2 focus:ring-eucalyptus">
                    </div>

                    <div>
                        <label class="block text-xs font-semibold uppercase text-gray-500 mb-1">¿Asistirás a la celebración?</label>
                        <select id="rsvpAttendance" class="w-full p-3 rounded-xl border border-gray-300 text-sm outline-none focus:ring-2 focus:ring-eucalyptus">
                            <option value="Sí, asistiré con mucho gusto">¡Sí, estaré presente para acompañarlos!</option>
                            <option value="Lamentablemente no podré asistir">Lamentablemente no podré asistir</option>
                        </select>
                    </div>

                    <div>
                        <label class="block text-xs font-semibold uppercase text-gray-500 mb-1">Pases a confirmar:</label>
                        <input type="number" id="rsvpPasses" min="1" max="10" value="1" required class="w-full p-3 rounded-xl border border-gray-300 text-sm outline-none focus:ring-2 focus:ring-eucalyptus">
                    </div>

                    <div>
                        <label class="block text-xs font-semibold uppercase text-gray-500 mb-1">Restricciones alimentarias / Alergias (opcional):</label>
                        <input type="text" id="rsvpDietary" placeholder="Ej. Vegetariano, alérgico a los mariscos, etc." class="w-full p-3 rounded-xl border border-gray-300 text-sm outline-none focus:ring-2 focus:ring-eucalyptus">
                    </div>

                    <button type="submit" class="w-full py-4 bg-eucalyptus text-white font-semibold rounded-xl shadow-lg hover:bg-eucalyptusLight transition-all text-xs uppercase tracking-widest flex items-center justify-center gap-2">
                        <i class="fab fa-whatsapp text-lg"></i> Confirmar Asistencia por WhatsApp
                    </button>
                </form>
            </div>
        </section>

        <!-- FOOTER -->
        <footer class="py-8 bg-eucalyptus text-white text-center">
            <p class="font-script text-3xl text-champagne mb-1">Camila & Daniel</p>
            <p class="text-[11px] uppercase tracking-widest text-white/70">06.03.2027 • Quito, Ecuador</p>
        </footer>

    </div>

    <!-- PANEL DE EDICIÓN / CONFIGURADOR -->
    <div id="configModal" class="fixed inset-0 z-50 bg-black/70 backdrop-blur-md flex items-center justify-center p-4 hidden">
        <div class="glass-card max-w-xl w-full max-h-[90vh] overflow-y-auto rounded-3xl p-6 md:p-8 shadow-2xl relative border-2 border-gold/40">
            <button id="closeConfigBtn" class="absolute top-4 right-4 text-gray-400 hover:text-darkText text-xl">
                <i class="fas fa-times"></i>
            </button>
            <h2 class="font-serif text-2xl font-bold text-eucalyptus mb-1"><i class="fas fa-edit mr-2"></i>Panel de Personalización</h2>
            <p class="text-xs text-gray-500 mb-6">Actualiza la información bancaria y la lista predeterminada de invitados.</p>

            <div class="space-y-6 text-xs">
                <!-- Datos Bancarios -->
                <div class="border-b pb-4">
                    <h3 class="font-semibold text-goldDark uppercase mb-3">Datos Bancarios para Transferencias</h3>
                    <div class="grid grid-cols-2 gap-3">
                        <input type="text" id="editBankName" placeholder="Banco" value="Banco Pichincha" class="p-2.5 border rounded-lg">
                        <input type="text" id="editAccountType" placeholder="Tipo Cuenta" value="Ahorros" class="p-2.5 border rounded-lg">
                        <input type="text" id="editAccountHolder" placeholder="Titular" value="Camila Palacios" class="p-2.5 border rounded-lg">
                        <input type="text" id="editAccountID" placeholder="C.I. / RUC" value="1712345678" class="p-2.5 border rounded-lg">
                        <input type="text" id="editAccountNum" placeholder="N° Cuenta" value="2200112233" class="p-2.5 border rounded-lg col-span-2">
                    </div>
                </div>

                <!-- Lista de Invitados -->
                <div>
                    <h3 class="font-semibold text-goldDark uppercase mb-2">Lista Predeterminada (Nombre|Pases)</h3>
                    <p class="text-[10px] text-gray-500 mb-2">Formato por línea: <i>Nombre|Pases</i></p>
                    <textarea id="editGuestListText" rows="5" class="w-full p-3 border rounded-xl font-mono">Familia Pérez Palacios|4
Lucía Portilla & Ricardo Palacios|2
Grace Andrade & Santiago González|2
Tía Carmen|1
Amigos Novios|2</textarea>
                </div>

                <button id="saveConfigBtn" class="w-full py-3.5 bg-gold text-white font-semibold rounded-xl hover:bg-goldDark transition-all uppercase tracking-wider">
                    Aplicar Cambios
                </button>
            </div>
        </div>
    </div>

    <!-- SCRIPT PRINCIPAL -->
    <script>
        // Data inicial de invitados predeterminados
        let guestList = [
            { name: "Familia Pérez Palacios", passes: 4 },
            { name: "Lucía Portilla & Ricardo Palacios", passes: 2 },
            { name: "Grace Andrade & Santiago González", passes: 2 },
            { name: "Tía Carmen", passes: 1 },
            { name: "Amigos de los Novios", passes: 2 }
        ];

        let currentGuest = { name: "Invitado de Honor", passes: 1 };

        const guestDropdown = document.getElementById('guestDropdown');
        const customGuestName = document.getElementById('customGuestName');
        const customGuestPasses = document.getElementById('customGuestPasses');
        const enterInvitationBtn = document.getElementById('enterInvitationBtn');
        const guestGateModal = document.getElementById('guestGateModal');
        const mainContent = document.getElementById('mainContent');

        function populateGuestDropdown() {
            guestDropdown.innerHTML = '<option value="">-- Selecciona de la lista --</option>';
            guestList.forEach((g, idx) => {
                const opt = document.createElement('option');
                opt.value = idx;
                opt.textContent = `${g.name} (${g.passes} ${g.passes > 1 ? 'Pases' : 'Pase'})`;
                guestDropdown.appendChild(opt);
            });
        }
        populateGuestDropdown();

        enterInvitationBtn.addEventListener('click', () => {
            const selectedIdx = guestDropdown.value;
            const customNameVal = customGuestName.value.trim();
            const customPassesVal = customGuestPasses.value;

            if (customNameVal !== "") {
                currentGuest.name = customNameVal;
                currentGuest.passes = customPassesVal ? parseInt(customPassesVal) : 1;
            } else if (selectedIdx !== "") {
                currentGuest.name = guestList[selectedIdx].name;
                currentGuest.passes = guestList[selectedIdx].passes;
            } else {
                currentGuest.name = "Estimado(a) Invitado(a)";
                currentGuest.passes = 1;
            }

            document.getElementById('displayGuestName').textContent = currentGuest.name;
            document.getElementById('displayGuestPasses').textContent = `${currentGuest.passes} ${currentGuest.passes > 1 ? 'Pases Asignados' : 'Pase Asignado'}`;
            document.getElementById('rsvpName').value = currentGuest.name;
            document.getElementById('rsvpPasses').value = currentGuest.passes;

            guestGateModal.classList.add('hidden');
            mainContent.classList.remove('hidden');

            startAmbientMusic();
        });

        // Cuenta regresiva exacta al 06 de Marzo de 2027 a las 16:00 hs
        const targetDate = new Date("March 6, 2027 16:00:00").getTime();

        function updateCountdown() {
            const now = new Date().getTime();
            const difference = targetDate - now;

            if (difference > 0) {
                const days = Math.floor(difference / (1000 * 60 * 60 * 24));
                const hours = Math.floor((difference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                const minutes = Math.floor((difference % (1000 * 60 * 60)) / (1000 * 60));
                const seconds = Math.floor((difference % (1000 * 60)) / 1000);

                document.getElementById('cd-days').textContent = String(days).padStart(2, '0');
                document.getElementById('cd-hours').textContent = String(hours).padStart(2, '0');
                document.getElementById('cd-minutes').textContent = String(minutes).padStart(2, '0');
                document.getElementById('cd-seconds').textContent = String(seconds).padStart(2, '0');
            }
        }
        setInterval(updateCountdown, 1000);
        updateCountdown();

        // Música ambiental con Web Audio API sintetizada
        let audioCtx = null;
        let isPlaying = false;
        let noteInterval = null;

        function startAmbientMusic() {
            if (isPlaying) return;
            try {
                const AudioContext = window.AudioContext || window.webkitAudioContext;
                audioCtx = new AudioContext();
                const notes = [261.63, 329.63, 392.00, 523.25, 659.25]; // Melodía C Major romántica
                let noteIdx = 0;

                isPlaying = true;
                document.getElementById('musicIcon').classList.replace('fa-music', 'fa-pause');

                noteInterval = setInterval(() => {
                    if (!isPlaying || !audioCtx) return;
                    const osc = audioCtx.createOscillator();
                    const gain = audioCtx.createGain();

                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(notes[noteIdx], audioCtx.currentTime);

                    gain.gain.setValueAtTime(0.001, audioCtx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.03, audioCtx.currentTime + 0.3);
                    gain.gain.exponentialRampToValueAtTime(0.0001, audioCtx.currentTime + 2.2);

                    osc.connect(gain);
                    gain.connect(audioCtx.destination);

                    osc.start();
                    osc.stop(audioCtx.currentTime + 2.3);

                    noteIdx = (noteIdx + 1) % notes.length;
                }, 900);
            } catch(e) {}
        }

        function stopAmbientMusic() {
            if (noteInterval) clearInterval(noteInterval);
            if (audioCtx) { audioCtx.close(); audioCtx = null; }
            isPlaying = false;
            document.getElementById('musicIcon').classList.replace('fa-pause', 'fa-music');
        }

        document.getElementById('musicToggle').addEventListener('click', () => {
            if (isPlaying) stopAmbientMusic(); else startAmbientMusic();
        });

        // Copiar número de cuenta
        document.getElementById('copyAccountBtn').addEventListener('click', () => {
            const accNum = document.getElementById('accountNumDisplay').textContent;
            const dummy = document.createElement("textarea");
            document.body.appendChild(dummy);
            dummy.value = accNum;
            dummy.select();
            document.execCommand("copy");
            document.body.removeChild(dummy);

            const alertEl = document.getElementById('copyAlert');
            alertEl.classList.remove('hidden');
            setTimeout(() => alertEl.classList.add('hidden'), 3000);
        });

        // RSVP WhatsApp
        document.getElementById('rsvpForm').addEventListener('submit', (e) => {
            e.preventDefault();
            const name = document.getElementById('rsvpName').value;
            const attendance = document.getElementById('rsvpAttendance').value;
            const passes = document.getElementById('rsvpPasses').value;
            const dietary = document.getElementById('rsvpDietary').value.trim();
            const phone = "593988229273"; // Número de WhatsApp configurado (0988229273 Ecuador)

            let message = `¡Hola Camila y Daniel! ✨%0A%0AConfirmación de Asistencia:%0A👤 *Nombre:* ${encodeURIComponent(name)}%0A✅ *Respuesta:* ${encodeURIComponent(attendance)}%0A🎟️ *Pases Confirmados:* ${passes}`;
            
            if (dietary !== "") {
                message += `%0A🥗 *Restricciones alimentarias:* ${encodeURIComponent(dietary)}`;
            }
            
            message += `%0A%0A¡Nos vemos en la boda!`;

            window.open(`https://wa.me/${phone}?text=${message}`, '_blank');
        });

        // Modal Configurador
        const configModal = document.getElementById('configModal');
        document.getElementById('openConfigBtn').addEventListener('click', () => configModal.classList.remove('hidden'));
        document.getElementById('closeConfigBtn').addEventListener('click', () => configModal.classList.add('hidden'));

        document.getElementById('saveConfigBtn').addEventListener('click', () => {
            document.getElementById('bankNameDisplay').textContent = document.getElementById('editBankName').value;
            document.getElementById('accountTypeDisplay').textContent = document.getElementById('editAccountType').value;
            document.getElementById('accountHolderDisplay').textContent = document.getElementById('editAccountHolder').value;
            document.getElementById('accountIDDisplay').textContent = document.getElementById('editAccountID').value;
            document.getElementById('accountNumDisplay').textContent = document.getElementById('editAccountNum').value;

            const lines = document.getElementById('editGuestListText').value.split('\n');
            const newGuestList = [];
            lines.forEach(line => {
                if(line.includes('|')) {
                    const [gName, gPasses] = line.split('|');
                    newGuestList.push({ name: gName.trim(), passes: parseInt(gPasses.trim()) || 1 });
                }
            });

            if(newGuestList.length > 0) {
                guestList = newGuestList;
                populateGuestDropdown();
            }

            configModal.classList.add('hidden');
        });
    </script>
</body>
</html>