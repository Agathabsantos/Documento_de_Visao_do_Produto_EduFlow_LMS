# 03 — Modelagem comportamental da Fatia 2

## Fatia 2 — Professor cria, organiza e publica o conteúdo do curso

**Histórias cobertas:** US-PED-001, US-PED-002, US-PED-003, US-PED-004, US-PED-005, US-PED-006 e US-PED-007.

Escolhemos **diagrama de atividades** para esta fatia porque ela representa melhor um fluxo de trabalho com decisões sucessivas: criação de módulo, criação de aula, upload de materiais, criação de avaliação, definição de critérios e decisão entre publicar ou manter em rascunho. Além disso, a notação com raias ajuda a separar claramente o que é ação do professor e o que é responsabilidade do sistema.

## Diagrama de atividades

```mermaid
flowchart LR
    subgraph Professor
        A([Início])
        B[Selecionar curso]
        C[Criar ou editar módulo]
        D[Criar aula]
        E{Deseja anexar materiais?}
        F[Enviar materiais]
        G{Deseja criar avaliação?}
        H[Definir questões e critérios]
        I{Deseja publicar agora?}
    end

    subgraph Sistema
        B1[Validar vínculo do professor com o curso]
        C1[Persistir módulo]
        D1[Persistir aula]
        F1[Validar tipo e armazenar material]
        H1[Persistir avaliação e critérios]
        I1{Conteúdo está completo?}
        J[Publicar conteúdo]
        K[Salvar como rascunho]
        X[Exibir erro de permissão]
        Z([Fim])
    end

    A --> B --> B1
    B1 -->|Professor vinculado ao curso| C
    B1 -->|Professor não vinculado| X --> Z

    C --> C1 --> D --> D1 --> E

    E -->|Sim| F --> F1 --> G
    E -->|Não| G

    G -->|Sim| H --> H1 --> I
    G -->|Não| I

    I -->|Sim| I1
    I -->|Não| K --> Z

    I1 -->|Sim| J --> Z
    I1 -->|Não| K --> Z
```

## Observação

Neste fluxo, a publicação só acontece quando o sistema considera que o conteúdo está em condição válida para ficar visível aos alunos. Caso contrário, o conteúdo permanece salvo como rascunho, respeitando a regra de que materiais incompletos não devem ser publicados.