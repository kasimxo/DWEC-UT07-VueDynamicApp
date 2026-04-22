# TAREA 7: App dinámica con Vue/Nuxt (API Rick & Morty)

## Objetivo

Desarrollar una aplicación web dinámica utilizando Vue 3 o Nuxt que consuma datos de forma asíncrona desde una API pública y actualice la interfaz sin recargar la página.

## API a utilizar

Se deberá utilizar obligatoriamente la API pública:
https://rickandmortyapi.com/

### Endpoints recomendados:

/character → personajes
/location → localizaciones
/episode → episodios

## Descripción de la tarea

El alumnado desarrollará una aplicación que permita consultar información de personajes
de forma dinámica.

## Funcionalidades mínimas

La aplicación deberá incluir:
- Búsqueda de personajes (por nombre)
- Filtros (estado o especie)
- Listado de personajes:
    - nombre
    - imagen
    - estado
    - especie
- Vista detalle de un personaje
- Paginación (incluida en la API)

## Estados de la aplicación
La aplicación deberá gestionar correctamente:
- Estado de carga (cargando datos)
- Estado de error (fallo en la API)
- Sin resultados

## Estructura recomendada

Se recomienda organizar el proyecto en componentes:
- App.vue
- components/
    - Buscador.vue
    - ListaPersonajes.vue
    - TarjetaPersonaje.vue
    - DetallePersonaje.vue
    - Filtros.vue

## Requisitos técnicos obligatorios

1. Comunicación asíncrona (CE7_c, CE7_e)

- Uso de fetch o axios
- Uso de async/await o then/catch
- Manejo de errores

2. Actualización dinámica del DOM (CE7_e)

Uso de directivas de Vue:
- v-for → listar datos
- v-if / v-show → estados
La página NO debe recargarse.

3. Uso de objetos, propiedades y métodos (CE7_d)

Uso de:
- fetch / axios
- response.json()
- Propiedades de la API ( name , status , etc.)

4. Formatos de datos (CE7_f)

- Uso de JSON
- Interpretación correcta de los datos

5. Compatibilidad (CE7_g)

La aplicación debe funcionar en:
- Google Chrome
- Mozilla Firefox

6. Uso de frameworks/librerías (CE7_h)

Obligatorio:
- Vue 3

7. Desarrollo y documentación (CE7_i)

Se deberá entregar:
- Código fuente (ZIP o repositorio)
- Documento explicativo (PDF)

## Recomendaciones

- Empieza por mostrar datos en pantalla
- Luego añade búsqueda
- Después filtros
- Por último, detalle y mejoras

Ve paso a paso, no intentes hacerlo todo de golpe.

## Objetivo final

Construir una aplicación web dinámica real que:
- Consuma una API
- Use asincronía correctamente
- Actualice la interfaz sin recargar
- Esté organizada en componentes