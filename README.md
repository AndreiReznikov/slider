# PooshkaSlider

# Описание

Это jQuery плагин, реализующий функциональность слайдера числовых значений.

При создании слайдера использовалась архитектура MVP, которая содержит три основных модуля - Model, View и Presenter.

- Модель - модуль, который хранит данные и обрабатывает их
- View - содержит элементы дерева DOM и методы их отображения
- Presenter - соединяет Model и View, а также содержит логику панели настройки

Когда пользователь взаимодействует с интерфейсом (представлением), вызываются методы модели, используя Observer. После этого Модель, используя Observer, уведомляет наблюдателей об изменении своего состояния. Наблюдателями в данном случае являются методы представления, которые принимают измененное состояние модели.

Все элементы DOM подписаны на события в классе Presenter. В этом классе также назначаются наблюдатели.

![UML](/uml/uml-pooshka-slider.png "UML")

# Демо 

Для демонстрации возможностей слайдера добавлена демо-страница.

На странице представлено несколько слайдеров с различными начальными настройками. Для изменения настроек "на лету" к каждому слайдеру подключена конфигурационная панель (не является частью плагина).

[Ссылка на демо](https://andreireznikov.github.io/metalamp-slider-demo/)

# Технологии

Проект совместим *jQuery 3.6.0* и *node 18.16.1*

# Использование

Добавьте следующие библиотеки в свой проект:

- jQuery 3.6.0
- pooshkaSlider.min.js

Подключите стили:

- main.css

# Работа с проектом

Создайте локальную копию репозитория:

`git clone https://github.com/AndreiReznikov/metalamp-slider`

Установите необходимые библиотеки для работы с проектом:

`npm install`

# Комманды NPM

- "build:demo": сборка демо
```
npm run build:demo
```

# Инициализация

Сначала вам нужно создать контейнер для слайдера и задать его длину и высоту:

`<div class="slider-container"></div>`

Контейнер может иметь любое имя класса.

Слайдер создается на основе элемента div. Его необходимо поместить в контейнер:

`<div class="slider js-slider"></div>`

Чтобы инициализировать ползунок, вызовите pooshkaSlider для элемента:

`$('.js-slider').pooshkaSlider();`

Вы также можете инициализировать ползунок, добавив класс *pooshka-range-slider* к своему элементу:

`<div class="pooshka-range-slider"></div>`

Доступны следующие опции:

| Опция | Тип | По-умолчания | Описание | Дата-атрибут |
| --- | --- | --- | --- | --- |
| double | boolean | false | один или два ползунка | data-double |
| vertical | boolean | false | вертикальный или горизонтальный вид | data-vertical |
| showTooltip | boolean | true | показать тултипы | data-show-tooltip |
| showLimit | boolean | true | показать минимальное и максимальное значение | data-show-limit |
| showRange | boolean | true | показать прогресс-бар | data-show-range |
| showScale | boolean | false | показать шкалу значений | data-show-scale |
| localeString | boolean | false | использовать для значений localString | data-locale-string |
| min | number | 0 | минимальное значение | data-min |
| max | number | 100 | максимальное значение | data-max |
| from | number | 10 | значение первого ползунка | data-from |
| to | number | 50 | значение второго ползунка | data-to |
| step | number | 1 | значение шага | data-step |
| scaleNumber | number | 5 | количество значений шкалы | data-scale-number |
| onChange | function | - | вызывается при изменении состояния слайдера | data-on-change |

Опции передаются в виде объекта:

`$('.js-slider').pooshkaSlider({
  double: true,
  showTooltip: true,
  step: 5,
});`

Опции также могут быть переданы черех data-атрибуты:

`<div class="slider js-slider" data-min="0" data-max="100" data-vertical="true"></div>`

# Публичные методы

Чтобы использовать Публичные методы, сначала вы должны сохранить экземпляр слайдера в переменной:

`$('.js-slider').pooshkaSlider({
   double: true
 });`

 `const $slider = $('.js-slider').data('pooshkaSlider');`

 `$slider.update();`

 - update - перезаписывает установленные опции

    `$slider.update({
      double: false
    });`

# Callback

Чтобы подписаться на изменения слайдера, используйте опцию onChange:

`$('.js-slider').pooshkaSlider({
   onChange: () => console.log('change')
 });`

 Вы можете использовать следующие аргументы, используя деструктуризацию:

 - event - получить событие внутри функции

 `$('.js-slider').pooshkaSlider({
   onChange: ({ event }) => console.log(event)
 });`

 - options - получить опции внутри функции

 `$('.js-slider').pooshkaSlider({
   onChange: ({ options }) => console.log(options.modelOptions.from)
 });`

 Чтобы отказаться от подписки на изменения слайдера, передайте функцию, которая возвращает значение false, используя метод обновления:

 `$slider.update({
   onChange: () => false
 });`

# Api

Что касается методов api, то сначала вы должны сохранить экземпляр слайдера в переменной:

`$('.js-slider').pooshkaSlider();`

 `const $slider = $('.js-slider').data('pooshkaSlider');`

 Затем используйте объект с методами API:

`$slider.data('api');`

Вы можете получить доступ к jQuery элементам слайдера: $document, $stripe, $runnerFrom, $runnerTo, $limitMin, $limitMax, $scaleContainer

Вы можете использовать следующие методы API:

| Метод | Описание | Пример |
| --- | --- | --- |
| getModelOptions | возвращает текущие значения: double, vertical, showTooltip, showLimit, showRange, show Scale, localeString,  positionParameter, lengthParameter, to, from, step, stepLength, min, max, scalePositionParameter, scaleNumber, scaleElements, numberOfCharactersAfterDot | $slider.data('api').getModelOptions().double |
| updateUserConfig | обновить значения модели | $slider.data('api').updateUserConfig({'vertical': true}) |
| toggleDouble | переключает | $slider.data('api').toggleDouble() |
| toggleTooltip | переключает showTooltip | $slider.data('api').toggleTooltip() |
| toggleRange | переключает showRange | $slider.data('api').toggleRange() |
| toggleScale | переключает showScale | $slider.data('api').toggleScale() |
| toggleVertical | переключает vertical | $slider.data('api').toggleVertical() |
| setFrom | задаёт значение from | $slider.data('api').setFrom(25) |
| setTo | задаёт значение to | $slider.data('api').setTo(50) |
| setMin | задаёт значение min | $slider.data('api').setMin(5) |
| setMax | задаёт значение max | $slider.data('api').setMax(100) |
| setStep | задаёт значение step | $slider.data('api').setStep(5) |




