#  Segundo Parcial - Seguridad de la Información

---

##  Información General
*   **Institución:** Fundación Universitaria Compensar
*   **Asignatura:** Seguridad de la Información
*   **Docente:** Diego Alejandro Barragán
*   **Estudiante:** Fabián Andrés Perilla Manjarrés
*   **Repositorio:** Evaluación 2

---

##  Parte 1: Fase Conceptual

### 1. Footprinting: Reconocimiento Activo vs. Pasivo
| Tipo | Definición | Herramientas comunes |
| :--- | :--- | :--- |
| **Pasivo** | Recolección de información sin interactuar directamente con el objetivo (evita ser detectado). | Google Dorks, Shodan, WHOIS, Redes Sociales. |
| **Activo** | Interacción directa con el sistema objetivo para encontrar vulnerabilidades (alto riesgo de detección). | Nmap, escaneo de puertos, banners grabbing. |

### 2. Exploits, Payloads y Post-Exploits
*   **Exploit:** Fragmento de software o comando que aprovecha una vulnerabilidad para causar un comportamiento no deseado.
    *   *Ejemplo 1:* EternalBlue (MS17-010).
    *   *Ejemplo 2:* Inyección SQL para saltar login.
*   **Payload:** El código que se ejecuta tras una explotación exitosa (la "carga útil").
    *   *Ejemplo 1:* Reverse Shell (Meterpreter).
    *   *Ejemplo 2:* Keylogger para capturar credenciales.
*   **Post-Exploit:** Actividades realizadas tras ganar acceso para mantener persistencia o elevar privilegios.
    *   *Ejemplo 1:* Dump de hashes de contraseñas de la SAM.
    *   *Ejemplo 2:* Movimiento lateral a otros servidores de la red.

### 3. Metasploit en Entornos Reales
**Metasploit** es un framework de pruebas de penetración que permite encontrar, explotar y validar vulnerabilidades.
*   **Uso real:** En auditorías, se usa para simular ataques y visualizar eventos anormales mediante la automatización de exploits conocidos, permitiendo a los administradores parchar sistemas antes de un ataque real.

### 4. Criptografía: Clásica, Cuántica y Post-Cuántica
*   **Clásica:** Basada en algoritmos matemáticos complejos (RSA, AES) difíciles de romper para computadoras actuales.
*   **Cuántica:** Utiliza principios de la física (entrelazamiento) para el intercambio de claves (QKD), siendo teóricamente irrompible.
*   **Post-Cuántica:** Algoritmos matemáticos nuevos diseñados para ser resistentes incluso ante ataques de computadoras cuánticas potentes.

### 5. Blockchain y Criptomonedas
El **Blockchain** es un libro de contabilidad digital descentralizado e inmutable. En las criptomonedas, se utiliza para validar transacciones mediante consenso sin necesidad de un banco central, asegurando que cada moneda solo se gaste una vez.

### 6. Autenticación Biométrica
*   **Protocolos:** FIDO2, BioAPI.
*   **Mecanismos:** Minucias (huella), reconocimiento de iris, patrones vasculares y algoritmos de "liveness detection" para evitar ataques de suplantación.

---

##  Parte 2: Diseño de Estrategia de Defensa (Caso Bitcoin Core)

###  Fase de Pentesting (Simulación)
1.  **Reconocimiento:** Análisis del repositorio GitHub de Bitcoin Core y escaneo de nodos activos.
2.  **Escaneo:** Identificación de versiones de software desactualizadas o puertos vulnerables (8333).
3.  **Explotación:** Intento de inyección de código en el repositorio o ataques DoS a los nodos.
4.  **Post-explotación:** Intento de exfiltración de llaves privadas o manipulación del consenso.

###  Matriz de Amenazas
| Vector de Ataque | Impacto | Probabilidad | Contramedida |
| :--- | :--- | :--- | :--- |
| Ataque a Cadena de Suministro (GitHub) |  Muy Alto |  Baja | Firmas PGP obligatorias en cada commit. |
| Vulnerabilidad en Nodo (Buffer Overflow) |  Alto |  Media | Actualización automática y sandboxing del proceso. |
| Ataque de Doble Gasto |  Muy Alto |  Baja | Aumento del número de confirmaciones requeridas. |

---

##  Parte 3: Parte Empírica (Blockchain de Mensajería)

###  Implementación de la Cadena de Bloques
Se propone una estructura donde cada bloque contiene un mensaje cifrado entre el Servidor A y el Servidor B.

**¿Cómo se crea un bloque?**
1.  **Datos:** El contenido del mensaje.
2.  **Timestamp:** Fecha y hora exacta.
3.  **Hash Anterior:** Enlace al bloque previo para asegurar la cadena.
4.  **Nonce:** Número utilizado para el proceso de Minería (Proof of Work).
5.  **Hash Actual:** Identificador único generado mediante SHA-256.

###  Criptografía en Blockchain
El tipo de encriptación predominante es la **Criptografía de Curva Elíptica (ECC)** para las llaves públicas/privadas y funciones **Hash (SHA-256)** para la integridad de los bloques.
```python
# Ejemplo conceptual de creación de bloque
import hashlib

def crear_hash(index, prev_hash, data):
    block_string = f"{index}{prev_hash}{data}"
    return hashlib.sha256(block_string.encode()).hexdigest()
