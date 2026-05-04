<script setup>
// Hier folgt im nächsten Schritt die Logik für unser dynamisches Menü
</script>

<template>
  <!-- Wrapper für das Flex-Layout (hält den Footer immer unten) -->
  <div class="min-h-screen flex flex-col bg-gray-50 text-gray-900 font-sans">
    
    <!-- HEADER (Anforderung b.b) -->
    <header class="bg-green-700 text-white shadow-md">
      <!-- Container sorgt für Responsive Design (Anforderung b.d) -->
      <div class="container mx-auto px-4 py-4 flex flex-col md:flex-row justify-between items-center gap-4">
        
        <!-- Logo & Titel (Anforderung b.a) -->
        <div class="flex items-center space-x-3">
          <span class="text-3xl" aria-hidden="true">🌍</span>
          <h1 class="text-2xl font-bold tracking-tight">Climate Action Tracker</h1>
        </div>
        
        <!-- Globale Navigation (Anforderung b.b) -->
        <nav>
          <ul class="flex space-x-6 font-medium">
            <li><a href="#" class="hover:text-green-200 transition-colors">Startseite</a></li>
            <li><a href="#" class="hover:text-green-200 transition-colors">Datenbank</a></li>
            <li><a href="#" class="hover:text-green-200 transition-colors">Über uns</a></li>
          </ul>
        </nav>
      </div>
    </header>

    <!-- CONTENT-BEREICH (Anforderung b.b) -->
    <main class="flex-grow container mx-auto px-4 py-8 w-full">
      <div class="bg-white p-6 md:p-8 rounded-xl shadow-sm border border-gray-100">
        <h2 class="text-2xl font-semibold mb-4 text-green-800">Transparenz für unser Klima</h2>
        <p class="text-gray-600 leading-relaxed mb-6">
          Willkommen beim Climate Action Tracker. Hier entsteht unsere interaktive Übersicht der weltweiten CO2-Emissionen nach Ländern und Unternehmen.
        </p>
        
        <!-- Platzhalter für die zukünftige Tabelle -->
        <div class="p-8 border-2 border-dashed border-gray-200 rounded-lg text-center text-gray-400">
          [ Platzhalter für die Datentabelle ]
        </div>
      </div>
    </main>

    <!-- FOOTER mit rechtlichen Hinweisen (Anforderung b.b) -->
    <footer class="bg-gray-800 text-gray-300 py-8 mt-auto">
      <div class="container mx-auto px-4 flex flex-col md:flex-row justify-between items-center gap-4 text-sm">
        <p>&copy; 2026 Non-Profit Climate Action. Alle Rechte vorbehalten.</p>
        
        <div class="flex space-x-6">
          <a href="#" class="hover:text-white transition-colors">Impressum</a>
          <a href="#" class="hover:text-white transition-colors">Datenschutz</a>
          <a href="#" class="hover:text-white transition-colors">Haftungsausschluss</a>
        </div>
      </div>
    </footer>

  </div>
</template>