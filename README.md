# Simulador de certificaciones

Simulador de exámenes de certificación en una sola página estática: `index.html` trae todas las preguntas incluidas y no necesita servidor ni base de datos.

## Exámenes disponibles
| Examen | Preguntas |
|---|---|
| AZ-900 Azure Fundamentals | 172 |
| DP-900 Azure Data Fundamentals | 185 |
| AI-901 Azure AI Fundamentals | 727 |
| AI-103 Azure AI Apps and Agents Developer Associate | 2,211 |
| Databricks Apache Spark Developer | 114 |
| Databricks Data Engineer Associate | 244 |
| Databricks Data Engineer Professional | 53 |

AI-200, Generative AI Engineer, PCEP y PCAP aparecen como *Próximamente*.

## Probarlo en tu computadora
Haz doble clic en `index.html`, o sírvelo localmente:

```bash
python -m http.server 8000
# abre http://localhost:8000
```

## Publicarlo en internet (elige una opción)

### Opción A: Azure Storage, sitio web estático
```bash
az storage account create -n <cuenta> -g <grupo> -l eastus --sku Standard_LRS --kind StorageV2
az storage blob service-properties update --account-name <cuenta> --static-website --index-document index.html
az storage blob upload -c '$web' --account-name <cuenta> -f index.html -n index.html --content-type "text/html; charset=utf-8" --overwrite
az storage account show -n <cuenta> -g <grupo> --query "primaryEndpoints.web" -o tsv
```

### Opción B: Azure Static Web Apps (plan gratuito)
Sube esta carpeta a un repositorio de GitHub y crea un Static Web App apuntando a ella (app location `/`, sin build). O, con la CLI:
```bash
npm i -g @azure/static-web-apps-cli
swa deploy . --env production
```

### Opción C: GitHub Pages
Sube `index.html` a un repositorio y activa Settings → Pages → Deploy from branch (`main`, carpeta raíz).

### Opción D: Netlify Drop
Arrastra la carpeta a https://app.netlify.com/drop

## Cómo se guarda el progreso
- Siempre se guarda en el navegador (`localStorage`).
- **Sincronizar entre dispositivos:** en la pantalla de inicio, sección *Guardar tu progreso*, conecta un token de GitHub con permiso `gist`. El progreso se guarda en un gist privado tuyo (`progreso-ai901.json`) y se combina automáticamente en cada dispositivo donde conectes el mismo token.
- Sin GitHub, usa **Exportar progreso** e **Importar progreso**.

## Traducción
El botón **Ver traducción** muestra el enunciado y las alternativas en español debajo del texto original, en la misma página. Las traducciones de todas las preguntas vienen incluidas dentro de `index.html`, así que aparecen al instante, sin abrir otra página, sin depender de ningún servicio externo y también sin internet. Los nombres de servicios, clases, métodos y el código se dejan en inglés, igual que en el examen. Las explicaciones de las respuestas se muestran en su idioma original (inglés).

## Ayuda por pregunta
Durante la prueba, el botón **Pedir ayuda** abre un panel con información para razonar la pregunta, sin darte la respuesta. Muestra un resumen del tema de la pregunta y definiciones breves de los conceptos que aparecen en el enunciado y en las alternativas. En computadora el panel aparece a la derecha; en el celular, debajo de la pregunta. Solo se muestra si lo pides, y se cierra al pasar a la siguiente pregunta.

## Varios exámenes
- Es una sola página para todos tus exámenes. La pantalla de inicio muestra el catálogo de certificaciones agrupado por Microsoft, Databricks y Python Institute. Las que ya tienen banco de preguntas se pueden abrir; las demás aparecen como *Próximamente*. Desde el simulador, **← Todas las certificaciones** te devuelve al catálogo.
- Cada examen tiene su propio progreso, su historial de intentos, sus filtros y su puntaje mínimo de aprobación.
- El mismo token de GitHub sincroniza todos los exámenes en el mismo gist privado. El archivo de exportación también los incluye todos.
- Tu progreso de AI-901 de la versión anterior se conserva tal cual.
- Para agregar un examen, envía el archivo de preguntas (PDF, JSON o Excel) y se genera un `index.html` nuevo con el examen incluido, junto con sus traducciones.

## Actualizar las preguntas
Las preguntas están dentro de `index.html`, en `<script id="bank" type="application/json">`. Su formato es:
`{"exams":[{"id":"AI-901", "name":"...", "pass":700, "data":{"cats":[...], "topics":[...], "q":[[id, índiceCategoría, índiceTema, enunciado, [opción A..E], [índices correctos], explicación], ...]}, "tre":{id:{"q":..., "o":[...]}}}, ...]}`
