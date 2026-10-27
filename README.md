№1
Фабричний метод (Factory Method) — це породжувальний патерн проєктування, який визначає спільний інтерфейс для створення об’єктів, але дозволяє дочірнім класам вирішувати, екземпляр якого класу потрібно створити. Основна мета патерна — відокремити процес створення об’єктів від коду, який їх використовує. Це дозволяє додавати нові типи об’єктів без значних змін наявної логіки програми. Патерн доцільно використовувати, коли програма повинна працювати з різними типами об’єктів, але не повинна залежати від конкретних класів під час їх створення.
Приклад коду

from abc import ABC, abstractmethod

class Notification(ABC):
    @abstractmethod
    def send(self, message):
        pass

class EmailNotification(Notification):
    def send(self, message):
        print(f"Email: {message}")


class SMSNotification(Notification):
    def send(self, message):
        print(f"SMS: {message}")

class NotificationFactory(ABC):
    @abstractmethod
    def create_notification(self):
        pass

    def notify(self, message):
        notification = self.create_notification()
        notification.send(message)

class EmailFactory(NotificationFactory):
    def create_notification(self):
        return EmailNotification()


class SMSFactory(NotificationFactory):
    def create_notification(self):
        return SMSNotification()

email_factory = EmailFactory()
email_factory.notify("Ваше замовлення оформлено!")

sms_factory = SMSFactory()
sms_factory.notify("Ваше замовлення відправлено!")

№2
Абстрактна фабрика (Abstract Factory) — це породжувальний патерн проєктування, який надає інтерфейс для створення сімейств взаємопов’язаних об’єктів без прив’язки до їхніх конкретних класів. Основна мета патерна — забезпечити створення сумісних між собою об’єктів. Наприклад, якщо програма підтримує різні стилі інтерфейсу, абстрактна фабрика може створювати відповідні кнопки та поля введення для Windows або macOS. Патерн корисний, коли програма працює з кількома сімействами продуктів і потрібно гарантувати, що компоненти одного сімейства використовуються разом.
Приклад коду
from abc import ABC, abstractmethod

class Button(ABC):
    @abstractmethod
    def render(self):
        pass


class Checkbox(ABC):
    @abstractmethod
    def render(self):
        pass

class WindowsButton(Button):
    def render(self):
        print("Кнопка Windows")


class WindowsCheckbox(Checkbox):
    def render(self):
        print("Прапорець Windows")

class MacButton(Button):
    def render(self):
        print("Кнопка macOS")


class MacCheckbox(Checkbox):
    def render(self):
        print("Прапорець macOS")

class GUIFactory(ABC):
    @abstractmethod
    def create_button(self):
        pass

    @abstractmethod
    def create_checkbox(self):
        pass

class WindowsFactory(GUIFactory):
    def create_button(self):
        return WindowsButton()

    def create_checkbox(self):
        return WindowsCheckbox()


class MacFactory(GUIFactory):
    def create_button(self):
        return MacButton()

    def create_checkbox(self):
        return MacCheckbox()

def create_interface(factory):
    button = factory.create_button()
    checkbox = factory.create_checkbox()

    button.render()
    checkbox.render()


create_interface(WindowsFactory())
create_interface(MacFactory())

№3
Будівельник (Builder) — це породжувальний патерн проєктування, який дає змогу створювати складні об'єкти поетапно. Він відокремлює процес створення об'єкта від його структури, тому один і той самий процес побудови можна використовувати для створення об'єктів із різними параметрами. Патерн корисний, коли об'єкт має багато властивостей, частина з яких необов'язкова. Наприклад, під час створення комп'ютера можна окремо визначити процесор, обсяг оперативної пам'яті, відеокарту та накопичувач.
Приклад коду
class Computer:
    def __init__(self):
        self.cpu = None
        self.ram = None
        self.gpu = None
        self.storage = None

    def show_info(self):
        print(f"Процесор: {self.cpu}")
        print(f"Оперативна пам'ять: {self.ram} ГБ")
        print(f"Відеокарта: {self.gpu}")
        print(f"Накопичувач: {self.storage} ГБ")


class ComputerBuilder:
    def __init__(self):
        self.computer = Computer()

    def set_cpu(self, cpu):
        self.computer.cpu = cpu
        return self

    def set_ram(self, ram):
        self.computer.ram = ram
        return self

    def set_gpu(self, gpu):
        self.computer.gpu = gpu
        return self

    def set_storage(self, storage):
        self.computer.storage = storage
        return self

    def build(self):
        return self.computer


builder = ComputerBuilder()

computer = (
    builder
    .set_cpu("Intel Core i7")
    .set_ram(32)
    .set_gpu("RTX 4060")
    .set_storage(1000)
    .build()
)

computer.show_info()


№4
Прототип (Prototype) — це породжувальний патерн проєктування, який дозволяє створювати нові об'єкти шляхом копіювання вже наявних об'єктів, замість створення їх з нуля. Цей патерн корисний, коли створення об'єкта є складним або потребує багато ресурсів, а також коли потрібно створювати багато об'єктів із подібними властивостями. Наприклад, у грі можна створити одного ворога з певними характеристиками, а потім копіювати його для створення інших ворогів, змінюючи за потреби їхні параметри.
Приклад коду
import copy

class Enemy:
    def __init__(self, name, health, damage):
        self.name = name
        self.health = health
        self.damage = damage

    def clone(self):
        return copy.deepcopy(self)

    def show_info(self):
        print(
            f"Ворог: {self.name}, "
            f"здоров'я: {self.health}, "
            f"шкода: {self.damage}"
        )

prototype = Enemy("Goblin", 100, 15)

enemy1 = prototype.clone()
enemy2 = prototype.clone()

enemy2.health = 150
enemy2.name = "Strong Goblin"

enemy1.show_info()
enemy2.show_info()


№5
Одинак (Singleton) — це породжувальний патерн проєктування, який гарантує, що клас матиме лише один екземпляр, і надає глобальну точку доступу до нього. Цей патерн використовують, коли в програмі потрібен єдиний об'єкт для керування певним ресурсом або спільними даними. Наприклад, це може бути менеджер налаштувань, журналювання подій або конфігурації програми.
ПРиклад коду
class Logger:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance

    def log(self, message):
        print(f"Повідомлення: {message}")


logger1 = Logger()
logger2 = Logger()

logger1.log("Програма запущена")

print(logger1 is logger2) 


№6
Адаптер (Adapter) — це структурний патерн проєктування, який дозволяє об'єктам із несумісними інтерфейсами працювати разом. Він виступає посередником між двома компонентами, перетворюючи інтерфейс одного компонента на інтерфейс, очікуваний іншим. Наприклад, програма очікує отримати температуру в градусах Цельсія, але стороння бібліотека повертає значення у Фаренгейтах. Адаптер може перетворити дані у потрібний формат, не змінюючи код бібліотеки.
Приклад коду
class FahrenheitSensor:
    def get_temperature_f(self):
        return 98.6


class TemperatureAdapter:
    def __init__(self, sensor):
        self.sensor = sensor

    def get_temperature(self):
        fahrenheit = self.sensor.get_temperature_f()
        celsius = (fahrenheit - 32) * 5 / 9
        return celsius


sensor = FahrenheitSensor()
adapter = TemperatureAdapter(sensor)

print(f"Температура: {adapter.get_temperature():.1f} °C")


№7
Міст (Bridge) — це структурний патерн проєктування, який відокремлює абстракцію від її реалізації, щоб їх можна було змінювати незалежно одна від одної. Цей патерн корисний, коли клас має кілька незалежних вимірів змін. Наприклад, у графічній програмі є різні типи фігур (коло, квадрат) і різні способи їх малювання (звичайний, розширений). Замість створення окремого класу для кожної комбінації можна розділити фігури та способи малювання.
Приклад коду
from abc import ABC, abstractmethod

class Color(ABC):
    @abstractmethod
    def apply_color(self):
        pass


class Red(Color):
    def apply_color(self):
        return "червоний"


class Blue(Color):
    def apply_color(self):
        return "синій"

class Shape(ABC):
    def __init__(self, color):
        self.color = color

    @abstractmethod
    def draw(self):
        pass


class Circle(Shape):
    def draw(self):
        print(f"Малювання кола ({self.color.apply_color()})")


class Square(Shape):
    def draw(self):
        print(f"Малювання квадрата ({self.color.apply_color()})")


circle = Circle(Red())
square = Square(Blue())

circle.draw()
square.draw()


№8
Компонувальник (Composite) — це структурний патерн проєктування, який дозволяє об'єднувати об'єкти в деревоподібні структури та працювати з окремими об'єктами й групами об'єктів через спільний інтерфейс. Цей патерн зручний, коли потрібно працювати з ієрархією елементів. Наприклад, у файловій системі файл є окремим об'єктом, а папка може містити файли та інші папки. Завдяки патерну можна виконувати однакові операції як над окремими файлами, так і над цілими папками.
Приклад коду
from abc import ABC, abstractmethod


class FileSystemItem(ABC):
    @abstractmethod
    def get_size(self):
        pass


class File(FileSystemItem):
    def __init__(self, name, size):
        self.name = name
        self.size = size

    def get_size(self):
        return self.size


class Folder(FileSystemItem):
    def __init__(self, name):
        self.name = name
        self.items = []

    def add(self, item):
        self.items.append(item)

    def get_size(self):
        return sum(item.get_size() for item in self.items)

file1 = File("document.txt", 10)
file2 = File("photo.png", 25)
file3 = File("notes.txt", 5)

documents = Folder("Документи")
documents.add(file1)
documents.add(file3)

root = Folder("Головна папка")
root.add(documents)
root.add(file2)

print(f"Розмір документа: {file1.get_size()} МБ")
print(f"Розмір папки: {documents.get_size()} МБ")
print(f"Загальний розмір: {root.get_size()} МБ")


№9
Декоратор (Decorator) — це структурний патерн проєктування, який дозволяє динамічно додавати об'єкту нові можливості без зміни його початкового класу. Замість створення великої кількості дочірніх класів функціональність розширюється за допомогою спеціальних об'єктів-обгорток — декораторів. Наприклад, у кав'ярні можна замовити звичайну каву, а потім додати до неї молоко, сироп або вершки. Кожен додатковий інгредієнт розширює замовлення та збільшує його вартість.
Приклад коду
from abc import ABC, abstractmethod


class Coffee(ABC):
    @abstractmethod
    def get_cost(self):
        pass

    @abstractmethod
    def get_description(self):
        pass


class SimpleCoffee(Coffee):
    def get_cost(self):
        return 50

    def get_description(self):
        return "Звичайна кава"


class CoffeeDecorator(Coffee):
    def __init__(self, coffee):
        self.coffee = coffee


class MilkDecorator(CoffeeDecorator):
    def get_cost(self):
        return self.coffee.get_cost() + 10

    def get_description(self):
        return self.coffee.get_description() + ", молоко"


class SyrupDecorator(CoffeeDecorator):
    def get_cost(self):
        return self.coffee.get_cost() + 15

    def get_description(self):
        return self.coffee.get_description() + ", сироп"


coffee = SimpleCoffee()
coffee = MilkDecorator(coffee)
coffee = SyrupDecorator(coffee)

print(f"Замовлення: {coffee.get_description()}")
print(f"Вартість: {coffee.get_cost()} грн")



№10
Фасад (Facade) — це структурний патерн проєктування, який надає спрощений інтерфейс для роботи зі складною підсистемою, приховуючи від користувача деталі її внутрішньої реалізації. Наприклад, для запуску комп'ютера потрібно активувати живлення, ініціалізувати процесор і завантажити операційну систему. Замість окремого виклику кожної операції можна створити один метод start(), який виконає всі необхідні дії.
Приклад коду
class CPU:
    def start(self):
        print("Процесор запущено")


class Memory:
    def load(self):
        print("Пам'ять ініціалізовано")


class HardDrive:
    def read_data(self):
        print("Операційну систему завантажено")


class ComputerFacade:
    def __init__(self):
        self.cpu = CPU()
        self.memory = Memory()
        self.drive = HardDrive()

    def start_computer(self):
        self.cpu.start()
        self.memory.load()
        self.drive.read_data()
        print("Комп'ютер готовий до роботи")


computer = ComputerFacade()
computer.start_computer()


№11
Легковаговик (Flyweight) — це структурний патерн проєктування, який дозволяє зменшити використання пам'яті завдяки повторному використанню спільних даних між багатьма об'єктами. Замість того щоб зберігати однакові дані в кожному об'єкті окремо, патерн виносить їх у спільні об'єкти, які можуть використовуватися багаторазово. Наприклад, у комп'ютерній грі на карті можуть бути тисячі дерев одного типу. Замість зберігання окремої копії моделі та текстури для кожного дерева можна спільно використовувати ці ресурси, зберігаючи позицію кожного дерева окремо.
Приклад коду
class TreeType:
    def __init__(self, color, texture):
        self.color = color
        self.texture = texture


class Tree:
    def __init__(self, x, y, tree_type):
        self.x = x
        self.y = y
        self.tree_type = tree_type

    def display(self):
        print(
            f"Дерево: позиція ({self.x}, {self.y}), "
            f"колір {self.tree_type.color}, "
            f"текстура {self.tree_type.texture}"
        )


class TreeFactory:
    _tree_types = {}

    @classmethod
    def get_tree_type(cls, color, texture):
        key = (color, texture)

        if key not in cls._tree_types:
            cls._tree_types[key] = TreeType(color, texture)

        return cls._tree_types[key]

oak_type = TreeFactory.get_tree_type(
    "зелений", "oak_texture"
)

tree1 = Tree(10, 20, oak_type)
tree2 = Tree(30, 40, oak_type)
tree3 = Tree(50, 60, oak_type)

tree1.display()
tree2.display()
tree3.display()

print(tree1.tree_type is tree2.tree_type)


№12
Замісник (Proxy) — це структурний патерн проєктування, який створює об'єкт-посередник для керування доступом до іншого об'єкта. Замісник має інтерфейс, подібний до інтерфейсу справжнього об'єкта, але може додатково перевіряти доступ, відкладати створення об'єкта до моменту використання або кешувати результати операцій. Наприклад, у програмі перегляду зображень великі файли можна завантажувати лише тоді, коли користувач відкриває відповідне зображення. Замісник представляє зображення, але завантажує його в пам'ять тільки за потреби.
Приклад коду
class RealImage:
    def __init__(self, filename):
        self.filename = filename
        self.load_image()

    def load_image(self):
        print(f"Завантаження зображення: {self.filename}")

    def display(self):
        print(f"Відображення: {self.filename}")


class ImageProxy:
    def __init__(self, filename):
        self.filename = filename
        self.real_image = None

    def display(self):
        if self.real_image is None:
            self.real_image = RealImage(self.filename)

        self.real_image.display()


image = ImageProxy("photo.jpg")

print("Об'єкт створено, але зображення ще не завантажено.")

image.display()
image.display()


№13
Ланцюжок обов'язків (Chain of Responsibility) — це поведінковий патерн проєктування, який дозволяє передавати запит послідовно через ланцюжок обробників. Кожен обробник вирішує, чи може він опрацювати запит, чи потрібно передати його наступному обробнику. Цей патерн корисний, коли запит можуть обробляти різні компоненти, але заздалегідь невідомо, який саме з них має це зробити. Наприклад, у системі підтримки клієнтів прості проблеми вирішує оператор першої лінії, складніші — спеціаліст, а найскладніші передаються керівнику.
Приклад коду
from abc import ABC, abstractmethod


class Handler(ABC):
    def __init__(self, next_handler=None):
        self.next_handler = next_handler

    @abstractmethod
    def handle(self, level):
        pass


class Operator(Handler):
    def handle(self, level):
        if level == 1:
            print("Запит оброблено оператором")
        elif self.next_handler:
            self.next_handler.handle(level)
        else:
            print("Немає відповідного обробника")


class Specialist(Handler):
    def handle(self, level):
        if level == 2:
            print("Запит оброблено спеціалістом")
        elif self.next_handler:
            self.next_handler.handle(level)
        else:
            print("Немає відповідного обробника")


class Manager(Handler):
    def handle(self, level):
        if level == 3:
            print("Запит оброблено керівником")
        elif self.next_handler:
            self.next_handler.handle(level)
        else:
            print("Немає відповідного обробника")

manager = Manager()
specialist = Specialist(manager)
operator = Operator(specialist)

operator.handle(2)
operator.handle(3)
operator.handle(5)

№14
Команда (Command) — це поведінковий патерн проєктування, який перетворює запит на окремий об'єкт. Це дозволяє передавати запити як параметри, зберігати їх, ставити в чергу або скасовувати виконані дії. Наприклад, у текстовому редакторі дії «копіювати», «вставити» та «скасувати» можна представити окремими об'єктами-командами. Завдяки цьому редактору не потрібно знати всі деталі виконання кожної дії.
Приклад коду
from abc import ABC, abstractmethod

class Command(ABC):
    @abstractmethod
    def execute(self):
        pass

class Light:
    def turn_on(self):
        print("Світло увімкнено")

    def turn_off(self):
        print("Світло вимкнено")

class TurnOnCommand(Command):
    def __init__(self, light):
        self.light = light

    def execute(self):
        self.light.turn_on()

class TurnOffCommand(Command):
    def __init__(self, light):
        self.light = light

    def execute(self):
        self.light.turn_off()

class RemoteControl:
    def set_command(self, command):
        self.command = command

    def press_button(self):
        self.command.execute()


light = Light()
remote = RemoteControl()

remote.set_command(TurnOnCommand(light))
remote.press_button()

remote.set_command(TurnOffCommand(light))
remote.press_button()


№15
Ітератор (Iterator) — це поведінковий патерн проєктування, який дозволяє послідовно перебирати елементи колекції, не розкриваючи її внутрішньої структури. Наприклад, якщо програма зберігає список студентів, ітератор дозволяє переглядати кожного студента по черзі, не знаючи, як саме список організований усередині.
ПРиклад коду
class StudentCollection:
    def __init__(self):
        self.students = []

    def add(self, student):
        self.students.append(student)

    def __iter__(self):
        return StudentIterator(self.students)


class StudentIterator:
    def __init__(self, students):
        self.students = students
        self.index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.index >= len(self.students):
            raise StopIteration

        student = self.students[self.index]
        self.index += 1
        return student


collection = StudentCollection()
collection.add("Олена")
collection.add("Андрій")
collection.add("Марія")

for student in collection:
    print(student)

№16
Посередник (Mediator) — це поведінковий патерн проєктування, який забезпечує взаємодію між об'єктами через окремий об'єкт-посередник, замість того щоб об'єкти напряму залежали один від одного. Наприклад, у чаті користувачі надсилають повідомлення не кожному учаснику окремо, а через сервер чату. Сервер виступає посередником і передає повідомлення потрібним користувачам. Основна мета патерна — зменшити залежність між об'єктами та спростити їхню взаємодію.
Приклад коду
class ChatMediator:
    def __init__(self):
        self.users = []

    def add_user(self, user):
        self.users.append(user)

    def send_message(self, message, sender):
        for user in self.users:
            if user != sender:
                user.receive_message(message, sender.name)


class User:
    def __init__(self, name, mediator):
        self.name = name
        self.mediator = mediator

    def send_message(self, message):
        self.mediator.send_message(message, self)

    def receive_message(self, message, sender):
        print(
            f"{self.name} отримав повідомлення від "
            f"{sender}: {message}"
        )


chat = ChatMediator()

user1 = User("Олена", chat)
user2 = User("Андрій", chat)
user3 = User("Марія", chat)

chat.add_user(user1)
chat.add_user(user2)
chat.add_user(user3)

user1.send_message("Привіт усім!")

№17
Знімок (Memento) — це поведінковий патерн проєктування, який дає змогу зберегти поточний стан об'єкта, щоб за потреби відновити його пізніше, не розкриваючи внутрішню структуру цього об'єкта. Наприклад, у текстовому редакторі можна скасувати останню дію (Ctrl + Z). Програма зберігає попередній стан документа, щоб користувач міг повернутися до нього. Основна мета патерна — забезпечити збереження та відновлення стану об'єкта.
Приклад коду
class Memento:
    def __init__(self, text):
        self.text = text

class TextEditor:
    def __init__(self):
        self.text = ""

    def write(self, text):
        self.text += text

    def save(self):
        return Memento(self.text)

    def restore(self, memento):
        self.text = memento.text

    def show(self):
        print(self.text)


editor = TextEditor()

editor.write("Привіт")
snapshot = editor.save()

editor.write(", світ!")
editor.show()

editor.restore(snapshot)
editor.show()


№18
Спостерігач (Observer) — це поведінковий патерн проєктування, який дозволяє одному об'єкту повідомляти інші об'єкти про зміни свого стану. Об'єкти, які хочуть отримувати такі повідомлення, підписуються на оновлення. Наприклад, коли користувач підписується на канал, він отримує сповіщення про нові відео. Канал не повинен знати, як саме кожен підписник обробляє ці сповіщення. Основна мета патерна — організувати автоматичне сповіщення залежних об'єктів про зміни без жорсткого зв'язування між ними.
Приклад коду
class NewsChannel:
    def __init__(self):
        self.subscribers = []
        self.news = ""

    def subscribe(self, subscriber):
        self.subscribers.append(subscriber)

    def unsubscribe(self, subscriber):
        self.subscribers.remove(subscriber)

    def publish(self, news):
        self.news = news
        print("Опубліковано новину:", news)

        for subscriber in self.subscribers:
            subscriber.update(news)


class Subscriber:
    def __init__(self, name):
        self.name = name

    def update(self, news):
        print(f"{self.name} отримав сповіщення: {news}")


channel = NewsChannel()

user1 = Subscriber("Олена")
user2 = Subscriber("Андрій")

channel.subscribe(user1)
channel.subscribe(user2)

channel.publish("Вийшла нова версія програми!")

channel.unsubscribe(user2)

channel.publish("Додано нові функції!")


№19
Стан (State) — це поведінковий патерн проєктування, який дозволяє об'єкту змінювати свою поведінку залежно від поточного внутрішнього стану. Замість великої кількості умов if та elif поведінку об'єкта можна винести в окремі класи станів. Наприклад, двері можуть бути відчиненими або зачиненими. Якщо двері зачинені, їх можна відчинити, а якщо відчинені — зачинити. Доступні дії залежать від поточного стану дверей. Основна мета патерна — спростити керування поведінкою об'єкта, яка залежить від його стану.
Приклад коду
class DoorState:
    def open_door(self, door):
        pass

    def close_door(self, door):
        pass

class ClosedState(DoorState):
    def open_door(self, door):
        print("Двері відчинено")
        door.state = OpenState()

    def close_door(self, door):
        print("Двері вже зачинені")

class OpenState(DoorState):
    def open_door(self, door):
        print("Двері вже відчинені")

    def close_door(self, door):
        print("Двері зачинено")
        door.state = ClosedState()

class Door:
    def __init__(self):
        self.state = ClosedState()

    def open(self):
        self.state.open_door(self)

    def close(self):
        self.state.close_door(self)


door = Door()

door.open()
door.open()
door.close()
door.close()

№20
Стратегія (Strategy) — це поведінковий патерн проєктування, який дозволяє визначити кілька алгоритмів виконання певного завдання, винести кожен із них в окремий клас і змінювати алгоритм незалежно від коду, який його використовує. Наприклад, у програмі для побудови маршрутів можна використовувати різні способи пересування: автомобілем, пішки або громадським транспортом. Замість написання великої кількості умов для кожного способу можна створити окремі стратегії побудови маршруту. Основна мета патерна — зробити алгоритми взаємозамінними та спростити додавання нових способів виконання завдання.
Приклад кодлу
class RouteStrategy:
    def build_route(self):
        raise NotImplementedError

class CarStrategy(RouteStrategy):
    def build_route(self):
        print("Маршрут для автомобіля")

class WalkStrategy(RouteStrategy):
    def build_route(self):
        print("Пішохідний маршрут")

class BusStrategy(RouteStrategy):
    def build_route(self):
        print("Маршрут громадським транспортом")

class Navigator:
    def __init__(self, strategy):
        self.strategy = strategy

    def set_strategy(self, strategy):
        self.strategy = strategy

    def build_route(self):
        self.strategy.build_route()

navigator = Navigator(CarStrategy())
navigator.build_route()

navigator.set_strategy(WalkStrategy())
navigator.build_route()

navigator.set_strategy(BusStrategy())
navigator.build_route()


№21
Шаблонний метод (Template Method) — це поведінковий патерн проєктування, який визначає загальну послідовність виконання алгоритму в базовому класі, залишаючи окремі кроки для реалізації в дочірніх класах. Наприклад, приготування різних напоїв може складатися з однакових етапів: нагріти воду, підготувати напій і налити його в чашку. При цьому спосіб приготування кави та чаю відрізняється. Основна мета патерна — визначити спільну структуру алгоритму та дозволити дочірнім класам змінювати окремі кроки без зміни загального порядку виконання.
Приклад коду
from abc import ABC, abstractmethod


class Beverage(ABC):
    def prepare(self):
        self.boil_water()
        self.brew()
        self.pour_into_cup()

    def boil_water(self):
        print("Нагріваємо воду")

    @abstractmethod
    def brew(self):
        pass

    def pour_into_cup(self):
        print("Наливаємо в чашку")


class Coffee(Beverage):
    def brew(self):
        print("Заварюємо каву")


class Tea(Beverage):
    def brew(self):
        print("Заварюємо чай")


coffee = Coffee()
tea = Tea()

print("Приготування кави:")
coffee.prepare()

print("\nПриготування чаю:")
tea.prepare()


№22
Відвідувач (Visitor) — це поведінковий патерн проєктування, який дозволяє додавати нові операції для об'єктів різних класів, не змінюючи код самих цих класів. Наприклад, у програмі є різні типи товарів: книги та електроніка. Потрібно обчислювати для них податок. Замість того щоб додавати метод обчислення податку до кожного класу товару, можна створити окремий клас-відвідувач, який виконуватиме цю операцію. Основна мета патерна — відокремити операції над об'єктами від самих об'єктів і спростити додавання нових операцій.
Приклад коду
from abc import ABC, abstractmethod

class Visitor(ABC):
    @abstractmethod
    def visit_book(self, book):
        pass

    @abstractmethod
    def visit_electronics(self, electronics):
        pass

class Book:
    def __init__(self, price):
        self.price = price

    def accept(self, visitor):
        visitor.visit_book(self)

class Electronics:
    def __init__(self, price):
        self.price = price

    def accept(self, visitor):
        visitor.visit_electronics(self)

class TaxVisitor(Visitor):
    def visit_book(self, book):
        return book.price * 0.05

    def visit_electronics(self, electronics):
        return electronics.price * 0.20


book = Book(1000)
laptop = Electronics(20000)

tax_visitor = TaxVisitor()

print("Податок на книгу:", tax_visitor.visit_book(book))
print("Податок на електроніку:", tax_visitor.visit_electronics(laptop))