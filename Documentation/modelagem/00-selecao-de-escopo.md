# 00 — Seleção de escopo

Antes de iniciarmos os diagramas, foi definido um recorte para a modelagem do EduFlow LMS. Esse recorte foi escolhido para concentrar a análise nos fluxos mais relevantes do produto, cobrindo o núcleo da experiência da plataforma e permitindo explorar interação entre atores, regras de negócio e estrutura do domínio.

## Subsistemas e atores identificados (recapitulação do Trabalho 1 e 2)

A plataforma EduFlow LMS foi organizada, nos trabalhos anteriores, em três subsistemas principais:

| Subsistema | Atores principais |
| --- | --- |
| **Administração Institucional** | Administrador da instituição |
| **Gestão Pedagógica** | Professor |
| **Aprendizagem do Aluno** | Aluno |

## Fatias selecionadas

### Fatia 1 — Matrícula do aluno em curso e liberação de acesso ao conteúdo

**Histórias cobertas:**  
US-ADM-006 (“Como administrador da instituição, eu quero gerenciar matrículas de alunos nos cursos, para que os alunos certos tenham acesso aos cursos corretos”),  
US-ALU-001 (“Como aluno, eu quero visualizar os cursos disponíveis, para que eu saiba quais opções de aprendizado a instituição oferece”),  
US-ALU-002 (“Como aluno, eu quero me matricular em um curso disponível, para que eu possa começar a estudar”),  
US-ALU-003 (“Como aluno, eu quero acessar aulas e materiais do curso, para que eu possa estudar os conteúdos disponibilizados”).

**Por que é representativa:**
- É uma fatia **Must Have**, pois sem matrícula e liberação de acesso o EduFlow não entrega sua função principal de ensino online.
- Envolve **mais de um tipo de usuário e mais de um subsistema**, conectando a gestão administrativa da matrícula com a experiência do aluno no consumo do curso.
- Tem uma regra importante de domínio: somente alunos devidamente matriculados podem acessar os conteúdos do curso.
- Permite observar variações do fluxo, já que a matrícula pode ser feita diretamente pelo aluno ou gerenciada pelo administrador, conforme a política da instituição.

**O que esperamos aprender:**
- Como modelar vínculos entre aluno, curso e matrícula.
- Como representar regras de autorização e acesso liberado após a matrícula.
- Como modelar um fluxo que atravessa dois contextos do sistema: administração e aprendizagem.

---

### Fatia 2 — Professor cria, organiza e publica o conteúdo do curso

**Histórias cobertas:**  
US-PED-001 (“Como professor, eu quero criar módulos em meus cursos, para que o conteúdo fique organizado por temas”),  
US-PED-002 (“Como professor, eu quero criar aulas dentro dos módulos, para que os alunos acessem o conteúdo de forma sequencial”),  
US-PED-003 (“Como professor, eu quero fazer upload de vídeos, PDFs e textos, para que os alunos tenham acesso aos materiais da aula”),  
US-PED-004 (“Como professor, eu quero reordenar módulos e aulas, para que o fluxo pedagógico do curso fique claro e coerente”),  
US-PED-005 (“Como professor, eu quero criar avaliações de múltipla escolha, para que eu possa verificar a aprendizagem dos alunos”),  
US-PED-006 (“Como professor, eu quero definir critérios de avaliação, para que o sistema calcule aprovação conforme regras do curso”),  
US-PED-007 (“Como professor, eu quero publicar ou despublicar conteúdos, para que eu controle quando um material ficará visível para os alunos”).

**Por que é representativa:**
- Também é uma fatia **Must Have**, pois o valor do EduFlow depende diretamente da existência de cursos estruturados e conteúdos publicados.
- Representa o núcleo da **gestão pedagógica**, indo além de simples cadastro: envolve organização hierárquica, ordenação de conteúdo, criação de avaliações e definição de critérios.
- Possui regras relevantes, como conteúdo em rascunho não ficar visível ao aluno e apenas professores vinculados ao curso poderem alterá-lo.
- Expõe a estrutura central do domínio educacional: curso, módulo, aula, material e avaliação.

**O que esperamos aprender:**
- Como representar composição e hierarquia entre os elementos pedagógicos do sistema.
- Como modelar estados de publicação de conteúdo.
- Como relacionar estrutura estática do curso com regras que afetam a experiência do aluno.

---

### Fatia 3 — Aluno realiza avaliação, tem o progresso registrado, recebe nota e obtém certificado

**Histórias cobertas:**  
US-ALU-004 (“Como aluno, eu quero ter meu progresso registrado nas aulas, para que eu acompanhe o que já concluí”),  
US-ALU-005 (“Como aluno, eu quero realizar avaliações online, para que eu possa comprovar meu aprendizado”),  
US-ALU-006 (“Como aluno, eu quero retomar avaliações sem perder respostas em caso de falha, para que quedas de conexão não prejudiquem meu desempenho”),  
US-ALU-007 (“Como aluno, eu quero visualizar minhas notas e meu resultado nas avaliações, para que eu saiba meu desempenho no curso”),  
US-ALU-008 (“Como aluno, eu quero acompanhar meu progresso geral no curso, para que eu planeje meus estudos e saiba o que falta concluir”),  
US-ALU-010 (“Como aluno, eu quero emitir meu certificado ao concluir o curso, para que eu tenha um comprovante formal da conclusão”).

**Por que é representativa:**
- É uma fatia **Must Have**, pois avaliação, progresso e certificação fazem parte do núcleo funcional definido para o EduFlow.
- É a fatia com **regras de negócio mais não triviais** dentro do recorte: salvamento automático da tentativa, cálculo de nota, verificação de aprovação, atualização de progresso e liberação do certificado.
- Tem caminhos de exceção importantes, como falha de conexão durante a avaliação e retomada sem perda de respostas.
- Fecha o ciclo de valor para o aluno: estudar, ser avaliado, acompanhar desempenho e comprovar conclusão.

**O que esperamos aprender:**
- Como modelar estados e transições no ciclo de realização de avaliação.
- Como representar regras de decisão para aprovação e emissão de certificado.
- Como conectar tentativa de avaliação, nota, progresso e conclusão do curso dentro de um mesmo fluxo.

## Cobertura dos critérios

| Critério | Fatia 1 | Fatia 2 | Fatia 3 |
| --- | --- | --- | --- |
| Must Have do MoSCoW | ✅ Sim | ✅ Sim | ✅ Sim |
| Múltiplos subsistemas/atores | ✅ Administração + Aluno | ⚪ Foco em 1 subsistema | ⚪ Foco em 1 ator principal |
| Regras de negócio não-triviais | ✅ Regras de matrícula e liberação de acesso | ✅ Publicação, visibilidade e critérios de avaliação | ✅ Salvamento automático, cálculo de nota, progresso e certificação |

Os três critérios estão cobertos. Cada fatia traz uma dimensão diferente do sistema: a Fatia 1 enfatiza interação entre perfis e controle de acesso; a Fatia 2 enfatiza a estrutura pedagógica do domínio; e a Fatia 3 concentra a maior complexidade de regras de negócio.

## O que fica de fora (e por quê)

As seguintes histórias do Trabalho 2 **não serão modeladas** neste trabalho:

- **Cadastro e configuração institucional** (US-ADM-001, US-ADM-002, US-ADM-003, US-ADM-004, US-ADM-005): conjunto de fluxos administrativos de criação e cadastro, importantes para operação do sistema, mas predominantemente orientados a configuração inicial e CRUD. Modelar tudo isso não traria tanto aprendizado de domínio quanto as fatias escolhidas.
- **Painel administrativo e relatórios da instituição** (US-ADM-007, US-ADM-008, US-ADM-009, US-ADM-010): funcionalidades relevantes de gestão, como relatórios, suspensão de usuários, configuração institucional de certificados e exportações, mas mais operacionais do que centrais para o fluxo principal de ensino-aprendizagem.
- **Acompanhamento pedagógico pelo professor** (US-PED-008, US-PED-009, US-PED-010): visualização de progresso, consulta de notas da turma e interação no fórum. São funcionalidades importantes em uso real, mas derivam de estruturas e dados já cobertos parcialmente pelas fatias de publicação de conteúdo e avaliação do aluno.
- **Participação do aluno no fórum** (US-ALU-009): funcionalidade útil para comunicação e suporte, mas que introduziria um novo núcleo de modelagem focado em interação social, desviando o foco do recorte principal escolhido pelo grupo.

A Fatia 3 modela a obtenção do certificado pelo aluno; a configuração institucional do certificado ficará fora da modelagem aprofundada, mas segue o **mesmo contexto administrativo** já definido no Trabalho 2.