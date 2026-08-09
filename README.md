# Lista de licencias anuladas

Este repositorio publica **un solo archivo**: `anuladas.txt`.

Es la lista de licencias que dejaron de ser válidas antes de su fecha de vencimiento. El
programa que las usa la consulta cuando sale a internet y bloquea la suya si aparece en ella.

## Qué contiene, y qué no

`anuladas.txt` es un JSON firmado, codificado en base64, con este formato:

```
<json en base64>.<firma en base64>
```

El JSON de adentro tiene exactamente dos cosas:

```json
{"anuladas": [{"id": "1b5192658d11"}], "emitida": "2026-08-09T23:12:57+00:00"}
```

- `id` — el código de la licencia. Doce caracteres al azar, sin significado.
- `emitida` — cuándo se generó esta versión de la lista.

**No hay nombres, ni empresas, ni fechas de contrato, ni motivos.** Un código no dice de quién
es ni por qué se anuló. Este archivo es público y queda en el historial de git para siempre, así
que no lleva nada que no pueda estar ahí dentro de diez años.

## Por qué va firmado

Sin firma, cualquiera podría hacer que el programa leyera una lista vacía —redirigiendo la
dirección con el archivo `hosts`, un DNS o un proxy— y la anulación desaparecería. Peor todavía:
alguien podría publicar una lista falsa con los códigos de las licencias buenas y dejar sin
trabajar a quien sí pagó.

La firma es **Ed25519**. La clave pública va compilada dentro del programa; la privada no sale
de la máquina de quien emite las licencias. Por eso da igual dónde esté publicado este archivo:
el lugar no necesita ser de confianza, porque una lista que no verifique se descarta.

## Cómo se actualiza

Se reemplaza el archivo entero, no se le agregan líneas: es una foto completa de las licencias
anuladas en ese momento. Sacar una de la lista y volver a publicar la reactiva.

El programa se queda siempre con la versión de `emitida` más reciente entre la que baja y la que
tiene guardada, así que servir una copia vieja —firmada de verdad, pero anterior a una
anulación— no sirve para revivir nada.
