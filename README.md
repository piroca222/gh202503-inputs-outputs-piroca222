Exercise
# Ejercicio Combinado: Inputs y Outputs en GitHub Actions

## Objetivo

Explorar las formas de proveer entradas (inputs) y establecer salidas (outputs) en flujos de trabajo de GitHub Actions.

## Tareas

1. Crear un archivo llamado `input-outputs.yml` en la carpeta `.github/workflows` en la raíz del repositorio. Los datos del flujo de trabajo deben ser los siguientes:

   - **Nombre del workflow:** Inputs and Outputs.
   - **Desencadenantes:** 
     - `workflow_dispatch` con los siguientes inputs:
       - `dry-run`: booleano, valor predeterminado `false`, descripción: "Skip deployment and only print build output".
       - `target`: environment, requerido, descripción: "Which environment the workflow will target".
       - `tag`: choice, opciones `v1`, `v2`, `v3`, valor predeterminado `v3`, descripción: "Release from which to build and deploy".
       - `build-status`: choice, opciones `success`, `failure`, valor predeterminado `success`, descripción: "Choose the build status for the demo".

2. **Trabajos:**
   - **build**:
     - Se ejecuta en `ubuntu-latest`.
     - Incluye los siguientes pasos:
       1. **Print GITHUB_OUTPUT path:** Imprimir el valor de la variable de entorno `GITHUB_OUTPUT`.
       2. **Build:** 
          - Definir un id llamado `build`.
          - Escribir dos salidas en el archivo de salida de GitHub Actions:
            - `tag=<valor del input tag>`.
            - `status=<valor del input build-status>`.
     - Proveer las siguientes salidas:
       - `build-status`: contiene el valor de la salida `status` del paso con ID `build`.
       - `tag`: contiene el valor del input `tag`.

   - **deploy**:
     - Se ejecuta en `ubuntu-latest`.
     - Condiciones:
       - Se ejecuta solo si:
         - El trabajo `build` se completó exitosamente.
         - La entrada `dry-run` es `false`.
         - La salida `build-status` del trabajo `build` es `success`.
     - Opciones:
       - Configura el entorno con el valor del input `target`.
     - Pasos:
       1. **Deploy:** Imprimir el mensaje: "Deploying to '<valor del target>' using tag '<valor del tag>'".

3. Confirmar los cambios y hacer `push` del código. Luego, desencadenar el flujo de trabajo desde la interfaz de usuario (UI), proporcionando diferentes combinaciones de valores para los inputs.

4. Inspeccionar los resultados de las ejecuciones del flujo de trabajo:
   - ¿Cómo afectaron los valores de `dry-run`, `target`, `tag` y `build-status` a los resultados de los trabajos `build` y `deploy`?
   - Reflexionar sobre cómo se utilizaron las salidas (`outputs`) entre los trabajos.
  

## Tips

Para agregar valores a un archivo, podemos usar la siguiente sintaxis: `echo "<contenido de la línea>" >> "<ruta del archivo>"`

Por ejemplo, para agregar el valor de una entrada a GITHUB_OUTPUT, usa:
  ```bash
echo "status=${{ inputs.build-status }}" >> $GITHUB_OUTPUT
   ```
