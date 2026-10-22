# TFR-Wii — port de *The Father: Reboot* a Wii (en desarrollo)

El juego original está hecho con **Godot 3.3.2** (3D, 102 MB). Godot no existe para Wii,
así que el port es una reescritura en C con **libogc** que carga los assets ya convertidos
desde la tarjeta SD.

## Estado actual

| Pieza | Estado |
|---|---|
| Extractor de `.pck` (`tools/pck.py`) | ✅ probado con el juego real (1878 archivos) |
| Texturas `.stex` → PNG (`tools/stex.py`) | ✅ 491/491 (RGBA8, L8, DXT1, PNG/WebP sin pérdida) |
| Audio → MP3/OGG/WAV (`tools/audio.py`) | ✅ 77/77 pistas |
| Parser de recursos binarios (`tools/rsrc.py`) | ✅ lee mallas, materiales, audio |
| Prueba de entorno Wii (`wii/`) | ⚠️ sintaxis comprobada; **aún sin compilar en el SDK real** |
| Compilación en la nube (`.github/workflows`) | ⚠️ sin ejecutar todavía |
| Scripts del juego (`.gdc`, bytecode v13) | ⏳ pendiente: desensamblador |
| Escenas `.tscn` y mallas → formato Wii | ⏳ pendiente |
| Motor de juego, mando configurable, guardado | ⏳ pendiente |

## Paso 1 — Compilar sin instalar nada

1. Crea un repositorio en GitHub y sube el contenido de esta carpeta (el `.gitignore` ya excluye el juego).
2. Pestaña **Actions** → *Compilar para Wii* → cuando termine, descarga el artefacto **TFR-Wii**.
3. Copia la carpeta `apps/` del artefacto a la raíz de la SD de tu Wii (o ábrela en Dolphin: *Open* → `boot.dol`).

Debe mostrar la pantalla de vídeo, los botones del Wiimote/Nunchuk/Classic Controller y
«Arranques registrados en la SD: N», que sube cada vez que lo abres (eso prueba el guardado).
**Si el paso de compilación falla, copia el error y envíamelo:** es justo para lo que sirve esta prueba.

## Paso 2 — Convertir los assets (en tu PC)

```
pip install pillow
python3 tools/convert_all.py TheFatherR_Windows.pck out/
```

Genera `out/png/` (texturas) y `out/audio/` (+ `audio_manifest.json`).
Solo hace falta Python 3.9+ y Pillow.
