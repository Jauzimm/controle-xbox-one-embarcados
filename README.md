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
* **4 artigos científicos** que detalhem uma ou mais de uma das principais tecnologias que viabilizam a existência do produto.
* **4 artigos científicos** sobre aplicação / uso do produto.

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
