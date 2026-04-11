# Documento de Requisitos
## Plataforma de Gestão de Aprendizagem (EduFlow LMS)

---

## Introdução

O **EduFlow LMS** é uma plataforma de gestão de aprendizagem voltada para instituições de ensino, empresas e organizações que desejam ofertar cursos online com acompanhamento de progresso, avaliações e gestão centralizada de usuários e conteúdos.

Com base no documento de visão do produto, este documento organiza os requisitos funcionais e não funcionais do sistema, estruturando-os em subsistemas e grupos de usuários.

### Subsistemas identificados

Para fins de levantamento de requisitos, o sistema foi dividido em três subsistemas principais:

1. **Subsistema de Administração Institucional**  
   Responsável pela gestão da instituição, cursos, usuários, permissões e matrículas.

2. **Subsistema de Gestão Pedagógica**  
   Responsável pela criação e manutenção de módulos, aulas, materiais, avaliações e acompanhamento acadêmico.

3. **Subsistema de Aprendizagem do Aluno**  
   Responsável pela experiência do aluno no consumo dos cursos, realização de avaliações, acompanhamento do progresso, fórum e emissão de certificado.

### Grupos de usuários

- **Administrador da Instituição**
- **Professor**
- **Aluno**

---

# 1. Levantamento dos Requisitos Funcionais

## 1.1 Histórias de Usuário

As histórias de usuário a seguir foram organizadas por subsistema, seguindo o formato:

> **Como** [tipo de usuário], **eu quero** [ação], **para que** [benefício].

---

## 1.1.1 Subsistema de Administração Institucional

| ID | História | Critérios de Aceitação | Dependências | Fonte |
| --- | --- | --- | --- | --- |
| **US-ADM-001** | Como **administrador da instituição**, eu quero **criar e configurar a instituição**, para que **o ambiente fique pronto para uso com identidade e dados próprios**. | a. Deve ser possível cadastrar nome, identificação e dados básicos da instituição.<br>b. A instituição deve ser salva com identificador único.<br>c. O sistema deve exibir confirmação de cadastro. | Nenhuma | Administrador da instituição |
| **US-ADM-002** | Como **administrador da instituição**, eu quero **cadastrar professores**, para que **eles possam ser vinculados aos cursos**. | a. Deve ser possível cadastrar nome, e-mail e perfil de professor.<br>b. O sistema deve impedir e-mails duplicados na mesma instituição.<br>c. O professor deve receber acesso compatível com seu papel. | US-ADM-001 | Administrador da instituição |
| **US-ADM-003** | Como **administrador da instituição**, eu quero **cadastrar alunos**, para que **eles possam acessar os cursos e realizar atividades**. | a. Deve ser possível cadastrar nome, e-mail e perfil de aluno.<br>b. O sistema deve impedir duplicidade de cadastro na instituição.<br>c. O aluno deve ser criado com acesso restrito ao seu perfil. | US-ADM-001 | Administrador da instituição |
| **US-ADM-004** | Como **administrador da instituição**, eu quero **criar cursos**, para que **a instituição possa ofertar trilhas de aprendizagem organizadas**. | a. Deve ser possível definir nome, descrição e status do curso.<br>b. O curso deve ficar associado à instituição correta.<br>c. O curso deve ficar disponível para receber módulos, professores e matrículas. | US-ADM-001 | Administrador da instituição |
| **US-ADM-005** | Como **administrador da instituição**, eu quero **atribuir professores aos cursos**, para que **cada curso tenha responsáveis pedagógicos definidos**. | a. Deve ser possível vincular um ou mais professores a um curso.<br>b. Apenas professores da mesma instituição podem ser vinculados.<br>c. O professor passa a visualizar apenas os cursos atribuídos. | US-ADM-002, US-ADM-004 | Administrador da instituição |
| **US-ADM-006** | Como **administrador da instituição**, eu quero **gerenciar matrículas de alunos nos cursos**, para que **os alunos certos tenham acesso aos cursos corretos**. | a. Deve ser possível matricular e remover alunos de cursos.<br>b. Apenas alunos da mesma instituição podem ser matriculados.<br>c. O aluno matriculado deve visualizar o curso em sua área. | US-ADM-003, US-ADM-004 | Administrador da instituição |
| **US-ADM-007** | Como **administrador da instituição**, eu quero **visualizar relatórios de uso da plataforma**, para que **eu acompanhe adesão, progresso e uso dos cursos**. | a. O sistema deve exibir indicadores básicos por curso e por período.<br>b. O sistema deve mostrar quantidade de matrículas, acessos e conclusão.<br>c. Os dados devem refletir apenas a instituição do administrador. | US-ADM-004, US-ADM-006 | Administrador da instituição |
| **US-ADM-008** | Como **administrador da instituição**, eu quero **suspender ou reativar usuários**, para que **eu mantenha o controle de acesso ao ambiente**. | a. Deve ser possível alterar o status de professores e alunos.<br>b. Usuários suspensos não podem autenticar no sistema.<br>c. Usuários reativados voltam a acessar conforme suas permissões anteriores. | US-ADM-002, US-ADM-003 | Administrador da instituição |
| **US-ADM-009** | Como **administrador da instituição**, eu quero **configurar regras institucionais para certificados**, para que **os certificados emitidos reflitam os critérios da organização**. | a. Deve ser possível definir nome exibido da instituição e regra mínima de conclusão.<br>b. As configurações devem ser aplicadas aos certificados emitidos.<br>c. Apenas administradores podem alterar essas regras. | US-ADM-001, US-ADM-004 | Administrador da instituição |
| **US-ADM-010** | Como **administrador da instituição**, eu quero **exportar relatórios de uso**, para que **eu compartilhe resultados com coordenação, direção ou RH**. | a. Deve ser possível exportar relatório em formato estruturado.<br>b. O arquivo deve refletir filtros selecionados.<br>c. Apenas dados da instituição do administrador devem constar na exportação. | US-ADM-007 | Administrador da instituição |

---

## 1.1.2 Subsistema de Gestão Pedagógica

| ID | História | Critérios de Aceitação | Dependências | Fonte |
| --- | --- | --- | --- | --- |
| **US-PED-001** | Como **professor**, eu quero **criar módulos em meus cursos**, para que **o conteúdo fique organizado por temas**. | a. Deve ser possível criar módulo com título e descrição.<br>b. O módulo deve ficar vinculado ao curso correto.<br>c. Apenas professores atribuídos ao curso podem criar módulos. | US-ADM-004, US-ADM-005 | Professor |
| **US-PED-002** | Como **professor**, eu quero **criar aulas dentro dos módulos**, para que **os alunos acessem o conteúdo de forma sequencial**. | a. Deve ser possível criar aula com título e descrição.<br>b. A aula deve ficar vinculada a um módulo existente.<br>c. A aula deve poder ser salva em rascunho ou publicada. | US-PED-001 | Professor |
| **US-PED-003** | Como **professor**, eu quero **fazer upload de vídeos, PDFs e textos**, para que **os alunos tenham acesso aos materiais da aula**. | a. Deve ser possível anexar materiais suportados pelo sistema.<br>b. O material deve ficar associado à aula correta.<br>c. O aluno deve conseguir visualizar ou baixar o material conforme o tipo. | US-PED-002 | Professor |
| **US-PED-004** | Como **professor**, eu quero **reordenar módulos e aulas**, para que **o fluxo pedagógico do curso fique claro e coerente**. | a. Deve ser possível alterar a ordem dos módulos.<br>b. Deve ser possível alterar a ordem das aulas dentro do módulo.<br>c. A nova ordem deve refletir imediatamente para os alunos. | US-PED-001, US-PED-002 | Professor |
| **US-PED-005** | Como **professor**, eu quero **criar avaliações de múltipla escolha**, para que **eu possa verificar a aprendizagem dos alunos**. | a. Deve ser possível cadastrar questões com alternativas.<br>b. Deve ser possível definir resposta correta.<br>c. A avaliação deve ficar vinculada ao curso ou módulo correspondente. | US-PED-001 | Professor |
| **US-PED-006** | Como **professor**, eu quero **definir critérios de avaliação**, para que **o sistema calcule aprovação conforme regras do curso**. | a. Deve ser possível definir nota mínima de aprovação.<br>b. Deve ser possível definir número de tentativas, quando aplicável.<br>c. O sistema deve usar essas regras no cálculo do resultado do aluno. | US-PED-005 | Professor |
| **US-PED-007** | Como **professor**, eu quero **publicar ou despublicar conteúdos**, para que **eu controle quando um material ficará visível para os alunos**. | a. Conteúdos em rascunho não devem aparecer ao aluno.<br>b. Conteúdos publicados devem aparecer apenas para alunos matriculados.<br>c. O professor deve poder alterar o status a qualquer momento. | US-PED-002, US-PED-003 | Professor |
| **US-PED-008** | Como **professor**, eu quero **visualizar o progresso dos alunos**, para que **eu identifique quem está acompanhando ou abandonando o curso**. | a. O sistema deve mostrar percentual de conclusão por aluno.<br>b. O sistema deve indicar aulas concluídas e pendentes.<br>c. O professor deve visualizar apenas os alunos dos seus cursos. | US-ADM-006, US-PED-007 | Professor |
| **US-PED-009** | Como **professor**, eu quero **visualizar notas e desempenho em avaliações**, para que **eu acompanhe resultados individuais e da turma**. | a. O sistema deve exibir nota por aluno e por avaliação.<br>b. O sistema deve permitir visualizar situação de aprovação/reprovação.<br>c. O professor deve visualizar apenas dados dos seus cursos. | US-PED-005, US-PED-006, US-ADM-006 | Professor |
| **US-PED-010** | Como **professor**, eu quero **responder dúvidas no fórum do curso**, para que **eu apoie os alunos durante o processo de aprendizagem**. | a. O professor deve conseguir responder tópicos do fórum.<br>b. As respostas devem ficar visíveis aos alunos matriculados no curso.<br>c. O sistema deve registrar data e autor da resposta. | US-ADM-006 | Professor |

---

## 1.1.3 Subsistema de Aprendizagem do Aluno

| ID | História | Critérios de Aceitação | Dependências | Fonte |
| --- | --- | --- | --- | --- |
| **US-ALU-001** | Como **aluno**, eu quero **visualizar os cursos disponíveis**, para que **eu saiba quais opções de aprendizado a instituição oferece**. | a. O sistema deve listar cursos visíveis para matrícula.<br>b. Cada curso deve exibir nome, descrição e informações básicas.<br>c. O aluno deve visualizar apenas cursos da sua instituição. | US-ADM-004 | Aluno |
| **US-ALU-002** | Como **aluno**, eu quero **me matricular em um curso disponível**, para que **eu possa começar a estudar**. | a. Deve ser possível solicitar ou confirmar matrícula, conforme regra da instituição.<br>b. Após matriculado, o curso deve aparecer na área do aluno.<br>c. O aluno não deve conseguir se matricular duas vezes no mesmo curso. | US-ALU-001 ou US-ADM-006 | Aluno |
| **US-ALU-003** | Como **aluno**, eu quero **acessar aulas e materiais do curso**, para que **eu possa estudar os conteúdos disponibilizados**. | a. O aluno deve visualizar módulos e aulas do curso matriculado.<br>b. O aluno deve conseguir abrir vídeos, textos e PDFs liberados.<br>c. O aluno não deve acessar cursos não matriculados. | US-ALU-002, US-PED-007 | Aluno |
| **US-ALU-004** | Como **aluno**, eu quero **ter meu progresso registrado nas aulas**, para que **eu acompanhe o que já concluí**. | a. O sistema deve registrar conclusão de aula quando o aluno a finalizar ou marcá-la como concluída.<br>b. O progresso deve atualizar o percentual do curso.<br>c. O histórico deve permanecer salvo entre sessões. | US-ALU-003 | Aluno |
| **US-ALU-005** | Como **aluno**, eu quero **realizar avaliações online**, para que **eu possa comprovar meu aprendizado**. | a. O aluno deve conseguir iniciar avaliação disponível.<br>b. O sistema deve exibir perguntas e alternativas de forma clara.<br>c. Ao enviar, a tentativa deve ser registrada. | US-ALU-002, US-PED-005, US-PED-006 | Aluno |
| **US-ALU-006** | Como **aluno**, eu quero **retomar avaliações sem perder respostas em caso de falha**, para que **quedas de conexão não prejudiquem meu desempenho**. | a. O sistema deve salvar respostas automaticamente durante a avaliação.<br>b. Em caso de reconexão, o aluno deve retomar do ponto salvo.<br>c. O sistema deve impedir perda total da tentativa por falha simples de conexão. | US-ALU-005 | Aluno |
| **US-ALU-007** | Como **aluno**, eu quero **visualizar minhas notas e meu resultado nas avaliações**, para que **eu saiba meu desempenho no curso**. | a. O sistema deve exibir nota obtida por avaliação.<br>b. O sistema deve exibir status de aprovado ou reprovado conforme critérios definidos.<br>c. O aluno deve visualizar apenas as próprias notas. | US-ALU-005 | Aluno |
| **US-ALU-008** | Como **aluno**, eu quero **acompanhar meu progresso geral no curso**, para que **eu planeje meus estudos e saiba o que falta concluir**. | a. O sistema deve exibir percentual total de conclusão do curso.<br>b. O sistema deve indicar aulas concluídas, pendentes e avaliações realizadas.<br>c. As informações devem atualizar automaticamente após novas ações do aluno. | US-ALU-004, US-ALU-007 | Aluno |
| **US-ALU-009** | Como **aluno**, eu quero **fazer perguntas no fórum do curso**, para que **eu possa tirar dúvidas com professores e colegas**. | a. O aluno deve conseguir criar tópicos e responder discussões.<br>b. Apenas participantes do curso podem interagir no fórum correspondente.<br>c. O sistema deve registrar data e autor da mensagem. | US-ALU-002 | Aluno |
| **US-ALU-010** | Como **aluno**, eu quero **emitir meu certificado ao concluir o curso**, para que **eu tenha um comprovante formal da conclusão**. | a. O certificado deve ser liberado apenas após cumprimento dos critérios definidos.<br>b. O documento deve conter dados do aluno, curso e instituição.<br>c. O aluno deve conseguir visualizar ou baixar o certificado. | US-ALU-008, US-ALU-007, US-ADM-009 | Aluno |

---

## 1.2 Estimativa de Tamanho

### Metodologia escolhida: Story Points

Para este projeto, foi utilizada a técnica de **Story Points** com escala de Fibonacci (**1, 2, 3, 5, 8, 13, 21**).

### Justificativa da escolha

A metodologia de Story Points foi escolhida porque o projeto ainda está em fase de definição de requisitos e, nesse contexto, é mais adequado estimar **complexidade relativa, esforço e incerteza** do que tempo exato em horas. Essa abordagem ajuda o grupo a:

- comparar histórias entre si;
- identificar histórias muito grandes que ainda precisam ser quebradas;
- priorizar melhor o backlog;
- considerar não apenas implementação, mas também validação, regras de negócio e integrações.

### Critério adotado pelo grupo

| Story Points | Interpretação usada no projeto |
| --- | --- |
| **1** | Ajuste muito simples, baixo esforço, sem regras complexas |
| **2** | Funcionalidade pequena com regra simples |
| **3** | CRUD ou fluxo simples com validações básicas |
| **5** | Funcionalidade média com múltiplas regras, permissões ou persistência relevante |
| **8** | Funcionalidade complexa com integração, recuperação de estado, exportação ou maior risco técnico |
| **13+** | História grande demais para o nível atual e que deveria ser quebrada |

### Base prática de estimativa

O grupo adotou como referência:

- **3 pontos** para operações simples de cadastro e visualização;
- **5 pontos** para fluxos com regras de negócio mais relevantes;
- **8 pontos** para fluxos com maior risco técnico, como exportações e retomada de avaliação com salvamento automático.

---

## 1.3 Priorização com MoSCoW

A priorização abaixo considera o valor para o negócio, dependência entre funcionalidades e impacto direto na operação mínima viável do EduFlow LMS.

| ID | Subsistema | Resumo da História | Story Points | MoSCoW | Justificativa da Priorização |
| --- | --- | --- | --- | --- | --- |
| **US-ADM-001** | Administração Institucional | Criar e configurar instituição | 3 | **M** | Base estrutural do sistema multi-instituição. |
| **US-ADM-002** | Administração Institucional | Cadastrar professores | 3 | **M** | Necessário para habilitar o subsistema pedagógico. |
| **US-ADM-003** | Administração Institucional | Cadastrar alunos | 3 | **M** | Necessário para permitir acesso e matrículas. |
| **US-ADM-004** | Administração Institucional | Criar cursos | 5 | **M** | Sem cursos, a plataforma não entrega valor principal. |
| **US-ADM-005** | Administração Institucional | Atribuir professores aos cursos | 3 | **M** | Garante governança e controle de acesso pedagógico. |
| **US-ADM-006** | Administração Institucional | Gerenciar matrículas | 5 | **M** | Essencial para controlar acesso dos alunos aos cursos. |
| **US-ADM-007** | Administração Institucional | Visualizar relatórios de uso | 5 | **S** | Importante para gestão, mas não bloqueia o uso inicial da plataforma. |
| **US-ADM-008** | Administração Institucional | Suspender ou reativar usuários | 3 | **S** | Relevante para controle operacional e segurança, mas pode vir após o núcleo do sistema. |
| **US-ADM-009** | Administração Institucional | Configurar regras institucionais de certificado | 5 | **C** | Agrega personalização institucional, mas não impede operação básica do LMS. |
| **US-ADM-010** | Administração Institucional | Exportar relatórios | 8 | **C** | Gera valor gerencial adicional, porém pode ser entregue em etapa posterior. |
| **US-PED-001** | Gestão Pedagógica | Criar módulos | 3 | **M** | Estrutura básica do conteúdo pedagógico. |
| **US-PED-002** | Gestão Pedagógica | Criar aulas | 3 | **M** | Essencial para publicação do conteúdo do curso. |
| **US-PED-003** | Gestão Pedagógica | Upload de materiais | 5 | **M** | Núcleo da proposta de ensino online. |
| **US-PED-004** | Gestão Pedagógica | Reordenar módulos e aulas | 3 | **S** | Melhora a organização do curso, mas não impede a publicação inicial. |
| **US-PED-005** | Gestão Pedagógica | Criar avaliações | 5 | **M** | Faz parte do núcleo funcional descrito no estudo de caso. |
| **US-PED-006** | Gestão Pedagógica | Definir critérios de avaliação | 3 | **M** | Necessário para cálculo correto de aprovação. |
| **US-PED-007** | Gestão Pedagógica | Publicar e despublicar conteúdo | 2 | **M** | Essencial para controlar visibilidade do material. |
| **US-PED-008** | Gestão Pedagógica | Visualizar progresso dos alunos | 5 | **S** | Muito relevante para acompanhamento, mas pode vir após o fluxo principal de ensino. |
| **US-PED-009** | Gestão Pedagógica | Visualizar notas e desempenho | 5 | **S** | Complementa a gestão pedagógica, sem bloquear a entrega mínima. |
| **US-PED-010** | Gestão Pedagógica | Responder dúvidas no fórum | 3 | **S** | Importante para suporte ao aluno, mas o curso pode existir sem essa etapa inicial. |
| **US-ALU-001** | Aprendizagem do Aluno | Visualizar cursos disponíveis | 3 | **M** | Ponto de entrada para descoberta dos cursos. |
| **US-ALU-002** | Aprendizagem do Aluno | Matricular-se em curso | 3 | **M** | Essencial para iniciar a jornada do aluno. |
| **US-ALU-003** | Aprendizagem do Aluno | Acessar aulas e materiais | 3 | **M** | Núcleo da experiência de aprendizagem. |
| **US-ALU-004** | Aprendizagem do Aluno | Registrar progresso nas aulas | 5 | **M** | Essencial para acompanhamento e para o valor da plataforma. |
| **US-ALU-005** | Aprendizagem do Aluno | Realizar avaliações online | 5 | **M** | Requisito central do sistema. |
| **US-ALU-006** | Aprendizagem do Aluno | Retomar avaliação sem perder respostas | 8 | **M** | Mitiga um risco técnico crítico já identificado no estudo de caso. |
| **US-ALU-007** | Aprendizagem do Aluno | Visualizar notas e resultado | 3 | **M** | Necessário para fechar o ciclo de avaliação. |
| **US-ALU-008** | Aprendizagem do Aluno | Acompanhar progresso geral do curso | 3 | **S** | Importante para autonomia do aluno, mas pode vir logo após o fluxo central. |
| **US-ALU-009** | Aprendizagem do Aluno | Participar do fórum | 3 | **S** | Enriquece a experiência, porém não bloqueia estudo e avaliação. |
| **US-ALU-010** | Aprendizagem do Aluno | Emitir certificado | 3 | **M** | Faz parte do escopo funcional explicitado no estudo de caso. |

### Resumo por prioridade

| Prioridade | Histórias |
| --- | --- |
| **Must Have** | US-ADM-001, US-ADM-002, US-ADM-003, US-ADM-004, US-ADM-005, US-ADM-006, US-PED-001, US-PED-002, US-PED-003, US-PED-005, US-PED-006, US-PED-007, US-ALU-001, US-ALU-002, US-ALU-003, US-ALU-004, US-ALU-005, US-ALU-006, US-ALU-007, US-ALU-010 |
| **Should Have** | US-ADM-007, US-ADM-008, US-PED-004, US-PED-008, US-PED-009, US-PED-010, US-ALU-008, US-ALU-009 |
| **Could Have** | US-ADM-009, US-ADM-010 |
| **Won’t Have (por agora)** | Nenhuma história foi classificada nessa categoria nesta versão, pois todas as demandas levantadas ainda permanecem aderentes ao escopo previsto. |

---

# 2. Levantamento de Requisitos Não Funcionais

## 2.1 Mapeamento de Requisitos Não Funcionais

Os requisitos não funcionais do EduFlow LMS foram definidos considerando atributos de qualidade esperados para um sistema web educacional multiusuário, com base em princípios de qualidade de software e documentação de requisitos.

As categorias priorizadas para o projeto foram:

- **Desempenho**
- **Escalabilidade**
- **Segurança**
- **Disponibilidade**
- **Usabilidade**
- **Portabilidade**
- **Manutenibilidade**
- **Qualidade de desenvolvimento**
- **Proteção de dados**

### Visão geral por categoria

| Categoria | Foco no EduFlow |
| --- | --- |
| **Desempenho** | Responder rapidamente a ações comuns, como login, abertura de cursos, envio de avaliações e navegação entre aulas |
| **Escalabilidade** | Suportar crescimento de cursos, matrículas e acessos em horários de pico |
| **Segurança** | Garantir autenticação, autorização por perfil e isolamento entre instituições |
| **Disponibilidade** | Manter o sistema acessível na maior parte do tempo, especialmente em horários de aula |
| **Usabilidade** | Facilitar uso por administradores, professores e alunos com diferentes níveis técnicos |
| **Portabilidade** | Funcionamento adequado em navegadores modernos e dispositivos móveis |
| **Manutenibilidade** | Facilitar evolução do sistema sem comprometer a operação |
| **Qualidade de desenvolvimento** | Padronização de código, testes, integração contínua e documentação técnica |
| **Proteção de dados** | Tratar dados pessoais com segurança e aderência à LGPD |

---

## 2.2 Requisitos Diferenciados por Tipo de Usuário

Os requisitos não funcionais variam conforme o perfil e o subsistema utilizado.

| Tipo de Usuário / Subsistema | Requisitos Não Funcionais mais críticos |
| --- | --- |
| **Administrador / Administração Institucional** | Segurança reforçada, auditoria, isolamento por instituição, tempo aceitável para relatórios |
| **Professor / Gestão Pedagógica** | Facilidade de uso, organização de conteúdo, estabilidade em upload de materiais, resposta consistente em dashboards acadêmicos |
| **Aluno / Aprendizagem** | Rapidez no acesso às aulas, responsividade, acessibilidade, salvamento automático de avaliações, disponibilidade elevada em horários de estudo |

### Diferenças principais

#### Administrador
- Exige maior rigor em **segurança**, **controle de permissões** e **auditoria**.
- Pode aceitar tempos um pouco maiores em relatórios mais pesados, desde que sejam previsíveis e estáveis.

#### Professor
- Precisa de **boa usabilidade** para organizar conteúdos sem alta complexidade técnica.
- Depende de **uploads confiáveis** e visualização clara de progresso e notas.

#### Aluno
- Exige **experiência fluida**, **responsiva** e com **baixo risco de perda de progresso**.
- Depende de alta disponibilidade em momentos de aula e avaliação.

---

## 2.3 Justificativa Baseada nos Aprendizados

Os requisitos não funcionais foram definidos com base em boas práticas de Engenharia de Software, especialmente:

- **ISO/IEC 25010**, para características de qualidade como desempenho, usabilidade, confiabilidade, segurança, compatibilidade, manutenibilidade e portabilidade;
- **IEEE-830**, como referência de estruturação e clareza na documentação dos requisitos;
- **ISO/IEC/IEEE 29148**, para organização e qualidade na especificação de requisitos;
- princípios de arquitetura de software discutidos em aula, principalmente separação de responsabilidades, escalabilidade e redução de riscos operacionais.

No contexto do EduFlow LMS, isso significa transformar qualidades abstratas, como “ser rápido” ou “ser seguro”, em requisitos mensuráveis, verificáveis e vinculados aos perfis reais de uso do sistema.

---

## 2.4 Requisitos Não Funcionais no formato IEEE-830

| Requisito ID | Título | Descrição | Entrada | Processamento | Saída | Restrições | Critérios de Aceitação |
| --- | --- | --- | --- | --- | --- | --- | --- |
| **NF-GER-001** | Tempo de Resposta Geral | O sistema deve responder às operações comuns de navegação, login, abertura de curso e carregamento de páginas em até **2 segundos** para pelo menos **95%** das requisições. | Solicitação web de administrador, professor ou aluno | O sistema processa autenticação, consulta de dados e renderização da interface | Página ou resultado exibido ao usuário | Relatórios analíticos pesados ficam fora desse limite e podem levar até 5 segundos | a. 95% das requisições comuns respondem em até 2 segundos.<br>b. Relatórios simples respondem em até 5 segundos.<br>c. Não deve haver travamentos perceptíveis em navegação comum. |
| **NF-GER-002** | Disponibilidade do Serviço | O sistema deve manter disponibilidade mensal mínima de **99,5%**, exceto janelas programadas de manutenção previamente comunicadas. | Acesso dos usuários ao sistema | Infraestrutura monitora saúde da aplicação, banco e integrações essenciais | Sistema acessível durante a maior parte do mês | Manutenções programadas devem ser comunicadas com antecedência | a. Uptime mensal igual ou superior a 99,5%.<br>b. Manutenções planejadas registradas e comunicadas.<br>c. Incidentes críticos devem gerar registro e acompanhamento. |
| **NF-GER-003** | Escalabilidade Operacional | O sistema deve suportar o volume previsto de **500 cursos**, **10.000 matrículas** e pelo menos **300 usuários autenticados simultaneamente**, sem degradação severa da experiência. | Pico de acessos simultâneos | Balanceamento da aplicação, otimização de consultas e uso de serviço externo para streaming | Continuidade do uso com desempenho aceitável | Vídeos devem ser servidos por integração externa especializada | a. O sistema suporta 300 usuários simultâneos com erros inferiores a 2%.<br>b. O tempo de resposta não deve aumentar mais que 20% em relação ao cenário normal.<br>c. O streaming não deve depender do servidor principal do LMS. |
| **NF-GER-004** | Autenticação, Autorização e Isolamento Institucional | O sistema deve implementar autenticação por login e senha, controle de acesso por perfil e isolamento lógico dos dados por instituição. | Tentativa de login ou acesso a recurso protegido | O sistema valida credenciais, perfil do usuário, vínculo institucional e permissões do recurso | Acesso concedido ou negado | Cada usuário deve acessar apenas os dados autorizados ao seu perfil e instituição | a. Aluno não acessa cursos não matriculados.<br>b. Professor não acessa cursos não atribuídos.<br>c. Administrador acessa somente sua instituição.<br>d. Senhas devem ser armazenadas com hash seguro. |
| **NF-GER-005** | Proteção de Dados Pessoais | O sistema deve proteger dados pessoais dos usuários em conformidade com princípios da LGPD, utilizando minimização de coleta, tráfego seguro e controle de exposição das informações. | Cadastro, login, atualização de perfil e uso do sistema | O sistema valida permissões, protege transporte de dados e restringe campos sensíveis | Dados visíveis apenas para usuários autorizados | O sistema não deve expor dados pessoais sem necessidade de negócio | a. Todas as comunicações autenticadas devem usar HTTPS.<br>b. Campos sensíveis não devem aparecer em logs públicos.<br>c. Usuários visualizam apenas dados compatíveis com seu papel. |
| **NF-ADM-001** | Auditoria de Ações Administrativas | O sistema deve registrar ações críticas realizadas por administradores, como criação de curso, matrícula, suspensão de usuário e alteração de permissões. | Ação administrativa sobre entidades do sistema | O sistema registra usuário, data, ação executada e entidade afetada | Log de auditoria disponível para consulta autorizada | Aplica-se apenas a ações administrativas críticas | a. Toda ação crítica gera registro auditável.<br>b. Cada log deve conter autor, data/hora e operação.<br>c. Logs devem ser mantidos por no mínimo 180 dias. |
| **NF-PED-001** | Upload e Disponibilização de Materiais | O sistema deve tratar uploads de materiais de forma confiável, com processamento assíncrono quando necessário e feedback claro ao professor. | Upload de vídeo, PDF ou material textual | O sistema valida tipo, envia para armazenamento apropriado e vincula à aula | Material disponível ou status de processamento exibido | Vídeos podem depender de processamento externo antes da liberação final | a. PDF deve ficar disponível em até 10 segundos após envio bem-sucedido.<br>b. Vídeos devem exibir status de processamento.<br>c. Falhas de upload devem gerar mensagem clara ao usuário. |
| **NF-PED-002** | Usabilidade para Organização de Conteúdo | O subsistema do professor deve permitir criar e organizar módulos, aulas e materiais de forma intuitiva, com baixa curva de aprendizado. | Ação do professor ao estruturar o curso | O sistema apresenta interface simples, consistente e com fluxo claro | Conteúdo criado e organizado com baixo esforço operacional | Aplica-se ao painel do professor | a. Professor deve conseguir criar módulo e aula em no máximo 5 passos.<br>b. Reordenação de conteúdo deve ocorrer sem necessidade de configuração técnica.<br>c. Em teste de usabilidade, ao menos 80% dos usuários devem concluir a tarefa sem ajuda externa. |
| **NF-ALU-001** | Salvamento Automático de Avaliações | O sistema deve salvar automaticamente as respostas do aluno durante a avaliação para reduzir perda de progresso em caso de falha de conexão ou navegador. | Alteração de resposta ou passagem de tempo durante a avaliação | O sistema grava rascunho automaticamente no intervalo definido | Respostas recuperáveis após reconexão ou retorno | Aplica-se a avaliações online do aluno | a. Respostas devem ser salvas a cada alteração relevante ou no máximo a cada 30 segundos.<br>b. Após queda simples de conexão, o aluno deve retomar a avaliação com as respostas preservadas.<br>c. O sistema deve informar quando o salvamento foi concluído. |
| **NF-ALU-002** | Acessibilidade e Responsividade | O sistema deve funcionar adequadamente em desktop e mobile web, com interface responsiva e requisitos mínimos de acessibilidade. | Acesso do aluno, professor ou administrador em diferentes dispositivos | O sistema adapta layout, navegação e contraste conforme a interface | Experiência consistente em múltiplas resoluções e navegadores | Aplica-se à versão web do sistema | a. Interface utilizável em largura mínima de 360px.<br>b. Navegação principal deve ser possível por teclado.<br>c. Contraste mínimo de texto e elementos principais deve ser adequado para leitura. |
| **NF-GER-006** | Backup e Recuperação | O sistema deve manter rotina de backup e capacidade de recuperação para minimizar perda de dados operacionais. | Execução programada de backup ou incidente de perda | O sistema ou infraestrutura realiza backup e restauração dos dados | Dados restaurados após incidente | Estratégia deve contemplar banco de dados e metadados essenciais | a. Backup completo diário executado com sucesso.<br>b. RPO máximo de 24 horas.<br>c. RTO máximo de 4 horas para ambiente principal. |
| **NF-GER-007** | Manutenibilidade e Qualidade de Desenvolvimento | O projeto deve seguir padrões de código, revisão técnica, testes automatizados e documentação mínima para facilitar evolução do sistema. | Alteração de código-fonte ou entrega de nova versão | Pipeline valida qualidade, executa testes e verifica integração | Versão consistente e mais segura para implantação | Aplica-se ao processo de desenvolvimento do sistema | a. Todo merge para a branch principal deve passar por revisão de código.<br>b. Testes automatizados devem cobrir pelo menos 70% da camada de serviço.<br>c. A API e componentes principais devem possuir documentação técnica atualizada. |

---

## Considerações finais

O conjunto de requisitos funcionais e não funcionais documentado para o EduFlow LMS busca garantir que o sistema:

- atenda aos fluxos essenciais de administração, ensino e aprendizagem;
- seja viável como MVP e também como base para evolução futura;
- tenha critérios objetivos de validação;
- reduza riscos já identificados no estudo de caso, especialmente em streaming de vídeos, perda de progresso e controle de acesso.

---

## Referências

AGILE ALLIANCE. *User Stories*. Material de referência sobre histórias de usuário e sua aplicação em projetos ágeis.

IEEE. *IEEE 830-1998 - Recommended Practice for Software Requirements Specifications*. Referência para documentação de especificação de requisitos de software.

ISO/IEC/IEEE. *29148 - Systems and software engineering — Life cycle processes — Requirements engineering*. Referência para engenharia e especificação de requisitos.

ISO/IEC. *25010 - Systems and software Quality Requirements and Evaluation (SQuaRE) — System and software quality models*. Referência para atributos de qualidade de software.

SOMMERVILLE, Ian. *Engenharia de Software*. Obra de referência para levantamento, especificação e validação de requisitos.
