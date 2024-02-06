# test2

_____________________________________________________________________________
test.py

import pytesseract
from ultralytics import YOLO
import matplotlib.image as mpimg
from PIL import Image

# Подключение обученной модели YOLO
model = YOLO(r'C:\Users\student.LAPI.000\PycharmProjects\pythonProject\last30.pt')

# Загрузка изображения и получение результатов обнаружения
results = model(r'C:\Users\student.LAPI.000\PycharmProjects\pythonProject\20210802_15_01_13_000_ezqXIKE8x4Qo3j3qx14DU4m6m3H3_T_4160_2080.jpg')

# Извлечение координат прямоугольников
x1, y1, x2, y2 = results[0].boxes.xyxy[0].tolist()
image_path = r'C:\Users\student.LAPI.000\PycharmProjects\pythonProject\20210802_15_01_13_000_ezqXIKE8x4Qo3j3qx14DU4m6m3H3_T_4160_2080.jpg'
img = mpimg.imread(image_path)

# Вывод координат
print(x1, y1, x2, y2)

# Обрезание картинки
im = Image.open('20210802_15_01_13_000_ezqXIKE8x4Qo3j3qx14DU4m6m3H3_T_4160_2080.jpg')
im_crop = im.crop((x1, y1, x2, y2))
im_crop.save('result.jpg', quality=95)

# Распознание текста с картинки
img2 = Image.open('result.jpg')
pytesseract.pytesseract.tesseract_cmd = r'C:\Users\student.LAPI.000\AppData\Local\Programs\Tesseract-OCR\tesseract'
custom_config = r'tessedit_char_whitelist=0123456789'
text = pytesseract.image_to_string(img2, config=custom_config)
print(text.strip())
_____________________________________________________________________________
lol.py

from PyQt5.QtWidgets import QApplication, QWidget, QPushButton, QVBoxLayout
app = QApplication([])
window = QWidget()
layout = QVBoxLayout()
layout.addWidget(QPushButton('Top'))
layout.addWidget(QPushButton('Bottom'))
layout.addWidget(QPushButton('Bottom'))
layout.addWidget(QPushButton('Bottom'))
window.setLayout(layout)
window.show()
app.exec_()
_____________________________________________________________________________
lol2.py

import pytesseract
from PIL import Image
from ultralytics import YOLO
import matplotlib.image as mpimg

# Подключение обученной модели YOLO
model = YOLO(r'C:\Users\student.LAPI.000\PycharmProjects\pythonProject\last30.pt')

image = '20210802_15_01_13_000_ezqXIKE8x4Qo3j3qx14DU4m6m3H3_T_4160_2080.jpg'

img = Image.open(image)
# изменяем размер
new_image = img.resize((80, 170))
new_image.save('Image.png')

# Загрузка изображения и получение результатов обнаружения
results = model(r'C:\Users\student.LAPI.000\PycharmProjects\pythonProject\Image.png')

# Извлечение координат прямоугольников
x1, y1, x2, y2 = results[0].boxes.xyxy[0].tolist()

# Вывод координат
print(x1, y1, x2, y2)

# Обрезание картинки
im = Image.open('Image.png')
im_crop = im.crop((x1, y1, x2, y2))
im_crop.save('result.jpg', quality=95)

img2 = Image.open('result.jpg')
pytesseract.pytesseract.tesseract_cmd = r'C:\Users\student.LAPI.000\AppData\Local\Programs\Tesseract-OCR\tesseract'
custom_config = r'tessedit_char_whitelist=0123456789'
text = pytesseract.image_to_string(img2, config=custom_config)
print(text)
