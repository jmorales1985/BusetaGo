# 🚐 BusetaGo

Prototipo web de **rastreo de busetas en tiempo real** para empresas de transporte.
Los pasajeros ven por dónde viene su buseta y marcan dónde están esperando; los
conductores se registran con clave, eligen su unidad y comparten su ubicación GPS.

> ⚠️ **Versión funcional.** Todo corre en un solo archivo `index.html`. Las busetas se
> mueven de forma simulada sobre un mapa real (distrito de **Sabanilla de Alajuela**, Costa Rica).
> Para un producto real con sincronización entre varios teléfonos se necesita un
> backend (ver más abajo).

---

## 🗺️ Rutas

Esquema tipo escolar con Sabanilla como punto central:

- Sabanilla → Los Ángeles / Los Ángeles → Sabanilla
- Sabanilla → San Luis / San Luis → Sabanilla
- Sabanilla → El Cerro / El Cerro → Sabanilla

> Las coordenadas están ubicadas en el **distrito de Sabanilla de Alajuela** (rumbo al
> Poás). Sabanilla y Los Ángeles vienen de OpenStreetMap; San Luis y El Cerro son
> aproximados y se ajustan a los pines reales de cada comunidad.

---

## ✨ Funcionalidades

### Vista Cliente
- Mapa en vivo con las busetas activas y su ruta.
- Botón **"Marcar dónde espero"**: al tocar el mapa se pide el **nombre** del pasajero.
- Botón **"Usar mi ubicación"** (GPS del navegador), también pide el nombre.
- **ETA estimado** de cada buseta hasta el punto de espera, actualizado en vivo.

### Vista Conductor
- **Acceso con clave** (login).
- Selección de la buseta que maneja hoy y **registro de nuevos conductores** (nombre, placa y ruta).
- Interruptor **"Compartir mi ubicación (GPS real)"**.
- Lista de **pasajeros esperando** con nombre, distancia y ETA, con botón **Recoger**.
- Sección **A bordo** con **selección múltiple** (casillas): dejar uno, varios o **todos**.
- Al dejar, se registra automáticamente la **ubicación actual** de la buseta como lugar de
  entrega (con su dirección), y los pasajeros desaparecen del mapa. Sin pasos extra.

### Vista Administrador
- **Acceso con clave** propio.
- **Historial de entregas** que se **guarda en el dispositivo** y se conserva aunque se recargue.
- Filtros **Hoy / Este mes / Todo** para sacar reportes por período.
- Cada entrega: fecha y hora, pasajero, **dirección de recogida** y **dirección de entrega**
  (por geocodificación inversa de la ubicación real), conductor y placa.
- Resumen (entregas, pasajeros y conductores del período).
- **Exportar a CSV** del período seleccionado (incluye direcciones y coordenadas).

### General
- Menús **contraíbles** (tocá el encabezado o la barra) para ver el mapa a pantalla completa en el celular.
- Todo se actualiza **en tiempo real**: posiciones, distancias y ETAs.

---

## 🔑 Accesos

Las vistas de **Conductor** y **Administrador** están protegidas por clave. Las claves
se configuran en el código (`DRIVER_KEY` y `ADMIN_KEY`) y no se publican aquí.

> En producción cada usuario tendría su propia cuenta y contraseña en la base de datos.

---

## ▶️ Cómo usarlo

1. Abrí `index.html` en el navegador (o la URL publicada).
2. En **Cliente**, marcá tu punto de espera y ponete un nombre.
3. En **Conductor**, ingresá la clave, elegí una buseta, **recogé** pasajeros y luego **dejalos**: la entrega se guarda con la ubicación actual.
4. En **Admin**, ingresá la clave para ver y exportar los **reportes**.

---

## 🛠️ Tecnología

- HTML + CSS + JavaScript (sin frameworks, un solo archivo).
- [Leaflet](https://leafletjs.com/) + [OpenStreetMap](https://www.openstreetmap.org/) para el mapa.
- API de geolocalización del navegador para el GPS real.
- Geocodificación inversa con **Nominatim (OpenStreetMap)** para convertir coordenadas en direcciones. En producción conviene un proveedor con llave y límites propios (Google, Mapbox o un Nominatim propio).

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

Uso libre para el desarrollo del proyecto.
