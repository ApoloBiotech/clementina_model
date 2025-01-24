# clementina_model
Biomarker discovery and Prediction

# Título del Proyecto: Desarrollo de biomarcadores y modelos predictores asociados a distintos estreses en cultivos de interés agronómico utilizando RNAseqs públicos

Procesar y analizar metadatos de muestras de RNAseq públicos en soja y tomate, enfocándose en la cuantificación precisa de la expresión génica para asociar estas muestras con distintos tipos de estrés (abiótico y biótico) y tejidos específicos.

-Identificación de biomarcadores mediante uso de modelos lineales:
Utilizar métodos estadísticos supervisados y lineales, como Sparse Partial Least Squares Discriminant Analysis (sPLS-DA) y LASSO, para identificar genes y patrones de expresión génica asociados a diferentes tipos de estrés. Estos modelos permitirán seleccionar biomarcadores robustos basados en su relevancia estadística y su capacidad explicativa en los datos disponibles.

-Predicción de respuestas al estrés mediante modelos no lineales:
Implementar modelos avanzados de aprendizaje automático, incluyendo redes neuronales y métodos de ensamble como Random Forest, para construir modelos predictivos capaces de clasificar muestras y predecir respuestas a estrés. Estos modelos no lineales aprovecharán relaciones complejas y no evidentes en los datos, mejorando la precisión y aplicabilidad en escenarios agrícolas.




# code_scheduler

### Scripts con scheduler (one repeats and loops checking jobs , other mapping scripts send jobs when needed automaticaly):
## in DownloadProcessData folder:

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


### Script to run modeling (script4) needs adjustments
## In Modeling folder:

model_4.py       (runs random forest and gradient boost code)






