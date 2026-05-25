# 02 — Modelo Entidade-Relacionamento (MER)

A partir do diagrama de classes da Seção 1, este MER representa apenas as **classes persistentes** das três fatias selecionadas: **matrícula e liberação de acesso**, **criação e publicação de conteúdo pedagógico** e **realização de avaliação com progresso e certificação**. Foram modeladas apenas as entidades cujos dados precisam permanecer armazenados no sistema após a execução dos fluxos.

Para manter consistência com o restante do trabalho, o modelo foi organizado em torno dos seguintes núcleos:
- **gestão institucional e usuários**;
- **estrutura pedagógica do curso**;
- **matrícula e progresso do aluno**;
- **avaliação e certificação**.

## Coerência com o diagrama de classes

Embora o MER tenha sido construído a partir do diagrama de classes, algumas adaptações foram feitas para adequá-lo ao modelo relacional. A principal delas foi transformar a hierarquia `Usuario -> AdministradorInstitucional`, `Professor` e `Aluno` em uma única entidade `usuario`, diferenciada pelo atributo `tipo_usuario`, já que os três perfis compartilham os mesmos dados persistentes básicos. Da mesma forma, a classe abstrata `ConteudoPublicavel` não foi mapeada como tabela própria, e seus atributos comuns foram incorporados diretamente em `aula` e `avaliacao`. Além disso, o relacionamento muitos-para-muitos entre professor e curso foi resolvido pela entidade associativa `professor_curso`, conforme prática comum em bancos relacionais. Por fim, comportamentos e atributos derivados do modelo orientado a objetos, como regras de publicação, cálculo de nota ou verificação de emissão de certificado, permanecem no nível de domínio e não aparecem como estruturas independentes no MER.

## Entidades e atributos

### instituicao
- `id: UUID` **PK**
- `nome: VARCHAR(150)`
- `codigo: VARCHAR(50)`

### usuario
- `id: UUID` **PK**
- `instituicao_id: UUID` **FK → instituicao.id**
- `nome: VARCHAR(120)`
- `email: VARCHAR(120)`
- `senha_hash: VARCHAR(255)`
- `status: VARCHAR(20)`
- `tipo_usuario: VARCHAR(30)`

> O atributo `tipo_usuario` identifica se o usuário é administrador institucional, professor ou aluno.

### curso
- `id: UUID` **PK**
- `instituicao_id: UUID` **FK → instituicao.id**
- `titulo: VARCHAR(150)`
- `descricao: TEXT`
- `status: VARCHAR(20)`

### professor_curso
- `professor_id: UUID` **PK, FK → usuario.id**
- `curso_id: UUID` **PK, FK → curso.id**

> Entidade associativa criada para resolver o relacionamento N:N entre professor e curso.

### matricula
- `id: UUID` **PK**
- `aluno_id: UUID` **FK → usuario.id**
- `curso_id: UUID` **FK → curso.id**
- `data_matricula: TIMESTAMP`
- `status: VARCHAR(20)`
- `origem: VARCHAR(20)`

### modulo
- `id: UUID` **PK**
- `curso_id: UUID` **FK → curso.id**
- `titulo: VARCHAR(150)`
- `descricao: TEXT`
- `ordem: INT`

### aula
- `id: UUID` **PK**
- `modulo_id: UUID` **FK → modulo.id**
- `titulo: VARCHAR(150)`
- `descricao: TEXT`
- `ordem: INT`
- `status_publicacao: VARCHAR(20)`
- `data_publicacao: TIMESTAMP`

### material_aula
- `id: UUID` **PK**
- `aula_id: UUID` **FK → aula.id**
- `nome_arquivo: VARCHAR(150)`
- `tipo: VARCHAR(30)`
- `url: VARCHAR(255)`

### avaliacao
- `id: UUID` **PK**
- `modulo_id: UUID` **FK → modulo.id**
- `titulo: VARCHAR(150)`
- `status_publicacao: VARCHAR(20)`
- `data_publicacao: TIMESTAMP`
- `nota_minima_aprovacao: DECIMAL(5,2)`
- `tentativas_maximas: INT`
- `tempo_limite_minutos: INT`

### questao
- `id: UUID` **PK**
- `avaliacao_id: UUID` **FK → avaliacao.id**
- `enunciado: TEXT`
- `ordem: INT`

### alternativa
- `id: UUID` **PK**
- `questao_id: UUID` **FK → questao.id**
- `texto: TEXT`
- `correta: BOOLEAN`

### tentativa_avaliacao
- `id: UUID` **PK**
- `matricula_id: UUID` **FK → matricula.id**
- `avaliacao_id: UUID` **FK → avaliacao.id**
- `data_inicio: TIMESTAMP`
- `data_ultimo_salvamento: TIMESTAMP`
- `status: VARCHAR(20)`
- `nota_obtida: DECIMAL(5,2)`

### resposta_aluno
- `id: UUID` **PK**
- `tentativa_id: UUID` **FK → tentativa_avaliacao.id**
- `questao_id: UUID` **FK → questao.id**
- `alternativa_id: UUID` **FK → alternativa.id**
- `data_resposta: TIMESTAMP`

### progresso_curso
- `id: UUID` **PK**
- `matricula_id: UUID` **FK → matricula.id**
- `percentual_conclusao: DECIMAL(5,2)`
- `aulas_concluidas: INT`
- `avaliacoes_concluidas: INT`

### certificado
- `id: UUID` **PK**
- `matricula_id: UUID` **FK → matricula.id**
- `codigo_validacao: VARCHAR(100)`
- `data_emissao: DATE`

## MER

```mermaid
erDiagram
    INSTITUICAO ||--o{ USUARIO : possui
    INSTITUICAO ||--o{ CURSO : oferece

    USUARIO ||--o{ MATRICULA : realiza
    CURSO ||--o{ MATRICULA : recebe

    USUARIO ||--o{ PROFESSOR_CURSO : atua_em
    CURSO ||--o{ PROFESSOR_CURSO : possui

    CURSO ||--|{ MODULO : compoe
    MODULO ||--|{ AULA : compoe
    MODULO ||--o{ AVALIACAO : compoe

    AULA ||--o{ MATERIAL_AULA : contem

    AVALIACAO ||--|{ QUESTAO : compoe
    QUESTAO ||--|{ ALTERNATIVA : compoe

    MATRICULA ||--o{ TENTATIVA_AVALIACAO : registra
    AVALIACAO ||--o{ TENTATIVA_AVALIACAO : recebe

    TENTATIVA_AVALIACAO ||--|{ RESPOSTA_ALUNO : compoe
    QUESTAO ||--o{ RESPOSTA_ALUNO : referencia
    ALTERNATIVA ||--o{ RESPOSTA_ALUNO : selecionada_em

    MATRICULA ||--|| PROGRESSO_CURSO : acompanha
    MATRICULA ||--o| CERTIFICADO : gera

    INSTITUICAO {
        UUID id PK
        VARCHAR nome
        VARCHAR codigo
    }

    USUARIO {
        UUID id PK
        UUID instituicao_id FK
        VARCHAR nome
        VARCHAR email
        VARCHAR senha_hash
        VARCHAR status
        VARCHAR tipo_usuario
    }

    CURSO {
        UUID id PK
        UUID instituicao_id FK
        VARCHAR titulo
        TEXT descricao
        VARCHAR status
    }

    PROFESSOR_CURSO {
        UUID professor_id PK, FK
        UUID curso_id PK, FK
    }

    MATRICULA {
        UUID id PK
        UUID aluno_id FK
        UUID curso_id FK
        TIMESTAMP data_matricula
        VARCHAR status
        VARCHAR origem
    }

    MODULO {
        UUID id PK
        UUID curso_id FK
        VARCHAR titulo
        TEXT descricao
        INT ordem
    }

    AULA {
        UUID id PK
        UUID modulo_id FK
        VARCHAR titulo
        TEXT descricao
        INT ordem
        VARCHAR status_publicacao
        TIMESTAMP data_publicacao
    }

    MATERIAL_AULA {
        UUID id PK
        UUID aula_id FK
        VARCHAR nome_arquivo
        VARCHAR tipo
        VARCHAR url
    }

    AVALIACAO {
        UUID id PK
        UUID modulo_id FK
        VARCHAR titulo
        VARCHAR status_publicacao
        TIMESTAMP data_publicacao
        DECIMAL nota_minima_aprovacao
        INT tentativas_maximas
        INT tempo_limite_minutos
    }

    QUESTAO {
        UUID id PK
        UUID avaliacao_id FK
        TEXT enunciado
        INT ordem
    }

    ALTERNATIVA {
        UUID id PK
        UUID questao_id FK
        TEXT texto
        BOOLEAN correta
    }

    TENTATIVA_AVALIACAO {
        UUID id PK
        UUID matricula_id FK
        UUID avaliacao_id FK
        TIMESTAMP data_inicio
        TIMESTAMP data_ultimo_salvamento
        VARCHAR status
        DECIMAL nota_obtida
    }

    RESPOSTA_ALUNO {
        UUID id PK
        UUID tentativa_id FK
        UUID questao_id FK
        UUID alternativa_id FK
        TIMESTAMP data_resposta
    }

    PROGRESSO_CURSO {
        UUID id PK
        UUID matricula_id FK
        DECIMAL percentual_conclusao
        INT aulas_concluidas
        INT avaliacoes_concluidas
    }

    CERTIFICADO {
        UUID id PK
        UUID matricula_id FK
        VARCHAR codigo_validacao
        DATE data_emissao
    }