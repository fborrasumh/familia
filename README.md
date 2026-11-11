# FamilIA

Sistema de aprendizaje de **Medicina Familiar y Comunitaria** (asignatura 1960, 5.º del Grado en Medicina, Universidad Miguel Hernández de Elche), construido a partir de su guía docente del curso 2026-2027. Aplicación web de un solo fichero.

**Usar la app:** https://fborrasumh.github.io/familia/

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.24128865.svg)](https://doi.org/10.5281/zenodo.24128865)

**Idiomas:** español (por defecto), inglés y portugués; selector en la barra superior (o `?lang=en` / `?lang=pt` en la URL).

> **Versión 1.0, contenido pendiente de revisión.** Las 40 preguntas, los 3 pacientes simulados y sus explicaciones se han redactado para esta app a partir de la guía docente y aún no los ha revisado el profesorado coautor. No son preguntas de examen reales ni sustituyen a la bibliografía ni a la docencia.

## Qué hace

- **Ruta adaptativa.** Diagnóstico inicial con una pregunta por cada uno de los 8 resultados de aprendizaje (texto literal de la guía). Después, el código elige la siguiente pregunta con reglas deterministas y explicables: primero los repasos espaciados vencidos (lo fallado antes), y si no hay, el resultado de aprendizaje con menos dominio, intercalando temas. El dominio se estima con un modelo tipo Elo; el estudiante siempre ve por qué le toca cada pregunta.
- **Consulta con pacientes simulados (Carmen, Antonio y Rosa).** El estudiante entrevista hablando (reconocimiento de voz del navegador) o escribiendo; el paciente responde en voz alta (síntesis de voz del sistema). Sin clave de IA, el paciente responde con su guion; con clave, la IA lo interpreta y el código descarta las respuestas que usan términos de diagnóstico o cifras que no están en el guion. Al terminar, el código analiza la cobertura de 14 temas de la entrevista clínica (recibimiento, fase exploratoria con ideas, preocupaciones y expectativas, y fase resolutiva) y muestra los puntos docentes del caso.
- **Comentario de la IA (opcional)** sobre la entrevista, con citas de lo que dijo el estudiante que el código verifica; las frases que ponen notas se eliminan.
- **Laboratorio de decisiones:** valores predictivos y cocientes de probabilidad con frecuencias naturales, NNT/RAR/RRR y recuento de comprimidos; retos con solución calculada que reconocen errores típicos (confundir VPP y sensibilidad, usar la RRR, contar los comprimidos restantes).
- **Simulacro** estratificado por resultados de aprendizaje con la regla de la guía (4 opciones, −0,33 por error) y tabla del valor esperado de contestar al azar.
- **Progreso:** dominio por resultado de aprendizaje, calculadora orientativa de la nota final con la regla de promediado de la guía, informe Word, CSV de respuestas y JSON anónimo.
- **Guía docente:** ficha, resultados de aprendizaje literales, relación con la app, observaciones sobre la guía y exportación del banco a Moodle (GIFT, con la penalización en cada opción incorrecta).

## Qué hace el código y qué hace la IA

El código hace la ruta, la corrección, los cálculos, la cobertura de la entrevista y la verificación de citas. La IA (con la clave de OpenAI, Google Gemini o Anthropic Claude de cada persona) solo interpreta al paciente y, si se pide, comenta la entrevista. **La IA nunca pone notas.** Todo funciona sin clave y sin conexión, salvo la IA y el dictado por voz.

## Privacidad

- Respuestas, progreso y consultas se guardan solo en el navegador (IndexedDB).
- Con clave de IA, las preguntas al paciente salen hacia el proveedor elegido con correos, DNI/NIE y teléfonos enmascarados, y antes del primer envío se muestra lo que sale.
- El dictado por voz usa el servicio de reconocimiento del navegador: en Chrome y Edge el audio puede procesarse en servidores de Google o Microsoft.
- Los pacientes son ficticios: no deben escribirse datos de pacientes reales.
- El JSON exportable lleva un seudónimo, las preguntas respondidas (acierto, fecha) y la cobertura de las consultas; nunca texto libre ni nombres. Compartirlo es voluntario.

## Límites

- Contenido clínico pendiente de revisión por el profesorado.
- El banco tiene 40 preguntas (el examen real, 80 + 10). En varias preguntas la opción correcta es la más completa, lo que puede servir de pista.
- La detección de temas de la entrevista es por palabras clave: es orientativa.
- El dominio es una estimación del modelo, no una nota. La calculadora interpreta que el examen teórico (test + casos) es una sola actividad del 70 %; la guía no lo precisa.
- Que la práctica de recuperación y espaciada funcione en general (Dunlosky et al., 2013, https://doi.org/10.1177/1529100612453266) no demuestra que esta app mejore el rendimiento. Para publicar resultados hace falta aprobación del comité de ética (OEP de la UMH), consentimiento informado y, idealmente, un diseño preregistrado con grupo de comparación.
- El reconocimiento de voz solo funciona en navegadores que lo ofrecen (Chrome, Edge); en los demás se escribe.

## Pruebas realizadas

Lógica pura en Node (oráculo de frecuencias naturales para Bayes, 300 retos, determinismo de la ruta y del simulacro) y 46 comprobaciones en Chromium con IA, micrófono y voz simulados: verificación de citas inventadas, respuestas del paciente que se diagnostican o inventan cifras, error de saldo, privacidad del envío, descargas (Word abierto con python-docx, también sin CDN), persistencia, tres idiomas, sin red, CPU ralentizada ×4, móvil sin desborde y cero infracciones WCAG 2.2 AA (axe-core) en todas las pantallas e idiomas. No se ha probado con claves reales ni con usuarios.

## Adoptarla en otra universidad

Cambia `src/curriculum.js` (guía), `src/banco.js` (preguntas) y `src/casos.js` (pacientes), y ejecuta `python3 build.py`.

## Autoría

- Fernando Borrás Rocher (Universidad Miguel Hernández de Elche) · ORCID [0000-0002-5519-4573](https://orcid.org/0000-0002-5519-4573)
- Domingo Luis Orozco Beltrán (Universidad Miguel Hernández de Elche) · ORCID [0000-0002-8231-2635](https://orcid.org/0000-0002-8231-2635)
- María Concepción Carratalá Munuera (Universidad Miguel Hernández de Elche) · ORCID [0000-0002-1303-6294](https://orcid.org/0000-0002-1303-6294)
- Salvador Pertusa Martínez (Universidad Miguel Hernández de Elche)
- Juan Manuel Zazo Menargues (Universidad Miguel Hernández de Elche)
- Vicente Francisco Gil Guillén (Universidad Miguel Hernández de Elche) · ORCID [0000-0003-3793-9786](https://orcid.org/0000-0003-3793-9786)
- Manuel Sánchez Mollá (Universidad Miguel Hernández de Elche)
- Enrique Hermida Campa (Universidad Miguel Hernández de Elche)
- Adriana López-Pineda (Universidad Miguel Hernández de Elche) · ORCID [0000-0002-2117-0178](https://orcid.org/0000-0002-2117-0178)
- Rafael Piñol Pleguezuelos (Universidad Miguel Hernández de Elche)
- Manuel Carlos Salas Tomás (Universidad Miguel Hernández de Elche)
- Concepción Sánchez Botella (Universidad Miguel Hernández de Elche)
- Rafael Gandulla Soriano (Universidad Miguel Hernández de Elche)
- María José Ramos Maestre (Universidad Miguel Hernández de Elche)
- Inmaculada Candela García (Universidad Miguel Hernández de Elche)
- Ana Margarita González Ferreño (Universidad Miguel Hernández de Elche)

Construida a partir de la Guía Docente de la asignatura 1960 Medicina Familiar y Comunitaria (curso 2026-2027). Roles (CRediT): Fernando Borrás Rocher, conceptualización y software; profesorado de la asignatura, recursos (guía docente) y validación del contenido (pendiente). Todos los autores aceptan la licencia MIT.

## Cómo citar

Borrás Rocher, F.; Orozco Beltrán, D.; Carratalá Munuera, M.; Pertusa Martínez, S.; Zazo Menargues, J.; Gil Guillén, V.; Sánchez Mollá, M.; Hermida Campa, E.; López-Pineda, A.; Piñol Pleguezuelos, R.; Salas Tomás, M.; Sánchez Botella, C.; Gandulla Soriano, R.; Ramos Maestre, M.; Candela García, I.; González Ferreño, A. (2026). *FamilIA* (v1.0.0) [Software]. DOI: [10.5281/zenodo.24128865](https://doi.org/10.5281/zenodo.24128865)

## Licencia

MIT. Véase [LICENSE](LICENSE). El contenido docente (preguntas y casos) se publica también bajo MIT a petición de los autores.
