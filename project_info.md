## Задача
Для получения оставшихся баллов (максимум 6) предлагается решить задачу декодирования аудио-записей, а именно: классифицировать набор записей длины в 1 секунду для трех слов: "One", "Two" и "Three" с соответствующими метками *1*, *2*, *3*. 

## Данные
Скачать данные можно по ссылке: [google drive folder](https://drive.google.com/drive/folders/1P1VpNNLTbsZC4dC51fUefhAncJedmpBk?usp=sharing)

В обучающей выборке `train.npz` в поле `'x'` лежит матрица размера *KxN*, где *K* - количество записей, *N* - количество временных отсчетов для каждой записи. Частота дискретизации *Fs = 16000 [Hz]*, поскольку длина записей *1 [с]* то *N=Fs*. 

В поле `'y'` файла `train.npz` лежит вектор размера *K* содержащий метку каждой записи. 

Тестовая выборка `'test.npz'` аналогична обучающей, но не содержит поле `'y'`, которое и предлагается восстановить. 

Все выборки сбалансированы, записи стандартизованы (*mean=0, std=1*)

## Оценка

Метрика оценки качества решения - точность классификации ([accuracy](https://scikit-learn.org/stable/modules/model_evaluation.html#accuracy-score)). 

Перевод в шестибалльную оценку за задание осуществляется по формуле:

> E = min(floor(10 * A - 2), 6)

где *A* - лучшая точность классификации тестовой выборки, *floor()* - округление вниз. К примеру, решение содержащие только метку *2* в качестве предсказания для всех записей даст точность равную *0,33...* и соответсвующая оценка составит *1* балл. Максимум в *6* баллов соответствует точности *0.8* и более (такое решение существует).


## Формат решения 

Выгружаемое решение в `.txt` формате должно содержать один столбец меток в целочисленном формате, последняя строчка - пустая. Метки должны соответсвовать порядку записей в `'test.npz'`. Сохранить numpy array `y` в нужном формате можно с помощью команды ```np.savetxt('y_test.txt', y.astype(int), encoding='utf-8', fmt='%i')```. 

Пример содержания файла:
<pre>1<br />3<br />2<br />1<br />...<br />2<br />3<br />3<br /> </pre>

## Проверка решения

Проверка решения осущевствляется Telegram ботом [@nis20dsp_bot](https://t.me/nis20dsp_bot). При в ходе в чат с ботом укажите свою фамилию (бот спросит сам, для исправление напишите /name). Для выгрузки решения пришлите файл с метками в формате ".txt". В ответ вы получите оценку. Максимальное количество попыток - 7.

Код решения присылать не нужно, однако в случае совпадения значений точности классификации у нескольких студентов я могу потребовать прислать код, если и код так же совпадает то оценка работы обнуляется у каждой копии.

## Подсказки

На лекции я приводил одно из возможных решений ([ссылка на решение](https://github.com/nikolaims/nis20dsp/blob/master/lectures/lecture7.ipynb)). В данной тетрадке можно найти код для загрузки данных, проигрывания записей и выгрузке файла нужного формата для сабмита. 

Вы можете улучшать предложенное решение или придумать свое. Полезные стратегии:

1. Поиск признаков и их оптиального количества (в примере всего 1 признак - среднее значение спектра в диапазоне 1800-2200Hz)
2. Предобработка записей (возможно имеет смысл выделять сегменты содержащие звук для более точной оценки спектра)
3. Перебор классификаторов (в примере - обычное дерево решений)

