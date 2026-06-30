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

Agora sobre componentes e sensores, o controle adota uma arquitetura focada em imersão e durabilidade. O sistema tátil é composto por uma topologia de quatro motores de massa rotativa excêntrica (ERM): dois motores principais integrados ao chassi (esquerdo e direito) para vibrações globais e dinâmicas, e dois micromotores independentes posicionados nos gatilhos esquerdo (LT) e direito (RT) para respostas táteis localizadas na ponta dos dedos. Além dos joysticks analógicos duplos, os próprios gatilhos utilizam sensores magnéticos de Efeito Hall, uma tecnologia que elimina o contato mecânico e evita o desgaste por atrito.

Para o comando digital, o projeto combina diferentes engenharias de acionamento para otimizar o tato e a vida útil do hardware. Os botões de ação (A, B, X, Y) utilizam membranas para um clique mais macio e confortável, já o direcional (D-Pad) emprega chapa metálicas para garantir respostas rápidas e aquele feedback firme e audível. Complementando o conjunto, os botões de ombro superiores utilizam micro-switches, priorizando a máxima precisão.

Por fim, a interface do sistema é gerenciada por botões de navegação dedicados. O "Botão Xbox", posicionado centralmente, atua como o guia principal para ligar o console e acessar rapidamente o painel de controle, enquanto os botões "Menu" e "View" desempenham funções contextuais, permitindo a interação com interfaces de software, gerenciamento de menus e navegação dinâmica dentro dos jogos.

## 1.3. Tecnologias de Comunicação e Controle Embarcadas
Na parte inferior abriga uma entrada de 3,5 mm (P2/P3), garantindo conectividade analógica direta para fones de ouvido e headsets.

Para jogar sem fios, ele usa uma conexão exclusiva super rápida e, nos modelos mais novos, também conta com Bluetooth. Para o uso cabeado, o sistema emprega uma interface Micro-USB 2.0

Por fim, O ecossistema é sustentado por um sistema eficiente de gerenciamento de energia. O hardware consegue estabilizar a alimentação do circuito constantemente em 3.3V. Esse mecanismo assegura que o controle opere com performance máxima e sem oscilações mesmo quando as pilhas estão com carga baixa, otimizando o consumo energético global e estendendo a vida útil da carga. 

---

# 2. Análise Técnica do Funcionamento
* **Principais módulos do sistema** (sensores, atuadores, controle, interface, conectividade);
* **Identificação de tecnologias críticas** (por exemplo, protocolos sem fio, sistemas operacionais embarcados, técnicas de economia de energia).

---

# 3. Proposta de Reprodução com ESP32
* **Descrição conceitual** de como as funcionalidades poderiam ser implementadas usando a ESP32 e componentes compatíveis com o ecossistema ESP-IDF;
* **Diagrama conceitual do sistema** (pode ser esquemático ou em diagrama de blocos);
* **Limitações e desafios esperados**.

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

# 5. Comparativo com Produtos Similares
* **Listagem de pelo menos 5 produtos de mercado relacionados** (mesma categoria), sejam da mesma geração (época) ou de gerações diferentes na história do produto.
* **Tabela comparativa** das principais especificações e características de cada um dos produtos relacionados com o produto sendo estudado.

---

## Referências Bibliográficas

FEDERAL COMMUNICATIONS COMMISSION. **FCC ID C3K1697**: Microsoft Corporation Xbox One Wireless Controller. Washington, DC: FCC, 2015. Disponível em: [https://fcc.report/FCC-ID/C3K1697](https://fcc.report/FCC-ID/C3K1697). Acesso em: 30 jun. 2026.

IFIXIT. **Xbox One Wireless Controller 1697**. [S. l.]: iFixit, 2026. Disponível em: [https://www.ifixit.com/Device/Xbox_One_Wireless_Controller_1697](https://www.ifixit.com/Device/Xbox_One_Wireless_Controller_1697). Acesso em: 30 jun. 2026.

MICROSOFT. **Gamepad e vibração**. [S. l.]: Microsoft Learn, 2024. Disponível em: [https://learn.microsoft.com/pt-br/windows/uwp/gaming/gamepad-and-vibration](https://learn.microsoft.com/pt-br/windows/uwp/gaming/gamepad-and-vibration). Acesso em: 30 jun. 2026.

MICROSOFT. **Xbox Wireless Controller**: product manual. [S. l.]: 1WorldSync. Disponível em: [https://cc.cs.1worldsync.com/inlinecontent/mediaserver/all/a4b/8b3/a4b8b3b058101aca23fec338a4e27d33/original.pdf](https://cc.cs.1worldsync.com/inlinecontent/mediaserver/all/a4b/8b3/a4b8b3b058101aca23fec338a4e27d33/original.pdf). Acesso em: 30 jun. 2026.

WIKIPEDIA. **Xbox Wireless Controller**. [S. l.], 2026. Disponível em: [https://en.wikipedia.org/wiki/Xbox_Wireless_Controller](https://en.wikipedia.org/wiki/Xbox_Wireless_Controller). Acesso em: 30 jun. 2026.
