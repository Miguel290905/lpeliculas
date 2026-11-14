# iPeliculas

Aplicación Android en Java que muestra una cartelera de cine.
Proyecto evaluable de **Programación Multimedia y Dispositivos Móviles** (2º CFGS DAM, IES Francisco Rodríguez Marín).

Autor: Miguel Angel Gonzalez Ortega

## Funcionalidades

**Requisitos del enunciado**

- `MainActivity`: cartelera con `RecyclerView` animado (las tarjetas entran deslizándose desde abajo y apareciendo gradualmente). Cada tarjeta (`CardView`) muestra título, género, valoración (`RatingBar`) y sinopsis corta.
- `DetailActivity`: título, género, calificación, sinopsis completa, director y fecha de estreno, con botón de retroceso en la Toolbar.
- `AboutActivity` ("Acerca de"): información del desarrollador y de la app, con botón de regreso.
- Menú de la Toolbar: **Acerca de**, **Ir al cine** y **Cómo llegar**.
- **API de Google integrada en la app** (no se abre el navegador ni la app de Google Maps):
  - **Maps SDK for Android**: el mapa se muestra dentro de `MapActivity`.
  - **Places API (New)**: busca los cines cercanos; el más cercano se marca en amarillo.
  - **Routes API**: en "Cómo llegar" dibuja la ruta en coche desde tu ubicación hasta el cine y muestra distancia y tiempo.
- Diseño oscuro con detalles dorados y rojos, responsive (vertical, horizontal, móvil y tablet).

**Extras añadidos**

- Buscador por título o género (ignora mayúsculas y tildes).
- Películas favoritas con persistencia (`SharedPreferences`) y filtro "Solo favoritas".
- Botón para compartir una película.
- Solicitud del permiso de ubicación en tiempo de ejecución (si se rechaza, se usa una ubicación por defecto).
- Mensaje cuando no hay resultados.
- En "Cómo llegar" se puede pulsar otro cine del mapa para calcular su ruta.

## Estructura

```
app/src/main/java/com/example/ipeliculas/
├── MainActivity.java      Cartelera y menú
├── DetailActivity.java    Detalle de una película
├── MapActivity.java       Mapa de Google, cines cercanos y ruta
├── AboutActivity.java     Acerca de
├── model/                 Movie, Cinema, RouteInfo
├── data/                  MovieRepository (datos) y FavoritesManager (favoritas)
├── adapter/MovieAdapter.java   Adapter del RecyclerView y filtros
├── network/               CinemaFinder (Places), RouteFinder (Routes), HttpHelper, BackgroundRunner
└── util/                  LocationHelper y PolylineDecoder
```

## Clave de la API de Google (imprescindible para el mapa)

1. En [Google Cloud Console](https://console.cloud.google.com/) crea un proyecto y activa la facturación (hay crédito gratuito mensual).
2. En *APIs y servicios > Biblioteca* habilita: **Maps SDK for Android**, **Places API (New)** y **Routes API**.
3. En *Credenciales* crea una **clave de API**. Para este proyecto, restringe la clave solo por API (esas tres) y no por aplicación Android, porque Places y Routes se llaman por HTTP.
4. Copia `local.properties.example` como `local.properties` (raíz del proyecto) y escribe `MAPS_API_KEY=tu_clave`. Si Android Studio ya creó `local.properties`, añade solo esa línea.
5. `local.properties` está en `.gitignore`: **la clave no se sube a GitHub**.

## Cómo ejecutarlo

1. Abrir la carpeta del proyecto con **Android Studio** (Koala 2024.1.1 o superior, JDK 17).
2. Configurar la clave de la API (apartado anterior) y esperar a que termine la sincronización de Gradle.
3. Ejecutar en un emulador con imagen **Google Play** (o `Google APIs`) o en un móvil con Android 8.0 (API 26) o superior, con conexión a internet.
4. En el emulador, fijar la ubicación en *Extended controls > Location* para probar "Ir al cine" y "Cómo llegar".
