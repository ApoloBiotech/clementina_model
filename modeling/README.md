# Ver y estimar uso GPU, tensorflow funciona con CPU, ver lo de GPU

### Optimizar el uso de la GPU en Python y en el archivo SLURM, :

*1. Modificaciones en el archivo SLURM para uso de GPU:*

bash
#!/bin/bash
#SBATCH --job-name=09b1_modelado    # Nombre del trabajo
#SBATCH --output=/home/shared/epilab/BiomarkersML/SoyBean_MLresults/modelado_09b1.out
#SBATCH --error=/home/shared/epilab/BiomarkersML/SoyBean_MLresults/modelado_09b1.err
#SBATCH --time=01:00:00
#SBATCH --partition=short
#SBATCH --mail-type=ALL
#SBATCH --mail-user=notificacionesdescripts@gmail.com

# Solicitar recursos de GPU
#SBATCH --gres=gpu:1               # Solicita 1 GPU
#SBATCH --cpus-per-task=10         # Asigna 10 CPUs por tarea
#SBATCH --mem=64G                  # Asigna 64GB de memoria RAM

# Carga el entorno de Micromamba
source ~/micromamba/etc/profile.d/micromamba.sh
micromamba activate modelado_env
cd /home/shared/epilab/BiomarkersML/SoyBean_MLresults/

# Ejecuta el script de Python
python /home/shared/epilab/BiomarkersML/SoyBean_MLresults/script_modelado_09b1.py


*Explicación:*

- #SBATCH --gres=gpu:1: Solicita una GPU para el trabajo.
- #SBATCH --cpus-per-task=10: Asigna 10 CPUs por tarea para manejar eficientemente la carga de trabajo junto con la GPU.
- #SBATCH --mem=64G: Asigna 64GB de memoria RAM, asegurando suficiente espacio para el procesamiento de datos.

*2. Modificaciones en el script de Python para uso de GPU:*

Asegúrate de que TensorFlow esté configurado para utilizar la GPU. TensorFlow generalmente detecta y utiliza la GPU automáticamente si está disponible. Sin embargo, es recomendable verificar que la GPU esté siendo utilizada correctamente.

Agrega las siguientes líneas al inicio de tu script para confirmar que TensorFlow reconoce la GPU:

python
import tensorflow as tf

# Verificar si TensorFlow está utilizando la GPU
gpus = tf.config.list_physical_devices('GPU')
if gpus:
    print(f"Se encontraron {len(gpus)} GPU(s):")
    for gpu in gpus:
        print(f"  - {gpu}")
else:
    print("No se encontraron GPUs. Se utilizará la CPU.")


Esto imprimirá información sobre las GPUs disponibles y confirmará si TensorFlow las está utilizando.

*3. Alternativa para uso exclusivo de CPU:*

Si deseas ejecutar el script únicamente en la CPU, puedes modificar el archivo SLURM y el script de Python de la siguiente manera:

*Archivo SLURM para CPU:*

bash
#!/bin/bash
#SBATCH --job-name=09b1_modelado_cpu    # Nombre del trabajo
#SBATCH --output=/home/shared/epilab/BiomarkersML/SoyBean_MLresults/modelado_09b1_cpu.out
#SBATCH --error=/home/shared/epilab/BiomarkersML/SoyBean_MLresults/modelado_09b1_cpu.err
#SBATCH --time=01:00:00
#SBATCH --partition=short
#SBATCH --mail-type=ALL
#SBATCH --mail-user=notificacionesdescripts@gmail.com

# Solicitar recursos de CPU
#SBATCH --cpus-per-task=10         # Asigna 10 CPUs por tarea
#SBATCH --mem=64G                  # Asigna 64GB de memoria RAM

# Carga el entorno de Micromamba
source ~/micromamba/etc/profile.d/micromamba.sh
micromamba activate modelado_env
cd /home/shared/epilab/BiomarkersML/SoyBean_MLresults/

# Ejecuta el script de Python
python /home/shared/epilab/BiomarkersML/SoyBean_MLresults/script_modelado_09b1_cpu.py


*Script de Python para CPU:*

Crea una copia de tu script original y nómbrala script_modelado_09b1_cpu.py. En este nuevo script, fuerza a TensorFlow a utilizar solo la CPU añadiendo las siguientes líneas al inicio:

python
import os
os.environ["CUDA_VISIBLE_DEVICES"] = "-1"

import tensorflow as tf

# Verificar que TensorFlow está utilizando la CPU
if not tf.config.list_physical_devices('GPU'):
    print("TensorFlow está configurado para usar únicamente la CPU.")
else:
    print("Advertencia: TensorFlow está utilizando la GPU.")


La línea os.environ["CUDA_VISIBLE_DEVICES"] = "-1" deshabilita la visibilidad de las GPUs para TensorFlow, forzándolo a utilizar solo la CPU.

*Consideraciones adicionales:*

- *Compatibilidad de versiones:* Asegúrate de que las versiones de TensorFlow y Keras Tuner que estás utilizando sean compatibles con el hardware y software del clúster.
- *Dependencias:* Verifica que todas las dependencias necesarias estén instaladas en el entorno modelado_env.
- *Pruebas locales:* Antes de enviar los trabajos al clúster, prueba los scripts en un entorno local para asegurarte de que funcionan correctamente.

Estas modificaciones te permitirán ejecutar tu script de manera eficiente tanto en GPU como en CPU, según tus necesidades




