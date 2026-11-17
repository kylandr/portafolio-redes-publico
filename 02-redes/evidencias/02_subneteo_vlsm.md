# Ficha — Subneteo y cálculo de subredes para una topología

**Documento fuente:** `SUBNETEO PACKET TRACER.pdf`
**Autoría:** individual — A.S.P.S.
**Fecha del documento:** no registrada

## Qué es

Ejercicio de direccionamiento: a partir de una topología dada, calcular cuántas subredes hacen falta, cuántos
bits hay que pedir prestados y cuántos hosts utilizables quedan en cada una.

## Objetivo

Dimensionar el direccionamiento antes de configurar la topología, evitando rangos mal calculados o
desperdicio de direcciones.

## Qué se hizo

1. Conteo de subredes necesarias según la topología: **6 subredes**.
2. Cálculo de bits a pedir prestados: **5 bits** para las primeras cuatro subredes y **2 bits** adicionales
   para las dos restantes.
3. Cálculo de hosts utilizables por subred y de las máscaras resultantes.
4. Verificación de que los rangos no se solapan y dejan direcciones para crecimiento.

## Resultados comprobados

- Las 6 subredes calculadas, con su máscara y rango de hosts utilizables, coherentes con la topología del
  ejercicio.
- El direccionamiento se aplica después en el archivo de Packet Tracer del curso.

## Límites

- Es un ejercicio de cálculo: no incluye implementación ni pruebas de tráfico.
- La topología es la del curso, no un diseño propio.
