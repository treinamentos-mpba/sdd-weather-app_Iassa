# Discovery: Aplicação de Previsão do Tempo

## Contexto

A empresa solicitou uma aplicação para consulta de informações meteorológicas por cidade. O briefing estabelece que usuários devem poder buscar cidades, consultar o clima atual, visualizar a previsão para cinco dias, alternar entre Celsius e Fahrenheit e usar a aplicação em dispositivos móveis.

O briefing original não define público-alvo, cenários prioritários, fonte dos dados meteorológicos ou critérios mensuráveis de qualidade. As definições posteriores estão registradas em Decisões; os pontos restantes precisam ser esclarecidos antes da especificação detalhada.

Personas iniciais para validação, ainda não confirmadas:

- Pessoa que consulta o tempo para decidir como se vestir ou planejar o dia.
- Pessoa que verifica a previsão de uma cidade antes de viajar.

## Decisões

| Decisão | Justificativa | Perguntas em aberto resolvidas e limites |
|---|---|---|
| **Fonte de dados: Open-Meteo, sem API key.** | Simplifica a integração e dispensa o gerenciamento de credenciais da API. | Define a fonte meteorológica e elimina a necessidade de chave. Custos, limites de uso e requisitos de licença aplicáveis ainda precisam ser verificados. |
| **Cinco dias = hoje + quatro dias seguintes.** | Estabelece um intervalo único para exibição e validação da previsão. | Resolve se o período inclui hoje. A granularidade diária, horária ou ambas e o fuso de referência para "hoje" continuam em aberto. |
| **Unidade padrão: Celsius.** | Garante uma apresentação inicial consistente, mantendo a alternância para Fahrenheit prevista no briefing. | Resolve a unidade inicial. Lembrar a preferência entre sessões no navegador ainda não foi decidido. |
| **Sem autenticação e sem persistência de servidor.** | Reduz o escopo inicial, dispensando contas e armazenamento de dados da aplicação no servidor. | Delimita o escopo de acesso e persistência, antes não definido, e descarta preferências salvas no servidor. Não decide persistência local no navegador nem funcionamento offline. |
| **Idioma da UI: pt-BR.** | Define um idioma único para os textos da interface e orienta sua localização. | Resolve o idioma da interface. As regiões e cidades atendidas continuam em aberto; pt-BR não restringe a busca ao Brasil. |

## Requisitos Funcionais

- **RF-01 - Buscar cidade:** permitir que o usuário pesquise uma cidade para consultar suas informações meteorológicas.
- **RF-02 - Consultar clima atual:** apresentar as condições meteorológicas atuais da cidade selecionada.
- **RF-03 - Consultar previsão:** apresentar a previsão do tempo para hoje e os quatro dias seguintes.
- **RF-04 - Alternar unidade:** permitir alternar a exibição da temperatura entre Celsius e Fahrenheit.
- **RF-05 - Usar em dispositivos móveis:** permitir o acesso e uso das capacidades descritas em dispositivos móveis. Os dispositivos e tamanhos de tela suportados precisam ser definidos.

## Requisitos Não-Funcionais

Os requisitos abaixo são propostas iniciais para validação; o briefing não define metas quantitativas.

- **RNF-01 - Responsividade:** a interface deve se adaptar a dispositivos móveis e permitir a conclusão dos fluxos principais em telas menores.
- **RNF-02 - Desempenho:** busca e consulta devem responder dentro de um tempo aceitável a ser acordado com as partes interessadas.
- **RNF-03 - Acessibilidade:** a aplicação deve considerar navegação por teclado, identificação acessível de controles e contraste adequado. O nível de conformidade precisa ser definido.
- **RNF-04 - Compatibilidade:** navegadores, sistemas operacionais e versões suportados devem ser acordados e documentados.
- **RNF-05 - Usabilidade:** resultados, unidades de temperatura e localização consultada devem ser apresentados de forma clara, inclusive em dispositivos móveis.

## Riscos

| Risco | Probabilidade | Impacto | Mitigação sugerida |
|---|---|---|---|
| Indisponibilidade, lentidão ou limite de uso da fonte de dados meteorológicos | Média | Alto: usuários podem não conseguir consultar informações atualizadas | Avaliar disponibilidade, custos e limites da fonte; definir timeout, mensagens de erro e estratégia de nova tentativa |
| Busca retorna cidades homônimas ou uma localização incorreta | Média | Alto: os dados apresentados podem corresponder a outra cidade | Definir como desambiguar resultados, incluindo região ou país quando disponível |
| Interpretação diferente sobre a granularidade ou o fuso da previsão | Média | Médio: o conteúdo pode não corresponder à expectativa do usuário | Definir se a previsão será diária, horária ou ambas e o fuso de referência para hoje; o intervalo já foi fixado em hoje + quatro dias |
| Falha de rede impede a consulta | Média | Médio: o usuário pode ficar sem dados meteorológicos | Definir estados de erro, possibilidade de nova tentativa e necessidade de cache ou indicação de dados desatualizados |
| Experiência inconsistente entre dispositivos móveis | Média | Alto: controles ou informações podem ficar difíceis de usar | Definir dispositivos e navegadores suportados e validar fluxos em diferentes tamanhos de tela |

## Perguntas em Aberto

- **Quais custos, limites de uso e requisitos de licença da Open-Meteo se aplicam ao projeto?** **Impacto:** influencia viabilidade, cobertura e custos operacionais; a fonte e a ausência de API key já estão definidas.
- **Qual fuso define "hoje" na previsão?** **Impacto:** determina as datas exibidas no intervalo de hoje + quatro dias.
- **A previsão deve ser diária, horária ou ambas?** **Impacto:** afeta o conteúdo, a interface e os dados necessários.
- **Como distinguir cidades com o mesmo nome?** **Impacto:** sem contexto como estado/região ou país, o usuário pode selecionar a localização errada.
- **A escolha de unidade deve ser lembrada entre sessões no navegador?** **Impacto:** define a persistência local de preferências; Celsius é o padrão e não haverá persistência de servidor.
- **Quais regiões e cidades devem ser atendidas?** **Impacto:** afeta a cobertura da busca e a disponibilidade de localidades; o idioma da UI já foi definido como pt-BR.
- **Quais dispositivos, tamanhos de tela e navegadores móveis devem ser suportados?** **Impacto:** sem esse escopo, não há critério objetivo para validar compatibilidade.
- **A aplicação deve solicitar geolocalização ou oferecer apenas busca manual?** **Impacto:** pode alterar o fluxo de uso, as permissões e as considerações de privacidade.
- **O que deve ocorrer quando não houver conexão ou a fonte de dados estiver indisponível?** **Impacto:** determina se haverá cache, nova tentativa ou apenas uma mensagem de erro.
- **Qual é o tempo máximo aceitável para exibir os resultados?** **Impacto:** sem uma meta, o desempenho não pode ser verificado objetivamente.

## Suposições

- As cinco capacidades listadas no briefing compõem o escopo inicial da aplicação.
- A consulta meteorológica será associada a uma cidade escolhida pelo usuário; o mecanismo de seleção ainda precisa ser definido.
- A alternância entre Celsius e Fahrenheit se aplica à apresentação das temperaturas. Não está definido se a preferência será persistida.
- Fonte de dados, unidade inicial, idioma, autenticação e persistência de servidor foram definidos na seção Decisões. Geolocalização automática e funcionamento offline permanecem em aberto.
