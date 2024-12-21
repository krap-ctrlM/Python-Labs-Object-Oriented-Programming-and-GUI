# Python Labs: Object-Oriented Programming and GUI

### Описание
Данный проект включает реализацию лабораторных работ, посвященных объектно-ориентированному программированию, обработке данных, созданию графического интерфейса и тестированию. Основные темы:
- Работа с матрицами: сложение, сравнение, эквивалентность.
- Генерация ключей и шифрование текста.
- Создание GUI-приложения для рисования.
- Моделирование поведения объектов с использованием наследования.

### Код
```python
# Лабораторная работа 11: Работа с матрицами
class Matrix:
    def __init__(self, values):
        self.values = values

    def __add__(self, other):
        if len(self.values) == len(other.values) and len(self.values[0]) == len(other.values[0]):
            result = []
            for i in range(len(self.values)):
                result.append([self.values[i][j] + other.values[i][j] for j in range(len(self.values[0]))])
            return Matrix(result)
        else:
            raise ValueError("Матрицы разной размерности")

    def __eq__(self, other):
        return self.values == other.values if len(self.values) == len(other.values) else False

    def __str__(self):
        return '\n'.join([' '.join(map(str, row)) for row in self.values])


class MatrixFinder:
    def __init__(self, matrix):
        self.matrix = matrix

    def find_equivalent_matrices(self):
        result = []
        for i in range(len(self.matrix.values)):
            for j in range(len(self.matrix.values[0])):
                new_matrix = [row[:] for row in self.matrix.values]
                new_matrix[i][j] = -new_matrix[i][j]
                result.append(Matrix(new_matrix))
        return result


class DataHandler:
    def __init__(self, input_data):
        self.input_data = input_data

    def parse_matrices(self):
        matrices = []
        values = []
        for line in self.input_data:
            if line.strip() == "":
                if values:
                    matrices.append(Matrix(values))
                    values = []
            else:
                values.append(list(map(int, line.strip().split())))
        if values:
            matrices.append(Matrix(values))
        return matrices

    def format_matrices(self, matrices):
        result = []
        for matrix in matrices:
            result.append(str(matrix))
            result.append("")
        return result


# Лабораторная работа 12: Генерация ключей и шифрование
class LFSR:
    def __init__(self, state):
        self.state = state

    def generate(self):
        while True:
            yield self.state & 1
            self.state = (self.state >> 1) ^ ((-(self.state & 1)) & 0xB400000000000000)


def encrypt_text(text, key_generator):
    return ''.join(chr(ord(char) ^ key_bit) for char, key_bit in zip(text, key_generator))


def decrypt_text(encrypted_text, key_generator):
    return ''.join(chr(ord(char) ^ key_bit) for char, key_bit in zip(encrypted_text, key_generator))


# Лабораторная работа 13: GUI для рисования
from PyQt5.QtWidgets import QApplication, QMainWindow, QGraphicsScene, QGraphicsView
from PyQt5.QtCore import Qt
from PyQt5.QtGui import QPen

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.scene = QGraphicsScene()
        self.view = QGraphicsView(self.scene)
        self.setCentralWidget(self.view)
        self.pen = QPen(Qt.black)

    def mousePressEvent(self, event):
        if event.button() == Qt.LeftButton:
            self.start_pos = event.pos()

    def mouseMoveEvent(self, event):
        if event.buttons() & Qt.LeftButton:
            end_pos = event.pos()
            self.scene.addLine(self.start_pos.x(), self.start_pos.y(), end_pos.x(), end_pos.y(), self.pen)
            self.start_pos = end_pos

    def keyPressEvent(self, event):
        if event.key() == Qt.Key_C:
            self.scene.clear()


# Лабораторная работа 14: Наследование и моделирование поведения
class Duck:
    def swim(self):
        print("Утка плывет")

    def quack(self):
        print("Утка крякает")

    def fly(self):
        print("Утка летит")


class NonQuacking:
    def quack(self):
        print("Не крякает")


class NonSwimming:
    def swim(self):
        print("Не умеет плавать")


class NonFlying:
    def fly(self):
        print("Не умеет летать")


class Penguin(Duck, NonQuacking):
    pass


class Ostrich(Duck, NonSwimming, NonFlying):
    def quack(self):
        print("Странное ворчание")


class MallardDuck(Duck):
    pass


class RedheadDuck(Duck):
    pass


class RubberDuck(Duck, NonFlying):
    def quack(self):
        print("Резиновая утка не крякает")


class DuckSimulator:
    def __init__(self, duck):
        self.duck = duck

    def simulate(self):
        self.duck.quack()
        self.duck.swim()
        self.duck.fly()


# Примеры использования
penguin = Penguin()
ostrich = Ostrich()
mallard_duck = MallardDuck()
redhead_duck = RedheadDuck()
rubber_duck = RubberDuck()

simulators = [
    DuckSimulator(penguin),
    DuckSimulator(ostrich),
    DuckSimulator(mallard_duck),
    DuckSimulator(redhead_duck),
    DuckSimulator(rubber_duck)
]

for simulator in simulators:
    simulator.simulate()
