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
  <div class="absolute top-0 left-0 w-[600px] h-[600px] bg-gradient-to-br from-accent via-purple-800 to-transparent opacity-20 blur-3xl rounded-full animate-pulse-slow pointer-events-none z-0"></div>

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
      <a href="#thecore" class="px-6 py-3 border-2 border-accent text-accent font-medium rounded-full hover:bg-accent hover:text-white transition">
        Platform Overview →
      </a>
    </div>
  </div>
</div>

<!-- Features section -->
<section id="features" class="py-20 px-6 max-w-6xl mx-auto space-y-14">
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-10">
    <div class="bg-neutralCard p-6 rounded-xl border border-white/10 hover:border-accent transition-all group">
      <div class="text-3xl text-accent mb-4"><i class="fas fa-road"></i></div>
      <h3 class="text-xl font-semibold group-hover:text-accent">Live Road Tracking</h3>
      <p class="text-gray-400 mt-2 text-sm">Visualize street data, work orders, and repair zones with smart overlays and map tools.</p>
    </div>
    <div class="bg-neutralCard p-6 rounded-xl border border-white/10 hover:border-accent transition-all group">
      <div class="text-3xl text-accent mb-4"><i class="fas fa-ruler-combined"></i></div>
      <h3 class="text-xl font-semibold group-hover:text-accent">Maintenance Forecasts</h3>
      <p class="text-gray-400 mt-2 text-sm">Use historical records and machine learning to predict degradation before it happens.</p>
    </div>
    <div class="bg-neutralCard p-6 rounded-xl border border-white/10 hover:border-accent transition-all group">
      <div class="text-3xl text-accent mb-4"><i class="fas fa-tachometer-alt"></i></div>
      <h3 class="text-xl font-semibold group-hover:text-accent">Performance Dashboards</h3>
      <p class="text-gray-400 mt-2 text-sm">Analyze KPIs, prioritize tasks, and create visual summaries for stakeholder updates.</p>
    </div>
  </div>
</section>

<!-- Quick links -->
<section id="thecore" class="py-20 px-6 max-w-6xl mx-auto">
  <h2 class="text-2xl font-bold text-white mb-8">Jump Into IMOP</h2>
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
    <a href="{{site.baseurl}}/blogs" class="bg-neutralCard border border-white/10 hover:border-accent p-5 rounded-xl transition group">
      <h3 class="text-lg font-semibold text-white group-hover:text-accent">Recent Blog Posts</h3>
      <p class="text-gray-400 text-sm">Catch up on new features, dev notes, and case studies.</p>
    </a>
    <a href="{{site.baseurl}}/blogs" class="bg-neutralCard border border-white/10 hover:border-accent p-5 rounded-xl transition group">
      <h3 class="text-lg font-semibold text-white group-hover:text-accent">Archives</h3>
      <p class="text-gray-400 text-sm">Browse older content and historical writeups.</p>
    </a>
    <a href="#features" class="bg-neutralCard border border-white/10 hover:border-accent p-5 rounded-xl transition group">
      <h3 class="text-lg font-semibold text-white group-hover:text-accent">Platform Features</h3>
      <p class="text-gray-400 text-sm">Explore everything SD IMOP has to offer.</p>
    </a>
  </div>
</section>

<!-- Core Modules Section -->
<section class="py-24 px-6 max-w-6xl mx-auto" id="modules">
  <div class="relative z-10">
    <h2 class="text-4xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-accent via-purple-500 to-white mb-12 animate-gradient-x">
      Explore the Core of IMOP
    </h2>
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8">
      <a href="{{ site.baseurl }}/map" class="group bg-neutralCard border border-white/10 hover:border-accent p-6 rounded-xl transition-all shadow-md">
        <div class="text-3xl text-accent mb-3"><i class="fas fa-map-marked-alt"></i></div>
        <h3 class="text-xl font-semibold group-hover:text-accent">Maintenance Map</h3>
        <p class="text-gray-400 mt-2 text-sm">Interactive map + table of areas needing repair across San Diego. Updated in real-time with backend data.</p>
      </a>
      <a href="{{ site.baseurl }}/pavements" class="group bg-neutralCard border border-white/10 hover:border-accent p-6 rounded-xl transition-all shadow-md">
        <div class="text-3xl text-accent mb-3"><i class="fas fa-file-upload"></i></div>
        <h3 class="text-xl font-semibold group-hover:text-accent">Data Uploads</h3>
        <p class="text-gray-400 mt-2 text-sm">Upload your own CSV datasets directly to our backend to track degradation, repair needs, and drive ML predictions.</p>
      </a>
      <a href="{{ site.baseurl }}/roads" class="group bg-neutralCard border border-white/10 hover:border-accent p-6 rounded-xl transition-all shadow-md">
        <div class="text-3xl text-accent mb-3"><i class="fas fa-road"></i></div>
        <h3 class="text-xl font-semibold group-hover:text-accent">Roads Overview</h3>
        <p class="text-gray-400 mt-2 text-sm">Browse all roads in San Diego — with live status, repair tags, and direct links to schedule updates.</p>
      </a>
      <a href="{{ site.baseurl }}/schedule" class="group bg-neutralCard border border-white/10 hover:border-accent p-6 rounded-xl transition-all shadow-md">
        <div class="text-3xl text-accent mb-3"><i class="fas fa-calendar-alt"></i></div>
        <h3 class="text-xl font-semibold group-hover:text-accent">Schedule Planner</h3>
        <p class="text-gray-400 mt-2 text-sm">Integrated with Google Calendar API — view and coordinate repair dates, city maintenance, and resource planning.</p>
      </a>
      <a href="{{ site.baseurl }}/districts" class="group bg-neutralCard border border-white/10 hover:border-accent p-6 rounded-xl transition-all shadow-md">
        <div class="text-3xl text-accent mb-3"><i class="fas fa-city"></i></div>
        <h3 class="text-xl font-semibold group-hover:text-accent">District Mapping</h3>
        <p class="text-gray-400 mt-2 text-sm">Visualize all council districts in San Diego. Identify maintenance priorities per region with geographic overlays.</p>
      </a>
    </div>
  </div>
</section>