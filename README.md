# EV ChargeOps — Enterprise Challenge 2026 | Sprint 01

**Instituição:** FIAP  
**Parceiro:** GoodWe  
**Desafio:** EV ChargeOps — Gestão Inteligente de Recargas Compartilhadas  
**Sprint:** 01 — Pesquisa e Documentação  
**Integrantes:** 
| Nome | RM |
|------|----|
| Leonardo Tims Carneiro Campello | 571048 |
| Emanuel Batista Braga Domingues | 569732 |
| Sarah Iraci Bessa de Moura | 573889 |
| João Pedro dos Anjos de Oliveira Hornos | 572004 |
---

## Descrição do Problema e Contexto do Desafio

A GoodWe opera um carregador de veículos elétricos modelo HCA G2 no estacionamento da FIAP — Unidade Aclimação, dentro do Energy Innovation Lab. Esse cenário ilustra com precisão um problema que se replica em condomínios residenciais, edifícios corporativos e campi universitários em todo o Brasil: infraestruturas de recarga compartilhadas carecem de mecanismos para identificar usuários, registrar o consumo individual, calcular rateios justos e apresentar essas informações de forma clara para gestores e moradores.

O crescimento acelerado da frota eletrificada no Brasil torna esse problema urgente. Segundo a ABVE, a frota nacional de veículos eletrificados ultrapassou 700 mil unidades em março de 2026, com 223.912 emplacamentos somente em 2025 — um crescimento de 26% sobre 2024. Ao mesmo tempo, o número de eletropostos públicos e semipúblicos era de cerca de 16.880 pontos em agosto de 2025, concentrados principalmente no Sudeste. Essa assimetria entre crescimento da frota e disponibilidade de infraestrutura torna a gestão de pontos compartilhados um gargalo operacional real.

Cada sessão de recarga gera dados: duração, energia entregue em kWh, horário de início e fim, identidade do usuário, potência consumida. Quando organizados, esses dados permitem calcular cobranças individuais, identificar padrões de uso, prever demandas e otimizar a disponibilidade do carregador. O EV ChargeOps propõe essa transformação: de registros brutos de sessão para uma plataforma de inteligência operacional com rateio automatizado e interface acessível.

---

## Frente 1 — Contexto e Problema

### Infraestruturas de Recarga Compartilhada

Uma infraestrutura de recarga compartilhada é um conjunto de um ou mais carregadores instalados em área de uso coletivo — garagem de condomínio, estacionamento corporativo, campus universitário — onde diferentes usuários se revezam no uso do mesmo equipamento. Diferente de um carregador doméstico dedicado a um único veículo, o carregador compartilhado exige regras de agendamento, identificação de usuário, medição individual de consumo e mecanismo de cobrança diferenciada.

Os principais desafios operacionais enfrentados por gestores nesse contexto são:

- Ausência de identificação automática do usuário, o que inviabiliza a cobrança individual sem intervenção manual.
- Risco de bloqueio do equipamento por usuários que permanecem conectados além do necessário.
- Dificuldade de comunicar o consumo e os custos de forma transparente para moradores e gestores.
- Sobrecarga da rede elétrica do condomínio quando múltiplos veículos tentam carregar simultaneamente sem controle de potência.
- Falta de integração entre os dados do carregador e os sistemas administrativos do condomínio.

### Como Funciona uma Sessão de Recarga

Do ponto de vista técnico, uma sessão de recarga passa pelas seguintes etapas:

1. Autenticação: o usuário se identifica por cartão RFID, aplicativo ou plug-and-charge automático.
2. Negociação de potência: o carregador e o veículo trocam sinais pelo protocolo IEC 61851 (via cabo) ou ISO 15118 (via comunicação digital) para definir a corrente máxima disponível.
3. Transferência de energia: a corrente alternada flui do carregador para o sistema de bordo do veículo (OBC — On-Board Charger), que converte para corrente contínua e carrega a bateria.
4. Monitoramento contínuo: ao longo da sessão, o carregador registra potência instantânea (kW), energia acumulada (kWh), tensão, corrente e temperatura.
5. Encerramento: a sessão termina por solicitação do usuário, atingimento da carga programada ou desconexão do cabo. O carregador gera um registro com os dados completos da sessão.

Os dados gerados em cada sessão incluem: identificador do usuário, carimbo de tempo de início e fim, energia entregue (kWh), potência máxima e média, e quaisquer eventos de erro ou interrupção.

### Modelos de Negócio para Recarga Compartilhada

Existem cinco modelos principais, cada um com características distintas:

- Recarga gratuita: o custo da energia é absorvido pelo condomínio ou empresa. Simples de operar, mas gera subsídio cruzado e favorece abuso por usuários de alto consumo.
- Cobrança por kWh: o usuário paga exatamente pela energia que consumiu. É o modelo mais justo e transparente, mas exige medição precisa e sistema de faturamento. No Brasil, tarifas em eletropostos AC públicos ficam na faixa de R$ 1,20 a R$ 2,50/kWh.
- Cobrança por tempo: o usuário paga pelo tempo de conexão, independentemente da energia transferida. Desencoraja ocupação prolongada, mas pode ser injusto quando a potência de carga varia (especialmente nos últimos 20% de carga, quando o veículo reduz a potência aceita).
- Assinatura mensal: o usuário paga uma mensalidade fixa que cobre infraestrutura, suporte e um volume de energia predefinido. Previsível para o usuário, mas pode gerar custos ocultos se o uso for baixo.
- Rateio condominial: o custo total da energia consumida no mês é dividido entre os usuários do carregador de forma proporcional ao consumo individual registrado. Esse modelo é o mais adequado ao contexto de campus universitário ou condomínio, pois combina justiça (cada um paga pelo que usou) com simplicidade administrativa.

### Opção de Aprofundamento Escolhida: Análise de Mercado (Opção A)

Foram analisadas três soluções existentes no mercado de gestão de recarga compartilhada:

**Zaptec Pro**  
Resolve o problema de gestão de recarga em edifícios com múltiplos usuários. Oferece balanceamento dinâmico de carga entre vários carregadores, identificação por RFID, relatórios de consumo por usuário e integração com sistemas de faturamento. O modelo de negócio combina venda do hardware com assinatura do software de gestão. A limitação é o custo elevado de implantação e a dependência de ecossistema fechado — o software de gestão só funciona com hardware Zaptec.

**ChargePoint (CPF25)**  
Resolve o problema de redes de recarga em escala, com foco em frotas corporativas e estacionamentos. Oferece autenticação por RFID e aplicativo, relatórios detalhados de sessão, cobrança automática e integração com ERPs corporativos. O modelo é baseado em assinatura do software de rede (ChargePoint Network). A limitação principal para o contexto brasileiro é que a plataforma foi desenvolvida para o mercado norte-americano, com suporte limitado e ausência de integração com distribuidoras brasileiras.

**Neocharge Smart**  
Resolve o problema de recarga inteligente residencial e de pequenos condomínios no Brasil. Oferece monitoramento de consumo, agendamento de recarga, controle por aplicativo e integração com inversores solares. O modelo de negócio é venda do hardware com software de gestão incluso. A limitação é o escopo restrito: não oferece gestão de múltiplos usuários em carregador compartilhado nem sistema de faturamento coletivo.

Nenhuma das três soluções analisadas resolve de forma integrada o problema específico do EV ChargeOps: um único carregador compartilhado por múltiplos usuários não identificados, com necessidade de rateio mensal e integração com a API do carregador GoodWe já instalado. Essa lacuna justifica o desenvolvimento da plataforma proposta.

---

## Frente 2 — Base Regulatória e Técnica

### Resolução Normativa ANEEL nº 1.000/2021

A RN 1.000/2021 consolidou 61 normas anteriores da ANEEL, incluindo a RN 819/2018, que era a regulamentação específica para recarga de veículos elétricos. O Capítulo V da norma trata exclusivamente das instalações de recarga.

Os pontos mais relevantes para o EV ChargeOps são:

- É permitida a recarga de veículos de propriedade distinta do titular da unidade consumidora, inclusive para fins de exploração comercial com preços livremente negociados (art. 9º).
- A atividade de recarga explorada por terceiros é classificada como atividade acessória complementar, não regulada pela ANEEL, o que significa que não é necessária concessão ou autorização específica para operar o serviço.
- Estações de recarga destinadas a uso não exclusivamente privado devem usar protocolos abertos de comunicação — requisito que o GoodWe HCA G2 atende via OCPP (Open Charge Point Protocol), disponível na integração com o SEMS Portal.
- A distribuidora local deve ser comunicada previamente à instalação quando o carregador estiver ligado à rede da unidade consumidora.

A solução EV ChargeOps está em conformidade com essa regulamentação: opera em ambiente de uso compartilhado dentro de uma única unidade consumidora (a FIAP), não realiza injeção de energia na rede, e utiliza o carregador já instalado e homologado pela GoodWe.

### Carregador GoodWe HCA G2 — Interfaces e Usos

O HCA G2 oferece cinco interfaces de conectividade, cada uma com um papel específico na plataforma:

- RS-485 (x2): protocolo serial para comunicação com inversores fotovoltaicos GoodWe e com medidores de energia MID. Permite integração direta com o sistema de energia solar do campus, habilitando o modo de recarga por excedente solar.
- LAN (Ethernet): conecta o carregador ao roteador local e ao SEMS Portal via rede cabeada. É a interface mais estável para coleta de dados em tempo real e para integração com o back-end da plataforma.
- Wi-Fi: alternativa à LAN para ambientes sem infraestrutura cabeada. Suporta as mesmas funções de monitoramento e controle remoto via SEMS+.
- Bluetooth: usado exclusivamente para configuração local e pareamento inicial com o aplicativo SolarGo. Não é uma interface de dados contínua para a plataforma.
- RFID: permite autenticação de usuários por cartão ou tag. O carregador é entregue com dois cartões RFID. Essa é a interface de identificação de usuário que o EV ChargeOps utilizará como mecanismo de início de sessão autenticada.

Para o EV ChargeOps, as interfaces relevantes são RFID (autenticação do usuário), LAN/Wi-Fi (coleta de dados via API) e RS-485 (integração com geração solar, opcional na Sprint 02).

### Opção de Aprofundamento Escolhida: APIs Complementares (Opção C)

Além da API do SEMS Portal da GoodWe, foram identificadas duas APIs externas que podem enriquecer a plataforma:

**API GoodWe SEMS Portal**

A GoodWe oferece três modalidades de acesso à API: Open API (para contas organizacionais SEMS), Real-time Data Monitoring API (para terceiros com whitelist de dispositivos) e Batch Remote Control Interface. Para o contexto do EV ChargeOps, o acesso mais relevante é via endpoint de sessão do carregador.

O fluxo de autenticação usa o endpoint `POST /api/v1/Common/CrossLogin` com credenciais de conta SEMS, retornando um token no formato JSON com campos `uid`, `timestamp` e `token`. Esse token é codificado em Base64 e usado como header nas chamadas subsequentes.

Para dados do carregador, o endpoint principal identificado na comunidade de desenvolvedores é:

```
GET /powerstation/EvChargerDetail?stationId={station_id}
GET /api/v4/EvCharger/GetEvChargerMoreView
GET /api/v3/EvCharger/GetCurrentChargeinfo
```

Os campos retornados incluem `evChargerStatus` (idle, charging, error), `power` (potência instantânea em kW), `energy` (energia entregue na sessão em kWh) e eventos de sessão com timestamps. O formato de resposta é JSON com envelope padrão `{ "hasError": false, "code": 0, "data": {...} }`.

Uma limitação importante: a API do SEMS Portal não tem documentação pública completa e oficial. Os endpoints para carregador foram mapeados pela comunidade open source (repositório `goodwe-sems-home-assistant` e projeto `goodwe-wallbox-sems-home-assistant`). O EV ChargeOps deve tratar esses endpoints como não documentados e implementar fallbacks robustos para mudanças de versão.

**Open Charge Map API**

A Open Charge Map é um banco de dados global e comunitário de estações de recarga, com mais de 300 mil localizações cadastradas. Sua API REST pública retorna dados em JSON com o endpoint:

```
GET https://api.openchargemap.io/v3/poi/?output=json&latitude={lat}&longitude={lng}&distance={km}&maxresults={n}&key={api_key}
```

Para o EV ChargeOps, essa API tem dois usos potenciais: contextualizar o carregador da FIAP no mapa de infraestrutura da região e, na interface do gestor, mostrar a densidade de eletropostos próximos como referência para comparação de disponibilidade.

Os campos retornados incluem nome e endereço da estação, tipo e potência dos conectores, operador, status operacional, e avaliações de usuários. A API é gratuita para uso não comercial mediante registro de chave.

**ANEEL Open Data**

O portal de dados abertos da ANEEL (`dadosabertos.aneel.gov.br`) disponibiliza datasets sobre concessões de distribuição, outorgas e, em processo de estruturação, dados sobre infraestrutura de recarga cadastrada. Para o EV ChargeOps, essa fonte é relevante para enriquecer relatórios regulatórios e para cruzar o perfil de uso do carregador com dados tarifários regionais.

---

## Frente 3 — Arquitetura e IA

### Camadas da Plataforma EV ChargeOps

A arquitetura da plataforma é organizada em quatro camadas:

**Camada Física**  
O carregador GoodWe HCA G2, instalado no estacionamento L1 da FIAP, Aclimação. É o ponto de geração de dados brutos: potência, energia, status de sessão, identidade RFID.

**Camada de Conectividade**  
A comunicação entre o carregador e a plataforma ocorre via LAN/Wi-Fi, com o carregador conectado ao SEMS Portal da GoodWe. A plataforma EV ChargeOps consome os dados via API REST do SEMS Portal, com polling periódico (a cada 30–60 segundos durante sessões ativas) e webhooks para eventos de início e fim de sessão quando disponíveis. O protocolo RFID cuida da autenticação local do usuário no momento da conexão.

**Camada de Aplicação**  
Back-end em Python responsável por: coletar e persistir os dados de sessão, associar cada sessão ao usuário autenticado via RFID, calcular o rateio mensal, gerar faturas individuais, e expor uma API interna para as interfaces de usuário. É aqui que os modelos de IA operam sobre os dados históricos.

**Camada de Apresentação**  
Duas interfaces distintas: um painel do gestor (web) com visão consolidada de sessões, consumo por usuário, alertas e exportação de relatórios; e uma interface do usuário (mobile-first ou web) com histórico de recargas pessoais, consumo do mês e fatura estimada.

O diagrama de arquitetura está disponível em `docs/arquitetura.png` neste repositório.

### Fluxo de Dados: da Sessão à Fatura

O caminho completo percorrido pelos dados, desde o início de uma recarga até o fechamento da fatura mensal, é o seguinte:

1. O usuário aproxima o cartão RFID do carregador. O carregador valida o ID do cartão contra a lista de cartões autorizados e inicia a sessão.
2. O carregador passa a registrar a sessão localmente e transmite os dados ao SEMS Portal em tempo real via LAN.
3. O back-end do EV ChargeOps faz polling na API do SEMS Portal a cada 60 segundos durante sessões ativas, coletando potência instantânea e energia acumulada.
4. Ao final da sessão (desconexão ou término manual), o carregador grava o registro completo: ID do usuário (RFID), timestamp de início e fim, energia total entregue (kWh).
5. O back-end persiste esse registro na tabela de sessões do banco de dados, associando o ID RFID ao cadastro do usuário na plataforma.
6. Ao final do mês, o módulo de faturamento agrega todas as sessões de cada usuário, calcula o consumo total em kWh, aplica a tarifa vigente e gera a fatura individual.
7. A fatura é disponibilizada na interface do usuário e enviada por e-mail. Um resumo consolidado é gerado para o gestor.

A IA entra nesse fluxo em duas etapas: na previsão de demanda (entre as etapas 3 e 5) e na detecção de anomalias (após a etapa 5), conforme detalhado na seção seguinte.

### Modelo de Rateio Proposto

O EV ChargeOps adota o modelo de rateio por consumo real medido (kWh), com uma taxa de infraestrutura mensal fixa distribuída entre os usuários cadastrados ativos no período.

A fórmula de cálculo da fatura mensal individual é:

```
Fatura_i = (kWh_i × tarifa_kWh) + (custo_infraestrutura / n_usuarios_ativos)
```

Onde:
- `kWh_i` é a energia total consumida pelo usuário i no mês, calculada como soma dos kWh de todas as suas sessões no período.
- `tarifa_kWh` é o custo unitário de energia, calculado dividindo o valor total da conta de energia do condomínio/campus pelo total de kWh consumidos no mesmo período (tarifa real, não estimada).
- `custo_infraestrutura` é a parcela mensal de amortização da instalação e manutenção do carregador, definida em assembleia ou pela gestão do campus.
- `n_usuarios_ativos` é o número de usuários que realizaram ao menos uma sessão no mês.

Tratamento de casos excepcionais:

- Sessão interrompida (falha de rede, queda de energia): o sistema usa o último valor de energia acumulada registrado antes da interrupção. Se nenhum dado foi capturado, a sessão é marcada como incompleta e excluída do faturamento, com notificação ao gestor para revisão manual.
- Usuário que não carregou no mês: não é cobrado pela energia (kWh_i = 0), mas pode ser cobrado pela taxa de infraestrutura conforme regra definida pela gestão. A configuração padrão exclui usuários inativos do rateio da infraestrutura.
- Dois veículos da mesma unidade: cada veículo é associado a um cartão RFID distinto. As sessões são registradas separadamente e somadas para compor uma fatura única para a unidade, ou faturas individuais conforme preferência do gestor.

**Justificativa da escolha:** O modelo por kWh real é o mais justo porque cada usuário paga exatamente pelo que consumiu, sem subsidiar o consumo alheio. Comparado ao modelo de assinatura mensal fixa, ele elimina a percepção de custo injusto por parte de usuários de baixo consumo. Comparado ao rateio por tempo, ele é mais preciso porque a potência de carregamento varia ao longo da sessão (especialmente nos últimos 20% de carga), e cobrar por tempo nesses casos superestima o custo.

### Opção de Aprofundamento 1: Papel da IA (Opção B)

Foram identificadas duas abordagens de IA com aplicação direta e estrutural no EV ChargeOps:

**Regressão para Previsão de Demanda**

Problema que resolve: o gestor não sabe quando o carregador estará disponível ou quando ocorrerão picos de demanda que possam sobrecarregar a rede elétrica do campus.

Técnica: regressão linear múltipla ou modelo de séries temporais (Prophet ou ARIMA), treinado sobre o histórico de sessões. As variáveis de entrada incluem dia da semana, horário, semana do mês, número de usuários cadastrados e, opcionalmente, dados climáticos. A saída é a probabilidade de o carregador estar ocupado em cada faixa horária.

Dados necessários: histórico de sessões com timestamps, duração e energia por faixa horária, mínimo de 30 dias de operação para o modelo inicial.

Impacto esperado: o gestor pode identificar horários de pico, redistribuir o uso por meio de alertas ou agendamento, e evitar cobranças de demanda da distribuidora por picos inesperados. Na interface do usuário, o sistema pode sugerir o melhor horário para recarregar com base na previsão de disponibilidade.

**Detecção de Anomalias**

Problema que resolve: sessões com consumo anormalmente alto, sessões muito longas sem carga efetiva ou padrões que sugerem mau uso do carregador passam despercebidos sem análise automatizada.

Técnica: Isolation Forest ou Z-score sobre as variáveis de consumo (kWh por sessão, duração, potência média). O modelo aprende o perfil típico de uso de cada usuário e sinaliza desvios significativos.

Dados necessários: histórico de sessões por usuário, com energia, duração e potência média. O modelo pode ser treinado com dados sintéticos inicialmente e refinado com dados reais a partir da Sprint 02.

Impacto esperado: redução de erros de faturamento causados por sessões corrompidas ou atípicas, alertas automáticos para o gestor em casos suspeitos, e proteção contra abuso do sistema por usuários não cadastrados que eventualmente consigam iniciar uma sessão.

### Opção de Aprofundamento 2: Esquema da Base de Dados (Opção C)

O esquema de dados da plataforma é composto por quatro entidades principais:

**Tabela `usuarios`**

| Campo | Tipo | Descrição |
|---|---|---|
| id | UUID | Identificador único |
| nome | VARCHAR(100) | Nome completo |
| email | VARCHAR(100) | E-mail de contato |
| rfid_id | VARCHAR(50) | ID do cartão RFID vinculado |
| unidade | VARCHAR(20) | Número da unidade ou sala |
| ativo | BOOLEAN | Usuário habilitado a carregar |
| criado_em | TIMESTAMP | Data de cadastro |

**Tabela `sessoes`**

| Campo | Tipo | Descrição |
|---|---|---|
| id | UUID | Identificador único da sessão |
| usuario_id | UUID | FK para usuarios |
| inicio | TIMESTAMP | Início da sessão |
| fim | TIMESTAMP | Fim da sessão (NULL se em andamento) |
| energia_kwh | DECIMAL(8,3) | Energia entregue na sessão |
| potencia_media_kw | DECIMAL(6,3) | Potência média durante a sessão |
| status | ENUM | completed, interrupted, in_progress |
| fonte_dados | VARCHAR(20) | sems_api, manual |

**Tabela `faturas`**

| Campo | Tipo | Descrição |
|---|---|---|
| id | UUID | Identificador único |
| usuario_id | UUID | FK para usuarios |
| periodo_inicio | DATE | Primeiro dia do período |
| periodo_fim | DATE | Último dia do período |
| energia_total_kwh | DECIMAL(10,3) | Total consumido no período |
| tarifa_kwh | DECIMAL(6,4) | Tarifa aplicada (R$/kWh) |
| custo_energia | DECIMAL(10,2) | energia_total_kwh × tarifa_kwh |
| custo_infraestrutura | DECIMAL(10,2) | Parcela da infraestrutura |
| total | DECIMAL(10,2) | Valor final da fatura |
| gerada_em | TIMESTAMP | Data de geração |

**Tabela `configuracoes`**

| Campo | Tipo | Descrição |
|---|---|---|
| chave | VARCHAR(50) | Nome do parâmetro |
| valor | TEXT | Valor atual |
| atualizado_em | TIMESTAMP | Última atualização |

Exemplos de registros simulados (para uso na Sprint 02):

```json
// Sessão completa
{
  "id": "a1b2c3d4-...",
  "usuario_id": "usr-001",
  "inicio": "2026-06-10T08:15:00",
  "fim": "2026-06-10T09:47:00",
  "energia_kwh": 11.240,
  "potencia_media_kw": 7.21,
  "status": "completed",
  "fonte_dados": "sems_api"
}

// Fatura mensal
{
  "id": "fat-001",
  "usuario_id": "usr-001",
  "periodo_inicio": "2026-06-01",
  "periodo_fim": "2026-06-30",
  "energia_total_kwh": 58.720,
  "tarifa_kwh": 0.9800,
  "custo_energia": 57.55,
  "custo_infraestrutura": 12.50,
  "total": 70.05
}
```

---

## Diagrama de Arquitetura
O diagrama de arquitetura está disponível em `docs/arquitetura.png` neste repositório.

A arquitetura segue o fluxo:

```
[Carregador GoodWe HCA G2]
        |
    [RFID / LAN]
        |
[SEMS Portal — API REST]
        |
[Back-end Python — Coleta, Processamento, IA]
        |
   [Banco de Dados PostgreSQL]
        |
   [API Interna REST]
       / \
[Painel Gestor]  [App Usuário]
```

---

## Plano para a Sprint 02

A Sprint 02 terá foco no desenvolvimento e prototipação da plataforma. O trabalho será organizado nas seguintes etapas, em ordem de execução:

**Etapa 1 — Infraestrutura e Integração (semanas 1–2)**  
Configuração do banco de dados PostgreSQL com o esquema definido nesta sprint. Implementação do módulo de coleta de dados via API do SEMS Portal, com autenticação, polling e persistência das sessões. Testes com dados reais do carregador da FIAP.

**Etapa 2 — Módulo de Faturamento (semanas 3–4)**  
Implementação da lógica de rateio conforme o modelo definido. Geração automática de faturas mensais por usuário. Tratamento dos casos excepcionais mapeados (sessão interrompida, usuário inativo, dois veículos por unidade).

**Etapa 3 — Modelos de IA (semanas 5–6)**  
Implementação do modelo de regressão para previsão de demanda, treinado sobre dados históricos simulados e refinado com dados reais. Implementação do módulo de detecção de anomalias com Isolation Forest. Integração dos modelos ao back-end como serviços internos.

**Etapa 4 — Interfaces (semanas 7–8)**  
Desenvolvimento do painel do gestor (web, framework a definir — Django ou FastAPI + React). Desenvolvimento da interface do usuário (mobile-first). Integração com a API interna.

**Etapa 5 — Testes e Documentação (semana 9)**  
Testes de integração ponta a ponta. Documentação técnica da API interna. Preparação do vídeo pitch de 3 minutos.

Tecnologias previstas: Python (back-end, IA), PostgreSQL (banco de dados), FastAPI (API interna), scikit-learn / Prophet (modelos de IA), React (front-end), Docker (containerização).

---

## Fontes Consultadas

- ABVE. Eletrificados crescem dez vezes mais do que o conjunto do mercado, e vendas chegam a 224 mil veículos em 2025. Disponível em: abve.org.br. Acesso em: jun. 2026.
- ABVE / Canal VE. Frota de eletrificados no Brasil ultrapassa 700 mil unidades. Disponível em: canalve.com.br. Acesso em: jun. 2026.
- ANEEL. Resolução Normativa nº 1.000, de 7 de dezembro de 2021. Disponível em: aneel.gov.br. Acesso em: jun. 2026.
- ANEEL. Veículos Elétricos — página temática. Disponível em: gov.br/aneel. Acesso em: jun. 2026.
- GoodWe. HCA G2 Series EV Charger — página do produto. Disponível em: en.goodwe.com/hca-g2. Acesso em: jun. 2026.
- GoodWe. User Manual AC Charger HCA Series V1.5-2025 (7-22kW) G2. Disponível em: goodwe.com. Acesso em: jun. 2026.
- GoodWe. GOODWE API Technical Document. Disponível em: community.goodwe.com. Acesso em: jun. 2026.
- Karunanayake, B. Accessing GoodWe Sems Portal API: A Comprehensive Guide. Medium, 2024. Disponível em: medium.com. Acesso em: jun. 2026.
- GitHub. TimSoethout/goodwe-sems-home-assistant — discussão sobre EV Charger endpoint (#179). Disponível em: github.com. Acesso em: jun. 2026.
- Open Charge Map. API Documentation. Disponível em: openchargemap.org/site/develop/api. Acesso em: jun. 2026.
- Power2Go. Cobrança por kWh ou assinatura mensal? O melhor modelo para recarga de carros elétricos. Disponível em: power2go.com.br. Acesso em: jun. 2026.
- Voltbras. Legislação brasileira sobre eletropostos: o que saber antes de investir. Disponível em: voltbras.com. Acesso em: jun. 2026.
- EnergySpot. Abastecimento de carros elétricos em condomínio: quem se responsabiliza pela conta? Disponível em: energyspot.com.br. Acesso em: jun. 2026.
- Zangari Administração. Carros elétricos em condomínio: o que mudou em 2026 e como se preparar. Disponível em: zangari.com.br. Acesso em: jun. 2026.
- EkkoGreen. Abastecimento de carros elétricos no Brasil. Disponível em: ekkogreen.com.br. Acesso em: jun. 2026.
- Canal Solar. O futuro da mobilidade e os desafios tributários no Brasil. Disponível em: canalsolar.com.br. Acesso em: jun. 2026.
