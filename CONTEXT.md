# Naruto: Os Pergaminhos de Asura — Context

**Engine**: Godot 4.6 | **Branch**: main | **Repo**: lucasgomeslabs/naruto-os-pergaminhos-de-asura

---

## Como usar este arquivo
- Cole o conteúdo deste arquivo no início de cada nova sessão de chat
- Peça "Atualiza o CONTEXT.md" ao final de cada sessão antes do último commit

---

## Regra permanente — zero commits sem teste
**Fluxo obrigatório:** coworker implementa → usuário testa no Godot Editor → traz feedback → aprovado → commit.
Nunca commitar antes da confirmação do usuário.

---

## Stack
- GDScript, 2D platformer — distribuição: PC + Android + Web (renderer Compatibility, ship 720p 16:9, stretch keep, sprites espelham via flip_h)
- Input: Teclado (WASD/Setas + J/K/O/Shift) — sem suporte a controle

---

## Estrutura de pastas
- `levels/` → zonas do jogo (zona_2 placeholder, floresta_da_nevoa)
- `scenes/` → player, entidades, test_stage
- `scenes/cutscenes/` → ichiraku, akatsuki_hideout
- `scenes/components/` → dialogue_trigger, rasengan_balloon
- `scenes/ui/` → dialogue_box
- `scripts/player/` → player_controller.gd (~799 linhas, FSM 12 estados)
- `scripts/entities/` → zabuza, melee_ninja, shuriken, dummy
- `scripts/components/` → hitbox, hurtbox, dialogue_trigger, kamui_trigger, rasengan_balloon
- `scripts/systems/` → level_manager.gd, dialogue_manager.gd (autoloads)
- `scripts/cutscenes/` → ichiraku, akatsuki_hideout
- `scripts/ui/` → debug_hud, dialogue_box
- `assets/audio/` → Zabuza_laugh.wav
- `assets/backgrounds/ichiraku/` → 3 frames teuchi_naruto_jiraya
- `assets/backgrounds/akatsuki/` → 2 frames guedomazo (FRAME_A/B atuais) + 3 novos aditivos akatsuki_01_intro/02_facepalm/03_kamui (1448×1086, p/ rewrite)
- `assets/backgrounds/` → Vila_da_folha.png
- `assets/sprites/` → Naruto_chakra_charge.png (transparente) + naruto/ (13 sprites + alternates/ 18 — NÃO integrados na FSM)
- `documentation/` → prints de validação

---

## Input Map atual
| Action | Teclado |
|---|---|
| move_left/right | ← → + A D |
| jump | ↑ + W |
| crouch | ↓ + S |
| attack_light | J |
| attack_heavy (shuriken) | K |
| special (rasengan) | O |
| chakra_charge | Shift Esq |

---

## Sistemas implementados
| Sistema | Arquivo | Status |
|---|---|---|
| PlayerController | player_controller.gd | ✅ FSM 12 estados (inclui WALL_SLIDE) |
| CombatSystem | hitbox.gd, hurtbox.gd | ✅ |
| ChakraSystem | embarcado no player | ✅ |
| BossController | zabuza.gd | ✅ FSM 9 estados |
| EnemyController | melee_ninja.gd | ✅ FSM 6 estados |
| LevelManager | level_manager.gd | ✅ autoload, RESPAWN_ZONE = zona_2 |
| DialogueSystem | dialogue_manager.gd + dialogue_box.gd | ✅ autoload + UI manga-style + signal line_advanced |
| DialogueTrigger | dialogue_trigger.gd | ✅ Area2D, modos AUTO e INTERACTION |
| RasengaBalloon | rasengan_balloon.gd | ✅ world-space, filho do Player, await 1.5s |
| CutsceneSystem | ichiraku.gd, akatsuki_hideout.gd | ✅ Ichiraku 100% integrado e testado (encontro + saída + fade); akatsuki integrado |
| KamuiTrigger | kamui_trigger.gd | ✅ ativo em akatsuki_hideout, player via grupo, fallback get_nodes_in_group |
| FadeTransition | fade_transition.gd | ✅ componente genérico, `fade(callback)` + signal `fade_completed`, duration=0.5s default |
| CollectibleSystem | — | ❌ pendente |
| SaveSystem | save_system.gd | ✅ autoload, snapshot em memória de hp/chakra/scrolls; `load_into()` emite signals |
| DebugHUD | debug_hud.gd + debug_hud.tscn | ✅ autoload, reconecta ao Player via `node_added` + grupo "Player" |

---

## Decisões de design fechadas
- Morte → respawn sempre na Zona 2, independente da zona atual
- Zona 1 = tutorial com Jiraiya — jogado só uma vez
- Wall slide = segurar direção contra a parede. Solta → cai. Pulo → wall jump
- Wall jump recarrega double jump (_jumps_made = 0)
- Shuriken durante WALL_SLIDE sai oposta à parede
- Sem checkpoint — morreu recomeça da Zona 2
- Galho fatal na Zona 3 = único elemento propositalmente injusto (sem telegraf)
- Fighting game style (KOF/MK) — sem mira livre com mouse
- Diálogos pausam o jogo (get_tree().paused = true); DialogueBox tem process_mode = ALWAYS
- DialogueBox estilo manga: fundo branco + borda colorida por personagem
- SPEAKER_COLORS: Naruto=laranja, Jiraiya=verde, Pain=roxo, Konan=azul, Tobi=laranja escuro
- Speakers desconhecidos → fallback DEFAULT_COLOR (preto)
- "Tô certo" é marca verbal exclusiva do Naruto (removida das falas do Jiraiya)
- RasengaBalloon é world-space (filho do Player), não UI screen-space — segue camera
- Cutscenes reagem a `DialogueManager.line_advanced(index)` pra trocar texturas (padrão Akatsuki frame swap)
- Akatsuki cutscene: abre com frame_a (Pain mão na cabeça), troca para frame_b na linha 2 ("Pain: Inesperado.")
- Ichiraku é sub-scene da Zona 4 (não `change_scene_to_file`) — Player permanece na árvore
- `exit_position` do KamuiTrigger = `Vector2(500, 0)` placeholder — ajustar quando Zona 2 for construída
- **Distribuição (Sessão 8):** alvo PC + Android + Web; renderer Compatibility; ship 720p 16:9; stretch keep; sprites espelham via `flip_h`. (Substitui "uso pessoal, máquina única".)
- **Cutscene Akatsuki — 3 beats (planejado, rewrite pendente):** intro (Pain olha) → facepalm ("Inacreditável", mão no rosto) → kamui (Tobi levanta a mão, expulsa Naruto). Beat 3 sincroniza com `KamuiTrigger`.
- **ChakraSprite calibrado por imagem:** usa region-crop + scale por sprite. Sprite novo do Player NÃO é drop-in — exige recalibrar `region_rect`/`scale`.
- **Estilo visual definitivo (Sessão 9):** arte desenhada / cel-shaded. Tentativa de pixel art para cenários foi **descartada**.

---

## Estrutura do jogo (5 zonas lineares)
| Zona | Cena | Status |
|---|---|---|
| 1 | zona_1_floresta_morte.tscn | ❌ não criada |
| 2 | zona_2_casa_central.tscn | 🟡 placeholder + JiraiyaTrigger AUTO |
| 3 | zona_3_arvores_gigantes.tscn | ❌ não criada |
| 4 | zona_4_aldeia_corredor.tscn | ❌ não criada (cutscene Ichiraku pronta pra encaixar) |
| 5 | zona_5_lago.tscn | 🟡 floresta_da_nevoa.tscn (renomear, cutscene Akatsuki pronta pra encaixar) |

---

## Commits desta sessão (Sessão 9)
- `Feat: backgrounds Ichiraku — arte nova (overwrite in-place dos 3 frames)`
- `Fix: Ichiraku — enquadramento full-rect (KEEP_ASPECT_COVERED) + remove Ichiraku duplicado no test_stage`
- `Docs: atualiza CONTEXT.md — sessão 9 (Ichiraku + Akatsuki + pendências)`

### Entregue (Sessão 9)
- **Backgrounds Ichiraku:** arte nova nos 3 frames (`teuchi_naruto_jiraya1/2/3`, overwrite in-place, 1672×941 16:9, UID/path preservados).
- **Enquadramento Ichiraku corrigido:** `Background` full-rect + `stretch_mode = KEEP_ASPECT_COVERED`; removido `size`/`position` hardcoded do `_ready()` (era calibração da arte antiga 2.26:1).
- **Bug corrigido:** `test_stage.tscn` tinha 2 Ichiraku (um aninhado no outro) — duplicata removida.
- **Backgrounds Akatsuki:** verificado por hash que os 3 frames do commit B já são a versão final (com tochas) — **nenhuma mudança necessária**. Continuam não ligados à cutscene (segue usando `guedomazo`).

### Pendências / divergências mapeadas (não-bloqueantes)
- Saída do Ichiraku reposiciona o Player em `(-600,0)` (placeholder Zona 4). Sem Zona 4, cai no `test_stage` e toma dano de queda. Resolve quando a Zona 4 existir (trocar pela entrada real).
- "Respawn parece Zona 1" é **falso-positivo**: vai pra `zona_2` corretamente (`RESPAWN_ZONE = zona_2`, zero referência a Zona 1 no código). A impressão vem do `jiraiya_intro` AUTO do placeholder da Zona 2.
- `jiraiya_intro` re-dispara a cada respawn (`one_shot` não persiste entre reloads) — resolver com flag no SaveSystem quando a Zona 2 virar design real.
- ✅ **Config divergente — RESOLVIDA (Sessão 10, Bloco 2):** `project.godot` agora em **1280×720 + GL Compatibility** (desktop e mobile), stretch `canvas_items`/`keep`. Testado no editor (Ichiraku enquadrado, floresta ok, FPS estável).

---

## Sessão 10 — Bloco 1: higiene do repo (sem mudança de comportamento)

### Resolvido neste bloco
- ✅ **Working tree suja** (6 `.txt` + `NOPA/` + zip untracked desde a sessão 8/9) — tudo destinado.
- ✅ **Docs de metodologia na raiz** → movidos para `documentation/` (na raiz ficam só os operacionais: CLAUDE.md, CLAUDE_CODE.md, CONTEXT.md, Changelog.md, README.md, PROMPT.md, SUGESTOES.md).
- ✅ **`NOPA/` e `naruto_sprites.zip` dentro do projeto** → movidos para **`C:\Projetos\naruto-game-assets-brutos\`** (fora do repo; local canônico dos assets brutos). 36 `.import` de NOPA removidos (conferidos 1 a 1); `.gitignore` bloqueia o retorno de ambos.
- ✅ **Divergência de controles na doc** — `documentation/Atualização status.txt` corrigido de H/J/L para J/K/O (input map real).
- ✅ **`README.md.md`** (write-up de combate/IA com extensão duplicada) → `documentation/combate-ia-writeup.md`.
- ✅ `.claude/settings.local.json` untracked + gitignorado (estado local do Claude Code).

### Nota
- `README.md` da raiz continua defasado (controles H/J/L, status "Semana 1") — atualização fica para bloco futuro, fora do escopo de higiene.

---

## Sessão 10 — Bloco 2: config alvo (720p 16:9 + Compatibility)

### Aplicado em `project.godot`
- `[display]`: viewport **1280×720**, stretch `canvas_items`. `aspect="keep"` é o default do Godot 4 — o editor removeu a linha redundante ao salvar; comportamento efetivo é keep.
- `[rendering]`: `renderer/rendering_method="gl_compatibility"` + `.mobile` idem. `d3d12`/Jolt intactos (inertes no alvo).
- Teste aprovado: test_stage ok, **Ichiraku enquadrado** (canário da regressão da sessão 9), respawn Zona 2 + diálogo Jiraiya enquadrado, floresta sem vazamento de parallax, FPS estável. Akatsuki não testável (frames ainda não ligados — rewrite sai com a Zona 5).

### Notas de ambiente
- **Projeto movido no disco:** repo agora em `C:\Projetos\Naruto projeto\naruto-game`; assets brutos em `C:\Projetos\Naruto projeto\naruto-game-assets-brutos` (corrige o caminho registrado no Bloco 1).
- **Owner do GitHub renomeado** para `lucasgomeslabs` — `git remote set-url` aplicado; URL atualizada no cabeçalho deste arquivo.

---

## Commits desta sessão (Sessão 8)
- `Feat: assets Naruto (primary+alternates) + chakra transparente + recalibra ChakraSprite`
- `Feat: backgrounds cutscene Akatsuki (3 frames) — aditivo, rewrite pendente`
- `Docs: atualiza CONTEXT.md sessão 8`

### Entregue (Sessão 8)
- `assets/sprites/naruto/` — 13 sprites do Naruto (idle, idle_kunai, stance_kunai, run, dash_start, punch, rasengan, shuriken_throw, kick_low, kick_high, kick_flying, land_impact) com fundo transparente. + `alternates/` (18: espelhados + 7 corridas redundantes). **Ainda NÃO integrados na FSM.**
- `Naruto_chakra_charge.png` agora transparente (mesmo path `assets/sprites/`, sobrescrito in-place; `.import`/UID preservados).
- `ChakraSprite` (`player.tscn`) recalibrado pro canvas novo 1672×941: `region_rect = Rect2(585,143,505,652)`, `scale = (0.16196,0.16196)`. Lição: ChakraSprite usa region-crop + scale calibrados por imagem — sprite novo do Player precisa desse ajuste, não é drop-in.
- `assets/backgrounds/akatsuki/` — 3 frames novos (`akatsuki_01_intro`, `02_facepalm`, `03_kamui`, 1448×1086). Aditivos. `guedomazo_naruto2/3` continuam FRAME_A/B da cutscene atual.

---

## Commits desta sessão (Sessão 7)
- `9ea1b21` — Feat: SaveSystem — persistência de hp/chakra/scrolls entre zonas
- `1cdfa3b` — Docs: CLAUDE.md — lições aprendidas sessão 7
- (este commit) — Docs: SUGESTOES.md #07 + CONTEXT.md sessão 7

### Sistemas entregues
- `SaveSystem` — autoload novo registrado em `project.godot` (`SaveSystem="*res://scripts/systems/save_system.gd"`). Snapshot em memória de `_hp`, `_chakra`, `_scrolls`. API: `save(player)`, `load_into(player)`, `reset()`, `has_data()`. `load_into()` emite `health_changed` e `chakra_changed` no Player após restore — sincroniza HUD sem esperar próxima emissão natural. ✅
- `DebugHUD` — refatorado para autoload (`DebugHUD="*res://scenes/ui/debug_hud.tscn"`). `@export var player_path` removido; agora resolve o Player via `get_tree().get_first_node_in_group("Player")` e reconecta automaticamente em troca de zona via `get_tree().node_added`. Disconnect-before-reconnect evita signals duplicados. Instâncias locais de DebugHUD removidas de `test_stage.tscn` e `floresta_da_nevoa.tscn`. ✅
- `zona_2_casa_central.tscn` — `ZoneLabel` corrigido: tipo `Label2D` (classe inexistente no Godot 4.6) → `Label`; propriedade `font_size = 28` → `theme_override_font_sizes/font_size = 28`. Corrige crash com `ERROR: Cannot get class 'Label2D'` em respawn/change_scene. ✅
- `debug_zone_switch.gd` — novo autoload de debug em `scripts/debug/`. F1 = `SaveSystem.save(player)` + `LevelManager.change_scene("zona_2")`; F2 = `SaveSystem.load_into(player)`; F3 = `SaveSystem.reset()`. Usado para validar persistência cross-zona. ✅
- `CLAUDE.md` — adicionada seção "Lições aprendidas — Sessão 7": direcionamento de tarefas (coworker vs Claude Code), alucinação/verificação contra disco, specs sem valores numéricos duplicados, prompts sem ambiguidade de escopo. ✅
- `SUGESTOES.md` — `#07 Tutorial interativo — Zona 2 (JiraiyaTrigger)` adicionado: sistema independente do DialogueSystem que aguarda input correto antes de avançar instrução. ✅

## Commits da sessão 6
- `Feat: ChakraSprite — Naruto sentado durante CHAKRA_CHARGE (#01)` (cb037d2)
- `Fix: Ichiraku background — sizing e posicionamento` (cb037d2)
- `Docs: atualiza CONTEXT.md sessão 6` (a355897)

## Commits da sessão 5
| SHA | Descrição |
|---|---|
| 65c4d44 | Docs: reformula papéis — CLAUDE.md + CLAUDE_CODE.md |
| 093af3b | Docs: atualiza CONTEXT.md sessão 5 |
| 88179a5 | Feat: FadeTransition genérico + ichiraku_saida trigger e fluxo de saída |
| 6530e3a | Feat: Bloco 3 completo — player grupo Player, KamuiTrigger fallback, akatsuki_hideout integrado |
| 933aea0 | Feat: ichiraku.tscn — DialogueTrigger AUTO ichiraku_encontro |

## Commits anteriores (Bloco 3 + refinamentos)
| SHA | Descrição |
|---|---|
| 2327c33 | Docs: PROMPT.md — instrução de sessão para o Tech Lead |
| ab8e2e1 | Docs: atualiza SUGESTOES.md com design fechado sessão 2 |
| 4e64af8 | Feat: DialogueSystem — DialogueManager + DialogueBox + DialogueTrigger + diálogos Jiraiya e Akatsuki |
| 90248be | Feat: assets Ichiraku/Akatsuki + diálogos ichiraku_encontro e ichiraku_saida |
| dac7503 | Feat: assets cutscenes + scenes/cutscenes Ichiraku e Akatsuki |
| d55ddd1 | Feat: Akatsuki cutscene — swap de textura via signal line_advanced |
| 6236313 | Feat: DialogueBox manga-style + RasengaBalloon world-space + fix diálogo Jiraiya |

---

## Pendências de integração (não-bloqueantes)
- 13 sprites em `assets/sprites/naruto/` aguardam integração na FSM do Player (precisa da análise de render — ver Próximo bloco)
- Renomear `floresta_da_nevoa.tscn` → `zona_5_lago.tscn` quando definir a Zona 5 final
- Warning: signal `respawned` declarado em `player_controller.gd` mas nunca conectado (cleanup futura, não-bloqueante)
- `exit_position` do KamuiTrigger (`Vector2(500, 0)`) é placeholder — revisar quando Zona 2 for construída
- Refactor `kamui_trigger.gd` para usar `FadeTransition` (eliminar duplicação de fade)
- Quando Zona 4 for construída, mover fala `Jiraiya: "Entra aí, garoto."` para trigger no corredor antes da entrada do Ichiraku

---

## Próximo bloco — Sessão 10 (pendências explícitas)
1. **Análise de render do Player** — mapear os 12 estados da FSM + como cada sprite é montado. Pré-requisito pra integrar os 13 sprites de `assets/sprites/naruto/`.
2. **Rewrite cutscene Akatsuki 2→3 frames** + sync `KamuiTrigger` + aposentar `guedomazo_naruto2/3` ao migrar — **fazer junto com a Zona 5** (a cutscene não tem como ser testada sem a cena da Zona 5 montada).
3. **Padronizar canvas + pivô** dos sprites antes de animar. "Corridas" = 8 desenhos do mesmo instante, não um ciclo de animação.

### Ainda na mesa (sessões anteriores)
- **TutorialTrigger Zona 2** (#07 — ver SUGESTOES.md)
- **CollectibleSystem** (#02–#06 — ver SUGESTOES.md)
- **Zona 5**: renomear/finalizar floresta_da_nevoa.tscn
