# Proyecto Final: Optimización Comercial y Asistencia Visual mediante Prompt Engineering en un Comercio de Materiales Eléctricos

**Autor:** Juan Manuel Diaz  
**Comisión:** 95920  

---

## Resumen
Este proyecto implementa un sistema de asistencia basado en inteligencia artificial y *Fast Prompting* diseñado para optimizar la atención en el mostrador de un comercio de materiales eléctricos. A través de un modelo de lenguaje (Gemini), el sistema interpreta las dudas de los clientes particulares, genera explicaciones claras, arma listas exactas de materiales y aplica reglas estrictas de seguridad (normas IRAM/AEA) para prevenir riesgos eléctricos.

Adicionalmente, el proyecto soluciona la falta de recursos gráficos del local generando automáticamente *prompts* descriptivos que luego se introducen en herramientas gratuitas de generación de imágenes (Nanobanana) para armar catálogos visuales.

## Introducción
**Presentación del problema a abordar:**  
En el mostrador atendemos clientes particulares que buscan soluciones rápidas pero desconocen las normativas eléctricas. El vendedor pierde entre 8 y 12 minutos por persona realizando cálculos a mano y explicando conceptos desde cero. Además, el comercio no cuenta con tiempo ni personal para diseñar imágenes promocionales, perdiendo oportunidades de venta por redes.

**Desarrollo de la propuesta de solución:**  
La solución consiste en un copiloto de IA para el vendedor. Mediante un modelo texto-texto, el vendedor ingresa la consulta. La IA (pre-configurada con un *System Prompt* de reglas eléctricas) evalúa la viabilidad, advierte incompatibilidades, pide más datos si hay ambigüedad y devuelve la lista de materiales. En paralelo, se utiliza un modelo texto-imagen para obtener una foto publicitaria del producto.

## Justificación de la viabilidad del proyecto
El proyecto es 100% viable a nivel técnico y económico. Al incorporar tablas fijas de calibres eléctricos en las instrucciones de sistema, logramos que el LLM no invente medidas peligrosas. El código corre de forma local consumiendo la API de Gemini 3.8 Flash, cuyo costo operativo es inferior a $0.0004 USD por consulta (siendo gratuito para este entorno de pruebas). No requiere inversión en servidores.

## Objetivos
* **General:** Agilizar el proceso de venta en mostrador y mejorar la presencia digital del comercio mediante automatización con IA.
* **Específicos:**
  * Reducir el tiempo de atención técnico de 9 minutos a menos de 2 minutos.
  * Evitar la venta de combinaciones de materiales que representen riesgo eléctrico.
  * Implementar un sistema de detección de ambigüedad para no presupuestar sin datos.
  * Generar recursos visuales promocionales a costo cero.

## Metodología
El desarrollo se llevó a cabo de forma iterativa. Comenzamos con un enfoque *One-Shot*, pero para garantizar la seguridad eléctrica, evolucionamos a *Few-Shot / System Instructions* aplicando restricciones duras. 

**Protocolo de Revisión Humana (Obligatorio):**  
La IA funciona únicamente como calculadora rápida. El vendedor es el responsable final de leer la salida en la pantalla, confirmar que la sugerencia sea lógica, revisar el stock físico y recién ahí entregar el presupuesto al cliente.

## Herramientas y tecnologías
* **Lenguaje:** Python ejecutado en Jupyter Notebook.
* **Modelo Texto-Texto:** API de Gemini. Utilizamos técnicas de *Fast Prompting* (Role-playing y System Instructions) para anclar el comportamiento a reglas estrictas.
* **Modelo Texto-Imagen:** Nanobanana. Utilizamos un prompt hiperdescriptivo para lograr fotorrealismo en insumos ferreteros.

## Implementación y Archivos del Repositorio
La implementación técnica y las pruebas de rendimiento se encuentran en el archivo Jupyter Notebook adjunto en este repositorio:
* `Proyecto_Final_JuanManuelDiaz.ipynb`

## Resultado Visual (Nanobanana)
*Prompt utilizado:* "Fotografía comercial de producto en alta resolución, iluminación de estudio profesional. Sobre una mesa de trabajo de madera rústica se exhibe un kit de materiales eléctricos ordenado: un rollo de cable normalizado color celeste de 2.5mm, una llave termomagnética moderna blanca, un módulo de tomacorriente de 20A y un rollo de cinta aisladora negra. Colores vivos, enfoque hiperrealista, estilo publicitario de ferretería, 8k, renderizado fotorealista."

![Imagen Generada del Kit Eléctrico](imagen.jpg)

## Resultados y Conclusiones
La implementación logra la solución esperada: el sistema procesa las consultas en un promedio de 1.8 a 2.5 segundos. El asistente recomienda correctamente los insumos, detecta de forma tajante el riesgo eléctrico y exige información cuando la consulta es ambigua. 
Pasamos de una dependencia total del conocimiento humano (que generaba demoras) a un modelo donde la IA hace el trabajo pesado de cálculo, dejando al vendedor en su rol ideal: el de validador y cerrador de la venta.# Proyecto-Final-Inteligencia-artificial-Generacion-de-Prompts-Diaz
