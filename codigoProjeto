#include <ArduinoJson.h>

#define POTENCIOMETRO_BATERIA_PIN A0 // Pino analógico para o potenciômetro da porcentagem de bateria
#define POTENCIOMETRO_VIDA_UTIL_PIN A1 // Pino analógico para o potenciômetro da vida útil da bateria

#define BATERIA_MINIMA 0   // Mínimo valor do potenciômetro da porcentagem de bateria (0%)
#define BATERIA_MAXIMA 100 // Máximo valor do potenciômetro da porcentagem de bateria (100%)

#define VIDA_UTIL_MINIMA 0  // Mínimo valor do potenciômetro da vida útil da bateria (0%)
#define VIDA_UTIL_MAXIMA 100 // Máximo valor do potenciômetro da vida útil da bateria (100%)

#define BATERIA_LIMITE_MINIMA 35   // Limite mínimo da porcentagem de bateria para carregar (35%)
#define BATERIA_LIMITE_MEDIA 70    // Limite médio da porcentagem de bateria (70%)
#define BATERIA_LIMITE_ALTA 85     // Limite alto da porcentagem de bateria (85%)
#define VIDA_UTIL_LIMITE_CRITICA 25  // Limite mínimo da vida útil da bateria em estado crítico (25%)
#define VIDA_UTIL_LIMITE_REVISAO 50  // Limite mínimo da vida útil da bateria para revisão (50%)
#define VIDA_UTIL_LIMITE_BOA 75      // Limite mínimo da vida útil da bateria para "Saúde da bateria boa" (75%)

void setup() {
  Serial.begin(9600);  // Inicializa a comunicação serial com o Node-RED
}

void loop() {
  // Leitura dos potenciômetros
  int leituraPotenciaBateria = analogRead(POTENCIOMETRO_BATERIA_PIN);
  int leituraPotenciaVidaUtil = analogRead(POTENCIOMETRO_VIDA_UTIL_PIN);

  // Mapeamento dos valores dos potenciômetros para a faixa de 0 a 100
  int porcentagemBateria = map(leituraPotenciaBateria, 0, 1023, BATERIA_MINIMA, BATERIA_MAXIMA);
  int vidaUtilBateria = map(leituraPotenciaVidaUtil, 0, 1023, VIDA_UTIL_MINIMA, VIDA_UTIL_MAXIMA);

  // Criar um objeto JSON
  StaticJsonDocument<200> json;

  // Adicionar dados ao objeto JSON
  json["porcentagemBateria"] = porcentagemBateria;

  // Verificação da porcentagem de bateria
  if (porcentagemBateria <= BATERIA_LIMITE_MINIMA) {
    json["cargaBateria"] = "Carga Da Bateria: Baixa! Carro precisa ser recarregado.";
  } else if (porcentagemBateria <= BATERIA_LIMITE_MEDIA) {
    json["cargaBateria"] = "Carga Da Bateria: Metade! Carro com carga moderada.";
  } else if (porcentagemBateria <= BATERIA_LIMITE_ALTA) {
    json["cargaBateria"] = "Carga Da Bateria: Alta! Carro com carga suficiente.";
  } else {
    json["cargaBateria"] = "Carga Da Bateria: Cheia! Carro pronto para uso.";
  }

  // Verificação da vida útil da bateria
  if (vidaUtilBateria <= VIDA_UTIL_LIMITE_CRITICA) {
    json["vidaUtilBateria"] = vidaUtilBateria;
    json["mensagemVidaUtil"] = "Em estado critico! Substituicao urgente.";
  } else if (vidaUtilBateria <= VIDA_UTIL_LIMITE_REVISAO) {
    json["vidaUtilBateria"] = vidaUtilBateria;
    json["mensagemVidaUtil"] = "Precisando de revisao.";
  } else if (vidaUtilBateria <= VIDA_UTIL_LIMITE_BOA) {
    json["vidaUtilBateria"] = vidaUtilBateria;
    json["mensagemVidaUtil"] = "Boa.";
  } else {
    json["vidaUtilBateria"] = vidaUtilBateria;
    json["mensagemVidaUtil"] = "Saudavel.";
  }

  // Serializar o objeto JSON em uma string
  String jsonString;
  serializeJson(json, jsonString);

  // Enviar a string JSON pela porta serial
  Serial.println(jsonString);

  delay(2000); // Aguardar 2 segundos antes de fazer a próxima verificação
}
