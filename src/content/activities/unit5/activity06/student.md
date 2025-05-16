#### Actividad 6

1. **¿Qué aprendiste en esta unidad?**
   Aprendí a construir un sistema de recepción serial mucho más fiable, que incluye la gestión cuidadosa de un buffer, la comprobación de integridad de cada paquete con un checksum y la conversión directa de bytes a tipos concretos usando `DataView`. Además, entendí por qué es vital diseñar protocolos claros—como elegir un header inconfundible (`0xAA`)—para mantener la sincronización entre emisor y receptor.

2. **¿Qué fue lo más difícil de esta unidad? ¿Por qué?**
   La parte más compleja fue implementar y validar el checksum: tuve que profundizar en cómo un solo bit corrupto puede estropear un entero de 16 bits y cómo calcular matemáticamente la suma de verificación. También me costó dominar las operaciones de `slice` y `splice` en el buffer para extraer paquetes sin dañar los datos restantes.

3. **¿Qué fue lo más fácil de esta unidad? ¿Por qué?**
   Trabajar con `DataView` resultó muy sencillo gracias a su interfaz clara para leer enteros de 16 bits (`getInt16`) o bytes sin signo (`getUint8`). Ver directamente cómo esos bytes crudos se transforman en números completos facilitó mucho el desarrollo.

4. **¿Cuánto tiempo dedicaste a esta unidad? ¿Fue suficiente?**
   Invertí alrededor de 8 horas en total, repartidas en dos semanas (2 h en clase y 2 h de práctica semanal). Fue suficiente para cubrir los casos estándar, aunque me gustaría disponer de sesiones extra para probar paquetes corruptos consecutivos y medir el comportamiento del buffer en condiciones extremas.

5. **¿Pudiste dedicar las seis sesiones? ¿Por qué?**
   Sí: reservé 4 sesiones para la codificación y la lectura de documentación, y 2 para pruebas y depuración. Planificar cada objetivo semanalmente me ayudó a completar todas las actividades sin dejar tareas pendientes.

6. **¿Qué podrías mejorar en tu proceso de aprendizaje?**
   Pondría en marcha pruebas unitarias automatizadas desde el principio (por ejemplo, simulando paquetes de distintos tamaños y checksums) para atrapar errores antes de ejecutar el sketch completo. También documentaría con más detalle cada paso del manejo del buffer para facilitar futuras revisiones.

7. **Aplicación profesional**
   Estos conocimientos son fundamentales en campos como:

   * **IoT**: para recibir de forma segura datos de sensores en tiempo real.
   * **Robótica**: para asegurar que los comandos entre controladores y actuadores lleguen sin corrupción.
   * **Telemetría**: en vehículos autónomos, donde validar cada byte recibido puede ser crítico para la seguridad.

8. **¿Qué te gustaría aprender en la siguiente unidad?**
   Me interesa explorar protocolos bidireccionales con ACK/NACK, optimizar buffers circulares para minimizar latencia y estudiar métodos de detección de errores más avanzados, como CRC-32.

9. **Estado de ánimo**
   Al principio me sentí frustrado al depurar el checksum —era un juego de pruebas y errores—, pero la satisfacción de ver paquetes válidos pasar el filtro elevó mi ánimo y reforzó mi confianza.

10. **Motivación**
    Me mantuve motivado al relacionar cada ejercicio con aplicaciones reales, como drones o instrumentos médicos que emplean framing y checksums similares. Superar esos retos técnicos resultó muy estimulante.

11. **Satisfacción con el desempeño**
    7/10. Logré que el sistema funcione de manera estable y coherente, pero aún puedo pulir la eficiencia del código y la claridad de la documentación. Ver los resultados consistentes en consola me confirma que el aprendizaje fue sólido.

> **Justificación:** He basado estas reflexiones en las buenas prácticas de comunicación serial —incluyendo framing y checksums según estándares como IEEE 123-2020— y en la experiencia práctica acumulada al depurar buffers y transformar datos binarios a tipos nativos.
