# PUMP, PLEASE — Design Bible

Reskin + evolución del motor *Turing, Please* (Papers-Please-like, un solo `index.html`,
Three.js r128). **Este documento es la fuente de verdad. En dudas, esto manda.**

## Concepto
Sos el único humano en el loop de una Solana convertida en ciudad-casino gobernada por
máquinas. En el último filtro de listing, cada memecoin manda un representante (dev,
mascota, IA, alien, KOL, ballena…). Leés al personaje pero **decidís por los indicadores
on-chain**. Dos acciones:
- **PUMP / AUTORIZAR** (verde, circular): la dejás pumpear → sello `LISTED`.
- **RUG / RECICLAR** (rojo, circular): la rugueás vos mismo; su liquidez se funde en
  silicio para la máquina → sello `RECYCLED`.

Tono: patético-gracioso (engranaje humano mal pago dentro de una máquina inhumana) +
sátira cripto. Cada decisión dispara **luces + papelitos** en toda la pantalla.

## Loop
menú → día (directiva = política de listing) → llega el representante (modelo GLB) →
leés su pitch + abrís su ficha on-chain 3D (**DATA**) → **PUMP / RUG** → economía
(silicio / strikes) → siguiente → fin del día → nota (grade).

Idioma: texto in-game en **inglés** (slang cripto); comentarios/doc en español.

## Modelo de datos: `COINS`
Cada solicitante es una coin. `coin({...})` deriva `id`, `avatar` (bot/human) y `lines`
para reusar el motor. Métricas: `devWalletPct, holders, topHolderPct, liqLocked, liqPct,
mintRenounced, bundled, ageMin, mcap`.

Flags (violaciones objetivas, derivadas por `coinFlags`, no escritas a mano):
- `DEV_HEAVY` → devWalletPct > 20
- `LIQ_UNLOCKED` → !liqLocked
- `MINT_LIVE` → !mintRenounced
- `BUNDLED` → bundled
- `LOW_HOLDERS` → holders < 100
- `SNIPER_TOP` → topHolderPct > 15

## Reglas por día (`SCHEDULE`)
La directiva dice qué flags obligan a **RUG**; el resto va **PUMP**.
`correctVerdict(coin)` = RUG si la coin tiene alguna flag **activa** ese día, si no PUMP.
1. DEV_HEAVY · 2. +LIQ_UNLOCKED · 3. +MINT_LIVE · 4. +BUNDLED ·
5. giro: +LOW_HOLDERS y se **revoca** DEV_HEAVY (directiva confusa a propósito).
`SNIPER_TOP` existe como flag pero el schedule de 5 días no la activa: trampa para el
sobre-cauto (una coin cuya única flag es SNIPER_TOP siempre es PUMP).

## Modelos (registry `CHAR_ASSETS` + `assetFor`)
Arquetipos → modelo humano/robot GLB + recolor. `BOT_ARCH` define qué arquetipos son
robots (aiBot, alienVC, volumeBot). // TODO: add GLB — sumar más assets al registry es
trivial (una entrada con `url/h/rot/z/idle`).

## Documentos 3D (reusa DNI/CV)
- **Ficha on-chain** (`drawCoinSheet`): ticker+nombre, candle chart (~22 velas), barra
  de dev wallet % (roja si >20), holders, top holder %, candado de liquidez, mint,
  bundled, edad, mcap, flags. Sello `LISTED`/`RECYCLED`.
- **Socials** (`drawSocials`): @handle, TG/followers, dev anon/doxxed, tweets shill.
  Da flavor y sesgo, **no decide**.

## Economía: silicio (◈)
Correcto → +Si. Incorrecto → −Si + strike. 3 strikes = *REASSIGNED TO THE MINES*.
Quota diaria = silicio que le debés a la máquina. Grade final (S · WHALE-TIER SIGNAL …
F · EXIT LIQUIDITY).

## Oficina
Terminal parodia-Bloomberg, luz violeta Solana, "DAYS SINCE A RUG: 0", posters cripto,
cabeza sintética confiscada (DamagedHelmet) y maquinaria de fondo.

## Legal (no negociable)
Fase 1: data **ficticia/autoral** (tickers, métricas inventadas con patrones realistas).
Con data en vivo: presentarla neutral (juzga el jugador), difuminar identidades reales,
scorear por precio público, preferir coins muertas/graduadas.

## Roadmap
- **Fase 1 (HECHO):** reskin jugable con data autoral (este commit).
- Fase 2: roster de modelos + eventos + medidores (reputación de cadena, calor
  regulatorio, confianza degen) + animación de "cosecha de silicio".
- Fase 3: cola en vivo desde Pump.fun (snapshot en el backend de Render).
- Fase 4: scoring veredicto-vs-realidad + leaderboard (el hook viral).

### Art overhaul (visual, sobre Fase 1)
Objetivo: que el juego se vea como la portada (chunky voxel, saturado, denso, jugoso).
- **Protagonista**: bot cabeza-monitor voxel (`buildMonitorBot`), recoloreado por coin
  (hash del ticker → HSL), con el **logo generado** de la coin en la pantalla
  (`makeCoinLogo`), ojos pixel + boca, gags de portada (shades "deal with it", corona
  si mcap alto). Reemplaza los GLB humanos como solicitante (los GLB quedan de decoración).
- **Luz**: key cálida fuerte desde arriba-frente + rims verde/violeta a los costados →
  el bot POP-ea; menos murk (ambient arriba); pantallas emisivas.
- **Botones ARCADE** rectangulares chunky (PUMP ↑ / RUG 💀) con bisel y hundimiento;
  **consola INSPECTING** con ecualizador que hace flip a LISTED/RECYCLED.
- **Juice**: confetti + flash, pop flotante `+◈N` / `−◈N`, screen-shake (`kick`),
  parpadeo/boca del bot, velas flotantes, DAYS SINCE A RUG.
- **Oficina densa**: latas, teclado, joystick, cables con glow, tacho de "rugueados",
  sticky notes ("wen lambo?", "few understand"), posters ("BUILT ON HOPIUM", "NO UTILITY
  ONLY VIBES"), figurita Doge, monitores extra con charts, **skyline** de ciudad al fondo.
- **Íconos cripto reales** (spothq, CC0) por jsdelivr con `crossOrigin='anonymous'` como
  decoración de pared (`loadCryptoDecor`); fallback silencioso si no cargan.
- **TODO (bloqueado)**: caras de *RUG WORLD CUP* (assets de Ariel) para público/tacho/
  hall of shame — falta la URL del repo; hook listo para enchufarlas igual que spothq.

### Estado Fase 1
Data (`COINS`+flags) · reglas (`SCHEDULE`/`correctVerdict`) · botones circulares
PUMP/RUG + confetti · ficha (`drawCoinSheet` con velas) + socials · registry de modelos ·
rulebook (política) · economía en silicio · oficina Solana · copy PUMP · fallback
procedural intacto. Verificado en Chromium headless (0 errores, GLB y fallback).
