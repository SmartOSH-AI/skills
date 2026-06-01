# SmartOSH Skills

Colección de **Agent Skills para Claude** orientadas a Prevención de Riesgos Laborales (PRL) y a las
plataformas **SmartOSH** y **MySmartOSH**. Cada skill encapsula una metodología de trabajo lista para
usar: Claude la activa automáticamente cuando la conversación encaja con su ámbito.

> Las skills son carpetas con un `SKILL.md` (instrucciones + metadatos) y ficheros de apoyo. Se distribuyen
> como `.skill` / `.zip` y se instalan en Claude. Más abajo tienes los pasos.

---

## Skills disponibles

| Skill | Descripción | Estado |
| :-- | :-- | :-- |
| [`investigacion-accidentes-smartosh`](#skill-investigacion-accidentes-smartosh) | Investigación de accidentes e incidentes laborales en España con enfoque sistémico y no punitivo. | v1.0.0 |

---

## Skill: `investigacion-accidentes-smartosh`

Guía a Claude para actuar como experto en investigación de accidentes (España), aplicando un flujo
estructurado con validación humana en cada paso y referenciando normativa verificada.

### Qué hace

- Conduce la investigación con la **regla de oro**: ningún análisis hasta que cada hecho esté
  **CONFIRMADO** o **DESCARTADO** por el investigador humano.
- Aplica y documenta cinco métodos: **Árbol de Causas, ICAM, 5 Porqués, Ishikawa y Árbol de Fallos**.
- Clasifica errores humanos con **GEMS-SHELL** (tipo de error + interfaz) y usa el modelo del **queso
  suizo** de Reason como marco de barreras/defensas.
- Cierra con **conclusiones, plan de acción SMART** (ordenado por jerarquía de controles) y **lecciones
  aprendidas**, más un cuadro comparativo entre métodos y un checklist de cierre.
- Se integra con el conector **MySmartOSH** para traer datos reales del expediente, consultar índices de
  siniestralidad, guardar el análisis de causas y enviar el informe.

### Flujo de trabajo

1. **Arranque** — si hay expediente en MySmartOSH, recupera sus datos reales.
2. **Tabla de Hechos a Confirmar** — todo empieza como *SIN CONFIRMAR*.
3. **Clasificación GEEPO** — Gente · Entorno · Equipo · Procedimientos · Organización.
4. **Aplicación del método** elegido (con diagramas ASCII y listados de causas inmediatas y básicas).
5. **Cierre** — conclusiones, plan SMART, lecciones aprendidas.
6. **Comparación entre métodos** (opcional) y **checklist de cierre**.

### Estructura

```
investigacion-accidentes-smartosh/
├── SKILL.md                              # Identidad, flujo y plantillas
└── references/
    ├── referencias_normativas.md         # Marco legal y fuentes citables
    └── conector_mysmartosh.md            # Catálogo de funciones del conector por fase
```

### Integración con MySmartOSH (opcional)

Cuando el conector está disponible, el skill usa funciones reales de la plataforma:

- **Lectura:** `list_accident_investigations`, `get_accident_investigation`, `get_organization_structure`,
  `aggregate_accident_investigations` (con índices INSST de frecuencia, gravedad, incidencia y duración
  media), `get_exposure_data`, `list_reports`.
- **Escritura / envío (con confirmación explícita):** `update_accident_causes`, `send_email`.
- **Alta de parte por el empleado:** `report_accident`.

El skill funciona también **sin conector**: en ese caso pide los datos al usuario y entrega el informe en
el chat o como archivo. La metodología vive en el propio skill (es la fuente de verdad), por lo que no
necesita llamar a `get_smartosh_methodology`.

### Referencias normativas

Marco legal y guías que el skill puede citar (detalle en `references/referencias_normativas.md`):

- **Ley 31/1995 (LPRL)** — arts. 16 (deber de investigar) y 23 (documentación); **RD 39/1997**.
- **NTP del INSST:** 274 (árbol de causas), 442 (procedimiento), 592/593/594 (gestión integral),
  924 (clasificación de causas), 405 y 415 (factor humano).
- Marcos internacionales: **Reason** (queso suizo, GEMS), **SHELL**, **ICAM**, **5 Porqués**, **Ishikawa**,
  jerarquía de controles; guías **OSHA** y **OIT/ILO**.

---

## Instalación

> Requisito previo: en *Settings → Capabilities*, activa **Code execution and file creation** y **Skills**.
> En planes Team/Enterprise puede requerir que un propietario lo habilite a nivel de organización.

### En Claude.ai (web / app)

1. Descarga el `.skill` (o `.zip`) del skill desde este repositorio.
2. Ve a **Customize → Skills**.
3. Pulsa **Upload skill** y selecciona el archivo. El ZIP debe contener una carpeta con un `SKILL.md`
   válido. *(Si el cargador no acepta la extensión `.skill`, renómbrala a `.zip`: es el mismo formato.)*
4. El skill aparecerá en tu lista; actívalo con el conmutador.

Guía oficial: <https://support.claude.com/en/articles/12512180-use-skills-in-claude>

### En Claude Code

Descomprime la carpeta del skill dentro de tu directorio de skills:

```bash
unzip investigacion-accidentes-smartosh.skill -d ~/.claude/skills/
# verifica que exista ~/.claude/skills/investigacion-accidentes-smartosh/SKILL.md
```

---

## Uso

Una vez instalado y activo, Claude carga el skill automáticamente cuando detecta el contexto: mencionar la
investigación de un accidente o incidente, un análisis de causa raíz, causas inmediatas/básicas, o
cualquiera de los métodos (árbol de causas, ICAM, 5 porqués, Ishikawa, árbol de fallos). Ejemplos:

- «Tengo que investigar un accidente: un operario se cortó con una radial en el taller.»
- «Analiza este incidente con el método del árbol de causas.»
- «Haz un análisis de causa raíz del expediente AC-2024-017.»

No hace falta nombrar el skill; si quieres forzarlo, pídelo explícitamente.

---

## Aviso

Estas skills son una **ayuda metodológica** para el técnico de PRL; no sustituyen el criterio profesional,
la normativa aplicable ni la validación humana de los hechos y las conclusiones. Revisa siempre el
contenido antes de usarlo en un expediente real. Las skills pueden incluir instrucciones que Claude
ejecuta: instala solo desde fuentes en las que confíes y revisa los ficheros antes de subirlos.

---

## Contribuir

¿Aportas una skill nueva o una mejora? Sugerencias:

- Una carpeta por skill, con `SKILL.md` y, si procede, `references/` para el detalle.
- Descripción del `SKILL.md` clara sobre **qué hace** y **cuándo activarse** (es el mecanismo de
  activación).
- Añade la skill a la tabla de *Skills disponibles* y describe los cambios en tu Pull Request.

---

## Licencia

Añade aquí la licencia del repositorio (por ejemplo, MIT o Apache-2.0). Hasta entonces, todos los derechos
reservados por sus autores.
