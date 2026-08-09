# 🚐 BusetaGo

Prototipo web de **rastreo de busetas en tiempo real** para empresas de transporte.
Los pasajeros ven por dónde viene su buseta y marcan dónde están esperando; los
conductores se registran con clave, eligen su unidad y comparten su ubicación GPS.

> ⚠️ **Versión funcional.** Todo corre en un solo archivo `index.html`. Las busetas se
> mueven de forma simulada sobre un mapa real (distrito de **Sabanilla de Alajuela**, Costa Rica).
> Para un producto real con sincronización entre varios teléfonos se necesita un
> backend (ver más abajo).

---

## ☁️ Base de datos central (Supabase)

Para que **varios conductores** registren entregas y el **admin** las vea todas juntas y
en tiempo real, la app usa [Supabase](https://supabase.com) (Postgres + Realtime).

Configuración:

1. Creá un proyecto en supabase.com (plan gratis).
2. En **SQL Editor**, pegá y ejecutá, en orden: `supabase.sql` (entregas),
   `supabase-posiciones.sql` (GPS en vivo) y `supabase-esperando.sql` (pasajeros esperando).
3. En **Project Settings → API**, copiá el **Project URL** y la **anon/publishable key**.
4. Pegalos en `index.html`, al inicio del script:
   ```js
   const SUPABASE_URL      = 'https://xxxxxxxx.supabase.co';
   const SUPABASE_ANON_KEY = 'eyJ...';
   ```
5. Subí el `index.html` al repo → Vercel redepliega.

Si los valores quedan vacíos, la app sigue funcionando pero guarda el historial solo en el
dispositivo (respaldo local).

> **Nota de seguridad:** la *anon key* es pública por diseño (va en el navegador) y el acceso
> se controla con las políticas RLS. En esta etapa las políticas son abiertas (MVP). El
> siguiente endurecimiento es **Supabase Auth** con cuentas reales de conductor y admin.

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
- Cuando un conductor comparte su GPS, su buseta se mueve **en tiempo real** en el mapa de
  todos los dispositivos (vía Supabase). Las busetas sin GPS activo se muestran simuladas.
- Botón **"Marcar dónde espero"**: al tocar el mapa se pide el **nombre** del pasajero.
  El punto se comparte en tiempo real y el conductor lo ve desde su dispositivo (vía Supabase).
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
