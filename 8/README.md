# Reconocimiento de Emociones en Pepper 

Este proyecto tiene como objetivo capturar imágenes de expresiones faciales usando el robot Pepper, entrenar un modelo de clasificación de emociones con Roboflow y utilizar la API `AIEmotionRecognition` para detectar emociones. A continuación se detallan los pasos de la implementación.

---

## 1. Creación del Dataset

Se generó un dataset con 4 emociones (feliz, triste, enojado, sorprendido), con 10 imágenes por emoción usando la cámara del robot Pepper y la herramienta Roboflow para etiquetar.

---

## 2. Captura de Imágenes con Pepper

Código corregido para capturar imágenes con Pepper:

```python
# -*- coding: utf-8 -*-

from naoqi import ALProxy
import os

# Conexión al servicio de captura de fotos
photo_service = ALProxy("ALPhotoCapture", "192.168.0.110", 9559)
photo_service.setResolution(2)  # 2 = VGA (640x480)
photo_service.setPictureFormat("jpg")

# Captura de una foto y guarda en el robot
photo_service.takePicture("/home/nao/recordings/cameras", "image")


# Mostrar la imagen en la tablet
tablet_service = ALProxy("ALTabletService", "198.18.0.1", 9559)
tablet_service.showImage("http://198.18.0.1/apps/image.jpg")

```

Este código debe ser ejecutado dentro de la carpeta `D3/Cámara`.

---

## 3. Implementación de AIEmotionRecognition

Se utilizó el código encontrado en la carpeta `D3/AIEmotionRecognition`. Se interactuó con la API para reconocer emociones desde las imágenes capturadas y se obtuvieron las siguientes conclusiones:

### Conclusiones:
1. La API responde con alta precisión a emociones con expresiones faciales muy marcadas.
2. Las condiciones de luz afectan significativamente la clasificación.
3. Las emociones neutras a menudo se clasifican erróneamente.
4. La latencia de respuesta es baja, adecuada para interacción en tiempo real.
5. La API se puede integrar con facilidad con otros módulos de Pepper.

---

## 4. 📚 Revisión de Tecnologías para Análisis de Sentimientos

Se exploraron 5 tecnologías actuales:

| Tecnología        | Descripción breve                                      |
|------------------|---------------------------------------------------------|
| TextBlob         | Fácil de usar para análisis de sentimientos en inglés. |
| HuggingFace      | Aprendizaje automatico                                 |
| RoBERTa          | Modelo profundo basado en Transformers.                |
| Google Cloud NLP | API poderosa con soporte multilenguaje.                |
| MonkeyLearn      | Plataforma sin código, rápida para prototipos.         |

Cada tecnología incluye ejemplos en el archivo.


## 6. 📄 Documento Académico

Un informe detallado ha sido generado en Overleaf, explicando todos los procesos, herramientas utilizadas y resultados obtenidos.

---
