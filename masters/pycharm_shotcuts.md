Como você já é experiente, eu focaria nos atalhos que eliminam navegação manual, mouse e refactors inseguros. Assumindo **PyCharm no Windows com o keymap padrão**:

## Os 15 que mais valem memorizar

| Atalho | Ação |
|---|---|
| `Shift` duas vezes | Buscar qualquer coisa no projeto |
| `Ctrl + Shift + A` | Procurar e executar qualquer ação do PyCharm |
| `Ctrl + Shift + N` | Abrir arquivo pelo nome |
| `Ctrl + Alt + Shift + N` | Buscar função, variável ou símbolo |
| `Ctrl + E` | Arquivos recentes |
| `Ctrl + B` | Ir para declaração |
| `Ctrl + Alt + B` | Ir para implementação |
| `Ctrl + Alt + ←/→` | Voltar/avançar no histórico de navegação |
| `Ctrl + F12` | Estrutura do arquivo atual |
| `Alt + F7` | Encontrar todos os usos |
| `Alt + Enter` | Quick fix/intention actions |
| `Shift + F6` | Renomear com segurança |
| `Ctrl + Alt + L` | Formatar código |
| `Ctrl + W` | Expandir seleção semanticamente |
| `Ctrl + Shift + W` | Reduzir seleção |

## Refactoring

| Atalho | Ação |
|---|---|
| `Ctrl + Alt + Shift + T` | Mostrar todos os refactors disponíveis |
| `Shift + F6` | Rename |
| `Ctrl + Alt + M` | Extract method |
| `Ctrl + Alt + V` | Introduce variable |
| `Ctrl + Alt + P` | Introduce parameter |
| `Ctrl + Alt + F` | Introduce field |
| `Ctrl + Alt + N` | Inline |
| `Ctrl + F6` | Change signature |
| `F6` | Mover classe, função ou arquivo |
| `Alt + Delete` | Safe delete |

Para seu assessment, `Shift + F6`, `Ctrl + Alt + M` e `Ctrl + Alt + V` serão especialmente úteis.

## Navegação em código grande

| Atalho | Ação |
|---|---|
| `Ctrl + N` | Buscar classe |
| `Ctrl + Shift + N` | Buscar arquivo |
| `Ctrl + Alt + Shift + N` | Buscar símbolo |
| `Ctrl + U` | Ir para classe ou método pai |
| `Ctrl + Shift + B` | Ir para a declaração do tipo |
| `Alt + ↑/↓` | Método anterior/próximo |
| `Ctrl + H` | Hierarquia de classes |
| `Ctrl + Alt + H` | Call hierarchy |
| `Ctrl + Shift + Backspace` | Voltar ao último local editado |
| `F2` / `Shift + F2` | Próximo/anterior erro ou warning |
| `Ctrl + G` | Ir para linha |

Para navegar entre `User`, `Customer` e `Admin`, use bastante:

```text
Ctrl + H       → ver hierarquia
Ctrl + B       → ir para definição
Ctrl + Alt + B → ver implementações
Ctrl + U       → voltar para o método/classe pai
```

## Edição rápida

| Atalho | Ação |
|---|---|
| `Ctrl + D` | Duplicar linha ou seleção |
| `Ctrl + Y` | Excluir linha |
| `Ctrl + Shift + ↑/↓` | Mover statement |
| `Alt + Shift + ↑/↓` | Mover linha |
| `Ctrl + Shift + Enter` | Completar statement |
| `Ctrl + /` | Comentar/descomentar linha |
| `Ctrl + Shift + /` | Comentário em bloco |
| `Ctrl + P` | Mostrar parâmetros da função |
| `Ctrl + Q` | Quick documentation |
| `Ctrl + Alt + O` | Organizar/remover imports |
| `Ctrl + Alt + L` | Reformatar |
| `Ctrl + Space` | Autocomplete |

## Multi-cursor e seleções

| Atalho | Ação |
|---|---|
| `Alt + J` | Selecionar próxima ocorrência |
| `Ctrl + Alt + Shift + J` | Selecionar todas as ocorrências |
| `Alt + Shift + clique` | Adicionar cursor |
| `Ctrl + W` | Expandir seleção |
| `Ctrl + Shift + W` | Reduzir seleção |
| `Esc` | Sair do modo multi-cursor |

## Executar e debugar

| Atalho | Ação |
|---|---|
| `Shift + F10` | Run |
| `Shift + F9` | Debug |
| `Ctrl + Shift + F10` | Executar arquivo, função ou teste atual |
| `Ctrl + F8` | Adicionar/remover breakpoint |
| `F8` | Step over |
| `F7` | Step into |
| `Shift + F8` | Step out |
| `Alt + F9` | Run to cursor |
| `Alt + F8` | Evaluate expression |
| `Ctrl + F2` | Parar execução |

## Testes e Git

| Atalho | Ação |
|---|---|
| `Ctrl + Shift + T` | Abrir ou criar teste correspondente |
| `Ctrl + K` | Commit |
| `Ctrl + Shift + K` | Push |
| `Ctrl + T` | Atualizar projeto |
| `Alt + `` | Menu de operações Git/VCS |
| `Alt + 9` | Janela do Version Control |

## Interface sem mouse

| Atalho | Ação |
|---|---|
| `Alt + 1` | Project panel |
| `Alt + 4` | Run |
| `Alt + 5` | Debug |
| `Alt + 6` | Problems |
| `Alt + 7` | Structure |
| `Alt + F12` | Terminal |
| `Esc` | Voltar ao editor |
| `Shift + Esc` | Fechar painel ativo |
| `Ctrl + Shift + F12` | Esconder todos os painéis |

Minha shortlist pessoal seria:

```text
Double Shift
Ctrl + Shift + A
Ctrl + E
Ctrl + B
Ctrl + Alt + B
Ctrl + Alt + Left/Right
Alt + F7
Alt + Enter
Shift + F6
Ctrl + Alt + M
Ctrl + W
Ctrl + Alt + L
Ctrl + Shift + F10
Ctrl + F8
Alt + J
```

Os atalhos são configuráveis em `Ctrl + Alt + S → Keymap`; alguns podem entrar em conflito com atalhos globais do Windows. A referência oficial está na [documentação de atalhos do PyCharm](https://www.jetbrains.com/help/pycharm/mastering-keyboard-shortcuts.html) e no [keymap completo](https://www.jetbrains.com/help/pycharm/keymap-reference.html).
