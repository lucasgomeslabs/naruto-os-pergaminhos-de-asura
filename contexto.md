# Naruto: Os Pergaminhos de Asura — Contexto

**Engine:** Godot 4.6 · **Branch:** main · **Repo:** lucasgomeslabs/naruto-os-pergaminhos-de-asura

Estado conceitual do projeto (onde estamos agora). Governança em `CLAUDE.md`.
Decisões de design em `decisoes.md`. Histórico por sessão em `docs/`.

## Stack
- GDScript · 2D platformer. Alvo de distribuição e config de display: ver `decisoes.md`.
- Input: teclado (WASD/Setas + J/K/O/Shift) — validado. Gamepad mapeado no Input Map
  (analógico/DPad, A, X, Y, B, gatilho direito), porém **nunca testado em hardware**.

## Input Map
| Ação | Teclado |
|---|---|
| move_left/right | ← → + A D |
| jump | ↑ + W |
| crouch | ↓ + S |
| attack_light | J |
| attack_heavy (shuriken) | K |
| special (rasengan) | O |
| chakra_charge | Shift Esq |

## Sistemas implementados
| Sistema | Arquivo | Status |
|---|---|---|
| PlayerController | player_controller.gd | ✅ FSM 12 estados (inclui WALL_SLIDE) |
| CombatSystem | hitbox.gd, hurtbox.gd | ✅ |
| ChakraSystem | embarcado no player | ✅ |
| BossController | zabuza.gd | ✅ FSM 9 estados |
| EnemyController | melee_ninja.gd | ✅ FSM 6 estados |
| LevelManager | level_manager.gd | ✅ autoload, RESPAWN_ZONE = zona_2 |
| DialogueSystem | dialogue_manager.gd + dialogue_box.gd | ✅ autoload + UI manga + line_advanced |
| DialogueTrigger | dialogue_trigger.gd | ✅ Area2D, modos AUTO e INTERACTION |
| RasengaBalloon | rasengan_balloon.gd | ✅ world-space, filho do Player |
| CutsceneSystem | ichiraku.gd, akatsuki_hideout.gd | ✅ Ichiraku testado; Akatsuki integrado |
| KamuiTrigger | kamui_trigger.gd | ✅ ativo em akatsuki_hideout, fallback por grupo |
| FadeTransition | fade_transition.gd | ✅ genérico, fade(callback)+signal, 0.5s |
| SaveSystem | save_system.gd | ✅ autoload, snapshot hp/chakra/scrolls |
| DebugHUD | debug_hud.gd + .tscn | ✅ autoload, reconecta via node_added |
| CollectibleSystem | — | ❌ pendente |

## Estrutura do jogo (5 zonas lineares)
| Zona | Cena | Status |
|---|---|---|
| 1 | zona_1_floresta_morte.tscn | ❌ não criada |
| 2 | zona_2_casa_central.tscn | 🟡 placeholder + JiraiyaTrigger AUTO |
| 3 | zona_3_arvores_gigantes.tscn | ❌ não criada |
| 4 | zona_4_aldeia_corredor.tscn | ❌ não criada (cutscene Ichiraku pronta) |
| 5 | floresta_da_nevoa.tscn | 🟡 placeholder da Zona 5 (cutscene Akatsuki pronta) |

## Pendências abertas
- Renomear `floresta_da_nevoa.tscn` → `zona_5_lago.tscn` ao definir a Zona 5.
- Integrar os 13 sprites de `assets/sprites/naruto/` na FSM do Player (precisa da análise de render).
- `KamuiTrigger.exit_position = Vector2(500,0)` é placeholder — revisar quando a Zona 2 existir.
- Refatorar `kamui_trigger.gd` para usar `FadeTransition` (elimina duplicação de fade).
- Signal `respawned` (player_controller.gd) declarado mas nunca conectado — cleanup.
- Saída do Ichiraku reposiciona o Player em `(-600,0)` (placeholder Zona 4); sem Zona 4 cai no test_stage.
- `jiraiya_intro` re-dispara a cada respawn (`one_shot` não persiste) — resolver com flag no SaveSystem.
- Ao construir a Zona 4, mover a fala `Jiraiya: "Entra aí, garoto."` para trigger no corredor.
- Validar o mapeamento de gamepad quando houver controle disponível — os binds existem
  no `project.godot` mas nunca foram exercitados.

## Próximos passos (Sessão 12)
1. Análise de render do Player — mapear os 12 estados da FSM e como cada sprite é montado (pré-requisito p/ integrar os 13 sprites).
2. Rewrite da cutscene Akatsuki 2→3 frames + sync `KamuiTrigger` + aposentar `guedomazo_naruto2/3` — junto com a Zona 5.
3. Padronizar canvas + pivô dos sprites antes de animar.

### Backlog (ver SUGESTOES.md)
- TutorialTrigger Zona 2 (#07).
- CollectibleSystem (#02–#06).
