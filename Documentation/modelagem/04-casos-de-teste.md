# 04 — Casos de teste das fatias modeladas

Com base nas três fatias selecionadas na Seção 0, foram elaborados **6 casos de teste**, sendo **2 para cada fatia**. Em cada fatia, foi definido:
- **1 caso de caminho principal (caminho feliz)**;
- **1 caso de fronteira**, envolvendo regra crítica, restrição de acesso ou caminho de erro.

O objetivo desta seção é demonstrar a rastreabilidade entre as histórias de usuário, os modelos produzidos e os comportamentos esperados do sistema.

---

## Fatia 1 — Matrícula do aluno em curso e liberação de acesso ao conteúdo

### Caso de teste 1 — Matrícula realizada com sucesso e acesso liberado

| Campo | Conteúdo |
| --- | --- |
| **ID** | TC-FATIA1-01 |
| **Fatia / História** | Fatia 1 — US-ADM-006, US-ALU-002, US-ALU-003 |
| **Pré-condições** | Aluno ativo e pertencente à instituição; curso disponível para matrícula; aluno ainda não matriculado no curso. |
| **Dados de entrada** | `aluno_id = ALU-001`; `curso_id = CUR-101`; origem da matrícula = `AUTO`. |
| **Passos** | 1) Aluno acessa a lista de cursos disponíveis; 2) Seleciona o curso; 3) Clica em “Matricular”; 4) O sistema valida disponibilidade e registra a matrícula; 5) O aluno tenta acessar o conteúdo do curso. |
| **Resultado esperado** | O sistema cria a matrícula com status ativo, libera o acesso ao curso e exibe os módulos e aulas ao aluno. |
| **Critério de aprovação** | (a) Existe registro de matrícula no banco; (b) a matrícula está com status ativo; (c) o curso aparece na área do aluno; (d) o conteúdo do curso pode ser acessado sem erro de permissão. |
| **Severidade em caso de falha** | **Alta** — impede a entrada do aluno no fluxo principal da plataforma. |

---

### Caso de teste 2 — Aluno sem matrícula ativa tenta acessar conteúdo do curso

| Campo | Conteúdo |
| --- | --- |
| **ID** | TC-FATIA1-02 |
| **Fatia / História** | Fatia 1 — US-ALU-003 |
| **Pré-condições** | Aluno autenticado no sistema; curso existente; aluno sem matrícula ativa no curso. |
| **Dados de entrada** | `aluno_id = ALU-002`; `curso_id = CUR-101`. |
| **Passos** | 1) Aluno acessa diretamente a página do curso; 2) O sistema valida a autorização de acesso; 3) O aluno tenta abrir os módulos e aulas do curso. |
| **Resultado esperado** | O sistema bloqueia o acesso ao conteúdo e exibe mensagem informando que o aluno não possui permissão para acessar aquele curso. |
| **Critério de aprovação** | (a) Nenhum conteúdo do curso é exibido; (b) o sistema retorna acesso negado; (c) não é criada matrícula automaticamente; (d) a mensagem de bloqueio é apresentada ao usuário. |
| **Severidade em caso de falha** | **Crítica** — falha de autorização compromete o controle de acesso do sistema. |

---

## Fatia 2 — Professor cria, organiza e publica o conteúdo do curso

### Caso de teste 3 — Professor cria conteúdo completo e publica com sucesso

| Campo | Conteúdo |
| --- | --- |
| **ID** | TC-FATIA2-01 |
| **Fatia / História** | Fatia 2 — US-PED-001, US-PED-002, US-PED-003, US-PED-005, US-PED-006, US-PED-007 |
| **Pré-condições** | Professor ativo e vinculado ao curso; curso existente e disponível para edição. |
| **Dados de entrada** | `curso_id = CUR-101`; módulo “Introdução”; aula “Visão Geral”; material PDF e vídeo; avaliação com 5 questões; nota mínima = 7,0; tentativas máximas = 2. |
| **Passos** | 1) Professor seleciona o curso; 2) Cria um módulo; 3) Cria uma aula dentro do módulo; 4) Anexa materiais; 5) Cria uma avaliação; 6) Define critérios de aprovação; 7) Solicita a publicação do conteúdo. |
| **Resultado esperado** | O sistema salva o módulo, aula, materiais e avaliação, e publica o conteúdo para visualização pelos alunos matriculados. |
| **Critério de aprovação** | (a) Módulo, aula, material e avaliação estão persistidos; (b) o status de publicação está como publicado; (c) o conteúdo fica visível para alunos com matrícula ativa; (d) os critérios da avaliação foram armazenados corretamente. |
| **Severidade em caso de falha** | **Alta** — compromete a criação e disponibilização do conteúdo pedagógico. |

---

### Caso de teste 4 — Professor tenta publicar conteúdo incompleto

| Campo | Conteúdo |
| --- | --- |
| **ID** | TC-FATIA2-02 |
| **Fatia / História** | Fatia 2 — US-PED-005, US-PED-006, US-PED-007 |
| **Pré-condições** | Professor ativo e vinculado ao curso; avaliação criada em rascunho, mas sem critérios mínimos configurados ou sem questões suficientes. |
| **Dados de entrada** | `curso_id = CUR-101`; avaliação sem nota mínima definida ou com quantidade insuficiente de questões para publicação. |
| **Passos** | 1) Professor acessa a avaliação em rascunho; 2) Solicita a publicação; 3) O sistema valida a integridade mínima do conteúdo. |
| **Resultado esperado** | O sistema impede a publicação, mantém o conteúdo como rascunho e informa ao professor que a avaliação está incompleta. |
| **Critério de aprovação** | (a) O status permanece como rascunho; (b) a avaliação não fica visível aos alunos; (c) o sistema exibe mensagem clara de erro; (d) nenhuma publicação parcial é realizada. |
| **Severidade em caso de falha** | **Alta** — pode expor conteúdo inválido ou incompleto para os alunos. |

---

## Fatia 3 — Aluno realiza avaliação, tem o progresso registrado, recebe nota e obtém certificado

### Caso de teste 5 — Aluno conclui avaliação, é aprovado e obtém certificado

| Campo | Conteúdo |
| --- | --- |
| **ID** | TC-FATIA3-01 |
| **Fatia / História** | Fatia 3 — US-ALU-005, US-ALU-007, US-ALU-008, US-ALU-010 |
| **Pré-condições** | Aluno com matrícula ativa; curso com progresso quase concluído; avaliação disponível; certificado ainda não emitido. |
| **Dados de entrada** | `matricula_id = MAT-201`; `avaliacao_id = AVA-301`; respostas suficientes para nota `8,5`; nota mínima exigida = `7,0`. |
| **Passos** | 1) Aluno inicia a avaliação; 2) Responde às questões; 3) Envia a tentativa; 4) O sistema corrige a avaliação; 5) O sistema atualiza o progresso do curso; 6) O aluno solicita emissão do certificado. |
| **Resultado esperado** | O sistema registra a tentativa como corrigida, atribui nota suficiente para aprovação, atualiza o progresso do curso e libera a emissão do certificado. |
| **Critério de aprovação** | (a) A nota foi registrada corretamente; (b) o aluno foi marcado como aprovado; (c) o progresso do curso foi atualizado; (d) o certificado foi gerado com código de validação e data de emissão. |
| **Severidade em caso de falha** | **Crítica** — afeta avaliação formal, progresso acadêmico e comprovação de conclusão. |

---

### Caso de teste 6 — Falha de conexão durante avaliação com retomada sem perda de respostas

| Campo | Conteúdo |
| --- | --- |
| **ID** | TC-FATIA3-02 |
| **Fatia / História** | Fatia 3 — US-ALU-006 |
| **Pré-condições** | Aluno com matrícula ativa; avaliação iniciada; salvamento automático habilitado; pelo menos parte das respostas já preenchida e salva. |
| **Dados de entrada** | `matricula_id = MAT-201`; `avaliacao_id = AVA-301`; 4 respostas registradas antes da falha; evento de desconexão após último salvamento automático. |
| **Passos** | 1) Aluno inicia a avaliação; 2) Responde parte das questões; 3) O sistema executa salvamento automático; 4) Ocorre falha de conexão; 5) O aluno acessa novamente a avaliação; 6) Solicita retomada da tentativa. |
| **Resultado esperado** | O sistema restaura a tentativa em andamento com as respostas já salvas, permitindo que o aluno continue do ponto em que parou. |
| **Critério de aprovação** | (a) As respostas salvas antes da falha continuam registradas; (b) a tentativa permanece válida; (c) o aluno consegue retomar a avaliação sem reinício completo; (d) não ocorre perda total da tentativa. |
| **Severidade em caso de falha** | **Crítica** — compromete a confiabilidade da avaliação e pode causar perda indevida de progresso do aluno. |

---

## Observação final

Os casos de teste acima não constituem um plano de testes completo, mas sim uma amostra rastreável às três fatias modeladas neste trabalho. Em cada fatia, foi incluído pelo menos um caso de fronteira, contemplando restrição de acesso, tentativa de publicação inválida e falha de conexão durante avaliação.