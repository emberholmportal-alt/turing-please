# PUMP, PLEASE

Juego tipo *Papers, Please* en el navegador (Three.js, un solo `index.html`).
Sos el único humano en el loop de una Solana convertida en ciudad-casino gobernada
por máquinas. En el último filtro de listing, cada memecoin manda un representante:
leés su pitch pero **decidís por la ficha on-chain**. Dos botones circulares —
**PUMP** (verde, listar) / **RUG** (rojo, reciclar) — y en cada decisión estallan
luces y papelitos. La máquina corre a silicio; vos la alimentás.

Reskin del motor *Turing, Please* (mismo engine: modelos GLB por CDN, documento 3D
canvas-textura, oficina 3D, fallback procedural, mobile-first). La fuente de verdad
del diseño es [`DESIGN.md`](DESIGN.md).

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

Cada coin tiene un `archetype` (mascotDog/Frog/Cat, devHoodie, devDoxxed, aiBot,
alienVC, kol, granny, rugger, volumeBot) que se mapea a modelo humano o robot y se
recolorea. Si un modelo no carga, todo cae a procedural (no rompe).

**Props GLB de escena** (`PROP_ASSETS`, `loadProps`):
- `DamagedHelmet.glb` → cabeza sintética confiscada, girando sobre el escritorio.
- `2CylinderEngine.glb` → maquinaria industrial de fondo (repintada oscura).

## Jugabilidad (PUMP, PLEASE)
- Cada solicitante es una **coin** (`COINS`) con métricas on-chain. Las **flags**
  (`FLAGS` + `coinFlags`) se derivan de las métricas: `DEV_HEAVY`, `LIQ_UNLOCKED`,
  `MINT_LIVE`, `BUNDLED`, `LOW_HOLDERS`, `SNIPER_TOP`.
- La **directiva del día** (`SCHEDULE`) dice qué flags obligan a **RUG**. Todo lo
  que no viola la política se **PUMP**. `correctVerdict` = RUG si hay flag activa,
  si no PUMP. 5 días, escalada estilo arms-race (día 5 pivotea).
- La decisión se toma en la **ficha on-chain** (botón DATA → `drawCoinSheet`: candle
  chart + barra de dev wallet + liquidez/mint/bundle/holders) y los **socials**
  (`drawSocials`). El pitch miente; la ficha no.
- Economía en **silicio (◈)**: +Si por acierto, -Si + strike por error. 3 strikes =
  *reassigned to the mines*. Quota diaria = lo que le debés a la máquina.
- Cada decisión dispara **confetti + flash de luz** (`burstConfetti`).

## Correr / previsualizar
Los modelos y las libs se cargan por URL, así que **conviene servirlo por HTTP**
(Render, o el preview del server local). Si el CDN no responde, cae solo a los
personajes procedurales (no rompe).

## Deploy en Render
Static Site → conectar este repo → **Publish Directory: `.`** → sin build command.
Push a `main` = redeploy automático.

## Perillas de ajuste (buscá el nombre en `index.html`)
- Coins y métricas: `COINS`. Umbrales de flags: `FLAGS`.
- Política por día: `SCHEDULE` (`add`/`remove` de flags). Veredicto: `correctVerdict`.
- Modelos GLB: `CHAR_ASSETS` (`url`, `h` altura, `rot` giro, `z` distancia, `idle`)
  y `PROP_ASSETS` (props de escena). Mapa arquetipo→modelo: `assetFor` + `BOT_ARCH`.
- Economía/dificultad: objeto `CFG` (silicio inicial, +/-, `patienceSec`, strikes).
- Ficha on-chain y velas: `drawCoinSheet` / `makeCandles`; socials: `drawSocials`.
- Confetti/luces: `burstConfetti`. Botones: `#human` (PUMP) / `#bot` (RUG) en CSS/HTML.
- Encuadre de cámara: `camBaseY` y `camera.lookAt(...)` dentro de `animate`.
- Props / detalles de escritorio (radio, taza, sellos): `buildRoom`; GLB en `loadProps`.
- Nueva coin: agregá un `coin({...})` a `COINS` (métricas → flags automáticas).

## Assets
Modelos GLB con licencia permisiva (three r128 examples + Khronos glTF-Sample-Models),
servidos por jsdelivr. Para sumar otros, agregá una entrada a `CHAR_ASSETS` con su
`url` y ajustá `h` / `rot` / `z` (idealmente un `.glb` riggeado con clip `Idle`).
