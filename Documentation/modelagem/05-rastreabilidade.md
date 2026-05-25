# 05 — Rastreabilidade

A tabela de rastreabilidade abaixo conecta as **fatias selecionadas**, as **histórias de usuário do Trabalho 2**, as **classes do diagrama de classes**, as **entidades do MER**, os **diagramas comportamentais** e os **casos de teste**. O objetivo é demonstrar que os artefatos produzidos ao longo do trabalho não foram construídos de forma isolada, mas sim como representações complementares do mesmo recorte funcional definido na Seção 0.

## Tabela de rastreabilidade

| Fatia | História(s) (T2) | Classes envolvidas | Entidades MER | Diagrama comportamental | Casos de teste |
| --- | --- | --- | --- | --- | --- |
| **Fatia 1 — Matrícula do aluno em curso e liberação de acesso ao conteúdo** | US-ADM-006, US-ALU-001, US-ALU-002, US-ALU-003 | `AdministradorInstitucional`, `Aluno`, `Instituicao`, `Curso`, `Matricula` | `instituicao`, `usuario`, `curso`, `matricula` | **Sequência** (`03-comportamental-fatia1.md`) | TC-FATIA1-01, TC-FATIA1-02 |
| **Fatia 2 — Professor cria, organiza e publica o conteúdo do curso** | US-PED-001, US-PED-002, US-PED-003, US-PED-004, US-PED-005, US-PED-006, US-PED-007 | `Professor`, `Curso`, `Modulo`, `Aula`, `MaterialAula`, `Avaliacao`, `Questao`, `Alternativa`, `ConteudoPublicavel` | `usuario`, `curso`, `professor_curso`, `modulo`, `aula`, `material_aula`, `avaliacao`, `questao`, `alternativa` | **Atividades** (`03-comportamental-fatia2.md`) | TC-FATIA2-01, TC-FATIA2-02 |
| **Fatia 3 — Aluno realiza avaliação, tem o progresso registrado, recebe nota e obtém certificado** | US-ALU-004, US-ALU-005, US-ALU-006, US-ALU-007, US-ALU-008, US-ALU-010 | `Aluno`, `Avaliacao`, `Questao`, `Alternativa`, `TentativaAvaliacao`, `RespostaAluno`, `ProgressoCurso`, `Certificado`, `Matricula` | `usuario`, `matricula`, `avaliacao`, `questao`, `alternativa`, `tentativa_avaliacao`, `resposta_aluno`, `progresso_curso`, `certificado` | **Estados** (`03-comportamental-fatia3.md`) | TC-FATIA3-01, TC-FATIA3-02 |

## Observações sobre a rastreabilidade

A tabela mostra que cada fatia selecionada na Seção 0 foi desdobrada de maneira consistente nas demais seções do trabalho. As histórias do Trabalho 2 orientaram a escolha das classes relevantes na Seção 1, que por sua vez deram origem às entidades persistentes da Seção 2. Na Seção 3, cada fatia foi modelada com o tipo de diagrama comportamental mais adequado ao seu comportamento predominante: sequência para interação entre atores e serviços, atividades para fluxo de trabalho do professor e estados para o ciclo de vida da tentativa de avaliação. Por fim, a Seção 4 validou essas mesmas fatias por meio de casos de teste rastreáveis aos fluxos e regras de negócio modelados.

Essa rastreabilidade é importante porque evidencia que o recorte feito na Seção 0 foi mantido de forma consistente ao longo de todo o documento, evitando que cada artefato fosse produzido de maneira independente ou desconectada dos demais.