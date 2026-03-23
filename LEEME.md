🕵️‍♂️ # 1-EXPERIENCIA-SOC---Ejercicio-de-Análisis-de-Tráfico-con-Reporte-de-Incidente

🌎 Contexto
Como analista proactivo en un Centro de Operaciones de Seguridad (SOC), revisas el sistema de Gestión de Eventos e Información de Seguridad (SIEM) y encuentras varias alertas de firmas para el RAT NetSupport Manager desde la IP 45.131.214[.]85 a través del puerto TCP 443. La actividad comenzó el 2026-02-28 a las 19:55 UTC.

Utilizando esta información, recuperas rápidamente una captura de paquetes (pcap) del tráfico de la dirección IP interna que activó estas alertas. ¡Ahora todo depende de ti! Se espera que redactes un informe de incidente para que alguien pueda localizar la computadora infectada y detener esta actividad.

Se detectó actividad maliciosa relacionada con NetSupport Manager RAT desde la dirección IP externa 45.131.214.85 a través del puerto TCP 443. El análisis del tráfico de red identificó un host interno comprometido dentro del segmento de red 10.2.28.0/24.

El programa utilizado para analizar los paquetes fue Wireshark 🦈.​

🌎 Detalles del Entorno
- Rango de LAN: 10.2.28.0/24
- Dominio: easyas123.tech
- Controlador de Dominio: 10.2.28.2
- Puerta de Enlace (Gateway): 10.2.28.1

🔎 Hallazgos:
- IP del Host Infectado: 10.2.28.88
- Dirección MAC Infectada: 00:19:d1:b2:4d:ad
- Dirección IP del Atacante: 45.131.214.85
- Nombre de Host Infectado: DESKTOP-TEYQ2NR
- Cuenta de Windows Infectada: brolf
- Usuario de Windows Infectado: Becka Rolf

⚠️ Actividad: Comunicación persistente sobre TCP 443 (Paquetes TCP ACK)(Retransmisiones)(Patrón de Conexión Persistente).
⚠️ Tipo: Posible Comando y Control (C2). Este comportamiento es consistente con un Troyano de Acceso Remoto (RAT).

🛜 Comportamiento de Red: Se observó una comunicación continua entre el host interno y la IP externa. Se detectó tráfico HTTP previo a la comunicación maliciosa; sin embargo, el vector inicial de infección no pudo ser confirmado.

🧐 Procesos y Filtros:

ip.addr == 45.131.214.85
Con este filtro pude identificar la comunicación persistente y repetitiva desde la IP 45.131.214.85 hacia la IP interna 10.2.28.88 y su dirección MAC.

<img width="903" height="453" alt="image" src="https://github.com/user-attachments/assets/fcd0c441-90fe-4644-9947-89ee961737eb" />

<img width="551" height="129" alt="image" src="https://github.com/user-attachments/assets/c3016487-3385-4409-ad40-751e707654aa" />

nbns
- Este filtro me permite identificar el nombre de host (DESKTOP-TEYQ2NR) que corresponde a la dirección IP 10.2.28.88.

<img width="862" height="125" alt="image" src="https://github.com/user-attachments/assets/26759eb6-0b26-4b06-827a-016b263ceebb" />

kerberos.CNameString
- Este filtro permite identificar el nombre de la cuenta de usuario de Windows (brolf). El apellido del empleado es "Rolf".

<img width="664" height="124" alt="image" src="https://github.com/user-attachments/assets/103ff43f-9469-49d5-a987-5dcd58b2578c" />

<img width="313" height="163" alt="image" src="https://github.com/user-attachments/assets/84a4de7d-a449-4b6b-a912-ce04548ee9f8" />

Con esta informacion, filtro por paquetes que incluyan "Rolf"

<img width="943" height="80" alt="image" src="https://github.com/user-attachments/assets/247e5779-620a-4f54-8185-74997a3fe084" />

<img width="472" height="314" alt="image" src="https://github.com/user-attachments/assets/c12c2572-8655-46d9-87ed-29fc6bfbdc5b" />

😎​ Conclusión:
El host 10.2.28.88 (DESKTOP-TEYQ2NR) muestra un comportamiento consistente con una infección de NetSupport RAT, basado en la comunicación persistente con infraestructura externa conocida. El vector de infección inicial y el usuario asociado no pudieron determinarse de forma definitiva.

🛡️​​ Recomendaciones como Profesional de Ciberseguridad:
1) Contención
- Aislar el host infectado.
- Bloquear la comunicación con la IP maliciosa.

2) Investigación
- Realizar un escaneo completo con antivirus/EDR.
- Verificar mecanismos de persistencia.

3) Seguridad de Credenciales
- Restablecer las credenciales del usuario.

4) Seguridad de Red
- Monitorear patrones de tráfico similares.
- Implementar reglas de detección.

5) Fortalecimiento (Hardening)
- Restringir el tráfico HTTP innecesario (Solo se habilitarán los servidores HTTP utilizados por la empresa).
- Aplicar políticas de filtrado de salida.
