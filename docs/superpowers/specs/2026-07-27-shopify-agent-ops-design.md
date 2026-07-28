# Shopify Agent Ops — Diseño del sistema

**Fecha:** 27 de julio de 2026  
**Estado:** especificación de diseño para revisión  
**Rama:** `agent/shopify-agent-ops-design`

## 1. Propósito

Construir un sistema modular de agentes de inteligencia artificial capaz de crear, interpretar, diseñar, automatizar, optimizar y operar tiendas Shopify bajo controles verificables de ecommerce.

El sistema deberá poder adaptarse a distintas marcas y mercados, aprender una identidad de marca a partir de evidencia, producir y mantener una tienda completa y ejecutar operaciones repetitivas dentro de políticas previamente aprobadas.

No se prometerá una operación literalmente “sin errores”. La promesa del producto será:

> Los agentes validan antes de ejecutar, simulan cambios riesgosos, detienen operaciones inconsistentes, registran cada acción y permiten restaurar el estado anterior.

## 2. Resultado comercial buscado

El sistema deberá convertirse en un producto de automatización vendible para tiendas Shopify y, simultáneamente, en el principal caso demostrable de la marca personal de Ignacio Kozadjian.

El posicionamiento inicial será:

> Sistemas de agentes de IA que crean, optimizan y operan tiendas Shopify con controles reales de ecommerce.

No se presentará como un chatbot, un generador de temas o una colección de prompts. Se presentará como un sistema operativo de ecommerce administrado por agentes especializados.

## 3. Decisiones de diseño aprobadas

1. No crear una skill monolítica.
2. Crear una skill orquestadora y agentes especializados.
3. Mantener Superpowers como metodología transversal, sin copiar todas sus skills dentro del producto.
4. Generalizar las reglas de seguridad, QA, preview, publicación y rollback de `editartiendameller`.
5. Incorporar las capacidades integrales de creación y administración de tienda atribuidas a `tiendashopifyv2` cuando su archivo exacto esté disponible.
6. Separar razonamiento, aprobación y ejecución.
7. Construir primero un MVP interno modular y evolucionarlo después a una aplicación Shopify multi-tenant.
8. Validar el sistema inicialmente en una development store o tienda sandbox.
9. No tocar ninguna tienda productiva durante el desarrollo inicial.

## 4. Alcance funcional final

El sistema deberá poder administrar, según permisos y disponibilidad de API:

- identidad y sistema de marca;
- arquitectura de información de la tienda;
- navegación, menús, colecciones y filtros;
- productos, variantes, SKUs, metafields, imágenes, videos y SEO;
- costos, precios, márgenes, descuentos, bundles y promociones;
- home, PDP, colecciones, landings, carrito, búsqueda, header y footer;
- banners y campañas programadas;
- CRO y merchandising;
- inventario y reglas de disponibilidad;
- pedidos, etiquetas, notas, fulfillment, devoluciones y excepciones;
- Shopify Flow, webhooks, Functions e integraciones externas;
- rendimiento, Core Web Vitals, analítica y alertas;
- QA, preview, deployment, observabilidad y rollback.

La disponibilidad de una función no implica autorización automática para utilizarla.

## 5. Arquitectura general

```text
Usuario / operador
        ↓
Store Orchestrator
        ↓
Policy & Approval Engine
        ↓
Agentes especializados
        ↓
QA & Safety Gate
        ↓
Execution Adapter
        ↓
Shopify / integraciones
        ↓
Audit Log + Store State + Evidence
```

### 5.1 Capa de skills

Define metodología, decisiones, estándares, rutas y validaciones.

### 5.2 Capa de agentes

Cada agente tiene una responsabilidad limitada, contratos definidos y ausencia de privilegios generales.

### 5.3 Capa de ejecución

Aplicación o adaptadores que realizan operaciones autenticadas mediante Shopify Admin GraphQL, Shopify CLI, Flow, Functions, webhooks y servicios externos.

### 5.4 Capa de control

Administra permisos, aprobaciones, políticas, límites, locks, auditoría, reintentos, snapshots y rollback.

## 6. Skill orquestadora

Nombre provisional: `shopify-agent-ops`.

Responsabilidades:

1. identificar la tienda y el entorno exactos;
2. cargar el estado operativo y la identidad de marca;
3. interpretar el objetivo del usuario;
4. clasificar alcance, riesgo e impacto;
5. dividir el objetivo en tareas independientes;
6. seleccionar agentes especializados;
7. definir dependencias y orden;
8. exigir evidencias de entrada;
9. enviar cada salida a validación;
10. detener la ejecución ante inconsistencias;
11. consolidar resultados, evidencias y próximos pasos.

La skill orquestadora no modifica Shopify directamente.

## 7. Agentes especializados

### 7.1 Store Orchestrator

Coordina el sistema. No diseña, programa ni escribe directamente en Shopify.

### 7.2 Brand Intelligence

Interpreta y mantiene:

- posicionamiento;
- público objetivo;
- propuesta de valor;
- tono;
- identidad visual;
- fotografía;
- jerarquía gráfica;
- reglas de copy;
- reglas promocionales;
- elementos prohibidos;
- diferencias por mercado.

Toda inferencia deberá registrar evidencia y nivel de confianza. Las decisiones permanentes de identidad requieren aprobación humana salvo que exista una política expresa diferente.

### 7.3 Catalog Manager

Administra:

- títulos;
- descripciones;
- especificaciones;
- categorías y taxonomía;
- vendor y tipo de producto;
- tags;
- metafields y metaobjects;
- variantes;
- SKUs y códigos de barras;
- imágenes, videos y alt text;
- SEO;
- colecciones;
- canales de publicación;
- estado de producto.

No podrá inventar dimensiones, materiales, compatibilidades, garantías, certificaciones ni atributos técnicos.

### 7.4 Pricing & Promotions

Administra:

- costos;
- margen mínimo;
- precio base;
- redondeos comerciales;
- compare-at price;
- descuentos;
- códigos;
- promociones automáticas;
- bundles;
- umbrales;
- envío gratis;
- reglas de combinación;
- protección de contribución.

No podrá activar una promoción que coloque la contribución estimada debajo del mínimo aprobado.

### 7.5 Theme, Design & CRO

Administra componentes visuales y de conversión:

- home;
- PDP;
- colecciones;
- landings;
- carrito;
- búsqueda;
- header y footer;
- formularios;
- banners;
- selectores y filtros;
- quick add;
- upselling y cross-selling;
- responsive;
- accesibilidad;
- Core Web Vitals.

Conserva obligatoriamente:

- comprobación de tienda y theme ID;
- desarrollo sobre tema no publicado;
- separación entre edición, subida y publicación;
- Theme Check;
- preview;
- QA mobile y desktop;
- medición de rendimiento;
- rollback;
- changelog;
- prohibición de afirmar éxito sin evidencia.

### 7.6 Merchandising & Search

Administra navegación, colecciones automáticas, ranking, filtros, sinónimos, recomendados, similares, complementarios y reglas de exposición basadas en stock, margen, demanda y conversión.

### 7.7 Order Operations

Puede gestionar pedidos dentro de políticas expresas:

- tags y notas;
- clasificación operativa;
- direcciones;
- fulfillment orders;
- tracking;
- devoluciones;
- cambios;
- reembolsos;
- cancelaciones;
- excepciones.

Reembolsos, cancelaciones, cambios monetarios, alteración de dirección, edición de líneas y contacto al cliente requieren autorización reforzada.

### 7.8 Automation & Integrations

Decide entre:

- Shopify Flow;
- webhooks;
- Admin GraphQL;
- Shopify Functions;
- jobs programados;
- ERP;
- marketplaces;
- logística;
- email;
- hojas de cálculo;
- dashboards;
- servicios externos.

Flow será preferido para automatizaciones nativas simples y transparentes. La aplicación propia será preferida cuando se necesite estado, lógica compleja, reintentos, integración externa o auditoría avanzada.

### 7.9 Analytics & Performance

Administra métricas comerciales, operativas y técnicas. Distingue explícitamente entre correlación, hipótesis, experimento y resultado confirmado.

### 7.10 QA, Safety & Deployment

Tiene autoridad para bloquear cualquier tarea.

Valida:

- tienda, entorno e IDs;
- scopes;
- diff;
- duplicados;
- integridad referencial;
- margen;
- inventario;
- Liquid y JSON;
- errores GraphQL;
- responsive;
- accesibilidad;
- flujos críticos;
- rendimiento;
- preview;
- snapshot y rollback.

Ninguna tarea se considera completada sin su evidencia.

## 8. Niveles de autonomía

### Nivel 0 — Lectura

Consulta, auditoría, análisis y recomendación.

### Nivel 1 — Borrador

Genera recursos no aplicados: productos borrador, copy, código local, banners, promociones desactivadas o workflows sin activar.

### Nivel 2 — Sandbox

Puede ejecutar en development store, tema no publicado, producto no publicado, workflow desactivado o preview.

### Nivel 3 — Producción bajo política

Puede ejecutar acciones rutinarias expresamente preaprobadas y dentro de límites cuantitativos.

Ejemplos:

- etiquetar pedidos;
- publicar productos previamente validados;
- actualizar stock desde fuente autorizada;
- activar banners programados;
- aplicar precios dentro de una banda;
- ordenar colecciones mediante reglas aprobadas.

### Nivel 4 — Confirmación humana obligatoria

Incluye:

- publicar un tema;
- modificar dominio;
- instalar o eliminar apps;
- modificar checkout;
- cambiar billing;
- ejecutar descuentos masivos fuera de política;
- eliminar productos;
- reembolsar o cancelar pedidos;
- cambiar identidad permanente;
- actuar sobre datos sensibles.

## 9. Contratos de datos

### 9.1 Store Profile

```yaml
store_id:
shop_domain:
organization:
market:
currency:
timezone:
environment:
active_theme_id:
working_theme_id:
locations:
channels:
apps:
api_version:
approval_policy_id:
```

### 9.2 Brand Profile

```yaml
brand_name:
positioning:
audience:
value_proposition:
tone:
visual_personality:
primary_colors:
secondary_colors:
typography:
photography_rules:
product_copy_rules:
promotion_rules:
forbidden_elements:
approved_components:
evidence:
confidence:
```

### 9.3 Action Plan

```yaml
action_id:
objective:
agent:
target_store:
target_environment:
target_resources:
risk_level:
required_scopes:
inputs:
preconditions:
proposed_changes:
expected_result:
verification:
rollback:
approval_required:
```

### 9.4 Execution Result

```yaml
action_id:
status:
started_at:
finished_at:
resources_changed:
before_snapshot:
after_snapshot:
validation_results:
errors:
warnings:
rollback_available:
evidence:
```

## 10. Seguridad e invariantes

El sistema deberá cumplir siempre estas invariantes:

1. Nunca operar sin identificar tienda y entorno.
2. Nunca escribir en producción cuando el alcance aprobado sea sandbox.
3. Nunca ejecutar una operación destructiva sin snapshot o reversión documentada cuando sea técnicamente posible.
4. Nunca inventar atributos de productos.
5. Nunca publicar un tema sin preview y QA.
6. Nunca aplicar precios o descuentos sin validación de margen.
7. Nunca ejecutar dos escrituras incompatibles sobre el mismo recurso simultáneamente.
8. Nunca marcar como completado un cambio no verificado.
9. Nunca esconder errores o resultados parciales.
10. Nunca exceder scopes o políticas asignadas al cliente.

## 11. Controles técnicos requeridos

- OAuth por tienda;
- scopes mínimos;
- tokens cifrados;
- verificación de webhooks;
- idempotency keys;
- locks por tienda y recurso;
- rate-limit handling;
- reintentos con backoff;
- dead-letter queue;
- audit log inmutable;
- snapshot antes/después;
- diffs;
- pause switch global;
- límites de costo por agente;
- feature flags;
- separación development, staging y production;
- observabilidad y alertas;
- políticas multi-tenant.

## 12. Flujo general de ejecución

```text
1. Recibir objetivo
2. Resolver tienda, entorno y política
3. Leer Store Profile y Brand Profile
4. Clasificar riesgo
5. Elaborar Action Plan
6. Validar precondiciones
7. Solicitar aprobación cuando corresponda
8. Crear snapshot
9. Ejecutar en sandbox o producción autorizada
10. Validar resultado
11. Ejecutar QA funcional y técnico
12. Registrar evidencia
13. Confirmar éxito o revertir
14. Actualizar Store State y changelog
```

## 13. MVP

El MVP se compondrá de tres módulos.

### 13.1 Product Factory

Desde URL, PDF, CSV, planilla o ficha de proveedor:

1. extrae hechos verificables;
2. identifica faltantes;
3. normaliza datos;
4. genera propuesta comercial;
5. calcula precio y margen;
6. estructura variantes;
7. genera o procesa recursos visuales;
8. controla duplicados;
9. crea producto borrador;
10. genera PDP;
11. ejecuta QA;
12. publica únicamente bajo política.

### 13.2 Store Builder

1. interpreta la marca;
2. crea Brand Profile;
3. propone arquitectura de navegación;
4. diseña home, colección y PDP;
5. crea secciones editables;
6. genera preview;
7. ejecuta QA responsive, Theme Check y rendimiento;
8. publica solo tras aprobación.

### 13.3 Campaign Operator

1. lee inventario, costos, margen y calendario;
2. propone campaña;
3. crea descuentos;
4. genera banners;
5. programa secciones;
6. actualiza colecciones;
7. activa automatizaciones;
8. monitorea resultado;
9. desactiva o revierte al finalizar.

## 14. Piloto recomendado

El primer piloto no productivo será una tienda demostrativa del nicho “control de pelos de mascotas en hogar, ropa y auto”.

Razones:

- existe investigación previa;
- el problema es demostrable visualmente;
- admite producto héroe, bundle y upsells;
- permite crear desde cero identidad, catálogo, pricing, tienda, campaña y automatizaciones;
- no requiere utilizar información confidencial de Meller;
- funciona como caso público para la marca personal.

## 15. Contenido para marca personal

El desarrollo deberá producir evidencia reutilizable para Instagram:

1. de ficha de proveedor a producto completo;
2. cómo el agente interpreta una marca;
3. creación de una tienda mediante agentes especializados;
4. prevención de escrituras en la tienda equivocada;
5. protección automática del margen;
6. creación de campañas completas;
7. QA y rollback;
8. límites de autonomía;
9. construcción pública del piloto;
10. resultados y aprendizajes reales.

## 16. Fuera de alcance inicial

- operación autónoma irrestricta de producción;
- reemplazo total de atención humana;
- promesa de cero errores;
- soporte a todas las plataformas ecommerce;
- decisiones legales, fiscales o regulatorias automáticas;
- conciliación financiera completa;
- automatización de reembolsos y cancelaciones sin control;
- modificación directa de tiendas Meller durante el MVP;
- SaaS multi-cliente completo antes de validar los tres módulos del MVP.

## 17. Estrategia de implementación

### Fase 1 — Diseño y skills

- skill orquestadora;
- agentes;
- referencias;
- contratos JSON/YAML;
- políticas;
- evaluaciones de comportamiento.

### Fase 2 — Sandbox operativo

- development store;
- Product Factory;
- Theme Builder básico;
- QA automático;
- audit log local.

### Fase 3 — Piloto completo

- identidad;
- catálogo;
- precios;
- tema;
- campañas;
- automatizaciones;
- contenido demostrativo.

### Fase 4 — Aplicación privada

- Shopify OAuth;
- backend;
- Supabase/Postgres;
- jobs y colas;
- dashboard de control;
- multi-tenant inicial.

### Fase 5 — Producto comercial

- onboarding;
- planes y límites;
- documentación;
- seguridad;
- soporte;
- observabilidad;
- facturación;
- casos de uso y venta.

## 18. Evaluaciones obligatorias

La skill y los agentes deberán superar escenarios que intenten inducirlos a:

1. modificar la tienda equivocada;
2. publicar directamente en producción;
3. crear productos con datos inventados;
4. duplicar SKUs;
5. aplicar precios con margen negativo;
6. ejecutar descuentos incompatibles;
7. cancelar o reembolsar sin autorización;
8. alterar pedidos cumplidos;
9. declarar éxito sin verificación;
10. ocultar un fallo parcial;
11. sobrescribir identidad de marca sin evidencia;
12. ejecutar tareas simultáneas incompatibles;
13. perder el rollback;
14. operar con scopes insuficientes;
15. ignorar una regresión de rendimiento.

## 19. Criterios de aceptación del MVP

El MVP se considerará válido cuando pueda demostrar, en sandbox:

1. identificación inequívoca de tienda y entorno;
2. creación de Store Profile y Brand Profile;
3. creación de al menos tres productos completos sin inventar datos;
4. cálculo verificable de precios y margen;
5. generación de home, colección y PDP coherentes con la marca;
6. preview responsive;
7. Theme Check sin errores bloqueantes;
8. creación de una campaña completa desactivada;
9. audit log de todas las acciones;
10. detección y bloqueo de una operación riesgosa;
11. snapshot y rollback de una modificación;
12. reporte final con evidencia verificable.

## 20. Preguntas resueltas por esta especificación

- La solución será modular.
- Superpowers será metodología, no contenido duplicado.
- La autonomía será graduada.
- Producción estará protegida por políticas y gates.
- El MVP tendrá tres módulos.
- El piloto será independiente de Meller.
- La evolución comercial será de skill interna a aplicación multi-tenant.

## 21. Dependencia pendiente

Todavía debe localizarse o reconstruirse el contenido exacto de `tiendashopifyv2` y confirmar si `editartiendameller3` corresponde al archivo ya revisado cuya versión interna es 3.0.0.

La ausencia de esos archivos no bloquea la arquitectura, pero sí impide afirmar que se realizó una fusión literal completa. Durante la implementación se deberá elaborar una matriz de trazabilidad que indique, regla por regla, qué se conserva, se transforma, se reemplaza o se elimina.
