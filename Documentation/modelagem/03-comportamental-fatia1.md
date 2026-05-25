# 03 — Modelagem comportamental da Fatia 1

## Fatia 1 — Matrícula do aluno em curso e liberação de acesso ao conteúdo

**Histórias cobertas:** US-ADM-006, US-ALU-001, US-ALU-002 e US-ALU-003.

Escolhemos **diagrama de sequência** para esta fatia porque ela envolve coordenação entre mais de um ator e mais de um subsistema. O fluxo atravessa a interface, a lógica de matrícula, a validação do curso e a liberação de acesso ao conteúdo, além de possuir caminhos alternativos conforme a origem da matrícula e a existência de restrições de acesso.

## Diagrama de sequência

```mermaid
sequenceDiagram
    actor Aluno
    actor Administrador as Administrador da Instituição
    participant Frontend as Portal Web
    participant CursoService as Serviço de Cursos
    participant MatriculaService as Serviço de Matrículas
    participant AutorizacaoService as Serviço de Autorização

    Aluno->>Frontend: visualizarCursosDisponiveis()
    Frontend->>CursoService: listarCursosDisponiveis(instituicaoId)
    CursoService-->>Frontend: cursosDisponiveis

    alt Autoinscrição habilitada
        Aluno->>Frontend: solicitarMatricula(cursoId)
        Frontend->>MatriculaService: criarMatricula(alunoId, cursoId, origem="AUTO")
    else Matrícula gerenciada pela instituição
        Administrador->>Frontend: matricularAluno(alunoId, cursoId)
        Frontend->>MatriculaService: criarMatricula(alunoId, cursoId, origem="ADMIN")
    end

    MatriculaService->>CursoService: validarCursoDisponivel(cursoId)

    alt Curso disponível e aluno apto
        CursoService-->>MatriculaService: validacaoOk
        MatriculaService->>MatriculaService: registrarMatricula()
        MatriculaService->>AutorizacaoService: liberarAcesso(alunoId, cursoId)
        AutorizacaoService-->>MatriculaService: acessoLiberado
        MatriculaService-->>Frontend: matriculaConfirmada()
        opt Exibir confirmação ao ator que iniciou o fluxo
            Frontend-->>Aluno: exibirCursoNaAreaDoAluno()
            Frontend-->>Administrador: exibirConfirmacaoMatricula()
        end
    else Aluno já matriculado
        CursoService-->>MatriculaService: alunoJaMatriculado
        MatriculaService-->>Frontend: erroMatriculaDuplicada()
    else Curso indisponível ou bloqueado
        CursoService-->>MatriculaService: cursoIndisponivel
        MatriculaService-->>Frontend: erroCursoIndisponivel()
    end

    Aluno->>Frontend: acessarConteudo(cursoId)
    Frontend->>AutorizacaoService: validarAcesso(alunoId, cursoId)

    alt Matrícula ativa
        AutorizacaoService-->>Frontend: acessoPermitido
        Frontend-->>Aluno: exibirModulosEAulas()
    else Sem matrícula ativa
        AutorizacaoService-->>Frontend: acessoNegado
        Frontend-->>Aluno: exibirMensagemSemPermissao()
    end
```

## Observação

O foco deste diagrama não é mostrar telas isoladas, mas sim a sequência ponta a ponta que transforma um aluno sem acesso em um aluno matriculado com acesso liberado ao conteúdo do curso.