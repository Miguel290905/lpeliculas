# iPeliculas

Aplicación Android en Java con una cartelera de cine.
Proyecto evaluable de Programación Multimedia y Dispositivos Móviles (2º CFGS DAM, IES Francisco Rodríguez Marín).

Autor: Miguel Angel Gonzalez Ortega

## Funcionalidades

- Cartelera con `RecyclerView` animado y tarjetas (`CardView`): título, género, valoración y sinopsis corta.
- Detalle de cada película y pantalla "Acerca de", con botón de retroceso.
- Menú de la Toolbar: **Acerca de**, **Ir al cine** y **Cómo llegar**.
- API de Google integrada en la app: Maps SDK (mapa), Places API (cines cercanos) y Routes API (ruta en coche).
- Extras: buscador, favoritas (guardadas y mostradas primero), filtro "Solo favoritas" y botón de compartir.

## Clave de la API de Google (MAPS_API_KEY)

El mapa, los cines y las rutas necesitan una clave de API propia. **No se incluye en el repositorio**: cada persona usa la suya.

### Cómo conseguirla

1. Entrar en [Google Cloud Console](https://console.cloud.google.com/) con una cuenta de Google y crear un proyecto.
2. Activar la facturación del proyecto (la prueba gratuita incluye crédito; conviene crear una alerta de presupuesto).
3. En *APIs y servicios > Biblioteca*, habilitar estas tres APIs:
   - **Maps SDK for Android** (dibuja el mapa).
   - **Places API (New)** (busca los cines cercanos).
   - **Routes API** (calcula la ruta en coche).
4. En *APIs y servicios > Credenciales*, pulsar *Crear credenciales > Clave de API* y copiarla. Se recomienda restringirla solo por API (las tres anteriores) y no por aplicación Android, porque Places y Routes se llaman por HTTP.

### Dónde se pone y dónde se usa

1. Copiar `local.properties.example` como `local.properties` (en la raíz del proyecto, junto a `settings.gradle`) y escribir la clave:
   ```
   MAPS_API_KEY=tu_clave
   ```
   Si Android Studio ya creó `local.properties`, basta con añadir esa línea sin borrar `sdk.dir`. Este archivo está en `.gitignore`, así que no se sube a GitHub.
2. `app/build.gradle` lee `MAPS_API_KEY` de `local.properties` y la reparte de dos formas:
   - `manifestPlaceholders`: sustituye `${MAPS_API_KEY}` en `AndroidManifest.xml`, en el `meta-data` `com.google.android.geo.API_KEY`. Con ella el Maps SDK dibuja el mapa.
   - `buildConfigField`: crea la constante `BuildConfig.MAPS_API_KEY`, que usa `MapActivity` para llamar a Places (`CinemaFinder`) y a Routes (`RouteFinder`).
3. Si la clave falta, `MapActivity` lo detecta (`BuildConfig.MAPS_API_KEY.isEmpty()`) y muestra un aviso en lugar del mapa.
4. Después de cambiar la clave, pulsar *Sync Now* y volver a ejecutar la app.

## Cómo ejecutarlo

1. Abrir el proyecto con Android Studio (Koala 2024.1.1 o superior, JDK 17) y esperar a la sincronización de Gradle.
2. Ejecutar en un emulador con imagen Google Play (API 34 recomendada) o en un móvil con Android 8.0 o superior, con internet.
3. En el emulador, fijar la ubicación en *Extended controls > Location* para probar "Ir al cine" y "Cómo llegar".
