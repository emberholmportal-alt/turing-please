# TURING, PLEASE

Juego tipo *Papers, Please* en el navegador (Three.js, un solo `index.html`).
Trabajás la última mesa de verificación "Humanity-as-a-Service": leés el chat de
cada solicitante, lo cotejás con la **directiva** del día y sellás **HUMAN** o **BOT**.

## Estructura
```
index.html        · juego completo (HTML + CSS + JS). Editá acá.
js/GLTFLoader.js   · loader de modelos (r128, local)
models/robot.glb   · modelo de los bots (CC0, Quaternius)
models/human.glb   · modelo de los humanos (CC0)
```
Three.js core se carga desde cdnjs (r128) en el `<head>`.

## Correr / previsualizar
Los modelos se cargan por URL (`models/*.glb`), así que **necesita servirse por HTTP**
(no funciona abriendo el archivo con doble clic por file://). Opciones:
- Deploy en Render (abajo).
- Preview del servidor local de Claude Code.
Si un modelo no carga, cae solo a personajes procedurales (no rompe).

## Deploy en Render
Static Site → conectar este repo → **Publish Directory: `.`** → sin build command.
Push a `main` = redeploy automático.

## Perillas de ajuste (arriba del `<script>` o buscando el nombre)
- `ROT_HUMAN` / `ROT_ROBOT`: poné `Math.PI` si un modelo mira para el lado contrario.
- Encuadre de cámara: `camBaseY` y `camera.lookAt(...)` dentro de `animate`.
- DNI presentado: el vector `sx,sy,sz` (~`0,1.48,0.35`) dentro de `animate`.
- Dificultad/economía: objeto `CFG` (plata, multas, `patienceSec`, strikes).
- Reglas por día y roster de solicitantes: `SCHEDULE` y `POOL`.

## Assets
Modelos CC0. Sustituibles por cualquier `.glb` low-poly manteniendo los nombres.
