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
