# Неизменяемые данные в Dart:
Неизменяемые типы данных — это те, которые нельзя изменить после того, как они были введены. Язык Dart обладает такими. 
После создания строки, числа и логические значения не могут быть изменены. Строковая переменная сама по себе не содержит строковых данных. 
Non-final строковые переменные можно переназначать, что направляет их к новым строковым данным, но однажды созданные строковые данные сами 
по себе не меняют содержание или длину.
```dart
var str = "This is a Flutter Devs.";
str = "This is a Aeologic Technologies.";
```
Данные устанавливаются в памяти, в этот момент ссылка на ее область помещается в переменную str. 
Следующая строка создает совершенно новую строку и передает ссылку на ее область памяти в аналогичную переменную, перезаписывая ссылку на основную строку. 
Первая строка не изменяется; однако, на случай, если в вашем коде на данный момент не будет никакой субстанции,
он будет отмечен как недоступный, и его память в конечном итоге будет освобождена сборщиком мусора Dart.
### Final Variables vs Constants:
**Final** разрешает только одиночное задание. У него должен быть инициализатор, и всякий раз, когда он был установлен со значением, переменная не может быть переназначена.
```dart
final str = "This is a Flutter Devs.";
str = "This is a Aeologic Technologies.";  // error
```
Dart не позволит вам изменить значение **final** переменной str. **Final** переменная может зависеть от выполнения кода во время выполнения для определения ее состояния,
но это должно произойти во время инициализации.</br>
Constant в Dart являются константами времени компиляции. Слово **const** корректирует значения. Такое состояние должно быть определено во время компиляции.</br>
Константы Dart имеют три основных свойства:

* Постоянные значения глубоко, транзитивно неизменны. Если вам нужно создать постоянную коллекцию (список, карту и т. д.), каждый компонент также должен быть постоянным, рекурсивно.
* Постоянные значения должны быть получены из данных, доступных во время компиляции. Например, DateTime.now()не может быть постоянным, потому что он зависит от данных, доступных только во время выполнения, чтобы сделать себя.
* Константы канонизированы. Одиночный элемент создается в памяти для некоторого случайного последовательного значения независимо от того, как часто оценивается устойчивая артикуляция.
Примеры:
```dart
const str = "This is a Flutter Devs.";
const SizedBox(width: 5);   // a constant object
const [1, 2, 3];            // a constant collection
1 + 2; 
```
```dart
List<int> get list => [1, 2, 3];
List<int> get constList => const [1, 2, 3];
var a = list;
var b = list;
var c = constList;
var d = constList;
print(a == b);  // false
print(c == d);  // true
```
Несмотря на то, что a, b, c и d ссылаются на список с неразличимым содержанием, только постоянные варианты анализируются как true. Dart сравнивает адрес памяти списка, 
а не значения компонентов. Каждый вызов геттера constListвозвращает ссылку на устойчивый список, но помните, что Dart всего один раз помещает список в память.
## Создание неизменяемых классов данных:
Создание простого неизменяемого великолепного класса может быть таким же простым, как использование final свойств и добавление const в конструктор.
```dart
class Student {
  final int rollNum;
  final String name;
  const Student(this.rollNum, this.name);
}
```
Класс Student имеет два свойства, каждое из которых объявлено как final , и они обычно инициализируются с помощью конструктора.
Конструктор использует ключевое слово const, чтобы сообщить Dart, что можно создавать экземпляр этого класса как compile-time значение.
```dart
const std1 = Student(1, "Sam");
var std2 = const Student(1, "Sam");
final std3 = const Student(1, "Sam");
```
Для std1 нам не нужно включать ключевое слово const в конструктор, потому что его необходимость сразу же подразумевается с помощью нашего использования его в
переменной, даже если вы можете состоять из него, если хотите.Переменная std2 — это обычная переменная типа Student, однако мы присвоили ей ссылку на 
неизменяемый, непротиворечивый объект.Переменная std3 аналогична std2, за исключением того, что ей ни в коем случае нельзя присваивать новую ссылку. </br>
Пример:
```dart
class Circle{
final double radius;
const Circle(this.radius);
}
```
```dart
//ОДНА ЯЧЕЙКА
final circle1 = const Circle(15.0);
final circle2 = const Circle(15.0);

final circle3 = Circle(15.0);
final circle4 = Circle(15.0);

//ОДНА ЯЧЕЙКА
const circle5 = Circle(15.0);
const circle6 = Circle(15.0);

//ОДНА ЯЧЕЙКА
var circle7 = const Circle(15.0);
var circle8 = const Circle(15.0);

var circle9 = Circle(15.0);
var circle10 = Circle(15.0);

print(circle1==circle2); // true
print(circle3==circle4);
print(circle5==circle6); // true
print(circle7==circle8); // true
print(circle9==circle10);
```
### Использование помощника по метатегам:
Вы можете использовать **@immutable** метатег из метапакета , чтобы получать полезные предупреждения анализатора о классах, которые должны быть неизменяемыми.
```dart
import 'package:meta/meta.dart';
@immutable
class Student {
  int rollNum;            // not final
  final String name;
Student(this.id, this.name);
}
```
Метатег теперь не делает ваш класс неизменным, однако если вы попытаетесь добавить ключевое слово const в свой конструктор одновременно с изменяемыми свойствами, вы получите сообщение об ошибке.
### Сложные объекты в неизменяемых классах:

```dart
class Color{
int red;
int green;
int blue;
Color(this.red,this.green,this.blue);
}

class Circle{
final double radius;
final Color color;
const Circle(this.radius, this.color);
}

void main(){
var cir = Circle(15.0, Color(255,255,255));
cir.color = Color(0,0,0); //'color' can't be used as a setter because it's final.
cir.color.red = 60;
}
```
### Неизменяемые коллекции:
```dart
class Message {
  final int id;
  final String text;
  const Message(this.id, this.text);
}
class MessageThread {
  final List<Message> messages;
  const MessageThread(this.messages);
}
```
При таком расположении информация действительно защищена. Каждое созданное сообщение является неизменным, и нереально заменить список сообщений внутри MessageThread. **Однако добавить элемент в массив можно.**
```dart
final thread = MessageThread([
  Message(1, "Message 1"),
  Message(2, "Message 2"),
]);
thread.messages.first.id = 10;                 // blocked
thread.messages.add(Message(3, "Message 3"));  // This works!
```
### Возвращаем изменяемую и нет копию коллекции
```dart
class MessageThread {
  final List<Message> _messages;
  List<Message> get messages => _messages.toList();
  const MessageThread(this._messages);
}
thread.messages.add(Message(3, "Message 3"));  // new list
```
```dart
class MessageThread {
  final List<Message> _messages;
  List<Message> get messages => List.unmodifiable(_messages);
  const MessageThread(this._messages);
}
thread.messages.add(Message(3, "Message 3"));  // exception!
```
### Функции обновления состояния:
```dart
class Student {
  final int rollNum;
  final String name;
const Student(this.rollNum, this.name);
}
Student updateStudentRollNum(Student oldState, int rollNum) {
  return Student(rollNum, oldState.name);
}
Student updateStudentName(Student oldState, String name) {
  return Student(oldState.id, name);
}
```
```dart
class Student {
  final int rollNum;
  final String name;
const Student(this.rollNum, this.name);
  
  Student updateRollNum(int rollNum) {
    return Student(rollNum, name);
  }
Student updateName(String name) {
    return Student(rollNum, name);
  }
}
```
### Copy Methods:
```dart
class Student {
  final int rollNum;
  final String name;
const Student(this.rollNum, this.name);
Student copyWith({int rollNum, String name}) {
    return Student(
      rollNum ?? this.rollNum, 
      name ?? this.name,
    );
  }
}
```
```dart
final std1 = Student(1, "Jake");
final std2 = std1.copyWith(rollNum: 2);
final std3 = std1.copyWith(name: "Jerry");
final std4 = std1.copyWith(rollNum: 2, name: "Jerry");
```
### Обновление коллекции:
```dart
class NumberList {
  final List<int> _numbers;
  List<int> get numbers => List.unmodifiable(_numbers);
  NumberList(this._numbers);
  
  NumberList addNumber(NumberList oldState, int number) {
    final list = oldState.numbers.toList();
    return NumberList(list..add(number));
  }
  
  NumberList add(int number) {
    return NumberList(_numbers..add(number));
  }
}
```
