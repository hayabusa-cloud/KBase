# KBase

Base de conocimiento estructurada y autoorganizada para agentes de programación con IA.

[English](README.md) | [中文](README.zh-CN.md) | [Español](README.es.md) | [日本語](README.ja.md) | [Français](README.fr.md)

## Contexto

KBase persiste el conocimiento del agente — reglas, referencias, decisiones, habilidades — entre sesiones, para que los agentes inicien informados en lugar de desde cero.

## Estructura

| Directorio | Función |
|------------|---------|
| `ROOT/` | Meta-principios, arranque, comandos |
| `refs/` | Conocimiento externo verificado |
| `notes/` | Percepciones y decisiones del proyecto |
| `skills/` | Flujos de trabajo automatizados |
| `rules/` | Restricciones y convenciones |
| `env/` | Configuraciones de entorno |
| `perms/` | Secretos y credenciales (en gitignore) |
| `hooks/` | Callbacks basados en eventos |
| `plans/` | Planes de tareas autogenerados |

Cada directorio contiene dos archivos: `AGENTS.md` (guía para el agente) e `INDEX.md` (tabla de contenidos).

## Principios

1. **Persistencia activa** — Los agentes guardan hallazgos de inmediato, sin necesidad de instrucción.
2. **KBase primero** — Buscar conocimiento interno antes que fuentes externas. Orden: rules, notes, refs, skills, luego externo.
3. **Fuentes autoritativas** — Todo conocimiento se verifica contra fuentes primarias. El conocimiento no verificado no se almacena.
4. **Documentación primero** — Actualizar la documentación antes de actuar. En caso de conflicto, actualizar la fuente de nivel inferior.
5. **Despacho de habilidades** — Revisar habilidades existentes antes de trabajo ad-hoc.

## Protocolo de Comunicación

KBase define una notación estructurada para la interacción humano–agente. Especificación completa: [`ROOT/communication.md`](ROOT/communication.md)

| Notación | Semántica |
|----------|-----------|
| `[[ ... ]]` | Entregable obligatorio |
| `[ ... ]` | Entregable opcional |
| `(( ... ))` | Restricción obligatoria — violación = blocked |
| `( ... )` | Restricción preferente — mejor esfuerzo |
| `( ... ) > ~( ... )` | Preferencia con reducción — preferir izquierda; reducir tendencia hacia derecha |
| `?( ... )` | Consulta de validación |
| `< ... >` | Contexto de entrada |
| `<< ... >>` | Puntero de referencia |
| `->` | Flujo de control — `?( Condición ) -> [ Acción ]` |

Estados: `ready` → `done` (normal) o `ready` → `blocked` (conflicto/entrada faltante).

## Autoridad

| Nivel | Fuente |
|-------|--------|
| 1 | `ROOT/` |
| 2 | `rules/` |
| 3 | `refs/` |
| 4 | `notes/` |
| 5 | Externo |

El nivel superior prevalece. En caso de conflicto, se actualiza la fuente de nivel inferior.

## Ciclo de vida

| Transición | Condición |
|------------|-----------|
| notes → refs | Verificado contra fuente primaria |
| notes → rules | El patrón se repite, se convierte en restricción |
| plans → notes/rules | Plan completado, conclusiones extraídas |

## Inicio rápido

1. Clonar: `git clone https://github.com/hayabusa-cloud/KBase.git`
2. Apuntar la configuración del agente de IA al directorio `KBase/`.
3. Trabajar. El agente lee `AGENTS.md` al iniciar la sesión y mantiene la base de conocimiento automáticamente.

## Auto-Save

El agente guarda hallazgos en el directorio correspondiente automáticamente:

| Hallazgo | Destino |
|----------|---------|
| Restricciones, convenciones | `rules/` |
| Percepciones, decisiones, patrones | `notes/` |
| Conocimiento externo verificado | `refs/` |
| Flujos de trabajo automatizados | `skills/` |
| Planes de tareas | `plans/` |

Para agregar contenido manualmente, crear un archivo Markdown en el directorio destino y añadir una entrada en su `INDEX.md`.

## Bootstrap

Al iniciar la sesión, el agente lee en este orden:

`AGENTS.md` → `ROOT/AGENTS.md` → `ROOT/instructions.md` → `rules/INDEX.md` → `skills/INDEX.md` → `notes/INDEX.md`

Tras la compactación de contexto, el agente relee al menos los tres primeros archivos.

## Comandos

```
research <topic>     Recopilar y verificar información; guardar en refs/ o notes/
dive into <topic>    Investigación profunda en múltiples pasadas con fuentes de vanguardia
review <subject>     Evaluar contra KBase e investigación web
tidy <scope>         Reorganizar estructura y contenido; tidy all para todo KBase
recall <topic>       Buscar conocimiento existente en KBase antes de consultar fuentes externas
save <topic>         Guardar un hallazgo en el directorio correspondiente; actualizar INDEX.md
promote <file>       Mover una nota a refs/ o rules/; actualizar ambos INDEX.md
```

## Licencia

[MIT](LICENSE) — ©2026 [Hayabusa Cloud Co., Ltd.](https://code.hybscloud.com)
