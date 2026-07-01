# PUMPDER — Art / Visual Direction

Clon del **look & feel y patrón de interacción de una dating app de swipe** (tipo
Tinder), con temática memecoins. Todo DOM + CSS + SVG, mobile-first, 60fps.

## Lo que NO copiamos (límites)
- Marca propia **"Pumpder"** (no el logo/nombre/llama de la app original).
- Personajes = **caricaturas ficticias con caras generadas** (SVG), nunca caras ni
  nombres de personas reales. Memecoins reales = su **logo real**. Métricas = sátira.

## Estética (fiel a una dating app)
- **Degradé rojo/coral típico de Tinder** de fondo: `linear-gradient(120deg,#FD267D →
  #FF4B6E → #FF6036)` (`--tinder`). HUD/policy/botones en blanco translúcido encima.
- Cards con esquinas muy redondeadas y sombra suave; tipografía sans redondeada
  (Fredoka); botones circulares grandes; mucho aire.
- Acentos sobre la card: PUMP `#20D26B`, RUG `#FF3B5C`. Cursiva de match: Dancing Script.
- Logo **Pumpder** propio (mark tipo "paleta/popsicle" en SVG) en header y menú.

## Patrón replicado
1. **Card full-bleed**: la foto (logo real, o cara generada para ficticias) ocupa casi
   toda la card; `$TICKER` + edad grandes abajo sobre degradé oscuro.
2. **Galería multi-foto** (`.photos`, `gotoPhoto`): 4 fotos por perfil con barras de
   progreso segmentadas arriba; tap izq/der para pasar. Foto 1 = logo/cara; fotos 2-4 =
   tarjetas de info (tokenomics, distribution con barra de dev% y línea de 20%, dev &
   vibes). **Reemplaza el background check.**
3. **Swipe físico** (`attachDrag`): arrastra + rota siguiendo el dedo; pasado el umbral
   vuela. Sellos PUMP (verde, derecha) / RUG (rojo, izquierda) con opacidad progresiva.
4. **Botones** circulares like/nope: RUG (💀 rojo) y PUMP (🚀 verde, más grande), con
   sombra y hundimiento al presionar.
5. **MATCH** (`showMatch`): al PUMP correcto, overlay sobre degradé Tinder con
   **"It's a Match!" en cursiva** (Dancing Script), cohete, logo y +PUMPCOIN. A veces
   (no siempre) aparece un botón-link real a Pump.fun / DexScreener / CoinMarketCap
   (dirección extraída del logo de Trust Wallet).
6. **HUD + directiva** del día arriba; economía **PUMPCOIN** (+6 acierto / −8 error);
   3 strikes = NGMI; nota final S/A/B/C/F.

## Assets
- Logos de memecoins reales desde Trust Wallet, `crossOrigin='anonymous'`:
  `https://cdn.jsdelivr.net/gh/trustwallet/assets@master/blockchains/solana/assets/<MINT>/logo.png`
  Roster: BONK, WIF, MEW, POPCAT, MYRO, WEN. Fallback: badge de color.
- Caras de arquetipos ficticios: **SVG generado** (`faceSVG`) por hash + arquetipo
  (chad con lentes, "printer" con ojos-$, bandido con antifaz, degen nervioso con
  gotita). No hay tool de generación de imágenes en el entorno; se usa SVG.

## Lógica
Se conserva la del prototipo: `DECK` de coins, `POLICY` del día, `resolve` (acierto =
seguir la política, no las vibras), economía. Solo se reconstruyó la capa visual al
patrón dating-app + galería + match.
