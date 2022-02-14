# Оглавление

- [Начало работы](#Начало-работы)
- [Драйверы](#Драйверы)
  * [Создание драйвера](#Создание-драйвера)
  * [Добавление команд](#Добавление-команд)
  * [Основные методы драйвера](#Основные-методы-драйвера)
  * [Подписка на событие](#Подписка-на-событие)
- [Устройства](#Устройства)
  * [Создание устройства](#Создание-устройства)
- [Комнаты](#Комнаты)
  * [Создание комнаты](#Создание-комнаты)
  * [Основные методы комнаты](#Основные-методы-комнаты)
  

# Начало работы

Загрузить архив или склонировать 

`git clone https://github.com/maxrgmax/GlobalScripter-Base-Project.git`

# Драйверы

## Создание драйвера

Драйвер наследуется от **DeviceGeneric**:

```python
from lib_driver import DeviceGeneric

class DeviceClass(DeviceGeneric):
    def __init__(self):
        DeviceGeneric.__init__(self)
```
## Добавление команд

В конструкторе добавляются команды:

```python
self._AddRegex('CommandName', re.compile(b'Regex_Response_String'))
```

Команды добавляются в одельный словарь. Если на команду нет подписки - входная строка не анализируется на приход данного ответа.

## Основные методы драйвера

В драйвере описываются три основные метода: 
- _Set(self, **kwargs) - посыл команды устройству
- _Update(self, **kwargs) - считать ответ от устройства
- _Match(self, match) - обработчик полченных ответов

В названии каждого метода необходимо добавить __CommandName__ (_SetCommandName...).

> Метод ___Match__ должен возвращать словарь с параметрами.

## Подписка на событие
Подписка осуществляется командой **Subscribe**
```python

def Name_Of_Handler(params):
    pass

driver1.Subscribe('CommandName', Name_Of_Handler)
```
где params - словарь, вида
```python
{
    'command':'CommandName', 
    'params': Response_From_Match
}
```
[наверх](#Оглавление)
# Устройства

## Создание устройства

Библиотека **lib_driver** содержит классы интерфейсов для подключения через TCP, UDP, Serial, а также метод **CreateDevice** для создания устройства.

> def CreateDevice(driver, interface, **kwargs), где: 
>
> - driver - [драйвер](#Драйверы) устройства
>
> - interface -класс интерфейса, через который происходит подключение
>
>   - Serial_Interface 
>
>   - TCP_Interface 
>
>   - UDP_Interface 
>
> - **kwargs - аргументы, передаваемые в класс интерфейса.

```python
from lib_driver import CreateDevice, TCP_Interface
from driver_mydriver import DemoClass as drv

Driver1 = CreateDevice(drv, TCP_Interface, Hostname='192.168.1.2', IPPort=12345)
```
[наверх](#Оглавление)

# Комнаты

## Создание комнаты

Комната наследуется от **BaseRoom**:

```python
from lib_room import BaseRoom

class CommonRoom(BaseRoom):
    def __init__(self, params=None):
        BaseRoom.__init__(self)
```
## Основные методы комнаты

Начинать необходимо с добавления панели, закончить можно инициализацией комнаты.

- AddPanel(UIHost) - добавляет тач-панель
- AddButtons(Buttons) - добавляет кнопки 
- AddLabels(Labels) - добавляет надписи
- AddDevice('DeviceName', Device) - добавляет устройства
- ShowPopups(Popups) - отображение страниц (страницы через запятую) 
- HideAllPopups() - скрывает все окна
- CreateMESet('MESetName', Buttons) - создает группу кнопок
- Start() - инициализация комнаты, после добавления всех объектов
- Device() - возвращает устройство из словаря 
- Panel - свойство, возвращает UIHost

 Надписи добавляются простым списком(кортежем).

 Кнопки добавляются словарем, где ключ - список(кортеж) имен(номеров) кнопок,  а значение - 
словарь вида 
```python
{
    'func': 'Func',
    'holdTime': 1, # default = None
    'repeatTime':.2,  # default = None
    'action': ['Pressed', 'Released'],  # default = 'Released'
}
```
[наверх](#Оглавление)