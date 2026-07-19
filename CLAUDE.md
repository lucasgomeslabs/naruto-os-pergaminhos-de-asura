# CLAUDE.md — naruto-game

Arquivo carregado automaticamente ao abrir este repo. Contém só o que é específico do
jogo; a governança de processo vem da operação comum.

## Governança comum (ler primeiro)

A operação é **Gomes + Chat + Code**. As regras de processo — papéis, gates separados de
commit e push, DIFF antes de aplicar, execução em blocos, abertura de sessão read-only —
vivem em `C:\Projetos\projetos-pessoais\projetos-pessoais-operacoes\`:

- `prompt.md` — governança do Chat (Tech Lead)
- `code.md` — governança do Code (Executor)
- `estrutura.md` — organização dos repos e do estado

## Projeto

Godot 4.6 · GDScript · 2D platformer. Estado e stack detalhada em `CONTEXT.md`.
(O split de `CONTEXT.md` em `contexto.md` + `docs/resumo-sessao-N.md` é bloco futuro.)

## Específico do game

### Gate de play-test (instanciação-game do gate de verificação)

Commit só após o Gomes testar no Godot Editor. Em jogo, sensação, fluidez e hit feel só
o humano jogando avalia — nenhum teste automatizado substitui. Complementa, não revoga,
os gates de commit/push do `code.md`.

### Higiene de cena (Godot) — antes do commit final

- Remover `print()` de debug dos scripts.
- Remover nós temporários de teste das cenas (ex.: `DebugZoneSwitch` no `test_stage`).
- Manter `debug_zone_switch.gd` e `DebugHUD` — ferramentas ativas de desenvolvimento.
- Manter comentários no código.
- **NUNCA** remover arquivos `.uid` (gerados pelo Godot, necessários).

### Comando

- `#lista` → exibe o backlog de sugestões (`SUGESTOES.md`).
