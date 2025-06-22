from PyQt6 import QtWidgets
from PyQt6.QtWidgets import QApplication, QMainWindow
from kalkulator import Ui_MainWindow
import sys
import math

class Kalkulator(QMainWindow):
    def __init__(self):
        super().__init__()
        self.ui = Ui_MainWindow()
        self.ui.setupUi(self)
        self.expression = ""
        self.connect_buttons()

    def connect_buttons(self):
        for i in range(10):
            getattr(self.ui, f"btn_{i}").clicked.connect(lambda _, x=str(i): self.add_to_expression(x))

        self.ui.btn_dot.clicked.connect(lambda: self.add_to_expression("."))
        self.ui.btn_plus.clicked.connect(lambda: self.add_to_expression("+"))
        self.ui.btn_minus.clicked.connect(lambda: self.add_to_expression("-"))
        self.ui.btn_multiply.clicked.connect(lambda: self.add_to_expression("*"))
        self.ui.btn_divide.clicked.connect(lambda: self.add_to_expression("/"))
        self.ui.btn_percent.clicked.connect(lambda: self.add_to_expression("/100"))

        self.ui.btn_open_paren.clicked.connect(lambda: self.add_to_expression("("))
        self.ui.btn_close_paren.clicked.connect(lambda: self.add_to_expression(")"))

        self.ui.btn_pow.clicked.connect(lambda: self.add_to_expression("**"))
        self.ui.btn_sqrt.clicked.connect(lambda: self.add_to_expression("math.sqrt("))
        self.ui.btn_ln.clicked.connect(lambda: self.add_to_expression("math.log("))
        self.ui.btn_lg.clicked.connect(lambda: self.add_to_expression("math.log10("))
        self.ui.btn_fact.clicked.connect(lambda: self.add_to_expression("math.factorial("))
        self.ui.btn_inverse.clicked.connect(lambda: self.add_to_expression("1/"))
        self.ui.btn_pi.clicked.connect(lambda: self.add_to_expression("math.pi"))
        self.ui.btn_e.clicked.connect(lambda: self.add_to_expression("math.e"))

        self.ui.btn_sin.clicked.connect(lambda: self.add_to_expression("math.sin(math.radians("))
        self.ui.btn_cos.clicked.connect(lambda: self.add_to_expression("math.cos(math.radians("))
        self.ui.btn_tan.clicked.connect(lambda: self.add_to_expression("math.tan(math.radians("))
        self.ui.btn_deg.clicked.connect(lambda: self.add_to_expression("math.degrees("))

        self.ui.btn_clear.clicked.connect(self.clear_expression)
        self.ui.btn_backspace.clicked.connect(self.backspace)
        self.ui.btn_equal.clicked.connect(self.calculate_result)
        self.ui.btn_negate.clicked.connect(self.negate)

    def add_to_expression(self, value):
        self.expression += value
        self.ui.label_display.setText(self.expression)

    def clear_expression(self):
        self.expression = ""
        self.ui.label_display.setText("0")

    def backspace(self):
        self.expression = self.expression[:-1]
        self.ui.label_display.setText(self.expression if self.expression else "0")

    def negate(self):
        if self.expression.startswith("-"):
            self.expression = self.expression[1:]
        else:
            self.expression = "-" + self.expression
        self.ui.label_display.setText(self.expression)

    def calculate_result(self):
        try:
            # Tutup kurung otomatis
            open_count = self.expression.count('(')
            close_count = self.expression.count(')')
            self.expression += ')' * (open_count - close_count)

            result = eval(self.expression, {"math": math, "__builtins__": {}})
            self.expression = str(result)
            self.ui.label_display.setText(self.expression)
        except:
            self.ui.label_display.setText("Error")
            self.expression = ""

if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = Kalkulator()
    window.show()
    sys.exit(app.exec())
