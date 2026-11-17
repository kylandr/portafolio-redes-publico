# Ficha — Cifrado de archivos y volúmenes (VeraCrypt, EFS, OpenSSL)

**Documento fuente:** informe de laboratorio del proyecto
**Autoría:** A.S.P.S. (según la portada del informe)
**Fecha del documento:** noviembre de 2026

## Qué es

Informe técnico y guía de ejecución de cifrado en reposo para información confidencial (nómina y
contabilidad) usando tres mecanismos distintos, comparando qué protege cada uno.

## Objetivo

Demostrar cómo se protege la información cuando el disco o el archivo se ve comprometido: volumen cifrado,
cifrado a nivel de sistema de archivos y cifrado de archivos sueltos con contraseña.

## Entorno

Windows con EFS y VeraCrypt; OpenSSL en línea de comandos; unidad cifrada montada como `Z:`; usuario
auditor sin permisos sobre la unidad protegida.

## Qué se hizo

1. Creación de un contenedor cifrado con VeraCrypt y montaje en `Z:` con la nómina dentro.
2. Cifrado de archivos con EFS y configuración de atributos avanzados del archivo.
3. Cifrado de archivos con OpenSSL (clave simétrica) como alternativa portable.
4. Prueba de acceso con un segundo usuario (auditor): lectura del contenido denegada.
5. Comparación de alcance: volumen completo vs. archivo individual, y qué pasa si se copia el archivo fuera.

## Resultados comprobados

- La unidad cifrada se monta con contraseña y sin ella el contenido no es accesible.
- El usuario auditor no puede abrir los archivos cifrados con EFS (error de acceso denegado).
- Los archivos cifrados con OpenSSL no se pueden leer sin la clave.
- Capturas de cada paso en la carpeta de evidencias original (10 imágenes, conservadas en local).

## Límites

- No se probó recuperación de datos ante pérdida de clave ni borrado seguro del volumen.
- No hay medición de impacto en rendimiento del cifrado.
