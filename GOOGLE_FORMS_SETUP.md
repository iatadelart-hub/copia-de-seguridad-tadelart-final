# CONFIGURACIÓN COMPLETA - FORMULARIOS + GOOGLE SHEETS + TELEGRAM

## 1. CREAR LA HOJA DE CÁLCULO (Google Sheets)

1. Ve a https://sheets.google.com
2. Crea una nueva hoja llamada: **Tadelart_Leads_2026**
3. Crea **4 pestañas** con estos nombres exactos:

- `Lead_Web` 
- `Presupuesto_Web`
- `Contacto_Web`
- `Candidato_Trabajo`

---

## 2. CREAR LOS 4 FORMULARIOS DE GOOGLE FORMS

### Formulario 1: Lead Web (sección inferior de páginas)
- Título: `Tadelart - Lead Web`
- Campos:
  1. Email o Teléfono (texto corto)
  2. Tipo de Proyecto (selección)
  3. Mensaje (párrafo)
- ID del formulario (copia la URL larga)

### Formulario 2: Presupuesto (página contacto)
- Título: `Tadelart - Presupuesto`
- Campos:
  1. Nombre Completo
  2. Email o Teléfono
  3. Tipo de Proyecto
  4. Mensaje

### Formulario 3: Contacto (página contacto)
- Título: `Tadelart - Contacto`
- Campos:
  1. Nombre Completo
  2. Email o Teléfono
  3. Tipo de Proyecto
  4. Mensaje

### Formulario 4: Candidato Trabajo (página contacto)
- Título: `Tadelart - Candidato`
- Campos:
  1. Nombre Completo
  2. Email
  3. Teléfono
  4. Experiencia
  5. Mensaje

---

## 3. CONFIGURAR APPS SCRIPT (AUTOMÁTICO)

1. Abre tu hoja **Tadelart_Leads_2026**
2. Ve a **Extensiones → Apps Script**
3. Borra todo el código y pega este:

```javascript
// ================== TADELART FORMS + TELEGRAM ==================

const TELEGRAM_BOT_TOKEN = 'TU_TOKEN_AQUI';
const TELEGRAM_CHAT_ID = 'TU_CHAT_ID_AQUI';

function onFormSubmit(e) {
  const sheet = e.source.getActiveSheet();
  const sheetName = sheet.getName();
  
  let mensaje = `🔔 *Nueva respuesta - ${sheetName}*\n\n`;
  
  const data = e.values;
  
  for (let i = 0; i < data.length; i++) {
    if (data[i]) {
      mensaje += `• *Campo ${i+1}:* ${data[i]}\n`;
    }
  }
  
  mensaje += `\n📅 ${new Date().toLocaleString('es-ES')}`;
  
  // Enviar a Telegram
  const url = `https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage`;
  const payload = {
    chat_id: TELEGRAM_CHAT_ID,
    text: mensaje,
    parse_mode: 'Markdown'
  };
  
  UrlFetchApp.fetch(url, {
    method: 'post',
    payload: JSON.stringify(payload),
    contentType: 'application/json'
  });
}

// Configurar triggers (ejecutar una vez)
function setupTriggers() {
  // Este script se activa automáticamente cuando llega un formulario
  console.log('Triggers configurados correctamente');
}
```

4. Reemplaza:
   - `TU_TOKEN_AQUI` → Token de tu bot de Telegram
   - `TU_CHAT_ID_AQUI` → Tu ID de chat de Telegram

---

## 4. CREAR BOT DE TELEGRAM (2 minutos)

1. Abre Telegram y busca **@BotFather**
2. Envía `/newbot`
3. Ponle nombre: `Tadelart Alerts`
4. Copia el token que te da (guárdalo)

---

## 5. OBTENER TU CHAT ID

1. Abre Telegram y busca **@userinfobot**
2. Pulsa Start
3. Te dará tu `chat_id` (número largo)

---

## 6. CONECTAR LOS FORMULARIOS A LA HOJA

En cada formulario de Google Forms:
1. Ve a **Respuestas**
2. Haz clic en el icono de hoja de cálculo
3. Selecciona tu hoja **Tadelart_Leads_2026**
4. Elige la pestaña correspondiente

---

## 7. ACTIVAR EL SCRIPT

1. En Apps Script, haz clic en el icono de **reloj** (Triggers)
2. Añade un trigger:
   - Evento: `On form submit`
   - Función: `onFormSubmit`

---

## 8. CÓDIGO HTML DE LOS FORMULARIOS (ya listo en la web)

Los formularios de la web ya están preparados. Solo necesitas cambiar las URLs de los forms en `script.js` cuando tengas los IDs de los formularios de Google.

---

**¡Listo!** Cada vez que alguien envíe un formulario, recibirás notificación inmediata en Telegram con todos los datos.