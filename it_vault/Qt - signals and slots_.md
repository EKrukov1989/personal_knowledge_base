___

### Сигнал

Сигнал это нечто, что можно высылать.

Добавить сигнал можно следующим образом:
```cpp
class Widget
{
Q_OBJECT
public:
	void sendSignedl() { emit doIt(); }
// ...
signals:
	void doIt();
};
```
- Метод сигнала должен возвращать `void`
- Выслать сигнал можно с помощью ключевого слова `emit`, но только из объекта.

___
### Слот

Слот это нечто, что может принимать сигналы.

Слоты можно обявлять как `public`, `protected` и `private`.
Если слот объявлен как  `protected` и `private`, то он может соединяться с сигналами сторонних объектов, но не может вызываться как метод со стороны.
Слоты могут быть виртуальными, но они на порядок медленнее невиртуальных.

Ограничения слотов:
- В слотах нельзя использовать параметры по умолчанию, например: `slotMethod(int n =  0)`
- Слоты нельзя определять как `static`.

 Определение слота:
```cpp
class Widget : public QObject
{
Q_OBJECT
public:
	Widget();
public slots:
	void slot()
	{
		# Внутри слота можно получить указатель на вызывающий объект:
		auto* caller_ptr = sender();
	}
};
```

___
### Соединение

Можно соединить сигнал и слот следующим образом:

```cpp
QObject::connect(const QObject* sender,
				const char* signal, // например SIGNAL(method());
				const QObject* receiver,
				const char* slot, // например SLOT(method());
				Qt: :ConnectionType type = Qt::AutoConnection
);
```

Существует несколько типов соединений:
- `Qt::DirectConnection` -> После отправки сигнала сразу же вызвать метод слота
- `Qt::QueuedConnection` -> После отправки сигнала сформировать событие и добавить его в общую очередь
- `Qt::AutoConnection` -> Если слот и сигнал находятся в одном потоке - `DirectConnection`, если в разных - `QueuedConnection`. Этот режим выбран по умолчанию.

Альтернативный метод соединения:
```cpp
QObject::connect(const QObject* sender,
				const QMetaMethod& signal, // например SIGNAL(method());
				const QObject* receiver,
				const QMetaMethod& slot, // например SLOT(method());
				Qt: :ConnectionType type = Qt::AutoConnection
);
```
Пример использования альтернативного метода: 
```cpp
QObject::connect(pSender, &SenderClass::signalMethod,
				 pReceiver, &ReceiverClass::slotMethod
);
```
- В этом случае ошибки в названиях методов и сигналов будут выявлены на этапе компиляции

Можно переслать сигнал:
```cpp
connect(pSender, SIGNAL(signalMethod1() ), this, SIGNAL(signalMethod2()) );
```
- В этом случае после отправки сигнала 1 будет отправлен сигнал 2.

Блокировка сигналов:
- `blockSignals()` - объект не будет отправлять сигналы
- `blockSignals(false)` - разаблокировать сигналы для объекта
- `signalsBlocked()` - узнать заблокированы сигналы или нет

Соединить сигнал с лямбда-функцией:
```cpp
connect(const QObject* sender, const QMetaMethod& signal, lambda_function ) ;
```


Можно соединять сигнал со слотом, которые имеют такой же набор типов аргументов или такой же набор, но короче.


При уничтожении объекта все связанные с ним соединения уничтожаются автоматически.

Можно разорвать соединение:
```cpp
QObject::disconnect(sender, signal, receiver, slot);
```


___
### QSignalMapper

Класс `QSignalMapper` позволяет перенаправлять сигналы, добавляя при этом какой-то дополнительный аргумент.

Класс `QSignalMapper` имеет:
- слот `map()`, к которому должен быть присоединен каждый объект
- сигнал `mapped()`, который нужно присоединить к слотам, на которые нужно пересылать.

Например при сигнале от кнопки, отослать сигнал дальше добавив к нему в качестве аргумента какое-то сообщение.