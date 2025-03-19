## LMs anotación de metadata de Soja y Tomate. (all NCBI entries)

1. Buscar todos los IDs de SRA de fastq files
2. Generar Tabla con todos los IDs de SRA (SRRs IDs) y BioSample ->  Organism.csv
3. Usar esa tabla y correr #sra.py.   El script lee un archivo CSV con metadatos, filtra los registros de RNA-Seq, elimina duplicados basándose en la columna biosample y transforma la columna attributes a un formato JSON para extraer información adicional. Luego, une esta información al DataFrame principal y guarda ambos resultados en archivos CSV para su posterior análisis 
