# Climate Action Tracker - Fallstudie

Dieses Projekt wurde im Rahmen einer interdisziplinären Fallstudie entwickelt und demonstriert die Umsetzung einer interaktiven, responsiven Web-Oberfläche zur Visualisierung von CO2-Emissionsdaten. 

## Eingesetzte Technologien (Anforderung a)
Für die Entwicklung wurde ein moderner und performanter Tech-Stack gewählt:
* **Vue 3 (Composition API):** Ermöglicht eine reaktive und komponentenbasierte Architektur. Die Logik wurde über den `<script setup>`-Block sauber von der Darstellung getrennt.
* **Vite:** Dient als schnelles Build-Tool und lokaler Entwicklungsserver, der durch Hot Module Replacement (HMR) eine effiziente Entwicklung ermöglicht.
* **Tailwind CSS v3:** Ein Utility-First CSS-Framework, das genutzt wurde, um das Layout direkt im HTML-Code effizient zu stylen und ein konsistentes Design-System zu gewährleisten.

## Layout & Responsive Design (Anforderung b.a, b.b, b.d)
Das Layout folgt einer klassischen, benutzerfreundlichen Dashboard-Struktur:
* **Header:** Enthält das Logo, den Titel und die globale Navigation inklusive Sprachumschalter.
* **Main Content:** Aufgeteilt in eine Seitenleiste (lokales Filter-Menü) und den Hauptanzeigebereich (Datentabelle).
* **Footer:** Beinhaltet rechtliche Platzhalter-Links und ist technisch als "Sticky Footer" (`mt-auto`) umgesetzt.
* **Responsiveness:** Dank Flexbox und den Breakpoints von Tailwind CSS (`md:flex-row`, `flex-col`) passt sich das Layout fließend an. Auf mobilen Endgeräten werden die Elemente platzsparend untereinander dargestellt.

## Internationalisierung & Schriftkultur (Anforderung b.c)
Um die Plattform global zugänglich zu machen, wurde eine dynamische i18n-Lösung integriert:
* Über einen Button im Header kann zwischen LTR (Deutsch) und RTL (Arabisch) gewechselt werden.
* Ein reaktives Wörterbuch-Objekt (`translations`) übersetzt die gesamte Oberfläche per Text-Interpolation in Echtzeit.
* Das Layout spiegelt sich dank des HTML-Attributs `dir="rtl"` und Tailwinds RTL-Unterstützung (`rtl:text-right`, etc.) automatisch.

## Datenverwaltung & Interaktion (Anforderung b.e)
Die Kernfunktion der Anwendung ist die interaktive Datentabelle:
* **Dynamische Ansichten:** Über die Seitenleiste kann die Darstellung gesteuert werden. Die Spalte "Land" wird beispielsweise via `v-if` dynamisch ausgeblendet, wenn der Fokus auf den Unternehmen liegt.
* **Sortierung:** Ein Klick auf die Tabellenköpfe sortiert die Daten auf- oder absteigend. Hierbei wurde eine Typprüfung (`typeof`) implementiert, um sowohl Strings als auch Zahlen korrekt zu sortieren.
* **Filterung:** Ein Suchfeld ermöglicht die Echtzeit-Filterung der Daten mittels `.filter()` und `.includes()`.
* **Visualisierung:** Emissionen über 50 Mt werden zur besseren Übersicht via dynamischem Class-Binding (`:class`) automatisch rot hervorgehoben.

## Security & XSS-Schutz (Anforderung b.f)
Die Anwendung ist aktiv gegen Cross-Site Scripting (XSS) abgesichert:
* **Standard-Schutz:** Vue.js neutralisiert potenziell gefährlichen Code durch automatisches Escaping von Haus aus.
* **Proof of Concept (Aktive Sanitization):** Zusätzlich überwacht ein `watch`-Handler das Suchfeld. Sobald versucht wird, für HTML-Tags kritische Zeichen (`<` oder `>`) einzugeben, werden diese in Echtzeit bereinigt. 
* **Logging:** Jeder Blockierungs-Vorgang wird als Sicherheitswarnung in der Browser-Konsole (`console.warn`) protokolliert.

---

## Projekt lokal starten (Setup-Anleitung)

Um dieses Projekt lokal auf Ihrem Rechner auszuführen, benötigen Sie [Node.js](https://nodejs.org/). Folgen Sie diesen Schritten:

1. Repository herunterladen:
   Laden Sie den Code als ZIP herunter oder klonen Sie das Repository:
   `git clone https://github.com/TomKleini/Programmierung-von-Webanwendungsoberfl-chen`

2. Abhängigkeiten installieren:
   Navigieren Sie im Terminal in den Projektordner und installieren Sie die benötigten Pakete:
    cd Programmierung-von-Webanwendungsoberfl-chen
    npm install

3. Starten Sie den lokalen Vite-Server:
    npm run dev

4. Im Browser öffnen:
    Klicken Sie auf den im Terminal angezeigten Link (meistens http://localhost:5173/), um die Anwendung im Browser zu betrachten.