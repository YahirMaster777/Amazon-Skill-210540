# Amazon-Skill-Marvin-210540
Repositorio con el reporte de practica donde se creo un skills personalizable de Alexa

## HISTORIAL DE PRACTICAS
|No.|Nombre|Potenciador|Estatus|
|--|--|--|--|
|26|Creacion de skill de alexa|Pendiente|⭐Activa|

## **Documentación**
---
<div style="display: flex; justify-content: space-between;">
    <img align="left" src="LOGO TIC.png" alt="Logo TIC" width="300" />
    <img align="right" src="LOGO UTXJ.png" alt="Logo UTXJ" width="300"/>
</div>

<div style="text-align: center;">
    <p><strong>UNIVERSIDAD TECNOLÓGICA DE XICOTEPEC DE JUÁREZ</strong></p>
    <p><strong>Materia:</strong> Desarrollo Móvil Integral</p>
    <p><strong>Alumno:</strong> Marvin Yair Tolentino Perez</p>
    <p><strong>Matricula:</strong> 210540</p>
    <p><strong>10 A - IDGS</strong></p>
</div>

**Practica 26:** Creacion de una Alexa Skill  
**Objetivo:** Desarrollar una Skill en Amazon developer para consumir el API del clima de OpenWeather


### **Codigo Fuente:**
- **index.js**
```bash
const Alexa = require('ask-sdk-core');
const axios = require('axios');

const LaunchRequestHandler = {
  canHandle(handlerInput) {
    return Alexa.getRequestType(handlerInput.requestEnvelope) === 'LaunchRequest';
  },
  handle(handlerInput) {
    const speakOutput = 'Esta Skill la creafteo Marvin, Que Ocupas?';

    return handlerInput.responseBuilder
      .speak(speakOutput)
      .reprompt(speakOutput)
      .getResponse();
  }
};

const WeatherIntentHandler = {
  canHandle(handlerInput) {
    return (
      Alexa.getRequestType(handlerInput.requestEnvelope) === 'IntentRequest' &&
      Alexa.getIntentName(handlerInput.requestEnvelope) === 'Clima'
    );
  },
  async handle(handlerInput) {
    try {
      const apiKey = '4cca2aa3b533bb7f69d044c6054dd006'; // Reemplaza con tu clave de API de OpenWeatherMap
      const lat = 20.273999; // Reemplaza con la latitud deseada
      const lon = -97.967833; // Reemplaza con la longitud deseada

      const response = await axios.get(
        `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${apiKey}&units=metric&lang=es`
      );

      const temperature = response.data.main.temp;
      const weatherDescription = response.data.weather[0].description;
      const location = response.data.name;
      const humedad = response.data.main.humidity;
      const tem_max = response.data.main.temp_min;
      //NUEVO
      const sepone = response.data.sys.sunset;
      const sunsetDate = new Date(sepone);
      const sunsetHour = sunsetDate.getHours();
      const sunsetMinutes = sunsetDate.getMinutes();

      

      const speakOutput = `La temperatura actual en ${location} es de ${temperature} grados Celsius y la humedad es de ${humedad} y el sol se oculta a las ${sunsetHour}:${sunsetMinutes}.`;

      return handlerInput.responseBuilder.speak(speakOutput).getResponse();
    } catch (error) {
      console.log('Error al obtener el clima:', error);

      const speakOutput = 'Lo siento, no pude obtener la información del clima en este momento.';
      return handlerInput.responseBuilder.speak(speakOutput).getResponse();
    }
  },
};

const HelpIntentHandler = {
  canHandle(handlerInput) {
    return (
      Alexa.getRequestType(handlerInput.requestEnvelope) === 'IntentRequest' &&
      Alexa.getIntentName(handlerInput.requestEnvelope) === 'AMAZON.HelpIntent'
    );
  },
  handle(handlerInput) {
    const speakOutput = 'Puedes preguntarme sobre el clima diciendo: ¿Cuál es el clima actual?';

    return handlerInput.responseBuilder.speak(speakOutput).getResponse();
  },
};

const CancelAndStopIntentHandler = {
  canHandle(handlerInput) {
    return (
      Alexa.getRequestType(handlerInput.requestEnvelope) === 'IntentRequest' &&
      (Alexa.getIntentName(handlerInput.requestEnvelope) === 'AMAZON.CancelIntent' ||
        Alexa.getIntentName(handlerInput.requestEnvelope) === 'AMAZON.StopIntent')
    );
  },
  handle(handlerInput) {
    const speakOutput = '¡Hasta luego!';

    return handlerInput.responseBuilder.speak(speakOutput).getResponse();
  },
};

const FallbackIntentHandler = {
  canHandle(handlerInput) {
    return (
      Alexa.getRequestType(handlerInput.requestEnvelope) === 'IntentRequest' &&
      Alexa.getIntentName(handlerInput.requestEnvelope) === 'AMAZON.FallbackIntent'
    );
  },
  handle(handlerInput) {
    const speakOutput = 'Lo siento, no puedo ayudarte con eso. Por favor, inténtalo de nuevo.';

    return handlerInput.responseBuilder.speak(speakOutput).getResponse();
  },
};

const SessionEndedRequestHandler = {
  canHandle(handlerInput) {
    return Alexa.getRequestType(handlerInput.requestEnvelope) === 'SessionEndedRequest';
  },
  handle(handlerInput) {
    console.log(`Sesión terminada: ${JSON.stringify(handlerInput.requestEnvelope)}`);
    return handlerInput.responseBuilder.getResponse();
  },
};

const IntentReflectorHandler = {
  canHandle(handlerInput) {
    return Alexa.getRequestType(handlerInput.requestEnvelope) === 'IntentRequest';
  },
  handle(handlerInput) {
    const intentName = Alexa.getIntentName(handlerInput.requestEnvelope);
    const speakOutput = `Acabas de activar la intención ${intentName}.`;

    return handlerInput.responseBuilder.speak(speakOutput).getResponse();
  },
};

const ErrorHandler = {
  canHandle() {
    return true;
  },
  handle(handlerInput, error) {
    console.log(`Error manejado: ${JSON.stringify(error)}`);

    const speakOutput = 'Lo siento, hubo un problema al procesar tu solicitud. Por favor, inténtalo de nuevo.';

    return handlerInput.responseBuilder.speak(speakOutput).getResponse();
  },
};

exports.handler = Alexa.SkillBuilders.custom()
  .addRequestHandlers(
    LaunchRequestHandler,
    WeatherIntentHandler,
    HelpIntentHandler,
    CancelAndStopIntentHandler,
    FallbackIntentHandler,
    SessionEndedRequestHandler,
    IntentReflectorHandler
  )
  .addErrorHandlers(ErrorHandler)
  .lambda();



```

- **interactionModel.json**
```bash
{
    "interactionModel": {
        "languageModel": {
            "invocationName": "clima marvinn",
            "intents": [
                {
                    "name": "AMAZON.CancelIntent",
                    "samples": []
                },
                {
                    "name": "AMAZON.HelpIntent",
                    "samples": []
                },
                {
                    "name": "AMAZON.StopIntent",
                    "samples": []
                },
                {
                    "name": "Clima",
                    "slots": [],
                    "samples": [
                        "hoy va a llover?",
                        "Hara sol hoy?",
                        "Dame el clima?",
                        "Deberia usar sueter?",
                        "Cual es el clima del dia de hoy"
                    ]
                },
                {
                    "name": "AMAZON.NavigateHomeIntent",
                    "samples": []
                }
            ],
            "types": []
        }
    }
}


```


## Resultados:
<div style="display: flex; justify-content: space-between;"> 
    <img src="Imagenes/1.png?raw=true" alt="Evidencia 1" width="350"/> 
    <img src="Imagenes/2.png?raw=true" alt="Evidencia 2" width="350"/> 
</div> <div style="display: flex; justify-content: space-between; margin-top: 10px;"> 
    <img src="Imagenes/3.png?raw=true" alt="Evidencia 4" width="350"/> 
    <img src="Imagenes/4.png?raw=true" alt="Evidencia 5" width="350"/> 
</div>


**Fecha de entrega:** 05 de diciembre de 2024

---

## AUTOR
Elaborado por: Marvin Yair Tolentino Perez [YahirMaster777](https://github.com/YahirMaster777)
