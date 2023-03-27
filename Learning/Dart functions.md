## 3 типа параметров Dart и их правильное использование
Dart предлагает три различных метода объявления параметров для методов.
* Required positional parameters
* Named parameters
* Optional positional parameters
### Required positional parameters
```dart
class SettingsIcon extends StatelessWidget {
  final VoidCallback callback;
  final String tooltip;

  const SettingsIcon(this.callback, this.tooltip);

...
```
Callback и tooltip являются параметрами конструктора, и они должны быть заданы в порядке объявления.
```dart
SettingsIcon(() {}, "Settings");
```
* Имена параметров важны только для функции локально, вызывающая функция должен знать только их позиции.
* Первая позиция в этом примере предназначена для callback, а вторая - для tooltip.
* Все параметры должны быть указаны при вызове функции, чтобы избежать ошибок компилятора.
### Named parameters
```dart
class SettingsIcon extends StatelessWidget {
  final VoidCallback callback;
  final double? iconSize;
  final Color? iconColor;
  final String? tooltip;

  const SettingsIcon(this.callback,
      {Key? key,
      this.iconSize,
      this.iconColor,
      this.tooltip})
      : super(key: key);

...
```
Мы определяем именованные параметры с помощью фигурных скобок, {}. 
```dart
SettingsIcon(() {},
   iconColor: Colors.amber, 
   tooltip: "Settings");
```
* Именованные параметры могут вызываться в любом порядке, поскольку они не являются позиционными.
* Они необязательны, если только не помечены как обязательные.
* Вызывающий объект может использовать параметр с именем. Таким образом, на него ссылается название.
* Название действительно важно, поскольку оно выставлено на всеобщее обозрение.
### Optional positional parameters
```dart
class SettingsIcon extends StatelessWidget {
  final VoidCallback callback;
  final double? iconSize;
  final Color? iconColor;
  final String? tooltip;

  const SettingsIcon(this.callback,
      [Key? key, this.iconSize, this.iconColor, this.tooltip])
      : super(key: key);

...
```
```dart
SettingsIcon(() {}, GlobalKey() , 16, Colors.amber);
```
* Мы можем пропускать параметры только с конца, а не с начала или середины.
* Мы не можем смешивать необязательные позиционные параметры и именованные параметры. Другими словами, за требуемыми позиционными параметрами могут следовать либо именованные параметры, либо необязательные позиционные параметры.
* Вы не можете поместить ключевое слово required перед необязательными позиционными параметрами, потому что не имеет смысла, чтобы что-то было одновременно обязательным и необязательным.
