# AREQUIPA — La Ciudad Blanca, en otra dimensión

Experiencia web inmersiva inspirada en Arequipa, Perú. Reinterpreta el proyecto de referencia de Mollendo/Islay —scrollytelling, capas visuales, recorrido narrativo y una identidad editorial— como una landing 3D con control por voz y reconocimiento de gestos.

## Qué incluye

- **Escena 3D en tiempo real:** Three.js con volcanes y silueta urbana construidos proceduralmente, iluminación atmosférica, niebla, partículas, parallax con el puntero y adaptación al tamaño de pantalla.
- **Narrativa por desplazamiento:** portada, territorio, patrimonio/experiencias, centro de interacción y cierre.
- **AURA, guía por voz:** reconocimiento de voz del navegador (`SpeechRecognition` cuando está disponible) y respuestas habladas con `SpeechSynthesis`. Comprende un conjunto acotado de intenciones sobre el Misti, el sillar, las experiencias y la navegación; no es un LLM ni requiere una API de pago.
- **IA de gestos:** MediaPipe Gesture Recognizer ejecutado en el navegador. Una mano abierta avanza, un puño retrocede, pulgar arriba muestra una respuesta y el signo de paz abre la guía.
- **Accesibilidad básica:** textos alternativos, estados de interacción, mensajes de estado, controles etiquetados y respeto por `prefers-reduced-motion`.
- **README y estructura** preparados para abrir en VS Code y ampliar.

## Requisitos

- Chrome o Edge actualizado recomendado.
- Conexión a internet para descargar Three.js, MediaPipe, el modelo de gestos y las fuentes.
- Cámara y permiso explícito del usuario para usar los gestos.
- Para micrófono/cámara, ejecutar desde `localhost` o un sitio HTTPS. Abrir el HTML directamente con `file://` no es el modo recomendado porque los módulos ES y los permisos del navegador pueden bloquearse.

## Ejecutar localmente

1. Descomprime el proyecto y abre la carpeta `arequipa-immersive` en VS Code.
2. Inicia un servidor local. Con la extensión **Live Server**, pulsa **Go Live**; o usa Python:

   ```bash
   python -m http.server 8000
   ```

3. Abre `http://localhost:8000`.
4. Para voz, pulsa **Hablar con AURA** y después **Activar micrófono**. Acepta el permiso del navegador y prueba frases como:
   - «Llévame al Misti»
   - «Háblame del sillar»
   - «Ir a experiencias»
   - «Volver al inicio»
5. Para gestos, pulsa **Control por gestos**, acepta el acceso a la cámara y coloca una mano dentro del encuadre.

## Mapa de archivos

```text
arequipa-immersive/
├── index.html          # Estructura de la experiencia y paneles de interacción
├── README.md           # Documentación
├── src/
│   ├── app.js           # Three.js, navegación, voz y MediaPipe
│   └── styles.css       # Diseño responsive, efectos y estados visuales
└── assets/              # Reservado para recursos locales adicionales
```

## Controles por gesto

| Gesto detectado | Acción |
|---|---|
| Mano abierta (`Open_Palm`) | Siguiente sección |
| Puño (`Closed_Fist`) | Sección anterior |
| Pulgar arriba (`Thumb_Up`) | Mensaje positivo de AURA |
| Signo de paz (`Victory`) | Abrir guía AURA |
| Dedo arriba (`Pointing_Up`) | Volver a portada |

El reconocimiento usa puntuación de confianza y una pausa entre activaciones para reducir disparos repetidos. La detección depende de la iluminación, el encuadre y el rendimiento del dispositivo.

## Arquitectura y tecnologías

- HTML semántico + CSS responsive.
- JavaScript ES modules.
- [Three.js](https://threejs.org/) para gráficos 3D WebGL.
- [MediaPipe Tasks Vision](https://ai.google.dev/edge/mediapipe/solutions/vision/gesture_recognizer/web_js) para clasificación de gestos.
- Web Speech API del navegador para voz de entrada y salida.
- Fuentes DM Sans y Manrope servidas por Google Fonts.

## Personalización

- **Escena:** `initScene()`, `makeMountain()` y las luces están en `src/app.js`.
- **Comandos hablados:** amplía `answerIntent()` en `src/app.js`.
- **Gestos:** ajusta el bloque `detectGestures()` y la confianza mínima (`g.score > .65`).
- **Estética:** colores, tipografía, tarjetas y breakpoints están en `src/styles.css`.
- **Contenido:** edita el texto de `index.html`.

## Notas importantes

- La escena 3D es una interpretación procedural estilizada, no un modelo topográfico exacto ni una reproducción geográfica a escala.
- El control por voz se apoya en las capacidades disponibles en el navegador; la compatibilidad varía entre navegadores y sistemas operativos.
- La guía AURA responde mediante reglas locales y frases predefinidas. Para convertirla en una conversación generativa, integra un backend propio con una API de IA y protege las claves en el servidor; nunca incluyas secretos en el JavaScript del cliente.
- MediaPipe descarga el modelo desde un recurso externo la primera vez. La cámara se inicia solo tras una acción explícita y se detiene al cerrar el panel o abandonar la página.
- Para producción, añade pruebas en dispositivos reales, una política de privacidad clara, analítica solo con consentimiento y una estrategia de fallback para equipos sin WebGL.

## Créditos

Proyecto educativo/conceptual de experiencia digital para Arequipa. La escena principal se genera con geometría 3D en código; las dependencias externas se cargan desde sus CDNs oficiales o de distribución.
