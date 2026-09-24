🎤 Karaoke Ubicuo
![UC3M](https://img.shields.io/badge/Universidad-Carlos%20III%20de%20Madrid-blue)
![Curso](https://img.shields.io/badge/Curso-2024--2025-green)
![SUS Score](https://img.shields.io/badge/SUS%20Score-82.1%20%28Excelente%29-brightgreen)
Karaoke Ubicuo es un sistema distribuido y multimodal diseñado para eliminar las interrupciones físicas durante las reuniones de karaoke en casa. Combina visión por computador, detección de movimiento en dispositivos móviles, reconocimiento de voz y búsqueda semántica con IA en local para permitir un control 100% manos libres y a distancia.
---
👥 Autores
Iván González Portero
Vanesa Elena Ionescu
Jaime Sánchez Sánchez
Asignatura: Sistemas Interactivos y Ubicuos  
Grupo/Equipo: Equipo 5 — UC3M (2024-2025)
---
💡 Motivación y Problema
En una fiesta de karaoke doméstica, la inmersión y la energía social se rompen frecuentemente cuando hay que cambiar de canción, ajustar el volumen o pausar la música. Esto obliga al usuario a soltar su bebida/micrófono, abandonar el grupo y desplazarse hasta el ordenador para interactuar con el teclado o el ratón a oscuras.
Karaoke Ubicuo resuelve los 4 pain points principales identificados:
Restricción física: Manos ocupadas con bebidas o el micrófono.
Ineficiencia: Tiempo perdido en desplazamientos innecesarios.
Aislamiento: Separación física del cantante respecto al grupo.
Atmósfera rota: Pérdida del clímax y de la energía social.
---
🛠️ Interacciones Multimodales
El sistema distribuye el control entre la pantalla principal (TV con webcam) y un mando móvil inteligente accesible por navegador web:
#	Interacción	Tipo	Tecnología	Función
1	🛑 Palma abierta	Gesto visual	Webcam + MediaPipe Hands	Pausar / Parar reproducción
2	✅ OK (Pulgar + Índice)	Gesto visual	Webcam + MediaPipe Hands	Reanudar reproducción
3	❌ Muñecas cruzadas (X)	Gesto visual	Webcam + MediaPipe Hands	Acción destructiva / Salir
4	👍 Pulgar arriba	Gesto visual	Webcam + MediaPipe Hands	Reiniciar canción actual
5	📱 Agitar el móvil (Shake)	Sensor móvil	DeviceMotion API	Saltar a la siguiente canción
6	🎙️ Comandos de voz	Voz	Web Speech API	Control e inserción por voz (Push-to-Talk y modo continuo)
7	🔍 Búsqueda semántica	IA en cliente	Transformers.js (`all-MiniLM-L6-v2`)	Buscar por concepto/ánimo ("pon algo bailable")
---
🏗️ Arquitectura del Sistema
El proyecto sigue un patrón centralizado basado en eventos (Event-Driven Architecture) que garantiza que la cola y el estado de reproducción se mantengan sincronizados en tiempo real entre todos los dispositivos conectados.
```text
               +----------------------------------+
               |         Servidor Node.js         |
               |      (Socket.IO - Central)       |
               +----------------+-----------------+
                                |
             sync-state broadcast / eventos
                                |
        +-----------------------+-----------------------+
        |                                               |
        v                                               v
+-------------------------------+               +-------------------------------+
|         Cliente TV            |               |        Cliente Móvil          |
|  - Visión: MediaPipe Hands    |               |  - Detección Shake (Motion)   |
|  - Reproductor YouTube IFrame |               |  - Reconocimiento de Voz      |
|  - UI Principal               |               |  - Búsqueda Semántica Local   |
+-------------------------------+               +-------------------------------+
```
Principales Tecnologías
Backend: Node.js, Express, Socket.IO.
Cliente TV / Visión: MediaPipe Hands (21 landmarks de mano), YouTube IFrame Player API.
Cliente Móvil: DeviceMotion API (filtro de 3 picos > $25,\text{m/s}^2$ en ventana de $800,\text{ms}$), Web Speech API.
Procesamiento de Lenguaje / IA: Transformers.js ejecutan el modelo `all-MiniLM-L6-v2` directamente en el navegador del cliente para realizar cálculos de similitud coseno sin depender de APIs de pago o conexión a la nube.
---
🚀 Instalación y Despliegue
Requisitos previos
Node.js (v16.x o superior) y npm.
Dispositivos (TV/PC y teléfono móvil) conectados a la misma red WiFi local.
Navegador web moderno con soporte para Web Speech API y DeviceMotion (ej. Chrome, Safari).
Pasos de instalación
Clonar el repositorio:
```bash
   git clone https://github.com/usuario/karaoke-ubicuo.git
   cd karaoke-ubicuo
   ```
Instalar dependencias:
```bash
   npm install
   ```
Iniciar el servidor:
```bash
   npm start
   ```
Acceder desde los dispositivos:
En la TV / PC Principal: Abre la URL local `http://localhost:3000` (o la IP local del servidor).
En el Móvil: Escanea el código QR en pantalla o accede a `http://<IP-LOCAL-SERVIDOR>:3000/mobile`.
---
📊 Evaluación y Resultados
El sistema fue validado mediante un estudio con usuarios en entornos reales de prueba aplicando elicitación formativa pre-test, cuestionario SUS (System Usability Scale) y dinámicas I Like / I Wish / What If.
Puntuación SUS Media: 82.1 / 100 (Clasificación: Excelente).
Efectividad: 6 de las 7 tareas obtuvieron un 100% de tasa de éxito.
Desplazamiento físico: 0%. Ninguno de los participantes necesitó acercarse a la pantalla ni abandonar su posición durante las pruebas.
---
📄 Licencia
Este proyecto se distribuye con fines académicos bajo la Licencia MIT.

Readme hecho por Gemini
