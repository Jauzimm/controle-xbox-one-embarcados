# Trabalho 2 - 2026-1: Estudo do Controle de Xbox One

## Integrantes do Grupo

| Nome | Matrícula |
| :--- | :--- |
| Edilson Ribeiro | 222024461 |
| Ruan Carvalho | 211043763 |
| Gustavo Feitosa Haubert | 222024793 |
| João Vitor Santos de Oliveira | 221022337 |

---

# 1. Descrição do Produto Selecionado

## 1.1. Funções Principais, Público-Alvo e Contexto de Uso

Este dispositivo foi projetado para usuários do Xbox One, sendo o controle tendo foco em alta performance, entregando baixa latência e um feedback tátil altamente imersivo. Graças a essa alta performance, ele atende a um público-alvo amplo, que vai desde jogadores casuais até atletas profissionais de esportes. Essa versatilidade se reflete também em seu contexto de uso: além de ser peça-chave em consoles Xbox, PCs (Windows) e plataformas móveis, o dispositivo vai além do mundo dos games, podendo ser aplicado em projetos de robótica e desenvolvimento acadêmico.

## 1.2. Componentes e Sensores Utilizados

Agora sobre componentes e sensores, o controle adota uma arquitetura focada em imersão e durabilidade. O sistema tátil é composto por uma topologia de quatro motores de massa rotativa excêntrica (ERM): dois motores principais integrados ao chassi (esquerdo e direito) para vibrações globais e dinâmicas, e dois micromotores independentes posicionados nos gatilhos esquerdo (LT) e direito (RT) para respostas táteis localizadas na ponta dos dedos. Já o sensoriamento de posição é heterogêneo: os gatilhos (LT/RT) utilizam sensores magnéticos de Efeito Hall (componentes U10 e U11 no PCB), uma tecnologia que elimina o contato mecânico e evita o desgaste por atrito; os joysticks analógicos duplos, por outro lado, utilizam potenciômetros convencionais — um contato condutor deslizando sobre uma trilha resistiva —, o que os torna suscetíveis ao desgaste mecânico e é a causa raiz do conhecido problema de *stick drift* deste modelo de controle.

Para o comando digital, o projeto combina diferentes engenharias de acionamento para otimizar o tato e a vida útil do hardware. Os botões de ação (A, B, X, Y) utilizam membranas para um clique mais macio e confortável, já o direcional (D-Pad) emprega chapa metálicas para garantir respostas rápidas e aquele feedback firme e audível. Complementando o conjunto, os botões de ombro superiores utilizam micro-switches, priorizando a máxima precisão.

Por fim, a interface do sistema é gerenciada por botões de navegação dedicados. O "Botão Xbox", posicionado centralmente, atua como o guia principal para ligar o console e acessar rapidamente o painel de controle, enquanto os botões "Menu" e "View" desempenham funções contextuais, permitindo a interação com interfaces de software, gerenciamento de menus e navegação dinâmica dentro dos jogos.

## 1.3. Tecnologias de Comunicação e Controle Embarcadas
Na parte inferior abriga uma entrada de 3,5 mm (P2/P3), garantindo conectividade analógica direta para fones de ouvido e headsets.

Para jogar sem fios, ele usa uma conexão exclusiva super rápida e, nos modelos mais novos, também conta com Bluetooth. Para o uso cabeado, o sistema emprega uma interface Micro-USB 2.0

Por fim, O ecossistema é sustentado por um sistema eficiente de gerenciamento de energia. O hardware consegue estabilizar a alimentação do circuito constantemente em 3.3V. Esse mecanismo assegura que o controle opere com performance máxima e sem oscilações mesmo quando as pilhas estão com carga baixa, otimizando o consumo energético global e estendendo a vida útil da carga. 

---

# 2. Análise Técnica do Funcionamento

## 2.1. Principais Módulos do Sistema

Do ponto de vista funcional, o controle pode ser decomposto em cinco módulos que operam de forma integrada e cíclica, coordenados por um microcontrolador central de baixo consumo.

O **módulo de sensoriamento** é responsável por capturar as entradas analógicas e digitais do usuário, com duas tecnologias distintas de posicionamento. Os gatilhos LT/RT utilizam sensores de Efeito Hall, que traduzem a posição do ímã acoplado ao eixo em uma tensão analógica proporcional; já os joysticks duplos utilizam potenciômetros resistivos (contato deslizante), uma solução mais suscetível a desgaste mecânico. Em ambos os casos, o sinal analógico resultante é lido por um conversor analógico-digital (ADC) interno ao microcontrolador. Já os botões de ação, D-Pad, botões de ombro e os botões de navegação (Xbox, Menu, View) formam uma matriz digital que é varrida (*scan*) continuamente para detectar mudanças de estado.

O **módulo de atuação** compreende os quatro motores ERM (dois no chassi e dois nos gatilhos), acionados por *drivers* dedicados que convertem comandos digitais (recebidos do console via protocolo sem fio) em sinais PWM (*Pulse Width Modulation*, ou Modulação por Largura de Pulso) que controlam a intensidade e a duração da vibração. Como o microcontrolador só consegue gerar em seus pinos digitais os níveis lógicos 0V ou 3,3V, o PWM simula uma tensão intermediária alternando rapidamente entre esses dois estados: a fração de tempo em que o sinal permanece "ligado" a cada ciclo (*duty cycle*) determina a tensão média entregue ao motor e, consequentemente, a intensidade da vibração percebida pelo usuário.

O **módulo de controle central** é o microcontrolador que orquestra todo o sistema: realiza a leitura periódica dos sensores (*polling*), empacota esses dados em quadros de telemetria, gerencia o acionamento dos atuadores hápticos conforme comandos recebidos, e supervisiona o estado de energia do dispositivo (incluindo a transição para modos de baixo consumo).

O **módulo de interface** é o conjunto físico de botões, gatilhos e joysticks que traduz a intenção do usuário em sinais elétricos, complementado pela porta P2/P3 de 3,5 mm para áudio analógico.

Por fim, o **módulo de conectividade** integra o rádio proprietário Xbox Wireless (e, nos modelos mais recentes, também Bluetooth Low Energy) para a comunicação sem fio com o console, PC ou dispositivo móvel, além da interface física Micro-USB 2.0 para conexão cabeada e carregamento.

## 2.2. Identificação de Tecnologias Críticas

**Protocolo sem fio proprietário (Xbox Wireless):** diferentemente de um gamepad Bluetooth convencional, o Xbox Wireless opera na mesma faixa de 2,4 GHz do Bluetooth, mas utiliza um canal dedicado com maior potência de transmissão e priorização de Quality of Service (QoS), o que reduz a latência de entrada para menos de 20 ms. Essa é a tecnologia crítica que garante a resposta "instantânea" percebida pelo jogador, sendo um dos principais motivos pelos quais o Bluetooth (introduzido apenas a partir do modelo 1708, junto ao Xbox One S) coexiste com o protocolo proprietário em vez de substituí-lo: o Bluetooth garante compatibilidade ampla com PCs e celulares, enquanto o Xbox Wireless prioriza o desempenho em jogo.

**Firmware embarcado sem sistema operacional completo:** o fabricante não documenta publicamente a arquitetura de firmware do controle, mas, dadas as restrições de custo, consumo energético e determinismo temporal típicas desse tipo de dispositivo, é razoável inferir que ele não execute um sistema operacional embarcado completo (como Linux), e sim um firmware dedicado (*bare-metal* ou baseado em um RTOS leve) que executa um laço cíclico de leitura de sensores, empacotamento e transmissão em alta frequência de amostragem, minimizando a latência ponta a ponta entre a ação do usuário e a resposta no jogo.

**Técnicas de economia de energia:** é documentado que o sistema de gerenciamento de energia estabiliza a alimentação em 3,3V mesmo com a queda de tensão natural das pilhas ao longo da descarga. A partir disso, é plausível inferir o uso de um regulador (LDO ou *buck converter*) entre a fonte e os circuitos sensíveis (MCU, sensores Hall e rádio), embora o componente exato não seja publicamente documentado. Adicionalmente, controles sem fio tipicamente empregam modos de baixo consumo (*sleep/idle*) quando não há atividade do usuário por um determinado período, reduzindo o duty-cycle do rádio e desligando os motores hápticos quando ociosos, o que estende significativamente a autonomia das pilhas/bateria — uma prática comum no setor, ainda que não confirmada especificamente para este modelo.

---

# 3. Proposta de Reprodução com ESP32

## 3.1 Descrição conceitual

A proposta consiste na reprodução das principais funcionalidades do controle do Xbox One utilizando uma **ESP32** como unidade de processamento e o framework **ESP-IDF** para o desenvolvimento do firmware. A comunicação com o computador seria realizada por meio do perfil **Bluetooth HID (Human Interface Device)**, permitindo que o dispositivo seja reconhecido como um gamepad.

Os componentes principais seriam:

| Função | Componente |
|---------|------------|
| Processamento | ESP32 |
| Joysticks | 2x Joystick com 2 eixos e botão |
| Botões | Botões tácteis (A, B, X, Y, LB, RB, Start, Back, D-Pad e Xbox) |
| Comunicação | Bluetooth integrado |

O firmware executaria continuamente as seguintes etapas:

1. Leitura dos joysticks e botões;
2. Filtragem e calibração das entradas;
3. Geração do relatório HID;
4. Envio dos comandos via Bluetooth.

---

### Implementação com Hall Effect

Como melhoria em relação aos joysticks convencionais, propõe-se a utilização da tecnologia **Hall Effect**, eliminando o desgaste dos potenciômetros e reduzindo a ocorrência de *stick drift*.

#### Opção 1 – Joystick Hall Effect

Substituição direta dos joysticks convencionais por módulos que já utilizam sensores Hall internamente.

**Vantagens**

- Fácil implementação;
- Alta precisão;
- Maior durabilidade;
- Elimina praticamente o *stick drift*.

**Desvantagens**

- Maior custo;
- Dependência de módulos específicos.

---

#### Opção 2 – Sensores Hall discretos

Construção do joystick utilizando **Sensores de Efeito Hall Linear** ou **Sensores Hall Analógicos** disponíveis no laboratório, juntamente com pequenos ímãs presos ao eixo do joystick.

**Vantagens**

- Menor custo;
- Maior flexibilidade;
- Melhor compreensão do funcionamento da tecnologia.

**Desvantagens**

- Projeto mecânico mais complexo;
- Necessidade de calibração e posicionamento preciso dos sensores.

---
## 3.2 Diagrama conceitual

```text
                 ┌──────────────────────────┐
                 │          ESP32           │
                 │      Firmware ESP-IDF    │
                 └───────────┬──────────────┘
                             │
              ┌──────────────┴─┬─────────────────┐
              ▼                ▼                 ▼              
         Joystick Esq.       Joystick Dir.      Botões/D-Pad    
            (ADC)                (ADC)             (GPIO)

                             │
                             ▼
                  Processamento dos comandos
                             │
                             ▼
                    Bluetooth HID (BLE)
                             │
                             ▼
                         Computador
```

---

## 3.3 Limitações e desafios

Os principais desafios da implementação são:

- Configuração do perfil Bluetooth HID na ESP32;
- Calibração das leituras analógicas dos joysticks;
- Redução de ruídos nas entradas ADC;
- Projeto mecânico mais complexo ao utilizar sensores Hall discretos.

Além disso, algumas funcionalidades do controle original não seriam implementadas, como vibração, gatilhos com resposta tátil e conector para headset.

---

# 4. Pesquisa Bibliográfica e Tecnológica

## 4.1. Artigos sobre Tecnologias que Viabilizam o Produto

### Artigo 1 - Feedback Tátil com Gamepads Comerciais

**Título:** A Versatile Tool for Haptic Feedback Design Towards Enhancing User Experience in Virtual Reality Applications

**Autores:** Vasilije Bursać e Dragan Ivetić

**Periódico:** *Applied Sciences* (MDPI) - indexado na Scopus e Web of Science

**Ano:** 2025

**Link:** [https://doi.org/10.3390/app15105419](https://doi.org/10.3390/app15105419)

**Resumo:** O trabalho investiga a integração de feedback tátil (*haptic feedback*) de alta fidelidade em ambientes virtuais imersivos utilizando gamepads comerciais amplamente disponíveis no mercado (como o controle de Xbox), que representam uma alternativa de baixo custo em relação a luvas e coletes táteis especializados. Os autores propõem um *framework* e uma ferramenta visual de edição baseada em curvas de animação paramétricas integradas ao Unity. Essa arquitetura de software permite criar bibliotecas de estímulos táteis orientadas a objetos, simulando de forma precisa a amplitude, a frequência, o ritmo e a atenuação temporal das vibrações com base no tipo de interação que o avatar executa no ambiente virtual.

**Relação com Sistemas Embarcados:** Este artigo aborda diretamente o desafio de controle de atuadores físicos (transdutores táteis). Na engenharia de embarcados, a conversão de curvas de intensidade lógica (de 0.0 a 1.0) em vibração física esbarra nas limitações mecânicas dos motores de Massa Rotativa Excêntrica (ERM) comumente usados no controle do Xbox. 

---

### Artigo 2 - Atuadores Hápticos Vestíveis com Comunicação Sem Fio de Baixa Latência

**Título:** Haplets: Finger-Worn Wireless and Low-Encumbrance Vibrotactile Haptic Feedback for Virtual and Augmented Reality

**Autores:** Pornthep Preechayasomboon e Eric Rombokas

**Periódico:** *Frontiers in Virtual Reality* (Frontiers) - indexado na Scopus e Web of Science

**Ano:** 2021

**Link:** [https://doi.org/10.3389/frvir.2021.738613](https://doi.org/10.3389/frvir.2021.738613)

**Resumo:** O artigo apresenta o Haplets, um dispositivo háptico vestível e sem fio, projetado para ser usado nas unhas dos dedos, capaz de renderizar feedback vibrotátil em aplicações de realidade virtual e aumentada. Do ponto de vista de engenharia embarcada, o trabalho detalha o projeto de hardware de cada unidade: um atuador ressonante linear (LRA) acionado por um *driver* de motor dedicado (DRV8838), um SoC sem fio (nRF52832) e uma bateria de célula tipo moeda. Os autores desenvolvem e caracterizam um protocolo de rádio proprietário de baixíssima latência (Enhanced ShockBurst, da Nordic Semiconductor), atingindo latência mediana de 1,50 ms, além de um procedimento de caracterização e compensação de amplitude do atuador em função da frequência e do local de fixação no corpo.

**Relação com Sistemas Embarcados:** Diferente do Artigo 1 (focado em *software* para curvas de animação háptica), este artigo aborda o problema pelo lado do *hardware* e do firmware: como projetar o circuito de acionamento de um motor vibratório (análogo aos ERM do controle Xbox) e como garantir baixa latência na comunicação sem fio entre o microcontrolador e o atuador. Essa metodologia de caracterização de atuador e de projeto de protocolo de rádio de baixa latência é diretamente aplicável ao replicar o subsistema háptico do controle usando um ESP32 e um motor de vibração controlado via PWM.

---

## 4.2. Artigos sobre Aplicação / Uso do Produto

### Artigo 1 - Teleoperação Veicular com Gamepad Xbox

**Título:** Enhanced Teleoperation for Manual Remote Driving: Extending ADAS Remote Control Towards Full Vehicle Operation

**Autores:** İsa Karaböcek, Ege Özdemir e Batıkan Kavak

**Periódico:** *Engineering Proceedings* (MDPI) - Apresentado no ECSA-12

**Ano:** 2025

**Link:** [https://www.mdpi.com/2673-4591/118/1/40](https://www.mdpi.com/2673-4591/118/1/40)

**Resumo:** Este estudo apresenta o desenvolvimento de uma arquitetura modular de teleoperação de veículos em tempo real, integrando o controle humano direto aos sistemas embarcados de assistência ao motorista (ADAS). Os autores implementaram uma interface de controle baseada em um gamepad sem fio padrão de Xbox, conectado via Bluetooth a uma estação cliente. Os dados de entrada analógicos dos gatilhos (aceleração/frenagem) e direcionais (esterço da direção) são capturados via biblioteca SDL, mapeados instantaneamente em pacotes de comunicação assíncronos e transmitidos por meio de protocolo UDP para o computador embarcado do veículo físico que executa uma pilha de software baseada em ROS (*Robot Operating System*).

**Relação com Sistemas Embarcados:** Este artigo é um caso de estudo sobre como interfaces de consumo podem ser aplicadas em malhas de controle embarcado de missão crítica e tempo real. Ele detalha os requisitos de projeto de sistemas *hardware-in-the-loop*, abordando a mitigação de latência fim-a-fim na comunicação sem fio (por meio da migração de Wi-Fi 4 para Wi-Fi 5 nas antenas do veículo de teste).

---

### Artigo 2 – Desenvolvimento de Joysticks com Sensores Hall Effect

**Título:** *Joystick and Lever Design With Hall-Effect Sensors (Rev. A)*

**Autores:** Patrick Simmons e Scott Bryson

**Publicação:** Texas Instruments – Application Report (SLYU064A)

**Ano:** 2023 (Revisão A)

**Link:** https://www.ti.com/lit/an/slyu064a/slyu064a.pdf

**Resumo:**  
O documento apresenta diretrizes para o desenvolvimento de joysticks utilizando sensores de efeito Hall, abordando o funcionamento da tecnologia, o posicionamento de ímãs e sensores, técnicas de calibração, processamento dos sinais e análise das principais fontes de erro. Também são apresentados exemplos de implementação para aplicações em controles de jogos, realidade virtual e sistemas automotivos, destacando as vantagens da tecnologia Hall Effect em relação aos potenciômetros convencionais.

**Relação com Sistemas Embarcados:**  
O documento serve como base para a implementação de joysticks utilizando sensores Hall em sistemas embarcados. Os conceitos apresentados podem ser aplicados na leitura dos sensores pela ESP32 por meio dos conversores ADC, permitindo o desenvolvimento de um controle mais preciso e durável. Além disso, as técnicas de calibração e tratamento dos sinais contribuem para reduzir erros de medição e eliminar problemas como o *stick drift*, tornando a solução mais confiável para aplicações embarcadas.

---

### Artigo 3 – Desenvolvimento de Dispositivos Bluetooth com ESP32

**Título:** *Practical Challenges and Pitfalls of Bluetooth Mesh Data Collection Experiments With ESP32 Microcontrollers*

**Autores:** Marcelo Paulon J. V., Bruno José Olivieri de Souza, Thiago de Souza Lamenza e Markus Endler

**Periódico:** arXiv (pré-publicação científica)

**Ano:** 2022

**Link:** https://arxiv.org/abs/2211.10696

**Resumo:**  
O artigo apresenta o desenvolvimento e a avaliação de uma infraestrutura de comunicação utilizando **Bluetooth Mesh** com microcontroladores ESP32. Os autores implementam uma rede composta por diversos dispositivos embarcados para coleta de dados, analisando aspectos como confiabilidade da comunicação, perda de pacotes, latência, consumo de energia e limitações práticas encontradas durante a implementação. Além dos experimentos em ambiente real, o trabalho compara os resultados obtidos com simulações, identificando desafios e propondo boas práticas para o desenvolvimento de dispositivos Bluetooth baseados na ESP32.

**Relação com Sistemas Embarcados:**  
O artigo demonstra a utilização da ESP32 como plataforma para o desenvolvimento de dispositivos embarcados com comunicação Bluetooth, explorando a integração entre hardware, firmware e protocolos de comunicação. Embora o foco seja Bluetooth Mesh, os conceitos de configuração da pilha Bluetooth, gerenciamento da comunicação sem fio, processamento local e otimização do consumo de energia são diretamente aplicáveis ao desenvolvimento de um controle Bluetooth baseado em ESP32. Essas contribuições servem como referência para a implementação da comunicação sem fio do controle utilizando o framework ESP-IDF.

---

### Artigo 4 - Teleoperação de Robô Móvel via Realidade Virtual com Gamepad Xbox

**Título:** Virtual Reality-Based Interface for Advanced Assisted Mobile Robot Teleoperation

**Autores:** J. Ernesto Solanes, Adolfo Muñoz, Luis Gracia e Josep Tornero

**Periódico:** *Applied Sciences* (MDPI) - indexado na Scopus e Web of Science

**Ano:** 2022

**Link:** [https://doi.org/10.3390/app12126071](https://doi.org/10.3390/app12126071)

**Resumo:** Os autores propõem uma interface de realidade virtual para a teleoperação assistida de robôs móveis, aplicada ao robô Turtlebot3 Burger (equipado com um controlador embarcado ARM Cortex-M7 e um Raspberry Pi como controlador de alto nível). O usuário, utilizando um headset Oculus Quest 2, arrasta uma referência de posição no ambiente virtual por meio de um controle sem fio Xbox conectado via Bluetooth; o robô, por sua vez, combina esse comando com um controlador reativo baseado em campos potenciais (via sensor LiDAR) para desviar de obstáculos automaticamente. Testes de usabilidade com 11 participantes de perfis variados (a maioria sem experiência prévia com VR ou gamepads) indicaram alta usabilidade (90,91/100 na escala SUS) e forte sensação de presença, validando o gamepad como interface preferível aos controles dedicados de VR para tarefas de teleoperação prolongadas.

**Relação com Sistemas Embarcados:** Assim como o Artigo 1 desta seção, trata-se de uma aplicação de teleoperação com o controle Xbox, porém em outro domínio (robótica móvel indoor via headset de VR, em vez do controle direto de um veículo real via ADAS). O artigo evidencia a robustez e a ergonomia do protocolo Bluetooth do controle como interface de alto nível para comandar um sistema embarcado reativo (o laço de navegação por campo potencial roda localmente no Raspberry Pi/OpenCR do robô), reforçando a viabilidade de usar um gamepad Xbox como fonte de referência de movimento em projetos de robótica baseados em ESP32.

---

# 5. Comparativo com Produtos Similares
* **Listagem de pelo menos 5 produtos de mercado relacionados** (mesma categoria), sejam da mesma geração (época) ou de gerações diferentes na história do produto.
* **Tabela comparativa** das principais especificações e características de cada um dos produtos relacionados com o produto sendo estudado.

---

## Referências Bibliográficas

ESPRESSIF SYSTEMS. **LED Control (LEDC) - ESP32**: ESP-IDF Programming Guide. [S. l.]: Espressif, 2026. Disponível em: [https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/peripherals/ledc.html](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/peripherals/ledc.html). Acesso em: 30 jun. 2026.

FEDERAL COMMUNICATIONS COMMISSION. **FCC ID C3K1697**: Microsoft Corporation Xbox One Wireless Controller. Washington, DC: FCC, 2015. Disponível em: [https://fcc.report/FCC-ID/C3K1697](https://fcc.report/FCC-ID/C3K1697). Acesso em: 30 jun. 2026.

IFIXIT. **Xbox One Wireless Controller 1697**. [S. l.]: iFixit, 2026. Disponível em: [https://www.ifixit.com/Device/Xbox_One_Wireless_Controller_1697](https://www.ifixit.com/Device/Xbox_One_Wireless_Controller_1697). Acesso em: 30 jun. 2026.

IFIXIT. **Xbox One Wireless Controller Teardown**. [S. l.]: iFixit, 2026. Disponível em: [https://www.ifixit.com/Teardown/Xbox+One+Wireless+Controller+Teardown/122645](https://www.ifixit.com/Teardown/Xbox+One+Wireless+Controller+Teardown/122645). Acesso em: 30 jun. 2026.

MICROSOFT. **Gamepad e vibração**. [S. l.]: Microsoft Learn, 2024. Disponível em: [https://learn.microsoft.com/pt-br/windows/uwp/gaming/gamepad-and-vibration](https://learn.microsoft.com/pt-br/windows/uwp/gaming/gamepad-and-vibration). Acesso em: 30 jun. 2026.

MICROSOFT. **Xbox Wireless Controller**: product manual. [S. l.]: 1WorldSync. Disponível em: [https://cc.cs.1worldsync.com/inlinecontent/mediaserver/all/a4b/8b3/a4b8b3b058101aca23fec338a4e27d33/original.pdf](https://cc.cs.1worldsync.com/inlinecontent/mediaserver/all/a4b/8b3/a4b8b3b058101aca23fec338a4e27d33/original.pdf). Acesso em: 30 jun. 2026.

WIKIPEDIA. **Xbox Wireless Controller**. [S. l.], 2026. Disponível em: [https://en.wikipedia.org/wiki/Xbox_Wireless_Controller](https://en.wikipedia.org/wiki/Xbox_Wireless_Controller). Acesso em: 30 jun. 2026.

WINDOWS CENTRAL. **Xbox Wireless: Everything you need to know**. [S. l.]: Windows Central, 2026. Disponível em: [https://www.windowscentral.com/xbox-wireless](https://www.windowscentral.com/xbox-wireless). Acesso em: 30 jun. 2026.
