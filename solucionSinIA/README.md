# 📧 Automatización de clasificación y respuesta de correos

## 📌 Descripción General
Este flujo de trabajo automatiza la clasificación de correos electrónicos entrantes y genera respuestas automáticas basadas en palabras clave. Es útil para gestionar consultas urgentes, preguntas de facturación y solicitudes generales de manera eficiente.

![GmailN8N](https://github.com/user-attachments/assets/252c2cea-e30d-4fed-9769-51de15daf588)

---

## 🛠️ Componentes del Flujo de Trabajo

### 🔹 1️⃣ Gmail Trigger (Nodo de Inicio)
- **Función**: Monitorea la bandeja de entrada en busca de nuevos correos.
- **Frecuencia**: Ejecuta la verificación cada minuto.
- **Criterios**: Solo procesa correos **no leídos** con etiqueta `CATEGORY_PERSONAL`.
- **Autenticación**: Requiere credenciales OAuth2 de Gmail.

### 🔹 2️⃣ Switch (Clasificación de Correos)
- **Función**: Clasifica los correos según su contenido usando coincidencias de palabras clave.
- **Reglas**:
  - **Urgente**: Contiene `urgente, emergencia, inmediato, crítico, problema`.
  - **Facturación**: Contiene `facturación, factura, pago, cobro, cuenta, suscripción`.
  - **Consulta**: Contiene `consulta, pregunta, información, ayuda, soporte`.
  - **Extra**: Categoría por defecto si no se reconoce la solicitud.

### 🔹 3️⃣ Edit Fields1 (Respuesta Urgente)
- **Función**: Genera una respuesta automática para correos urgentes.
- **Mensaje**:
  - **Prioridad**: Alta.
  - **Plantilla**: `"Hemos recibido su mensaje y entendemos la urgencia de su solicitud. Nuestro equipo de soporte..."`

### 🔹 4️⃣ Edit Fields2 (Respuesta de Facturación)
- **Función**: Genera una respuesta automática para correos relacionados con la facturación.
- **Mensaje**:
  - **Prioridad**: Media.
  - **Plantilla**: `"Hemos recibido su consulta sobre facturación. Para brindarle una mejor asistencia..."`

### 🔹 5️⃣ Edit Fields3 (Respuesta General)
- **Función**: Responde automáticamente a consultas generales.
- **Mensaje**:
  - **Prioridad**: Baja.
  - **Plantilla**: `"Gracias por ponerse en contacto con nosotros. Hemos recibido su consulta y nuestro equipo la está revisando..."`

### 🔹 6️⃣ Edit Fields4 (Respuesta No Clasificada)
- **Función**: Maneja correos no categorizados en las reglas anteriores.
- **Mensaje**:
  - **Prioridad**: Baja.
  - **Plantilla**: `"Hemos recibido su mensaje, pero no pudimos clasificar automáticamente su solicitud. Nuestro equipo lo revisará..."`

---

## 🔀 Flujo de Datos
1. 📥 **Gmail Trigger** obtiene nuevos correos electrónicos no leídos.
2. 📊 **Switch Node** analiza el contenido y lo clasifica según palabras clave.
3. 📩 Se genera una respuesta en función de la categoría:
   - **Urgente** → `Edit Fields (Respuesta Urgente)`
   - **Facturación** → `Edit Fields2 (Respuesta de Facturación)`
   - **Consulta General** → `Edit Fields3 (Respuesta General)`
   - **No clasificado** → Mensaje estándar de revisión manual.
4. 💾 La respuesta generada se almacena o se usa para responder automáticamente.

---

## ⚙️ Personalización
- **Añadir nuevas categorías** en el nodo **Switch** editando las condiciones de filtrado.
- **Modificar las respuestas** en los nodos de **Set/Edit Fields** para ajustar los mensajes según la necesidad.
- **Cambiar la frecuencia** del nodo **Gmail Trigger** para ajustar la velocidad de procesamiento.

---

## ❗ Manejo de Errores
| Problema                   | Posible Causa                               | Solución |
|----------------------------|--------------------------------------------|----------|
| No se procesan correos     | Permisos incorrectos de OAuth2 en Gmail    | Revisar credenciales en n8n |
| Correos no clasificados    | Faltan palabras clave en Switch Node      | Agregar más términos relevantes |
| Respuestas demoradas       | El Trigger no se ejecuta con suficiente frecuencia | Ajustar configuración del disparador |

---
