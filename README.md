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
- `GLTFLoader` (r128) desde jsdelivr (`npm/three@0.128.0/examples/js/loaders/`).
- **Modelos GLB** (CC0 / licencia permisiva) desde jsdelivr, sirviendo los repos de
  GitHub (`gh/mrdoob/three.js@r128/...` y `gh/KhronosGroup/glTF-Sample-Models@master/...`).
  Nota: el paquete **npm** de three NO incluye los `.glb` (dan 404); por eso se usa
  el mirror de GitHub, que sí tiene los binarios.

## Modelos y personajes
El elenco principal usa **modelos GLB profesionales** (riggeados y animados),
recoloreados por entidad para dar variedad (`CHAR_ASSETS`, `assetFor`, `recolorRig`):
- **Humanos** → `Soldier.glb`, `CesiumMan.glb`.
- **Robots genéricos** → `RobotExpressive.glb` (animado, expresivo).

Enfoque **mixto**: los robots de marca (T-800, R2-D2, WALL-E, HAL 9000, Bender,
C-3PO, Marvin, Roomba, GLaDOS…) no existen como GLB con licencia en ningún CDN, así
que se dibujan **procedural** (`buildBot` + `botXxx()`, listados en `PROC_STYLES`).
Si un modelo no carga, todo cae a procedural (no rompe).

**Props GLB de escena** (`PROP_ASSETS`, `loadProps`):
- `DamagedHelmet.glb` → cabeza sintética confiscada, girando sobre el escritorio.
- `2CylinderEngine.glb` → maquinaria industrial de fondo (repintada oscura).

7 días con reglas que aparecen y se revocan (arms race): DELVE, HELP, ANGER,
EMDASH, TYPO, EMOJI, VAGUE, SLOP y MORTAL. Reglas y roster: `RULES`, `SCHEDULE`, `POOL`.

## Correr / previsualizar
Los modelos y las libs se cargan por URL, así que **conviene servirlo por HTTP**
(Render, o el preview del server local). Si el CDN no responde, cae solo a los
personajes procedurales (no rompe).

## Deploy en Render
Static Site → conectar este repo → **Publish Directory: `.`** → sin build command.
Push a `main` = redeploy automático.

## Perillas de ajuste (buscá el nombre en `index.html`)
- Modelos GLB: `CHAR_ASSETS` (personajes: `url`, `h` altura, `rot` giro para mirar a
  cámara, `z` distancia, `idle` clip) y `PROP_ASSETS` (props de escena).
- Qué robots quedan procedurales (de marca): el set `PROC_STYLES`.
- Distancia del solicitante procedural: `g.position.set(0,0,-1.7)` en `spawnApplicant`
  (más negativo = más lejos). Para GLB, el `z` de cada asset en `CHAR_ASSETS`.
- Encuadre de cámara: `camBaseY` y `camera.lookAt(...)` dentro de `animate`.
- DNI presentado: el vector `sx,sy,sz` (~`0,1.48,0.35`) dentro de `animate`.
- Dificultad/economía: objeto `CFG` (plata, multas, `patienceSec`, strikes).
- Reglas por día y roster de solicitantes: `SCHEDULE` y `POOL`.
- Nuevos personajes: agregá un `ent({...})` a `POOL`; para un robot de marca nuevo,
  sumá un `botXxx()`, su caso en `buildBot` y el `style` a `PROC_STYLES`.
- Props / detalles de escritorio (radio, lápices, taza, sellos): en `buildRoom`;
  posición/escala de los props GLB en `loadProps`.

## Assets
Modelos GLB con licencia permisiva (three r128 examples + Khronos glTF-Sample-Models),
servidos por jsdelivr. Para sumar otros, agregá una entrada a `CHAR_ASSETS` con su
`url` y ajustá `h` / `rot` / `z` (idealmente un `.glb` riggeado con clip `Idle`).
