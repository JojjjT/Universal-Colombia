# 🎵 Data Lake NoSQL — Disquera Online
**Universal Studios Colombia | Taller Práctico de Bases de Datos NoSQL con MongoDB**

---

## 📋 Descripción General del Proyecto

Este repositorio contiene la implementación completa de un **Data Lake NoSQL** para una tienda de discos online, desarrollado como taller práctico de la asignatura de Bases de Datos NoSQL en la Universidad Externado de Colombia. El proyecto modela un catálogo musical usando **MongoDB Atlas**, donde cada álbum es una **colección** y cada canción es un **documento**.

**Stack tecnológico:** Python 3.11 · PyMongo 4.17.0 · MongoDB Atlas · Rich 15.0.0 · Jupyter Notebook

---

## 🎤 Artistas y Álbumes Seleccionados

| Artista | Álbum | Año | Género |
|---------|-------|-----|--------|
| Lady Gaga | The Fame | 2008 | Dance-pop / Electropop |
| Diomedes Díaz | El Regreso del Cóndor | 1992 | Vallenato |
| BTS | Map of the Soul: 7 | 2020 | K-Pop / Hip-Hop |
| Michael Jackson | Thriller | 1982 | Pop / R&B / Funk |
| Queen | A Night at the Opera | 1975 | Rock Progresivo |
| Ivy Queen | Diva | 2003 | Reggaeton / Latin Hip-Hop |

---

## 🗂️ Estructura del Repositorio

```
main/                              ← Tarea 1: Base de datos inicial
├── Taller_NoSQL_Universal_Studios.ipynb
├── images/
│   ├── lady_gaga_the_fame.jpg
│   ├── diomedes_el_regreso_del_condor.jpg
│   ├── bts_map_of_the_soul_7.jpg
│   ├── michael_jackson_thriller.jpg
│   ├── queen_a_night_at_the_opera.jpg
│   └── ivy_queen_diva.jpg
├── disquera_tarea1.csv
└── bson_tarea1/
    ├── lady_gaga_the_fame.json      ← colección completa
    ├── lgf_001.json … lgf_005.json  ← documentos individuales
    └── [demás colecciones y documentos]

actualizaciones/                   ← Tarea 2: Inserción de canciones
├── disquera_tarea2.csv
└── bson_tarea2/

eliminaciones/                     ← Tarea 3: Eliminaciones
├── disquera_tarea3.csv
└── bson_tarea3/
```

---

## 📐 Modelo de Datos

### Reglas de Modelado
- Cada **álbum** → una **colección** en MongoDB
- Cada **canción** → un **documento** dentro de su colección
- Todo álbum y toda canción posee un campo `_id` único

### Campos Obligatorios por Documento (Canción)

```json
{
  "_id": "lgf_001",
  "titulo": "Just Dance",
  "anio_salida": 2008,
  "autor": "Lady Gaga",
  "id_imagen_portada": "images/lady_gaga_the_fame.jpg"
}
```

### Campos Opcionales Agregados

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `genero` | string | Género musical de la canción |
| `duracion_seg` | entero | Duración en segundos |
| `numero_pista` | entero | Número de pista en el álbum |
| `popularidad` | entero | Puntuación de popularidad (0–100) |

---

## 🎯 Tarea 1 — Creación de la Base de Datos Inicial
**Rama:** `main`

Se crearon 6 colecciones en MongoDB Atlas (base de datos `BD_DisqueraUniversal`), una por artista/álbum. Cada colección fue cargada con las **5 canciones más representativas** del álbum seleccionado usando `insert_many()`. Antes de cada inserción se ejecuta `collection.drop()` para garantizar idempotencia.

### ✅ Output de conexión

```
✅ Pinged your deployment. You successfully connected to MongoDB!
🗄️  Base de datos activa: BD_DisqueraUniversal
✅ Datos de los 6 álbumes definidos correctamente.
```

### ✅ Output de inserción

```
🚀 Insertando álbumes en MongoDB...

📀 Lady Gaga — The Fame: 5 canciones insertadas en 'lady_gaga_the_fame'
📀 Diomedes Díaz — El Regreso del Cóndor: 5 canciones insertadas en 'diomedes_diaz_el_regreso_del_condor'
📀 BTS — Map of the Soul: 7: 5 canciones insertadas en 'bts_map_of_the_soul_7'
📀 Michael Jackson — Thriller: 5 canciones insertadas en 'michael_jackson_thriller'
📀 Queen — A Night at the Opera: 5 canciones insertadas en 'queen_a_night_at_the_opera'
📀 Ivy Queen — Diva: 5 canciones insertadas en 'ivy_queen_diva'
```

### ✅ Tablas de verificación

```
                    🎵 Lady Gaga — The Fame
┏━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━┳━━━━━━━━━━━━┳━━━━━━━┳━━━━━━━━━━━━━┓
┃ _id    ┃ Título                 ┃ Año  ┃ Género     ┃ Pista ┃ Popularidad ┃
┡━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━╇━━━━━━━━━━━━╇━━━━━━━╇━━━━━━━━━━━━━┩
│ lgf_001│ Just Dance             │ 2008 │ Dance-pop  │   1   │     95      │
│ lgf_002│ LoveGame               │ 2008 │ Dance-pop  │   2   │     88      │
│ lgf_003│ Poker Face             │ 2008 │ Synth-pop  │   3   │     99      │
│ lgf_004│ Paparazzi              │ 2008 │ Dance-pop  │   4   │     90      │
│ lgf_005│ Beautiful Dirty Rich   │ 2008 │ Electropop │   5   │     82      │
└────────┴────────────────────────┴──────┴────────────┴───────┴─────────────┘

                🎵 Diomedes Díaz — El Regreso del Cóndor
┏━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━┳━━━━━━━━━━━┳━━━━━━━┳━━━━━━━━━━━━━┓
│ erc_001│ El Verdadero Culpable          │ 1992 │ Vallenato │   1   │     92      │
│ erc_002│ Eso No Es Na                 │ 1992 │ Vallenato │   2   │     84      │
│ erc_003│ No Más Cadenas               │ 1992 │ Vallenato │   3   │     79      │
│ erc_004│ El Regreso del Cóndor        │ 1992 │ Vallenato │   4   │     85      │
│ erc_005│ La Vida                      │ 1992 │ Vallenato │   5   │     77      │
└────────┴─────────────────────────────┴──────┴───────────┴───────┴─────────────┘

                    🎵 BTS — Map of the Soul: 7
┏━━━━━━━━┳━━━━━━━━━━━━━━┳━━━━━━┳━━━━━━━━━━┳━━━━━━━┳━━━━━━━━━━━━━┓
│ bts_001│ ON           │ 2020 │ K-Pop    │   1   │     96      │
│ bts_002│ Black Swan   │ 2020 │ Art pop  │   2   │     92      │
│ bts_003│ Boy With Luv │ 2019 │ K-Pop    │   3   │     98      │
│ bts_004│ Filter       │ 2020 │ Latin pop│   4   │     88      │
│ bts_005│ Ego          │ 2020 │ Hip-Hop  │   5   │     85      │
└────────┴──────────────┴──────┴──────────┴───────┴─────────────┘

                  🎵 Michael Jackson — Thriller
┏━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━┳━━━━━━━━━━━━━━━━━┳━━━━━━━┳━━━━━━━━━━━━━┓
│ mj_001 │ Thriller                  │ 1982 │ Funk            │   1   │     100     │
│ mj_002 │ Billie Jean               │ 1982 │ Post-disco      │   2   │     100     │
│ mj_003 │ Beat It                   │ 1982 │ Hard rock / Pop │   3   │     99      │
│ mj_004 │ Wanna Be Startin' Somethin'│ 1982 │ Funk            │   4   │     90      │
│ mj_005 │ Human Nature              │ 1982 │ Soft rock       │   5   │     88      │
└────────┴───────────────────────────┴──────┴─────────────────┴───────┴─────────────┘

               🎵 Queen — A Night at the Opera
┏━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━┳━━━━━━┳━━━━━━━━━━━━━━━━━━━┳━━━━━━━┳━━━━━━━━━━━━━┓
│ qn_001 │ Bohemian Rhapsody   │ 1975 │ Progressive rock  │   1   │     100     │
│ qn_002 │ Love of My Life     │ 1975 │ Ballad            │   2   │     92      │
│ qn_003 │ You're My Best Friend│ 1975│ Pop rock          │   3   │     88      │
│ qn_004 │ '39                 │ 1975 │ Folk rock         │   4   │     78      │
│ qn_005 │ The Prophet's Song  │ 1975 │ Progressive rock  │   5   │     75      │
└────────┴─────────────────────┴──────┴───────────────────┴───────┴─────────────┘

                    🎵 Ivy Queen — Diva
┏━━━━━━━━┳━━━━━━━━━━━━━━━┳━━━━━━┳━━━━━━━━━━━┳━━━━━━━┳━━━━━━━━━━━━━┓
│ iq_001 │ Quiero Bailar │ 2003 │ Reggaeton │   1   │     97      │
│ iq_002 │ Dime          │ 2003 │ Reggaeton │   2   │     85      │
│ iq_003 │ Cuentale      │ 2003 │ Reggaeton │   3   │     82      │
│ iq_004 │ La Mala       │ 2003 │ Reggaeton │   4   │     88      │
│ iq_005 │ Así Son       │ 2003 │ Reggaeton │   5   │     79      │
└────────┴───────────────┴──────┴───────────┴───────┴─────────────┘
```

### ✅ Output de exportación CSV y BSON

```
✅ CSV exportado: disquera_tarea1.csv

_id,titulo,anio_salida,autor,id_imagen_portada,genero,duracion_seg,numero_pista,popularidad,coleccion
lgf_001,Just Dance,2008,Lady Gaga,images/lady_gaga_the_fame.jpg,Dance-pop,241,1,95,lady_gaga_the_fame
lgf_002,LoveGame,2008,Lady Gaga,images/lady_gaga_the_fame.jpg,Dance-pop,200,2,88,lady_gaga_the_fame
lgf_003,Poker Face,2008,Lady Gaga,images/lady_gaga_the_fame.jpg,Synth-pop,238,3,99,lady_gaga_the_fame
lgf_004,Paparazzi,2008,Lady Gaga,images/lady_gaga_the_fame.jpg,Dance-pop,214,4,90,lady_gaga_the_fame
lgf_005,Beautiful Dirty Rich,2008,Lady Gaga,images/lady_gaga_the_fame.jpg,Electropop,195,5,82,lady_gaga_the_fame
erc_001,El Verdadero Culpable,1992,Diomedes Díaz,images/diomedes_el_regreso_del_condor.jpg,Vallenato,248,1,92,diomedes_diaz_el_regreso_del_condor
erc_002,Eso No Es Na,1992,Diomedes Díaz,images/diomedes_el_regreso_del_condor.jpg,Vallenato,230,2,84,diomedes_diaz_el_regreso_del_condor
...

📦 lady_gaga_the_fame: 5 docs exportados
📦 diomedes_diaz_el_regreso_del_condor: 5 docs exportados
📦 bts_map_of_the_soul_7: 5 docs exportados
📦 michael_jackson_thriller: 5 docs exportados
📦 queen_a_night_at_the_opera: 5 docs exportados
📦 ivy_queen_diva: 5 docs exportados
✅ BSON (JSON) exportados en carpeta bson_tarea1/
Tip: Para BSON binario real → mongodump --uri=<URI> --db BD_DisqueraUniversal
```

---

## 🎯 Tarea 2 — Actualización de Álbumes
**Rama:** `actualizaciones`

Se identificaron y añadieron **2 canciones poco conocidas** por álbum. El proceso de inserción usa un patrón seguro: antes de insertar cada documento se verifica si el `_id` ya existe, evitando `DuplicateKeyError` en re-ejecuciones.

### Canciones adicionales insertadas

| Colección | Canciones agregadas |
|-----------|---------------------|
| lady_gaga_the_fame | Eh Eh (Nothing Else I Can Say) · Money Honey |
| diomedes_diaz_el_regreso_del_condor | La Falla Fue Tuya · El Desquite |
| bts_map_of_the_soul_7 | We are Bulletproof: the Eternal · Respect |
| michael_jackson_thriller | The Lady in My Life · Baby Be Mine |
| queen_a_night_at_the_opera | Seaside Rendezvous · I'm in Love with My Car |
| ivy_queen_diva | Feeling Good · Tócame |

### ✅ Output de actualización

```
✅ Canciones adicionales definidas.

🔄 Actualizando álbumes con canciones adicionales...

🎵 Lady Gaga — The Fame: 5 canciones → +2 nuevas → 7 total
🎵 Diomedes Díaz — El Regreso del Cóndor: 5 canciones → +2 nuevas → 7 total
🎵 BTS — Map of the Soul: 7: 5 canciones → +2 nuevas → 7 total
🎵 Michael Jackson — Thriller: 5 canciones → +2 nuevas → 7 total
🎵 Queen — A Night at the Opera: 5 canciones → +2 nuevas → 7 total
🎵 Ivy Queen — Diva: 5 canciones → +2 nuevas → 7 total

✅ Tarea 2 completada — Álbumes actualizados.
```

### ✅ Tabla de verificación de integridad post-Tarea 2

```
         Resumen de Colecciones — Post Tarea 2
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━┓
┃ Colección                     ┃ Artista         ┃ Álbum                ┃ Total Canciones┃
┡━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━┩
│ lady_gaga_the_fame            │ Lady Gaga       │ The Fame             │       7        │
│ diomedes_diaz_el_regreso_del_condor │ Diomedes Díaz   │ El Regreso del Cóndor      │       7        │
│ bts_map_of_the_soul_7         │ BTS             │ Map of the Soul: 7   │       7        │
│ michael_jackson_thriller      │ Michael Jackson │ Thriller             │       7        │
│ queen_a_night_at_the_opera    │ Queen           │ A Night at the Opera │       7        │
│ ivy_queen_diva                │ Ivy Queen       │ Diva                 │       7        │
└───────────────────────────────┴─────────────────┴──────────────────────┴────────────────┘
```

---

## 🎯 Tarea 3 — Eliminación de Registros
**Rama:** `eliminaciones`

Se realizaron dos tipos de eliminación: la **colección completa de Ivy Queen** (`ivy_queen_diva`) usando `collection.drop()`, y **2 canciones por álbum** en los 5 restantes usando `delete_many()` con el operador `$in`.

### ✅ Output — Eliminación de colección completa

```
🗑️  Colección eliminada: ivy_queen_diva
   Documentos que contenía: 7
   ¿Eliminación verificada?: ✅ SÍ

   Colecciones restantes: ['lady_gaga_the_fame', 'diomedes_diaz_el_regreso_del_condor',
   'bts_map_of_the_soul_7', 'michael_jackson_thriller', 'queen_a_night_at_the_opera']
```

### ✅ Output — Eliminación de canciones por álbum

```
🗑️  Eliminando canciones seleccionadas...

🎵 Lady Gaga — The Fame: 7 → -2 eliminadas → 5 restantes
🎵 Diomedes Díaz — El Regreso del Cóndor: 7 → -2 eliminadas → 5 restantes
🎵 BTS — Map of the Soul: 7: 7 → -2 eliminadas → 5 restantes
🎵 Michael Jackson — Thriller: 7 → -2 eliminadas → 5 restantes
🎵 Queen — A Night at the Opera: 7 → -2 eliminadas → 5 restantes

✅ Eliminaciones de canciones completadas.
```

### ✅ Tabla de estado final — Post Tarea 3

```
                          📊 Estado Final — Post Tarea 3
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━┓
┃ Colección                     ┃ Artista         ┃ Álbum                ┃ Canciones Restantes ┃    Estado    ┃
┡━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━┩
│ lady_gaga_the_fame            │ Lady Gaga       │ The Fame             │          5          │  ✅ Activa   │
│ diomedes_diaz_el_regreso_del_condor │ Diomedes Díaz   │ El Regreso del Cóndor      │          5          │  ✅ Activa   │
│ bts_map_of_the_soul_7         │ BTS             │ Map of the Soul: 7   │          5          │  ✅ Activa   │
│ michael_jackson_thriller      │ Michael Jackson │ Thriller             │          5          │  ✅ Activa   │
│ queen_a_night_at_the_opera    │ Queen           │ A Night at the Opera │          5          │  ✅ Activa   │
│ ivy_queen_diva                │ Ivy Queen       │ Diva                 │          0          │ 🗑️ Eliminada │
└───────────────────────────────┴─────────────────┴──────────────────────┴─────────────────────┴──────────────┘
```

> **Nota:** La celda de exportación CSV/BSON de la Tarea 3 generó un `PermissionError` al intentar sobreescribir `disquera_tarea3.csv` mientras estaba abierto en otro proceso. Todas las operaciones de MongoDB (eliminaciones) se ejecutaron y verificaron correctamente; el error fue exclusivamente de escritura en disco local.

---

## 🏗️ Decisiones de Diseño

**Nomenclatura de colecciones en snake_case sin acentos** (ej. `lady_gaga_the_fame`): evita problemas de codificación entre sistemas operativos y facilita la referencia desde código.

**`_id` manual tipo string** (ej. `lgf_001`): hace los documentos autodescriptivos, simplifica la depuración y produce archivos CSV/BSON legibles sin necesidad de convertir `ObjectId`.

**JSON como sustituto de BSON binario**: el BSON binario real requiere `mongodump` (CLI). Desde Python/Jupyter se exportan archivos JSON equivalentes; la estructura es idéntica, solo difiere la codificación binaria.

**Tarea 1 idempotente**: `collection.drop()` antes de `insert_many()` permite re-ejecutar el notebook sin acumular duplicados.

**Patrón upsert manual en Tarea 2**: se verifica `find_one({_id})` antes de insertar para prevenir `DuplicateKeyError` en re-ejecuciones, sin necesidad de usar `replace_one` con `upsert=True`.

**Ivy Queen elegida para eliminación total**: ilustra claramente la diferencia entre `drop()` (elimina colección e índices) y `delete_many()` (elimina documentos, conserva la colección).

---

## 💡 Hallazgos y Aprendizajes

**La flexibilidad del esquema requiere disciplina explícita.** MongoDB no impone estructura, pero para que la exportación CSV funcione correctamente fue necesario definir y respetar un esquema consistente desde el código Python. Sin esto, `DictWriter` falla ante campos ausentes.

**`count_documents({})` vs `estimated_document_count()`**: se usó `count_documents` para exactitud. La versión estimada es más rápida pero puede reportar valores desactualizados en clústeres del tier gratuito de Atlas.

**`drop()` vs `delete_many()`**: son operaciones conceptualmente distintas. `drop()` elimina la colección completa incluyendo sus índices; `delete_many()` elimina documentos pero conserva la colección y sus metadatos. Esta distinción fue evidente en los outputs de verificación.

**Error de permisos en exportación local (Tarea 3)**: se presentó un `PermissionError` al intentar escribir `disquera_tarea3.csv` porque el archivo estaba abierto en otro proceso. Las operaciones de MongoDB se ejecutaron correctamente; el problema fue exclusivamente de escritura en disco. Solución: cerrar el archivo antes de re-ejecutar la celda de exportación.

**La librería `Rich` mejora significativamente la legibilidad de los outputs**: las tablas generadas con `rich.table.Table` facilitaron la verificación visual del estado de cada colección a lo largo de las tres tareas, especialmente al comparar conteos antes y después de cada operación.

**Modelado de géneros musicales**: varias canciones pertenecen a múltiples géneros (ej. *Beat It* de Michael Jackson es pop y hard rock). Se eligió un único género representativo por documento para mantener el esquema simple y consistente.

---

## 🚀 Instrucciones para Replicar el Ejercicio

### Prerrequisitos

- Python 3.9 o superior
- Cuenta en MongoDB Atlas (el tier gratuito M0 es suficiente)
- Jupyter Notebook o Google Colab

### Pasos

**1. Clonar el repositorio**
```bash
git clone https://github.com/<tu-org>/nosql-disquera-universal.git
cd nosql-disquera-universal
```

**2. Instalar dependencias**
```bash
pip install pymongo rich
```

**3. Configurar la cadena de conexión**

En el notebook, reemplazar la línea:
```python
MONGO_URI = "mongodb+srv://<usuario>:<password>@cluster0.xxxxx.mongodb.net/"
```
con las credenciales de tu propio clúster de MongoDB Atlas.

**4. Ejecutar el notebook de arriba a abajo**

Las tres tareas se ejecutan secuencialmente. Cada tarea genera su propio CSV y carpeta BSON automáticamente.

**5. Importar BSON a otra instancia de MongoDB** *(opcional)*
```bash
mongorestore --uri="mongodb+srv://..." --dir=bson_tarea1/
```

**6. Navegar entre ramas para inspeccionar cada estado**
```bash
git checkout actualizaciones
git checkout eliminaciones
```

---

## 👥 Equipo

| Nombre | Rol |
|--------|-----|
| Joshua Trujillo | Desarrollo, documentación y gestión del repositorio |
| [Integrante 2] | Modelado de datos y pruebas |
| [Integrante 3] | Subida al repositorio y entrega |

*Universidad Externado de Colombia — Bases de Datos NoSQL*
