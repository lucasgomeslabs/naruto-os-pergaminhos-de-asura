# Resumo — Sessão 8 (08/06/2026, manhã)

**Repo:** naruto-game · **Commits:** `350fb54`, `cbf9dc4`, `4907cad`

## O que foi feito
- `assets/sprites/naruto/`: 13 sprites do Naruto (idle, idle_kunai, stance_kunai, run,
  dash_start, punch, rasengan, shuriken_throw, kick_low/high/flying, land_impact) com fundo
  transparente + `alternates/` (18: espelhados + 7 corridas redundantes). **Não integrados na FSM.**
- `Naruto_chakra_charge.png` agora transparente (overwrite in-place; `.import`/UID preservados).
- ChakraSprite recalibrado para o canvas 1672×941: `region_rect = Rect2(585,143,505,652)`,
  `scale = (0.16196, 0.16196)`.
- `assets/backgrounds/akatsuki/`: 3 frames novos (`akatsuki_01_intro`, `02_facepalm`,
  `03_kamui`, 1448×1086), aditivos. `guedomazo_naruto2/3` seguem como FRAME_A/B da cutscene atual.

## Decisão
- Distribuição do jogo: alvo PC + Android + Web; renderer Compatibility; ship 720p 16:9;
  stretch keep; sprites via `flip_h`. Substitui "uso pessoal, máquina única".
  (Registrada em `decisoes.md`.)
