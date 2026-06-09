# MixAudioLab — MixOff · Guía de setup

## Lo que hay en esta carpeta
- `index.html` — la app completa (no tocar)
- `config.json` — editás este archivo cada semana
- `SETUP.md` — esta guía

---

## Setup único (hacerlo una sola vez)

### 1. Crear proyecto Firebase

1. Ir a https://console.firebase.google.com
2. Click en **Agregar proyecto** → ponerle nombre (ej: `mixaudiolab`)
3. Desactivar Google Analytics (no es necesario) → **Crear proyecto**

### 2. Activar autenticación con Google

1. En el menú izquierdo: **Authentication** → **Comenzar**
2. Tab **Método de acceso** → click en **Google** → activar → guardar

### 3. Crear base de datos Firestore

1. En el menú izquierdo: **Firestore Database** → **Crear base de datos**
2. Elegir **Modo de producción** → seleccionar región (ej: `us-central1`) → **Habilitar**
3. Ir a la tab **Reglas** y reemplazar el contenido con:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /rounds/{roundId}/votes/{userId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
    match /rounds/{roundId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null;
    }
  }
}
```

4. Click **Publicar**

### 4. Obtener credenciales Firebase

1. En Firebase: ícono de engranaje → **Configuración del proyecto**
2. Bajar hasta **Tus apps** → click en `</>` (Web)
3. Registrar app con cualquier nombre → copiar el objeto `firebaseConfig`
4. Pegar los valores en `config.json` en la sección `"firebase"`

### 5. Agregar dominio autorizado

1. En Firebase: **Authentication** → **Configuración** → **Dominios autorizados**
2. Agregar tu dominio de GitHub Pages (ej: `tunombre.github.io`)

### 6. Subir a GitHub Pages

1. Crear repositorio nuevo en github.com (público)
2. Subir los tres archivos: `index.html`, `config.json`, `SETUP.md`
3. Ir a **Settings** → **Pages** → **Branch: main** → **Save**
4. Tu link quedará en: `https://tunombre.github.io/nombre-del-repo/`

---

## Cada semana (5 minutos)

### Subir audios a Google Drive

1. Subir los WAV a Google Drive
2. Click derecho en cada archivo → **Compartir** → **Cualquier persona con el link** → **Lector**
3. Copiar el link. Tiene la forma:
   `https://drive.google.com/file/d/ESTE_ES_EL_ID/view`
4. Usar el ID en el config así:
   `https://drive.google.com/uc?export=download&id=ESTE_ES_EL_ID`

### Editar config.json en GitHub

1. Ir al repositorio en github.com
2. Click en `config.json` → ícono del lápiz (editar)
3. Cambiar:
   - `"numero"` — incrementar cada ronda
   - `"nombre"` — nombre de la ronda
   - `"fecha"` — fecha de la ronda
   - `"tracks"` — actualizar nombres y drive_urls
4. Pueden ser entre 2 y 8 tracks
5. Click en **Commit changes** → listo

---

## Panel de admin

Cuando entrés con tu email de Google (el que pusiste en `admins`), vas a ver el panel de admin con:

- **ABRIR VOTACIÓN** — habilita el botón de voto para todos
- **CERRAR VOTACIÓN** — deshabilita la votación
- **★ REVELAR** — aparece el gráfico de pizza en todos los dispositivos al mismo tiempo
- **EXPORTAR** — descarga un .txt con el detalle completo de votos

Los jueces no ven resultados en ningún momento hasta el reveal.
