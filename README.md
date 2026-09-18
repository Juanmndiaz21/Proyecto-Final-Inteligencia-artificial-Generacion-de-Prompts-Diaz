# Proyecto Final: Asistente para Mostrador y Generación Visual en Comercio de Materiales Eléctricos

**Curso:** Inteligencia Artificial: Generación de Prompts  
**Autor:** Juan Manuel Diaz  
**Comisión:** 95920  
**Repositorio GitHub:** [Juanmndiaz21/Proyecto-Final-Inteligencia-artificial-Generacion-de-Prompts-Diaz](https://github.com/Juanmndiaz21/Proyecto-Final-Inteligencia-artificial-Generacion-de-Prompts-Diaz)  
**Notebook del Proyecto:** [`Aistente_Comercio.ipynb`](./Aistente_Comercio.ipynb)

---

## Resumen

Este proyecto nace de una necesidad real del día a día en un comercio de materiales eléctricos: la demora que se genera en el mostrador cuando atendemos a clientes particulares que no tienen conocimientos técnicos. Armar una instalación eléctrica exige respetar normativas de seguridad (sección del cable, amperaje de la térmica, disyuntor diferencial de 30mA). Como el cliente desconoce esto, el vendedor pierde entre 8 y 12 minutos por persona haciendo cálculos en un papel y explicando todo de cero. Además, el negocio no cuenta con un diseñador ni tiempo para armar fotos de producto para publicar ofertas en WhatsApp o redes sociales.

Para resolverlo, desarrollamos una solución práctica con inteligencia artificial dividida en dos partes: un copiloto de texto a texto basado en Gemini que funciona como asistente técnico para el vendedor (interpretando dudas, calculando materiales contra el inventario real y frenando combinaciones peligrosas o consultas incompletas), y un generador de texto a imagen con Nanobanana para armar catálogos y fotos publicitarias de kits eléctricos a costo cero.

---

## Introducción

### 1. Nombre del Proyecto
**Asistente Inteligente de Mostrador y Catálogo Visual para Negocios de Electricidad**

### 2. Presentación del Problema a Abordar
En el local recibimos dos perfiles de clientes: los electricistas matriculados (que ya saben exactamente qué pedir y cuánto llevar) y los particulares o dueños de casa. El problema crítico se da con este segundo grupo:
* **Pérdida de tiempo en mostrador:** El vendedor pasa entre 8 y 12 minutos por cliente haciendo cuentas a mano ("cuántos watts consume el aire", "qué térmica va con qué cable"), lo que en horas pico genera colas y que otros clientes se vayan sin comprar.
* **Riesgo grave de siniestro eléctrico:** Si un cliente pide algo peligroso por desconocimiento (como poner un cable fino de 1.5 mm² con una térmica de 25A para una cocina) y el vendedor no se da cuenta o está apurado, el cable se recalienta dentro de la pared hasta derretirse e incendiarse sin que la térmica corte, violando las normas IRAM y la reglamentación AEA 90364.
* **Falta de recursos visuales:** El comercio pierde ventas por canales digitales porque no tiene tiempo ni presupuesto para contratar fotógrafos o diseñadores que armen imágenes de los productos o kits armados.

### 3. Desarrollo de la Propuesta de Solución
La propuesta utiliza dos modelos de IA integrados al flujo de trabajo del local:
* **Modelo Texto a Texto (Asistente de Mostrador):** El vendedor ingresa en una notebook la consulta tal como se la hace el cliente. El modelo tiene precargado un *System Prompt* con las reglas técnicas inquebrantables de electricidad domiciliaria y recibe el catálogo de stock en formato JSON (`products.json`). El sistema procesa la consulta en etapas:
  * *Etapa V1:* Da una explicación simple, sugiere insumos del stock y recuerda la regla de seguridad.
  * *Etapa V2 (Mejorada):* Si la consulta es incompleta o ambigua, no inventa números y responde pidiendo aclaraciones. Si el cliente pide algo peligroso, frena la venta de inmediato con el aviso `"SISTEMA INCOMPATIBLE / RIESGO ELÉCTRICO"`.
* **Modelo Texto a Imagen (Promoción y Catálogo):** Para promocionar los kits armados en estados de WhatsApp o Instagram, usamos una herramienta gratuita (Nanobanana) mediante un prompt descriptivo que genera fotos comerciales realistas de los productos sobre una mesa de taller.

### 4. Justificación de la Viabilidad del Proyecto
* **Viabilidad técnica:** El código corre en Python local dentro de un Jupyter Notebook (`Aistente_Comercio.ipynb`) consumiendo la API de Gemini (`gemini-3.5-flash`). La respuesta tarda entre 7 y 15 segundos reales, suficiente para que el vendedor lea y asesore al cliente al instante.
* **Viabilidad económica y fuente de costos:**
  * **Costo de IA (Gemini API):** Según la lista oficial de precios de Google AI Studio / Google Cloud (relevada a **Septiembre de 2026**), el costo del modelo Flash es de $0.075 USD por millón de tokens de entrada y $0.30 USD por millón de salida. En nuestras corridas reales, cada consulta consume entre 1.400 y 2.300 tokens, lo que representa un costo aproximado de **$0.0004 USD por consulta** (prácticamente cero, cubierto en su totalidad por el plan gratuito de Google).
  * **Costo de imagen:** Al usar Nanobanana no se pagan suscripciones mensuales de DALL-E ni herramientas pagas.
  * **Infraestructura:** No requiere servidores dedicados; corre en la misma PC que se usa para facturar en el local.

---

## Objetivos del Proyecto

* **Agilizar la atención en mostrador:** Reducir el tiempo promedio de atención técnica de 9 minutos a menos de 2 minutos por persona.
* **Garantizar la seguridad eléctrica:** Evitar que el cliente se lleve combinaciones de cables y térmicas fuera de norma que representen riesgo de incendio.
* **Controlar la ambigüedad:** Lograr que el sistema no adivine ni recomiende materiales a ciegas cuando faltan datos de potencia o consumo.
* **Vender contra stock real:** Cotizar únicamente artículos que existen físicamente en el local con sus precios vigentes.
* **Generar contenido visual a costo cero:** Crear imágenes publicitarias profesionales para redes sin costo de diseño.

---

## Metodología

El desarrollo del proyecto se realizó de forma práctica e iterativa a lo largo de las entregas del curso:

1. **Relevamiento técnico y armado del stock:** Seleccionamos los artículos de mayor rotación (cables unipolares de 1.5 a 6 mm², térmicas de 10A a 32A y disyuntor de 40A/30mA) y volcamos los precios reales de mostrador en `products.json`.
2. **Evolución de los prompts:**
   * *De Preentrega 1 a Preentrega 2:* Pasamos de prompts simples que hacían varias llamadas a un System Prompt unificado con rol de vendedor técnico y reglas duras IRAM/AEA.
   * *De Preentrega 2 a Entrega Final:* Siguiendo la devolución docente, agregamos el manejo estricto de ambigüedades (para que no adivine si el cliente no da datos) y conectamos el inventario en JSON para que devuelva precios y disponibilidad.
3. **Pruebas en Notebook:** Diseñamos casos de prueba típicos de mostrador (caso normal de aire acondicionado, caso ambiguo de taller y caso de combinación peligrosa) para medir tiempos, tokens y respuestas reales.
4. **Generación de imagen:** Diseñamos el prompt de producto en Nanobanana, evaluamos la calidad de la foto generada y la sumamos al repositorio (`imagen.jpg`).
5. **Definición del protocolo humano:** Establecimos las pautas obligatorias de control que debe seguir el vendedor antes de cobrar o entregar materiales.

---

## Herramientas y Técnicas de Fast Prompting Utilizadas

* **Lenguaje y Entorno:** Python 3 ejecutado en Jupyter Notebook ([`Aistente_Comercio.ipynb`](./Aistente_Comercio.ipynb)).
* **Librería de IA:** `google-generativeai`.
* **Modelo de Lenguaje:** Gemini (`gemini-3.5-flash`).
* **Herramienta de Imagen:** Nanobanana (generación por difusión).

### Justificación de las Técnicas de Prompting

* **Role Prompting (Asignación de Rol):** En la primera línea del prompt definimos: *"Sos el vendedor técnico de una casa de materiales eléctricos"*. Esto ubica al modelo en el contexto ferretero argentino, usando términos locales (térmica, disyuntor, tomas, frigorías) y un tono directo de mostrador.
* **System Instructions (Instrucciones de Sistema):** Cargamos las reglas de calibres y amperajes como directivas fijas del sistema (`system_promptV1.txt` y `system_promptV2.txt`). Al estar fijas en el sistema, el modelo no se "olvida" de las normas de seguridad durante la conversación.
* **In-Context Data Injection (Inyección de Stock en JSON):** En la función `probar_mostrador`, adjuntamos el contenido de `products.json` bajo la etiqueta `[STOCK DISPONIBLE]`. De esta forma, el modelo cotiza con precios del local y stock real, sin inventar productos que no tenemos.
* **Manejo de Ambigüedad (Guardrail):** En la Versión 2 agregamos la regla: *"Si el cliente no especifica qué aparatos va a enchufar o su potencia, NO ADIVINES. Respondé SOLO esto: FALTA INFORMACIÓN..."*. Esto evita recomendar cables al azar cuando la consulta es incompleta.
* **Negative Constraints (Restricciones de Seguridad):** Instrucción explícita de bloqueo: si piden una combinación fuera de norma (ej. cable de 1.5 mm² con térmica de 25A), el modelo tiene la orden tajante de responder *"SISTEMA INCOMPATIBLE / RIESGO ELÉCTRICO"* y explicar el motivo técnico.
* **Prompting Descriptivo de Imagen:** Para Nanobanana describimos los elementos físicos reales del mostrador (cable celeste de 2.5mm, térmica blanca, toma de 20A y cinta negra) sobre una mesa rústica con luz de estudio, logrando que parezca una foto publicitaria real de ferretería.

---

## Protocolo Obligatorio de Revisión Humana

> [!IMPORTANT]
> **La IA funciona únicamente como un copiloto de cálculo veloz; jamás reemplaza la responsabilidad del vendedor ni despacha pedidos de forma automática.**

Antes de entregar cualquier presupuesto o material al cliente, el vendedor debe cumplir obligatoriamente con los siguientes pasos:
1. **Lectura crítica en pantalla:** El vendedor lee la respuesta del modelo en la PC del mostrador para corroborar que la sugerencia tenga sentido con lo que pidió el cliente.
2. **Repregunta de validación al cliente:** Si el cliente tiene dudas, el vendedor valida detalles clave que la IA no puede saber a distancia (por ejemplo: *"¿Cuántos metros tenés desde el tablero hasta el aire?"* o *"¿La cañería va por adentro de la pared o por afuera?"*).
3. **Chequeo físico de stock:** Antes de cerrar la cuenta, el empleado confirma visualmente en la estantería que haya rollos y térmicas disponibles de la marca solicitada.
4. **Advertencia de seguridad en mano:** Al momento de entregar la mercadería, el vendedor le repite verbalmente al cliente la regla crítica de seguridad: *"Llevás cable de 2.5 mm², acordate de que la térmica no puede pasar los 16A porque si no el cable no te queda protegido"*.

---

## Implementación y Código en la Notebook

El código ejecutable completo se encuentra en el archivo [`Aistente_Comercio.ipynb`](./Aistente_Comercio.ipynb).

### Estructura del Código:
1. **Configuración del Modelo y Credenciales:** Conexión segura con la API de Gemini usando variables de entorno o `getpass`.
2. **Funciones de Carga de Datos:**
   * `cargar_prompt(ruta)`: Lee los archivos de texto con las instrucciones de sistema.
   * `cargar_stock(ruta)`: Parsea el catálogo `products.json` con los precios y stock del local.
3. **Función Principal `probar_mostrador`:**
   ```python
   def probar_mostrador(prompt_version, consulta):
       modelo = getattr(genai, "GenerativeModel")(
           model_name="gemini-3.5-flash",
           system_instruction=prompt_version
       )
       consulta_completa = f"{consulta}\n\n[STOCK DISPONIBLE]\n{json.dumps(STOCK, indent=2)}"
       inicio = time.time()
       respuesta = modelo.generate_content(consulta_completa)
       tiempo = round(time.time() - inicio, 2)
       # Muestra la respuesta formateada con tiempo y tokens consumidos
   ```

### Generación Visual de Catálogo (Nanobanana)
Para armar la foto de promoción del kit eléctrico en redes sociales se utilizó la herramienta **Nanobanana** con el siguiente prompt:

> **Prompt Utilizado:**  
> *"Fotografía comercial de producto en alta resolución, iluminación de estudio profesional. Sobre una mesa de trabajo de madera rústica se exhibe un kit de materiales eléctricos ordenado: un rollo de cable normalizado color celeste de 2.5mm, una llave termomagnética moderna blanca, un módulo de tomacorriente de 20A y un rollo de cinta aisladora negra. Colores vivos, enfoque hiperrealista, estilo publicitario de ferretería, 8k, renderizado fotorealista."*

#### Resultado Visual Obtenido:
![Kit de Materiales Eléctricos](imagen.jpg)

---

## Resultados y Comparación con Entregas Anteriores

### Comparación con Preentrega 1 y Preentrega 2

A continuación mostramos con datos explícitos cómo fue evolucionando la solución desde la primera entrega hasta el resultado final:

| Parámetro | Preentrega 1 | Preentrega 2 | Entrega Final (Actual) |
| :--- | :--- | :--- | :--- |
| **Enfoque de Prompting** | One-Shot básico sin restricciones duras. | Few-Shot con reglas IRAM y rol técnico. | System Instructions + Grounding con JSON + Guardrail de ambigüedad. |
| **Llamadas a la API** | 2 a 3 llamadas separadas por cliente. | 1 llamada unificada. | 1 llamada unificada y optimizada. |
| **Consumo de Tokens** | ~3.800 a 4.500 tokens por consulta. | ~2.000 tokens promedio. | **1.423 a 2.282 tokens** (medidos en notebook). |
| **Tiempo de Respuesta** | 20 a 25 segundos (por llamadas en cadena). | 10 a 12 segundos estimados. | **7.8 s a 15.1 s** (ejecución real en código). |
| **Manejo de Stock** | Alucinaba marcas y precios que no existían. | Mencionaba marcas genéricas en texto. | **Cruza contra stock real en `products.json`**. |
| **Consultas ambiguas** | Adivinaba o inventaba una potencia promedio. | Intentaba responder asumiendo un taller tipo. | **Frena y pide datos (`FALTA INFORMACIÓN`)**. |
| **Riesgo Eléctrico** | A veces advertía, otras cotizaba igual. | Freno con *"SISTEMA INCOMPATIBLE"*. | **Freno tajante con explicación técnica**. |
| **Generación de Imagen** | No contemplado. | Prompt teórico en texto. | **Imagen real generada en Nanobanana (`imagen.jpg`)**. |
| **Entorno de Entrega** | Texto suelto / boceto. | Documento Markdown con salidas pegadas. | **Jupyter Notebook (`Aistente_Comercio.ipynb`) funcionando**. |

---

### Pruebas Reales Ejecutadas en la Notebook

#### Caso 1: Consulta Normal (Instalación de Aire Acondicionado)
* **Consulta:** `"Necesito instalar un aire de 3500 frigorías. ¿Qué llevo?"`
* **Tiempo y tokens:** **15.17 segundos | 2.282 tokens**.
* **Resultado del Asistente:**
  * **Explicación Simple:** Explica que el aire consume entre 1200W y 1600W y necesita circuito independiente desde el tablero general con cable de 2.5 mm² y térmica bipolar de 16A.
  * **Lista de Insumos (con precios de stock):**
    * 3 rollos de Cable Unipolar 2.5 mm² a **$25.000** c/u (stock: 35 disponibles).
    * 1 Llave Termomagnética Bipolar 16A a **$12.000** (stock: 25 disponibles).
    * 1 Disyuntor Diferencial Bipolar 40A 30mA a **$45.000** (stock: 12 disponibles).
  * **Regla de Seguridad:** Recuerda que para cable de 2.5 mm² la térmica jamás debe superar los 16A para evitar incendios, y que el disyuntor es obligatorio.

#### Caso 2: Consulta Ambigua / Incompleta (Datos faltantes)
* **Consulta:** `"Hola, dame cable y térmica para enchufar unas cosas en el taller."`
* **Tiempo y tokens:** **7.8 segundos | 1.423 tokens**.
* **Resultado del Asistente:**
  > `"FALTA INFORMACIÓN: Por favor, consultale al cliente qué equipos va a conectar o su consumo en Watts para poder calcular la sección del cable."`
* **Evaluación:** Demuestra el funcionamiento del *Guardrail*. No inventó calibres para el taller, ahorró tokens (1.423 vs 2.282) y forzó al vendedor a pedir los datos necesarios.

#### Caso 3: Consulta de Riesgo Eléctrico (Combinación prohibida)
* **Consulta típica:** Pedir cable de 1.5 mm² con una térmica de 25A o 32A para conectar aparatos pesados.
* **Resultado del Asistente:** Frena la cotización con el mensaje `"SISTEMA INCOMPATIBLE / RIESGO ELÉCTRICO"`, explicando que el cable de 1.5 mm² solo tolera hasta 10A de térmica (solo iluminación) y que poner una térmica más grande provocará el derretimiento del conductor sin que la térmica corte.

---

## Conclusiones

1. **Objetivos cumplidos:** Logramos transformar un cuello de botella de 9 minutos de mostrador en una consulta resuelta en menos de 2 minutos (entre la respuesta de la IA y la validación del vendedor).
2. **Evolución y aprendizaje:** La comparación entre la Preentrega 1, la 2 y esta versión final demuestra que la clave no es pedirle a la IA que "haga todo", sino ponerle **límites estrictos**:
   * Sin System Instructions, el modelo alucina precios.
   * Sin control de ambigüedad, el modelo adivina potencias peligrosas.
   * Con el inventario en JSON y frenos de seguridad, se convierte en una herramienta comercial seria y confiable.
3. **Valor para el negocio:** El comercio gana en dos frentes concretos: atiende más rápido en el local con menor margen de error técnico, y dispone de fotos profesionales para vender por redes sociales sin haber gastado un solo peso en licencias de software o diseñadores.
4. **El rol irremplazable del humano:** La IA demostró ser excelente para calcular y recordar tablas técnicas rápido, pero el criterio final, la inspección del material y el trato con el vecino siguen estando en manos del vendedor.

---

## Referencias

1. **Asociación Electrotécnica Argentina (AEA):** *Reglamentación para la Ejecución de Instalaciones Eléctricas en Inmuebles* (Norma AEA 90364 - Edición 2006/actualizada).
2. **Instituto Argentino de Normalización y Certificación (IRAM):** *Norma IRAM 2183: Conductores unipolares de cobre para instalaciones fijas interiores*.
3. **Lista de Precios de Materiales Eléctricos:** Lista de mostrador y distribución mayorista del comercio, relevada a **Septiembre de 2026** (valores expresados en pesos argentinos ARS).
4. **Google Cloud / Google AI Studio:** *Gemini API Pricing & Documentation* ([https://ai.google.dev/pricing](https://ai.google.dev/pricing)), valores consultados a **Septiembre de 2026**.
5. **Nanobanana Community:** *Plataforma de generación visual basada en modelos de difusión latente*.
6. **Material del Curso:** *Conceptos de Fast Prompting, Role Playing, Few-Shot y Guardrails*.
