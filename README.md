# 💡 Luminária Temporizadora Dinâmica - Arquitetura de 3 Botões

## Visão Geral

Este projeto é uma **Luminária Temporizadora Dinâmica** baseada em Arduino, projetada para gerenciar sessões de estudo ou leitura.
Esta versão implementa uma interface de **3 Botões de Controle** e adiciona recursos avançados de feedback visual e movimento gradual do atuador.

### 🌟 Destaques desta Versão

1.  **Interface de 3 Botões:** Controle dedicado para Liga/Desliga, Seleção de Tempo (Ciclo) e Iniciar/Cancelar.
2.  **Movimento Controlado (Varredura):** O Servo Motor não se move instantaneamente, mas sim com uma velocidade controlada, simulando um movimento suave de abertura/fechamento.
3.  **Feedback de Início:** Ao iniciar a contagem, o Mini LED selecionado pisca **10 vezes** e depois permanece fixo, indicando que o timer está ativo.

---

## ⚙️ Funcionalidades e Fluxo Operacional

| Módulo | Ação | Detalhes da Implementação |
| :--- | :--- | :--- |
| **Liga/Desliga (Pin 2)** | Controle Geral | Liga o sistema (LED Principal ON, Servo para $0^\circ$). Desliga o sistema (LEDs OFF, Servo para $90^\circ$). |
| **Seleção de Tempo (Pin 4)** | Ciclo de Seleção | Pressionar avança para o próximo tempo (10 $\rightarrow$ 20 $\rightarrow$ ... $\rightarrow$ 50 $\rightarrow$ 10 min). |
| **Iniciar/Cancelar**  (Pin 3) | Cancelamento Imediato | Se pressionado enquanto o timer está ativo, a contagem é **cancelada**, o servo fecha, e a seleção avança para o próximo tempo. |
| **Iniciar/Cancelar (Pin 3)** | Controle do Timer | **Timer Inativo:** Inicia a contagem, emite bipe, e inicia o ciclo de 10 piscadas no Mini LED. **Timer Ativo:** Cancela a contagem, emite bipe e fecha o servo. |
| **Mini LEDs (A0-A4)** | Indicadores | Ficam **fixos** no modo de seleção. Ficam **fixos** durante a contagem (após as 10 piscadas). |
| **Movimento** | Fechamento Parcial | Servo move para $45^\circ$ **2 minutos antes** do fim. |
| **Fim da Sessão** | Encerramento Total | Ao final do tempo, LEDs OFF, bipe duplo e Servo para $90^\circ$ (Fechado). |

---

## 🛠️ Diagrama de Hardware (Conexões)

Esta versão utiliza um único Servo Motor e três botões principais de controle, além dos 5 Mini LEDs.

| Componente | Tipo | Pino Arduino | Observações |
| :--- | :--- | :--- | :--- |
| **Botão Liga/Desliga** | Entrada (INPUT_PULLUP) | **2** | Controle principal. |
| **Botão Iniciar/Cancelar** | Entrada (INPUT_PULLUP) | **3** | Inicia e Interrompe a contagem. |
| **Botão Seleção Tempo** | Entrada (INPUT_PULLUP) | **4** | Clicla entre 10, 20, 30, 40, 50 min. |
| **Mini LEDs** | Saída (Digital) | **A0, A1, A2, A3, A4** | Indicadores de tempo selecionado. |
| **LED Principal** | Saída (Digital) | **9 (PWM)** | Requer MOSFET/Transistor. |
| **Buzzer** | Saída (Digital) | **10 (PWM)** | Feedback sonoro. |
| **Servo Motor** | Saída (Digital) | **11 (PWM)** | **Único Servo**. |

### Configuração de Ângulos

| Estado | Ângulo do Servo (Pino 11) | Observações |
| :--- | :--- | :--- |
| **Aberto/Ativo** | $0^\circ$ (`ANGULO_ABERTO`) | Luminária totalmente aberta. |
| **Aviso Parcial** | $45^\circ$ (`ANGULO_PARCIAL`) | Fecha parcialmente 2 minutos antes. |
| **Fechado/Desligado** | $90^\circ$ (`ANGULO_FECHADO`) | Luminária totalmente fechada. |

---

## 💻 Estrutura e Controle de Tempo

O código utiliza um loop não bloqueante (`millis()`) para controlar o *countdown* e o pisca-pisca dos LEDs.

* **Tempos Padrão:** $10, 20, 30, 40, 50$ minutos.
* **Velocidade do Servo:** Controlada pela constante `INTERVALO_VARREDURA_MS = 10`, que define o atraso em milissegundos a cada passo de $1^\circ$ de movimento.
* **Log de Debug:** O tempo restante é impresso no Monitor Serial (9600 baud) a cada **1 segundo**.
<img width="749" height="518" alt="image" src="https://github.com/user-attachments/assets/a2b371da-b369-40c5-9af4-d3f63ccdacd9" />

