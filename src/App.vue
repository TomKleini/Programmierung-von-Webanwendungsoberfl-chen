<script setup>
import { ref, computed } from 'vue'

// --- i18n: SPRACHE & SCHRIFTKULTUR (Anforderung b.c) ---
const isRtl = ref(false)

// Unser kleines Wörterbuch für Deutsch und Arabisch
const translations = {
  de: {
    title: 'Climate Action Tracker',
    navHome: 'Startseite',
    navData: 'Datenbank',
    navAbout: 'Über uns',
    btnLang: '🌐 LTR (Deutsch)',
    filterTitle: 'Filter-Menü',
    filterCountries: 'Nach Ländern',
    filterCompanies: 'Nach Unternehmen',
    filterMethod: 'Methodik & Quellen',
    contentTitle: 'Transparenz für unser Klima',
    contentDesc: 'Willkommen beim offiziellen Portal. Hier entsteht in Kürze unsere interaktive Übersicht der weltweiten CO2-Emissionen.',
    footerRights: '© 2026 Non-Profit Climate Action. Alle Rechte vorbehalten.',
    footerImprint: 'Impressum',
    footerPrivacy: 'Datenschutz',
    footerTerms: 'Haftungsausschluss'
  },
  ar: {
    title: 'متتبع العمل المناخي',
    navHome: 'الصفحة الرئيسية',
    navData: 'قاعدة البيانات',
    navAbout: 'معلومات عنا',
    btnLang: '🌐 RTL (العربية)',
    filterTitle: 'قائمة التصفية',
    filterCountries: 'حسب البلدان',
    filterCompanies: 'حسب الشركات',
    filterMethod: 'المنهجية والمصادر',
    contentTitle: 'الشفافية من أجل مناخنا',
    contentDesc: 'مرحبًا بك في البوابة الرسمية. ستجد هنا قريبًا نظرة عامة تفاعلية على انبعاثات ثاني أكسيد الكربون العالمية.',
    footerRights: '© 2026 المنظمة غير الربحية للعمل المناخي. جميع الحقوق محفوظة.',
    footerImprint: 'بصمة',
    footerPrivacy: 'حماية البيانات',
    footerTerms: 'تنصل'
  }
}

// Eine "computed property" gibt uns immer das richtige Wörterbuch zurück
const t = computed(() => isRtl.value ? translations.ar : translations.de)

const toggleCulture = () => {
  isRtl.value = !isRtl.value
}

// --- FILTER-LOGIK (Vorbereitung für Anforderung b.e) ---
// Wir speichern den aktiven Filter-Status ('countries', 'companies' oder 'method')
const activeFilter = ref('countries')
</script>

<template>
  <div class="min-h-screen flex flex-col bg-gray-50 text-gray-900 font-sans" :dir="isRtl ? 'rtl' : 'ltr'">
    
    <!-- HEADER -->
    <header class="bg-green-700 text-white shadow-md">
      <div class="container mx-auto px-4 py-4 flex flex-col md:flex-row justify-between items-center gap-4">
        
        <div class="flex items-center space-x-3 rtl:space-x-reverse">
          <span class="text-3xl" aria-hidden="true">🌍</span>
          <h1 class="text-2xl font-bold tracking-tight">{{ t.title }}</h1>
        </div>
        
        <nav class="flex items-center gap-6">
          <ul class="flex space-x-6 rtl:space-x-reverse font-medium">
            <li><a href="#" class="hover:text-green-200 transition-colors">{{ t.navHome }}</a></li>
            <li><a href="#" class="hover:text-green-200 transition-colors">{{ t.navData }}</a></li>
            <li><a href="#" class="hover:text-green-200 transition-colors">{{ t.navAbout }}</a></li>
          </ul>
          
          <button 
            @click="toggleCulture" 
            class="ml-4 rtl:mr-4 rtl:ml-0 bg-green-800 px-3 py-1.5 rounded text-sm hover:bg-green-600 transition-colors border border-green-600"
          >
            {{ t.btnLang }}
          </button>
        </nav>
      </div>
    </header>

    <!-- CONTENT-BEREICH -->
    <main class="flex-grow container mx-auto px-4 py-8 w-full flex flex-col md:flex-row gap-6 md:items-start">
      
      <!-- LOKALES MENÜ -->
      <aside class="w-full md:w-64 shrink-0 bg-white p-5 rounded-xl shadow-sm border border-gray-100">
        <h3 class="font-bold text-lg mb-4 text-green-800 border-b pb-2">{{ t.filterTitle }}</h3>
        <ul class="space-y-2">
          <li>
            <!-- Dynamische Klassen mit Vue: Wenn aktiv, dann grüner Hintergrund -->
            <button 
              @click="activeFilter = 'countries'"
              class="w-full text-left rtl:text-right p-2 rounded transition-colors"
              :class="activeFilter === 'countries' ? 'bg-green-50 text-green-700 font-medium' : 'hover:bg-gray-50 text-gray-600'"
            >
              {{ t.filterCountries }}
            </button>
          </li>
          <li>
            <button 
              @click="activeFilter = 'companies'"
              class="w-full text-left rtl:text-right p-2 rounded transition-colors"
              :class="activeFilter === 'companies' ? 'bg-green-50 text-green-700 font-medium' : 'hover:bg-gray-50 text-gray-600'"
            >
              {{ t.filterCompanies }}
            </button>
          </li>
          <li>
            <button 
              @click="activeFilter = 'method'"
              class="w-full text-left rtl:text-right p-2 rounded transition-colors"
              :class="activeFilter === 'method' ? 'bg-green-50 text-green-700 font-medium' : 'hover:bg-gray-50 text-gray-600'"
            >
              {{ t.filterMethod }}
            </button>
          </li>
        </ul>
      </aside>

      <!-- HAUPTINHALT -->
      <div class="flex-grow bg-white p-6 md:p-8 rounded-xl shadow-sm border border-gray-100 w-full">
        <h2 class="text-2xl font-semibold mb-4 text-green-800">{{ t.contentTitle }}</h2>
        <p class="text-gray-600 leading-relaxed mb-6">
          {{ t.contentDesc }}
        </p>
        
        <!-- Ein kleiner Indikator, was gerade ausgewählt ist -->
        <div class="mb-4 text-sm font-medium text-green-600">
          Gewählter Filter: {{ activeFilter }}
        </div>

        <div class="p-8 border-2 border-dashed border-gray-200 rounded-lg text-center text-gray-400">
          [ Platzhalter für die Datentabelle ]
        </div>
      </div>
      
    </main>

    <!-- FOOTER -->
    <footer class="bg-gray-800 text-gray-300 py-8 mt-auto">
      <div class="container mx-auto px-4 flex flex-col md:flex-row justify-between items-center gap-4 text-sm">
        <p>{{ t.footerRights }}</p>
        <div class="flex space-x-6 rtl:space-x-reverse">
          <a href="#" class="hover:text-white transition-colors">{{ t.footerImprint }}</a>
          <a href="#" class="hover:text-white transition-colors">{{ t.footerPrivacy }}</a>
          <a href="#" class="hover:text-white transition-colors">{{ t.footerTerms }}</a>
        </div>
      </div>
    </footer>

  </div>
</template>