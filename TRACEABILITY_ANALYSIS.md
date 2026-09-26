# 14. Fase 5 — Traceability: Issue → PR → Code → Test

**Repositorio:** `Monse1011/E.L.TENDEJON`  
**Fecha de Análisis:** Septiembre 2026  
**Modo:** Solo lectura (*Read-Only*) vía GitHub MCP  

---

## 1. Criterios Metodológicos de Trazabilidad

1. **Evidencia Remota Verificada:** Todas las conexiones registradas en esta matriz provienen exclusivamente de payloads JSON obtenidos mediante herramientas de lectura del MCP de GitHub (`get_issue`, `get_pull_request`, `get_pull_request_files`, `get_file_contents`, `list_commits`).
2. **Distinción entre Hechos e Inferencias:**
   * **Hecho:** El PR #2 no contiene texto en su cuerpo (`body: null`) ni palabras clave formales de cierre (`Closes #1`, `Fixes #1`).
   * **Inferencia de ingeniería:** La vinculación entre el PR #2 y las tareas del Issue #1 se clasifica como una deducción analítica fundamentada en la coincidencia semántica de títulos, mensajes de commit y archivos modificados.

---

## 2. Matriz de Trazabilidad

| Issue / Need | PR | Code / Files | Test / Evidence | Estado |
| :--- | :--- | :--- | :--- | :--- |
| **Issue #1 (Tarea):**<br>Mejorar nombres de clases, métodos y variables (*Refactorización del idioma*) | **PR #2**<br>`feature/Modifications`<br>*(Relación inferida por título y diff)* | `src/model/Empleado.java`<br>*(Renombrado de campos a `name`, `password` y métodos a `getname()`, `getpassword()`)* | **Inexistentes en repo.**<br>`src/test/` contiene únicamente `ProgramaTendejon.java` (ejecutable). `mvn test` corre 0 pruebas. | **Gap / Defectuoso**<br>*(Ruptura de compilación: `EmpleadoDAO`, `InicioSesionControl` y `AgregarEmpleadoControl` no fueron actualizados)* |
| **Issue #1 (Tarea):**<br>Revisar la estructura actual del proyecto (*Documentar arquitectura y requerimientos*) | **PR #2**<br>`feature/Modifications`<br>*(Relación inferida por commit `fb276b3`)* | `docs/ARCHITECTURE.md`<br>`docs/REQUIREMENTS.md`<br>`docs/SPEC.md`<br>`docs/MCP_GITHUB_TOOL_INVENTORY.md` | **Verificado documentalmente.**<br>Archivos estáticos añadidos e inspeccionados vía MCP. | **Covered**<br>*(Documentación exhaustiva agregada; formaliza requerimientos RF/RNF y arquitectura MVC+DAO)* |
| **Issue #1 (Alcance / CA-08):**<br>Refactorización de clases de productos y operaciones CRUD | **N/A**<br>*(Sin PR asociado)* | `src/model/Articulo.java`<br>`src/DAO/DAOArticulo.java`<br>`src/control/ControlAgregarProducto.java`<br>`src/control/ControlDarBaja.java`<br>`src/control/ControlInventario.java` | **Inexistentes.**<br>No hay tests automatizados para validar no-regresión del CRUD. | **Gap**<br>*(No implementado en ningún PR activo en el repositorio)* |
| **Issue #1 (Alcance / CA-01):**<br>Refactorización del POS, cálculo de totales y cobro | **N/A**<br>*(Sin PR asociado)* | `src/control/ControlVenta.java`<br>`src/control/ControlPago.java`<br>`src/view/SaleWindowView.java`<br>`src/view/PagarView.java`<br>`registro/Ticket.txt`<br>`registro/RegistroVentas.txt` | **Inexistentes.**<br>No hay pruebas unitarias para validación de stock, descuentos ni cálculo de cambio. | **Gap**<br>*(No implementado en ningún PR activo en el repositorio)* |
| **Issue #1 (Tarea / CA-05):**<br>Centralizar operaciones de persistencia / acceso a datos | **N/A**<br>*(Sin PR asociado)* | `src/DAO/DAOArticulo.java`<br>`src/DAO/EmpleadoDAO.java`<br>`src/control/ControlVenta.java`<br>*(persistencia directa en archivos planos)* | **Inexistentes.**<br>Sin tests unitarios de I/O de archivos planos. | **Gap**<br>*(No implementado; la persistencia sigue acoplada dentro de controladores)* |
| **Issue #1 (Criterio CA-07):**<br>El proyecto compila correctamente | **PR #2**<br>`feature/Modifications` | `src/model/Empleado.java`<br>frente a:<br>`src/DAO/EmpleadoDAO.java`<br>`src/control/InicioSesionControl.java`<br>`src/control/AgregarEmpleadoControl.java` | **Evidencia estática de fallo.**<br>Incompatibilidad de firmas visible en diff (`cannot find symbol: method getNombre()`). | **Gap / Defectuoso**<br>*(El código introducido en PR #2 rompe la compilación limpia con Maven)* |
| **Issue #1 (Criterio CA-09):**<br>Las pruebas existentes continúan pasando | **PR #2**<br>`feature/Modifications` | `pom.xml` (`junit:junit:4.13.2`)<br>`src/test/ProgramaTendejon.java` | **Evidencia en repo.**<br>`mvn test` no detecta casos (`Tests run: 0`). No existen clases `*Test.java`. | **Gap**<br>*(Criterio no evaluable por inexistencia de suite de pruebas previa en el proyecto)* |
| **Need Técnico (RNF-03 / SPEC 6.2):**<br>Portabilidad multiplataforma de rutas de archivo | **N/A**<br>*(Sin PR asociado)* | `src/control/ControlAgregarProducto.java`<br>`src/control/ControlDarBaja.java`<br>*(Uso de `"registro\\Articulos.txt"`)* | **Inexistentes.**<br>Sin tests en entornos Linux/macOS. | **Gap**<br>*(Pendiente de estandarización con `File.separator` o `java.nio.file.Path`)* |

---

## 3. Desglose Técnico de Evidencias e Inferencias

### 3.1 Nombres de Métodos e Idioma (`Empleado.java`)
* **Hecho comprobado:** En el commit `bcaecf5` de PR #2 se renombran los atributos de `Empleado.java` de `nombre`/`contraseña` a `name`/`password`, y los métodos a `getname()`, `setname()`, `getpassword()`, `setpassword()`.
* **Inferencia de ingeniería:** Se deduce que este cambio intentaba avanzar sobre la tarea de Issue #1: *"Mejorar nombres de clases, métodos y variables cuando sea necesario"*.
* **Evaluación:** **Gap / Defectuoso.** Provoca error de compilación inmediato (`cannot find symbol`) en clases dependientes como `EmpleadoDAO.java`, `InicioSesionControl.java` y `AgregarEmpleadoControl.java`, que no fueron modificadas y siguen invocando `getNombre()` y `getContraseña()`.

### 3.2 Documentación Técnica de Requerimientos y Arquitectura
* **Hecho comprobado:** El commit `fb276b3` del PR #2 agrega 4 documentos completos en `docs/`: `ARCHITECTURE.md`, `REQUIREMENTS.md`, `SPEC.md` y `MCP_GITHUB_TOOL_INVENTORY.md`.
* **Inferencia de ingeniería:** Se asocia a la tarea de Issue #1: *"Revisar la estructura actual del proyecto"*.
* **Evaluación:** **Covered.** La documentación formaliza exhaustivamente la arquitectura en capas, los requerimientos (RF-01 a RF-27, RNF-01 a RNF-09) y documenta explícitamente en `docs/SPEC.md: Secc. 6.1` el defecto de compilación introducido en `Empleado.java` como deuda técnica conocida.

### 3.3 Verificación de Compilación y Suite de Pruebas
* **Hecho comprobado:** El Issue #1 establece como criterios explícitos que *"El proyecto compila correctamente"* (CA-07) y que *"Las pruebas existentes continúan pasando"* (CA-09).
* **Hecho comprobado vía MCP:** No existen pruebas automatizadas en `src/test/` (solo la clase ejecutable `ProgramaTendejon.java` con el método `main`). La ejecución de `mvn test` reporta 0 pruebas.
* **Evaluación:** **Gap.** El PR #2 no compila limpiamente, y el criterio CA-09 no puede validarse mediante tests existentes al no haber un arnés de pruebas preexistente en el proyecto.
