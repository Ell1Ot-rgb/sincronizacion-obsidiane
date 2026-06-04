# 📁 Estructura del Proyecto — Sistema Vivo Hexagonal v4.0

Este documento contiene el mapa estructural completo y detallado del código fuente del sistema en la versión v4.0 (con integración MemGraphRAG).

---

## 🏛️ Árbol General de Directorios y Archivos

```text
sistema_vivo_final_v2/
├── .env.example                               # Plantilla básica de variables de entorno
├── .env.template                              # Plantilla extendida documentada
├── .gitignore                                 # Reglas de exclusión para Git (cachés, credenciales y estados)
├── ESTRUCTURA_PROYECTO.md                     # Este mapa de la estructura
├── README.md                                  # Documentación inicial general
├── README_NUEVA_ESTRUCTURA.md                 # Guía rápida de la arquitectura hexagonal
├── _patch2.py                                 # Parche corrector de imports de tipados
├── _patch_pipeline.py                         # Parche del orquestador del pipeline
├── _verify.py                                 # Diagnóstico rápido de dependencias de Python
├── docker-compose.memgraph.yml                # Configuración de Docker para levantar Memgraph Platform
├── inicializar_vault.py                       # Constructor de bóveda de Obsidian (v3.0 legado)
├── inicializar_vault_v4.py                    # Constructor del Vault de Obsidian (v4.0 hexagonal)
├── main.py                                    # Punto de entrada principal CLI
├── main_absoluto.py                           # CLI de rutas absolutas (legado)
├── main_bio.py                                # CLI para pruebas de motores biológicos (legado)
├── memgraph_rag_server.py                     # Servidor de la API REST HTTP para MemGraphRAG (puerto 7688)
├── panel_control.bat                          # Lanzador en lote del Panel de Control en Windows
├── panel_control.py                           # Dashboard Web Flask (puerto 5680)
├── requirements.txt                           # Dependencias base
├── requirements-bio.txt                       # Dependencias para lógica biológica
├── requirements-gpu.txt                       # Dependencias para tokenizadores GPU
├── requirements_dualcore.txt                  # Dependencias para configuraciones duales
├── requirements_memgraph.txt                  # Dependencias del subsistema MemGraphRAG
├── test_memgraph.py                           # Test funcional y de validación de MemGraphRAG
│
├── adapters/                                  # CAPA DE ADAPTADORES (Hexágono Externo)
│   ├── __init__.py
│   ├── inbound/                               # Puertos de Entrada (Ingesta)
│   │   ├── __init__.py
│   │   ├── carpeta_watcher.py                 # Monitor del sistema de archivos (Watchdog)
│   │   ├── redis_listener.py                  # Escucha de mensajes pub/sub en Redis
│   │   └── webhook_handler.py                 # Servidor de webhooks Flask de ingesta (puerto 5679)
│   └── outbound/                              # Puertos de Salida (Persistencia/Integración)
│       ├── __init__.py
│       ├── automatizacion.py                  # Rutinas genéricas de automatización
│       ├── gdrive_client.py                   # Sincronizador de Google Drive
│       ├── gemini_client.py                   # Conector a la API de LLMs Google Gemini
│       ├── n8n_integrator.py                  # Adaptador para workflows externos en n8n
│       ├── neo4j_repository.py                # Consultas Cypher para la persistencia en grafos
│       ├── obsidian_sync.py                   # Escritor de Markdown y MOCs en el Zettelkasten
│       ├── supabase_client.py                 # Persistencia en base PostgreSQL administrada
│       └── memgraph_rag/                      # NUEVO: Adaptador y lógica interna del RAG de Memgraph
│           ├── __init__.py
│           ├── README.md
│           ├── global_memory.py               # Memoria global de 3 capas (M_ont, M_fac, M_pas)
│           ├── hierarchical_graph.py          # Constructor del grafo jerárquico G_ont + G_fac + G_pas
│           ├── memgraph_client.py             # Cliente de conexión Bolt y fallback local
│           ├── memgraph_rag_adapter.py        # Adaptador unificado del RAG
│           ├── retrieval.py                   # Motor de búsqueda por PPR (Personalized PageRank)
│           └── agents/                        # Agentes del RAG
│               ├── __init__.py
│               ├── detection_agent.py         # Agente de detección de conflictos lógicos (A_det)
│               ├── extraction_agent.py        # Agente de extracción de esquemas y hechos (A_ext)
│               └── resolution_agent.py        # Agente de resolución de conflictos (A_res)
│
├── config/                                    # Configuraciones globales de red y servicios
│
├── core/                                      # Lógica base y conectores heredados
│
├── core_new/                                  # HEXÁGONO INTERNO (Dominio y Motores de Negocio)
│   ├── __init__.py
│   ├── domain/                                # Modelos Ontológicos Puros
│   │   ├── __init__.py
│   │   ├── contexto.py                        # Entidad del Contexto (⊞ Nivel +1)
│   │   ├── fenomeno.py                        # Entidad del Fenómeno (◉ Nivel 0)
│   │   ├── metacontexto.py                    # Entidades Macro (⊡ Nivel +2) y Meta (⊠ Nivel +3)
│   │   ├── simbologia.py                      # Definición de glifos y tags
│   │   ├── vohexistencia.py                   # Entidad Vohexistencia (◊ Nivel -1)
│   │   ├── preinstancia.py                    # Entidad Preinstancia (ø Nivel -4)
│   │   └── instancia.py                       # Entidad Instancia (• Nivel -3)
│   └── engines/                               # Motores de Inferencia Cognitivos
│       ├── __init__.py
│       ├── pipeline_evolucionado.py           # Orquestador del ciclo de vida de los datos
│       ├── bio/                               # 17 Subsistemas biológicos (homeostasis)
│       │   ├── orchestrator.py                # Orquestador asíncrono bio
│       │   ├── apoptosis/                     # ApoptosisManager
│       │   ├── autonomy/                      # ResourceAutonomy
│       │   ├── dream/                         # DreamEngine + MemoryReplay
│       │   └── ... (otros subsistemas biológicos)
│       ├── chaos/                             # Estabilizadores caóticos (regla 110 de Wolfram)
│       │   ├── automata_1d.py
│       │   └── automata_v2.py
│       ├── logica_extendida/                  # Lógicas no-clásicas
│       │   ├── logica_deontica.py             # Auditoría de voluntades (Permisión, Obligación, Prohibición)
│       │   ├── logica_mereologica.py          # Transitividad parte-todo (⊂ / ∪)
│       │   └── logica_modal.py                # Semántica de accesibilidad Kripke (□ / ◇)
│       ├── logica_pura/                       # Lógicas deductivas y de concepto
│       │   ├── instancia_abstracta.py
│       │   ├── motor_axiomas.py               # Inferencia Horn a punto fijo (⊢)
│       │   ├── motor_hipotetico.py            # Aprendizaje FCA incremental
│       │   └── mundo_hipotetico.py
│       ├── relaciones/
│       │   └── generador_relaciones.py        # Mapea las 12 relaciones fenomenológicas
│       ├── yo_emergente/
│       │   └── motor_yo.py                    # Mapeo del nivel de conciencia del YO (☉)
│       └── retroalimentacion/
│           └── ciclo_resignificacion.py       # Ciclo hermenéutico de resignificación (⟳)
│
├── docker/                                    # Entornos y Dockerfiles de soporte
│
├── docs/                                      # Documentación conceptual e histórica de versiones
│
├── emergencia_concepto/                       # Copia basal del motor de emergencia conceptual
│
├── input/                                     # Carpeta local monitoreada para ingreso de datos
│
├── integraciones/                             # Scripts de automatización legados
│
├── interfaces/                                # Contratos de Puertos Cognitivo-Vitales
│   ├── __init__.py
│   ├── benchmark.py
│   ├── health_monitor.py
│   ├── neural_ports.py                        # Contrato supremo de puertos
│   └── sistema_integrado.py                   # Orquestador cognitivo macro
│
├── logica_pura/                               # Copia basal de lógica pura original
│
├── niveles/                                   # Representaciones de qualia legadas
│
├── procesadores/                              # Tokenizadores y analizadores legados
│
└── tests/                                     # Suite de Pruebas de Funcionamiento
    ├── __init__.py
    ├── test_pipeline_evolucionado.py          # Prueba del pipeline hexagonal
    ├── test_sistema_completo.py               # Suite de 63 tests de alianeación e integración
    ├── test_validacion.py
    ├── ultimo_reporte_test.json               # Reporte JSON del último test
    └── demos/                                 # Scripts de simulación rápida
```

---

## 🛠️ Roles del Sistema por Capas

1.  **`core_new/domain/`**: Define las invariantes de la mente (qualidades del pensamiento puro). No tiene dependencias de librerías ni accesos a base de datos.
2.  **`core_new/engines/`**: Ejecuta los procesos lógicos e inferencias sobre el dominio. Recibe la entrada del orquestador y devuelve el estado procesado.
3.  **`interfaces/` (Puertos)**: Especifica los contratos que los adaptadores de entrada deben seguir y cómo interactúan con los motores de negocio.
4.  **`adapters/` (Adaptadores)**: Implementa la comunicación con la tecnología real (escritura de archivos locales `.md`, consultas Neo4j, escuchas Webhook Flask, etc.).
