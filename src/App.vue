<script setup>
import { ref, computed } from 'vue'

// --- 1. i18n: SPRACHE & SCHRIFTKULTUR ---
const isRtl = ref(false)

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
    contentTitle: 'Emissionsdaten 2026',
    searchPlaceholder: 'Suchen...',
    thCompany: 'Unternehmen',
    thCountry: 'Land',
    thEmissions: 'CO2-Ausstoß (Megatonnen)',
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
    contentTitle: 'بيانات الانبعاثات 2026',
    searchPlaceholder: 'البحث...',
    thCompany: 'شركة',
    thCountry: 'بلد',
    thEmissions: 'انبعاثات ثاني أكسيد الكربون (ميجاطن)',
    footerRights: '© 2026 المنظمة غير الربحية للعمل المناخي. جميع الحقوق محفوظة.',
    footerImprint: 'بصمة',
    footerPrivacy: 'حماية البيانات',
    footerTerms: 'تنصل'
  }
}

const t = computed(() => isRtl.value ? translations.ar : translations.de)
const toggleCulture = () => isRtl.value = !isRtl.value

// --- 2. FIKTIVE DATEN ---
const rawData = [
  { id: 1, company: 'EcoEnergy Corp', country: 'Deutschland', emissions: 12.5 },
  { id: 2, company: 'Global Steel Works', country: 'USA', emissions: 84.2 },
  { id: 3, company: 'Nordic Trans', country: 'Schweden', emissions: 3.1 },
  { id: 4, company: 'Asian Manufacturing', country: 'China', emissions: 145.8 },
  { id: 5, company: 'Bavaria Motors', country: 'Deutschland', emissions: 22.4 },
  { id: 6, company: 'Desert Oil', country: 'VAE', emissions: 98.0 },
  { id: 7, company: 'Tech Giant Inc', country: 'USA', emissions: 18.7 },
]

// --- 3. SORTIER- UND FILTERLOGIK ---
const activeFilter = ref('countries')
const searchQuery = ref('')
const sortKey = ref('country') // Standardmäßig nach Land sortiert
const sortAsc = ref(true)

// NEU: Diese Funktion wird beim Klick im linken Menü aufgerufen
const setFilter = (filterType) => {
  activeFilter.value = filterType
  searchQuery.value = '' // Suchfeld leeren beim Wechseln

  // Automatisch passend sortieren, je nachdem was geklickt wurde
  if (filterType === 'countries') {
    sortKey.value = 'country'
    sortAsc.value = true
  } else if (filterType === 'companies') {
    sortKey.value = 'company'
    sortAsc.value = true
  }
}

const sortBy = (key) => {
  if (sortKey.value === key) {
    sortAsc.value = !sortAsc.value
  } else {
    sortKey.value = key
    sortAsc.value = true
  }
}

const processedData = computed(() => {
  let result = rawData.filter(item => {
    const query = searchQuery.value.toLowerCase()
    return item.company.toLowerCase().includes(query) || 
           item.country.toLowerCase().includes(query)
  })

  result.sort((a, b) => {
    let valA = a[sortKey.value]
    let valB = b[sortKey.value]

    if (typeof valA === 'string') {
      return sortAsc.value ? valA.localeCompare(valB) : valB.localeCompare(valA)
    } else {
      return sortAsc.value ? valA - valB : valB - valA
    }
  })

  return result
})
</script>

<template>
  <div class="min-h-screen flex flex-col bg-gray-50 text-gray-900 font-sans" :dir="isRtl ? 'rtl' : 'ltr'">
    
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
          <button @click="toggleCulture" class="ml-4 rtl:mr-4 rtl:ml-0 bg-green-800 px-3 py-1.5 rounded text-sm hover:bg-green-600 transition-colors border border-green-600">
            {{ t.btnLang }}
          </button>
        </nav>
      </div>
    </header>

    <main class="flex-grow container mx-auto px-4 py-8 w-full flex flex-col md:flex-row gap-6 md:items-start">
      
      <!-- SEITENLEISTE MIT MENÜ -->
      <aside class="w-full md:w-64 shrink-0 bg-white p-5 rounded-xl shadow-sm border border-gray-100">
        <h3 class="font-bold text-lg mb-4 text-green-800 border-b pb-2">{{ t.filterTitle }}</h3>
        <ul class="space-y-2">
          <!-- Button nutzt jetzt @click="setFilter(...)" -->
          <li>
            <button @click="setFilter('countries')" class="w-full text-left rtl:text-right p-2 rounded transition-colors" :class="activeFilter === 'countries' ? 'bg-green-50 text-green-700 font-medium' : 'hover:bg-gray-50 text-gray-600'">
              {{ t.filterCountries }}
            </button>
          </li>
          <li>
            <button @click="setFilter('companies')" class="w-full text-left rtl:text-right p-2 rounded transition-colors" :class="activeFilter === 'companies' ? 'bg-green-50 text-green-700 font-medium' : 'hover:bg-gray-50 text-gray-600'">
              {{ t.filterCompanies }}
            </button>
          </li>
          <li>
            <button @click="setFilter('method')" class="w-full text-left rtl:text-right p-2 rounded transition-colors" :class="activeFilter === 'method' ? 'bg-green-50 text-green-700 font-medium' : 'hover:bg-gray-50 text-gray-600'">
              {{ t.filterMethod }}
            </button>
          </li>
        </ul>
      </aside>

      <!-- HAUPTINHALT (Tabelle oder Methodik) -->
      <div class="flex-grow bg-white p-6 md:p-8 rounded-xl shadow-sm border border-gray-100 w-full overflow-hidden">
        
        <!-- Ansicht 1: Die Tabelle -->
        <div v-if="activeFilter === 'countries' || activeFilter === 'companies'">
          <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-6 gap-4">
            <h2 class="text-2xl font-semibold text-green-800">{{ t.contentTitle }}</h2>
            <div class="relative w-full md:w-72">
              <input v-model="searchQuery" type="text" :placeholder="t.searchPlaceholder" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-500 bg-gray-50 text-sm">
            </div>
          </div>

          <div class="overflow-x-auto rounded-lg border border-gray-200">
            <table class="w-full border-collapse text-left rtl:text-right">
              <thead>
                <tr class="bg-gray-100 text-gray-700 border-b-2 border-gray-200 text-sm uppercase tracking-wider">
                  <th @click="sortBy('company')" class="p-4 cursor-pointer hover:bg-gray-200 transition-colors font-semibold w-1/3">
                    <div class="flex items-center gap-1">{{ t.thCompany }} <span v-if="sortKey === 'company'" class="text-green-600">{{ sortAsc ? '▲' : '▼' }}</span></div>
                  </th>
                  <!-- Spalte "Land" wird mit v-if nur angezeigt, wenn der Länder-Filter aktiv ist -->
                  <th v-if="activeFilter === 'countries'" @click="sortBy('country')" class="p-4 cursor-pointer hover:bg-gray-200 transition-colors font-semibold w-1/3">
                    <div class="flex items-center gap-1">{{ t.thCountry }} <span v-if="sortKey === 'country'" class="text-green-600">{{ sortAsc ? '▲' : '▼' }}</span></div>
                  </th>
                  <th @click="sortBy('emissions')" class="p-4 cursor-pointer hover:bg-gray-200 transition-colors font-semibold w-1/3">
                    <div class="flex items-center gap-1">{{ t.thEmissions }} <span v-if="sortKey === 'emissions'" class="text-green-600">{{ sortAsc ? '▲' : '▼' }}</span></div>
                  </th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="row in processedData" :key="row.id" class="border-b border-gray-100 hover:bg-gray-50 transition-colors">
                  <td class="p-4 font-medium text-gray-800">{{ row.company }}</td>
                  <!-- Zelle "Land" wird ebenfalls ausgeblendet, wenn "Nach Unternehmen" gewählt ist -->
                  <td v-if="activeFilter === 'countries'" class="p-4 text-gray-600">{{ row.country }}</td>
                  <td class="p-4 font-bold" :class="row.emissions > 50 ? 'text-red-600' : 'text-green-600'">{{ row.emissions }} Mt</td>
                </tr>
                <tr v-if="processedData.length === 0">
                  <td :colspan="activeFilter === 'countries' ? 3 : 2" class="p-8 text-center text-gray-500">Keine Daten für "{{ searchQuery }}" gefunden.</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>

        <!-- Ansicht 2: Methodik & Quellen -->
        <div v-else-if="activeFilter === 'method'" class="animate-fade-in">
          <h2 class="text-2xl font-semibold text-green-800 mb-6">{{ t.filterMethod }}</h2>
          <div class="prose text-gray-600 leading-relaxed space-y-4">
            <p><strong>Datenerhebung:</strong> Die auf dieser Plattform dargestellten fiktiven Daten orientieren sich am Greenhouse Gas Protocol und internationalen Standards.</p>
            <p><strong>Einheit:</strong> Alle Angaben erfolgen in Megatonnen (Mt) CO2-Äquivalent. Eine Megatonne entspricht einer Million Tonnen.</p>
            <div class="bg-green-50 p-4 rounded-lg border border-green-100 mt-6">
              <h4 class="font-bold text-green-800 mb-2">Transparenz-Hinweis</h4>
              <p class="text-sm">Dieses Projekt wurde im Rahmen einer interdisziplinären Fallstudie entwickelt. Die Tabellenwerte dienen ausschließlich der Demonstration der funktionalen Oberfläche.</p>
            </div>
          </div>
        </div>

      </div>
    </main>

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