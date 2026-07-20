# decisoes.md — Decisões de design fechadas

Decisões de design e técnicas já fechadas do naruto-game. Estado atual em `contexto.md`;
histórico em `historico/`.

## Distribuição e display
- Alvo: PC + Android + Web. Renderer **Compatibility**. Ship **720p 16:9** (1280×720),
  stretch `canvas_items`/`keep`. Sprites espelham via `flip_h`. (Decidido na Sessão 8;
  substitui o alvo anterior "uso pessoal, máquina única"; aplicado em `project.godot` na Sessão 10.)

## Estilo visual
- Arte desenhada / cel-shaded. Pixel art para cenários foi descartada (Sessão 9).

## Movimento e combate
- Morte → respawn sempre na Zona 2, independente da zona atual. Sem checkpoint.
- Zona 1 = tutorial com Jiraiya, jogado só uma vez.
- Wall slide = segurar direção contra a parede; soltar → cai; pulo → wall jump.
- Wall jump recarrega o double jump (`_jumps_made = 0`).
- Shuriken durante WALL_SLIDE sai no sentido oposto à parede.
- Galho fatal na Zona 3 = único elemento propositalmente injusto (sem telegraf).
- Fighting game style (KOF/MK) — sem mira livre com mouse.

## Diálogo e cutscenes
- Diálogos pausam o jogo (`get_tree().paused = true`); DialogueBox tem `process_mode = ALWAYS`.
- DialogueBox estilo manga: fundo branco + borda colorida por personagem.
- SPEAKER_COLORS: Naruto=laranja, Jiraiya=verde, Pain=roxo, Konan=azul, Tobi=laranja escuro.
  Speaker desconhecido → DEFAULT_COLOR (preto).
- "Tô certo" é marca verbal exclusiva do Naruto (removida das falas do Jiraiya).
- Cutscenes reagem a `DialogueManager.line_advanced(index)` para trocar texturas.
- Akatsuki (atual): abre com frame_a (Pain, mão na cabeça), troca para frame_b na linha 2.
- Akatsuki (planejado, rewrite pendente): 3 beats — intro → facepalm → kamui; beat 3 sincroniza com `KamuiTrigger`.
- Ichiraku é sub-scene da Zona 4 (não `change_scene_to_file`) — o Player permanece na árvore.

## Sprites / render
- RasengaBalloon é world-space (filho do Player), não UI screen-space — segue a câmera.
- ChakraSprite calibrado por imagem: usa region-crop + scale por sprite. Sprite novo do Player
  NÃO é drop-in — exige recalibrar `region_rect`/`scale` (valores atuais vivem em `player.tscn`).
