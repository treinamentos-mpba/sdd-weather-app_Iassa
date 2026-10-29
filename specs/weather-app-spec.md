# Weather App — Especificação de Produto

## Overview

- **Fonte:** [Discovery: Aplicação de Previsão do Tempo](discovery.md).
- **Status:** proposta de especificação para validação; as decisões confirmadas abaixo são vinculantes. As Open Questions não representam decisões aprovadas.
- **Produto:** aplicação para buscar cidades, consultar o clima atual e visualizar a previsão para hoje e os quatro dias seguintes.
- **Objetivo:** apoiar decisões cotidianas e o planejamento de viagens com informações meteorológicas claras, inclusive em dispositivos móveis.
- **Público inicial, ainda a validar:** pessoas que consultam o tempo para planejar o dia e pessoas que consultam o destino antes de viajar.
- **Fluxo principal:** buscar uma cidade, identificar a localidade desejada, selecioná-la, consultar o clima atual e a previsão e, opcionalmente, alternar a unidade de temperatura.

### Decisões confirmadas

| Tema | Definição |
|---|---|
| Fonte de dados | Open-Meteo, sem API key. |
| Intervalo da previsão | Hoje + quatro dias seguintes, totalizando cinco datas consecutivas. |
| Unidade padrão | Celsius, com opção de Fahrenheit. |
| Acesso | Sem autenticação. |
| Armazenamento no servidor | Sem persistência de dados da aplicação no servidor. |
| Idioma da interface | pt-BR. Isso não restringe a cobertura geográfica ao Brasil. |

### Limites desta versão

- Os requisitos abaixo detalham as capacidades do discovery e os estados necessários para seu uso. Os refinamentos de interação são propostas sujeitas à validação do produto.
- Não se define arquitetura, implementação ou plano de tarefas neste documento.
- Critérios com dependência explícita de uma Open Question não autorizam escolher silenciosamente uma solução; sua validação integral exige fechar a questão indicada.

## Functional Requirements

| ID | Requisito | Origem | História | Critérios |
|---|---|---|---|---|
| RF-01 | Permitir buscar cidades por nome, apresentar resultados selecionáveis e consultar a cidade escolhida. Não selecionar arbitrariamente uma cidade homônima. | RF-01 do discovery; risco de homônimos | US-01 | CA-01 a CA-04 |
| RF-02 | Exibir a temperatura e as condições meteorológicas atuais, identificando a cidade à qual pertencem. | RF-02 do discovery | US-02 | CA-05 e CA-06 |
| RF-03 | Exibir a previsão para hoje e os quatro dias seguintes, com identificação das datas e sem inventar dados ausentes. | RF-03 e decisão de intervalo | US-03 | CA-07 e CA-08 |
| RF-04 | Iniciar em Celsius quando não houver preferência local previamente aplicada e permitir alternar entre Celsius e Fahrenheit em todas as temperaturas exibidas. A aplicação de uma preferência local depende de OQ-05. | RF-04 e decisão de unidade | US-04 | CA-09 a CA-11 |
| RF-05 | Disponibilizar busca, seleção, clima atual, previsão e alternância de unidade em dispositivos móveis. | RF-05 do discovery | US-05 | CA-12 e CA-13 |
| RF-06 | Distinguir os estados inicial, carregando, vazio, sucesso, dados incompletos e erro; permitir repetir uma operação que falhou. | Riscos de rede e indisponibilidade; refinamento proposto | US-06 | CA-14 a CA-18 |
| RF-07 | Apresentar os textos da interface, controles, estados e mensagens em pt-BR, com datas e unidades identificáveis. | Decisão de idioma | US-07 | CA-19 e CA-20 |
| RF-08 | Permitir o fluxo principal sem conta, login ou API key fornecida pelo usuário e sem persistência de servidor. | Decisões de fonte, acesso e armazenamento | US-08 | CA-21 e CA-22 |

## User Stories

As histórias usam as duas personas iniciais do discovery, ainda pendentes de validação.

- **US-01 / RF-01:** Como pessoa que verifica a previsão de uma cidade antes de viajar, quero buscar e selecionar a cidade de destino para consultar informações da localidade correta.
- **US-02 / RF-02:** Como pessoa que consulta o tempo para decidir como se vestir ou planejar o dia, quero consultar a temperatura e as condições atuais da minha cidade para escolher uma roupa adequada antes de sair.
- **US-03 / RF-03:** Como pessoa que verifica a previsão de uma cidade antes de viajar, quero consultar a previsão do destino para hoje e os quatro dias seguintes para organizar a mala e os passeios nesse período.
- **US-04 / RF-04:** Como pessoa que verifica a previsão de uma cidade antes de viajar, quero alternar as temperaturas entre Celsius e Fahrenheit para avaliar o clima do destino na unidade que compreendo melhor.
- **US-05 / RF-05:** Como pessoa que consulta o tempo para decidir como se vestir ou planejar o dia, quero buscar minha cidade, consultar o clima atual e a previsão e alternar a unidade pelo celular para planejar minhas atividades sem depender de um desktop.
- **US-06 / RF-06:** Como pessoa que consulta o tempo para decidir como se vestir ou planejar o dia, quero identificar se a consulta está carregando, sem resultados ou com falha e repetir uma operação que falhou para obter as informações necessárias antes de sair.
- **US-07 / RF-07:** Como pessoa que consulta o tempo para decidir como se vestir ou planejar o dia, quero ler controles, datas, unidades e mensagens em pt-BR para compreender as informações e decidir como me preparar.
- **US-08 / RF-08:** Como pessoa que verifica a previsão de uma cidade antes de viajar, quero consultar o tempo sem criar conta nem informar credenciais para planejar a viagem sem etapas de cadastro ou login.

## Acceptance Criteria

Os cenários de dados abaixo devem ser verificáveis com respostas conhecidas, sem depender de uma condição meteorológica real ocorrer. Ausência de dados não equivale a temperatura zero.

### US-01 — Buscar e selecionar cidade

- **CA-01:** Dado um nome com correspondência na fonte, quando a busca terminar com sucesso, então apresentar as cidades retornadas e permitir selecionar uma delas.
- **CA-02:** Dado que há cidades homônimas nos resultados, quando forem exibidas, então não selecionar automaticamente uma delas e fornecer contexto geográfico para diferenciá-las. O formato e o tratamento de contexto ausente dependem de OQ-04.
- **CA-03:** Dado que a consulta está vazia ou contém apenas espaços, quando a pessoa tentar buscar, então não iniciar uma consulta de cidade e indicar que é necessário informar um nome.
- **CA-04:** Dada uma cidade selecionada, quando a consulta meteorológica concluir, então o nome e os dados exibidos devem corresponder à seleção, não a outro resultado da busca.

### US-02 — Consultar clima atual

- **CA-05:** Dada uma resposta completa para a cidade selecionada, quando o clima atual for apresentado, então mostrar sua temperatura com unidade, a descrição compreensível das condições e a identificação da cidade, consistentes com os dados recebidos.
- **CA-06:** Dada a troca da cidade A para a cidade B, quando B estiver selecionada, então não apresentar os dados de A como se pertencessem a B, inclusive durante carregamento ou erro.

### US-03 — Consultar previsão

- **CA-07:** Dados o fuso de referência aprovado em OQ-02 e uma resposta completa, quando a previsão for apresentada, então abranger exatamente as datas D, D+1, D+2, D+3 e D+4, em ordem cronológica, sendo D a data de hoje nesse fuso.
- **CA-08:** Dada uma resposta com dados ausentes para uma das cinco datas, quando a previsão for apresentada, então identificar a indisponibilidade dessa data sem substituí-la por D+5, sem inventar valores e sem ocultar dados válidos das demais datas. Os campos e a granularidade a conferir dependem de OQ-03 e OQ-11.

### US-04 — Alternar unidade

- **CA-09:** Dado um acesso sem preferência salva aplicável, quando a temperatura aparecer pela primeira vez, então usar Celsius e identificar a unidade como °C.
- **CA-10:** Dados valores de referência de 0 °C e 20 °C, quando Fahrenheit for selecionado, então mostrar respectivamente 32 °F e 68 °F. A alternância deve atualizar todas as temperaturas do clima atual e da previsão sem alterar a cidade ou as datas.
- **CA-11:** Dada a exibição em Fahrenheit, quando Celsius for selecionado novamente, então recuperar os valores equivalentes em Celsius sem acumular erro de conversão. Arredondamento de valores fracionários depende de OQ-11.

### US-05 — Usar em dispositivos móveis

- **CA-12:** Dado cada tamanho de tela móvel aprovado em OQ-07, quando a pessoa executar busca, seleção, consulta e alternância de unidade, então conseguir concluir as etapas por toque, sem controles inacessíveis, sobreposição de textos ou rolagem horizontal da página.
- **CA-13:** Dada uma consulta em andamento ou concluída, quando a orientação da tela mudar em um dispositivo suportado, então manter a cidade e a unidade selecionadas, sem perder o acesso aos resultados e controles.

### US-06 — Compreender estados e recuperar falhas

- **CA-14:** Dado o primeiro acesso sem cidade selecionada, então apresentar a busca e um estado inicial distinto de erro ou de busca sem resultados, sem exibir dados fictícios como reais.
- **CA-15:** Dada uma busca ou consulta meteorológica pendente, então indicar carregamento até o sucesso ou a falha. Uma resposta antiga não pode substituir o resultado de uma seleção mais recente.
- **CA-16:** Dada uma busca bem-sucedida sem correspondências, então informar que nenhuma cidade foi encontrada e permitir editar o nome e realizar outra busca.
- **CA-17:** Dada falha de rede, resposta de erro do serviço ou timeout, então encerrar o carregamento, informar a falha em pt-BR e disponibilizar nova tentativa. Restabelecido o serviço, a nova tentativa deve poder concluir a mesma operação. O limite de timeout depende de OQ-10.
- **CA-18:** Dada uma resposta vazia ou incompleta para o clima, então identificar as informações indisponíveis e manter as válidas, sem apresentar valores ausentes como zero. O tratamento de dados antigos depende de OQ-09.

### US-07 — Compreender a interface

- **CA-19:** Dados os estados inicial, carregando, vazio, sucesso e erro, então os textos de interface e os nomes acessíveis dos controles devem estar em pt-BR, sem expor mensagens técnicas brutas do serviço. Nomes próprios de localidades podem preservar a forma fornecida pela fonte.
- **CA-20:** Dada uma previsão exibida, então cada data deve ser identificável em formato pt-BR e toda temperatura deve ter unidade identificável, inclusive após alternância entre Celsius e Fahrenheit.

### US-08 — Consultar sem conta

- **CA-21:** Dado um primeiro acesso sem sessão autenticada, quando a pessoa realizar o fluxo principal, então não exigir cadastro, login, senha ou API key própria.
- **CA-22:** Dado o fluxo principal completo, então nenhuma operação deve depender de criar ou recuperar contas, cidades, histórico ou preferências em armazenamento de servidor da aplicação. Consultar a Open-Meteo não constitui persistência de servidor da aplicação.

## Non-Functional Requirements

| ID | Categoria | Requisito e forma de verificação | Situação |
|---|---|---|---|
| RNF-01 | Responsividade | Executar os critérios de US-05 em toda a matriz móvel aprovada, sem perda de conteúdo ou funcionalidade. | Matriz pendente em OQ-07. |
| RNF-02 | Desempenho | Medir separadamente o tempo entre iniciar uma busca e exibir seus resultados e entre selecionar uma cidade e exibir o clima e a previsão. Definir limiares, perfil de rede, dispositivo e condições de teste. | Meta quantitativa pendente em OQ-10; não há SLA confirmado. |
| RNF-03 | Acessibilidade | Permitir o fluxo principal por teclado, com foco visível, ordem coerente, controles com nomes acessíveis e estados perceptíveis por tecnologia assistiva; não depender apenas de cor ou ícones para comunicar informação. Verificar manualmente com teclado e leitor de tela. | Base proposta; nível normativo, contraste e tecnologias de teste pendentes em OQ-12. |
| RNF-04 | Compatibilidade | Executar o fluxo principal nos navegadores, sistemas e versões acordados, em mobile e desktop. | Matriz pendente em OQ-07. |
| RNF-05 | Usabilidade | Identificar cidade, datas e unidades sem ambiguidade; distinguir os estados de US-06 e não confundir ausência de resultado com falha. Verificar com os cenários de aceite. | Proposto a partir do discovery. |
| RNF-06 | Confiabilidade | Não misturar localidades nem substituir a seleção atual por respostas anteriores; identificar dados ausentes sem fabricar medições. Verificar troca rápida de cidade e respostas incompletas. | Refinamento proposto dos riscos. |
| RNF-07 | Privacidade e acesso | Não exigir credenciais nem persistir dados da aplicação no servidor. O uso de armazenamento local e a solicitação de localização dependem de decisões específicas. | Restrição de servidor confirmada; OQ-05 e OQ-08 pendentes. |
| RNF-08 | Conformidade da fonte | Respeitar termos, licença, atribuição e limites aplicáveis à Open-Meteo antes da publicação. | Verificação pendente em OQ-01. |

## Edge Cases

| Caso | Comportamento esperado | Referência |
|---|---|---|
| Busca vazia ou somente espaços | Não consultar; solicitar um nome. | CA-03 |
| Nome com acentos, espaços ou caracteres de outro idioma | Permitir a entrada; exibir os resultados da fonte ou o estado vazio, sem presumir cobertura garantida. | CA-01, CA-16; OQ-06 |
| Cidade inexistente | Exibir ausência de resultados e permitir nova busca. | CA-16 |
| Cidades homônimas | Solicitar seleção explícita com contexto geográfico; não escolher arbitrariamente. | CA-02; OQ-04 |
| Região ou país ausente no resultado | Não inventar informação de localização; o meio alternativo de diferenciação precisa ser definido. | OQ-04 |
| Troca rápida de cidade; respostas fora de ordem | Manter os dados associados à seleção mais recente. | CA-06, CA-15 |
| Falha de rede, serviço indisponível ou limite de requisições | Exibir erro recuperável; não confundir com cidade inexistente. | CA-17; OQ-01, OQ-09 |
| Serviço não responde | Encerrar a espera ao atingir o timeout aprovado e permitir nova tentativa. | CA-17; OQ-10 |
| Resposta vazia, inválida ou parcialmente utilizável | Não inventar valores; mostrar o que for válido e identificar indisponibilidade ou erro conforme o caso. | CA-17, CA-18 |
| Falta de um dia na previsão | Identificar a lacuna dentro do intervalo definido, sem acrescentar um sexto dia. | CA-08 |
| Temperatura zero ou negativa | Tratar como valor válido, exibir o sinal quando negativo e aplicar a unidade selecionada. | CA-05, CA-10, CA-18 |
| Condição meteorológica desconhecida | Não atribuir uma condição conhecida sem correspondência; indicar descrição indisponível e preservar a temperatura válida. | CA-18 |
| Cidade em outro fuso; virada de dia, mês ou ano | Preservar cinco datas consecutivas segundo o fuso aprovado; definir quando recalcular o intervalo em uma sessão aberta. | CA-07; OQ-02 |
| Alternância de unidade durante consulta | Quando a resposta aparecer, respeitar a unidade atualmente selecionada. | RF-04, CA-10 |
| Tela pequena, rotação e nomes extensos | Manter textos legíveis, seleção inequívoca e controles acessíveis. | CA-12, CA-13 |
| Reabertura da aplicação | Aplicar Celsius na ausência de preferência local aprovada; não pressupor restauração de cidade ou histórico. | CA-09; OQ-05 |

## Assumptions

- **A-01:** As duas personas do discovery são hipóteses de público, não resultado de pesquisa validada.
- **A-02:** A busca manual por nome é o fluxo mínimo. Geolocalização não é necessária para completar esse fluxo e só será adicionada mediante decisão em OQ-08.
- **A-03:** Temperatura e descrição das condições formam o conjunto mínimo proposto para clima atual. Campos adicionais e dados por intervalo da previsão dependem de OQ-11.
- **A-04:** O cenário de sucesso com cinco datas pressupõe dados completos da fonte. Dados incompletos devem produzir um estado explícito, não uma previsão fabricada.
- **A-05:** As regras de entrada vazia, seleção explícita, nova tentativa e proteção contra respostas antigas são refinamentos propostos para tornar os fluxos verificáveis; não são decisões já confirmadas no discovery.
- **A-06:** Não há requisito aprovado de persistência local, cache ou uso offline. Isso não equivale a proibir essas capacidades; sua inclusão depende de OQ-05 e OQ-09.
- **A-07:** O serviço meteorológico fornece estimativas e pode apresentar atrasos ou indisponibilidade. Esta especificação não garante exatidão das condições reais.
- **A-08:** Metas quantitativas, cobertura geográfica e matrizes de compatibilidade e acessibilidade não podem ser inferidas das decisões confirmadas.

## Risks

| ID | Risco | Probabilidade | Impacto | Mitigação |
|---|---|---|---|---|
| R-01 | Lentidão, indisponibilidade ou limite de uso da Open-Meteo | Média | Alto: bloqueio de consultas | Confirmar termos e limites; definir timeout, erro recuperável e nova tentativa. |
| R-02 | Seleção incorreta de cidades homônimas | Média | Alto: consulta da localidade errada | Definir contexto geográfico e testar seleção com homônimos e dados incompletos. |
| R-03 | Ambiguidade de granularidade e fuso | Média | Médio: previsão inconsistente com a expectativa | Fechar OQ-02 e OQ-03 antes de detalhar a previsão no plano. |
| R-04 | Falta de rede ou apresentação de dados antigos como atuais | Média | Médio: informação indisponível ou enganosa | Distinguir erro e dados ausentes; definir política de cache e atualização em OQ-09. |
| R-05 | Experiência móvel inconsistente | Média | Alto: impossibilidade de concluir o fluxo | Aprovar matriz de telas e navegadores e validar os critérios móveis. |
| R-06 | Perguntas abertas tratadas como decisões durante a implementação | Média | Alto: retrabalho e divergência de escopo | Validar premissas e registrar respostas antes das tarefas dependentes; manter rastreabilidade RF → US → CA. |
| R-07 | Termos ou limites da fonte incompatíveis com o uso pretendido | A avaliar | Alto: impedimento de publicação ou necessidade de revisão | Verificar licença, atribuição e modalidade de uso antes da publicação. |

## Out of Scope

### Exclusões confirmadas

- Cadastro, login, gerenciamento de contas e autenticação.
- Persistência de dados da aplicação no servidor, incluindo histórico, cidades e preferências por conta.
- Fornecimento ou gerenciamento de API key pelo usuário para o fluxo definido.
- Interface em idiomas diferentes de pt-BR nesta versão.
- Previsão de datas além de hoje e dos quatro dias seguintes.

### Capacidades não incluídas no escopo inicial proposto

- Alertas meteorológicos, notificações, mapas e radar.
- Favoritos, comparação simultânea entre cidades e histórico de consultas.
- Recomendações automáticas de roupas, bagagem ou atividades; o app informa o tempo, e a pessoa toma a decisão.
- Aplicativos nativos e funcionalidades sociais.

Geolocalização, persistência local, cache, funcionamento offline e previsão horária não são exclusões confirmadas: permanecem pendentes nas Open Questions.

## Open Questions

| ID | Pergunta a decidir | Impacto e dependência |
|---|---|---|
| OQ-01 | Quais termos, custos, limites de uso e obrigações de atribuição da Open-Meteo se aplicam ao uso pretendido? | Condiciona viabilidade e publicação; RF-08, RNF-08. A fonte e a ausência de API key já estão decididas. |
| OQ-02 | Qual fuso define "hoje": cidade consultada, dispositivo ou outro? Quando o intervalo é recalculado se a sessão atravessar a meia-noite? | Bloqueia a definição completa das datas e sua atualização; RF-03, CA-07. |
| OQ-03 | A previsão será diária, horária ou ambas? | Bloqueia a definição do conteúdo por data e a validação detalhada da previsão; RF-03, CA-08. |
| OQ-04 | Quais informações diferenciam cidades homônimas, em que formato e como agir quando região ou país estiver ausente? | Bloqueia o detalhamento da seleção inequívoca; RF-01, CA-02. |
| OQ-05 | A unidade deve ser lembrada entre sessões no navegador? A cidade também? Como uma preferência salva afeta o início em Celsius? | Define persistência local e reabertura; RF-04, CA-09, CA-22. Persistência de servidor continua excluída. |
| OQ-06 | Quais regiões, países e cidades devem ser atendidos e quais limitações de cobertura serão aceitas? | Define os cenários de cobertura da busca e dos dados; RF-01 a RF-03. pt-BR não restringe geografia. |
| OQ-07 | Quais dispositivos, tamanhos de tela, orientações, sistemas, navegadores e versões móveis e desktop serão suportados? | Bloqueia a matriz final de validação de responsividade e compatibilidade; RF-05, RNF-01, RNF-04. |
| OQ-08 | Além da busca manual, haverá geolocalização? Se sim, o que ocorre quando a permissão for negada ou a localização falhar? | Pode ampliar escopo, permissões e requisitos de privacidade; A-02, RNF-07. |
| OQ-09 | Haverá cache ou funcionamento offline? Dados anteriores podem continuar visíveis após falha? Como indicar atualização e desatualização? | Define recuperação, validade dos dados e necessidade de persistência local; RF-06, CA-18. |
| OQ-10 | Quais os tempos máximos de busca, consulta e timeout e sob quais condições de rede e dispositivo devem ser medidos? | Bloqueia metas quantitativas e validação temporal; CA-17, RNF-02. |
| OQ-11 | Quais campos meteorológicos são obrigatórios no clima atual e em cada intervalo da previsão? Quais precisão e arredondamento usar? | Completa o contrato de apresentação e conversão; RF-02 a RF-04, CA-05, CA-08, CA-11. |
| OQ-12 | Qual nível de conformidade de acessibilidade, requisitos de contraste e tecnologias assistivas compõem a validação? | Completa o critério normativo de acessibilidade; RNF-03. |

As questões bloqueiam o fechamento dos requisitos que delas dependem, não alteram as decisões confirmadas e devem ser resolvidas antes de considerar esses requisitos prontos para implementação e aceite final.