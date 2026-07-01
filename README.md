# TURING, PLEASE

Juego tipo *Papers, Please* en el navegador (Three.js, un solo `index.html`).
Trabajás la última mesa de verificación "Humanity-as-a-Service": leés el chat de
cada solicitante, lo cotejás con la **directiva** del día y sellás **HUMAN** o **BOT**.

## Estructura
```
index.html   · juego completo (HTML + CSS + JS). Editá acá.
```
Dependencias por CDN (jsdelivr / cdnjs), no hay assets locales:
- Three.js core (r128) desde cdnjs, en el `<head>`.
- `GLTFLoader` (r128) desde jsdelivr:
  `three@0.128.0/examples/js/loaders/GLTFLoader.js`
- Modelos CC0 desde jsdelivr (`three@0.128.0/examples/models/gltf/`),
  usados como **cameo de alta fidelidad** (entidades con `useModel`):
  - robot → `RobotExpressive/RobotExpressive.glb`
  - humano → `Soldier.glb`

## Personajes
El grueso del elenco es **procedural** (se dibuja en código, ver `buildBot`/`buildHuman`
y `applyHumanStyle`), así siempre carga y hay variedad sin depender del CDN:
- **Robots célebres** (`style`): T-800, R2-D2, WALL-E, Optimus, HAL 9000, Bender,
  C-3PO, Marvin, Roomba, GLaDOS, y variantes genéricas (sentry/chrome/scrap).
- **Humanos** con accesorios (`shades`, `cap`, `beanie`, `hoodie`, `suit`, `crown`).
- **Bots encubiertos** que se hacen pasar por humanos y se delatan con un *tell*
  (delve, em-dash, emoji, "as an AI language model", etc.).

7 días, con reglas que aparecen y se revocan (arms race): DELVE, HELP, ANGER,
EMDASH, TYPO, EMOJI, VAGUE, SLOP y MORTAL. Reglas y roster: `RULES`, `SCHEDULE`, `POOL`.

## Correr / previsualizar
Los modelos GLTF (cameos) y las libs se cargan por URL, así que **conviene servirlo por
HTTP** (Render, o el preview del server local). Si el CDN no responde, cae solo a los
personajes procedurales (no rompe).

## Deploy en Render
Static Site → conectar este repo → **Publish Directory: `.`** → sin build command.
Push a `main` = redeploy automático.

## Perillas de ajuste (buscá el nombre en `index.html`)
- Distancia del solicitante: `g.position.set(0,0,-1.7)` en `spawnApplicant` (más
  negativo = más lejos).
- `ROT_HUMAN` / `ROT_ROBOT`: poné `Math.PI` si un cameo GLTF mira al lado contrario.
- Encuadre de cámara: `camBaseY` y `camera.lookAt(...)` dentro de `animate`.
- DNI presentado: el vector `sx,sy,sz` (~`0,1.48,0.35`) dentro de `animate`.
- Dificultad/economía: objeto `CFG` (plata, multas, `patienceSec`, strikes).
- Reglas por día y roster de solicitantes: `SCHEDULE` y `POOL`.
- Nuevos personajes: agregá un `ent({...})` a `POOL` con `avatar`, `style`, `features`
  y líneas; para un robot nuevo, sumá un `botXxx()` y su caso en `buildBot`.
- Detalles de escritorio (radio, lápices, taza, sellos): en `buildRoom`.

## Assets
Modelos CC0 (three r128 examples) servidos desde jsdelivr, sólo para cameos.
Para otros, cambiá `ROBOT_URL` / `HUMAN_URL` en `index.html` por cualquier `.glb`
low-poly (idealmente con animación `Idle`). El resto del elenco es procedural.
