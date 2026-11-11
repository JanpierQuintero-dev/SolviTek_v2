# ⚡ SolviTek v2.0 — Sistema de Gestión Comercial, POS & Inventario Multi-sucursal

![Estado](https://img.shields.io/badge/Estado-En_Desarrollo_/_MVP-orange)
![Node.js](https://img.shields.io/badge/Node.js-v20-brightgreen?logo=nodedotjs)
![Express](https://img.shields.io/badge/Express-v5-000000?logo=express)
![SQLite](https://img.shields.io/badge/SQLite-v3-003B57?logo=sqlite)
![Tests](https://img.shields.io/badge/Tests-Jest_--_Passing-success?logo=jest)
![Licencia](https://img.shields.io/badge/Licencia-ISC-blue)

SolviTek v2.0 es un sistema web integral de punto de venta (POS), control de inventarios, gestión de créditos y analítica de ventas. Diseñado para ofrecer alta disponibilidad en pequeños y medianos comercios (minimarkets, papelerías, tiendas de retail) con soporte de operaciones **offline** y arquitectura **multi-sucursal**.

---

## ✨ Características Principales

### 🛒 Punto de Venta (POS) & Modo Offline
- **Ventas Rápidas:** Registro veloz de artículos, múltiples métodos de pago (efectivo, transferencia, crédito) e impresión/generación de comprobantes.
- **Resiliencia Offline (`offline-sync.js`):** Permite continuar facturando aunque falle la conexión a internet. Las ventas se guardan en la cola local (`localStorage`) con UUIDs y se sincronizan con la base de datos de manera **idempotente** (evitando duplicaciones mediante `sync_log`).

### 🏬 Control Multi-sucursal & Kardex de Inventario
- **Gestión por Sucursal:** Stock independiente por punto de venta mediante claves compuestas (`product_id + branch_id`).
- **Kardex Dinámico (`stock_moves`):** Registro detallado de trazabilidad para cada ajuste, compra o venta con cálculo de saldo resultante (`balance_after`).
- **Compras & Proveedores:** Actualización automática de inventario y recalculo de costos al registrar compras a proveedores.

### 💳 Clientes, Cartera & Créditos
- **Ventas a Crédito:** Control estricto de cupo de crédito por cliente con validación en servidor antes de procesar transacciones.
- **Soporte de Cuotas (Installments):** Gestión de cartera y seguimiento a diferidos/pagos parciales.

### 📊 Reportes & Analítica en Tiempo Real
- **Dashboard de KPIs:** Gráficas interactivas (Chart.js) de ingresos, gastos y distribución por método de pago.
- **Filtros Personalizados:** Análisis por periodos de tiempo y sucursales específicas con soporte de zona horaria configurable (`REPORT_TZ_OFFSET`).
- **Exportación:** Generación de reportes tabulares exportables a Excel.

---

## 🛠️ Stack Tecnológico

| Capa | Tecnologías |
|---|---|
| **Backend** | Node.js (ES6+), Express.js 5 |
| **Base de Datos** | SQLite3 (Helpers promisificados, migraciones automáticas) |
| **Autenticación & Seguridad** | JWT (`jsonwebtoken`), Bcryptjs, Helmet, Rate-Limiting, CORS |
| **Frontend** | HTML5, CSS3 (Tailwind CSS via CDN), Vanilla JS (ES6+), SweetAlert2, Chart.js |
| **Testing & Calidad** | Jest, Supertest, ESLint |
| **CI/CD** | GitHub Actions (Pipeline automatizado de linting y tests en Node 20) |

---

## 🔒 Seguridad & Arquitectura

- **Control de Acceso Basado en Roles (RBAC):** Middleware de protección (`requireRole`) para perfiles `admin`, `cajero` y `bodeguero`.
- **Protección Anti-Fuerza Bruta:** Rate limiting diferenciado (300 req/min global en API y 20 intentos por 15 min en endpoints de autenticación).
- **PIN de Seguridad para Módulos Sensibles:** Verificación de segundo factor en cliente para apertura de inventarios y reportes.
- **Pista de Auditoría (`audit_log`):** Registro de IP, usuario, acción y timestamps para operaciones críticas del sistema.

---

## 📁 Estructura del Proyecto

```text
App-Inventario1.0/
├── server.js              # Servidor Express, middlewares globales y rutas
├── db.js                  # Conexión SQLite, esquemas, relaciones y migraciones
├── offline-sync.js        # Motor de sincronización offline e idempotencia
├── index.html             # Interfaz principal del POS e inventario por sucursal
├── login.html             # Módulo de autenticación y recuperación
├── customers.html/.js     # Módulo de clientes y gestión de créditos
├── purchases.html/.js     # Gestión de proveedores y compras
├── reports.html/.js       # Panel de analítica con Chart.js y exportación Excel
├── security.js            # Control de PIN para áreas restringidas
├── middleware/            # JWT, RBAC y auditoría (`auth.js`, `audit.js`)
├── routes/                # Endpoints RESTful (`auth.js`, `catalog.js`, `inventory.js`, `sales.js`)
├── tests/                 # Pruebas de integración con Jest y Supertest
└── .github/workflows/     # Pipeline CI en GitHub Actions
🚀 Instalación y Ejecución LocalPrerrequisitosNode.js v18+ o v20+GitPasosClonar el repositorio:Bashgit clone [https://github.com/JanpierQuintero-dev/SolviTek-v2.git](https://github.com/JanpierQuintero-dev/SolviTek-v2.git)
cd SolviTek-v2
Instalar dependencias:Bashnpm install
Configurar variables de entorno (Opcional):Bashexport PORT=3000
export JWT_SECRET=tu_clave_secreta_local
export REPORT_TZ_OFFSET=-5
Iniciar servidor en modo desarrollo:Bashnpm run dev
Abrir el navegador en http://localhost:3000. El primer usuario que registre la aplicación obtendrá automáticamente el rol de admin.🧪 Pruebas & Calidad de CódigoEjecutar la suite de pruebas unitarias y de integración:Bashnpm test
Ejecutar el analizador de código estático (ESLint):Bashnpm run lint
🗺️ Roadmap & Próximas Mejoras[ ] Implementación de transacciones explícitas (BEGIN/COMMIT) en transacciones complejas.[ ] Integración de servicio SMTP para recuperación de credenciales vía email.[ ] Exportación de reportes comerciales a formato PDF.[ ] Lectura de códigos de barras mediante cámara web en la interfaz del POS.[ ] Dockerización del entorno (Dockerfile y docker-compose).👤 AutorJanpier QuinteroGitHub: @JanpierQuintero-dev   Email: jampierquintero0@gmail.com   Ubicación: Colombia
