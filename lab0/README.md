# Laboratorio 0: Protocolos de Aplicación - hget
### 🎓 Redes y Sistemas Distribuidos 2026 | FAMAF, UNC

Este repositorio contiene la implementación de **hget**, un cliente HTTP/1.0 desarrollado en Python 3. El proyecto se enfoca en la comprensión profunda de la programación de sockets, la resolución de nombres (DNS) y la interacción con protocolos de la capa de aplicación.

---

## 🎯 Objetivos y Alcance
[cite_start]El objetivo principal es aplicar la comunicación cliente/servidor mediante sockets desde la perspectiva del cliente, siguiendo las especificaciones de los RFC correspondientes[cite: 21, 22]:

* **HTTP/1.0 (RFC 1945):** Implementación de una petición GET, manejo de headers y recepción del body.
* **DNS (RFC 1035):** Implementación de un cliente DNS propio sobre UDP para resolver hostnames mediante el servidor Quad9 (9.9.9.9).
* [**Sockets:** Uso explícito de la biblioteca `socket` de Python, evitando abstracciones de alto nivel para comprender el flujo real de datos.

---

## 🛠️ Tecnologías y Requisitos
* **Lenguaje:** Python 3.
* **Protocolos:** TCP (para HTTP) y UDP (para DNS).
* **Métricas de Calidad:** El código cumple con los siguientes estándares obligatorios:
    * **PEP 8:** Estilo de código conforme a la guía de Python.
    * **Análisis estático:** Cero incidencias reportadas por `ruff`.
    * **Complejidad Ciclomática:** Ninguna función supera el límite de McCabe establecido.
    * **Cobertura:** Cobertura de tests mínima garantizada según `pyproject.toml`.

---
