# Resumo — Sessão 11 (18–19/07/2026)

**Repo:** naruto-game · **Commits no repo:** `5876006`, `c93155e`

Sessão de reorganização estrutural e de governança. A maior parte do trabalho foi fora
do naruto-game (pasta-mãe, novo repo de governança, limpeza de disco); os dois commits
do naruto-game são consequência direta dela.

## Contexto da sessão (cross-repo)
- **Renomeação da pasta-mãe:** `C:\Projetos\Todos` → `C:\Projetos\projetos-pessoais`
  (fora de git). Precedida de verificação de processos/handles e varredura de caminhos
  embutidos nos 6 projetos.
- **Governança comum criada** em repo próprio `C:\Projetos\projetos-pessoais\projetos-pessoais-operacoes`
  (`prompt.md`, `code.md`, `estrutura.md`, `README.md`; commits `c2c02f9` e `dd86536`,
  pushados; remoto privado em `lucasgomeslabs`). A operação passa a ser **Gomes + Chat +
  Code**; o papel Coworker foi eliminado.
- **Limpeza de `C:\Projetos`:** `geral` desmontada (instaladores → `C:\Ferramentas\instaladores`,
  Godot portátil → `C:\Ferramentas\Godot`, `.claude.json` perdido apagado); `pgsql`
  (dist. PostgreSQL 18.4 ociosa, sem serviço/dados) removida. Remotes de todos os repos
  conferidos — todos em `lucasgomeslabs`, conta antiga inexistente.

## O que mudou no naruto-game
- **`5876006` — correção de caminhos pós-rename:** `CONTEXT.md:159` e `.gitignore:41`
  passaram a refletir `C:\Projetos\projetos-pessoais\Naruto projeto\...`. `CONTEXT.md:141`
  e `Changelog.md:23` **preservados** como registro histórico (descreviam o caminho verdadeiro
  na data em que foram escritos).
- **`c93155e` — alinhamento à governança comum:** `CLAUDE.md` reescrito (aponta para a
  operação comum + reúne só o específico-game: gate de play-test, higiene de cena Godot,
  `#lista`); `PROMPT.md` e `CLAUDE_CODE.md` removidos (obsoletos com o fim do Coworker).
- Push do naruto-game levando os dois commits; Local = Remote confirmado.

## Decisões / aprendizados
- Caminho relativo de dois níveis é frágil (já quebrou no move anterior) — `CLAUDE.md`
  referencia a governança comum por **caminho absoluto**.
- Registro histórico não se reescreve para "corrigir" caminho: `141`/`Changelog:23` ficam.
- Conflito herdado "commit+push juntos" foi **revogado**; a operação comum usa gates separados.

## Split do CONTEXT.md (`4b15c7a`)

- **O quê:** `CONTEXT.md` (235 linhas monolíticas) dividido em `contexto.md` (69, estado
  conceitual), `decisoes.md` (37, decisões fechadas) e
  `docs/resumo-sessao-{pre-5,5,6,7,8,9,10,11}.md` (histórico por sessão).
- **Motivo:** o arquivo único misturava estado atual e histórico de 10 sessões; a mistura
  já havia gerado contradição real (as linhas 141/159 afirmavam caminhos diferentes para o
  mesmo diretório — uma histórica, outra vigente).
- **Duplicados resolvidos:** config alvo 720p (4 cópias → `decisoes.md`); renomear
  `floresta_da_nevoa` → `zona_5` (3 cópias → uma pendência em `contexto.md`);
  `KamuiTrigger.exit_position` reclassificado de decisão para pendência.
- **Descartado:** "Como usar este arquivo" (obsoleto); "Estrutura de pastas" (mapa de
  diretórios que defasa a cada arquivo criado — o Code lê o disco); "Regra permanente zero
  commits" (já migrada ao `CLAUDE.md` como gate de play-test).
- **Numeração das sessões:** confirmada por FATO via marcadores nas mensagens de commit,
  não por data (sessões 3 e 4 caem no mesmo dia; a 10 se espalha por dois). Sessões 1–4
  consolidadas em `resumo-sessao-pre-5.md` — a governança de controle de sessão ainda não
  existia à época e o histórico não permite reconstruir a divisão.
- **Renumeração:** o heading "Próximo bloco — Sessão 10" (trabalho futuro rotulado com
  número já consumido) virou "Próximos passos (Sessão 12)".

## Continuação — 20/07/2026

**Commits:** `903175f`, `f368545` (ambos publicados no mesmo push)

Nota de numeração: o trabalho de 20/07 é **continuação desta sessão**, não uma Sessão 12.
A numeração do projeto segue blocos de trabalho, não datas (a Sessão 10 se espalhou por
02–03/07). O "Próximos passos (Sessão 12)" do `contexto.md` segue válido — é trabalho que
ainda não começou.

### Auditoria read-only do repo

Varredura de README, `documentation/` × `docs/`, raiz e estrutura interna. Achados:

- 6 defasagens no README (detalhadas adiante).
- `documentation/` operando como catch-all: mídia de validação, docs de produto e legado
  misturados; nomes `docs/` e `documentation/` quase sinônimos para conteúdos distintos.
- ~9 MB de mídia não referenciada nem pelo README nem pelo código.
- **Case-mismatch no git:** o índice tinha `documentation/Zabuza.png` e `Zabuza_melee.png`
  com Z maiúsculo enquanto o disco mostrava minúsculo — invisível no Windows
  (case-insensitive), apareceria como arquivo duplicado/ausente em clone Linux.
  Normalizado durante o move.

### Capturas novas

6 imagens do jogo em execução, extraídas de sessão de gameplay: `rasengan`, `shuriken`,
`hit-feedback`, `wall-slide`, `dash`, `dialogo-ichiraku`. Chegaram com extensão duplicada
(`.png.png`) — renomeadas antes de entrar no versionamento.

### Reorganização de `documentation/` (`903175f`)

- Criadas `documentation/capturas/` (15 imagens) e `documentation/produto/` (7 documentos).
- 5 mídias históricas externalizadas para `naruto-game-assets-brutos/capturas-historicas/`
  (~7,9 MB): `teste1.mp4`, `Teste2.mp4`, `Teste1.png`, `Wall_slide.png`, `dash_hud.png`.
- `documentation/README.md` (órfão da Semana 1) removido.
- Os 4 links de imagem do README atualizados **no mesmo commit** — mover sem corrigir
  deixaria a vitrine pública com imagem quebrada até a reescrita.
- FATO registrado: externalizar **não** reduz o tamanho do repo (os blobs permanecem na
  história do git); o ganho é organização, não peso.

### README reescrito (`f368545`)

Seis defasagens corrigidas:

1. **Renderer** — `gl_compatibility`, não "Forward Plus + Jolt Physics". (Jolt é
   `3d/physics_engine`, inerte num jogo 2D; saiu da apresentação.)
2. **Teclas do chakra** — Shuriken **K**, Rasengan **O**. A tabela dizia J e L, e a tecla
   L não existe no Input Map.
3. **FSM do Player** — 12 estados (lidos do `enum State`), não 8.
4. **Estrutura de pastas** — reconstruída a partir do disco real.
5. **Collision layers (§2 e §4)** — Player é layer 8 (`player_body`), MeleeNinja é layer 7
   (`enemy_body`), e a `DetectionArea` mascara a layer 8. O texto dizia "layer 1 (world)"
   e justificava a ausência de uma layer dedicada que, na prática, já existia.
6. **Gamepad** — ver decisão abaixo.

Adicionado: seção da camada narrativa (DialogueSystem, cutscenes, LevelManager,
SaveSystem, boss Zabuza, as 5 zonas), seção "Status" honesta (2 zonas em placeholder,
3 não criadas, 13 sprites não integrados à FSM) e as capturas distribuídas com
enquadramento explícito de arena de teste. Preservados íntegros: arquitetura, mecânicas de
combate, FSM do inimigo, pulo preditivo e o post-mortem de bugs.

### Decisão — gamepad: implementado, não validado

FATO: o `project.godot` tem mapeamento de joypad nas **8 ações** (analógico/DPad, A, X, Y,
B, gatilho direito), enquanto o `contexto.md` afirmava "sem suporte a controle". O Gomes
não tem controle e nunca testou.

Decidido: os binds **permanecem** no `project.godot`; o README registra como "configurado
no Input Map, ainda não validado em hardware"; o `contexto.md` foi corrigido e a validação
virou pendência aberta. **Suporte não verificado não se anuncia como feature em vitrine
pública.**

### Método — parar ao encontrar contradição

O handoff do Chat instruiu remover a tabela de gamepad afirmando que "o `project.godot`
não tem ações de joypad", e atribuiu a `decisoes.md` uma frase que na verdade estava no
`contexto.md`. O Executor verificou o disco antes de escrever, encontrou os binds, **parou
e reportou** em vez de obedecer.

Obedecer teria produzido o pior resultado: um README **omitindo** um mapeamento que existe
e funciona, com o `project.godot` seguindo cheio de binds — a defasagem mudaria de lugar
em vez de sumir. A regra "confirmar em disco antes de afirmar" foi o que separou instrução
de fato.

### Consolidação sob `docs/` e limpeza da raiz (`0577182`)

- 34 renames; `documentation/` deixou de existir. A raiz saiu de 6 `.md` soltos para **2** —
  `README.md` e `CLAUDE.md`, ambos por restrição técnica (GitHub e Claude Code só os
  carregam da raiz).
- Estrutura final: `docs/` com `contexto.md`, `decisoes.md` e `backlog.md` na raiz;
  `historico/` (changelog + 8 resumos de sessão); `capturas/` (15 imagens); `produto/`
  (7 documentos).
- Renomeações: `SUGESTOES.md` → `backlog.md` e `Changelog.md` → `changelog.md` —
  minúsculas e nome que diz o que é.

**Ponteiros atualizados no mesmo commit:** os 12 links de imagem do README, a árvore de
pastas do README, `CLAUDE.md`, `contexto.md`, `decisoes.md`, `changelog.md`, e a regra do
`.gitignore` (`documentation/**/*.import` → `docs/**/*.import`, para os `.import` que o
Godot regenera no novo caminho). Mover sem corrigir deixaria a vitrine pública com imagem
quebrada.

**Decisão — comentários em código não foram tocados:** 4 comentários em `.tscn`/`.gd`
citam `SUGESTOES.md` (`levels/zona_2_casa_central.tscn`, `scenes/cutscenes/akatsuki_hideout.tscn`,
`scripts/cutscenes/akatsuki_hideout.gd`, `scripts/cutscenes/ichiraku.gd`). Citam o documento
pelo **nome + número do item**, não por caminho — continuam resolvendo por busca. Editar
2 cenas e 2 scripts numa reorganização de docs ampliaria o risco sem ganho.
