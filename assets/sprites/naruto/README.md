# Naruto sprites — flip_h set

Fundo removido (PNG transparente). flip_h: 1 arquivo por pose; engine espelha p/ a outra direção.

## primary/ (integrar no jogo)

| pose | arquivo | fundo orig | canvas |
|---|---|---|---|
| idle | `naruto_idle.png` | black | 1672x941 |
| idle_kunai | `naruto_idle_kunai.png` | black | 1536x1024 |
| stance_kunai | `naruto_stance_kunai.png` | black | 1672x941 |
| dash_start | `naruto_dash_start.png` | white | 1672x941 |
| punch | `naruto_punch.png` | white | 1672x941 |
| rasengan | `naruto_rasengan.png` | white | 1672x941 |
| shuriken_throw | `naruto_shuriken_throw.png` | white | 1672x941 |
| kick_low | `naruto_kick_low.png` | black | 1672x941 |
| kick_high | `naruto_kick_high.png` | black | 1672x941 |
| kick_flying | `naruto_kick_flying.png` | black | 1444x1089 |
| land_impact | `naruto_land_impact.png` | white | 1672x941 |
| hurt_air | `naruto_hurt_air.png` | white | 1672x941 |
| run | `naruto_run.png` | black | 1536x1024 |
| chakra_charge | `Naruto_chakra_charge.png` | white | 1672x941 |

## alternates/ (extras — coworker IGNORA)

- 11 versões espelhadas das poses acima (caso queira trocar a direção/desenho).
- `naruto_run_alt_01..07`: as outras 7 corridas. **Nota:** as 8 corridas são o MESMO instante desenhado de formas diferentes, NÃO um ciclo. Animação de corrida real exige frames sequenciais novos.

## Pendente (próximo passo de asset)
- Padronizar canvas + pivô (pé na mesma posição) antes de animar.
- `Naruto_chakra_charge.png` = substituto drop-in do arquivo existente em `assets/sprites/`.