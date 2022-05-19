# Pavlov_OC-semestr4
## Лекционные материалы


Оглавление:
+ [Lec2_Lec3](#Lecture23)
   + [Program №1](#Program_№1)
   + [Program №2](#Program_№2)
   + [Program №3](#Program_№3)
+ [Lec4](#Lecture4)
   + [Program №1](#Program_№12)
   + [Program №2](#Program_№22)
   + [Program №3](#Program_№32)

Операционные системы, подходящие для установки и запуска программ:
- GNU/Linux.
- Mac OS.

Коды программ, которые находятся в папках с лекциями, можно запустить с помощью make-файла или использовать компилятор по типу gcc. Используя ручную компиляцию,  нам нужно перейти в папку с программным кодом и запустить компилятор, написав команду -  `gcc program_filename.c -o executable_filename.out`. Однако следует пользоваться Makefile'ом, потому что там  все шаги уже предусмотрены. Для того, чтобы использовать make-файл необходимо находиться в корневой папке репозитория.

Структура Makefile:
- all
  - Lec2_Lec3_1
  - Lec2_Lec3_2
  - Lec2_Lec3_3
  - Lec4_1
  - Lec4_2
  - Lec4_3

## Обзор программ и результов их работы
___
## <a name="Lecture23">Лекции 2-3</a>
#### <a name="Program_№1">Program №1</a>  
Пример программы динамического выделения памяти для массива.

Программа запрашивает длину массива. Затем выделяется память с помощью функции malloc. При выделении памяти возвращается указатель, который будет равен NULL, если память не выделилась. Если массив инициализирован, то выделенная память осовобождается.  

Тест 1 
```
Enter length of array: 1  
All fine 
```
Тест 2 
```
Enter length of array: -10  
Error: can't allocate memory 
```
___ 
#### <a name="Program_№2">Program №2</a>  
Пример программы для чтения данных из файла.  

В начале выделяем память для массива чаров с помощью функции calloc. Затем создаем дескриптор и открываем файл для чтения, если файла не будет, то он будет создан. Выводим значение дескриптора файла. После читаем из файла 10 байт и записываем в переменную `sz` количество байт, которое удалось прочитать. Далее обязательно записываем в конец массива, в который записывали прочитанную информацию, символ конца строки (терминальный ноль). В конце программы закрываем файл.  

Тест 
```
fd = 3  
called read( 3, c, 10). returned that 0 bytes were read.  
closed the fd.  
```
___
#### <a name="Program_№3">Program №3</a>  
Пример программы системного вызова fork().  

В начале вызывается системный вызов fork(). После этого момента программа делится на двое и в результате вызова функции появляется один child. С помощью оператора switch-case мы определяем каким является процесс - error `(case -1:)` или child `(case 0:)`, или parent `(default:)`. Как правило, процесс потомка всегда больше, чем у родителя, а определяется это с помощью функции `getpid()`.

Тест
```
my pid = 22781, returned pid = 22782
my pid = 22782, returned pid = 0
```
___
## <a name="Lecture4">Лекция 4</a>
#### <a name="Program_№12">Program №1</a>  
Сигналы. Пример работы программы.

В начале работы программы срабатывает системный вызов `signal()` и привязывает сигнал SIGUSR1 к одному из обработчиков `handler1`. Далее выполнятеся проверка на то, что мы child и, если мы им оказываемся, то переопределяем обработчик `handler1` на `handler2`. Из child используется системный вызов `kill()` и отправляется сигнал родителю. `handler1` и `handler2` - две обрабатывающие функции, которые принимают на вход наш сигнал с целочисленным типом данных.

Тестирование  
```
counter = 1  
counter = 3  
counter = 5  
```
___ 
#### <a name="Program_№22">Program №2</a>  
Неименованные каналы. Пример работы программы.

Для работы с неименованными каналами используются два дескриптора. Системный вызов `pipe` записывает два дескриптора с обработкой ошибок. Далее используется системный вызов `fork()` для получения дочернего процесса, и обрабатывания ошибки. Если `cpid` равняется 0, то процесс - потомок. Получается что один дескриптор - для чтения, а второй дескриптор - для записи. То есть сообщение передается между процессами с помощью каналов.

Тест 
```
./prog2.out unnamed
unnamed
```
___
#### <a name="Program_№32">Program №3</a>  
Именованные каналы. Пример работы программы.

Создаём дескриптор канала и именованный канал с правами доступа для всех. После запускается цикл прослушивания канала. В момент, когда в этот канал попадает сообщение, то выводится количество символов, включая 0 конца строки, и само сообщение.

Тест
```
mypipe is created  
$ echo Eureka! > mypipe  
mypipe is opened  
Incomming message (8): Eureka! 
  
read error: Success  
```
___
