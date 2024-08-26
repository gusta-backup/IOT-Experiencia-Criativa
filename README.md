# Componentes no WOKWI

**ESP1 (Monitoramento dos ambientes e controle das comportas):**
* 2 Sensores de Temperatura e Umidade (um para cada ambiente)
* 2 Servo Motores (um para cada comporta)
* 1 Slide Switch (para controle de envio de mensagens para ESP2)

**ESP2 (Alerta e controle):**
* 1 Buzzer (para emitir alerta sonoro)
* 1 LED (para piscar a cada 1 segundo em caso de alerta)
* 1 Slide Switch (para priorizar o desligamento dos alarmes)

# Lógica do Sistema

**ESP1:**
* Monitora os sensores de temperatura e umidade
* O código para ações subsequentes só era executado se houver uma mudança da medida anterior.
* Se a temperatura atingir 60°C:
* Abre a comporta do Servo Motor 1 em 50° para o primeiro ambiente.
* Abre a comporta do Servo Motor 2 em 180° para o segundo ambiente.
* Se a umidade for inferior a 20% em qualquer um dos sensores, o ESP 1 envia uma mensagem via MTTP para a ESP2 emitir um alerta via buzina e led.
* Se o Slide Switch da ESP1 estiver ativado, desativa o envio de mensagens e retorna os servos à posição inicial (0°).

**ESP2:**
* Ao receber a mensagem da ESP1 indicando baixa umidade, aciona o buzzer e faz o LED piscar a cada segundo.
* Se o Slide Switch da ESP2 estiver ativado, os alarmes são desativados, independentemente das mensagens recebidas da ESP1.

**Utilização de Threads:**
* A execução dos buzzers e do alarme no segundo ESP32 são executadas em uma thread separada, para permitir que o sistema possa lidar com múltiplas tarefas ao mesmo tempo.
