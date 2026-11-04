# Lucas Emanuel Ramos Carvalho

**Desarrollador full stack · sistemas de gestión, integraciones y datos**  
PHP / Laravel · Python / FastAPI · Go · React / TypeScript · PostgreSQL · MySQL · Linux

Desarrollo el software que sostiene un negocio: **vender, controlar el inventario, cobrar y cumplir con las entregas**. Llevo una funcionalidad desde la conversación con quien la usa hasta la pantalla, las reglas del backend, la base de datos, las integraciones externas y el despliegue.

[Web](https://lerczin.github.io/portfolio/) · [Correo](mailto:lercsz1472@gmail.com) · [GitHub](https://github.com/LercZin) · [English](README.md)

## Proyectos

| Proyecto | Qué hace | Mi papel · tecnologías | Ver |
| --- | --- | --- | --- |
| **Pinheiro Móveis** | Gestiona una cadena de tiendas de muebles: ventas, inventario, pagos a plazos, entrega y montaje. | Único desarrollador desde 2022 · Laravel, Livewire, MySQL | [Caso de estudio](case-studies/pinheiro-moveis.md) · [Web](https://pinheiromoveis.com) |
| **Viure Maranhão** | Tienda online con acceso de usuarios, checkout, webhooks de pago y cálculo de envíos. | Único desarrollador desde 2022 · Next.js, React, TypeScript, Laravel, MySQL | [Caso de estudio](case-studies/viure.md) · [Web](https://viuremaranhao.com.br) |
| **Cérebro** | Plataforma multiempresa de atención al cliente con IA por WhatsApp, aislada por empresa en PostgreSQL. | Desarrollador principal, en curso · FastAPI, PostgreSQL RLS, Redis, React | [Caso de estudio](case-studies/cerebro.md) |
| **Retail Operations Demo** | Versión pública y ejecutable de los problemas de comercio anteriores, con tiendas ficticias. | Único desarrollador · Laravel, React/TypeScript, Go, MySQL/SQLite | [Código](https://github.com/LercZin/retail-operations-demo) · [Verificación](https://github.com/LercZin/retail-operations-demo/blob/main/docs/verification.md) |
| **UniBot** | Asistente para un portal de estudiantes: datos académicos, preguntas frecuentes y chat con IA opcional. | Implementé la aplicación en un equipo académico · FastAPI, MySQL, JavaScript | [Código](https://github.com/LercZin/unibot) · [CI](https://github.com/LercZin/unibot/actions/workflows/tests.yml) |

Los proyectos comerciales son privados: sus casos de estudio explican las decisiones y las pruebas en lugar de mostrar el código. Los repositorios públicos se pueden clonar y ejecutar.

## Laboratorios de ingeniería

Proyectos pequeños con datos ficticios, cada uno centrado en un problema fácil de resolver mal.

| Laboratorio | Problema | Ver |
| --- | --- | --- |
| **PostgreSQL Outbox** | Workers en Go/pgx que reclaman tareas en paralelo y sobreviven a caídas y entregas duplicadas. | [Código](https://github.com/LercZin/postgres-outbox-lab) |
| **Pathfinding** | A* y Dijkstra sobre cuadrículas con pesos en TypeScript, con reproducción paso a paso. | [Código](https://github.com/LercZin/pathfinding-lab) · [Demo](https://lerczin.github.io/pathfinding-lab/) |
| **Data Reconciliation** | Importación en Python/SQL que pone en cuarentena las filas inválidas, se puede repetir sin riesgo y explica cada diferencia. | [Código](https://github.com/LercZin/data-reconciliation-lab) |

## Problemas que he resuelto

- **Una tienda nunca debe ver los datos de otra.** En Pinheiro Móveis el ámbito de la tienda se aplica en el servidor, en cada consulta y al crear registros, con pruebas que intentan leer por ID un cliente de otra tienda.
- **Un recuento de inventario que falla a mitad no puede dejar medio inventario ajustado.** Todo el recuento va en una transacción con bloqueo de filas y se rechaza si el saldo cambió desde que empezó.
- **Los proveedores de pago envían la misma notificación varias veces, a veces antes de que exista el cobro.** Viure guarda cada evento, lo aplica una sola vez y lo reprocesa cuando aparece el pago correspondiente.
- **Repetir un checkout no debe cobrar dos veces.** El checkout mantiene una única clave de intento ante fallos de red y se detiene en vez de crear un cobro nuevo sin avisar.
- **El aislamiento entre empresas no debería depender de acordarse de un `WHERE`.** Cérebro usa row-level security de PostgreSQL: la propia base de datos rechaza las filas de otra empresa.

## Competencias

| Área | Tecnologías | Dónde |
| --- | --- | --- |
| Backend | PHP/Laravel, Python/FastAPI, Go, APIs REST, autorización, transacciones | Todos los proyectos |
| Datos | PostgreSQL (RLS, advisory locks), MySQL, SQLite, SQLAlchemy, Alembic, pgx | Cérebro, laboratorios, sistemas de gestión |
| Frontend | React, TypeScript, Next.js, Livewire, Tailwind, HTML/CSS | Viure, Cérebro, Pinheiro Móveis |
| Integraciones | Pagos Asaas, envíos Melhor Envio, WhatsApp Cloud API, webhooks | Viure, Pinheiro Móveis, Cérebro |
| Pruebas | PHPUnit, pytest, Playwright, runners de pruebas JavaScript, CI en GitHub Actions | Todos los repositorios públicos |
| Despliegue | Git, Docker Compose, VPS Linux, Nginx/Apache, colas, scripts de release y rollback | Sistemas de gestión, Cérebro |

Más detalle (en inglés): [mapa técnico](docs/engineering-map.md) (áreas → código) y [evidencia técnica](docs/engineering-evidence.md) (pruebas y ejecuciones de CI).

## Sobre mí

Antes y a la vez que el software, trabajé en **montaje de muebles, ventas en tienda y online, y marketing**. Por eso mis proyectos parten de cómo se hace el trabajo de verdad: he vendido y montado los muebles yo mismo.

Empecé Pinheiro Móveis y Viure en **2022**, mientras estudiaba Ciencias de la Computación en **Anhanguera**; me gradúo en **diciembre de 2026**.

- **Idiomas:** portugués (nativo), español e inglés (intermedio).
- **Ubicación:** São Luís, Brasil. Dispuesto a trasladarme a España (necesito que la empresa tramite el permiso de trabajo). Disponible para entrevistas por videollamada y pruebas técnicas.
- **Busco:** puestos de desarrollador de software, full stack, backend Laravel o React/TypeScript.

**Contacto:** [lercsz1472@gmail.com](mailto:lercsz1472@gmail.com) · perfil estructurado: [`candidate.json`](candidate.json)

<sub>No aparecen cifras de facturación, tráfico o rendimiento porque no las he medido. Las pruebas privadas se describen a partir de su código; los repositorios públicos enlazan a sus ejecuciones de CI.</sub>
