"# Proyecto-II---Innovacion-Tecnologica" 
## UNIVERSIDAD ICESI - MIAA
<img width="242" height="148" alt="icesi" src="https://github.com/user-attachments/assets/2244ac43-8ae7-4252-b648-58af28b38587" />

# Agente de IA para Gestión de Vulnerabilidades

Sistema orientado a la **priorización, análisis y recomendación de remediación de vulnerabilidades** en entornos **IT, OT, Cloud y AppSec**, usando un enfoque combinado de:

- **scoring determinístico explicable**
- **enriquecimiento contextual**
- **RAG (Retrieval-Augmented Generation)**
- **LLM** para explicación, justificación y apoyo a la toma de decisiones

---

## 1. Propósito del proyecto

Este proyecto busca construir un **Agente de IA para Gestión de Vulnerabilidades** capaz de:

- ingerir hallazgos desde múltiples herramientas de seguridad,
- normalizar la información en un modelo canónico,
- enriquecer cada hallazgo con contexto de negocio y amenaza,
- calcular un **riesgo operativo real**,
- recomendar acciones de remediación o mitigación,
- y generar salidas útiles para SOC, infraestructura, OT, AppSec, gestión de cambios y dirección.

La iniciativa está pensada para evolucionar desde un **motor de priorización asistido por IA** hasta una plataforma de **orquestación inteligente de vulnerabilidades**.

---

## 2. Problema que resuelve

En la mayoría de organizaciones, la gestión de vulnerabilidades presenta estos problemas:

- gran volumen de hallazgos sin contexto suficiente,
- priorización basada solo en **CVSS**, lo cual no representa el riesgo real,
- separación entre detección, priorización y remediación,
- baja integración entre herramientas IT, OT, cloud y desarrollo,
- dificultad para traducir datos técnicos a decisiones operativas y ejecutivas.

Este proyecto responde a ese problema con una arquitectura que prioriza por:

- **severidad técnica**
- **probabilidad de explotación**
- **explotación activa conocida**
- **criticidad del activo**
- **exposición a internet**
- **reachability**
- **ruta de ataque**
- **disponibilidad de parche**
- **antigüedad del hallazgo**
- **controles compensatorios**

---

## 3. Objetivos

### Objetivo general
Diseñar e implementar un agente de IA para apoyar la gestión integral de vulnerabilidades mediante correlación de hallazgos, priorización basada en riesgo y recomendación de acciones de remediación o mitigación.

### Objetivos específicos
- Definir un **modelo canónico de datos** para hallazgos, activos y remediaciones.
- Consolidar una **base de conocimiento** sobre herramientas, métricas y reglas de decisión.
- Implementar un **motor de scoring explicable**.
- Diseñar prompts y estructura RAG para enriquecer el razonamiento del agente.
- Producir una salida auditable para operación, gestión y dirección.
- Preparar la solución para integración futura con scanners, ITSM, CMDB y plataformas de parcheo.

---

## 4. Alcance

El proyecto cubre los siguientes dominios:

- **IT Infrastructure**
- **Endpoints**
- **Cloud / CNAPP / Exposure**
- **OT / ICS / XIoT**
- **Application Security / DevSecOps**
- **Patch & Remediation**
- **Exposure Management / RBVM**

No reemplaza inicialmente las herramientas existentes de detección; su función es servir como capa de:

1. **normalización**
2. **enriquecimiento**
3. **priorización**
4. **explicación**
5. **recomendación**

---

## 5. Arquitectura conceptual

```text
+------------------------+
| Fuentes de hallazgos   |
|------------------------|
| Tenable / Qualys       |
| Rapid7 / Defender      |
| Wiz / Armis / Claroty  |
| Snyk / Checkmarx       |
+-----------+------------+
            |
            v
+------------------------+
| Capa de Ingesta        |
|------------------------|
| conectores / ETL / API |
+-----------+------------+
            |
            v
+------------------------+
| Normalización          |
|------------------------|
| modelo canónico        |
| finding / asset / fix  |
+-----------+------------+
            |
            v
+------------------------+
| Enriquecimiento        |
|------------------------|
| EPSS / KEV / criticidad|
| exposición / attackpath|
+-----------+------------+
            |
            v
+------------------------+
| Motor de scoring       |
|------------------------|
| reglas explicables     |
| risk_score 0-100       |
+-----------+------------+
            |
            v
+------------------------+
| LLM + RAG              |
|------------------------|
| explicación            |
| recomendación          |
| resumen ejecutivo      |
+-----------+------------+
            |
            v
+------------------------+
| Integraciones          |
|------------------------|
| ITSM / CMDB / SIEM     |
| Patch / CAB / SOC      |
+------------------------+
```

---

## 6. Componentes actuales del repositorio

- `Base_conocimiento_agente_vulnerabilidades.json`
- `Catalogo_herramientas_vulnerabilidades.csv`
- `Catalogo_metricas_vulnerabilidades.csv`
- `Ejemplo_scoring_agente.csv`
- `Diseno_prompts_agente_vulnerabilidades.md`
- `Esquema_RAG_agente_vulnerabilidades.md`
- `reglas_scoring_agente.py`
- `ejemplo_findings.json`
- `ejemplo_assets.json`

---

## 7. Modelo canónico de datos

### VulnerabilityFinding
- `finding_id`
- `source_tool`
- `cve_id`
- `title`
- `asset_id`
- `cvss`
- `epss`
- `kev_flag`
- `internet_exposed`
- `reachable`
- `attack_path`
- `patch_available`
- `first_seen`
- `last_seen`
- `finding_status`
- `recommended_action`

### Asset
- `asset_id`
- `hostname`
- `asset_type`
- `environment`
- `business_owner`
- `service_owner`
- `criticality`
- `data_sensitivity`
- `internet_exposed`
- `ot_it_flag`
- `compensating_controls`

### RemediationAction
- `action_id`
- `finding_id`
- `action_type`
- `patch_or_mitigation`
- `effort_level`
- `outage_required`
- `reboot_required`
- `target_sla_days`
- `validation_required`
- `status`

---

## 8. Enfoque de scoring

El modelo actual usa una escala de **0 a 100** e incorpora factores como:

- **CVSS**
- **EPSS**
- **KEV**
- **exposición a internet**
- **reachability**
- **presencia en ruta de ataque**
- **criticidad del activo**
- **antigüedad del hallazgo**
- **disponibilidad de parche**
- **controles compensatorios**

### Bandas de riesgo
- `critical`
- `high`
- `medium`
- `low`

### Acciones sugeridas
- `patch_now_or_mitigate_immediately`
- `remediate_priority`
- `schedule_fix`
- `monitor_or_accept`

---

## 9. Estructura RAG recomendada

Colecciones sugeridas:

- `tool_profiles`
- `metric_definitions`
- `decision_rules`
- `remediation_playbooks`
- `asset_criticality_policy`
- `exception_policies`
- `sla_policies`

Metadatos recomendados:

- `doc_type`
- `domain`
- `environment`
- `source_tool`
- `severity_band`
- `risk_band`
- `review_date`
- `owner_team`

---

## 10. Flujo operativo esperado

1. Recibir hallazgos desde scanners o plataformas RBVM.
2. Mapearlos al modelo canónico.
3. Cruce con inventario, criticidad y datos contextuales.
4. Enriquecer con señales como `EPSS`, `KEV` y exposición.
5. Calcular `risk_score`.
6. Invocar LLM para resumir, justificar y recomendar.
7. Integrar la salida con ITSM, ticketing, CMDB, patching y reporting ejecutivo.

---

## 11. Ejemplo de uso

### Ejecutar el script de scoring
```bash
python reglas_scoring_agente.py
```

### Resultado esperado
El script devuelve una estructura con:

- `risk_score`
- `risk_band`
- `recommended_action`
- `target_sla_days`
- `reasons`

---

## 12. Casos de uso prioritarios

- Priorización de vulnerabilidades críticas en activos expuestos.
- Gestión diferencial de vulnerabilidades IT vs OT.
- Identificación de hallazgos con explotación activa conocida.
- Recomendación de mitigaciones cuando no existe parche.
- Resumen ejecutivo para CISO / Gerencia.
- Apoyo a mesa de remediación y gestión de cambios.
- Preparación de backlog accionable por equipo técnico.

---

## 13. Herramientas objetivo para integración futura

### VM / RBVM
- Tenable
- Qualys
- Rapid7
- Microsoft Defender Vulnerability Management
- Cisco Vulnerability Management

### Cloud / Exposure
- Wiz
- Microsoft Security Exposure Management
- Tenable Exposure Management

### OT / CPS
- Armis
- Claroty
- Nozomi Networks

### AppSec / DevSecOps
- Snyk
- Checkmarx
- Veracode
- GitHub Advanced Security

### Patch / Remediation
- BigFix
- Automox
- Tanium

---

## 14. Roadmap propuesto

### Fase 1
- Modelo canónico
- Base de conocimiento
- Reglas de scoring
- README técnico
- Dataset de ejemplo

### Fase 2
- Integración con fuentes reales
- APIs / ETL
- Conectores con CMDB e ITSM
- Reglas SLA por dominio

### Fase 3
- RAG operativo
- Prompting avanzado por rol
- Explicaciones ejecutivas y técnicas
- Dashboard de backlog y riesgo

### Fase 4
- Orquestación semi-automática
- Recomendación de cambio
- Validación post-remediación
- Aprendizaje a partir de decisiones humanas

---

## 15. Métricas de éxito del proyecto

- reducción del backlog crítico,
- disminución del MTTR,
- aumento del porcentaje de cierre dentro de SLA,
- mejor cobertura de priorización contextual,
- menor ruido operativo,
- mejor trazabilidad de decisiones.

---

## 16. Requisitos técnicos recomendados

- Python 3.10+
- entorno virtual (`venv`)
- bibliotecas de manejo JSON/CSV
- futura integración con APIs REST, motores vectoriales, LLM APIs, bases documentales y sistemas ITSM/CMDB

---

## 17. Consideraciones de seguridad

- no exponer datos sensibles del inventario,
- proteger credenciales de integraciones,
- versionar reglas y políticas,
- registrar trazabilidad de decisiones,
- mantener override humano,
- diferenciar claramente **riesgo** de **severidad**.

---

## 18. Contribución

Las futuras contribuciones deberían enfocarse en:

- nuevos conectores,
- mejoras del scoring,
- playbooks por tecnología,
- taxonomías RAG,
- plantillas de remediación,
- reportes ejecutivos.

---

## 19. Licencia

Definir según el modelo institucional del proyecto.

Opciones habituales:
- MIT
- Apache 2.0
- licencia propietaria institucional

---

## 20. Autor / contexto del proyecto

Proyecto orientado al diseño de un **Agente de IA para Gestión de Vulnerabilidades**, con aplicabilidad en contextos empresariales donde conviven dominios IT, OT, cloud y desarrollo seguro.

---

## 21. Próximos pasos recomendados

- conectar fuentes reales de hallazgos,
- definir política de criticidad de activos,
- establecer reglas SLA por dominio,
- integrar con ITSM,
- construir dashboard de riesgo,
- desplegar una primera versión piloto del agente.

---

## 22. Resumen ejecutivo

Este repositorio constituye la base de un **sistema inteligente de gestión de vulnerabilidades**.

Su valor no está en reemplazar scanners existentes, sino en convertir hallazgos dispersos en **decisiones priorizadas, explicables y accionables**.

La propuesta combina:
- datos estructurados,
- reglas transparentes,
- contexto operacional,
- capacidades de IA generativa.

El resultado esperado es una gestión de vulnerabilidades más madura, trazable y orientada al riesgo real.

## 👨‍💻 Integrantes del Grupo

Este proyecto está siendo desarrollado por los siguientes alumnos de la Maestría:

| Nombre | Correo | Usuario |
|:-------|:--------|:--------|
| [Cristian Quebrada](https://github.com/cris-bytes) | criistianq90@gmail.com| @cris-bytes|
| [Edwin  Perez Lozano](https://github.com/poppetmaster) | edwinandperez@gmail.com| @poppetmaster|
| [Rubén Darío Sabogal Urbano](https://github.com/rubenesticesi) | 16704992@u.icesi.edu.co| @rubenesticesi|

---
## 🧠 Métodos Utilizados

- Estadística inferencial 
- Aprendizaje automático (Machine Learning)  
- Visualización de datos  
- Modelado predictivo  
- Análisis exploratorio  
- ETL (Extracción, Transformación y Carga de datos)

---

## 🧰 Tecnologías

- **Lenguajes:** Python 
- **Herramientas:** Pandas, NumPy, Jupyter Notebooks
- **Otros:** GitHub

---

## 📓 Cuadernos del Proyecto (Notebooks)

El análisis y desarrollo de los modelos se encuentran documentados en los siguientes cuadernos:

* **Cuaderno 1 (Colab):** [Análisis Exploratorio y Preprocesamiento de Datos](https://colab.research.google.com/drive/1rUkTJ5FIrboe7dDwUMuLb76V952u38q3?usp=sharing)
* **Cuaderno 2 (Colab):** [Desarrollo y Evaluación de Modelos Predictivos](https://colab.research.google.com/drive/1KhpMslnJGm7HGy2X7283wjlAE9kbvd7I?usp=sharing)
* **Cuaderno 3 (GitHub):** [Notebook SIMAT](https://github.com/rubenesticesi/aprendizajeautomatico1/blob/master/simat.ipynb)
* **Cuaderno 4 (GitHub):** [INFORME 2] (https://github.com/rubenesticesi/Proyecto-I-innovacion-tecnologica-IA/tree/main/INFORME%202](https://github.com/rubenesticesi/Proyecto-I-innovacion-tecnologica-IA/blob/main/INFORME%202/segunda_entrega_EDA_y_modelos_Run.ipynb)
* **Cuaderno 5 (GitHub):** [INFORME FINAL] (https://github.com/rubenesticesi/Proyecto-I-innovacion-tecnologica-IA/blob/main/INFORME%20FINAL/Informe_Final_Proyecto_TEA.ipynb)
* **Video:** (https://youtu.be/m-EO7Ex7vW4)
  
---

## 📄 Licencia

Este proyecto se distribuye bajo licencia [MIT](https://opensource.org/licenses/MIT) o la que defina la universidad.

---

## 🏫 Universidad Icesi – Maestría en Inteligencia Artificial Aplicada

Proyecto académico desarrollado en el marco del curso **Proyecto II – Innovación Tecnológica**.  
**Cali, Colombia – 2026**
