<!doctype html>
<html lang="ms" class="h-full">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sistem Booking Bilik Kuliah / Makmal / Bilik Mesyuarat JKE</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="/_sdk/element_sdk.js"></script>
  <script src="/_sdk/data_sdk.js"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&amp;display=swap" rel="stylesheet">
  <style>
    body {
      box-sizing: border-box;
    }
    * {
      font-family: 'Plus Jakarta Sans', sans-serif;
    }
    .room-card {
      transition: all 0.3s ease;
    }
    .room-card:hover {
      transform: translateY(-4px);
    }
    .booking-item {
      animation: slideIn 0.3s ease;
    }
    @keyframes slideIn {
      from {
        opacity: 0;
        transform: translateX(-10px);
      }
      to {
        opacity: 1;
        transform: translateX(0);
      }
    }
    .modal-overlay {
      animation: fadeIn 0.2s ease;
    }
    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }
    .status-available {
      background: linear-gradient(135deg, #10b981 0%, #059669 100%);
    }
    .status-booked {
      background: linear-gradient(135deg, #ef4444 0%, #dc2626 100%);
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
 </head>
 <body class="h-full bg-slate-900 text-white overflow-auto">
  <div id="app-wrapper" class="w-full min-h-full bg-gradient-to-br from-slate-900 via-slate-800 to-indigo-900"><!-- Header -->
   <header class="bg-slate-800/50 backdrop-blur-sm border-b border-slate-700 sticky top-0 z-40">
    <div class="max-w-7xl mx-auto px-4 py-4">
     <div class="flex items-center justify-between">
      <div>
       <h1 id="main-title" class="text-2xl font-bold bg-gradient-to-r from-indigo-400 to-purple-400 bg-clip-text text-transparent">Sistem Booking Bilik Kuliah / Makmal / Bilik Mesyuarat JKE</h1>
       <p id="subtitle" class="text-slate-400 text-sm mt-1">JKE POLISAS</p>
      </div>
      <div class="flex items-center gap-3"><span id="booking-count" class="text-sm text-slate-400">0 tempahan aktif</span> <button id="view-bookings-btn" class="px-4 py-2 bg-slate-700 hover:bg-slate-600 rounded-lg text-sm font-medium transition-colors flex items-center gap-2">
        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewbox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2" />
        </svg> Senarai Tempahan </button>
      </div>
     </div>
    </div>
   </header><!-- Main Content -->
   <main class="max-w-7xl mx-auto px-4 py-8"><!-- Room Grid -->
    <section>
     <h2 class="text-xl font-semibold text-white mb-6 flex items-center gap-2">
      <svg class="w-6 h-6 text-indigo-400" fill="none" stroke="currentColor" viewbox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4" />
      </svg> Lokasi</h2>
     <div id="rooms-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4"><!-- Rooms will be rendered here -->
     </div>
    </section>
   </main><!-- Booking Modal -->
   <div id="booking-modal" class="hidden fixed inset-0 z-50 flex items-center justify-center p-4">
    <div class="modal-overlay absolute inset-0 bg-black/60 backdrop-blur-sm" id="modal-backdrop"></div>
    <div class="relative bg-slate-800 rounded-2xl shadow-2xl w-full max-w-md border border-slate-700 overflow-hidden">
     <div class="bg-gradient-to-r from-indigo-600 to-purple-600 px-6 py-4">
      <h3 id="modal-title" class="text-lg font-semibold text-white">Booking Bilik</h3>
      <p id="modal-room-name" class="text-indigo-200 text-sm"></p>
     </div>
     <form id="booking-form" class="p-6 space-y-4"><input type="hidden" id="selected-room-id"> <input type="hidden" id="selected-room-name">
      <div><label for="booker-name" class="block text-sm font-medium text-slate-300 mb-1">Nama Penuh</label> <input type="text" id="booker-name" required class="w-full px-4 py-2.5 bg-slate-700 border border-slate-600 rounded-lg text-white placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent" placeholder="Masukkan nama anda">
      </div>
      <div><label for="purpose" class="block text-sm font-medium text-slate-300 mb-1">Tujuan Penggunaan</label> <input type="text" id="purpose" required class="w-full px-4 py-2.5 bg-slate-700 border border-slate-600 rounded-lg text-white placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent" placeholder="Cth: Kuliah, Mesyuarat, Tutorial">
      </div>
      <div><label for="booker-password" class="block text-sm font-medium text-slate-300 mb-1">Password Penempah</label> <input type="password" id="booker-password" required minlength="4" class="w-full px-4 py-2.5 bg-slate-700 border border-slate-600 rounded-lg text-white placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent" placeholder="Minimum 4 aksara (untuk memadam tempahan)">
       <p class="text-xs text-slate-400 mt-1">💡 Password ini diperlukan untuk memadam tempahan anda nanti</p>
      </div>
      <div><label for="booking-date" class="block text-sm font-medium text-slate-300 mb-1">Tarikh</label> <input type="date" id="booking-date" required class="w-full px-4 py-2.5 bg-slate-700 border border-slate-600 rounded-lg text-white focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent">
      </div>
      <div class="grid grid-cols-2 gap-4">
       <div><label for="start-time" class="block text-sm font-medium text-slate-300 mb-1">Masa Mula</label> <input type="time" id="start-time" required min="07:00" max="18:00" class="w-full px-4 py-2.5 bg-slate-700 border border-slate-600 rounded-lg text-white focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent">
       </div>
       <div><label for="end-time" class="block text-sm font-medium text-slate-300 mb-1">Masa Tamat</label> <input type="time" id="end-time" required min="07:00" max="18:00" class="w-full px-4 py-2.5 bg-slate-700 border border-slate-600 rounded-lg text-white focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent">
       </div>
      </div>
      <div id="time-conflict-warning" class="hidden p-3 bg-red-500/20 border border-red-500/50 rounded-lg text-red-300 text-sm">
       ⚠️ Masa ini telah ditempah. Sila pilih masa lain.
      </div>
      <div id="non-working-day-warning" class="hidden p-3 bg-amber-500/20 border border-amber-500/50 rounded-lg text-amber-300 text-sm">
       ⚠�� Tarikh ini adalah hujung minggu atau cuti umum. Sila pilih hari bekerja.
      </div>
      <div class="flex gap-3 pt-2"><button type="button" id="cancel-btn" class="flex-1 px-4 py-2.5 bg-slate-700 hover:bg-slate-600 rounded-lg font-medium transition-colors"> Batal </button> <button type="submit" id="submit-btn" class="flex-1 px-4 py-2.5 bg-gradient-to-r from-indigo-600 to-purple-600 hover:from-indigo-500 hover:to-purple-500 rounded-lg font-medium transition-all disabled:opacity-50 disabled:cursor-not-allowed"> <span id="submit-text">Tempah Sekarang</span> <span id="submit-loading" class="hidden">Menempah...</span> </button>
      </div>
     </form>
    </div>
   </div><!-- Bookings List Modal -->
   <div id="bookings-modal" class="hidden fixed inset-0 z-50 flex items-center justify-center p-4">
    <div class="modal-overlay absolute inset-0 bg-black/60 backdrop-blur-sm" id="bookings-modal-backdrop"></div>
    <div class="relative bg-slate-800 rounded-2xl shadow-2xl w-full max-w-2xl max-h-[80%] border border-slate-700 overflow-hidden flex flex-col">
     <div class="bg-gradient-to-r from-indigo-600 to-purple-600 px-6 py-4 flex items-center justify-between">
      <div>
       <h3 class="text-lg font-semibold text-white">Senarai Tempahan</h3>
       <p class="text-indigo-200 text-sm">Urus semua tempahan anda</p>
      </div><button id="close-bookings-btn" class="p-2 hover:bg-white/20 rounded-lg transition-colors">
       <svg class="w-5 h-5" fill="none" stroke="currentColor" viewbox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
       </svg></button>
     </div><!-- Filters -->
     <div class="px-6 py-4 border-b border-slate-700 space-y-4">
      <div><label for="filter-date" class="block text-sm font-medium text-slate-300 mb-2">Tapis mengikut tarikh:</label>
       <div class="flex gap-2"><input type="date" id="filter-date" class="flex-1 px-4 py-2 bg-slate-700 border border-slate-600 rounded-lg text-white focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent"> <button id="clear-filter-btn" class="px-4 py-2 bg-slate-700 hover:bg-slate-600 rounded-lg text-sm font-medium transition-colors"> Padam Tapisan </button>
       </div>
      </div>
      <div><label class="block text-sm font-medium text-slate-300 mb-2">Tapis mengikut lokasi:</label>
       <div class="flex flex-wrap gap-2" id="room-filter-buttons"><!-- Room filter buttons will be rendered here -->
       </div>
      </div>
     </div>
     <div id="bookings-list" class="p-4 overflow-y-auto flex-1"><!-- Bookings will be rendered here -->
     </div>
    </div>
   </div><!-- Delete Confirmation -->
   <div id="delete-confirm" class="hidden fixed inset-0 z-50 flex items-center justify-center p-4">
    <div class="absolute inset-0 bg-black/60 backdrop-blur-sm"></div>
    <div class="relative bg-slate-800 rounded-2xl shadow-2xl w-full max-w-sm border border-slate-700 p-6">
     <div class="w-16 h-16 mx-auto mb-4 rounded-full bg-red-500/20 flex items-center justify-center">
      <svg class="w-8 h-8 text-red-400" fill="none" stroke="currentColor" viewbox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
      </svg>
     </div>
     <h3 class="text-lg font-semibold text-white mb-2 text-center">Pengesahan Pemadaman</h3>
     <p class="text-slate-400 text-sm mb-4 text-center">Masukkan password penempah atau kod admin untuk meneruskan</p>
     <div class="mb-4"><label for="delete-verification" class="block text-sm font-medium text-slate-300 mb-1">Password Penempah / Kod Admin</label> <input type="password" id="delete-verification" class="w-full px-4 py-2.5 bg-slate-700 border border-slate-600 rounded-lg text-white placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent" placeholder="Masukkan password atau kod admin">
     </div>
     <div id="delete-error" class="hidden mb-4 p-3 bg-red-500/20 border border-red-500/50 rounded-lg text-red-300 text-sm text-center">
      Password tidak sepadan atau kod admin salah
     </div>
     <div class="flex gap-3"><button id="cancel-delete-btn" class="flex-1 px-4 py-2.5 bg-slate-700 hover:bg-slate-600 rounded-lg font-medium transition-colors"> Batal </button> <button id="confirm-delete-btn" class="flex-1 px-4 py-2.5 bg-red-600 hover:bg-red-500 rounded-lg font-medium transition-colors"> <span id="delete-text">Padam</span> <span id="delete-loading" class="hidden">Memadam...</span> </button>
     </div>
    </div>
   </div><!-- Toast -->
   <div id="toast" class="hidden fixed bottom-4 right-4 z-50 px-4 py-3 rounded-lg shadow-lg flex items-center gap-2 max-w-sm"><span id="toast-message"></span>
   </div>
  </div>
  <script>
    // Reference data for rooms
    const ROOMS = [
      { id: 'DK2', name: 'Dewan Kuliah 2', capacity: 200, floor: 'Tingkat Bawah', facilities: ['Projektor', 'AC', 'Mic'] },
      { id: 'BPJKE', name: 'Bilik Perbincangan JKE', capacity: 10, floor: 'Tingkat Bawah (Depan Bilik KJKE)', facilities: ['Projektor', 'AC', 'Mic'] },
      { id: 'BMJKE', name: 'Bilik Mesyuarat JKE', capacity: 50, floor: 'Tingkat 1 Blok B', facilities: ['Projektor', 'AC', 'Mic'] },
      { id: 'A201', name: 'Bilik Kuliah A201', capacity: 40, floor: 'Tingkat 2', facilities: ['Projektor', 'AC', 'Papan Putih'] },
      { id: 'A202', name: 'Bilik Kuliah A202', capacity: 40, floor: 'Tingkat 2', facilities: ['Projektor', 'AC', 'Papan Putih'] },
      { id: 'A301', name: 'Bilik Kuliah A301', capacity: 40, floor: 'Tingkat 3', facilities: ['Papan Putih'] },
      { id: 'A302', name: 'Bilik Kuliah A302', capacity: 40, floor: 'Tingkat 3', facilities: ['Papan Putih'] },
      { id: 'A303', name: 'Bilik Kuliah A303', capacity: 40, floor: 'Tingkat 3', facilities: ['Papan Putih'] },
      { id: 'A304', name: 'Bilik Kuliah A304', capacity: 40, floor: 'Tingkat 3', facilities: ['Papan Putih'] },
      { id: 'C203', name: 'Bilik Kuliah C203', capacity: 40, floor: 'Tingkat 2', facilities: ['Projektor', 'AC', 'Papan Putih'] }
    ];

    // Pahang public holidays and non-working days for 2024-2025
    const PAHANG_HOLIDAYS = [
      // 2024
      '2024-01-01', // New Year
      '2024-01-25', // Thaipusam
      '2024-02-01', // Federal Territory Day
      '2024-02-10', // Chinese New Year
      '2024-02-11', // Chinese New Year
      '2024-02-12', // Chinese New Year (replacement)
      '2024-03-28', // Nuzul Al-Quran
      '2024-04-10', // Hari Raya Aidilfitri
      '2024-04-11', // Hari Raya Aidilfitri
      '2024-05-01', // Labour Day
      '2024-05-07', // Hari Hol Pahang (Birthday of Sultan of Pahang)
      '2024-05-22', // Wesak Day
      '2024-06-03', // Agong's Birthday
      '2024-06-17', // Hari Raya Aidiladha
      '2024-07-07', // Awal Muharram
      '2024-08-31', // National Day
      '2024-09-16', // Malaysia Day
      '2024-09-16', // Maulidur Rasul
      '2024-10-24', // Deepavali
      '2024-12-25', // Christmas
      
      // 2025
      '2025-01-01', // New Year
      '2025-01-13', // Thaipusam
      '2025-01-29', // Chinese New Year
      '2025-01-30', // Chinese New Year
      '2025-03-17', // Nuzul Al-Quran
      '2025-03-31', // Hari Raya Aidilfitri
      '2025-04-01', // Hari Raya Aidilfitri
      '2025-05-01', // Labour Day
      '2025-05-12', // Wesak Day
      '2025-05-22', // Hari Hol Pahang (Birthday of Sultan of Pahang)
      '2025-06-02', // Agong's Birthday
      '2025-06-07', // Hari Raya Aidiladha
      '2025-06-27', // Awal Muharram
      '2025-08-31', // National Day
      '2025-09-05', // Maulidur Rasul
      '2025-09-16', // Malaysia Day
      '2025-10-20', // Deepavali
      '2025-12-25', // Christmas
    ];

    function isWorkingDay(dateString) {
      const date = new Date(dateString + 'T00:00:00');
      const dayOfWeek = date.getDay();
      
      // Check if weekend (Saturday = 6, Sunday = 0)
      if (dayOfWeek === 0 || dayOfWeek === 6) {
        return false;
      }
      
      // Check if public holiday
      if (PAHANG_HOLIDAYS.includes(dateString)) {
        return false;
      }
      
      return true;
    }

    function getNextWorkingDay(fromDate) {
      let checkDate = new Date(fromDate);
      checkDate.setDate(checkDate.getDate() + 1);
      
      // Check up to 30 days ahead to find next working day
      for (let i = 0; i < 30; i++) {
        const dateString = checkDate.toISOString().split('T')[0];
        if (isWorkingDay(dateString)) {
          return dateString;
        }
        checkDate.setDate(checkDate.getDate() + 1);
      }
      
      return null;
    }

    // App state
    let allBookings = [];
    let deleteTarget = null;
    let currentRecordCount = 0;
    let filterDate = null; // For date filtering
    let filterRoom = null; // For room filtering

    // Default config
    const defaultConfig = {
      system_title: 'Sistem Booking Bilik Kuliah / Makmal / Bilik Mesyuarat JKE',
      subtitle: 'JKE POLISAS',
      background_color: '#0f172a',
      surface_color: '#1e293b',
      text_color: '#f8fafc',
      primary_action_color: '#6366f1',
      secondary_action_color: '#64748b',
      font_family: 'Plus Jakarta Sans',
      font_size: 16
    };

    let config = { ...defaultConfig };

    // Element SDK integration
    const elementApi = {
      defaultConfig,
      onConfigChange: async (newConfig) => {
        config = { ...defaultConfig, ...newConfig };
        applyConfig();
      },
      mapToCapabilities: (cfg) => ({
        recolorables: [
          {
            get: () => cfg.background_color || defaultConfig.background_color,
            set: (v) => { cfg.background_color = v; window.elementSdk.setConfig({ background_color: v }); }
          },
          {
            get: () => cfg.surface_color || defaultConfig.surface_color,
            set: (v) => { cfg.surface_color = v; window.elementSdk.setConfig({ surface_color: v }); }
          },
          {
            get: () => cfg.text_color || defaultConfig.text_color,
            set: (v) => { cfg.text_color = v; window.elementSdk.setConfig({ text_color: v }); }
          },
          {
            get: () => cfg.primary_action_color || defaultConfig.primary_action_color,
            set: (v) => { cfg.primary_action_color = v; window.elementSdk.setConfig({ primary_action_color: v }); }
          },
          {
            get: () => cfg.secondary_action_color || defaultConfig.secondary_action_color,
            set: (v) => { cfg.secondary_action_color = v; window.elementSdk.setConfig({ secondary_action_color: v }); }
          }
        ],
        borderables: [],
        fontEditable: {
          get: () => cfg.font_family || defaultConfig.font_family,
          set: (v) => { cfg.font_family = v; window.elementSdk.setConfig({ font_family: v }); }
        },
        fontSizeable: {
          get: () => cfg.font_size || defaultConfig.font_size,
          set: (v) => { cfg.font_size = v; window.elementSdk.setConfig({ font_size: v }); }
        }
      }),
      mapToEditPanelValues: (cfg) => new Map([
        ['system_title', cfg.system_title || defaultConfig.system_title],
        ['subtitle', cfg.subtitle || defaultConfig.subtitle]
      ])
    };

    function applyConfig() {
      const bgColor = config.background_color || defaultConfig.background_color;
      const surfaceColor = config.surface_color || defaultConfig.surface_color;
      const textColor = config.text_color || defaultConfig.text_color;
      const primaryColor = config.primary_action_color || defaultConfig.primary_action_color;
      const secondaryColor = config.secondary_action_color || defaultConfig.secondary_action_color;
      const fontFamily = config.font_family || defaultConfig.font_family;
      const fontSize = config.font_size || defaultConfig.font_size;

      // Apply colors
      document.body.style.backgroundColor = bgColor;
      document.getElementById('app-wrapper').style.background = `linear-gradient(to bottom right, ${bgColor}, ${adjustColor(bgColor, 20)}, ${adjustColor(primaryColor, -60)})`;
      
      document.querySelectorAll('.bg-slate-800, .bg-slate-800\\/50').forEach(el => {
        el.style.backgroundColor = surfaceColor;
      });

      document.querySelectorAll('.bg-slate-700').forEach(el => {
        el.style.backgroundColor = adjustColor(surfaceColor, 10);
      });

      document.body.style.color = textColor;
      document.querySelectorAll('.text-white, .text-slate-300, .text-slate-400').forEach(el => {
        el.style.color = textColor;
      });

      // Primary actions
      document.querySelectorAll('.bg-gradient-to-r.from-indigo-600').forEach(el => {
        el.style.background = `linear-gradient(to right, ${primaryColor}, ${adjustColor(primaryColor, 20)})`;
      });

      // Secondary actions
      document.getElementById('cancel-btn').style.backgroundColor = secondaryColor;
      document.getElementById('cancel-delete-btn').style.backgroundColor = secondaryColor;

      // Titles
      document.getElementById('main-title').textContent = config.system_title || defaultConfig.system_title;
      document.getElementById('subtitle').textContent = config.subtitle || defaultConfig.subtitle;

      // Font
      document.body.style.fontFamily = `${fontFamily}, Plus Jakarta Sans, sans-serif`;
      document.getElementById('main-title').style.fontSize = `${fontSize * 1.5}px`;
      document.getElementById('subtitle').style.fontSize = `${fontSize * 0.875}px`;

      renderRooms();
      renderBookingsList();
    }

    function adjustColor(hex, percent) {
      const num = parseInt(hex.replace('#', ''), 16);
      const amt = Math.round(2.55 * percent);
      const R = Math.max(0, Math.min(255, (num >> 16) + amt));
      const G = Math.max(0, Math.min(255, ((num >> 8) & 0x00FF) + amt));
      const B = Math.max(0, Math.min(255, (num & 0x0000FF) + amt));
      return `#${(0x1000000 + R * 0x10000 + G * 0x100 + B).toString(16).slice(1)}`;
    }

    // Data SDK integration
    const dataHandler = {
      onDataChanged(data) {
        allBookings = data;
        currentRecordCount = data.length;
        
        // Count only active bookings (future and ongoing)
        const now = new Date();
        const today = now.toISOString().split('T')[0];
        const currentTime = now.toTimeString().slice(0, 5);
        
        const activeCount = data.filter(b => {
          if (b.date > today) return true;
          if (b.date === today && b.end_time > currentTime) return true;
          return false;
        }).length;
        
        document.getElementById('booking-count').textContent = `${activeCount} tempahan aktif`;
        renderRooms();
        renderBookingsList();
      }
    };

    function getTodayBookingsForRoom(roomId) {
      const now = new Date();
      const today = now.toISOString().split('T')[0];
      const currentTime = now.toTimeString().slice(0, 5); // Format HH:MM
      
      // Calculate tomorrow's date
      const tomorrow = new Date(now);
      tomorrow.setDate(tomorrow.getDate() + 1);
      const tomorrowDate = tomorrow.toISOString().split('T')[0];
      
      return allBookings.filter(b => {
        if (b.room_id !== roomId) return false;
        
        // Only show today's bookings (that haven't ended yet)
        if (b.date === today && b.end_time > currentTime) return true;
        
        // Only show tomorrow's bookings
        if (b.date === tomorrowDate) return true;
        
        return false;
      });
    }

    function renderRooms() {
      const grid = document.getElementById('rooms-grid');
      const surfaceColor = config.surface_color || defaultConfig.surface_color;
      const primaryColor = config.primary_action_color || defaultConfig.primary_action_color;
      const textColor = config.text_color || defaultConfig.text_color;

      grid.innerHTML = ROOMS.map(room => {
        const todayBookings = getTodayBookingsForRoom(room.id);
        const hasBookings = todayBookings.length > 0;

        return `
          <div class="room-card rounded-xl p-5 cursor-pointer border border-slate-700/50 hover:border-indigo-500/50"
               style="background-color: ${surfaceColor}; color: ${textColor};"
               onclick="openBookingModal('${room.id}', '${room.name}')">
            <div class="flex items-start justify-between mb-3">
              <div class="p-2 rounded-lg" style="background-color: ${adjustColor(primaryColor, -40)};">
                <svg class="w-6 h-6" style="color: ${primaryColor};" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4"/>
                </svg>
              </div>
              <span class="px-2 py-1 rounded-full text-xs font-medium text-white ${hasBookings ? 'status-booked' : 'status-available'}">
                ${hasBookings ? todayBookings.length + ' tempahan' : 'Tersedia'}
              </span>
            </div>
            <h3 class="font-semibold text-lg mb-1">${room.name}</h3>
            <p class="text-sm opacity-70 mb-3">${room.id} �� ${room.floor}</p>
            <div class="flex items-center gap-4 text-sm opacity-70">
              <span class="flex items-center gap-1">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z"/>
                </svg>
                ${room.capacity} orang
              </span>
            </div>
            <div class="flex flex-wrap gap-1 mt-3">
              ${room.facilities.map(f => `<span class="px-2 py-0.5 rounded text-xs" style="background-color: ${adjustColor(surfaceColor, 15)};">${f}</span>`).join('')}
            </div>
            ${todayBookings.length > 0 ? `
              <div class="mt-4 pt-4 border-t border-slate-600/50">
                <p class="text-xs font-medium opacity-70 mb-2">Tempahan Akan Datang:</p>
                <div class="space-y-2 max-h-32 overflow-y-auto">
                  ${todayBookings.map(booking => `
                    <div class="text-xs p-2 rounded" style="background-color: ${adjustColor(surfaceColor, 10)};">
                      <div class="font-medium">${booking.booker_name}</div>
                      <div class="opacity-70 mt-0.5">${formatDate(booking.date)}</div>
                      <div class="opacity-70">${booking.start_time} - ${booking.end_time} • ${booking.purpose}</div>
                    </div>
                  `).join('')}
                </div>
              </div>
            ` : ''}
          </div>
        `;
      }).join('');
    }

    function renderRoomFilterButtons() {
      const container = document.getElementById('room-filter-buttons');
      const primaryColor = config.primary_action_color || defaultConfig.primary_action_color;
      const surfaceColor = config.surface_color || defaultConfig.surface_color;
      
      const buttons = ROOMS.map(room => {
        const isActive = filterRoom === room.id;
        return `
          <button onclick="setRoomFilter('${room.id}')" 
                  class="px-3 py-1.5 rounded-lg text-sm font-medium transition-all ${isActive ? 'ring-2 ring-offset-2 ring-offset-slate-800' : ''}"
                  style="background-color: ${isActive ? primaryColor : adjustColor(surfaceColor, 10)}; color: #fff; ${isActive ? 'ring-color: ' + primaryColor : ''}">
            ${room.id}
          </button>
        `;
      }).join('');
      
      const clearButton = `
        <button onclick="clearRoomFilter()" 
                class="px-3 py-1.5 rounded-lg text-sm font-medium transition-all"
                style="background-color: ${adjustColor(surfaceColor, 10)}; color: #fff;">
          Semua Lokasi
        </button>
      `;
      
      container.innerHTML = clearButton + buttons;
    }

    function setRoomFilter(roomId) {
      filterRoom = roomId;
      renderRoomFilterButtons();
      renderBookingsList();
    }

    function clearRoomFilter() {
      filterRoom = null;
      renderRoomFilterButtons();
      renderBookingsList();
    }

    function renderBookingsList() {
      const list = document.getElementById('bookings-list');
      const surfaceColor = config.surface_color || defaultConfig.surface_color;
      const textColor = config.text_color || defaultConfig.text_color;
      const primaryColor = config.primary_action_color || defaultConfig.primary_action_color;

      // Filter out past bookings
      const now = new Date();
      const today = now.toISOString().split('T')[0];
      const currentTime = now.toTimeString().slice(0, 5);
      
      let activeBookings = allBookings.filter(b => {
        // Show future dates
        if (b.date > today) return true;
        
        // For today, only show bookings that haven't ended yet
        if (b.date === today && b.end_time > currentTime) return true;
        
        return false;
      });

      // Apply date filter if set
      if (filterDate) {
        activeBookings = activeBookings.filter(b => b.date === filterDate);
      }

      // Apply room filter if set
      if (filterRoom) {
        activeBookings = activeBookings.filter(b => b.room_id === filterRoom);
      }

      if (activeBookings.length === 0) {
        const message = filterDate 
          ? `Tiada tempahan pada ${formatDate(filterDate)}`
          : 'Tiada tempahan aktif';
        const submessage = filterDate
          ? 'Cuba pilih tarikh lain atau padam tapisan'
          : 'Pilih bilik untuk membuat tempahan baru';
        
        list.innerHTML = `
          <div class="text-center py-12" style="color: ${textColor}; opacity: 0.7;">
            <svg class="w-16 h-16 mx-auto mb-4 opacity-50" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/>
            </svg>
            <p class="font-medium">${message}</p>
            <p class="text-sm opacity-70">${submessage}</p>
          </div>
        `;
        return;
      }

      const sortedBookings = [...activeBookings].sort((a, b) => {
        const dateCompare = a.date.localeCompare(b.date);
        if (dateCompare !== 0) return dateCompare;
        return a.start_time.localeCompare(b.start_time);
      });

      // Group bookings by date for better visualization
      const bookingsByDate = {};
      sortedBookings.forEach(booking => {
        if (!bookingsByDate[booking.date]) {
          bookingsByDate[booking.date] = [];
        }
        bookingsByDate[booking.date].push(booking);
      });

      list.innerHTML = Object.keys(bookingsByDate).sort().map(date => {
        const dateBookings = bookingsByDate[date];
        return `
          <div class="mb-6">
            <div class="flex items-center gap-2 mb-3 pb-2 border-b border-slate-700/50">
              <svg class="w-5 h-5 opacity-70" style="color: ${primaryColor};" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/>
              </svg>
              <h4 class="font-semibold" style="color: ${textColor};">${formatDate(date)}</h4>
              <span class="text-xs px-2 py-0.5 rounded" style="background-color: ${primaryColor}; opacity: 0.8;">${dateBookings.length} tempahan</span>
            </div>
            <div class="space-y-3">
              ${dateBookings.map(booking => `
                <div class="booking-item rounded-lg p-4 border border-slate-700/50" style="background-color: ${adjustColor(surfaceColor, 5)}; color: ${textColor};">
                  <div class="flex items-start justify-between">
                    <div class="flex-1">
                      <div class="flex items-center gap-2 mb-1">
                        <span class="font-semibold">${booking.room_name}</span>
                        <span class="text-xs px-2 py-0.5 rounded" style="background-color: ${primaryColor};">${booking.room_id}</span>
                      </div>
                      <p class="text-sm opacity-70 mb-2">${booking.purpose}</p>
                      <div class="flex flex-wrap gap-3 text-sm opacity-80">
                        <span class="flex items-center gap-1">
                          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"/>
                          </svg>
                          ${booking.booker_name}
                        </span>
                        <span class="flex items-center gap-1">
                          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"/>
                          </svg>
                          ${booking.start_time} - ${booking.end_time}
                        </span>
                      </div>
                    </div>
                    <button onclick="showDeleteConfirm('${booking.__backendId}')" 
                            class="p-2 hover:bg-red-500/20 rounded-lg transition-colors text-red-400 hover:text-red-300">
                      <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"/>
                      </svg>
                    </button>
                  </div>
                </div>
              `).join('')}
            </div>
          </div>
        `;
      }).join('');
    }

    function formatDate(dateStr) {
      const date = new Date(dateStr);
      return date.toLocaleDateString('ms-MY', { weekday: 'short', day: 'numeric', month: 'short', year: 'numeric' });
    }

    function openBookingModal(roomId, roomName) {
      document.getElementById('selected-room-id').value = roomId;
      document.getElementById('selected-room-name').value = roomName;
      document.getElementById('modal-room-name').textContent = roomName + ' (' + roomId + ')';
      document.getElementById('booking-form').reset();
      document.getElementById('selected-room-id').value = roomId;
      document.getElementById('selected-room-name').value = roomName;
      
      // Set minimum date to next working day
      const today = new Date();
      const nextWorkingDay = getNextWorkingDay(today);
      if (nextWorkingDay) {
        document.getElementById('booking-date').min = nextWorkingDay;
      }
      
      document.getElementById('time-conflict-warning').classList.add('hidden');
      document.getElementById('booking-modal').classList.remove('hidden');
    }

    function closeBookingModal() {
      document.getElementById('booking-modal').classList.add('hidden');
    }

    function openBookingsModal() {
      renderRoomFilterButtons();
      document.getElementById('bookings-modal').classList.remove('hidden');
    }

    function closeBookingsModal() {
      document.getElementById('bookings-modal').classList.add('hidden');
    }

    function showDeleteConfirm(bookingId) {
      deleteTarget = allBookings.find(b => b.__backendId === bookingId);
      document.getElementById('delete-verification').value = '';
      document.getElementById('delete-error').classList.add('hidden');
      document.getElementById('delete-confirm').classList.remove('hidden');
    }

    function hideDeleteConfirm() {
      deleteTarget = null;
      document.getElementById('delete-verification').value = '';
      document.getElementById('delete-error').classList.add('hidden');
      document.getElementById('delete-confirm').classList.add('hidden');
    }

    function verifyDeletePermission() {
      if (!deleteTarget) return false;
      
      const input = document.getElementById('delete-verification').value.trim();
      const ADMIN_CODE = '123456';
      
      // Check if input matches admin code
      if (input === ADMIN_CODE) {
        return true;
      }
      
      // Check if input matches booker password
      if (deleteTarget.booker_password && input === deleteTarget.booker_password) {
        return true;
      }
      
      return false;
    }

    function checkTimeConflict(roomId, date, startTime, endTime) {
      return allBookings.some(booking => {
        if (booking.room_id !== roomId || booking.date !== date) return false;
        const existingStart = booking.start_time;
        const existingEnd = booking.end_time;
        return (startTime < existingEnd && endTime > existingStart);
      });
    }

    function showToast(message, type = 'success') {
      const toast = document.getElementById('toast');
      const toastMessage = document.getElementById('toast-message');
      toastMessage.textContent = message;
      toast.className = `fixed bottom-4 right-4 z-50 px-4 py-3 rounded-lg shadow-lg flex items-center gap-2 max-w-sm ${type === 'success' ? 'bg-green-600' : 'bg-red-600'} text-white`;
      toast.classList.remove('hidden');
      setTimeout(() => toast.classList.add('hidden'), 3000);
    }

    function setSubmitLoading(loading) {
      document.getElementById('submit-text').classList.toggle('hidden', loading);
      document.getElementById('submit-loading').classList.toggle('hidden', !loading);
      document.getElementById('submit-btn').disabled = loading;
    }

    function setDeleteLoading(loading) {
      document.getElementById('delete-text').classList.toggle('hidden', loading);
      document.getElementById('delete-loading').classList.toggle('hidden', !loading);
      document.getElementById('confirm-delete-btn').disabled = loading;
      document.getElementById('cancel-delete-btn').disabled = loading;
    }

    // Event listeners
    document.getElementById('modal-backdrop').addEventListener('click', closeBookingModal);
    document.getElementById('cancel-btn').addEventListener('click', closeBookingModal);
    document.getElementById('view-bookings-btn').addEventListener('click', openBookingsModal);
    document.getElementById('bookings-modal-backdrop').addEventListener('click', closeBookingsModal);
    document.getElementById('close-bookings-btn').addEventListener('click', closeBookingsModal);
    document.getElementById('cancel-delete-btn').addEventListener('click', hideDeleteConfirm);

    // Date filter listeners
    document.getElementById('filter-date').addEventListener('change', (e) => {
      filterDate = e.target.value || null;
      renderBookingsList();
    });

    document.getElementById('clear-filter-btn').addEventListener('click', () => {
      filterDate = null;
      filterRoom = null;
      document.getElementById('filter-date').value = '';
      renderRoomFilterButtons();
      renderBookingsList();
    });

    document.getElementById('booking-form').addEventListener('submit', async (e) => {
      e.preventDefault();
      
      if (currentRecordCount >= 999) {
        showToast('Had maksimum 999 tempahan telah dicapai. Sila padam tempahan lama.', 'error');
        return;
      }

      const roomId = document.getElementById('selected-room-id').value;
      const roomName = document.getElementById('selected-room-name').value;
      const bookerName = document.getElementById('booker-name').value.trim();
      const bookerPassword = document.getElementById('booker-password').value.trim();
      const purpose = document.getElementById('purpose').value.trim();
      const date = document.getElementById('booking-date').value;
      const startTime = document.getElementById('start-time').value;
      const endTime = document.getElementById('end-time').value;

      // Validate it's a working day
      if (!isWorkingDay(date)) {
        showToast('Tempahan hanya dibenarkan pada hari bekerja (Isnin-Jumaat, kecuali cuti umum)', 'error');
        document.getElementById('non-working-day-warning').classList.remove('hidden');
        return;
      }

      // Validate booking date is at least 1 day in advance
      const selectedDate = new Date(date);
      const tomorrow = new Date();
      tomorrow.setDate(tomorrow.getDate() + 1);
      tomorrow.setHours(0, 0, 0, 0);
      selectedDate.setHours(0, 0, 0, 0);
      
      if (selectedDate < tomorrow) {
        showToast('Tempahan mesti dibuat sekurang-kurangnya sehari sebelum penggunaan', 'error');
        return;
      }

      if (startTime >= endTime) {
        showToast('Masa tamat mesti selepas masa mula', 'error');
        return;
      }

      if (checkTimeConflict(roomId, date, startTime, endTime)) {
        document.getElementById('time-conflict-warning').classList.remove('hidden');
        return;
      }

      setSubmitLoading(true);

      const result = await window.dataSdk.create({
        room_id: roomId,
        room_name: roomName,
        booker_name: bookerName,
        booker_password: bookerPassword,
        purpose: purpose,
        date: date,
        start_time: startTime,
        end_time: endTime,
        created_at: new Date().toISOString()
      });

      setSubmitLoading(false);

      if (result.isOk) {
        closeBookingModal();
        showToast('Tempahan berjaya disimpan!');
      } else {
        showToast('Gagal menyimpan tempahan. Sila cuba lagi.', 'error');
      }
    });

    document.getElementById('confirm-delete-btn').addEventListener('click', async () => {
      if (!deleteTarget) return;

      // Verify permission first
      if (!verifyDeletePermission()) {
        document.getElementById('delete-error').classList.remove('hidden');
        return;
      }

      document.getElementById('delete-error').classList.add('hidden');
      setDeleteLoading(true);

      const result = await window.dataSdk.delete(deleteTarget);

      setDeleteLoading(false);

      if (result.isOk) {
        hideDeleteConfirm();
        showToast('Tempahan berjaya dipadam!');
      } else {
        showToast('Gagal memadam tempahan. Sila cuba lagi.', 'error');
      }
    });

    // Check for time conflicts when inputs change
    ['booking-date', 'start-time', 'end-time'].forEach(id => {
      document.getElementById(id).addEventListener('change', () => {
        const roomId = document.getElementById('selected-room-id').value;
        const date = document.getElementById('booking-date').value;
        const startTime = document.getElementById('start-time').value;
        const endTime = document.getElementById('end-time').value;
        
        // Check if it's a working day
        if (date) {
          if (!isWorkingDay(date)) {
            document.getElementById('non-working-day-warning').classList.remove('hidden');
          } else {
            document.getElementById('non-working-day-warning').classList.add('hidden');
          }
        }
        
        if (roomId && date && startTime && endTime) {
          const hasConflict = checkTimeConflict(roomId, date, startTime, endTime);
          document.getElementById('time-conflict-warning').classList.toggle('hidden', !hasConflict);
        }
      });
    });

    // Initialize
    async function init() {
      if (window.elementSdk) {
        window.elementSdk.init(elementApi);
      }

      if (window.dataSdk) {
        const result = await window.dataSdk.init(dataHandler);
        if (!result.isOk) {
          console.error('Failed to initialize Data SDK');
        }
      }

      renderRooms();
      
      // Auto-refresh display every minute to hide past bookings in real-time
      setInterval(() => {
        renderRooms();
        renderBookingsList();
        
        // Update active booking count
        const now = new Date();
        const today = now.toISOString().split('T')[0];
        const currentTime = now.toTimeString().slice(0, 5);
        
        const activeCount = allBookings.filter(b => {
          if (b.date > today) return true;
          if (b.date === today && b.end_time > currentTime) return true;
          return false;
        }).length;
        
        document.getElementById('booking-count').textContent = `${activeCount} tempahan aktif`;
      }, 60000); // Refresh every 60 seconds
    }

    init();
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9c0aadc9f4f94748',t:'MTc2ODg3MTI4Ny4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
