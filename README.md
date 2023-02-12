# Копирование и Перемещение
Задачей этой работы является детальное изучение разницы между семантикой копирования и перемещения с подробной визуализацией библиотеки graphviz.
Блок схемы построены на основе незамысловатого кода

```
#include "SuperType.hpp"
          
#define start_function() Tracker::getInstance().print_open_func(__func__);
#define end_function()   Tracker::getInstance().print_close_func(__func__);


template<class T>
SuperType<T> func(SuperType<T> a, SuperType<T> b)
{
    start_function()
    SuperType<int> c = a + b;
    end_function()
    return c;
}

int main()
{
    start_function();
    
    SuperType<int> a(5,  "a");
    SuperType<int> b(14, "b");
    SuperType<int> c(20, "c");
    SuperType<int> result(0, "result");
    
    result = c;
    result = func(a, b * c);
    
    end_function();
}
```
    
# Специальный флаг
Оптимизации компилятора позволяют не создавать временный объект, который используется только для инициализации объекта такого же типа. 
Флаг -fno-elide-constructors отключает эту оптимизацию и позволит более детально рассмотреть вызовы конструкторов копирования (перемещения) во всех случаях.

<p>
    <img src="diagrams/graph_no_flag.png" alt="Фотография 1" width="500" height="500">
    <img src="diagrams/graph_flag.png"    alt="Фотография 2" width="500" height="1000">
</p>

# Конструктор и оператор копирования 
Если писать следуя стандартам с++98/03, то мы будем располагать лишь копирующими конструкторами, суть которых заключается в полном копировании классов с их атрибутами. Полное копирование является ресурсоемкой операцией, поэтому в блок схеме она выделена красным, как нежелательные блоки в программе, хотя в некоторых случаях их избежать нельзя.

<p> 
    <img src="diagrams/graph_copy.png"    alt="Фотография 2" width="500" height="1000">
</p>

# Конструктор и оператор перемещения
В c++11/14 появилась семантика перемещения, дарующя возможность оптимизировать код с помощью идеи обмена атрибутами с умирающими объектами, заменяя собой семантику копирования. Перемещающие конструкторы и операторы присваивания менее ресурсоемкие в сравнении с копирующими, поэтому в блок схеме они горят зеленым, как наиболее желательные для вызова. 

<p>    
    <img src="diagrams/graph_move.png"    alt="Фотография 2" width="500" height="1000">
</p>  

Несмотря на лучшую производительность компилятор не заменил всю копирующую семантику на перемещающую, так как она становятся альтернативой, только когда речь заходит об объектах типа rvalue, выступающих в роли объектов для инициализации или присваивания. Если внимательно посмотреть на код, то становится ясно, что в некоторых случаях без копирования обойтись нельзя.
