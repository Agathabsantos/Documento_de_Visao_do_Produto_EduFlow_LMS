# 01 — Diagrama de classes

Com base nas três fatias verticais selecionadas na Seção 0, este diagrama de classes modela apenas os elementos centrais do domínio envolvidos em: **matrícula e liberação de acesso**, **criação e publicação de conteúdo pedagógico** e **realização de avaliação com progresso e certificação**. O objetivo não é representar o sistema inteiro, mas sim o subconjunto de classes que participa diretamente desses fluxos.

A modelagem foi organizada para evitar classes excessivamente grandes e para manter rastreabilidade clara com as histórias de usuário do Trabalho 2. Para isso, algumas decisões foram tomadas:

- `Matricula` foi modelada como uma classe própria, e não apenas como um relacionamento entre `Aluno` e `Curso`, porque ela possui estado, origem, data e responsabilidade de controlar o acesso ao conteúdo.
- `ConteudoPublicavel` foi modelada como **classe abstrata**, pois `Aula` e `Avaliacao` compartilham características e comportamentos de publicação.
- `TentativaAvaliacao` e `RespostaAluno` foram separadas para representar corretamente o salvamento automático, a retomada da prova e o cálculo da nota.
- `ProgressoCurso` foi modelado separadamente de `Matricula` para concentrar a lógica de acompanhamento da jornada do aluno e a verificação de emissão de certificado.

## Diagrama de classes

```mermaid
classDiagram
direction LR

class Usuario {
  <<abstract>>
  -UUID id
  -String nome
  -String email
  -String senhaHash
  -StatusUsuario status
  +Boolean estaAtivo()
}

class AdministradorInstitucional {
  +Matricula matricularAluno(aluno: Aluno, curso: Curso)
  +void removerMatricula(matricula: Matricula)
}

class Professor {
  +Modulo criarModulo(curso: Curso, titulo: String)
  +Aula criarAula(modulo: Modulo, titulo: String)
  +void publicarConteudo(conteudo: ConteudoPublicavel)
}

class Aluno {
  +Matricula solicitarMatricula(curso: Curso)
  +TentativaAvaliacao iniciarAvaliacao(avaliacao: Avaliacao)
  +Certificado emitirCertificado(matricula: Matricula)
}

class Instituicao {
  -UUID id
  -String nome
  -String codigo
  +void adicionarCurso(curso: Curso)
  +Boolean pertenceAoContexto(usuario: Usuario)
}

class Curso {
  -UUID id
  -String titulo
  -String descricao
  -StatusCurso status
  +void vincularProfessor(professor: Professor)
  +Boolean possuiAlunoMatriculado(aluno: Aluno)
  +List~ConteudoPublicavel~ listarConteudosPublicados()
}

class Matricula {
  -UUID id
  -DateTime dataMatricula
  -StatusMatricula status
  -OrigemMatricula origem
  +void ativarAcesso()
  +void cancelar()
  +Boolean podeAcessarConteudo()
}

class Modulo {
  -UUID id
  -String titulo
  -String descricao
  -int ordem
  +Aula adicionarAula(titulo: String)
  +void reordenarAulas()
}

class ConteudoPublicavel {
  <<abstract>>
  -UUID id
  -String titulo
  -StatusPublicacao statusPublicacao
  -DateTime dataPublicacao
  +void publicar()
  +void despublicar()
  +Boolean estaPublicado()
}

class Aula {
  -String descricao
  -int ordem
  +void anexarMaterial(material: MaterialAula)
}

class MaterialAula {
  -UUID id
  -String nomeArquivo
  -TipoMaterial tipo
  -String url
  +Boolean validarUpload()
}

class Avaliacao {
  -Decimal notaMinimaAprovacao
  -int tentativasMaximas
  -int tempoLimiteMinutos
  +void adicionarQuestao(pergunta: String)
  +void definirCriterios(notaMinima: Decimal, tentativas: int)
  +Decimal corrigir(tentativa: TentativaAvaliacao)
}

class Questao {
  -UUID id
  -String enunciado
  -int ordem
  +void adicionarAlternativa(texto: String, correta: Boolean)
}

class Alternativa {
  -UUID id
  -String texto
  -Boolean correta
}

class TentativaAvaliacao {
  -UUID id
  -DateTime dataInicio
  -DateTime dataUltimoSalvamento
  -StatusTentativa status
  -Decimal notaObtida
  +void registrarResposta(questao: Questao, alternativa: Alternativa)
  +void salvarRascunho()
  +Decimal finalizar()
  +Boolean podeSerRetomada()
}

class RespostaAluno {
  -UUID id
  -DateTime dataResposta
  +void atualizarAlternativa(alternativa: Alternativa)
}

class ProgressoCurso {
  -UUID id
  -Decimal percentualConclusao
  -int aulasConcluidas
  -int avaliacoesConcluidas
  +void registrarConclusaoAula(aula: Aula)
  +void atualizarPercentual()
  +Boolean podeEmitirCertificado()
}

class Certificado {
  -UUID id
  -String codigoValidacao
  -Date dataEmissao
  +void emitir()
  +Boolean validarAutenticidade()
}

Usuario <|-- AdministradorInstitucional
Usuario <|-- Professor
Usuario <|-- Aluno

ConteudoPublicavel <|-- Aula
ConteudoPublicavel <|-- Avaliacao

Instituicao "1" o-- "0..*" Curso : oferece
Instituicao "1" o-- "0..*" Usuario : possui

Professor "0..*" -- "1..*" Curso : leciona
Aluno "1" -- "0..*" Matricula : possui
Curso "1" -- "0..*" Matricula : recebe

Curso "1" *-- "1..*" Modulo : compõe
Modulo "1" *-- "1..*" Aula : compõe
Modulo "1" *-- "0..*" Avaliacao : compõe
Aula "1" *-- "0..*" MaterialAula : contém

Avaliacao "1" *-- "1..*" Questao : compõe
Questao "1" *-- "2..*" Alternativa : compõe

Matricula "1" -- "0..*" TentativaAvaliacao : registra
Avaliacao "1" -- "0..*" TentativaAvaliacao : recebe
TentativaAvaliacao "1" *-- "1..*" RespostaAluno : compõe
RespostaAluno "1" --> "1" Questao : refere-se a
RespostaAluno "1" --> "1" Alternativa : seleciona

Matricula "1" *-- "1" ProgressoCurso : acompanha
Matricula "1" o-- "0..1" Certificado : gera

TentativaAvaliacao ..> ProgressoCurso : atualiza
AdministradorInstitucional ..> Matricula : gerencia
Professor ..> ConteudoPublicavel : publica
Aluno ..> Certificado : solicita