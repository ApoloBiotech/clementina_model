# clementina_model
Biomarker discovery and Prediction

# Título del Proyecto: Desarrollo de biomarcadores y modelos predictivos asociados a distintos estreses en cultivos de interés agronómico utilizando RNAseqs públicos

Procesar y analizar metadatos de muestras de RNAseq públicos en soja y tomate, enfocándose en la cuantificación precisa de la expresión génica para asociar estas muestras con distintos tipos de estrés (abiótico y biótico) y tejidos específicos.

-Identificación de biomarcadores mediante uso de modelos lineales:
Utilizar métodos estadísticos supervisados y lineales, como Sparse Partial Least Squares Discriminant Analysis (sPLS-DA) y LASSO, para identificar genes y patrones de expresión génica asociados a diferentes tipos de estrés. Estos modelos permitirán seleccionar biomarcadores robustos basados en su relevancia estadística y su capacidad explicativa en los datos disponibles.

-Predicción de respuestas al estrés mediante modelos no lineales:
Implementar modelos avanzados de aprendizaje automático, incluyendo redes neuronales y métodos de ensamble como Random Forest, para construir modelos predictivos capaces de clasificar muestras y predecir respuestas a estrés. Estos modelos no lineales aprovecharán relaciones complejas y no evidentes en los datos, mejorando la precisión y aplicabilidad en escenarios agrícolas.




# Monitoreo, usado en Serafin CCAD https://wiki.ccad.unc.edu.ar/ :

### Scripts con scheduler (one repeats and loops checking jobs , other mapping scripts send jobs when needed automaticaly):
## in DownloadProcessData folder:

kallisto_mapping_sortOlder.sh

repeat_loop_kallisto_mappingOlder_v2.sh

### Pseudocódigo  Descarga y Procesamiento Concurrente

    Se intentan descargas con hasta 3 intentos por archivo.
    Cada 10 minutos, se lanza un job de alineamiento solo para archivos descargados completamente (identificados por el prefijo SAM).
    Se asegura la ejecución en paralelo de descargas y alineamientos.

# Configuración inicial
1. Definir archivo y nombres de ejecución (run) y muestras (sample).
2. Establecer dirs para guardar:
   - Archivos FASTQ 
   - Registros (logs).
   - Resultados del alineamiento Kallisto.
3. Establecer el número máximo de reintentos para la descarga. (son 3 ahora)
4. Configurar el intervalo para verificar descargas y lanzar alineamientos a cola SLURM (e.g., cada 10 minutos).

# Proceso principal
Mientras haya líneas en el archivo de entrada:
   Leer `run_name` y `sample_name` desde el archivo.

   # Descargar archivos FASTQ
   Inicializar `attempt = 0` y `success = 0`.

   Mientras `attempt < MAX_RETRIES`:
      Intentar descargar con `fastq-dump` usando el comando:
      ```bash
      fastq-dump --split-3 --outdir FASTQ_DIR run_name
      ```
      Si la descarga es exitosa:
         Establecer `success = 1` y salir del bucle.
      Si falla:
         Incrementar `attempt`.
         Mostrar mensaje de error y reintentar.

   Si todos los intentos fallan:
      Intentar descargar con `prefetch`.
      Si `prefetch` tiene éxito:
         Reintentar `fastq-dump`.
      Si también falla:
         Registrar el error y continuar con la siguiente muestra.

   # Renombrar archivos descargados
   Para cada archivo FASTQ descargado:
      - Verificar existencia de los archivos `_1.fastq`, `_2.fastq` o `.fastq`.
      - Renombrar con el prefijo `sample_name`.

   Marcar archivos completados con el prefijo **SAM**.

   Registrar la finalización de la descarga en el log.

# Lanzar jobs de alineamiento
Cada 10 minutos:
   Identificar archivos con el prefijo **SAM** en el directorio.
   Para cada archivo identificado:
      - Verificar que no esté siendo procesado actualmente.
      - Iniciar el proceso de alineamiento:
        ```bash
        kallisto quant -i index_file -o output_dir -t NUM_THREADS input_fastq
        ```
    Registrar la ejecución en el log.

# Monitoreo
   Scripts que:
      - Detectan archivos incompletos y los excluyan.
      - Reintentan descargas fallidas (hasta el máximo configurado).
      - Supervisan los logs para detectar fallas en tiempo real.

# Finalización
   Una vez que todas las descargas y alineamientos estén completos:
      - Limpiar directorios temporales (fastq y sra files).
      - Generar un resumen de errores, tiempos y resultados.





### List of SRR ids:

2167part00.txt

2172part_01.txt

2240part_03.txt

2322part_02.txt

2345part_04.txt


### bash code to run  in "screen" to download in parralele, does 3 attempts per sun:


downloadFastq_part00.sh

downloadFastq_part01.sh

downloadFastq_part02.sh

downloadFastq_part03.sh

downloadFastq_part04.sh


### Script to run modeling (script4) needs adjustments
## In Modeling folder:

model_4.py       (runs random forest and gradient boost code)
model_6.py       (run Neural Networks Tensor-flow ScikitLearn code)







