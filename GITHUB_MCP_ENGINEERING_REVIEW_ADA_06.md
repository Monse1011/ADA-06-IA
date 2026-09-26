# GitHub MCP Engineering Review — ADA-06

---

## 1. Repository
* **Owner / Repo:** `Monse1011/E.L.TENDEJON`
* **Visibility:** Privado (`private: true`, bifurcado / `fork: true`)
* **Branch / reference analyzed:**
  * Rama base: `main` (commit `33d5480b36e7c7c4aa1c083eeb7981b940c3c964`)
  * Rama de desarrollo analizada: `feature/Modifications` (commit `fb276b37f72ae6847ce492877f8cba1408657199`)
  * Entidades rastreadas: Issue #1 y Pull Request #2

---

## 2. MCP Connection
* **Server:** `github`
* **Mode:** READ-ONLY (Solo lectura estricta)
* **Toolsets:**
  * `repos`: `get_file_contents`, `list_commits`, `search_code`, `search_repositories`
  * `issues`: `get_issue`, `list_issues`, `search_issues`, `add_issue_comment`
  * `pull_requests`: `get_pull_request`, `list_pull_requests`, `get_pull_request_files`, `get_pull_request_status`, `get_pull_request_comments`, `get_pull_request_reviews`
  * `users`: `search_users`

---

## 3. Repository Understanding
* **Purpose:**  
  Aplicación de escritorio monolítica desarrollada en **Java SE 11 con Java Swing** para la gestión operativa de un punto de venta (POS) y control de inventario en tiendas minoristas tradicionales (*tendejones* / abarrotes). Incluye control de acceso con roles (**Administrador** y **Empleado**), catálogo de productos, registro de ventas con descuentos y cálculo de cambio, y almacenamiento persistente local en archivos planos sin motor de base de datos relacional.
* **Architecture summary:**  
  Implementa una arquitectura en capas basada en **MVC (Model-View-Controller)** y **DAO (Data Access Object)**:
  * **Presentación (`view.*`):** Componentes basados en `javax.swing.JFrame` con maquetación absoluta (`null layout`), paleta institucional `#C7ECF8` y más de 15 diálogos modales construidos como ventanas independientes.
  * **Control (`control.*`):** Manejadores de eventos `java.awt.event.ActionListener` que validan entradas, ejecutan la navegación entre pantallas (`dispose()`, `setVisible()`) y median entre las vistas y la persistencia.
  * **Dominio (`model.*`):** Entidades POJO (`Articulo`, `Empleado`) y excepciones de negocio no chequeadas (`StockInsuficienteException`, `NumeroNegativoOCeroException`, `ValorDescuentoException`).
  * **Acceso a Datos (`DAO.*`):** Clases `DAOArticulo` y `EmpleadoDAO` que gestionan una caché en memoria (`ArrayList<T>`) y reescriben los archivos planos en cada mutación.
  * **Almacenamiento Físico (`registro/*`):** Archivos de texto plano delimitados por comas (`Articulos.txt`, `Empleados.txt`, `RegistroVentas.txt`, `Ticket.txt`).
* **Important files:**  
  * `pom.xml`: Archivo de construcción Maven (Java 11, empaquetado JAR ejecutable y dependencia JUnit 4).
  * `src/test/ProgramaTendejon.java`: Punto de entrada oficial de la aplicación (`public static void main`).
  * `src/control/ControlVenta.java`: Orquestador principal de la terminal de cobro y deducción inmediata de stock.
  * `SETUP.md` y `MAVEN_SETUP.md`: Guías de ejecución y configuración en IDEs.
* **Tests:**  
  **Inexistentes.** El directorio `src/test/` contiene exclusivamente la clase ejecutable de inicio `ProgramaTendejon.java`. Aunque JUnit 4.13.2 está declarado en `pom.xml`, `mvn test` reporta 0 casos ejecutados. El aseguramiento de calidad depende exclusivamente de pruebas manuales.

---

## 4. Issue Analysis
* **Issue:** Issue #1 — `Refactorización del código` (Estado: Abierto | Autor: `Monse1011`).
* **Facts from Issue:**
  * Solicita refactorizar el sistema CRUD POS en Java para reducir código duplicado, separar responsabilidades y aplicar buenas prácticas de POO y Clean Code, **sin alterar el comportamiento funcional**.
  * Define 13 tareas con checkboxes y 9 criterios de aceptación formales (paridad funcional, compilación correcta, ausencia de duplicación y separación de capas).
  * Incluye menciones textuales a *"centralizar operaciones de conexión a base de datos"*, *"simplificar consultas SQL repetidas"* y *"las pruebas existentes continúan pasando"*.
* **Ambiguities:**
  1. **Discrepancia SQL vs. Archivos planos:** El Issue menciona repetidamente bases de datos y SQL, pero el repositorio no utiliza SQL ni JDBC; persiste en archivos `.txt`. Falta aclarar si se refiere a optimizar el I/O existente o a una migración técnica a SQLite/H2.
  2. **Inexistencia de suite de pruebas:** Exige que *"las pruebas existentes continúen pasando"*, pero no hay tests unitarios previos en el repositorio.
  3. **Indefinición de idioma:** No especifica si la nomenclatura de clases y métodos debe estandarizarse en español o migrarse a inglés.
  4. **Nivel de consolidación UI:** No aclara si la multiplicidad de más de 15 ventanas `JFrame` modales puede sustituirse por `JOptionPane` o `JDialog`.
* **Related requirements/spec:**  
  * `docs/REQUIREMENTS.md`: RF-09..RF-15 (CRUD de artículos), RF-16..RF-21 (Operaciones POS), RNF-03 (Compatibilidad multiplataforma de rutas), RNF-04 (Compilación estándar Maven), RNF-09 (Separación de responsabilidades MVC+DAO) y reglas de negocio RN-01..RN-08.
  * `docs/SPEC.md`: Sección 2 (Estructura de archivos en `registro/`), Sección 3 (Contratos y retornos DAO) y Sección 6 (Catálogo de deuda técnica).
* **Related code:**  
  `src/model/Articulo.java`, `src/model/Empleado.java`, `src/DAO/DAOArticulo.java`, `src/DAO/EmpleadoDAO.java`, `src/control/ControlVenta.java`, `src/control/ControlAgregarProducto.java`, `src/control/ControlDarBaja.java`, `src/view/PagarView.java`.
* **Related tests:**  
  Ninguno. No existen archivos de prueba asociados en el repositorio.

---

## 5. Pull Request Review
* **PR:** PR #2 — `Refactorización del idioma.` (Estado: Abierto | Rama: `feature/Modifications` $\rightarrow$ `main`).
* **Changed behavior:**  
  En el commit `bcaecf5`, renombra atributos y métodos en `src/model/Empleado.java` a minúsculas en inglés (`name`, `password`, `getname()`, `setname()`, `getpassword()`, `setpassword()`). En el commit `fb276b3`, incorpora 4 documentos de especificación técnica en `docs/`.
* **Changed files:**  
  1. `docs/ARCHITECTURE.md` (+310 líneas)
  2. `docs/MCP_GITHUB_TOOL_INVENTORY.md` (+41 líneas)
  3. `docs/REQUIREMENTS.md` (+156 líneas)
  4. `docs/SPEC.md` (+280 líneas)
  5. `src/model/Empleado.java` (+13 / -13 líneas)
* **Tests:**  
  0 pruebas añadidas o modificadas.
* **Observations:**  
  * La descripción del PR está vacía (`body: null`) y no hay comentarios de revisión previos.
  * La documentación agregada en `docs/` es de alto valor técnico y formaliza exhaustivamente la arquitectura, pero excede el alcance del título (*"Refactorización del idioma"*).
* **Risks:**  
  * Fragmentación del código con mezcla inconsistente de idiomas (solo `Empleado` fue alterado; el resto del dominio, controladores y DAOs permanecen en español).
  * Fusionar el PR a `main` en su estado actual romperá la rama principal para todos los desarrolladores.
* **Questions:**  
  * ¿Cuál es la estrategia idiomática definitiva para el proyecto (100% español o migración integral a inglés)?
  * ¿Es conveniente separar la documentación técnica en un PR dedicado antes de tocar código?
* **Potential defects:**  
  * **[POTENTIAL DEFECT] Ruptura de compilación:** Clases consumidoras como `EmpleadoDAO.java`, `InicioSesionControl.java` y `AgregarEmpleadoControl.java` no fueron actualizadas y siguen llamando a `getNombre()`, `setNombre()`, `getContraseña()` y `setContraseña()`. El proyecto falla al compilar (`cannot find symbol`).
  * **[POTENTIAL DEFECT] Incumplimiento de Java Beans:** Los métodos fueron nombrados en minúsculas continuas (`getname()`, `getpassword()`) violando la convención estándar `getName()` y `getPassword()`.

---

## 6. Traceability

> **Nota metodológica:** Las conexiones marcadas como *(Inferencia)* obedecen a deducciones técnicas por coincidencia semántica y de commits, ya que el PR #2 no referencia formalmente al Issue #1 en su descripción.

| Issue / Need | Requirement / Spec | PR | Code / Files | Test / Evidence | Estado |
|---|---|---|---|---|---|
| **Issue #1 (Tarea):** Nombres e idioma | `SPEC.md` §6.1 / `ARCHITECTURE.md` §7.1 | **PR #2** *(Inferencia)* | `src/model/Empleado.java` | Inexistentes (`mvn test`: 0 pruebas) | **Gap / Defectuoso** (Ruptura de compilación) |
| **Issue #1 (Tarea):** Revisar estructura | `REQUIREMENTS.md` / `SPEC.md` / `ARCHITECTURE.md` | **PR #2** *(Inferencia)* | `docs/*` (4 documentos) | Verificado documentalmente vía MCP | **Covered** (Especificación exhaustiva añadida) |
| **Issue #1 (Alcance):** CRUD de productos | `REQUIREMENTS.md` RF-09..RF-15 | **N/A** (Sin PR) | `src/model/Articulo.java`, `src/DAO/DAOArticulo.java`, `ControlAgregarProducto.java`, `ControlDarBaja.java` | Inexistentes | **Gap** (No implementado en PR alguno) |
| **Issue #1 (Alcance):** Operaciones POS | `REQUIREMENTS.md` RF-16..RF-25 | **N/A** (Sin PR) | `ControlVenta.java`, `ControlPago.java`, `SaleWindowView.java`, `PagarView.java` | Inexistentes | **Gap** (No implementado en PR alguno) |
| **Issue #1 (CA-07):** Compilación limpia | `REQUIREMENTS.md` RNF-04 | **PR #2** | `src/model/Empleado.java` vs consumidores en `DAO/` y `control/` | Fallo evidente en diff de firmas de métodos | **Gap / Defectuoso** (Regresión introducida) |
| **Issue #1 (CA-09):** Pruebas existentes | `REQUIREMENTS.md` RNF-04 | **PR #2** | `pom.xml`, `src/test/ProgramaTendejon.java` | `mvn test` reporta 0 casos | **Gap** (No evaluable por falta de suite previa) |
| **Need Técnico:** Rutas multiplataforma | `REQUIREMENTS.md` RNF-03 / `SPEC.md` §6.2 | **N/A** (Sin PR) | `ControlAgregarProducto.java`, `ControlDarBaja.java` (`\\`) | Inexistentes | **Gap** (Pendiente normalización de rutas) |

---

## 7. Permission Review
* **Authentication:** Conexión mediante token de acceso personal (PAT) / integración configurada en el cliente MCP de Antigravity.
* **GitHub token permissions:** Lectura sobre repositorios privados (`repo:read`, `read:org`).
* **MCP read-only:** Verificado y cumplido estrictamente en esta sesión. No se invocó ningún endpoint de escritura, creación de issues, comentarios ni fusiones.
* **Enabled toolsets:** `repos`, `issues`, `pull_requests`, `users`.
* **Write capabilities exposed:**  
  * Herramientas de escritura de código (`create_issue`, `create_or_update_file`, `push_files`, `merge_pull_request`, `create_pull_request_review`): **NO expuestas**.
  * Herramientas con capacidad de mutación presentes en interfaz: Únicamente `add_issue_comment`. *(No ejecutada en observancia del mandato de solo lectura)*.

---

## 8. Security Notes
* **Credential exposure:**  
  * El archivo `registro/Empleados.txt` almacena contraseñas en texto plano sin funciones hash (`Edwin,Cafe123`, `Shaden,poo`, `Cesar,123cesar123`).
  * En `src/control/MenuControl.java:53,96`, las credenciales de administración están hardcodeadas directamente en el código fuente.
* **Prompt injection:**  
  La información leída de títulos, cuerpos y descripciones remotas de GitHub se procesa exclusivamente como datos pasivos de análisis estructural, eliminando el riesgo de inyección indirecta de instrucciones.
* **Excess permissions:**  
  La presencia de `add_issue_comment` en la lista de herramientas activas de una sesión declarada como *Read-Only* representa una sobreexposición de permisos. Se recomienda revocar herramientas de comentarios a nivel de configuración del servidor MCP en auditorías de solo lectura.
* **Repository scope:**  
  Todas las invocaciones del MCP estuvieron rigurosamente acotadas al repositorio objetivo `Monse1011/E.L.TENDEJON`.

---

## 9. Human Review
* **What did you verify yourself?**
  1. Se verificó línea por línea el diff del PR #2 en `src/model/Empleado.java`, corroborando que únicamente se alteró esa clase y que no se renombraron los métodos en `EmpleadoDAO.java`, `InicioSesionControl.java` ni `AgregarEmpleadoControl.java`.
  2. Se inspeccionó el árbol del proyecto confirmando que `src/test/` aloja únicamente la clase ejecutable de interfaz gráfica `ProgramaTendejon.java`, desmintiendo la existencia de tests unitarios previos.
  3. Se confirmó la ausencia total de dependencias JDBC o archivos `.sql`, identificando que la persistencia se realiza exclusivamente mediante I/O sobre `registro/*.txt`.
  4. Se corroboró la existencia y contenido de los 4 archivos de documentación agregados en `docs/` dentro de la rama `feature/Modifications`.
* **What AI conclusions did you reject or modify?**
  1. **Se rechazó la asunción de que el Issue #1 describía una base de datos real:** Se determinó que las cláusulas sobre "consultas SQL y base de datos" en el Issue corresponden a una plantilla genérica incongruente con la realidad del repositorio.
  2. **Se rechazó la viabilidad del criterio CA-09 ("las pruebas existentes continúan pasando"):** Se clarificó que dicho criterio no puede satisfacerse formalmente al carecer el proyecto de un arnés de pruebas automatizadas previo.
  3. **Se rechazó asumir que el PR #2 estaba listo para merge:** A pesar de incorporar documentación de alto nivel, se identificó y etiquetó como defecto bloqueante la incompatibilidad de firmas de métodos en `Empleado.java`.
  4. **Se rechazó presentar el vínculo entre Issue #1 y PR #2 como un hecho del repositorio:** Se etiquetó explícitamente como una inferencia de ingeniería ante la falta de enlaces formales (`Closes #1`) en GitHub.

---

## 10. Conclusion
### What did MCP add to the engineering workflow?
1. **Acceso determinista y en tiempo real a artefactos remotos:** Permitió inspeccionar metadatos de PRs, issues, commits de múltiples ramas y diffs de código sin requerir clonaciones locales ni intervención manual del usuario.
2. **Capacidad de auditoría cruzada profunda:** Facilitó contrastar lo solicitado en los issues frente a lo realmente implementado en los pull requests, detectando defectos de compilación e incongruencias de arquitectura antes de cualquier despliegue o fusión.
3. **Confinamiento de seguridad verificable:** Hizo posible auditar repositorios privados de manera exhaustiva bajo una política estricta de solo lectura, impidiendo mutaciones accidentales o no autorizadas en el repositorio remoto.
