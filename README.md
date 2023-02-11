# Копирование и Перемещение
Задачей этой работы является детальное изучение разницы между семантикой копирования и перемещения с подробной визуализацией.
# Специальный флаг
Оптимизации компилятора позволяют не создавать временный объект, который используется только для инициализации объекта такого же типа. 
Флаг -fno-elide-constructors отключает эту оптимизацию и позволит более детально рассмотреть вызовы конструкторов копирования (перемещения) во всех случаях.

<p>
    <img src="images/graph_no_flag.png" alt="Фотография 1" width="400" height="800">
    <img src="images/graph_flag.png" alt="Фотография 2"    width="400" height="800">
 </p>
