# 🚐 BusetaGo

Prototipo web de **rastreo de busetas en tiempo real** para empresas de transporte.
Los pasajeros ven por dónde viene su buseta y marcan dónde están esperando; los
conductores se registran con clave, eligen su unidad y comparten su ubicación GPS.

> ⚠️ **Demo funcional.** Todo corre en un solo archivo `index.html`. Las busetas se
> mueven de forma simulada sobre un mapa real (Gran Área Metropolitana, Costa Rica).
> Para un producto real con sincronización entre varios teléfonos se necesita un
> backend (ver más abajo).

---

## ✨ Funcionalidades

### Vista Cliente
- Mapa en vivo con las busetas activas y su ruta.
- Botón **"Marcar dónde espero"**: al tocar el mapa se pide el **nombre** del pasajero.
- Botón **"Usar mi ubicación"** (GPS del navegador), también pide el nombre.
- **ETA estimado** de cada buseta hasta el punto de espera, actualizado en vivo.

### Vista Conductor
- **Acceso con clave** (login).
- Selección de la buseta que maneja hoy.
- **Registro de nuevos conductores** (nombre, placa y ruta) → aparece una nueva unidad.
- Interruptor **"Compartir mi ubicación (GPS real)"** para transmitir la posición del teléfono.
- Lista de **pasajeros esperando** en su ruta, con nombre, distancia y ETA.

---

## 🔑 Clave de acceso (demo)

Para entrar al modo Conductor:

```
buseta2026
```

> La clave está fija en el código solo para la demostración. En producción cada
> conductor tendría su propio usuario y contraseña en la base de datos.

---

## ▶️ Cómo usarlo

1. Abrí `index.html` en el navegador (o la URL publicada).
2. En **Cliente**, marcá tu punto de espera y ponete un nombre.
3. Cambiá a **Conductor**, ingresá la clave, elegí o registrá una buseta y activá el GPS.
4. Volvé a **Cliente** para ver cómo cambia el ETA mientras la buseta se mueve.

---

## 🛠️ Tecnología

- HTML + CSS + JavaScript (sin frameworks, un solo archivo).
- [Leaflet](https://leafletjs.com/) + [OpenStreetMap](https://www.openstreetmap.org/) para el mapa.
- API de geolocalización del navegador para el GPS real.

---

## 🚀 Publicar en Vercel

1. Subí este repositorio a GitHub.
2. En [vercel.com](https://vercel.com): **Add New → Project** e importá el repo.
3. Vercel lo detecta como sitio estático y publica `index.html` automáticamente.

---

## 🗺️ Camino a producción

Para convertir esto en una app real con datos compartidos entre dispositivos:

- **Backend en tiempo real:** Supabase (Postgres + Realtime + Auth) o Firebase.
- **App del conductor:** envía su GPS cada pocos segundos (PWA o app nativa).
- **App/vista del cliente:** se suscribe a las posiciones y publica su punto de espera.
- **Autenticación real** de conductores (usuario/contraseña o verificación por teléfono).
- **Persistencia** de rutas, unidades, pasajeros e historial.

---

## 📄 Licencia

Uso libre para fines de demostración y desarrollo del proyecto.
