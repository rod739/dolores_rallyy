# 🔔 Rally de la Independencia AR — Dolores Hidalgo

Juego de realidad aumentada para recorrer Dolores Hidalgo: radar, frío o caliente, trivia histórica, álbum coleccionable y modo demo. Desarrollado con **TDD** (Rojo → Verde → Refactor).

| | |
|---|---|
| **Alumna** | Yaneli Sánchez Cárdenas |
| **Grupo** | GTID244 |
| **Materia** | Desarrollo de Aplicaciones Móviles |
| **Docente** | Prof. Anastacio Rodríguez García |
| **Actividad** | 7. Caliente-Frío Dolores AR |
| **Universidad** | UTNG |

---

## 🗺️ Estructura del proyecto

```
dolores_rally/
  lib/
    dominio/                 <- reglas del juego (Dart puro, sin Flutter)
      geo.dart               <- distancia, rumbo, ángulos y destino
      personaje.dart         <- Personaje y Pregunta
      catalogo.dart          <- 6 personajes de la Independencia
      aparicion.dart         <- dónde se esconde cada personaje (azar inyectado)
      radar.dart             <- puntoEnRadar y frío/caliente
      motor_ar.dart          <- qué se ve en pantalla y dónde
      partida.dart           <- reglas de captura, puntos y huida
      almacen.dart           <- contrato para guardar el progreso
      fuentes.dart           <- contratos de GPS y brújula
      excepciones.dart       <- errores con mensajes amables
    datos/
      fuentes_reales.dart    <- GPS y brújula del teléfono
      fuentes_simuladas.dart <- modo demo (caminar con botones)
      almacen_prefs.dart     <- guarda el álbum con shared_preferences
    presentacion/
      rally_viewmodel.dart   <- junta todo (ChangeNotifier)
      fondo_camara.dart      <- cámara trasera o degradado
      radar_widget.dart      <- radar con CustomPainter
      pantalla_rally.dart    <- pantalla del juego
      pantalla_coleccion.dart<- álbum de personajes
      pantalla_inicio.dart   <- menú principal
    main.dart
  test/
    ayudas.dart
    geo_test.dart
    aparicion_test.dart
    radar_test.dart
    motor_ar_test.dart
    partida_test.dart
    rally_viewmodel_test.dart
  capturas/                  <- evidencias (imágenes)
```

## ▶️ Cómo ejecutarlo

```bash
flutter create .                # genera android/, ios/, etc. (no borra lo que ya existe)
del test\widget_test.dart       # (Windows) borra la prueba de ejemplo
flutter pub get
flutter test                    # corre todas las pruebas
flutter run                     # abre la app
```

---

## 📸 Evidencias

### Ronda 0 · Preparar el taller
Proyecto creado, paquetes instalados (`camera`, `geolocator`, `flutter_compass`, `shared_preferences`) y permisos de cámara y ubicación en `AndroidManifest.xml`.

![Estructura del proyecto en VS Code](capturas/00_estructura.png)

### Ronda 1 · Diseño del juego
| Pregunta | Respuesta |
|---|---|
| Objetivo | Atrapar a los 6 personajes de la Independencia |
| ¿Cómo se encuentran? | Radar, frío o caliente y vista RA |
| ¿Cuándo se atrapan? | A 15 m o menos |
| ¿Cómo se atrapan? | Respondiendo bien una pregunta de historia |
| Si fallas | 2 intentos; con 2 fallos el personaje huye y reaparece |
| Puntos | 100 a la primera, 50 a la segunda |
| ¿Cómo se gana? | Atrapando a los 6 (se guarda en el álbum) |
| Distancia | Entre 60 y 500 m del Jardín Principal |

**Escala de temperatura:** 🌋 Ardiente ≤ 15 m · 🔥 Caliente ≤ 50 m · 🙂 Tibio ≤ 150 m · ❄️ Frío ≤ 400 m · 🥶 Helado > 400 m

### Ronda 2 · Geometría de la Tierra (`geo_test.dart`)
🔴 Rojo (falla porque `geo.dart` no existe):

![geo rojo](capturas/02_geo_rojo.png)

🟢 Verde (pasa la prueba de ida y vuelta en 9 direcciones):

![geo verde](capturas/02_geo_verde.png)

### Ronda 3 · Personajes y preguntas
Catálogo con 6 personajes: Miguel Hidalgo, Ignacio Allende, Josefa Ortiz de Domínguez, Juan Aldama, Mariano Abasolo y Leona Vicario.

### Ronda 4 · Esconder personajes (`aparicion_test.dart`)
![aparicion rojo](capturas/04_aparicion_rojo.png)
![aparicion verde](capturas/04_aparicion_verde.png)

### Ronda 5 · Radar y frío o caliente (`radar_test.dart`)
![radar rojo](capturas/05_radar_rojo.png)
![radar verde](capturas/05_radar_verde.png)

### Ronda 6 · Motor de realidad aumentada (`motor_ar_test.dart`)
![motor rojo](capturas/06_motor_rojo.png)
![motor verde](capturas/06_motor_verde.png)

### Ronda 7 · Reglas del juego (`partida_test.dart`)
![partida rojo](capturas/07_partida_rojo.png)
![partida verde](capturas/07_partida_verde.png)

### Ronda 8 y 9 · Álbum y fuentes de posición
Contrato `AlmacenColeccion` (memoria y shared_preferences) y fuentes reales/simuladas de GPS y brújula.

### Ronda 10 · ViewModel (`rally_viewmodel_test.dart`)
![viewmodel rojo](capturas/10_viewmodel_rojo.png)
![viewmodel verde](capturas/10_viewmodel_verde.png)

### ✅ Todas las pruebas pasando
![flutter test](capturas/todas_las_pruebas.png)

### Ronda 11 · La app funcionando (modo demo)
| Inicio | Juego / radar | Personaje cerca (¡Tócame!) |
|---|---|---|
| ![](capturas/11_inicio.png) | ![](capturas/11_juego.png) | ![](capturas/11_tocame.png) |

| Pregunta | Se escapó (2 fallos) | Álbum |
|---|---|---|
| ![](capturas/11_pregunta.png) | ![](capturas/11_escapo.png) | ![](capturas/11_album.png) |

### Ronda 12 · Prueba de campo, balanceo y seguridad
- **Seguridad física:** la pantalla de inicio muestra un aviso; jugar en grupo y evitar cruzar calles.
- **Privacidad:** la ubicación nunca sale del teléfono; el progreso guardado solo tiene ids y puntaje.
- **Números ajustables:** radio de captura (15 m), radio de aparición (60–500 m), umbrales de temperatura, campo de visión (60°) y alfa de la brújula (0.2).

---

## ✔️ Checklist de calidad
- [x] Las reglas del juego viven en `dominio/` y no importan Flutter ni plugins.
- [x] El azar está inyectado: las pruebas usan semilla fija y verifican propiedades.
- [x] La función `destino` se probó en 9 direcciones (ida y vuelta).
- [x] Los resultados de captura son una `sealed class` y el switch es exhaustivo.
- [x] El radar y la temperatura tienen pruebas de orientación y de umbrales.
- [x] El progreso guardado contiene solo ids y puntaje (nada de coordenadas).
- [x] El modo demo permite jugar sin GPS ni brújula.
- [x] La app no se rompe sin cámara, sin permiso o sin brújula.
- [x] Se cancelan suscripciones y se libera la cámara en `dispose()`.
- [x] Hay aviso de seguridad física.

## 💬 Preguntas para discutir
1. **¿Por qué inyectar `Random`?** Porque en las pruebas se usa una semilla fija (`Random(42)`) y el "azar" siempre da lo mismo, así las pruebas son confiables.
2. **¿Qué pasa sin la raíz cuadrada?** Los personajes se amontonarían cerca del centro, porque los círculos pequeños tienen menos área que los grandes.
3. **¿Por qué una prueba de "ida y vuelta"?** Porque compara una función con su inversa sin tener que calcular valores a mano, y revisa todas las direcciones.
4. **¿Ventaja de la `sealed class`?** Cada resultado lleva sus propios datos y el compilador obliga a cubrir todos los casos en el `switch`.
5. **El GPS dice 18 m pero lo veo enfrente:** subiría el radio de captura (15 → 20 m) y revisaría los umbrales de temperatura, porque el GPS tiene un error de 5 a 20 m.
6. **Riesgos de un ranking en línea:** hay apps que falsifican el GPS; el servidor no debe confiar en las coordenadas y tiene que validar velocidades y saltos imposibles.

> ⚠️ Los datos históricos y coordenadas son de práctica; se deben verificar con fuentes locales antes de usar el juego con alumnos.
