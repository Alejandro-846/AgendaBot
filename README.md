# AgendaBot - Sistema de Gestión de Citas y Tareas

## 📋 Descripción del Proyecto

AgendaBot es un bot automatizado de Telegram que permite agendar, planificar y automatizar tareas básicas sin depender de plataformas de pago ni servicios que exijan tarjeta de crédito.

### Stack Tecnológico
- **Telegram**: Interfaz conversacional
- **n8n Community Edition**: Automatización y lógica
- **Google Sheets**: Almacenamiento de datos

## 🚀 Requisitos Previos

1. **Telegram Bot**
   - Cuenta de Telegram
   - Bot creado mediante @BotFather
   - Token de API del bot

2. **n8n Community Edition**
   - Instalación local de n8n (self-hosted)
   - Node.js v16 o superior
   - npm o Docker

3. **Google Sheets**
   - Cuenta de Google
   - Hoja de cálculo creada
   - API de Google Sheets habilitada

## 📦 Instalación

### 1. Instalar n8n Community Edition

#### Opción A: NPM
```bash
npm install n8n -g
n8n start
```

#### Opción B: Docker
```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

### 2. Crear Bot de Telegram

1. Abre Telegram y busca **@BotFather**
2. Envía el comando `/newbot`
3. Sigue las instrucciones para nombrar tu bot
4. Guarda el **token** que te proporciona (ejemplo: `123456789:ABCdefGHIjklMNOpqrsTUVwxyz`)

### 3. Configurar Google Sheets

1. Ve a [Google Sheets](https://sheets.google.com)
2. Crea una nueva hoja llamada **"AgendaBot_DB"**
3. Crea las siguientes pestañas con sus encabezados:

#### Hoja: CITAS
```
id_cita | fecha | hora | nombre | motivo | canal | estado | creado_por | timestamp_creacion
```

#### Hoja: TAREAS
```
id_tarea | titulo | prioridad | estado | fecha_objetivo | creado_por
```

#### Hoja: HABITOS
```
id_habito | nombre | frecuencia | hora_recordatorio | estado
```

#### Hoja: LISTAS
```
id_lista | nombre_lista | tipo | creado_por
```

#### Hoja: ITEMS_LISTA
```
id_item | id_lista | item | estado
```

#### Hoja: USUARIOS
```
telegram_user | nombre | rol | permitido
```

#### Hoja: LOGS
```
timestamp | telegram_user | pantalla | opcion_elegida | resultado
```

#### Hoja: SESSIONS
```
telegram_user | pantalla_actual | paso_actual | datos_parciales | timestamp_ultima_interaccion
```

4. Copia el **ID de la hoja** de la URL (la parte después de `/d/` y antes de `/edit`)

### 4. Habilitar Google Sheets API

1. Ve a [Google Cloud Console](https://console.cloud.google.com)
2. Crea un nuevo proyecto o selecciona uno existente
3. Habilita la **Google Sheets API**
4. Crea credenciales OAuth 2.0
5. Descarga el archivo JSON de credenciales

## ⚙️ Configuración del Workflow

### 1. Importar Workflow en n8n

1. Abre n8n en tu navegador (http://localhost:5678)
2. Haz clic en **"Import from File"**
3. Selecciona el archivo `AgendaBot_Workflow.json`

### 2. Configurar Credenciales

#### Telegram Bot API
1. En n8n, ve a **Settings** → **Credentials**
2. Crea nueva credencial tipo **"Telegram API"**
3. Ingresa el token de tu bot
4. Guarda con nombre: **"Telegram Bot API"**

#### Google Sheets OAuth2
1. En n8n, crea credencial tipo **"Google Sheets OAuth2 API"**
2. Sube el archivo JSON de credenciales de Google
3. Completa el proceso de autenticación OAuth
4. Guarda con nombre: **"Google Sheets OAuth2"**

### 3. Configurar Variables de Entorno

En n8n, configura la siguiente variable de entorno:

```bash
GOOGLE_SHEET_ID=tu_id_de_google_sheet_aqui
```

O actualiza todos los nodos de Google Sheets reemplazando:
```
={{ $env.GOOGLE_SHEET_ID }}
```
por tu ID real de Google Sheets.

### 4. Activar el Workflow

1. Abre el workflow importado
2. Verifica que todos los nodos estén correctamente conectados
3. Haz clic en **"Activate"** en la esquina superior derecha

## 🎯 Uso del Bot

### Comandos Básicos

1. **Iniciar el bot**: Envía `/start` en Telegram
2. **Navegar**: Escribe el número de la opción deseada
3. **Cancelar**: Escribe `9` durante cualquier proceso

### Menú Principal

```
0. Ayuda
1. Agenda (citas)
2. Tareas
3. Recordatorios
4. Hábitos
5. Listas
6. Reportes
7. Configuración
8. Administrador
```

### Flujo de Agendamiento

1. Selecciona opción **1** (Agenda)
2. Selecciona opción **1** (Agendar nueva cita)
3. Sigue los 6 pasos:
   - Paso 1: Ingresa fecha (YYYY-MM-DD)
   - Paso 2: Ingresa hora (HH:MM)
   - Paso 3: Ingresa nombre del cliente
   - Paso 4: Ingresa motivo de la cita
   - Paso 5: Selecciona canal (1=Presencial, 2=Virtual, 3=Llamada)
   - Paso 6: Confirma la información

## 🔍 Validaciones Implementadas

- ✅ Formato de fecha correcto (YYYY-MM-DD)
- ✅ Formato de hora correcto (HH:MM en formato 24h)
- ✅ No permite agendar en el pasado
- ✅ Validación de nombres (mínimo 3 caracteres)
- ✅ Validación de opciones numéricas
- ✅ Control de permisos por usuario
- ✅ Registro automático en logs

## 📊 Estructura de Datos

### Google Sheets

Todas las interacciones se almacenan en Google Sheets:

- **CITAS**: Registro completo de todas las citas
- **SESSIONS**: Estado actual de cada usuario
- **LOGS**: Historial de todas las interacciones
- **USUARIOS**: Control de acceso y permisos

## 🛠️ Personalización

### Agregar Nuevas Funciones

1. **Crear nuevo menú**: Agrega un nodo de función con el mensaje
2. **Agregar opción al router**: Modifica el nodo "Router Principal"
3. **Conectar en el switch**: Agrega la nueva ruta en "Switch Rutas"
4. **Actualizar sesión**: Asegura que se actualice la sesión correctamente

### Modificar Mensajes

Todos los mensajes están en nodos de tipo "Function". Busca el nodo correspondiente y modifica la variable `mensaje`.

## 🧪 Pruebas

### Plan de Pruebas Mínimo

- [ ] 30 pruebas de navegación por menús
- [ ] 10 agendamientos completos
- [ ] 10 errores controlados (validaciones)
- [ ] 10 pruebas de permisos
- [ ] Verificar logs en Google Sheets

### Evidencias

Captura de pantalla de:
- Conversaciones en Telegram
- Datos en Google Sheets
- Logs de interacciones
- Workflow activo en n8n

## 📝 Mantenimiento

### Logs
Todos los logs se guardan automáticamente en la hoja **LOGS** con:
- Timestamp
- Usuario
- Pantalla visitada
- Opción elegida
- Resultado

### Sesiones
Las sesiones se mantienen activas y permiten:
- Continuar flujos interrumpidos
- Mantener contexto del usuario
- Almacenar datos parciales

## 🚨 Solución de Problemas

### El bot no responde
1. Verifica que el workflow esté activado en n8n
2. Revisa que las credenciales de Telegram sean correctas
3. Comprueba que n8n esté ejecutándose

### No se guardan datos en Google Sheets
1. Verifica las credenciales de Google Sheets
2. Comprueba que el ID de la hoja sea correcto
3. Asegúrate de que las hojas tengan los nombres exactos

### Errores de validación
1. Revisa el formato de entrada (fecha, hora)
2. Consulta los logs en Google Sheets
3. Verifica el nodo de validación correspondiente

## 📚 Recursos Adicionales

- [Documentación n8n](https://docs.n8n.io/)
- [Telegram Bot API](https://core.telegram.org/bots/api)
- [Google Sheets API](https://developers.google.com/sheets/api)

## 👥 Contribuciones

Este es un proyecto educativo. Para contribuir:
1. Fork del repositorio
2. Crea una rama para tu feature
3. Commit de cambios
4. Push a la rama
5. Crea un Pull Request
