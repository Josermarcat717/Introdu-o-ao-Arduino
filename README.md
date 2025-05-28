# Introdu-o-ao-Arduino
Podem copiar o Código em C para executar o circuito

// Definição das variáveis globais
int trigger_pin = 2;  // Pino que envia o sinal ultrassônico
int echo_pin = 3;     // Pino que recebe o sinal refletido
int buzzer_pin = 10;  // Pino que controla o buzzer (alerta sonoro)
int time;             // Variável para armazenar o tempo de retorno do sinal
int distance;         // Variável para armazenar a distância calculada

void setup() {
    Serial.begin(9600);  // Inicia a comunicação serial a 9600 bps

    // Define os modos dos pinos: saída para trigger, entrada para echo e saída para buzzer ou seja os output's e input's
    pinMode(trigger_pin, OUTPUT);
    pinMode(echo_pin, INPUT);
    pinMode(buzzer_pin, OUTPUT);
}

void loop() {
    // Emissão do pulso ultrassônico
    digitalWrite(trigger_pin, HIGH);  // Envia um sinal de alta tensão
    delayMicroseconds(10);            // Mantém o sinal por 10 microssegundos
    digitalWrite(trigger_pin, LOW);   // Desliga o sinal

    // Mede o tempo que o sinal leva para retornar após ser refletido por um objeto
    time = pulseIn(echo_pin, HIGH);  // Obtém o tempo em microssegundos

    // Cálculo da distância com base no tempo e na velocidade do som (aprox. 343 m/s)
    distance = (time * 0.034) / 2;  // Multiplicação para converter microssegundos em cm

    // Verifica a distância medida e ativa o buzzer se necessário
    if (distance <= 10) {  // Caso o objeto esteja a 10 cm ou menos
        Serial.println("Door Open");  // Exibe "porta aberta" no monitor serial
        Serial.print("Distance = ");  // Exibe a distância calculada
        Serial.println(distance);
        digitalWrite(buzzer_pin, HIGH);  // Ativa o buzzer como alerta
        delay(500);  // Mantém o alerta por 500 ms
    } else {  // Caso o objeto esteja a mais de 10 cm
        Serial.println("Door Closed");  // Exibe "porta fechada"
        Serial.print("Distance = ");
        Serial.println(distance);
        digitalWrite(buzzer_pin, LOW);  // Desativa o buzzer
        delay(500);  // Espera 500 ms antes de repetir o loop
    }
}
