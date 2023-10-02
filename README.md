
# Генератор случайных чисел

Приложением генерируются следующие распределения:

1) Равномерное распределение
Функция плотности вероятности: ![image](https://github.com/FakeYury/MainRepo/assets/146737815/3e2074b5-933a-41cf-b4f2-51e1e473d9ca)


2) Нормальное распределение
Функция плотности вероятности: ![image](https://github.com/FakeYury/MainRepo/assets/146737815/0af896f4-0837-4da2-aa2d-aaf79e94e064)


3)Логнормальное распределение
Функция плотности вероятности: ![image](https://github.com/FakeYury/MainRepo/assets/146737815/084c62ac-fbef-48b3-81bf-2a3d480da9ff)


4) Распределение Пуассона
Функция вероятности: ![image](https://github.com/FakeYury/MainRepo/assets/146737815/574768b0-711f-4050-84f3-741e482cf656)


5) Экспоненциальное распределение         
Функция плотности вероятности:      ![image](https://github.com/FakeYury/MainRepo/assets/146737815/34ebbffb-a27c-4ad6-989e-eb05bf67759d)



6) Биномиальное распределение
Функция вероятности: ![image](https://github.com/FakeYury/MainRepo/assets/146737815/ceb05719-c1f3-43bb-8d34-f7ac119cc4ba)


## 1) Взаимодействие с пользовательским интерфейсом

При запуске приложения пользователю предлагается выбрать распределение:

![image](https://github.com/FakeYury/MainRepo/assets/146737815/75a2317a-df52-41db-b258-a5a46d25a063)


![image](https://github.com/FakeYury/MainRepo/assets/146737815/3da38244-7bcd-46b4-b277-ba5815652ffe)


После нажатия кнопки «Отправить» пользователю необходимо ввести соответствующие данные параматров выбранного распределения. Например для экспоненциального распределения необходимо ввести интенсивность, количество испытаний и правую границу значений:


![image](https://github.com/FakeYury/MainRepo/assets/146737815/e3ca4685-ed9c-4132-924a-b0748b6d9bf4)





После нажатия кнопки  «Отправить» появится график полученного распределения следующего вида:


![image](https://github.com/FakeYury/MainRepo/assets/146737815/61a1c2d0-dc50-4acc-955f-fafdc3308b32)





Области графика можно увеличивать, уменьшать, при наведении мыши на график можно смотреть точные значения.
В это же время (сразу после нажатия кнопки «Отправить») все данные занесутся в таблицу excel в лист с соответствующим распределением:

## 2) Описание и корректность работы программы

Все параметры сгенерированного распределения совпадают с введёнными пользователем параметрами, необходимыми для генерации распределения. В этом можно убедиться с помощью графика и сгенерированного в таблице распределения. Но как на программном уровне достигается генерация распределения, отвечающая заданным параметрам?
Для этого воспользуемся определением определённого интеграла Римана.
Определение интеграла Римана.
Пусть на отрезке [a, b] определена некоторая функция. f.  Будем считать a < b.
Разбиением (неразмеченным) отрезка  [a, b] назовём конечное множество точек отрезка   [a, b] , в которое входят точки  a и b. Как видно из определения, в разбиение всегда входят хотя бы две точки. Точки разбиения можно расположить по возрастанию.
Точки разбиения, между которыми нет других точек разбиения, называются соседними. Отрезок, концами которого являются соседние точки разбиения, называется частичным отрезком разбиения.
Длина наибольшего из отрезков называется диаметром разбиения. 
Интегралом Римана функции f в пределах от  a до b называется предел интегральных сумм Римана функции f на разбиениях отрезка [a, b]  при диаметре разбиения, стремящемуся к нулю.

Основная идея программы очень похожа на определение определённого интеграла: заданный пользователем диапазон разбивается на маленькие части – частичные отрезки. Для каждого частичного отрезка вычисляется количество чисел сгенерированного распределения, которое будет лежать в нём. Это количество зависит от длины диапазона и количества чисел – параметров, заданных пользователем. Примерная модель программы представлена на рисунке:







Генерация разбиений на примере нормального распределения


![image](https://github.com/FakeYury/MainRepo/assets/146737815/099932b3-0c63-4dd6-ad16-4ce62cc3b6e7)













Для дальнейшего объяснения необходимо погрузиться в детали реализации программы.

Введём обозначения:
quantity – количество чисел,
begin – начало диапазона,
end – конец диапазона,
 Суть построения всех распределений состоит в том что заданный диапазон разбивается на равные части — частичные отрезки, длина которых вычисляется по формуле:
(конец диапазона – начало диапазона + 1)/length,
где length – некоторая переменная, полученная в результате следующих вычислений:
prefix – дробная часть длины диапазона
введём переменную gcd:

gcd = НОД(prefix, 10^(len(prefix)))  				*len - длина
Тогда
length = ((10^(len(prefix)))/gcd)*(end – begin)
Но если length получается очень большое, то получится слишком много разбиений и вычислительной мощности компьютера не хватит для таких вычислений. Поэтому воспользуемся формулой Стёрджесса, чтобы сократить длину настолько, чтобы вычислительной мощности было достаточно и распределение аппроксимировалось к идеальному:
length = length/(1 + 3.322*lg(length))
Для каждого частичного отрезка высчитывается величина height, равная количеству чисел, принадлежащих отрезку, которое войдёт в итоговую выборку. В зависимости от вида распределения высчитываться это число будет по-разному.
Как было сказано выше, изначально числа генерируются с помощью модуля random, программа лишь решает для каждого следующего числа, включать ли его в итоговую выборку или нет. Когда количество чисел в некотором частичном отрезке, включённых в выборку, достигает максимума, то есть будет равна height, то для данного частичного отрезка числа более генерироваться не будут. 
Можно заключить, что сгенерированные распределения отвечают заданным параметрам благодаря способу высчитывания частичных отрезков и высоты столбиков — количества чисел, которое войдёт в частичный отрезок.

3) Виды распределений и особенности их генерирования

1) Равномерное распределение
Для данного распределения пользователю необходимо указать количество чисел и диапазон чисел, в котором будет генерироваться распределение. В равномерном распределении в каждом разбиении должно лежать количество чисел, равное quantity/length. Это достигается с помощью программы. Как только все разбиения полностью заполняются, программа завершается.

2) Нормальное и логнормальное распределения
Для данных распределений пользователю необходимо указать математическое ожидание, среднеквадратическое отклонение, количество чисел и диапазон чисел, в котором будет генерироваться распределение. В каждом разбиении должно лежать количество чисел, равное интегралу от функции плотности вероятности с границами, равными границам разбиения, умноженному на количество чисел в распределении, которое задавалось пользователем.

3) Экспоненциальное распределение
Пользователю необходимо ввести интенсивность (величина обратная математическому ожиданию), количество чисел и диапазон (от 0 до указанного числа). Программа использует метод обратного преобразования. Пусть U – последовательность равномерно распределенных величин на отрезке (0;1), Х – искомая выборка, тогда

Итоговая формула:	![image](https://github.com/FakeYury/MainRepo/assets/146737815/f64c62cc-30ba-4c9d-a8c7-53ba7146311d)


4) Распределение Пуассона
Пользователь вводит математическое ожидание и количество испытаний. Для создания выборки используется свойство:         ![image](https://github.com/FakeYury/MainRepo/assets/146737815/e4cd064b-7efb-41d2-9abf-1fbf520f2b17)                  ,где Е – распределенные экспоненциально с интенсивностью 1 случайные величины.


5) Биномиальное распределение
Пользователь вводит вероятность успеха, число испытаний и необходимое количество чисел. Для создания используются испытания Бернулли.

## 4) Вывод в файл

Все созданные распределения сохраняются в excel файл под названием «Distributions.xlsx». Для каждого вида распределения существует свой лист, каждое распределение добавляется в новый столбец листа.
Для удобства была создана кнопка для очистки файла, при нажатии на которую содержимое файла бесследно удаляется.
Данные удобно импортировать в различные системы автоматизированного проектирования.

## 5) Недостатки приложения

Существует 3 аспекта, замедляющие работу программы:
1) Программа написана на интерпретируемом языке(python);
2) Алгоритм осуществляет проверку на возможность вхождения сгенерированного числа в конечную выборку;
3) Программа однопоточна.
В результате возможна генерация чисел только в определённых пределах и в определённых количествах. Теоретически высчитанно, что если брать диапазон длины 100. то возможна генерация порядка миллиона целых чисел или 15000 дробных чисел.


## 6) Область применения

Область применения программы включает:
1) Исследование параметров личности человека в психологии и психиатрии;
2) Вероятностную модель в финансах; 
3) Расчёт вероятности ковенантов по кредитной линии в экономике;

4) Карты контроля качества;

5) Теория массового обслуживания;

6) Телекоммуникация;

7) Медицинская статистика.

Например, для исследования параметров личности человека обычно необходимо нормальное распределение, а для построения финансовой модели нужно воспользоваться равномерным распределением. Распределение Пуассона используется в картах контроля качества, теории массового обслуживания, телекоммуникации, медицинской статистике.

Если говорить непосредственно о нашем приложении, то данные, сгенерированные в файл excel и графики удобно импортировать для дальнейшей работы в различные системы автоматизированного проектирования(matlab, mathcad и др.).


## 7) Отличия от аналогов

Реализация представленного проекта уникальна своим подходом к генерации чисел. Для того, чтобы параметры итогового распределения совпадали с введёнными пользователем значениями, использовалась концепция определения интеграла Римана, а для того, чтобы 
программа работала быстро  и в то же время не терялась точность итогового распределения, использовалась формула Стёрджесса. Всё проекты, представленные ниже, представляют собой либо аппаратную реализацию с применением теории по схемотехнике, либо логическую, либо программную. Но даже в проектах с программной реализацией не использовались перечисленные выше концепции.


### Преимущества программы над аналогами с программной реализацией:
1) В некоторых аналогах числа генерируются в полуавтоматическом режиме, то есть тратятся ресурсы на высчитывание числа, попадающего в итоговую выборку и выборка получается не случайной, а значит, высок шанс того, что выборки будут повторяться.

2) Многие программные продукты затрагивают аппаратную часть и требуют дополнительной аппаратуры, а наше приложение мобильно и независимо, его можно развёртывать на любом устройстве.

3) Многие продукты реализовывают какое-то конкретное распределение. Наше приложение может генерировать 6 различных видов распределений.

4) Пользовательский интерфейс: пользователю предоставлен пользовательский интерфейс для генерации распределений, также предоставлена возможность убедиться в правильности разбиения с помощью графика и возможность хранения данных в таблице. Преимущество записи данных в excel состоит в том, что в этой утилите реализованы функции, отображающие параметры распределения.

### Запуск программы
Программу можно запустить с помощью исполняемого файла main.exe, который находится в корне репозитория.





