# 💳 CobrApp - Automatización de Registro de Pagos

Sistema automatizado para registrar comprobantes de pago enviados por Telegram. El sistema recibe capturas de Yape, Plin o Lemon, extrae datos mediante OCR, valida la información, evita duplicados, registra los pagos en Google Sheets y envía confirmaciones automáticas al grupo.

---

## 🚀 1. Objetivo

Automatizar el registro de pagos enviados como imágenes (Yape, Plin), eliminando el registro manual y reduciendo errores.

---

## 🧰 2. Tecnologías utilizadas

- Telegram Bot API  
- n8n Cloud  
- Python (FastAPI)  
- Tesseract OCR  
- OpenCV  
- Docker  
- Render (deploy API)  
- Google Sheets  
- GitHub  

---

## 🏗️ 3. Arquitectura del sistema

```txt
Usuario (Telegram)
↓
Grupo de Telegram
↓
n8n Cloud (Trigger)
↓
Validación de imagen
↓
Descarga del archivo
↓
API FastAPI (Render)
↓
OCR (Python + Tesseract)
↓
Validación de datos
↓
Google Sheets (duplicados + registro)
↓
Respuesta automática en Telegram

```

---

## ⚙️ 4. Funcionalidades

- Recepción de imágenes desde Telegram  
- Validación de formato (solo imágenes)  
- Extracción automática de:
  - Nombre
  - Monto
  - Número de operación
  - Fecha y hora  
- Registro automático en Google Sheets  
- Detección de pagos duplicados  
- Confirmación automática en Telegram  
- Reporte diario automático  

---

## 📁 5. Estructura del proyecto

```
Trabajo01-automatizacion/
│
├── python-api/
│   ├── main.py              # API FastAPI con OCR (Tesseract + OpenCV) para extraer datos de comprobantes
│   ├── Dockerfile           # Imagen Docker con Python 3.11, Tesseract OCR y dependencias del sistema
│   └── requirements.txt     # Dependencias de Python (FastAPI, pytesseract, OpenCV, numpy, etc.)
│
├── docker-compose.yml       # Orquestación de servicios: n8n (puerto 5678) y python-api (puerto 8000)
└── README.md                # Documentación general del proyecto
```

---

## 🐳 6. Ejecución local con Docker

### Requisitos

- Docker instalado  
- Git instalado  

### Ejecutar:

```bash
docker compose up --build
````

Servicios:

* n8n → [http://localhost:5678](http://localhost:5678)
* API → [http://localhost:8000](http://localhost:8000)

---

## 🌐 7. API en Render

### Endpoint principal:

```
POST /procesar-imagen
```

### URL:

```
https://dessolnube-trabajoautomatizacion.onrender.com
```

---

## 🔄 8. Flujo en n8n

El flujo incluye:

1. Telegram Trigger
2. IF (validar imagen)
3. Get File
4. HTTP Request (API OCR)
5. IF válido
6. Google Sheets (buscar operación)
7. IF duplicado
8. Google Sheets (append row)
9. Mensajes en Telegram

---

## 📊 9. Google Sheets

Nombre del documento:

```
Pagos CobrApp
```

Columnas:

```
Fecha | Hora | Nombre | Monto | Tipo | Operación | Estado | Registrado_por
```

---

## 🔁 10. Detección de duplicados

Se compara el número de operación.

⚠️ Se eliminan ceros iniciales para evitar errores:

```txt
00286700 → 286700
```

---

## 💬 11. Mensajes automáticos

### ✔️ Pago registrado

```
Pago registrado correctamente ✅

Monto: S/ 10.30
Nombre: Anderson Ram
Operacion: 286700
Fecha: 01 may. 2026
Hora: 23:25:44
```

---

### ❌ Imagen inválida

```
No se pudo validar el comprobante
```

---

### ⚠️ Duplicado

```
Pago duplicado detectado
```

---

## 📅 12. Reporte diario

Automático con:

* Schedule Trigger
* Google Sheets
* JavaScript

Ejemplo:

```
REPORTE DIARIO

Total recaudado: S/ 30.90
Cantidad de pagos: 3
Fecha: 01 may 2026
```

---

## ⚠️ 13. Consideraciones

* Render Free puede demorar en responder (sleep)
* Google Sheets evita pérdida de datos
* n8n Cloud permite ejecución 24/7
* Docker asegura portabilidad

---

## 👨‍💻 14. Autor

**Anderson Jampier Ramos Iman**

Curso:
Desarrollo de Soluciones en la Nube

---

## ✅ 15. Estado del proyecto

```txt
✔ API desplegada en Render
✔ Bot de Telegram funcionando
✔ Flujo en n8n Cloud activo
✔ Registro en Google Sheets
✔ Detección de duplicados
✔ Reporte diario automático
```

