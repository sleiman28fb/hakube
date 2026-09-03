# Carné digital de Hakübe

La página que ve un veterinario cuando escanea el QR de una mascota.

**Es solo presentación.** No hay llaves, no hay lógica de permisos y no hay base de datos
acá adentro: la página le pide los datos a una Edge Function de Supabase, que es la única
que decide si un token vale y qué muestra. El código de la app vive en otro repositorio,
privado.

## Por qué está en un repositorio aparte

Las Edge Functions de Supabase no pueden servir HTML en el dominio compartido
`*.supabase.co`: un `GET` que devuelva `text/html` llega reescrito a `text/plain`, con
`nosniff`, y quien escanea ve el código fuente en lugar del expediente. Es una defensa
contra páginas engañosas y solo la levanta un dominio propio de pago.

Así que la función devuelve datos y esta página los pinta. Vive en su propio repositorio
porque GitHub Pages no publica desde repositorios privados sin plan pago, y el código del
producto no tiene por qué ser público para que esta página lo sea.

## El token va en el fragmento

El QR apunta a `…/#t=EL_TOKEN`, no a `…?t=EL_TOKEN`. Los fragmentos no se mandan al
servidor: quien hospeda esta página nunca ve el código que abre el expediente de una
mascota. Solo lo lee el navegador para pedírselo a Supabase, que sí tiene que recibirlo
porque es quien decide si vale.

## Cómo se actualiza

No se edita acá. La fuente está en `sitio/` del repositorio del producto y se publica con
`scripts/publicar-sitio.sh`. Un cambio hecho directamente en este repositorio se pierde en
la siguiente publicación.
