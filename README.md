# Облачные  и высокопроизводительные вычисления

### Matrix Multiplication
- Лабораторная написана на языке Python 3 с использованием библиотеки `pycuda`
    - CUDA-ядро написано на С++ и обернуто в python-функцию
- Для вычислений на CPU использовалась библиотека `numpy`
    - Не вижу смысла реализовывать `ijk` и ему подобные алгоритмы, потому что они работают заведомо медленнее   
- Каждый элемент выходной матрицы вычислялся (на GPU) отдельной нитью (потоком)
- Для ускорения вычислений на GPU внутри каждого блока матрицы было реализовано копирование элементов из глобальной памяти в разделяемую, что позволило уменьшить число обращений к глобальной памяти в BLOCK_SIZE раз, где BLOCK_SIZE - размер блока GPU 

- Результаты (усредненные по нескольким запускам)

  |Size | CPU time, ms | GPU time, ms | Speedup|
  |:---:|:------------:|:------------:|:------:|
  | 128 |        0.293 |        0.661 |    0.44|
  | 256 |        0.638 |        0.899 |    0.71|
  | 512 |        4.608 |        2.926 |    1.57|
  |1024 |       35.526 |       11.086 |    3.20|
  |2048 |      266.146 |       59.547 |    4.47|
  
- Выводы
    -  При небольших размерах исходных матриц вычисление на GPU происходит медленнее, так помимо самих вычислений происходит передача данных с хоста на девайс, которая и влечет за собой такие результаты
    -  Учитывая полученные результаты (здесь нам интересно ускорение), можно сделать вывод о том, что в рамках задачи перемножения матриц перенос вычислений с CPU на GPU будет целесообразен только для очень больших матриц.
---