## Задание
Исходные данные: временной ряд исторических котировок газа с поставкой на хаб ТТФ «на следующий день» (спот) за 01.01.2016-01.10.2018 (файлTTF_2.xlsx).
Описание гибкого контракта: 
Период действия: 01.10.2018-31.03.2019
Покупатель гибкого контракта имеет право:
  1) Ежедневно покупать 1 млн куб. м газа. по цене, определяющейся за день до поставки по формуле как минимум из котировки «на следующий день» (спот) + 2 евро и фиксированной цены, равной 260 евро/тыс. куб. м. 
  2) Продавать полученный от Продавца 1 млн куб. м на хабе с поставкой «на следующий день» по соответствующей (спотовой) цене.
Задание: оценить стоимость такого контракта (такого права)

## Решение

### Анализ и подготовка данных
1) Построим график по исходным данным
2) В данных наблюдается нулевое значение цены актива. Будем считать это значение браком и заменим его на среднее значение относительно двух соседних дней.
3) Определим цены покупки и продажи актива:
  -В качестве базовой возьмем спотовую цену на начало действия контракта;
  -Цену покупки определим как минимум из спотовой цены + 2 евро и фиксированной цены 260 евро на момент начала действия контракта;
  -Цена продажи равна спотовой цене на день начала действия контракта.
4) Так как действие контракта продолжается 6 месяцев, для оценки волатильности мы возьмем такой же промежуток из исторических котировок. Волатильность определим как среднеквадратичное отклонение базового актива за последние 6 месяцев.
5) В качестве безрисковой процентной ставки возьмем значение в 5%. 
6) Создадим контрольный датасет с датами на период действия контракта и рассчитаем для каждой даты оставшееся до экспирации количество дней.

### Решение по формуле Блэка-Шоулза
Контракт, который дает покупателю право в будущем продать актив, называется опционом. Для расчета стоимости опциона на покупку (Call) и дальнейшую продажу (Put) будем использовать формулу Блэка-Шоулза. 
  Формула для расчета стоимости европейского опциона колл выглядит следующим образом:
    C = S * N(d1) - K * e^(-r*T) * N(d2)
      где:
        N(d1) и N(d2) - нормальные распределения, вычисляемые по формулам:
        d1 = (ln(S/K) + (r + σ^2/2)T) / (σ√T)
        d2 = d1 - σ*√T
  Формула для расчета стоимости европейского опциона пут выглядит следующим образом:
    P = K * e^(-r*T) * N(-d2) - S * N(-d1)
      где:
        N(-d1) и N(-d2) - нормальные распределения, вычисляемые по формулам:
        -d1 = -(ln(S/K) + (r + σ^2/2)T) / (σ√T)
        -d2 = -d1 - σ*√T
  S - текущая цена базового актива
  K - цена исполнения опциона
  r - безрисковая процентная ставка
  T - время до истечения опциона (в годах)
  σ - стандартное отклонение базового актива (волатильность)
  
7) Напишем функцию для расчета стоимости опционов Call и Put  по формуле Блэка-Шоулза.
8)  Рассчитаем стоимости опционов на покупку и продажу 1 млн куб. м газа для каждого дня действия контракта.

### Решение через биномиальную модель
9) В качестве примера генерируем тестовое биномиальное дерево. В качестве параметров возьмем базовую цену 260, множитель для повышения 1.1, множитель для понижения 1/1.1, количество генерируемых слоев 5.
10) Напишем функцию для расчета стоимости опционов Call и Put  по биномиальной модели с учетом вида опциона.
11) Рассчитаем стоимости опционов на покупку и продажу 1 млн куб. м газа для каждого дня действия контракта.
12) Можно увидеть, что оба подхода показывают практически одинаковые значения.
13) Для оценки итоговой стоимости контракта я получу разницу между суммой всех Call опционов и суммой всех Put опционов.





