# 🔧 GUÍA COMPLETA DE FUSIÓN
## Oracle Quiz: Versión Kimi + Traducciones + Funcionalidades Extra

---

## 📋 RESUMEN

Vamos a tomar la versión **Kimi** (la del ZIP) como base y añadirle el **sistema completo de traducciones EN/ES** de tu versión original.

---

## ✅ PASO 1: Preparar el archivo base

1. Abre el archivo `index.html` de la carpeta **oracle-quiz** (versión Kimi)
2. Haz una copia de seguridad llamándola `index-backup.html`

---

## ✅ PASO 2: Añadir sistema de idiomas al estado de la aplicación

**Ubicación:** Busca donde está definido `const appState = {` (aproximadamente línea 1470)

**AÑADIR** esta línea dentro del objeto appState:

```javascript
const appState = {
    // ... todas las propiedades existentes ...
    currentLanguage: 'en',  // ← AÑADIR ESTA LÍNEA
    // ... resto de propiedades ...
};
```

---

## ✅ PASO 3: Añadir función completa de traducciones

**Ubicación:** Después de la definición de `allQuestionsData` (línea ~3450, antes de las funciones)

**AÑADIR** esta función completa:

```javascript
// ==================== TRADUCCIONES ====================
function getTranslation(questionId) {
    const translations = {
        1: {
            q: "Usas el Asistente de Configuración para crear un nuevo dominio que consistirá en un clúster de tres servidores administrados junto con una instancia de WebLogic Server actuando como proxy HTTP para distribuir tráfico al clúster. ¿Qué tres pasos debes realizar con el Asistente de Configuración? (Elije tres.)",
            o: [
                "Crear un clúster.",
                "Instalar Oracle HTTP Server.",
                "Crear un servidor Coherence.",
                "Crear cuatro servidores administrados.",
                "Asignar tres servidores a un clúster.",
                "Especificar un puerto proxy para el servidor de administración."
            ]
        },
        2: {
            q: "Decides usar una base de datos para almacenar logs de transacciones de WebLogic Server para un servidor administrado. ¿Qué dos pasos se requieren para configurar esto? (Elije dos.)",
            o: [
                "En la configuración de Servicios del servidor administrado, selecciona 'JDBC' como el tipo para el almacén de logs de transacciones.",
                "En la configuración de Servicios del servidor administrado, selecciona 'Deshabilitar el Almacén Predeterminado'.",
                "Crear un origen de datos no-XA en el dominio para el log de transacciones de la base de datos.",
                "Crear un origen de datos XA en el dominio para el log de transacciones de la base de datos."
            ]
        },
        3: {
            q: "¿Qué tipo de persistencia de sesión es inválido?",
            o: [
                "async_replicated",
                "async_jdbc",
                "replicated",
                "async_memory",
                "cookie",
                "file",
                "replicated_if_clustered"
            ]
        },
        4: {
            q: "Un administrador en tu organización dice que no necesitas hacer backup de los archivos del dominio en una máquina donde solo corren servidores administrados. Selecciona la mejor explicación para esta afirmación.",
            o: [
                "Esto es cierto porque los archivos del dominio pueden recrearse con las utilidades pack y unpack.",
                "Esto es cierto (con una excepción) porque los archivos del dominio pueden recrearse con las utilidades pack y unpack. La excepción es si tu dominio de seguridad usa el sistema LDAP integrado, que no está incluido en el archivo JAR creado por la utilidad pack.",
                "Esto es cierto (con una excepción) porque los archivos del dominio pueden recrearse con las utilidades pack y unpack. La excepción es para aplicaciones que están desplegadas en el servidor administrado. Esos archivos no están incluidos en el archivo JAR creado por la utilidad pack. Si haces backup de esos manualmente, entonces pack y unpack pueden usarse.",
                "Esto no es cierto. Necesitas hacer backup de los archivos del dominio en una máquina donde solo corren servidores administrados usando comandos del sistema operativo (como tar o zip)."
            ]
        },
        5: {
            q: "¿Qué afirmación es verdadera sobre detener Node Manager?",
            o: [
                "Si detienes Node Manager, los servidores administrados que inició también se detendrán.",
                "Puedes matar el proceso de Node Manager usando comandos del sistema operativo.",
                "Usa el script stopNodeManager.sh para que Node Manager se detenga elegantemente.",
                "Usando la consola de administración, selecciona la máquina apropiada y haz clic en Detener.",
                "El servidor de administración entra en modo ADMIN si ya no puede comunicarse con Node Manager.",
                "Los servidores administrados que Node Manager inició entran en modo ADMIN si ya no pueden comunicarse con Node Manager."
            ]
        },
        // ... AÑADIR TODAS LAS OTRAS 122 TRADUCCIONES DESDE LA VERSIÓN ORIGINAL ...
        // Por brevedad no incluyo todas aquí, pero debes copiar TODAS las traducciones
        // desde la función getTranslation de tu versión original
    };
    
    return translations[questionId] || { q: "", o: [] };
}
```

**⚠️ IMPORTANTE:** Debes copiar **TODAS las 127 traducciones** del archivo original. Busca en tu versión original la función `getTranslation` completa y cópiala entera.

---

## ✅ PASO 4: Añadir botón de cambio de idioma en el header

**Ubicación:** En el HTML del header, busca la línea con el botón de sonido (aproximadamente línea 600-650)

**DESPUÉS** del botón de sonido, **AÑADIR**:

```html
<button class="icon-btn" id="langToggle" onclick="toggleLanguage()" title="Cambiar idioma">
    <span id="langIcon">🌐 EN</span>
</button>
```

---

## ✅ PASO 5: Añadir estilos CSS para el botón de idioma

**Ubicación:** En la sección de estilos, después de `.icon-btn` (aproximadamente línea 250)

**AÑADIR**:

```css
#langIcon {
    font-size: 0.9rem;
    font-weight: 600;
}
```

---

## ✅ PASO 6: Añadir función para toggle de idioma

**Ubicación:** En la sección de JavaScript, después de `toggleSound()` (aproximadamente línea 3480)

**AÑADIR**:

```javascript
function toggleLanguage() {
    appState.currentLanguage = appState.currentLanguage === 'en' ? 'es' : 'en';
    document.getElementById('langIcon').textContent = 
        appState.currentLanguage === 'en' ? '🌐 EN' : '🌐 ES';
    
    // Reload current question to show/hide translations
    if (appState.currentView === 'quiz') {
        renderQuestion();
    }
    
    // Save preference
    saveToLocalStorage();
    playSound('click');
}
```

---

## ✅ PASO 7: Modificar la función renderQuestion para mostrar traducciones

**Ubicación:** Busca la función `renderQuestion()` (aproximadamente línea 3700)

**MODIFICAR** la parte donde se muestra la pregunta para incluir la traducción:

**ANTES:**
```javascript
const questionHTML = `
    <div class="question-text">${currentQ.question}</div>
`;
```

**DESPUÉS:**
```javascript
const translation = getTranslation(currentQ.id);
const translationHTML = appState.currentLanguage === 'es' && translation.q
    ? `<div class="translation">${translation.q}</div>`
    : '';

const questionHTML = `
    <div class="question-text">
        ${currentQ.question}
        ${translationHTML}
    </div>
`;
```

---

## ✅ PASO 8: Añadir traducciones a las opciones

En la misma función `renderQuestion()`, busca donde se generan las opciones:

**MODIFICAR** el bucle de opciones:

**ANTES:**
```javascript
currentQ.options.forEach((opt, idx) => {
    optionsHTML += `
        <div class="option" data-index="${idx}">
            <span class="option-letter">${opt.letter}</span>
            ${opt.text}
        </div>
    `;
});
```

**DESPUÉS:**
```javascript
currentQ.options.forEach((opt, idx) => {
    const optTranslation = appState.currentLanguage === 'es' && translation.o && translation.o[idx]
        ? `<div class="translation">${translation.o[idx]}</div>`
        : '';
    
    optionsHTML += `
        <div class="option" data-index="${idx}">
            <span class="option-letter">${opt.letter}</span>
            ${opt.text}
            ${optTranslation}
        </div>
    `;
});
```

---

## ✅ PASO 9: Añadir estilos para la clase .translation

**Ubicación:** En la sección CSS (aproximadamente línea 300)

**AÑADIR**:

```css
.translation {
    font-size: 0.85rem;
    color: #a0a0a0;
    font-style: italic;
    margin-top: 8px;
    line-height: 1.4;
    padding-left: 5px;
    border-left: 2px solid #667eea;
}

.dark-mode .translation {
    color: #9ca3af;
}
```

---

## ✅ PASO 10: Actualizar localStorage para guardar idioma

**Ubicación:** En la función `saveToLocalStorage()` (aproximadamente línea 3509)

**AÑADIR** `currentLanguage` al objeto que se guarda:

**MODIFICAR:**
```javascript
function saveToLocalStorage() {
    const data = {
        bookmarks: Array.from(appState.bookmarks),
        notes: appState.notes,
        historicallyIncorrect: Array.from(appState.historicallyIncorrect),
        quizHistory: appState.quizHistory,
        streakDays: appState.streakDays,
        lastStudyDate: appState.lastStudyDate,
        soundEnabled: appState.soundEnabled,
        darkMode: appState.darkMode,
        currentLanguage: appState.currentLanguage  // ← AÑADIR ESTA LÍNEA
    };
    localStorage.setItem('oracleQuizData', JSON.stringify(data));
}
```

---

## ✅ PASO 11: Cargar idioma desde localStorage

**Ubicación:** En la función `loadFromLocalStorage()` (aproximadamente línea 3523)

**AÑADIR** la carga del idioma:

**MODIFICAR:**
```javascript
function loadFromLocalStorage() {
    const data = localStorage.getItem('oracleQuizData');
    if (data) {
        const parsed = JSON.parse(data);
        // ... todas las líneas existentes ...
        appState.currentLanguage = parsed.currentLanguage || 'en';  // ← AÑADIR
        
        // Actualizar icono de idioma
        document.getElementById('langIcon').textContent = 
            appState.currentLanguage === 'en' ? '🌐 EN' : '🌐 ES';
    }
    updateSoundIcon();
}
```

---

## ✅ PASO 12: Inicializar el botón de idioma al cargar

**Ubicación:** Al final del script, en el evento `DOMContentLoaded` (aproximadamente línea 4200)

**AÑADIR** después de `loadFromLocalStorage()`:

```javascript
document.addEventListener('DOMContentLoaded', () => {
    loadFromLocalStorage();
    
    // Inicializar botón de idioma
    document.getElementById('langIcon').textContent = 
        appState.currentLanguage === 'en' ? '🌐 EN' : '🌐 ES';
    
    // ... resto del código existente ...
});
```

---

## 🎯 RESULTADO FINAL

Después de aplicar todos estos cambios, tendrás:

✅ Versión Kimi con diseño moderno  
✅ Sistema completo de traducciones EN/ES  
✅ Botón para cambiar idioma en el header  
✅ Traducciones que se muestran/ocultan dinámicamente  
✅ Preferencia de idioma guardada en localStorage  
✅ PWA completa con Service Worker robusto  
✅ Todas las funcionalidades originales  

---

## ⚠️ CHECKLIST ANTES DE PUBLICAR

- [ ] Probado en Chrome/Edge
- [ ] Probado en Firefox
- [ ] Probado en Safari
- [ ] Probado en móvil Android
- [ ] Probado en móvil iOS
- [ ] Todas las 127 preguntas con traducciones
- [ ] Botón de idioma funciona
- [ ] LocalStorage guarda idioma
- [ ] Service Worker funciona offline
- [ ] APK generado correctamente

---

## 🚀 PRÓXIMOS PASOS

1. Aplicar todos los cambios indicados arriba
2. Probar localmente abriendo el HTML en el navegador
3. Subir a GitHub
4. Verificar que funciona en GitHub Pages
5. Generar nuevo APK con PWABuilder

---

**¿Necesitas ayuda con algún paso específico?** 😊

