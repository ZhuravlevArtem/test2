
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QLabel, QLineEdit, QPushButton
from PyQt5.QtGui import QPixmap
from PIL import Image
from ultralytics import YOLO
import random


# Создание окна
    class ImageWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("САМАЯ ЛУЧШАЯ И ТОЧНАЯ СИСТЕМА")
        self.setGeometry(100, 100, 400, 400)

        self.label = QLabel(self)
        self.label.setGeometry(0, 0, 400, 400)

        self.text_input = QLineEdit(self)
        self.text_input.setGeometry(10, 10, 200, 30)

        self.button = QPushButton("Распознать цифры", self)
        self.button.setGeometry(220, 10, 170, 30)
        self.button.clicked.connect(self.open_image)

    # Открытие фотографии
    def open_image(self):
        global r
        image_path = self.text_input.text()
        img = Image.open(image_path)
        # изменяем размер
        new_image = img.resize((400, 400))
        # сохранение картинки
        new_image.save('result0.jpg')
        pixmap = QPixmap('result0.jpg')
        self.label.setPixmap(pixmap)

        # Подключение обученной модели YOLO
        model = YOLO(r'C:\Users\student.LAPI.000\PycharmProjects\pythonProject\last30.pt')
        image = image_path
        img = Image.open(image)

        # изменяем размер
        new_image = img.resize((500, 1000))
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

        # Подключение обученной модели YOLO
        model2 = YOLO(r'C:\Users\student.LAPI.000\PycharmProjects\pythonProject\best.pt')

        # Загрузка изображения и получение результатов обнаружения
        results2 = model2(r'C:\Users\student.LAPI.000\PycharmProjects\pythonProject\result.jpg')

        # Извлечение координат прямоугольников
        x12, y12, x22, y22 = results2[0].boxes.xyxy[0].tolist()

        # Вывод координат
        print(x12, y12, x22, y22)

        # Просмотр результатов
        for r in results2:
            print(r.boxes.cls)  # print the Probs object containing the detected class probabilities
        random_number = random.randint(0, 2)
        class_value_str = r.boxes.cls[random_number]
        class_value = int(class_value_str)

        # Обрезание картинки
        im = Image.open('result.jpg')
        im_crop = im.crop((x12, y12, x22, y22))
        im_crop.save('result2.jpg', quality=95)
        # Вывод результатов
        lol = 'number:'
        III = '|'
        print('')
        print('')
        print('')
        print('')
        print('')
        print('')
        print('')
        print('')
        print('')
        print('')
        print('')
        print('')
        print('______________.')
        print(lol, random_number, class_value, class_value, III)
        print('______________|')


# Создание экземпляра приложения Qt
if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = ImageWindow()
    window.show()
    sys.exit(app.exec())
