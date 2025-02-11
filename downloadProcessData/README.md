# Info 
### Pseudocódigo  Descarga y Procesamiento Concurrente

    Se intentan descargas con hasta 3 intentos por archivo.
    Cada 10 minutos, se lanza un job de alineamiento solo para archivos descargados completamente (identificados por el prefijo SAM).
    Se asegura la ejecución en paralelo de descargas y alineamientos.

# Configuración inicial
1. Definir archivo de entrada con nombres de ejecución (run) y muestras (sample).
2. Establecer directorios para almacenar:
   - Archivos FASTQ temporales.
   - Registros (logs).
   - Resultados del alineamiento.
3. Establecer el número máximo de reintentos para la descarga.
4. Configurar el intervalo para verificar descargas y lanzar alineamientos (e.g., cada 10 minutos).

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
        kallisto quant -i index_file -o output_dir -t NUM_THREADS -b bootstrap_samples input_fastq
        ```
      - Registrar la ejecución en el log.

# Monitoreo
   Implementar scripts que:
      - Detecten archivos incompletos y los excluyan.
      - Reintenten descargas fallidas (hasta el máximo configurado).
      - Supervisen los logs para detectar fallas en tiempo real.

# Finalización
   Una vez que todas las descargas y alineamientos estén completos:
      - Limpiar directorios temporales.
      - Generar un resumen de errores, tiempos y resultados.


# code_scheduler

### Scripts con scheduler (one repeats and loops checking jobs , other mapping scripts send jobs when needed automaticaly):

kallisto_mapping_sortOlder.sh

repeat_loop_kallisto_mappingOlder_v2.sh




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




