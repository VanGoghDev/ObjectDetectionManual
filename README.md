# ObjectDetectionManual
Порядок работы с проектом для обучения модели

## Этап 1
1. Собрать изображения (в данном случае минимум 50 на один объект)
Для этой задачи я использовал различные интернет ресурсы, где можно по названию или тегам скачать изображение. Желательно собрать данные, которые максимально близки к тем, для которых будем делать прогноз
![Repo List](readmeimages/многокартиноксклубникой.png)

2. Аннотации. 
  То есть, необходимо нарисовать рамки на изображениях. Я использовал инструмент labelImg. Пожалуй, это самый долгий процесс.
  ![Repo List](readmeimages/labelimg.png)

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
Once the server is up and running, open your browser, and go to http://localhost:8000/, and you'll be greeted by a prompt window requesting permission to access the webcam. Upon accepting said request, wait a bit until the model is downloaded and voila, rejoice with the glory of out-of-the-box deep learning. Have fun!

### Шаг 3
Необходимо установить Environment Variable, для того, чтобы в скриптах Python была вся необходимая информация о вашем аккаунте для того, чтобы найти именно вашу модель, которую собираемся обучать
Replace the model file url in `detect.js` with your model.json url
`let MODEL_URL = 'REPLACE_ME'`

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
![Repo List](readmeimages/Графикточности.jpg)


На следующем рисунке видно, насколько плохо алгоритм распознает объект при малом количестве данных для обучения.
![Repo List](readmeimages/manyErrors.png)

Результат после полной выборки
![Repo List](readmeimages/afterLearning.png)

Однако, когда сеть обучится по всей выборке, результат становится гораздо лучше. Ниже на картинке видно, что "сетка" корректно определила объект. 

Теперь, после команды 
```
python ./code/prediction.py PATH_TO_YOUR_IMAGE.jpg
```
Можно видеть результат работы алгоритма. Это идеальный случай
![Repo List](readmeimages/Идеальныйслучай.png)

А на следующей картинке видно, что распозналось то яблоко, которое видно целиком. Сеть не обучалась на "дольках" и поэтому не имеет представления о том, что это яблоко. А так же не распознает яблоко, которое видно наполовину.
![Repo List](readmeimages/яблокосдольками.png)

Когда много объектов, распознаются не все, но большинство
![Repo List](readmeimages/многояблок.png)
![Repo List](readmeimages/многоклубники.png)
