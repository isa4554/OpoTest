# Estudia con Opus · Guía completa del sistema

Sistema de preparación de oposiciones a Auxiliar Administrativo compuesto por dos partes:
- **App de estudio** (`index.html`) — la usa la opositora para practicar
- **Exportador** (`opoadmin-exportar.html`) — lo usa el preparador para subir preguntas a Google Sheets

---

## Cómo funciona el sistema

```
Claude (genera preguntas) → JSON → opoadmin-exportar.html → Google Sheets → App de estudio
```

1. El preparador adjunta un PDF de temario en Claude y pide que genere preguntas
2. Claude devuelve un JSON con las preguntas
3. El preparador pega ese JSON en `opoadmin-exportar.html` y las guarda en Google Sheets
4. La opositora abre la app y las preguntas se cargan automáticamente desde Sheets

---

## Formato de las preguntas en Google Sheets

La hoja debe tener estos **14 encabezados** en la primera fila, en este orden exacto:

| Columna | Encabezado | Descripción |
|---------|-----------|-------------|
| A | Píldora | Frase tipo "¿Sabías que...?" relacionada con la pregunta |
| B | Tema | Bloque temático (máx. 4 palabras) |
| C | Dificultad | `Fácil`, `Media`, `Alta` o `Muy Alta` |
| D | Tipo | `definicion`, `relacion`, `aplicacion`, `excepcion`, `comparacion` o `secuencia` |
| E | Pregunta | El enunciado de la pregunta |
| F | OpciónA | Texto de la opción A |
| G | OpciónB | Texto de la opción B |
| H | OpciónC | Texto de la opción C |
| I | OpciónD | Texto de la opción D |
| J | Correcta | Letra de la respuesta correcta: `A`, `B`, `C` o `D` |
| K | Explicación | Razonamiento de por qué cada opción es correcta o incorrecta |
| L | Referencia | Ubicación en el documento (artículo, epígrafe, página...) |
| M | Documento | Nombre del PDF de origen |
| N | Fecha | Fecha de generación |

> ⚠️ Los valores de **Dificultad** deben escribirse exactamente como aparecen arriba (con tildes y mayúsculas). Si no coinciden, los filtros de la app no funcionarán.

---

## Formato del JSON que genera Claude

Claude genera las preguntas en este formato JSON. Cada vez que pidas preguntas, recibirás un bloque como este:

```json
{
  "preguntas": [
    {
      "pildora": "¿Sabías que el plazo de detención preventiva es de 72 horas?",
      "tema": "Derechos Fundamentales",
      "dificultad": "Alta",
      "tipo": "definicion",
      "texto": "¿Cuál es el plazo máximo de detención preventiva según el art. 17 CE?",
      "opciones": [
        { "letra": "A", "texto": "24 horas", "correcta": false },
        { "letra": "B", "texto": "48 horas", "correcta": false },
        { "letra": "C", "texto": "72 horas", "correcta": true },
        { "letra": "D", "texto": "96 horas", "correcta": false }
      ],
      "explicacion": "El art. 17.2 CE establece un plazo máximo de 72 horas...",
      "referencia": "Constitución Española, art. 17.2"
    }
  ]
}
```

---

## Proceso paso a paso para añadir preguntas

### 1. Generar las preguntas con Claude

1. Abre una conversación en [claude.ai](https://claude.ai)
2. Adjunta el PDF de temario
3. Pide: *"Genera X preguntas de oposición a administrativo basadas en este documento, con el formato JSON de OpoAdmin"*
4. Claude devuelve un bloque JSON con las preguntas

### 2. Exportar a Google Sheets

1. Abre el archivo `opoadmin-exportar.html` en tu navegador (doble clic)
2. **Obtén el token de Google:**
   - Ve a [developers.google.com/oauthplayground](https://developers.google.com/oauthplayground)
   - En el panel izquierdo busca **Google Sheets API v4** y selecciona `spreadsheets`
   - Pulsa **"Authorize APIs"** → selecciona tu cuenta de Google → acepta
   - Pulsa **"Exchange authorization code for tokens"**
   - Copia el valor de **"Access token"** (empieza por `ya29.`)
   - ⚠️ El token caduca en **1 hora** — si ves un error al guardar, obtén uno nuevo
3. Pega el token en el campo correspondiente del exportador
4. Copia el JSON completo que te dio Claude (desde `{` hasta `}`)
5. Pégalo en el campo de texto del exportador
6. Escribe el nombre del documento de origen
7. Pulsa **"Guardar en Google Sheets"**
8. Repite con el siguiente bloque de preguntas si hay más

### 3. Verificar en Google Sheets

Abre tu hoja en [sheets.google.com](https://sheets.google.com) y comprueba que las filas se han añadido correctamente. La opositora verá las preguntas nuevas la próxima vez que abra la app.

---

## Configuración de Google Sheets

Para que la app pueda leer las preguntas, la hoja debe estar:

1. **Compartida como pública:** Compartir → Acceso general → "Cualquiera con el enlace" → Lector
2. **Publicada en la web:** Archivo → Compartir → Publicar en la web → Publicar

El ID de la hoja es el código largo que aparece en la URL entre `/d/` y `/edit`:
```
https://docs.google.com/spreadsheets/d/ESTE-ES-EL-ID/edit
```

---

## Tipos de pregunta

| Tipo | Descripción |
|------|-------------|
| `definicion` | Pregunta sobre el significado de un concepto o término |
| `relacion` | Pregunta sobre la relación entre dos o más conceptos |
| `aplicacion` | Caso práctico o escenario real |
| `excepcion` | Pregunta sobre excepciones o casos especiales de una norma |
| `comparacion` | Pregunta que compara dos elementos, normas u órganos |
| `secuencia` | Pregunta sobre el orden o los pasos de un procedimiento |

---

## Distribución de dificultad recomendada

Al pedir preguntas a Claude, se usa esta distribución:

- **20%** Fácil — conceptos básicos y definiciones directas
- **45%** Media — aplicación de conceptos y relaciones
- **35%** Alta / Muy Alta — casos prácticos, excepciones, interpretación

---

## Estructura de archivos

```
/
├── index.html              → App de estudio (la usa la opositora)
├── opoadmin-exportar.html  → Exportador de preguntas a Sheets (uso del preparador)
└── README.md               → Esta guía
```

---

## ID de Google Sheet

```
1rFfxU1LJ9cuo74DOwl38BFT3qVbhwlgFW7PGa_46bcs
```

---

## Contacto y mantenimiento

- Las preguntas se generan con **Claude** (claude.ai) a partir de los PDFs de temario
- El exportador funciona en cualquier navegador moderno (Chrome, Firefox, Edge, Safari)
- La app de estudio está disponible en GitHub Pages y no requiere instalación
