# 🎓 TESIS_RAG — Sistema RAG para Generación Automática de Sílabos UDLA

> **Herramienta inteligente para la elaboración de sílabos de educación superior en modalidades en línea**  
> Universidad de Las Américas (UDLA) — Maestría en Inteligencia Artificial Aplicada  
> Grupo 32 | María José Fernández Lazcano · Alicia Fernanda Sacoto Macías · Ana Cristina Zaldumbide Egas

---

## 📋 Descripción del Problema y Solución

### Problema organizacional
El área de Educación en Línea (EDL) de la UDLA enfrenta un cuello de botella crítico en el diseño de sílabos académicos: el proceso manual tarda **aproximadamente 4 semanas** por asignatura, limitando la capacidad operativa del equipo de Calidad Académica.

### Solución implementada
Sistema de **Retrieval-Augmented Generation (RAG)** que genera sílabos académicos completos en formato institucional UDLA en menos de **15 minutos**, consultando automáticamente tres bases de datos institucionales:

| BD | Archivo | Contenido |
|----|---------|-----------|
| BD1 | `Tabla ADN_UDLA5H111_*.xlsx` / `Tabla ADN_360_UDLA5H116_*.xlsx` | ADN curricular (RdAs, créditos, horas) |
| BD2 | `dataset_recursosmultimedia.xlsx` | Recursos multimedia institucionales |
| BD3 | `dataset_referenciasbibliograficas.xlsx` | 5,482 referencias bibliográficas |

### Resultados clave (6 experimentos, 2 ADNs)
- ✅ **Cobertura RdA: 100%** en los 6 experimentos
- ✅ **Recall: 1.0** en los 6 experimentos  
- ✅ **URLs válidas: 8/8 MM + 12/12 BIB** en todos los experimentos
- ✅ **Tiempo: 10.4 – 13.0 min** vs 4 semanas del proceso manual
- ✅ **Reducción de tiempo: > 99%**

---

## 🛠️ Requisitos Técnicos y Dependencias

### Sistema operativo
- Windows 10/11 (probado) · Linux/macOS compatible

### Python
```
Python 3.10 (entorno Anaconda: rag_udla)
```

### Instalación del entorno
```bash
# 1. Crear entorno virtual
conda create -n rag_udla python=3.10
conda activate rag_udla

# 2. Instalar dependencias
pip install langchain langchain-community langchain-groq langchain-text-splitters
pip install faiss-cpu sentence-transformers
pip install pandas openpyxl python-docx
pip install scikit-learn matplotlib numpy
pip install groq
```

### Dependencias completas (`requirements.txt`)
```
langchain==0.3.x
langchain-community==0.3.x
langchain-groq==0.2.x
langchain-text-splitters==0.3.x
faiss-cpu==1.8.x
sentence-transformers==3.x
pandas==2.x
openpyxl==3.x
python-docx==1.x
scikit-learn==1.x
matplotlib==3.x
numpy==1.x
groq==0.x
```

### API Key requerida
- **Groq API** (gratuita): Registrarse en https://console.groq.com y obtener una API Key
- Modelo utilizado: `llama-3.3-70b-versatile` (100,000 tokens/día en plan gratuito)

---

## 📁 Estructura del Repositorio

```
TESIS_RAG/
│
├── 📓 TESIS_RAG.ipynb              # Notebook principal (40 celdas)
├── 📄 README.md                    # Este archivo
├── 📄 requirements.txt             # Dependencias del proyecto
│
├── 📁 data/                        # Bases de datos institucionales
│   ├── Tabla ADN_UDLA5H111_Maestría en Asesoría y Gerencia Legal de Empresas.xlsx
│   ├── Tabla ADN 360_UDLA5H116_M. TRANSFORMACIÓN DIGITAL.xlsx
│   ├── dataset_recursosmultimedia.xlsx
│   ├── dataset_referenciasbibliograficas.xlsx
│   ├── Modelo-Educativo-UDLA-2025-2030.pdf
│   │
│   └── 📁 indices/                 # Índices FAISS (generados automáticamente)
│       ├── faiss_normativa/
│       ├── faiss_multimedia/
│       ├── faiss_bibliografica/
│       ├── faiss_normativa_Exp-1/  (chunk 300)
│       ├── faiss_normativa_Exp-2/  (chunk 500)
│       └── ...                     (un índice por experimento)
│
├── 📁 templates/                   # Plantillas institucionales
│   ├── DEL_02_PLANTILLA_SILABO_UDLA_LIMPIA.docx
│   └── logo_udla.png
│
└── 📁 output/                      # Archivos generados (sílabos, índices, métricas)
    ├── SILABO_Exp-1_MAGL0003_*.docx
    ├── INDICE_Gestión_De_Riesgos_*.docx
    ├── GRAFICAS_Exp-1_MAGL0003_*.png
    ├── COMPARATIVA_EXP123_*.png
    ├── COMPARATIVA_EXP456_*.png
    ├── COMPARATIVA_BLOQUES_123vs456_*.png
    ├── METRICAS_6_EXPERIMENTOS_*.xlsx
    └── LISTA_COTEJO_EXPERTOS_*.docx
```

---

## 🚀 Instrucciones de Ejecución Paso a Paso

### Paso 1: Abrir el entorno
```bash
# Abrir Anaconda Prompt
conda activate rag_udla
cd C:\Users\Fer\TESIS_RAG
jupyter notebook
```

### Paso 2: Ejecutar el notebook completo
En Jupyter: **Kernel → Restart & Run All**

O ejecutar las celdas en orden:

| Celda | Descripción |
|-------|-------------|
| **1** | Instalación de paquetes |
| **2** | Configurar API Key de Groq y datos del sílabo |
| **3** | Definir rutas de archivos y carpetas |
| **4** | Inicializar modelos (embeddings + LLM) |
| **5** | Cargar e indexar las 3 BDs con FAISS |
| **6** | Recuperar contexto semántico |
| **7** | BD1: Extraer RdAs del ADN + generar descripción + índice |
| **8** | BD2: Selección multimedia por similitud coseno (2/semana) |
| **9** | BD3: Selección bibliográfica por similitud coseno (3/semana) |
| **10** | Generar sílabo Word + índice temático independiente |
| **11** | Aplicar formato (logo, Calibri, granate, secciones A-I) |

### Paso 3: Configurar el experimento (editar Celda 2)
```python
GROQ_API_KEY = "gsk_..."          # Tu clave de Groq API
CODIGO_ASIGNATURA = "MAGL0003"    # Código del ADN a procesar
```

### Paso 4: Para los experimentos comparativos (Celdas 12-13)
```python
# Cambiar estas 5 líneas según el experimento:
CODIGO_EXP      = "MAGL0003"     # Exp-1: MAGL0003 | Exp-2: MAGL0006 | Exp-3: MAGL0009
CHUNK_SIZE_EXP  = 300             # Exp-1: 300      | Exp-2: 500      | Exp-3: 500
TOP_K_EXP       = 5               # Exp-1: 5        | Exp-2: 5        | Exp-3: 10
NUM_EXPERIMENTO = "Exp-1"         # "Exp-1" | "Exp-2" | "Exp-3"
PALABRAS_EXP    = ["RIESGO","CORPORATIVO","GESTION","EMPRESARIAL","CONTROL","LEGAL"]
```

### Paso 5: Verificar los archivos generados
Los archivos se guardan en `output/`:
- `SILABO_Exp-N_CODIGOXXX_YYYYMMDD_HHMM.docx` — Sílabo Word con plantilla UDLA
- `INDICE_NombreAsignatura_YYYYMMDD_HHMM.docx` — Índice temático independiente
- `GRAFICAS_Exp-N_CODIGOXXX_YYYYMMDD_HHMM.png` — Gráficas de métricas
- `METRICAS_6_EXPERIMENTOS_YYYYMMDD_HHMM.xlsx` — Excel comparativo

---

## 🔄 Explicación General del Pipeline

```
┌─────────────────────────────────────────────────────────────────────┐
│                    PIPELINE RAG — TESIS_RAG                         │
└─────────────────────────────────────────────────────────────────────┘

ENTRADA: Código de asignatura (ej. MAGL0003)
    │
    ▼
[PASO 1] Extracción determinística del ADN (Python/pandas)
    ├── Asignatura, créditos, horas (posición fija en Excel)
    ├── RdAs (búsqueda por prefijo: MAGL0003-RDA*)
    └── Sesiones = créditos × 4

    │
    ▼
[PASO 2] Indexación FAISS (3 bases de datos)
    ├── BD1: ADN curricular → RecursiveCharacterTextSplitter (chunk_size, overlap=20%)
    ├── BD2: Multimedia → fragmentos por recurso
    └── BD3: Bibliografía → fragmentos por referencia
    Embeddings: paraphrase-multilingual-MiniLM-L12-v2 (local)

    │
    ▼
[PASO 3] Recuperación semántica (Top-K)
    └── query = "Maestría + Asignatura" → FAISS retriever → Top-K fragmentos

    │
    ▼
[PASO 4] Generación LLM (Groq API / LLaMA 3.3-70b)
    ├── Llamada 1: Descripción del curso + Índice temático (12 unidades)
    ├── Llamada 2: Sesiones sincrónicas + Post-sesión (JSON estructurado)
    └── Llamada 3: Referencias principales y complementarias (APA 7)

    │
    ▼
[PASO 5] Selección de recursos por similitud coseno
    ├── BD2: Para cada semana → cosine_similarity(RdA_emb, TEMA_GLOBAL_emb) → top 2
    └── BD3: Para cada semana → cosine_similarity(RdA_emb, TEMA_emb) → top 3
    Filtros BD3: idioma=español + año 2020-2025 + URL válida + palabras clave

    │
    ▼
[PASO 6] Ensamblaje del cronograma (4 semanas)
    └── Pre-sesión (MM + BIB) + Sincrónica + Post-sesión + Evaluación + Ponderación

    │
    ▼
[PASO 7] Generación del documento Word
    ├── Plantilla: DEL_02_PLANTILLA_SILABO_UDLA_LIMPIA.docx
    ├── Reemplazar etiquetas {{ CAMPO }} con contenido generado
    ├── Rellenar tabla cronograma con datos del Paso 6
    └── Aplicar formato: Calibri, granate #8B1A2F, logo UDLA, márgenes 2.54cm

    │
    ▼
SALIDA: SILABO_Exp-N_CODIGO_FECHA.docx + INDICE_Asignatura_FECHA.docx
```

---

## 📊 Resultados de los 6 Experimentos

### Bloque 1 — ADN: Maestría en Asesoría y Gerencia Legal de Empresas

| Exp | Código | Asignatura | Chunk | K | Tiempo | Accuracy | Similitud | F1 |
|-----|--------|-----------|-------|---|--------|----------|-----------|-----|
| Exp-1 | MAGL0003 | Gestión de Riesgos Corporativos | 300 | 5 | 13.0 min | 5.0% | 0.2084 | 0.0952 |
| Exp-2 | MAGL0006 | Gestión Integral de RRHH | 500 | 5 | 10.6 min | 10.0% | 0.2690 | 0.1818 |
| Exp-3 | MAGL0009 | Innovación y Transformación Legal | 500 | 10 | 11.1 min | 10.0% | 0.2215 | 0.1818 |

### Bloque 2 — ADN: Maestría en Transformación Digital

| Exp | Código | Asignatura | Chunk | K | Tiempo | Accuracy | Similitud | F1 |
|-----|--------|-----------|-------|---|--------|----------|-----------|-----|
| Exp-4 | MTDL0001 | Estrategia de Transformación Digital | 300 | 5 | 11.8 min | 35.0% | 0.2158 | 0.2632 |
| Exp-5 | MTDL0005 | Estrategia de Negocios en IA | 500 | 5 | 10.4 min | 5.0% | 0.2078 | 0.0952 |
| Exp-6 | MKTA0006 | Negocios Electrónicos y Marketing Digital | 500 | 10 | 10.4 min | 5.0% | 0.3480 | 0.0952 |

**Métricas constantes en todos los experimentos:** Recall=1.0 · Cobertura RdA=100% · URLs MM=8/8 · URLs BIB=12/12

---

## ⚙️ Configuración Óptima Identificada

| Configuración | ADN Legal | ADN Transformación Digital |
|---------------|-----------|---------------------------|
| **Chunk size** | 500 tokens | 300 tokens |
| **Top-K** | 5 | 5 |
| **Mejor experimento** | Exp-2 (F1=0.182, Sim=0.269) | Exp-4 (F1=0.263, Acc=35%) |

---

## 🔒 Consideraciones Éticas y de Privacidad

- **Sin datos personales**: el sistema no procesa información de estudiantes ni docentes
- **Datos locales**: embeddings generados localmente con MiniLM (sin enviar a servidores externos)
- **Trazabilidad completa**: cada recurso asignado incluye fuente, nombre y URL verificable
- **Supervisión humana**: el sílabo generado es un borrador que requiere validación por expertos
- **Clasificación EU AI Act**: sistema de riesgo limitado (Art. 6, Reglamento UE 2024/1689)
- **Cumplimiento LOPDP Ecuador 2021**: sin tratamiento de datos personales

---

## 📚 Referencias Técnicas Principales

- Lewis et al. (2020). Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks. NeurIPS. https://arxiv.org/abs/2005.11401
- Reimers & Gurevych (2019). Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks. EMNLP. https://arxiv.org/abs/1908.10084
- Johnson et al. (2019). Billion-scale similarity search with GPUs. IEEE Transactions on Big Data. https://arxiv.org/abs/1702.08734
- European Commission. (2019). Ethics Guidelines for Trustworthy AI.
- OECD. (2024). OECD AI Principles. https://oecd.ai/en/ai-principles

---

## 👥 Autoras

| Nombre | Rol |
|--------|-----|
| María José Fernández Lazcano | Desarrollo del pipeline RAG y experimentación |
| Alicia Fernanda Sacoto Macías | Diseño pedagógico y validación curricular |
| Ana Cristina Zaldumbide Egas | Análisis de métricas y documentación técnica |

---

*Proyecto de Titulación — Maestría en Inteligencia Artificial Aplicada — UDLA 2025*
