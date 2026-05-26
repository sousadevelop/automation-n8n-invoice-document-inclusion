# Automatización n8n para inclusión de documentos de factura

## Visión general

Este repositorio documenta un workflow n8n exportado como `workflow/workflow.sanitized.json`. La automatización procesa adjuntos recibidos por Gmail, analiza documentos de factura con Google Gemini, sube archivos a Google Drive y registra datos estructurados en Google Sheets.

El archivo del workflow no fue modificado durante esta organización.

## Objetivo del workflow

Automatizar la entrada de facturas recibidas por correo: detectar mensajes de Gmail con adjuntos, extraer datos estructurados, guardar el archivo original en Google Drive y escribir los datos en Google Sheets.

## Problema resuelto

El workflow reduce tareas manuales como abrir correos, descargar archivos, leer facturas, renombrar documentos, subir archivos y completar hojas de cálculo.

## Arquitectura

Secuencia principal:

1. `Gmail Trigger` monitorea Gmail y descarga adjuntos.
2. `If` verifica si existe un adjunto.
3. `Split Out` separa los binarios.
4. `Loop Over Items` procesa cada adjunto.
5. `Analyze document` usa Google Gemini para extraer JSON.
6. `Edit Fields` normaliza el objeto devuelto.
7. `If1` verifica si el documento es corporativo.
8. `Merge` y `Merge1` combinan datos estructurados y binarios.
9. `Upload file` y `Upload file1` suben el archivo a Google Drive.
10. `Append row in sheet` escribe los datos en Google Sheets.

Consulta [assets/diagrams/workflow-architecture.mmd](../../assets/diagrams/workflow-architecture.mmd).

## Nodos n8n utilizados

- `n8n-nodes-base.gmailTrigger`
- `n8n-nodes-base.if`
- `n8n-nodes-base.splitOut`
- `n8n-nodes-base.splitInBatches`
- `@n8n/n8n-nodes-langchain.googleGemini`
- `n8n-nodes-base.noOp`
- `n8n-nodes-base.set`
- `n8n-nodes-base.merge`
- `n8n-nodes-base.googleDrive`
- `n8n-nodes-base.googleSheets`

## Integraciones necesarias

- Gmail OAuth2
- Google Gemini o Google PaLM API
- Google Drive OAuth2
- Google Sheets OAuth2

## Configuración esperada

Usa `templates/env.example` como referencia de placeholders. Los valores reales deben configurarse solo en n8n o en un gestor seguro de secretos.

La configuración esperada incluye credenciales de Gmail, Gemini/PaLM, Drive y Sheets, además de la hoja de cálculo, pestaña y carpetas de Drive de destino.

## Importación en n8n

1. Abre n8n.
2. Importa `workflow/workflow.sanitized.json`.
3. Revisa todos los nodos antes de activar.
4. Reconecta las credenciales locales.
5. Sustituye referencias de hoja, pestaña y carpetas por valores del entorno.
6. Prueba con adjuntos ficticios.
7. Activa solo después de validar salidas y logs.

## Credenciales

Crea las credenciales directamente en n8n. No edites el JSON sanitizado para insertar secrets. Prueba cada credencial antes de activar el workflow.

## Ejemplos

- [Ejemplo de entrada](../../examples/sample-input.md)
- [Ejemplo de salida](../../examples/sample-output.md)

## Limitaciones

- La calidad de salida depende del documento y del modelo.
- El prompt espera JSON puro; una respuesta inválida puede romper pasos posteriores.
- Las referencias específicas del entorno deben revisarse después de importar.
- Consulta [SECURITY_NOTES.md](../../SECURITY_NOTES.md) para los riesgos detectados.

## Próximos pasos

- Reemplazar referencias específicas por placeholders seguros.
- Agregar capturas actualizadas.
- Probar facturas ficticias corporativas y personales.
- Mejorar el manejo de errores para IA, Drive y Sheets.
