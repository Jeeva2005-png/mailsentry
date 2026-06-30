# mailsentry

<!DOCTYPE html>
<html lang="en" class="scroll-smooth">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Grandeur Hospitality — Hotel Management</title>
<script src="https://cdn.tailwindcss.com"></script>
<script src="https://code.iconify.design/3/3.1.0/iconify.min.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&family=Playfair+Display:ital,wght@0,400;0,500;0,600;1,400&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
tailwind.config = {
  theme: {
    extend: {
      colors: {
        lux: {
          black: '#050505',
          charcoal: '#121212',
          panel: '#1A1A1A',
          gold: '#C5A059',
          golddim: '#8a7044',
          border: '#2A2A2A'
        }
      },
      fontFamily: {
        sans: ['Inter', 'sans-serif'],
        serif: ['Playfair Display', 'serif'],
      },
    }
  }
}
</script>
<style>
  ::-webkit-scrollbar { width: 6px; }
  ::-webkit-scrollbar-track { background: #050505; }
  ::-webkit-scrollbar-thumb { background: #2A2A2A; border-radius: 3px; }
  ::-webkit-scrollbar-thumb:hover { background: #C5A059; }

  @keyframes fadeInUp {
    from { opacity: 0; transform: translateY(30px); }
    to { opacity: 1; transform: translateY(0); }
  }
  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }
  @keyframes countUp {
    from { opacity: 0; transform: scale(0.8); }
    to { opacity: 1; transform: scale(1); }
  }
  @keyframes shimmer {
    0% { background-position: -200% center; }
    100% { background-position: 200% center; }
  }
  .animate-fade-in-up { animation: fadeInUp 0.8s ease-out forwards; }
  .animate-fade-in { animation: fadeIn 1s ease-out forwards; }
  .delay-100 { animation-delay: 0.1s; }
  .delay-200 { animation-delay: 0.2s; }
  .delay-300 { animation-delay: 0.3s; }
  .delay-400 { animation-delay: 0.4s; }
  .delay-500 { animation-delay: 0.5s; }
  .delay-600 { animation-delay: 0.6s; }

  .gold-shimmer {
    background: linear-gradient(90deg, #C5A059, #e8d5a3, #C5A059);
    background-size: 200% auto;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    animation: shimmer 4s linear infinite;
  }

  .classic-divider {
    display: flex;
    align-items: center;
    gap: 16px;
  }
  .classic-divider::before,
  .classic-divider::after {
    content: '';
    flex: 1;
    height: 1px;
    background: linear-gradient(to right, transparent, #C5A059, transparent);
  }

  #hero-3d-canvas {
    position: absolute;
    top: 0; left: 0;
    width: 100%; height: 100%;
    z-index: 0;
  }

  .service-card {
    position: relative;
    overflow: hidden;
  }
  .service-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0;
    width: 100%; height: 2px;
    background: linear-gradient(to right, transparent, #C5A059, transparent);
    opacity: 0;
    transition: opacity 0.5s;
  }
  .service-card:hover::before { opacity: 1; }

  .property-card img {
    transition: transform 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94), filter 0.8s;
  }
  .property-card:hover img {
    transform: scale(1.08);
    filter: brightness(0.7);
  }

  .nav-scrolled {
    background: rgba(5, 5, 5, 0.92) !important;
    backdrop-filter: blur(16px) !important;
    border-bottom: 1px solid rgba(197,160,89,0.15) !important;
  }

  .toast {
    position: fixed;
    bottom: 32px;
    right: 32px;
    background: #1A1A1A;
    border: 1px solid #C5A059;
    color: #fff;
    padding: 16px 24px;
    border-radius: 4px;
    z-index: 9999;
    transform: translateY(100px);
    opacity: 0;
    transition: all 0.4s ease;
    font-family: 'Inter', sans-serif;
    font-size: 14px;
  }
  .toast.show {
    transform: translateY(0);
    opacity: 1;
  }
</style>
</head>
<body class="bg-lux-black text-white font-sans antialiased">

<!-- TOAST -->
<div id="toast" class="toast"></div>

<!-- NAVIGATION -->
<nav id="navbar" class="fixed top-0 left-0 right-0 z-50 h-20 flex items-center px-6 transition-all duration-300" style="background:rgba(5,5,5,0.5); backdrop-filter:blur(8px); border-bottom:1px solid transparent;">
  <div class="max-w-7xl mx-auto w-full flex items-center justify-between">
    <!-- Logo -->
    <a href="#" class="flex items-center gap-3 group">
      <div class="w-9 h-9 border border-lux-gold/50 flex items-center justify-center group-hover:border-lux-gold transition-colors duration-300">
        <span class="font-serif text-lux-gold text-lg leading-none">G</span>
      </div>
      <div class="flex flex-col">
        <span class="font-serif text-base tracking-tight text-white">Grandeur</span>
        <span class="text-[9px] uppercase tracking-[0.2em] text-neutral-500">Hospitality Group</span>
      </div>
    </a>
    <!-- Desktop Links -->
    <div class="hidden md:flex items-center gap-8">
      <a href="#about" class="text-sm font-medium tracking-wide uppercase text-neutral-400 hover:text-white transition-colors duration-150">About</a>
      <a href="#services" class="text-sm font-medium tracking-wide uppercase text-neutral-400 hover:text-white transition-colors duration-150">Services</a>
      <a href="#portfolio" class="text-sm font-medium tracking-wide uppercase text-neutral-400 hover:text-white transition-colors duration-150">Portfolio</a>
      <a href="#testimonials" class="text-sm font-medium tracking-wide uppercase text-neutral-400 hover:text-white transition-colors duration-150">Testimonials</a>
      <a href="#contact" class="ml-4 px-5 py-2.5 bg-white text-lux-black text-xs font-semibold uppercase tracking-wider hover:bg-lux-gold hover:text-lux-black transition-all duration-150">Contact Us</a>
    </div>
    <!-- Mobile Menu Button -->
    <button id="mobile-menu-btn" class="md:hidden text-white">
      <span class="iconify" data-icon="lucide:menu" data-width="24"></span>
    </button>
  </div>
</nav>

<!-- MOBILE MENU -->
<div id="mobile-menu" class="fixed inset-0 z-40 bg-lux-black/95 backdrop-blur-lg flex flex-col items-center justify-center gap-8 transition-all duration-300 opacity-0 pointer-events-none">
  <a href="#about" class="mobile-link text-2xl font-serif text-neutral-300 hover:text-lux-gold transition-colors">About</a>
  <a href="#services" class="mobile-link text-2xl font-serif text-neutral-300 hover:text-lux-gold transition-colors">Services</a>
  <a href="#portfolio" class="mobile-link text-2xl font-serif text-neutral-300 hover:text-lux-gold transition-colors">Portfolio</a>
  <a href="#testimonials" class="mobile-link text-2xl font-serif text-neutral-300 hover:text-lux-gold transition-colors">Testimonials</a>
  <a href="#contact" class="mobile-link mt-4 px-8 py-3 bg-lux-gold text-lux-black text-sm font-semibold uppercase tracking-wider">Contact Us</a>
  <button id="mobile-close-btn" class="absolute top-6 right-6 text-white">
    <span class="iconify" data-icon="lucide:x" data-width="28"></span>
  </button>
</div>

<!-- HERO SECTION -->
<section class="relative h-screen flex items-center justify-center overflow-hidden">
  <canvas id="hero-3d-canvas"></canvas>
  <!-- Overlay -->
  <div class="absolute inset-0 bg-gradient-to-t from-lux-black via-lux-black/40 to-transparent z-[1]"></div>
  <div class="absolute inset-0 bg-gradient-to-r from-lux-black/60 to-transparent z-[1]"></div>

  <div class="relative z-10 max-w-4xl mx-auto px-6 text-center">
    <div class="opacity-0 animate-fade-in-up">
      <p class="text-[10px] uppercase tracking-[0.3em] text-lux-gold mb-6">Established 2008 · Worldwide Operations</p>
    </div>
    <h1 class="font-serif text-5xl md:text-7xl lg:text-8xl font-normal leading-tight tracking-tight opacity-0 animate-fade-in-up delay-100">
      The Art of<br>
      <span class="gold-shimmer">Hotel Excellence</span>
    </h1>
    <p class="mt-8 text-sm md:text-base font-light leading-relaxed text-neutral-400 max-w-2xl mx-auto opacity-0 animate-fade-in-up delay-300">
      We transform properties into legendary destinations. From boutique retreats to grand resorts, 
      our management philosophy blends timeless elegance with modern precision.
    </p>
    <div class="mt-10 flex flex-col sm:flex-row items-center justify-center gap-4 opacity-0 animate-fade-in-up delay-400">
      <a href="#services" class="px-8 py-4 bg-white text-lux-black text-xs font-semibold uppercase tracking-wider hover:bg-lux-gold transition-all duration-150">
        Explore Services
      </a>
      <a href="#portfolio" class="px-8 py-4 border border-white/20 text-white text-xs font-semibold uppercase tracking-wider hover:border-lux-gold hover:text-lux-gold transition-all duration-300 backdrop-blur-sm">
        View Portfolio
      </a>
    </div>
    <p class="mt-12 text-[10px] uppercase tracking-[0.2em] text-neutral-600 opacity-0 animate-fade-in delay-600">
      Drag to rotate the model
    </p>
  </div>

  <!-- Scroll indicator -->
  <div class="absolute bottom-8 left-1/2 -translate-x-1/2 z-10 flex flex-col items-center gap-2 opacity-0 animate-fade-in delay-600">
    <span class="text-[9px] uppercase tracking-[0.2em] text-neutral-600">Scroll</span>
    <div class="w-px h-8 bg-gradient-to-b from-lux-gold/50 to-transparent"></div>
  </div>
</section>

<!-- ABOUT SECTION -->
<section id="about" class="py-24 px-6">
  <div class="max-w-7xl mx-auto">
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-16 items-center">
      <!-- Image -->
      <div class="relative overflow-hidden group">
        <img src="https://picsum.photos/seed/grandeur-lobby/700/900.jpg" alt="Luxury Hotel Lobby" class="w-full h-[500px] lg:h-[600px] object-cover grayscale opacity-60 group-hover:grayscale-0 group-hover:opacity-80 transition-all duration-700">
        <div class="absolute inset-0 bg-gradient-to-t from-lux-black/80 to-transparent"></div>
        <div class="absolute bottom-8 left-8">
          <p class="text-[10px] uppercase tracking-[0.2em] text-lux-gold mb-2">Since 2008</p>
          <p class="font-serif text-3xl text-white">16 Years of<br>Distinguished Service</p>
        </div>
      </div>
      <!-- Text -->
      <div>
        <p class="text-[10px] uppercase tracking-[0.3em] text-lux-gold mb-4">Our Philosophy</p>
        <h2 class="font-serif text-3xl md:text-4xl tracking-tight text-white mb-8">
          Where Tradition Meets<br><em class="text-lux-gold">Modern Mastery</em>
        </h2>
        <div class="space-y-6 text-sm font-light leading-relaxed text-neutral-400">
          <p>
            Grandeur Hospitality Group was founded on a singular belief: that every hotel has a soul waiting to be revealed. 
            We don't impose cookie-cutter solutions — we listen to the story each property tells and amplify it into an 
            unforgettable guest experience.
          </p>
          <p>
            Our team of seasoned hospitality professionals brings decades of combined experience from the world's 
            most prestigious hotel brands. We blend this heritage with cutting-edge revenue management, 
            digital innovation, and operational excellence.
          </p>
          <p>
            From the moment a guest discovers your property online to the farewell at checkout, every touchpoint 
            is orchestrated with intention, warmth, and an unwavering commitment to perfection.
          </p>
        </div>
        <div class="mt-10 flex items-center gap-8">
          <div>
            <p class="font-serif text-3xl text-lux-gold">47</p>
            <p class="text-[10px] uppercase tracking-[0.2em] text-neutral-500 mt-1">Properties Managed</p>
          </div>
          <div class="w-px h-12 bg-lux-border"></div>
          <div>
            <p class="font-serif text-3xl text-lux-gold">12</p>
            <p class="text-[10px] uppercase tracking-[0.2em] text-neutral-500 mt-1">Countries</p>
          </div>
          <div class="w-px h-12 bg-lux-border"></div>
          <div>
            <p class="font-serif text-3xl text-lux-gold">98%</p>
            <p class="text-[10px] uppercase tracking-[0.2em] text-neutral-500 mt-1">Retention Rate</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- CLASSIC DIVIDER -->
<div class="max-w-7xl mx-auto px-6">
  <div class="classic-divider">
    <span class="iconify text-lux-gold/60" data-icon="lucide:diamond" data-width="12"></span>
  </div>
</div>

<!-- SERVICES SECTION -->
<section id="services" class="py-24 px-6">
  <div class="max-w-7xl mx-auto">
    <div class="text-center mb-16">
      <p class="text-[10px] uppercase tracking-[0.3em] text-lux-gold mb-4">What We Offer</p>
      <h2 class="font-serif text-3xl md:text-4xl tracking-tight text-white">
        Comprehensive Hotel<br><em class="text-lux-gold">Management Services</em>
      </h2>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      <!-- Service 1 -->
      <div class="service-card bg-lux-panel border border-lux-border p-8 hover:border-lux-gold/30 transition-all duration-500 group">
        <div class="w-12 h-12 border border-lux-gold/30 flex items-center justify-center mb-6 group-hover:border-lux-gold transition-colors duration-300">
          <span class="iconify text-lux-gold" data-icon="lucide:building-2" data-width="20"></span>
        </div>
        <h3 class="font-serif text-xl text-white mb-4">Property Management</h3>
        <p class="text-sm font-light leading-relaxed text-neutral-400">
          Full operational oversight from housekeeping to maintenance. We ensure your property runs flawlessly 
          every day, every season, every year.
        </p>
      </div>
      <!-- Service 2 -->
      <div class="service-card bg-lux-panel border border-lux-border p-8 hover:border-lux-gold/30 transition-all duration-500 group">
        <div class="w-12 h-12 border border-lux-gold/30 flex items-center justify-center mb-6 group-hover:border-lux-gold transition-colors duration-300">
          <span class="iconify text-lux-gold" data-icon="lucide:trending-up" data-width="20"></span>
        </div>
        <h3 class="font-serif text-xl text-white mb-4">Revenue Optimization</h3>
        <p class="text-sm font-light leading-relaxed text-neutral-400">
          Data-driven pricing strategies, channel management, and demand forecasting that maximize RevPAR 
          and drive sustainable profit growth.
        </p>
      </div>
      <!-- Service 3 -->
      <div class="service-card bg-lux-panel border border-lux-border p-8 hover:border-lux-gold/30 transition-all duration-500 group">
        <div class="w-12 h-12 border border-lux-gold/30 flex items-center justify-center mb-6 group-hover:border-lux-gold transition-colors duration-300">
          <span class="iconify text-lux-gold" data-icon="lucide:users" data-width="20"></span>
        </div>
        <h3 class="font-serif text-xl text-white mb-4">Staff Training</h3>
        <p class="text-sm font-light leading-relaxed text-neutral-400">
          World-class training programs that transform teams into hospitality artisans. From front desk 
          to culinary, every role is elevated.
        </p>
      </div>
      <!-- Service 4 -->
      <div class="service-card bg-lux-panel border border-lux-border p-8 hover:border-lux-gold/30 transition-all duration-500 group">
        <div class="w-12 h-12 border border-lux-gold/30 flex items-center justify-center mb-6 group-hover:border-lux-gold transition-colors duration-300">
          <span class="iconify text-lux-gold" data-icon="lucide:sparkles" data-width="20"></span>
        </div>
        <h3 class="font-serif text-xl text-white mb-4">Guest Experience</h3>
        <p class="text-sm font-light leading-relaxed text-neutral-400">
          Designing unforgettable journeys from pre-arrival to post-departure. We craft moments that 
          turn guests into lifelong advocates.
        </p>
      </div>
      <!-- Service 5 -->
      <div class="service-card bg-lux-panel border border-lux-border p-8 hover:border-lux-gold/30 transition-all duration-500 group">
        <div class="w-12 h-12 border border-lux-gold/30 flex items-center justify-center mb-6 group-hover:border-lux-gold transition-colors duration-300">
          <span class="iconify text-lux-gold" data-icon="lucide:crown" data-width="20"></span>
        </div>
        <h3 class="font-serif text-xl text-white mb-4">Brand Development</h3>
        <p class="text-sm font-light leading-relaxed text-neutral-400">
          Positioning your property in the luxury marketplace. From visual identity to brand narrative, 
          we build legacies that command premium rates.
        </p>
      </div>
      <!-- Service 6 -->
      <div class="service-card bg-lux-panel border border-lux-border p-8 hover:border-lux-gold/30 transition-all duration-500 group">
        <div class="w-12 h-12 border border-lux-gold/30 flex items-center justify-center mb-6 group-hover:border-lux-gold transition-colors duration-300">
          <span class="iconify text-lux-gold" data-icon="lucide:globe" data-width="20"></span>
        </div>
        <h3 class="font-serif text-xl text-white mb-4">Digital Strategy</h3>
        <p class="text-sm font-light leading-relaxed text-neutral-400">
          OTAs, direct bookings, social media, and reputation management. We build your digital presence 
          into a seamless revenue engine.
        </p>
      </div>
    </div>
  </div>
</section>

<!-- STATS SECTION -->
<section class="py-16 px-6 border-y border-lux-border">
  <div class="max-w-7xl mx-auto grid grid-cols-2 md:grid-cols-4 gap-8 text-center">
    <div>
      <p class="font-serif text-4xl md:text-5xl text-white stat-number" data-target="47">0</p>
      <p class="text-[10px] uppercase tracking-[0.2em] text-neutral-500 mt-2">Managed Properties</p>
    </div>
    <div>
      <p class="font-serif text-4xl md:text-5xl text-white stat-number" data-target="320">0</p>
      <p class="text-[10px] uppercase tracking-[0.2em] text-neutral-500 mt-2">Million Revenue</p>
    </div>
    <div>
      <p class="font-serif text-4xl md:text-5xl text-white stat-number" data-target="96">0</p>
      <p class="text-[10px] uppercase tracking-[0.2em] text-neutral-500 mt-2">% Satisfaction</p>
    </div>
    <div>
      <p class="font-serif text-4xl md:text-5xl text-white stat-number" data-target="16">0</p>
      <p class="text-[10px] uppercase tracking-[0.2em] text-neutral-500 mt-2">Years of Excellence</p>
    </div>
  </div>
</section>

<!-- PORTFOLIO SECTION -->
<section id="portfolio" class="py-24 px-6">
  <div class="max-w-7xl mx-auto">
    <div class="text-center mb-16">
      <p class="text-[10px] uppercase tracking-[0.3em] text-lux-gold mb-4">Our Portfolio</p>
      <h2 class="font-serif text-3xl md:text-4xl tracking-tight text-white">
        Properties We<br><em class="text-lux-gold">Have Elevated</em>
      </h2>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-3 gap-1">
      <!-- Property 1 -->
      <div class="property-card relative overflow-hidden cursor-pointer group">
        <img src="https://picsum.photos/seed/palace-hotel/600/800.jpg" alt="The Palace Heritage" class="w-full h-[400px] md:h-[500px] object-cover grayscale opacity-60">
        <div class="absolute inset-0 bg-gradient-to-t from-lux-black via-transparent to-transparent"></div>
        <div class="absolute bottom-0 left-0 right-0 p-6 translate-y-4 group-hover:translate-y-0 transition-transform duration-500">
          <p class="text-[10px] uppercase tracking-[0.2em] text-lux-gold mb-1">Bali, Indonesia</p>
          <h3 class="font-serif text-2xl text-white">The Palace Heritage</h3>
          <p class="text-xs text-neutral-400 mt-2 opacity-0 group-hover:opacity-100 transition-opacity duration-500">128 rooms · 5-Star Resort · Revenue +42%</p>
        </div>
      </div>
      <!-- Property 2 -->
      <div class="property-card relative overflow-hidden cursor-pointer group">
        <img src="https://picsum.photos/seed/chateau-royal/600/800.jpg" alt="Château Royal" class="w-full h-[400px] md:h-[500px] object-cover grayscale opacity-60">
        <div class="absolute inset-0 bg-gradient-to-t from-lux-black via-transparent to-transparent"></div>
        <div class="absolute bottom-0 left-0 right-0 p-6 translate-y-4 group-hover:translate-y-0 transition-transform duration-500">
          <p class="text-[10px] uppercase tracking-[0.2em] text-lux-gold mb-1">Loire Valley, France</p>
          <h3 class="font-serif text-2xl text-white">Château Royal</h3>
          <p class="text-xs text-neutral-400 mt-2 opacity-0 group-hover:opacity-100 transition-opacity duration-500">36 suites · Boutique Hotel · Revenue +58%</p>
        </div>
      </div>
      <!-- Property 3 -->
      <div class="property-card relative overflow-hidden cursor-pointer group">
        <img src="https://picsum.photos/seed/grand-meridian/600/800.jpg" alt="Grand Meridian" class="w-full h-[400px] md:h-[500px] object-cover grayscale opacity-60">
        <div class="absolute inset-0 bg-gradient-to-t from-lux-black via-transparent to-transparent"></div>
        <div class="absolute bottom-0 left-0 right-0 p-6 translate-y-4 group-hover:translate-y-0 transition-transform duration-500">
          <p class="text-[10px] uppercase tracking-[0.2em] text-lux-gold mb-1">Dubai, UAE</p>
          <h3 class="font-serif text-2xl text-white">Grand Meridian</h3>
          <p class="text-xs text-neutral-400 mt-2 opacity-0 group-hover:opacity-100 transition-opacity duration-500">312 rooms · Luxury Business · Revenue +35%</p>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- TESTIMONIALS SECTION -->
<section id="testimonials" class="py-24 px-6 bg-lux-charcoal">
  <div class="max-w-7xl mx-auto">
    <div class="text-center mb-16">
      <p class="text-[10px] uppercase tracking-[0.3em] text-lux-gold mb-4">Testimonials</p>
      <h2 class="font-serif text-3xl md:text-4xl tracking-tight text-white">
        Words From Our<br><em class="text-lux-gold">Valued Partners</em>
      </h2>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
      <!-- Testimonial 1 -->
      <div class="border border-lux-border p-8 hover:border-lux-gold/30 transition-all duration-500">
        <div class="flex gap-1 mb-6">
          <span class="iconify text-lux-gold" data-icon="lucide:star" data-width="14"></span>
          <span class="iconify text-lux-gold" data-icon="lucide:star" data-width="14"></span>
          <span class="iconify text-lux-gold" data-icon="lucide:star" data-width="14"></span>
          <span class="iconify text-lux-gold" data-icon="lucide:star" data-width="14"></span>
          <span class="iconify text-lux-gold" data-icon="lucide:star" data-width="14"></span>
        </div>
        <p class="text-sm font-light leading-relaxed text-neutral-400 italic mb-6">
          "Grandeur transformed our family property into a world-class destination. Revenue increased by 42% 
          in the first year alone. Their attention to detail is simply unmatched."
        </p>
        <div class="flex items-center gap-3">
          <div class="w-10 h-10 rounded-full bg-lux-panel border border-lux-border flex items-center justify-center">
            <span class="font-serif text-lux-gold text-sm">AR</span>
          </div>
          <div>
            <p class="text-sm text-white">Arun Rathanakorn</p>
            <p class="text-[10px] uppercase tracking-[0.15em] text-neutral-500">Owner, The Palace Heritage</p>
          </div>
        </div>
      </div>
      <!-- Testimonial 2 -->
      <div class="border border-lux-border p-8 hover:border-lux-gold/30 transition-all duration-500">
        <div class="flex gap-1 mb-6">
          <span class="iconify text-lux-gold" data-icon="lucide:star" data-width="14"></span>
          <span class="iconify text-lux-gold" data-icon="lucide:star" data-width="14"></span>
          <span class="iconify text-lux-gold" data-icon="lucide:star" data-width="14"></span>
          <span class="iconify text-lux-gold" data-icon="lucide:star" data-width="14"></span>
          <span class="iconify text-lux-gold" data-icon="lucide:star" data-width="14"></span>
        </div>
        <p class="text-sm font-light leading-relaxed text-neutral-400 italic mb-6">
          "What sets Grandeur apart is their genuine passion. They don't just manage hotels — they breathe life 
          into them. Our guest satisfaction scores have never been higher."
        </p>
        <div class="flex items-center gap-3">
          <div class="w-10 h-10 rounded-full bg-lux-panel border border-lux-border flex items-center justify-center">
            <span class="font-serif text-lux-gold text-sm">CM</span>
          </div>
          <div>
            <p class="text-sm text-white">Claire Montague</p>
            <p class="text-[10px] uppercase tracking-[0.15em] text-neutral-500">Director, Château Royal</p>
          </div>
        </div>
      </div>
      <!-- Testimonial 3 -->
      <div class="border border-lux-border p-8 hover:border-lux-gold/30 transition-all duration-500">
        <div class="flex gap-1 mb-6">
          <span class="iconify text-lux-gold" data-icon="lucide:star" data-width="14"></span>
          <span class="iconify text-lux-gold" data-icon="lucide:star" data-width="14"></span>
          <span class="iconify text-lux-gold" data-icon="lucide:star" data-width="14"></span>
          <span class="iconify text-lux-gold" data-icon="lucide:star" data-width="14"></span>
          <span class="iconify text-lux-gold" data-icon="lucide:star" data-width="14"></span>
        </div>
        <p class="text-sm font-light leading-relaxed text-neutral-400 italic mb-6">
          "In the competitive Dubai market, Grandeur gave us the edge. Their revenue strategy and brand 
          positioning elevated us from a good hotel to an iconic one."
        </p>
        <div class="flex items-center gap-3">
          <div class="w-10 h-10 rounded-full bg-lux-panel border border-lux-border flex items-center justify-center">
            <span class="font-serif text-lux-gold text-sm">FA</span>
          </div>
          <div>
            <p class="text-sm text-white">Faisal Al-Rashid</p>
            <p class="text-[10px] uppercase tracking-[0.15em] text-neutral-500">CEO, Grand Meridian</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- CONTACT CTA SECTION -->
<section id="contact" class="py-24 px-6 relative overflow-hidden">
  <!-- Background glow -->
  <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[600px] h-[600px] bg-lux-gold/5 rounded-full blur-3xl"></div>
  
  <div class="max-w-3xl mx-auto relative z-10">
    <div class="text-center mb-12">
      <p class="text-[10px] uppercase tracking-[0.3em] text-lux-gold mb-4">Begin Your Journey</p>
      <h2 class="font-serif text-3xl md:text-4xl tracking-tight text-white mb-4">
        Let's Elevate Your<br><em class="text-lux-gold">Property Together</em>
      </h2>
      <p class="text-sm font-light text-neutral-400 max-w-lg mx-auto">
        Share your vision with us. Whether you're launching a new property or seeking to transform an existing one, 
        we're ready to listen.
      </p>
    </div>

    <form id="contact-form" class="space-y-6">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div>
          <label class="text-[10px] uppercase tracking-[0.2em] text-neutral-500 block mb-2">Full Name</label>
          <input type="text" required placeholder="John Smith" class="w-full bg-lux-panel border border-lux-border px-4 py-3 text-sm text-white placeholder-neutral-600 focus:outline-none focus:border-lux-gold/50 transition-colors duration-300">
        </div>
        <div>
          <label class="text-[10px] uppercase tracking-[0.2em] text-neutral-500 block mb-2">Email Address</label>
          <input type="email" required placeholder="john@example.com" class="w-full bg-lux-panel border border-lux-border px-4 py-3 text-sm text-white placeholder-neutral-600 focus:outline-none focus:border-lux-gold/50 transition-colors duration-300">
        </div>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div>
          <label class="text-[10px] uppercase tracking-[0.2em] text-neutral-500 block mb-2">Property Name</label>
          <input type="text" placeholder="Your Hotel Name" class="w-full bg-lux-panel border border-lux-border px-4 py-3 text-sm text-white placeholder-neutral-600 focus:outline-none focus:border-lux-gold/50 transition-colors duration-300">
        </div>
        <div>
          <label class="text-[10px] uppercase tracking-[0.2em] text-neutral-500 block mb-2">Service Interest</label>
          <select class="w-full bg-lux-panel border border-lux-border px-4 py-3 text-sm text-neutral-400 focus:outline-none focus:border-lux-gold/50 transition-colors duration-300 appearance-none cursor-pointer">
            <option>Full Management</option>
            <option>Revenue Optimization</option>
            <option>Staff Training</option>
            <option>Brand Development</option>
            <option>Digital Strategy</option>
            <option>Consultation</option>
          </select>
        </div>
      </div>
      <div>
        <label class="text-[10px] uppercase tracking-[0.2em] text-neutral-500 block mb-2">Your Message</label>
        <textarea rows="4" placeholder="Tell us about your property and your goals..." class="w-full bg-lux-panel border border-lux-border px-4 py-3 text-sm text-white placeholder-neutral-600 focus:outline-none focus:border-lux-gold/50 transition-colors duration-300 resize-none"></textarea>
      </div>
      <div class="text-center pt-4">
        <button type="submit" class="px-10 py-4 bg-white text-lux-black text-xs font-semibold uppercase tracking-wider hover:bg-lux-gold transition-all duration-150">
          Send Inquiry
        </button>
      </div>
    </form>
  </div>
</section>

<!-- FOOTER -->
<footer class="pt-24 pb-12 px-6 border-t border-lux-border">
  <div class="max-w-7xl mx-auto">
    <div class="grid grid-cols-1 md:grid-cols-4 gap-12 mb-16">
      <!-- Brand -->
      <div class="md:col-span-1">
        <div class="flex items-center gap-3 mb-6">
          <div class="w-9 h-9 border border-lux-gold/50 flex items-center justify-center">
            <span class="font-serif text-lux-gold text-lg leading-none">G</span>
          </div>
          <div class="flex flex-col">
            <span class="font-serif text-base tracking-tight text-white">Grandeur</span>
            <span class="text-[9px] uppercase tracking-[0.2em] text-neutral-500">Hospitality Group</span>
          </div>
        </div>
        <p class="text-sm font-light text-neutral-500 leading-relaxed">
          Crafting extraordinary hotel experiences since 2008. Where tradition meets innovation.
        </p>
      </div>
      <!-- Services -->
      <div>
        <p class="text-[10px] uppercase tracking-[0.2em] text-lux-gold mb-6">Services</p>
        <ul class="space-y-3">
          <li><a href="#" class="text-sm text-neutral-500 hover:text-white transition-colors duration-150">Property Management</a></li>
          <li><a href="#" class="text-sm text-neutral-500 hover:text-white transition-colors duration-150">Revenue Optimization</a></li>
          <li><a href="#" class="text-sm text-neutral-500 hover:text-white transition-colors duration-150">Staff Training</a></li>
          <li><a href="#" class="text-sm text-neutral-500 hover:text-white transition-colors duration-150">Guest Experience</a></li>
          <li><a href="#" class="text-sm text-neutral-500 hover:text-white transition-colors duration-150">Brand Development</a></li>
        </ul>
      </div>
      <!-- Company -->
      <div>
        <p class="text-[10px] uppercase tracking-[0.2em] text-lux-gold mb-6">Company</p>
        <ul class="space-y-3">
          <li><a href="#" class="text-sm text-neutral-500 hover:text-white transition-colors duration-150">About Us</a></li>
          <li><a href="#" class="text-sm text-neutral-500 hover:text-white transition-colors duration-150">Our Portfolio</a></li>
          <li><a href="#" class="text-sm text-neutral-500 hover:text-white transition-colors duration-150">Careers</a></li>
          <li><a href="#" class="text-sm text-neutral-500 hover:text-white transition-colors duration-150">Press & Media</a></li>
          <li><a href="#" class="text-sm text-neutral-500 hover:text-white transition-colors duration-150">Contact</a></li>
        </ul>
      </div>
      <!-- Contact -->
      <div>
        <p class="text-[10px] uppercase tracking-[0.2em] text-lux-gold mb-6">Get in Touch</p>
        <ul class="space-y-3">
          <li class="flex items-center gap-3">
            <span class="iconify text-lux-gold/60" data-icon="lucide:map-pin" data-width="14"></span>
            <span class="text-sm text-neutral-500">42 Mayfair Lane, London W1K</span>
          </li>
          <li class="flex items-center gap-3">
            <span class="iconify text-lux-gold/60" data-icon="lucide:phone" data-width="14"></span>
            <span class="text-sm text-neutral-500">+44 (0) 20 7946 0958</span>
          </li>
          <li class="flex items-center gap-3">
            <span class="iconify text-lux-gold/60" data-icon="lucide:mail" data-width="14"></span>
            <span class="text-sm text-neutral-500">inquire@grandeur.com</span>
          </li>
        </ul>
        <div class="flex items-center gap-4 mt-6">
          <a href="#" class="text-neutral-600 hover:text-lux-gold transition-colors duration-150">
            <span class="iconify" data-icon="lucide:linkedin" data-width="18"></span>
          </a>
          <a href="#" class="text-neutral-600 hover:text-lux-gold transition-colors duration-150">
            <span class="iconify" data-icon="lucide:instagram" data-width="18"></span>
          </a>
          <a href="#" class="text-neutral-600 hover:text-lux-gold transition-colors duration-150">
            <span class="iconify" data-icon="lucide:facebook" data-width="18"></span>
          </a>
          <a href="#" class="text-neutral-600 hover:text-lux-gold transition-colors duration-150">
            <span class="iconify" data-icon="lucide:twitter" data-width="18"></span>
          </a>
        </div>
      </div>
    </div>
    <!-- Bottom -->
    <div class="border-t border-lux-border pt-8 flex flex-col md:flex-row items-center justify-between gap-4">
      <p class="text-[10px] uppercase tracking-[0.15em] text-neutral-600">
        © 2024 Grandeur Hospitality Group. All rights reserved.
      </p>
      <div class="flex items-center gap-6">
        <a href="#" class="text-[10px] uppercase tracking-[0.15em] text-neutral-600 hover:text-neutral-400 transition-colors">Privacy Policy</a>
        <a href="#" class="text-[10px] uppercase tracking-[0.15em] text-neutral-600 hover:text-neutral-400 transition-colors">Terms of Service</a>
        <a href="#" class="text-[10px] uppercase tracking-[0.15em] text-neutral-600 hover:text-neutral-400 transition-colors">Cookie Policy</a>
      </div>
    </div>
  </div>
</footer>

<!-- ===================== SCRIPTS ===================== -->
<script>
// ---- TOAST ----
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 3500);
}

// ---- MOBILE MENU ----
const mobileBtn = document.getElementById('mobile-menu-btn');
const mobileMenu = document.getElementById('mobile-menu');
const mobileClose = document.getElementById('mobile-close-btn');
mobileBtn.addEventListener('click', () => {
  mobileMenu.classList.remove('opacity-0','pointer-events-none');
  mobileMenu.classList.add('opacity-100','pointer-events-auto');
});
mobileClose.addEventListener('click', () => {
  mobileMenu.classList.add('opacity-0','pointer-events-none');
  mobileMenu.classList.remove('opacity-100','pointer-events-auto');
});
document.querySelectorAll('.mobile-link').forEach(l => {
  l.addEventListener('click', () => {
    mobileMenu.classList.add('opacity-0','pointer-events-none');
    mobileMenu.classList.remove('opacity-100','pointer-events-auto');
  });
});

// ---- NAVBAR SCROLL ----
window.addEventListener('scroll', () => {
  const nav = document.getElementById('navbar');
  if (window.scrollY > 60) nav.classList.add('nav-scrolled');
  else nav.classList.remove('nav-scrolled');
});

// ---- CONTACT FORM ----
document.getElementById('contact-form').addEventListener('submit', function(e) {
  e.preventDefault();
  showToast('✓ Thank you! Your inquiry has been received. We\'ll be in touch within 24 hours.');
  this.reset();
});

// ---- STAT COUNTER (Intersection Observer) ----
const statNumbers = document.querySelectorAll('.stat-number');
let statAnimated = false;
const statsObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting && !statAnimated) {
      statAnimated = true;
      statNumbers.forEach(el => {
        const target = parseInt(el.getAttribute('data-target'));
        let current = 0;
        const increment = target / 60;
        const timer = setInterval(() => {
          current += increment;
          if (current >= target) {
            current = target;
            clearInterval(timer);
          }
          el.textContent = Math.floor(current);
        }, 25);
      });
    }
  });
}, { threshold: 0.5 });
statNumbers.forEach(el => statsObserver.observe(el));

// ---- SCROLL REVEAL ----
const revealEls = document.querySelectorAll('.service-card, .property-card, #about .grid > div');
const revealObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.style.opacity = '1';
      entry.target.style.transform = 'translateY(0)';
    }
  });
}, { threshold: 0.1, rootMargin: '0px 0px -50px 0px' });
revealEls.forEach(el => {
  el.style.opacity = '0';
  el.style.transform = 'translateY(20px)';
  el.style.transition = 'opacity 0.7s ease, transform 0.7s ease';
  revealObserver.observe(el);
});

// ===================== THREE.JS 3D HOTEL BUILDING =====================
(function() {
  const canvas = document.getElementById('hero-3d-canvas');
  const renderer = new THREE.WebGLRenderer({ canvas, antialias: true, alpha: true });
  renderer.setSize(window.innerWidth, window.innerHeight);
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  renderer.shadowMap.enabled = true;
  renderer.shadowMap.type = THREE.PCFSoftShadowMap;
  renderer.toneMapping = THREE.ACESFilmicToneMapping;
  renderer.toneMappingExposure = 0.8;

  const scene = new THREE.Scene();
  scene.fog = new THREE.FogExp2(0x050505, 0.015);

  const camera = new THREE.PerspectiveCamera(35, window.innerWidth / window.innerHeight, 0.1, 100);
  camera.position.set(12, 8, 14);
  camera.lookAt(0, 2, 0);

  // Lights
  const ambient = new THREE.AmbientLight(0xffffff, 0.15);
  scene.add(ambient);

  const keyLight = new THREE.SpotLight(0xfff5e0, 1.5);
  keyLight.position.set(10, 15, 8);
  keyLight.angle = 0.5;
  keyLight.penumbra = 0.8;
  keyLight.castShadow = true;
  keyLight.shadow.mapSize.set(1024, 1024);
  scene.add(keyLight);

  const rimLight = new THREE.SpotLight(0xC5A059, 0.8);
  rimLight.position.set(-8, 10, -5);
  rimLight.angle = 0.6;
  rimLight.penumbra = 0.9;
  scene.add(rimLight);

  const fillLight = new THREE.PointLight(0x3a4a6b, 0.4);
  fillLight.position.set(-5, 3, 8);
  scene.add(fillLight);

  // Materials
  const stoneMat = new THREE.MeshStandardMaterial({
    color: 0xd4cfc4,
    roughness: 0.85,
    metalness: 0.05
  });
  const stoneDarkMat = new THREE.MeshStandardMaterial({
    color: 0xb8b0a2,
    roughness: 0.9,
    metalness: 0.05
  });
  const goldMat = new THREE.MeshStandardMaterial({
    color: 0xC5A059,
    roughness: 0.3,
    metalness: 0.7
  });
  const windowMat = new THREE.MeshStandardMaterial({
    color: 0x1a2a40,
    roughness: 0.2,
    metalness: 0.3,
    emissive: 0x2a3a55,
    emissiveIntensity: 0.3
  });
  const windowLitMat = new THREE.MeshStandardMaterial({
    color: 0xffe8b0,
    roughness: 0.2,
    metalness: 0.3,
    emissive: 0xffd080,
    emissiveIntensity: 0.6
  });
  const groundMat = new THREE.MeshStandardMaterial({
    color: 0x1a1a1a,
    roughness: 1.0,
    metalness: 0.0
  });

  const building = new THREE.Group();

  // Base/Steps
  const baseMat = new THREE.MeshStandardMaterial({ color: 0x9a9590, roughness: 0.9 });
  const base1 = new THREE.Mesh(new THREE.BoxGeometry(12, 0.3, 8), baseMat);
  base1.position.y = 0.15;
  base1.castShadow = true;
  base1.receiveShadow = true;
  building.add(base1);

  const base2 = new THREE.Mesh(new THREE.BoxGeometry(11, 0.3, 7), baseMat);
  base2.position.y = 0.45;
  base2.castShadow = true;
  building.add(base2);

  const base3 = new THREE.Mesh(new THREE.BoxGeometry(10, 0.3, 6), baseMat);
  base3.position.y = 0.75;
  base3.castShadow = true;
  building.add(base3);

  // Main building body
  const mainBody = new THREE.Mesh(new THREE.BoxGeometry(9, 5, 5.5), stoneMat);
  mainBody.position.y = 3.25;
  mainBody.castShadow = true;
  mainBody.receiveShadow = true;
  building.add(mainBody);

  // Windows - Front face
  for (let row = 0; row < 3; row++) {
    for (let col = 0; col < 6; col++) {
      const w = new THREE.Mesh(
        new THREE.BoxGeometry(0.7, 1.0, 0.05),
        Math.random() > 0.4 ? windowLitMat : windowMat
      );
      w.position.set(-3.5 + col * 1.3, 1.8 + row * 1.5, 2.78);
      building.add(w);
      // Window frame
      const frame = new THREE.Mesh(
        new THREE.BoxGeometry(0.8, 1.1, 0.03),
        stoneDarkMat
      );
      frame.position.set(-3.5 + col * 1.3, 1.8 + row * 1.5, 2.76);
      building.add(frame);
    }
  }

  // Windows - Back face
  for (let row = 0; row < 3; row++) {
    for (let col = 0; col < 6; col++) {
      const w = new THREE.Mesh(
        new THREE.BoxGeometry(0.7, 1.0, 0.05),
        Math.random() > 0.5 ? windowLitMat : windowMat
      );
      w.position.set(-3.5 + col * 1.3, 1.8 + row * 1.5, -2.78);
      building.add(w);
    }
  }

  // Side windows
  for (let row = 0; row < 3; row++) {
    for (let col = 0; col < 3; col++) {
      const wL = new THREE.Mesh(
        new THREE.BoxGeometry(0.05, 1.0, 0.7),
        Math.random() > 0.5 ? windowLitMat : windowMat
      );
      wL.position.set(-4.53, 1.8 + row * 1.5, -1.3 + col * 1.3);
      building.add(wL);

      const wR = new THREE.Mesh(
        new THREE.BoxGeometry(0.05, 1.0, 0.7),
        Math.random() > 0.5 ? windowLitMat : windowMat
      );
      wR.position.set(4.53, 1.8 + row * 1.5, -1.3 + col * 1.3);
      building.add(wR);
    }
  }

  // Entablature (horizontal band above columns)
  const entablature = new THREE.Mesh(new THREE.BoxGeometry(10, 0.5, 6.5), stoneDarkMat);
  entablature.position.y = 5.75;
  entablature.castShadow = true;
  building.add(entablature);

  // Gold trim on entablature
  const trim = new THREE.Mesh(new THREE.BoxGeometry(10.1, 0.08, 6.6), goldMat);
  trim.position.y = 6.01;
  building.add(trim);

  // Pediment (triangular roof on front)
  const pedimentShape = new THREE.Shape();
  pedimentShape.moveTo(-5.2, 0);
  pedimentShape.lineTo(5.2, 0);
  pedimentShape.lineTo(0, 2.2);
  pedimentShape.lineTo(-5.2, 0);
  const pedimentGeo = new THREE.ExtrudeGeometry(pedimentShape, { depth: 0.5, bevelEnabled: false });
  const pediment = new THREE.Mesh(pedimentGeo, stoneMat);
  pediment.position.set(0, 6.0, 2.75);
  pediment.castShadow = true;
  building.add(pediment);

  // Back pediment
  const pedimentBack = new THREE.Mesh(pedimentGeo, stoneMat);
  pedimentBack.position.set(0, 6.0, -3.25);
  pedimentBack.rotation.y = Math.PI;
  building.add(pedimentBack);

  // Roof
  const roofShape = new THREE.Shape();
  roofShape.moveTo(-5.5, 0);
  roofShape.lineTo(5.5, 0);
  roofShape.lineTo(0, 2.5);
  roofShape.lineTo(-5.5, 0);
  const roofGeo = new THREE.ExtrudeGeometry(roofShape, { depth: 6, bevelEnabled: false });
  const roofMat = new THREE.MeshStandardMaterial({ color: 0x3a3530, roughness: 0.8 });
  const roof = new THREE.Mesh(roofGeo, roofMat);
  roof.position.set(0, 6.0, -3.0);
  roof.castShadow = true;
  building.add(roof);

  // Columns - Front
  function createColumn(x, z) {
    const group = new THREE.Group();
    // Base
    const colBase = new THREE.Mesh(new THREE.CylinderGeometry(0.3, 0.35, 0.3, 16), stoneDarkMat);
    colBase.position.y = 0.15;
    group.add(colBase);
    // Shaft
    const shaft = new THREE.Mesh(new THREE.CylinderGeometry(0.22, 0.26, 4.7, 16), stoneMat);
    shaft.position.y = 2.65;
    shaft.castShadow = true;
    group.add(shaft);
    // Capital
    const capital = new THREE.Mesh(new THREE.CylinderGeometry(0.35, 0.22, 0.3, 16), stoneDarkMat);
    capital.position.y = 5.15;
    group.add(capital);
    // Gold detail on capital
    const goldRing = new THREE.Mesh(new THREE.TorusGeometry(0.32, 0.03, 8, 16), goldMat);
    goldRing.position.y = 5.3;
    goldRing.rotation.x = Math.PI / 2;
    group.add(goldRing);

    group.position.set(x, 0.9, z);
    return group;
  }

  // Front columns
  const frontColPositions = [-4.2, -2.4, -0.6, 1.2, 3.0];
  frontColPositions.forEach(x => building.add(createColumn(x, 2.8)));

  // Back columns
  frontColPositions.forEach(x => building.add(createColumn(x, -2.8)));

  // Side columns
  for (let i = 0; i < 3; i++) {
    building.add(createColumn(-4.6, -0.8 + i * 1.6));
    building.add(createColumn(4.6, -0.8 + i * 1.6));
  }

  // Grand entrance door
  const doorFrame = new THREE.Mesh(new THREE.BoxGeometry(2.0, 3.0, 0.15), stoneDarkMat);
  doorFrame.position.set(0, 2.4, 2.85);
  building.add(doorFrame);

  const doorLeft = new THREE.Mesh(new THREE.BoxGeometry(0.9, 2.8, 0.1), new THREE.MeshStandardMaterial({ color: 0x2a1f15, roughness: 0.6 }));
  doorLeft.position.set(-0.45, 2.3, 2.9);
  building.add(doorLeft);

  const doorRight = new THREE.Mesh(new THREE.BoxGeometry(0.9, 2.8, 0.1), new THREE.MeshStandardMaterial({ color: 0x2a1f15, roughness: 0.6 }));
  doorRight.position.set(0.45, 2.3, 2.9);
  building.add(doorRight);

  // Door arch
  const archShape = new THREE.Shape();
  archShape.absarc(0, 0, 1, 0, Math.PI, false);
  const archGeo = new THREE.ExtrudeGeometry(archShape, { depth: 0.12, bevelEnabled: false });
  const arch = new THREE.Mesh(archGeo, stoneDarkMat);
  arch.position.set(0, 3.7, 2.82);
  building.add(arch);

  // Gold door handle details
  const handleL = new THREE.Mesh(new THREE.SphereGeometry(0.06, 8, 8), goldMat);
  handleL.position.set(-0.1, 2.3, 2.97);
  building.add(handleL);
  const handleR = new THREE.Mesh(new THREE.SphereGeometry(0.06, 8, 8), goldMat);
  handleR.position.set(0.1, 2.3, 2.97);
  building.add(handleR);

  // Flagpole accents on top
  const finial = new THREE.Mesh(new THREE.SphereGeometry(0.12, 8, 8), goldMat);
  finial.position.set(0, 8.6, 0);
  building.add(finial);
  const pole = new THREE.Mesh(new THREE.CylinderGeometry(0.03, 0.03, 0.3, 8), goldMat);
  pole.position.set(0, 8.4, 0);
  building.add(pole);

  scene.add(building);

  // Ground plane
  const ground = new THREE.Mesh(new THREE.PlaneGeometry(60, 60), groundMat);
  ground.rotation.x = -Math.PI / 2;
  ground.position.y = 0;
  ground.receiveShadow = true;
  scene.add(ground);

  // Subtle grid
  const gridHelper = new THREE.GridHelper(60, 60, 0x1a1a1a, 0x1a1a1a);
  gridHelper.position.y = 0.01;
  scene.add(gridHelper);

  // Particles (floating dust motes)
  const particleCount = 200;
  const particleGeo = new THREE.BufferGeometry();
  const positions = new Float32Array(particleCount * 3);
  for (let i = 0; i < particleCount; i++) {
    positions[i * 3] = (Math.random() - 0.5) * 30;
    positions[i * 3 + 1] = Math.random() * 15;
    positions[i * 3 + 2] = (Math.random() - 0.5) * 30;
  }
  particleGeo.setAttribute('position', new THREE.BufferAttribute(positions, 3));
  const particleMat = new THREE.PointsMaterial({
    color: 0xC5A059,
    size: 0.04,
    transparent: true,
    opacity: 0.5
  });
  const particles = new THREE.Points(particleGeo, particleMat);
  scene.add(particles);

  // ---- Mouse / Drag Interaction ----
  let isDragging = false;
  let previousMouse = { x: 0, y: 0 };
  let targetRotation = { y: -0.3, x: 0.15 };
  let currentRotation = { y: -0.3, x: 0.15 };
  let autoRotate = true;

  canvas.addEventListener('mousedown', (e) => {
    isDragging = true;
    autoRotate = false;
    previousMouse = { x: e.clientX, y: e.clientY };
    canvas.style.cursor = 'grabbing';
  });

  window.addEventListener('mousemove', (e) => {
    if (!isDragging) return;
    const dx = e.clientX - previousMouse.x;
    const dy = e.clientY - previousMouse.y;
    targetRotation.y += dx * 0.005;
    targetRotation.x += dy * 0.003;
    targetRotation.x = Math.max(-0.5, Math.min(0.8, targetRotation.x));
    previousMouse = { x: e.clientX, y: e.clientY };
  });

  window.addEventListener('mouseup', () => {
    isDragging = false;
    canvas.style.cursor = 'grab';
    setTimeout(() => { autoRotate = true; }, 3000);
  });

  // Touch support
  canvas.addEventListener('touchstart', (e) => {
    isDragging = true;
    autoRotate = false;
    previousMouse = { x: e.touches[0].clientX, y: e.touches[0].clientY };
  });
  canvas.addEventListener('touchmove', (e) => {
    if (!isDragging) return;
    e.preventDefault();
    const dx = e.touches[0].clientX - previousMouse.x;
    const dy = e.touches[0].clientY - previousMouse.y;
    targetRotation.y += dx * 0.005;
    targetRotation.x += dy * 0.003;
    targetRotation.x = Math.max(-0.5, Math.min(0.8, targetRotation.x));
    previousMouse = { x: e.touches[0].clientX, y: e.touches[0].clientY };
  }, { passive: false });
  canvas.addEventListener('touchend', () => {
    isDragging = false;
    setTimeout(() => { autoRotate = true; }, 3000);
  });

  canvas.style.cursor = 'grab';

  // ---- Animation Loop ----
  const clock = new THREE.Clock();
  function animate() {
    requestAnimationFrame(animate);
    const elapsed = clock.getElapsedTime();

    // Auto rotate
    if (autoRotate) {
      targetRotation.y += 0.002;
    }

    // Smooth lerp
    currentRotation.y += (targetRotation.y - currentRotation.y) * 0.05;
    currentRotation.x += (targetRotation.x - currentRotation.x) * 0.05;
    building.rotation.y = currentRotation.y;
    building.rotation.x = currentRotation.x;

    // Floating particles
    const posArr = particleGeo.attributes.position.array;
    for (let i = 0; i < particleCount; i++) {
      posArr[i * 3 + 1] += Math.sin(elapsed + i) * 0.002;
      if (posArr[i * 3 + 1] > 15) posArr[i * 3 + 1] = 0;
    }
    particleGeo.attributes.position.needsUpdate = true;

    // Subtle light animation
    rimLight.intensity = 0.6 + Math.sin(elapsed * 0.5) * 0.2;

    renderer.render(scene, camera);
  }
  animate();

  // ---- Resize ----
  window.addEventListener('resize', () => {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  });
})();
</script>

</body>
</html>
