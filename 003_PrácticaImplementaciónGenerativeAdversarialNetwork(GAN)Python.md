# **Práctica: Implementación de una Generative Adversarial Network (GAN) en Python**

## Objetivo:

El objetivo de esta práctica es crear una GAN simple para generar imágenes sintéticas de dígitos manuscritos (usando el conjunto de datos MNIST). Los estudiantes aprenderán los conceptos básicos de GANs, cómo se entrenan y cómo evaluar los resultados generados.

## Requisitos Previos:

- Conocimientos básicos de Python.
- Familiaridad con bibliotecas como `TensorFlow` o `PyTorch`.
- Conocimiento básico de redes neuronales.

### Pasos:

### 1. **Instalación de Bibliotecas Necesarias**

Primero, asegúrate de que las bibliotecas necesarias estén instaladas. Puedes instalar TensorFlow utilizando el siguiente comando:

```bash
!pip install tensorflow
```

### 2. **Importación de Bibliotecas y Carga del Conjunto de Datos**

Comenzaremos importando las bibliotecas necesarias y cargando el conjunto de datos MNIST, que contiene imágenes de dígitos manuscritos.

```python
import tensorflow as tf
from tensorflow.keras import layers
import numpy as np
import matplotlib.pyplot as plt

# Cargar el conjunto de datos MNIST
(train_images, train_labels), (_, _) = tf.keras.datasets.mnist.load_data()

# Normalizar las imágenes a un rango de [0, 1]
train_images = train_images.reshape(train_images.shape[0], 28, 28, 1).astype('float32')
train_images = (train_images - 127.5) / 127.5  # Normalización a [-1, 1]

# Tamaño del lote (batch size)
BUFFER_SIZE = 60000
BATCH_SIZE = 256

# Crear un dataset de TensorFlow
train_dataset = tf.data.Dataset.from_tensor_slices(train_images).shuffle(BUFFER_SIZE).batch(BATCH_SIZE)
```

**Documentación:**

- **tensorflow:** Biblioteca para construir y entrenar modelos de aprendizaje profundo.
- **mnist.load_data():** Carga el conjunto de datos MNIST.
- **reshape y normalización:** Las imágenes se redimensionan y normalizan a un rango de [-1, 1] para facilitar el entrenamiento de la GAN.

### 3. **Definición del Generador**

El generador es una red neuronal que toma un vector aleatorio (ruido) como entrada y produce una imagen sintética como salida.

```python
def make_generator_model():
    model = tf.keras.Sequential()
    model.add(layers.Dense(7*7*256, use_bias=False, input_shape=(100,)))
    model.add(layers.BatchNormalization())
    model.add(layers.LeakyReLU())

    model.add(layers.Reshape((7, 7, 256)))
    model.add(layers.Conv2DTranspose(128, (5, 5), strides=(1, 1), padding='same', use_bias=False))
    model.add(layers.BatchNormalization())
    model.add(layers.LeakyReLU())

    model.add(layers.Conv2DTranspose(64, (5, 5), strides=(2, 2), padding='same', use_bias=False))
    model.add(layers.BatchNormalization())
    model.add(layers.LeakyReLU())

    model.add(layers.Conv2DTranspose(1, (5, 5), strides=(2, 2), padding='same', use_bias=False, activation='tanh'))

    return model

# Crear el modelo generador
generator = make_generator_model()

# Probar el generador con un vector aleatorio
noise = tf.random.normal([1, 100])
generated_image = generator(noise, training=False)

plt.imshow(generated_image[0, :, :, 0], cmap='gray')
plt.show()
```

**Documentación:**

- **make_generator_model():** Define la estructura de la red del generador.
- **layers.Conv2DTranspose:** Capa que realiza una convolución transpuesta (upsampling).
- **LeakyReLU:** Función de activación que permite un pequeño gradiente cuando la entrada es negativa.
- **generated_image:** Imprime una imagen generada a partir de un vector de ruido aleatorio.

### 4. **Definición del Discriminador**

El discriminador es una red neuronal que clasifica imágenes como reales o falsas.

```python
def make_discriminator_model():
    model = tf.keras.Sequential()
    model.add(layers.Conv2D(64, (5, 5), strides=(2, 2), padding='same', input_shape=[28, 28, 1]))
    model.add(layers.LeakyReLU())
    model.add(layers.Dropout(0.3))

    model.add(layers.Conv2D(128, (5, 5), strides=(2, 2), padding='same'))
    model.add(layers.LeakyReLU())
    model.add(layers.Dropout(0.3))

    model.add(layers.Flatten())
    model.add(layers.Dense(1))

    return model

# Crear el modelo discriminador
discriminator = make_discriminator_model()

# Probar el discriminador con una imagen generada
decision = discriminator(generated_image)
print(decision)
```

**Documentación:**

- **make_discriminator_model():** Define la estructura de la red del discriminador.
- **layers.Conv2D:** Capa que realiza una convolución normal, reduciendo la dimensión de la imagen.
- **Dropout:** Capa que ayuda a prevenir el sobreajuste, apagando aleatoriamente neuronas durante el entrenamiento.
- **decision:** Imprime la decisión del discriminador sobre si la imagen generada es real o falsa.

### 5. **Definición de las Funciones de Pérdida y Optimizadores**

El generador y el discriminador se entrenan utilizando diferentes funciones de pérdida y optimizadores.

```python
cross_entropy = tf.keras.losses.BinaryCrossentropy(from_logits=True)

def generator_loss(fake_output):
    return cross_entropy(tf.ones_like(fake_output), fake_output)

def discriminator_loss(real_output, fake_output):
    real_loss = cross_entropy(tf.ones_like(real_output), real_output)
    fake_loss = cross_entropy(tf.zeros_like(fake_output), fake_output)
    total_loss = real_loss + fake_loss
    return total_loss

generator_optimizer = tf.keras.optimizers.Adam(1e-4)
discriminator_optimizer = tf.keras.optimizers.Adam(1e-4)
```

**Documentación:**

- **cross_entropy:** Función de pérdida que mide la diferencia entre dos distribuciones de probabilidad (real y generada).
- **generator_loss():** Calcula la pérdida del generador, comparando las salidas falsas con la etiqueta de "real".
- **discriminator_loss():** Calcula la pérdida del discriminador, comparando salidas reales y falsas.
- **Adam:** Optimizador popular que ajusta los pesos del modelo durante el entrenamiento.

### 6. **Entrenamiento de la GAN**

Entrenar una GAN implica entrenar simultáneamente el generador y el discriminador. Esto se realiza en múltiples iteraciones (épocas).

```python
EPOCHS = 50
noise_dim = 100
num_examples_to_generate = 16

# Seed para generar imágenes coherentes a lo largo del entrenamiento
seed = tf.random.normal([num_examples_to_generate, noise_dim])

@tf.function
def train_step(images):
    noise = tf.random.normal([BATCH_SIZE, noise_dim])

    with tf.GradientTape() as gen_tape, tf.GradientTape() as disc_tape:
        generated_images = generator(noise, training=True)

        real_output = discriminator(images, training=True)
        fake_output = discriminator(generated_images, training=True)

        gen_loss = generator_loss(fake_output)
        disc_loss = discriminator_loss(real_output, fake_output)

    gradients_of_generator = gen_tape.gradient(gen_loss, generator.trainable_variables)
    gradients_of_discriminator = disc_tape.gradient(disc_loss, discriminator.trainable_variables)

    generator_optimizer.apply_gradients(zip(gradients_of_generator, generator.trainable_variables))
    discriminator_optimizer.apply_gradients(zip(gradients_of_discriminator, discriminator.trainable_variables))

def train(dataset, epochs):
    for epoch in range(epochs):
        for image_batch in dataset:
            train_step(image_batch)

        # Generar y mostrar imágenes en cada epoch
        generate_and_save_images(generator, epoch + 1, seed)

        print(f'Epoch {epoch+1} completada')

    # Generar una imagen final después de las epoch
    generate_and_save_images(generator, epochs, seed)

def generate_and_save_images(model, epoch, test_input):
    predictions = model(test_input, training=False)

    fig = plt.figure(figsize=(4, 4))

    for i in range(predictions.shape[0]):
        plt.subplot(4, 4, i + 1)
        plt.imshow(predictions[i, :, :, 0] * 127.5 + 127.5, cmap='gray')
        plt.axis('off')

    plt.savefig(f'image_at_epoch_{epoch:04d}.png')
    plt.show()

# Ejecutar el entrenamiento
train(train_dataset, EPOCHS)
```

**Documentación:**

- **train_step():** Función que realiza un paso de entrenamiento tanto para el generador como para el discriminador.
- **train():** Función que itera sobre todas las épocas, entrenando la GAN y generando imágenes después de cada época.
- **generate_and_save_images():** Función que genera y guarda imágenes para visualizar el progreso del generador.

### 7. **Conclusión y Reflexión**

En esta práctica, hemos implementado una GAN básica utilizando TensorFlow. La GAN es capaz de generar imágenes de dígitos manuscritos después de ser entrenada en el conjunto de datos MNIST. Aunque este es un ejemplo básico, proporciona una base sólida para entender cómo funcionan las GANs y cómo se pueden aplicar en otros contextos, como la generación de imágenes de alta calidad, el arte generativo, y mucho más.

### Reflexión:

- **Limitaciones:** La GAN implementada es básica y solo funciona con imágenes de baja resolución. Para generar imágenes más complejas y de alta calidad, se requeriría una red más profunda y un entrenamiento más largo.