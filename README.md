<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AnesisWorld - Выкуй Свою Легенду</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel+Decorative:wght@700;900&family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg: #050505;
            --surface: #111111;
            --text: #c7c7c7;
            --text-muted: #7a7a7a;
            --primary: #d4af37; /* Gold */
            --primary-glow: rgba(212, 175, 55, 0.2);
            --primary-glow-strong: rgba(212, 175, 55, 0.4);
            --border-color: rgba(212, 175, 55, 0.2);
        }
        html { scroll-behavior: smooth; }
        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg);
            color: var(--text);
            overflow-x: hidden;
        }
        .font-cinzel { font-family: 'Cinzel Decorative', cursive; }

        .hero-bg {
            background-image: linear-gradient(to top, var(--bg) 10%, transparent 50%), url('https://placehold.co/1920x1080/050505/d4af37?text=AnesisWorld+Citadel');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
        }

        .btn-primary {
            @apply inline-block px-10 py-5 font-bold uppercase tracking-widest rounded-sm shadow-lg transition-all duration-300 relative overflow-hidden;
            background: var(--primary);
            color: #050505;
            border: 1px solid var(--primary);
        }
        .btn-primary:before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(120deg, transparent, rgba(255, 255, 255, 0.4), transparent);
            transition: all 650ms;
        }
        .btn-primary:hover:before { left: 100%; }
        .btn-primary:hover { transform: translateY(-4px); box-shadow: 0 10px 20px var(--primary-glow); }

        .section-title {
            @apply text-5xl md:text-7xl font-cinzel font-bold text-center mb-4;
            color: var(--primary);
            text-shadow: 0 0 25px var(--primary-glow);
        }
        .section-subtitle { @apply text-lg text-center mb-16 max-w-3xl mx-auto; color: var(--text-muted); }

        #particles-js { position: absolute; width: 100%; height: 100%; top: 0; left: 0; z-index: 0; }

        .reveal { opacity: 0; transform: translateY(60px); filter: blur(5px); transition: opacity 1s, transform 1s, filter 1s; transition-timing-function: cubic-bezier(0.16, 1, 0.3, 1); }
        .reveal.visible { opacity: 1; transform: translateY(0); filter: blur(0); }

        /* Sidebar */
        #sidebar {
            background: rgba(5, 5, 5, 0.7);
            backdrop-filter: blur(15px);
            border-right: 1px solid var(--border-color);
            transition: width 0.5s cubic-bezier(0.16, 1, 0.3, 1);
            width: 14rem;
        }
        #sidebar.collapsed { width: 4.5rem; }
        .sidebar-link { 
            @apply flex items-center p-3 rounded-lg overflow-hidden; 
            transition: background-color 0.3s; 
        }
        #sidebar.collapsed .sidebar-link { @apply justify-center; }
        .sidebar-link svg { 
            @apply flex-shrink-0 w-8 h-8;
            transition: transform 0.3s, color 0.3s; 
        }
        .sidebar-link:hover {
             background-color: var(--primary-glow);
        }
        .sidebar-link:hover svg {
            color: var(--primary);
        }
        .link-text { 
            @apply font-semibold ml-4;
            transition: opacity 0.2s, transform 0.3s, width 0.5s, margin-left 0.5s; 
            white-space: nowrap;
        }
        #sidebar.collapsed .link-text { 
            opacity: 0; 
            transform: translateX(-100%); 
            pointer-events: none; 
            width: 0;
            margin-left: 0;
        }
        
        main { transition: margin-left 0.5s cubic-bezier(0.16, 1, 0.3, 1); margin-left: 14rem; }
        #sidebar.collapsed ~ main { margin-left: 4.5rem; }
        @media (max-width: 768px) {
            main { margin-left: 4.5rem; }
            #sidebar { width: 4.5rem; }
            .link-text { display: none; }
            #sidebar .sidebar-link { justify-center; }
        }

        /* Custom Section Styles & Parallax */
        .parallax-section {
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
        }
        .lore-section {
            background-image: linear-gradient(rgba(5,5,5,0.95), rgba(5,5,5,0.95)), url('https://placehold.co/1920x1080/0a0a0a/333?text=Ancient+Scroll');
        }
        .rule-card {
            @apply flex items-start gap-6 p-6 border border-transparent rounded-lg;
            background: linear-gradient(var(--surface), var(--surface)) padding-box, linear-gradient(145deg, var(--border-color), transparent) border-box;
            transition: transform 0.3s, box-shadow 0.3s;
        }
        .rule-card:hover { 
            transform: translateY(-5px);
            box-shadow: 0 0 20px rgba(212, 175, 55, 0.1);
        }
        .rule-number {
            @apply text-6xl font-cinzel font-bold;
            color: var(--text-muted);
            line-height: 1;
        }
        
        /* IP Copy Toast */
        #toast {
            @apply fixed bottom-5 right-5 bg-green-600 text-white py-3 px-6 rounded-lg shadow-2xl;
            transform: translateY(200%);
            transition: transform 0.5s cubic-bezier(0.16, 1, 0.3, 1);
            z-index: 10000;
        }
        #toast.show {
            transform: translateY(0);
        }
    </style>
</head>
<body class="antialiased">

    <!-- Sidebar -->
    <aside id="sidebar" class="fixed top-0 left-0 h-screen z-50 flex flex-col">
        <div class="text-center py-8 flex-shrink-0">
            <a href="#home" class="text-4xl font-cinzel font-bold text-white z-10">A</a>
        </div>
        <nav class="flex-grow flex flex-col space-y-4 w-full px-3">
            <a href="#features" title="Особенности" class="sidebar-link text-stone-400 hover:text-white">
                <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 3v4M3 5h4M6 17v4m-2-2h4m5-16v4m-2-2h4m5 11v4m-2-2h4M9 21h6a2 2 0 002-2V5a2 2 0 00-2-2H9a2 2 0 00-2 2v14a2 2 0 002 2z"></path></svg>
                <span class="link-text">Особенности</span>
            </a>
            <a href="#lore" title="Лор Мира" class="sidebar-link text-stone-400 hover:text-white">
                <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6.253v11.494m-9-5.747h18M5.47 5.47l13.06 13.06M5.47 18.53l13.06-13.06"/></svg>
                <span class="link-text">Лор Мира</span>
            </a>
            <a href="#rules" title="Правила" class="sidebar-link text-stone-400 hover:text-white">
                 <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"></path></svg>
                <span class="link-text">Правила</span>
            </a>
            <a href="#gallery" title="Галерея" class="sidebar-link text-stone-400 hover:text-white">
                <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l-1.586-1.586a2 2 0 00-2.828 0L6 14m6-6l.01.01"></path></svg>
                <span class="link-text">Галерея</span>
            </a>
             <a href="#team" title="Команда" class="sidebar-link text-stone-400 hover:text-white">
                <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z"></path></svg>
                <span class="link-text">Команда</span>
            </a>
        </nav>
        <div class="p-3 mt-auto flex-shrink-0">
            <button id="sidebar-toggle" class="w-full text-stone-400 hover:text-white p-3 flex items-center justify-center">
                <svg id="sidebar-icon-open" class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path></svg>
                <svg id="sidebar-icon-close" class="w-8 h-8 hidden" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
            </button>
        </div>
    </aside>

    <main>
        <!-- Header & Hero Section -->
        <header id="home" class="hero-bg min-h-screen flex flex-col relative">
            <div id="particles-js"></div>
            <div class="flex-grow flex items-center justify-center text-center text-white z-10">
                <div class="container mx-auto px-6">
                    <h1 class="text-6xl md:text-8xl lg:text-9xl font-cinzel font-black uppercase tracking-wider leading-tight reveal">
                        Anesis<span style="color: var(--primary);">World</span>
                    </h1>
                    <p class="text-lg md:text-2xl text-stone-300 mt-6 mb-12 max-w-4xl mx-auto backdrop-blur-sm bg-black/20 p-2 rounded-md reveal" style="transition-delay: 200ms;">
                        Выкуй свою легенду в мире, где каждый выбор имеет значение. Здесь рождаются герои и рушатся империи.
                    </p>
                    <div class="reveal" style="transition-delay: 400ms;">
                        <button id="copy-ip-btn" class="btn-primary text-xl">Начать Приключение</button>
                    </div>
                </div>
            </div>
        </header>

        <!-- Main Content Sections -->
        <section id="features" class="py-24 md:py-40">
            <div class="container mx-auto px-6">
                <h2 class="section-title reveal">Почему Anesis?</h2>
                <p class="section-subtitle reveal">Мы переосмыслили выживание. Это не просто игра, это сага. Ваша сага.</p>
                <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-8">
                    <div class="text-center p-4 reveal">
                        <div class="text-primary mb-4 flex justify-center"><svg class="w-20 h-20" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z"></path><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z"></path></svg></div>
                        <h3 class="text-xl font-bold mb-2">Эпические Сюжеты</h3>
                        <p class="text-text-muted">Глубокие квестовые линии, которые влияют на мир и его историю.</p>
                    </div>
                    <div class="text-center p-4 reveal" style="transition-delay: 150ms;">
                        <div class="text-primary mb-4 flex justify-center"><svg class="w-20 h-20" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M21 16V8a2 2 0 0 0-1-1.73l-7-4a2 2 0 0 0-2 0l-7 4A2 2 0 0 0 3 8v8a2 2 0 0 0 1 1.73l7 4a2 2 0 0 0 2 0l7-4A2 2 0 0 0 21 16z"></path><polyline points="3.27 6.96 12 12.01 20.73 6.96"></polyline><line x1="12" y1="22.08" x2="12" y2="12"></line></svg></div>
                        <h3 class="text-xl font-bold mb-2">Живой Мир</h3>
                        <p class="text-text-muted">Авторская карта с уникальными биомами и скрытыми подземельями.</p>
                    </div>
                    <div class="text-center p-4 reveal" style="transition-delay: 300ms;">
                        <div class="text-primary mb-4 flex justify-center"><svg class="w-20 h-20" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"></path></svg></div>
                        <h3 class="text-xl font-bold mb-2">Система Кланов</h3>
                        <p class="text-text-muted">Создавайте могущественные кланы, стройте цитадели и доминируйте.</p>
                    </div>
                    <div class="text-center p-4 reveal" style="transition-delay: 450ms;">
                        <div class="text-primary mb-4 flex justify-center"><svg class="w-20 h-20" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M13 10V3L4 14h7v7l9-11h-7z"></path></svg></div>
                        <h3 class="text-xl font-bold mb-2">Динамичные События</h3>
                        <p class="text-text-muted">Мировые боссы, осады замков и ивенты от администрации.</p>
                    </div>
                </div>
            </div>
        </section>
        
        <section id="lore" class="parallax-section lore-section py-24 md:py-40">
             <div class="container mx-auto px-6 max-w-4xl text-center">
                <h2 class="section-title reveal">Летописи Древних</h2>
                <p class="section-subtitle reveal text-lg leading-relaxed">Давным-давно, когда мир был молод, титаны выковали континент из хаоса и магии. Они оставили после себя артефакты невообразимой силы и загадки, которые до сих пор ждут своих героев. Но с уходом титанов пробудилось древнее зло... Ваша история начинается здесь, на руинах былого величия, и только вам решать, возродите ли вы славу прошлого или погрузите мир в вечную тьму.</p>
            </div>
        </section>

        <section id="rules" class="py-24 md:py-40">
             <div class="container mx-auto px-6">
                <h2 class="section-title reveal">Законы Мира</h2>
                <p class="section-subtitle reveal">Свод правил, который обеспечивает порядок и честную игру для всех жителей.</p>
                <div class="max-w-4xl mx-auto space-y-12">
                    <div class="rule-card reveal">
                        <span class="rule-number">01</span>
                        <div>
                            <h3 class="font-bold text-xl text-primary mb-2">Уважайте друг друга</h3>
                            <p class="text-text-muted">AnesisWorld — это сообщество. Мы не терпим оскорблений, травли, расизма или любой другой формы унижения игроков. Ваша свобода заканчивается там, где начинается свобода другого. Будьте достойными героями, а не мелкими злодеями.</p>
                        </div>
                    </div>
                    <div class="rule-card reveal" style="transition-delay: 100ms;">
                        <span class="rule-number">02</span>
                        <div>
                            <h3 class="font-bold text-xl text-primary mb-2">Честная игра — закон</h3>
                            <p class="text-text-muted">Использование читов, дюпов, эксплойтов и любых сторонних программ, дающих нечестное преимущество, карается немедленной и вечной блокировкой. Мы ценим мастерство и упорство, а не желание обмануть систему.</p>
                        </div>
                    </div>
                    <div class="rule-card reveal" style="transition-delay: 200ms;">
                        <span class="rule-number">03</span>
                        <div>
                            <h3 class="font-bold text-xl text-primary mb-2">Право на собственность</h3>
                            <p class="text-text-muted">Гриферство и воровство в пределах заприваченных территорий запрещены. Ваш дом — ваша крепость. Однако помните, что земли за пределами ваших владений — дикие и опасные. Рискуйте с умом.</p>
                        </div>
                    </div>
                    <div class="rule-card reveal" style="transition-delay: 300ms;">
                        <span class="rule-number">04</span>
                        <div>
                            <h3 class="font-bold text-xl text-primary mb-2">Не вредите миру</h3>
                            <p class="text-text-muted">Запрещено намеренное создание лагов, строительство бессмысленных и уродливых конструкций, которые портят пейзаж, и любые действия, направленные на нарушение стабильной работы сервера. Этот мир — наш общий дом.</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="gallery" class="py-24 md:py-40 bg-surface">
             <div class="container mx-auto px-6">
                <h2 class="section-title reveal">Хроники Anesis</h2>
                <p class="section-subtitle reveal">Каждый кадр — это часть истории. Вашей истории.</p>
                <div class="grid grid-cols-2 md:grid-cols-3 gap-4">
                    <img src="https://placehold.co/600x400/111111/d4af37?text=Скриншот+1" class="reveal rounded-lg transition-transform duration-300 hover:scale-105" alt="Gallery Image 1">
                    <img src="https://placehold.co/600x800/111111/d4af37?text=Скриншот+2" class="reveal md:row-span-2 rounded-lg transition-transform duration-300 hover:scale-105" alt="Gallery Image 2" style="transition-delay: 100ms;">
                    <img src="https://placehold.co/600x400/111111/d4af37?text=Скриншот+3" class="reveal rounded-lg transition-transform duration-300 hover:scale-105" alt="Gallery Image 3" style="transition-delay: 200ms;">
                    <img src="https://placehold.co/600x400/111111/d4af37?text=Скриншот+4" class="reveal rounded-lg transition-transform duration-300 hover:scale-105" alt="Gallery Image 4" style="transition-delay: 300ms;">
                    <img src="https://placehold.co/600x400/111111/d4af37?text=Скриншот+5" class="reveal rounded-lg transition-transform duration-300 hover:scale-105" alt="Gallery Image 5" style="transition-delay: 400ms;">
                </div>
            </div>
        </section>

        <section id="team" class="py-24 md:py-40">
            <div class="container mx-auto px-6">
                <h2 class="section-title reveal">Хранители Мира</h2>
                <p class="section-subtitle reveal">Люди, которые вкладывают душу в AnesisWorld.</p>
                <div class="flex flex-wrap justify-center gap-12">
                    <div class="text-center reveal">
                        <img class="w-32 h-32 mx-auto rounded-full shadow-lg border-4 border-border-color" src="https://placehold.co/128x128/111111/d4af37?text=A" alt="Admin">
                        <h4 class="mt-4 text-xl font-bold">AdminName</h4>
                        <p style="color: var(--primary);">Основатель</p>
                    </div>
                    <div class="text-center reveal" style="transition-delay: 150ms;">
                        <img class="w-32 h-32 mx-auto rounded-full shadow-lg border-4 border-border-color" src="https://placehold.co/128x128/111111/d4af37?text=D" alt="Developer">
                        <h4 class="mt-4 text-xl font-bold">DeveloperName</h4>
                        <p style="color: var(--primary);">Разработчик</p>
                    </div>
                    <div class="text-center reveal" style="transition-delay: 300ms;">
                        <img class="w-32 h-32 mx-auto rounded-full shadow-lg border-4 border-border-color" src="https://placehold.co/128x128/111111/d4af37?text=M" alt="Moderator">
                        <h4 class="mt-4 text-xl font-bold">ModeratorName</h4>
                        <p style="color: var(--primary);">Главный модератор</p>
                    </div>
                </div>
            </div>
        </section>

        <section id="discord" class="parallax-section lore-section py-24 md:py-40">
            <div class="container mx-auto px-6 text-center">
                <h2 class="section-title reveal">Вступите в Наши Ряды</h2>
                <p class="section-subtitle reveal">Общайтесь, ищите соратников и будьте в курсе всех событий. Наш Discord — это сердце сообщества.</p>
                <div class="reveal">
                    <a href="#" class="btn-primary text-xl">Присоединиться к Discord</a>
                </div>
            </div>
        </section>
    </main>

    <!-- Footer -->
    <footer class="py-8 border-t border-stone-900">
        <div class="container mx-auto px-6 text-center text-stone-500">
            <p>&copy; 2025 AnesisWorld. Все права защищены.</p>
            <p class="text-sm mt-2">AnesisWorld не является официальным продуктом Minecraft и не связан с Mojang AB.</p>
        </div>
    </footer>
    
    <div id="toast">IP скопирован!</div>

    <script src="https://cdn.jsdelivr.net/particles.js/2.0.0/particles.min.js"></script>
    <script>
        // --- Sidebar Toggle ---
        const sidebar = document.getElementById('sidebar');
        const sidebarToggle = document.getElementById('sidebar-toggle');
        const iconOpen = document.getElementById('sidebar-icon-open');
        const iconClose = document.getElementById('sidebar-icon-close');
        sidebarToggle.addEventListener('click', () => {
            sidebar.classList.toggle('collapsed');
            iconOpen.classList.toggle('hidden');
            iconClose.classList.toggle('hidden');
        });

        // --- Particles.js ---
        particlesJS("particles-js", {
            particles: {
                number: { value: 40, density: { enable: true, value_area: 800 } },
                color: { value: "#d4af37" },
                shape: { type: "circle" },
                opacity: { value: 0.4, random: true, anim: { enable: true, speed: 0.8, opacity_min: 0.1, sync: false } },
                size: { value: 2.5, random: true },
                move: { enable: true, speed: 1, direction: "none", random: true, straight: false, out_mode: "out" }
            },
            interactivity: { detect_on: "canvas", events: { onhover: { enable: true, mode: "repulse" }, onclick: { enable: false } }, retina_detect: true }
        });

        // --- Scroll Reveal Animation ---
        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('visible');
                }
            });
        }, { threshold: 0.15 });
        document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
        
        // --- IP Copy Toast Notification ---
        const copyIpBtn = document.getElementById('copy-ip-btn');
        const toast = document.getElementById('toast');
        copyIpBtn.addEventListener('click', () => {
            navigator.clipboard.writeText('play.anesis.world').then(() => {
                toast.classList.add('show');
                setTimeout(() => {
                    toast.classList.remove('show');
                }, 3000);
            });
        });

    </script>

</body>
</html>
