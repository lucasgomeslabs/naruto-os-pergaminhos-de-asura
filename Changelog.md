# Changelog - Naruto: Os Pergaminhos de Asura (MVP)

Toda alteração relevante na estrutura do projeto, correções de bugs e adições de mecânicas são registradas de forma transparente neste documento.

> O histórico detalhado sessão a sessão (commits, decisões, pendências) vive no `CONTEXT.md`.

---

## [2026-07-02] - Higiene do repo (sessão 10, bloco 1)

### 🧹 Organização (Changed)
- **Docs para `documentation/`:** movidos da raiz os 6 documentos de metodologia/escopo (`Atualização status`, `Jogo completo Naruto`, `METODOLOGIA OBRIGATÓRIA DE DESENVOL`, `OTIMIZAÇÃO EXTREMA E LEVEZA`, `expansão do mapa`, `implementação futura# Naruto`). Na raiz permanecem só os operacionais: `CLAUDE.md`, `CLAUDE_CODE.md`, `CONTEXT.md`, `Changelog.md`, `README.md`, `PROMPT.md`, `SUGESTOES.md`.
- **Assets brutos externalizados:** `NOPA/` e `naruto_sprites.zip` movidos para `C:\Projetos\naruto-game-assets-brutos\` (fora do repo); 36 `.import` gerados pelo Godot para os PNGs de NOPA removidos (verificação 1 a 1); `.gitignore` agora bloqueia `NOPA/` e `naruto_sprites.zip`.
- **`README.md.md` renomeado:** era um write-up técnico de combate/IA com extensão duplicada → `documentation/combate-ia-writeup.md`.
- **Controles corrigidos na doc:** `documentation/Atualização status.txt` atualizado de H/J/L/Shift para o input map real J/K/O/Shift.
- **`.claude/settings.local.json` untracked** (estado local de permissões do Claude Code) e adicionado ao `.gitignore`.

Zero mudança de comportamento do jogo — nenhuma cena, script ou asset importado em uso foi tocado.

---

## [Unreleased] - Semana 1: Core Gameplay

### 🛠️ Corrigido (Fixed)
- **Tracking de Arquivos:** Resolvido o erro silencioso `File has not been read yet` nas ferramentas de automação após a migração dos arquivos da subpasta para a raiz do repositório.
- **Acoplamento de Entrada:** Correção da lógica de agachar no teclado/controle, acumulando as ações de agachar e interagir de acordo com o Manual de Escopo Oficial.

### 🏗️ Adicionado (Added)
- **Mapeamento de Inputs:** Injeção das 8 Input Actions oficiais diretamente no `project.godot` (suporte a teclado e controles Xbox/PlayStation via Godot Input Actions).
- **FSM do Player:** Implementada Máquina de Estados Finita (8 estados) no `player_controller.gd` com suporte a Coyote Time (0.12s) e Input Buffer (0.15s).
- **Mecânica de Chakra:** Adicionado o sistema centralizado de Chakra com taxa de regeneração passiva e ativa.
- **Sistema de Colisão de Combate:** Criados os scripts estruturais e reutilizáveis para `hitbox.gd` e `hurtbox.gd` na pasta de componentes.
- **Boneco de Testes (Dummy):** Criada a cena física e lógica inicial para a entidade do Dummy (`dummy.tscn` / `dummy.gd`) com vida base (3 HP).
- **Ambiente de Teste:** Configuração de zonas de perigo (*Kill Zone*) com respawn funcional para o Y > 1000 e Debug HUD para exibição de dados em tempo real.
- **Organização de Árvore:** Remoção completa da subpasta antiga e limpeza da File Tree.

### ⚠️ Pendente (Foco Atual)
- Vinculação da `Area2D` de Hitbox no nó do Player.
- Instanciar fisicamente o `dummy.tscn` na cena de testes (`test_stage.tscn`).
- Definição dos nomes das Collision Layers no `project.godot` (Layer 2 para Player Hitbox, Layer 3 para Enemy Hurtbox).
- Substituição dos métodos `pass` em `_enable_attack_hitbox` no controlador do jogador por chamadas físicas ativas.