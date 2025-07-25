🤖 BOT DE TELEGRAM – Asistente de Heber (n8n)
Este workflow de n8n implementa un bot de Telegram que actúa como asistente personal virtual de Heber Daniel Ismael Rocha. Está diseñado para:

Responder preguntas relacionadas con la operatoria de Cohen.

Clasificar mensajes entre "importantes" y "no importantes".

Derivar a Heber solo si es estrictamente necesario.

Usar IA (GPT-4 mini) con memoria y herramientas externas.

Mandar mails si alguien pide hablar directamente con él.

📦 Estructura General
El bot está compuesto por estos bloques principales:

1. Telegram Trigger
Escucha todos los mensajes entrantes desde el bot de Telegram.

Extrae el chat.id y el texto del mensaje.

2. Clasificación de Importancia
Basic LLM Chain + modelo OpenAI (gpt-4.1-mini) analizan si el mensaje exige hablar con Heber.

El prompt define reglas estrictas: solo mensajes que piden explícitamente hablar con él se marcan como "importante".

El output se parsea con Structured Output Parser.

3. Switch
Si el mensaje es "no importante":

Se envía al nodo AI Agent.

Este responde con tono sarcástico/cordial y estilo argentino.

Usa memoria de contexto (Simple Memory) y herramientas como Wikipedia y Calculator si hace falta.

Si el mensaje es "importante":

Se dispara un formulario de Telegram para que el remitente complete sus datos (nombre, apellido, motivo, celular).

Una vez respondido, se envía un email automático a heber_rocha@outlook.com.ar con los datos.

Finalmente, el bot contesta: “Listo la puta madre, ya le avisé a mi creador…”

🧠 AI Agent
El nodo AI Agent:

Tiene un prompt extremadamente detallado (ver JSON).

Está diseñado para responder con información de:

Tesorería, Títulos, Back Office, Mandatos, Aperturas, Retenciones, Fondos, Cauciones, Cheques, etc.

Usa respuestas sarcásticas, pero útiles, para filtrar ruido y evitar molestar a Heber.

Usa herramientas:

OpenAI Chat

Simple Memory

Wikipedia

Calculator


🛠 Herramientas utilizadas
Nodo	Función
telegramTrigger	Escucha mensajes entrantes
telegram	Envía mensajes simples o formularios
gmail	Envía correos si alguien quiere hablar con Heber
langchain.chainLlm	Clasifica la importancia del mensaje
langchain.agent	Responde usando IA con memoria, herramientas y prompt personalizado
langchain.memoryBufferWindow	Mantiene contexto de la conversación por chat.id
langchain.outputParserStructured	Asegura que la IA clasifique entre "importante" y "no importante"
switch	Decide si responder con la IA o escalar la solicitud

🧪 Ejemplo de conversación
Usuario:

Hola, ¿me podés decir los horarios de transferencia de títulos?

Bot:

¿Cómo no, rey? Transferencias de títulos locales: 10:00 a 17:30. Exterior, armate la carta y que firme el cliente, y ojo con Compliance. No me hagas repetir.

Usuario:

Pasame con Heber, no quiero hablar con vos.

Bot:

Bueno bueno la concha tuya… Completá este formulario...

📤 Luego envía mail a Heber con los datos del remitente.

📎 Consideraciones
El bot no responde si el workflow está desactivado.

El email se envía siempre que se etiquete como "importante".

Toda la lógica se basa en el campo "output.Mensaje" que debe ser "importante" o "no importante".

No hay fallback para errores de conexión o mensajes sin texto (se podría agregar).


✅ Requisitos
Cuenta de Telegram con bot configurado

API Key de OpenAI (GPT-4 mini)

Credenciales de Gmail configuradas

Acceso a n8n con nodos LangChain habilitados

