# ObjectDetectionManual
Порядок работы с проектом для обучения модели

## Этап 1
1. Собрать изображения (в данном случае минимум 50 на один объект)
Для этой задачи я использовал различные интернет ресурсы, где можно по названию или тегам скачать изображение. Желательно собрать данные, которые максимально близки к тем, для которых будем делать прогноз
(вставить картинку)

2. Аннотации. 
  То есть, необходимо нарисовать рамки на изображениях. Я использовал инструмент labelImg. Пожалуй, это самый долгий процесс.
  (вставить картинку)

## Этап 2
1. Обучение модели.
Тут возможно несколько вариантов. Обучение на GPU, CPU или на удаленном сервере. В моем случае я выбрал удаленный сервер т.к. у меня старый компьютер и любые вычисления происходят крайне медленно.

## Этап 3 
1. Осуществление прогноза. Теперь можем работать с нашей моделью, распознавать образы, рисовать рамки.

# Создание своей модели
### Шаг 1
1. Клонируйте репозиторий 
```
git clone https://github.com/VanGoghDev/ObjectDetectionManual
```
2. В скачанной папке зайдите в директорию 
```
cd object-detection-sample-python
```
3. Выполните команду 
```
sudo pip install requests
```
### Шаг 2
Для того, чтобы воспользоваться облачной системой, необходимо получить бесплатный API, который позволяет работать с сервисом
http://app.nanonets.com/user/api_key

### Шаг 3
Необходимо установить Environment Variable, для того, чтобы в скриптах Python была вся необходимая информация о вашем аккаунте для того, чтобы найти именно вашу модель, которую собираемся обучать

### Шаг 4
Собственно создаем модель. Т.е. генерируется Id нашей модели.
```
python ./code/create-model.py
```

### Шаг 5 
Устанавливаем ModelID как Environment Variable. Опять же, для того, чтобы скрипту было ее видно

### Шаг 6
Загружаем данные для обучения
Тут важный момент. Чтобы все корректно работало, загружаем в папку images, изображения, по которым будем обучать модель. А после того как разметили классы в labelimg, мы выгружаем данные в формат json или xmls, там хранятся данные о boundbox для каждого конкретного изображения. Эти данные кладем в папку annotations/ а дальше в json или xmls в зависимости от того, какой формат мы выбрали (можно сразу тот и другой)
```
python ./code/upload-training.py
```
### Шаг 7
Обучение модели
Как только изображения загрузили, начинаем обучать модель

### Шаг 8
Пройдет где-то минут 30 и все готово. Можно делать прогноз
```
python ./code/prediction.py PATH_TO_YOUR_IMAGE.jpg
```

# Мои результаты
Я работал над моделью, которая смогла бы отличить яблоко от клубники. Данные для обучения взял из интернета. А так же нашел модель, обученную на огромной объеме данных (очевидно, что эта модель будет распознавать гораздо лучше моей). Однако, целью данного курса является обучение модели на своих данных. А так же пройти весь этот тернистый путь самому. 
Ниже представлен график, на котором видно как расходятся точности моей модели, с моделью, веса  которой уже посчитаны.
(вставить график)


На следующем рисунке видно, насколько плохо алгоритм распознает объект
(вставить картинку где много красных квадратов)
