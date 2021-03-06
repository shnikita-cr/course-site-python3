Рекурсия: фракталы
##################

:date: 2017-10-20 09:00
:lecture_link: https://youtu.be/2XFaK3bgT7w

.. default-role:: code
.. contents:: Содержание

Рекурсия
========

Как мы видели раньше функции могут вызывать другие функции — это вполне обыденная ситуация. При этом функция может
вызывать саму себя. Такой тип вызова называется **рекурсивным**. Самый простой пример рекурсивного вызова функции —
вычисление факториала числа:

.. code-block:: pycon

   >>> def fac(n):
   ...     if n == 0:
   ...         return 1
   ...     else:
   ...         return n * fac(n - 1)
   ...
   >>> fac(5)
   120

Конечно, эту программу можно переписать и без рекурсивных вызовов:

.. code-block:: pycon

   >>> def fac(n):
   ...     f = 1
   ...     x = 2
   ...     while x <= n:
   ...         f *= x
   ...         x += 1
   ...
   ...     return f
   ...
   >>> fac(5)
   120

Отличие этих двух программ кроется в подходе к их построению. Первая написана в **декларативном** стиле, то есть для
вычисления факториала используются его *свойства*, а именно `n! = n*(n-1)!` и `0!=1`. Второй же подход использует
**императивный** стиль: мы *явно описываем*, что *представляет из себя* факториал: `n! = 1*2*…*n`. В большинстве случаев
один и тот же алгорит может быть легко записан, как в рекурсивной форме, так и в нерекурсивной, но существует ряд задач,
для которых построение нерекурсивного алгоритма представляется весьма трудозатратным.

Количество вложенных рекурсивных вызовов называется **глубиной** рекурсии. В силу ограниченности вычислительных ресурсов
рекурсия в компьютерных программах не бывает бесконечной — программист должен явно следить за тем, чтобы глубина
рекурсивных вызовов не превышала заранее известного числа. Если программист об этом не позаботился (или же сделал это
некорректно), операционная система (или интерпретатор) аварийно завершит программу по исчерпанию доступых ресурсов.
Чтобы убедиться в этом, попробуйте вычислить `(-5)!` при помощи рассмотренного ранее примера рекурсивного алгоритма
вычисления факториала.

Модуль `sys` обеспечивает доступ к некоторым переменным и функциям, взаимодействующим с интерпретатором `python`
и операционной системой.

Упражнение №1: длина рекурсии
-----------------------------

С помощью функции fac(n) определите текущую установленную глубину рекурсии и сравните ваш результат с возвращаемым
значением функции sys.getrecursionlimit(). Учтите, что sys.getrecursionlimit() возвращает максимальную глубину
стека вызовов, а не максимальную глубину рекурсии для какой-либо функции.

Упражнение №2: числа Фибоначчи
------------------------------

Напишите программу, вычисляющую n-ное число Фибоначчи. Используйте рекурсивные вызовы функций. Пример

+------+-------+
| Ввод | Вывод |
+======+=======+
| 7    | 13    |
+------+-------+


Черепашка
=========

В следующих заданиях снова будет использоваться модуль turtle. Напомним полезные функции из этого модуля:

+-------------+--------------------------------------------+
| Команда     | Значение                                   |
+=============+============================================+
| forward(X)  | Пройти вперёд X пикселей                   |
+-------------+--------------------------------------------+
| backward(X) | Пройти назад X пикселей                    |
+-------------+--------------------------------------------+
| left(X)     | Повернуться налево на X градусов           |
+-------------+--------------------------------------------+
| right(X)    | Повернуться направо на X градусов          |
+-------------+--------------------------------------------+
| penup()     | Не оставлять след при движении             |
+-------------+--------------------------------------------+
| pendown()   | Оставлять след при движении                |
+-------------+--------------------------------------------+
| shape(X)    | Изменить значок черепахи (“arrow”,         |
|             | “turtle”, “circle”, “square”, “triangle”,  |
|             | “classic”)                                 |
+-------------+--------------------------------------------+
|stamp()      | Нарисовать копию черепахи в текущем месте  |
+-------------+--------------------------------------------+
|color()      | Установить цвет                            |
+-------------+--------------------------------------------+
|begin_fill() | Необходимо вызвать перед рисованием фигуры,|
|             | которую надо закрасить                     |
+-------------+--------------------------------------------+
|end_fill()   | Вызвать после окончания рисования фигуры   |
+-------------+--------------------------------------------+
|width()      | Установить толщину линии                   |
+-------------+--------------------------------------------+
|goto(x, y)   | Переместить черепашку в точку (x, y)       |
+-------------+--------------------------------------------+


Фракталы
========

Хорошим примером для иллюстрации рекурсивных алгоритмов являются задачи рисования фракталов_. Фрактальные кривые,
обладающие бесконечным самоподобием, не являются спрямляемыми_: хоть их и можно изобразить на плоскости конечной
площади, эти кривые имют бесконечную длину. Соответственно, программно их невозможно нарисовать полностью: всегда будет
возможность нарисовать кривую детальнее. Поэтому, фрактальные кривые рисуют в некотором приближении, заранее фиксируя
максимально допустимую глубину рекурсии.

.. _фракталов: https://wikipedia.org/ru/%D0%A4%D1%80%D0%B0%D0%BA%D1%82%D0%B0%D0%BB
.. _спрямляемыми: https://wikipedia.org/ru/%D0%94%D0%BB%D0%B8%D0%BD%D0%B0_%D0%BA%D1%80%D0%B8%D0%B2%D0%BE%D0%B9


Пример программы, использующей рекурсивные вызовы функции, чтобы нарисовать ветку:

.. code-block:: python

   import turtle

   def draw(l, n):
       if n == 0:
           turtle.left(180)
           return

       x = l / (n + 1)
       for i in range(n):
           turtle.forward(x)
           turtle.left(45)
           draw(0.5 * x * (n - i - 1), n - i - 1)
           turtle.left(90)
           draw(0.5 * x * (n - i - 1), n - i - 1)
           turtle.right(135)

       turtle.forward(x)
       turtle.left(180)
       turtle.forward(l)

   draw(400, 5)

Результат выполнения программы при разной глубине рекурсии:

.. image:: {filename}/images/lab8/leaf2.gif
   :width: 250 px
.. image:: {filename}/images/lab8/leaf3.gif
   :width: 250 px
.. image:: {filename}/images/lab8/leaf5.gif
   :width: 250 px

Упражнение №3: кривая Коха
--------------------------

Нарисуйте `кривую Коха`_.
Процесс её построения выглядит следующим образом: берём единичный отрезок, разделяем на три равные части и заменяем
средний интервал равносторонним треугольником без этого сегмента.
В результате образуется ломаная, состоящая из четырёх звеньев длины 1/3.
На следующем шаге повторяем операцию для каждого из четырёх получившихся звеньев и т. д…
Предельная кривая и есть кривая Коха.

Пример работы алгоритма при разной глубине рекурсии:

.. _`кривую Коха`: https://wikipedia.org/ru/%D0%9A%D1%80%D0%B8%D0%B2%D0%B0%D1%8F_%D0%9A%D0%BE%D1%85%D0%B0

.. image:: {filename}/images/lab8/koch_curve1.gif
   :width: 350 px
.. image:: {filename}/images/lab8/koch_curve2.gif
   :width: 350 px
.. image:: {filename}/images/lab8/koch_curve3.gif
   :width: 350 px
.. image:: {filename}/images/lab8/koch_curve4.gif
   :width: 350 px

Для ускорения рисования используйте:

.. code-block:: python

   turtle.speed('fastest')


Упражнение №4: снежинка Коха
----------------------------

Три копии кривой Коха, построенные (остриями наружу) на сторонах правильного треугольника,
образуют замкнутую кривую бесконечной длины, называемую `снежинкой Коха`_.
Нарисуйте ee.

Пример работы алгоритма при разной глубине рекурсии:

.. _`снежинкой Коха`: https://wikipedia.org/ru/%D0%9A%D1%80%D0%B8%D0%B2%D0%B0%D1%8F_%D0%9A%D0%BE%D1%85%D0%B0

.. image:: {filename}/images/lab8/koch_snowflake1.gif
   :width: 350 px
.. image:: {filename}/images/lab8/koch_snowflake2.gif
   :width: 350 px
.. image:: {filename}/images/lab8/koch_snowflake3.gif
   :width: 350 px
.. image:: {filename}/images/lab8/koch_snowflake4.gif
   :width: 350 px


Упражнение №5 кривая Минковского
--------------------------------

Нарисуйте `кривую Минковского`_.
Кривая Минковского нулевого порядка - горизонтальный отрезок.
Затем на каждом шаге каждый из отрезков заменяется на ломанную, состоящую из 8 звеньев.

Пример работы алгоритма при разной глубине рекурсии:

.. _`кривую Минковского`: http://wikipedia.org/ru/%D0%9A%D1%80%D0%B8%D0%B2%D0%B0%D1%8F_%D0%9C%D0%B8%D0%BD%D0%BA%D0%BE%D0%B2%D1%81%D0%BA%D0%BE%D0%B3%D0%BE

.. image:: {filename}/images/lab8/minkowski_curve1.gif
   :width: 250 px
.. image:: {filename}/images/lab8/minkowski_curve2.gif
   :width: 250 px
.. image:: {filename}/images/lab8/minkowski_curve3.gif
   :width: 250 px


Упражнение №6: кривая Леви
--------------------------

Нарисуйте `кривую Леви`_.
Она получается, если взять половину квадрата вида /\\, а затем каждую сторону заменить таким же фрагментом и так далее.

Пример работы алгоритма при разной глубине рекурсии:

.. _`кривую Леви`: https://wikipedia.org/ru/%D0%9A%D1%80%D0%B8%D0%B2%D0%B0%D1%8F_%D0%9B%D0%B5%D0%B2%D0%B8

.. image:: {filename}/images/lab8/levi_curve1.gif
   :width: 350 px
.. image:: {filename}/images/lab8/levi_curve2.gif
   :width: 350 px
.. image:: {filename}/images/lab8/levi_curve3.gif
   :width: 350 px
.. image:: {filename}/images/lab8/levi_curve9.gif
   :width: 350 px


Упражнение №7: кривая дракона
-----------------------------

Нарисуйте `кривую дракона`_.
Кривая дракона нулевого порядка - горизонтальный отрезок.
Разделим отрезок пополам и построим на нем прямой угол, получив кривую дракона первого порядка:

.. _`кривую дракона`: https://ru.wikipedia.org/wiki/%D0%9A%D1%80%D0%B8%D0%B2%D0%B0%D1%8F_%D0%B4%D1%80%D0%B0%D0%BA%D0%BE%D0%BD%D0%B0

.. image:: {filename}/images/lab8/dragon_curve1.gif
   :width: 100 px

На сторонах прямого угла снова построим прямые углы. При этом вершина первого угла находится справа от начальной точки A,
а направления, в которых строятся вершины остальных углов, чередуются.

.. image:: {filename}/images/lab8/dragon_curve2.gif
   :width: 100 px

Примеры:

.. image:: {filename}/images/lab8/dragon_curve5.gif
   :width: 350 px
.. image:: {filename}/images/lab8/dragon_curve9.gif
   :width: 350 px

Упражнение №8: Канторово множество
----------------------------------

Нарисуйте `Канторово множество`_.
Канторово множество нулевого порядка - горизонтальный отрезок.
Удалив среднюю треть получим множество первого порядка.
Повторяя данную процедуру получим остальные множества.

.. _`Канторово множество`: https://ru.wikipedia.org/wiki/%D0%9A%D0%B0%D0%BD%D1%82%D0%BE%D1%80%D0%BE%D0%B2%D0%BE_%D0%BC%D0%BD%D0%BE%D0%B6%D0%B5%D1%81%D1%82%D0%B2%D0%BE

.. image:: {filename}/images/lab8/cantor_set4.gif
   :width: 350 px
.. image:: {filename}/images/lab8/cantor_set2.gif
   :width: 350 px
