---
layout: page
title:
search_exclude: true
description: God knows
hide: true
menu: nav/home.html
---

<div class="relative py-24 px-6 overflow-hidden max-w-6xl mx-auto">
  <!-- Glowing blob background -->
    <div id="mouseBlob"
       class="absolute w-[600px] h-[600px] bg-gradient-to-br from-accent via-purple-800 to-transparent 
              opacity-20 blur-3xl rounded-full animate-pulse-slow pointer-events-none z-0 transition-transform duration-200">
    </div>

  <!-- Hero content -->
<div class="relative z-10 max-w-5xl mx-auto text-center space-y-8">
    <h1 class="text-5xl md:text-6xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-white via-accent to-purple-400 animate-gradient-x">
      Infrastructure Intelligence, <br> Built for the Real World.
    </h1>
    <p class="text-lg text-gray-400 max-w-2xl mx-auto animate-slide-in">
      SD IMOP is a next-gen infrastructure platform helping cities like San Diego manage roads, repairs, and resources with precision. It’s fast, data-rich, and optimized for scale.
    </p>
    <div class="flex flex-col sm:flex-row justify-center gap-4 mt-4 animate-fade-in">
      <a href="{{site.baseurl}}/blogs" class="px-6 py-3 bg-accent text-white font-medium rounded-full shadow-lg hover:bg-white hover:text-accent border border-accent transition duration-300 transform hover:scale-105">
        Read the Blogs
      </a>
      <a href="#modules" class="px-6 py-3 border-2 border-accent text-accent font-medium rounded-full hover:bg-accent hover:text-white transition">
        Platform Overview →
      </a>
    </div>
  </div>
</div>

<!-- Features section -->
<!-- Insights Section -->
<section id="insights" class="py-16 px-6 max-w-6xl mx-auto">
  <h2 class="text-3xl font-bold text-white mb-8">Insights & Research</h2>
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
    <a href="{{site.baseurl}}/blogs" class="bg-neutralCard border border-white/10 hover:border-accent p-5 rounded-xl transition group">
      <div class="text-2xl text-accent mb-2"><i class="fas fa-lightbulb"></i></div>
      <h3 class="text-lg font-semibold text-white group-hover:text-accent">Blog Posts</h3>
      <p class="text-gray-400 text-sm">Developer writeups, new features, internal research, and data applications.</p>
    </a>
    <a href="https://github.com/adik1025/imop_frontend/issues/8" class="bg-neutralCard border border-white/10 hover:border-accent p-5 rounded-xl transition group">
      <div class="text-2xl text-accent mb-2"><i class="fas fa-archive"></i></div>
      <h3 class="text-lg font-semibold text-white group-hover:text-accent">Project Overview</h3>
      <p class="text-gray-400 text-sm">Access our plans, delegations, and preliminary writeups.</p>
    </a>
  </div>
</section>

<!-- Core Modules Section -->
<section class="pt-12 pb-20 px-6 max-w-6xl mx-auto" id="modules">
  <h2 class="text-4xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-accent via-purple-500 to-white mb-12 animate-gradient-x">
    Explore the Core of IMOP
  </h2>
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8">
    <a href="{{ site.baseurl }}/map" class="group bg-neutralCard border border-white/10 hover:border-accent p-6 rounded-xl transition-all shadow-md">
      <div class="text-3xl text-accent mb-3"><i class="fas fa-map-marked-alt"></i></div>
      <h3 class="text-xl font-semibold group-hover:text-accent">Maintenance Map</h3>
      <p class="text-gray-400 mt-2 text-sm">Real-time map + table of active repair zones and condition scores city-wide.</p>
    </a>
    <a href="{{ site.baseurl }}/pavements" class="group bg-neutralCard border border-white/10 hover:border-accent p-6 rounded-xl transition-all shadow-md">
      <div class="text-3xl text-accent mb-3"><i class="fas fa-file-upload"></i></div>
      <h3 class="text-xl font-semibold group-hover:text-accent">Data Uploads</h3>
      <p class="text-gray-400 mt-2 text-sm">Upload CSV data directly to the backend for infrastructure analysis.</p>
    </a>
    <a href="{{ site.baseurl }}/roads" class="group bg-neutralCard border border-white/10 hover:border-accent p-6 rounded-xl transition-all shadow-md">
      <div class="text-3xl text-accent mb-3"><i class="fas fa-road"></i></div>
      <h3 class="text-xl font-semibold group-hover:text-accent">Roads Overview</h3>
      <p class="text-gray-400 mt-2 text-sm">Browse active roads, tagged work orders, and repair intervals across the network.</p>
    </a>
    <a href="{{ site.baseurl }}/schedule" class="group bg-neutralCard border border-white/10 hover:border-accent p-6 rounded-xl transition-all shadow-md">
      <div class="text-3xl text-accent mb-3"><i class="fas fa-calendar-alt"></i></div>
      <h3 class="text-xl font-semibold group-hover:text-accent">Schedule Planner</h3>
      <p class="text-gray-400 mt-2 text-sm">See repair timelines and scheduled maintenance.</p>
    </a>
    <a href="{{ site.baseurl }}/districts" class="group bg-neutralCard border border-white/10 hover:border-accent p-6 rounded-xl transition-all shadow-md">
      <div class="text-3xl text-accent mb-3"><i class="fas fa-city"></i></div>
      <h3 class="text-xl font-semibold group-hover:text-accent">District Mapping</h3>
      <p class="text-gray-400 mt-2 text-sm">Visualize San Diego’s council districts and geographic maintenance clustering.</p>
    </a>
  </div>
</section>

<script>
  document.addEventListener("DOMContentLoaded", () => {
    const blob = document.getElementById('mouseBlob');
    const hero = document.querySelector('.relative.py-24');

    hero.addEventListener('mousemove', (e) => {
      const rect = hero.getBoundingClientRect();
      const x = e.clientX - rect.left - 300;
      const y = e.clientY - rect.top - 300;

      blob.style.transform = `translate(${x}px, ${y}px)`;
    });

    hero.addEventListener('mouseleave', () => {
      blob.style.transform = `translate(0px, 0px)`;
    });
  });
</script>