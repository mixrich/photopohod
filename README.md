# Style guide reference repo

Будем собирать здесь наше видение верстки на примере сайта про фототуры.

Итак, поехали (дописывайте свои идеи и всё такое).

[Как здесь писать?](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

## 1 Верстка "классами" 
 
Идея заключается в том, что для разметки мы используем только классы.
Никаких ID и тегов. 

ID нужны только для программирования. Пример.

```css
/* Хорошо */
.content-article { /* ... */ }

/* Нет-нет-нет! Даже под дулом пистолета :-) */
p {...}
#content {...}
```

### Знаете ли вы... ###
При загрузке веб-страницы css-селекторы выполняются справа налево.

То есть для селектора ```css .header nav ul p {...}``` сначала на странице будут найдены все теги **&lt;p&gt;**, потом из них - выбраны те, что внутри **&lt;ul&gt;**, потом из оставшихся - те, что внутри **&lt;nav&gt;**, ..., ну вы поняли))). 

Вывод. Чем более общий селектор стоит справа, тем дольше он обрабатывается при загрузке страницы. Не то, чтобы это сильно влияло на что-то, просто, сведения для размышления :-)

## 2 Компонентный подход

Посыл: **Количество зависимостей между отдельными модулями страницы стремится к 0.**

При таком подходе блоки можно менять местами, не боясь что верстка "поедет", 
блоки можно использовать повторно и в других проектах.

### 2.1 Каждый блок в своем CSS-файле

Для формы заказа звонка - свой файл, для футера - свой, для блока со ссылкой на фототур - свой.

Чем меньше отдельные блоки - тем, в общем случае, лучше.

Ну и, как обычно, наш девиз - **без фанатизма** :-)

В итоге, файл с версткой, подключаемый на страницу выглядит вот так:

```css

@import header.css;
@import footer.css;
@import tour.css;
@import feedback.css;
/* ...еще 100500 импортов... */
```

Перед загрузкой на **production** при сборке проекта все такие импорты собираются в один файл, который как единое целое и будет встроен в HTML-страницу.

Следствия из п2.1 - в п2.2, 2.3

### 2.2 Верстка без reset.css

Ибо reset.css как устанавливает правила для всего - даже для тех блоков и модулей, где он не нужен.
Если нужно установить свои стили - делаем это внутри css-файла блока.

### 2.3 Верстка без вложенных селекторов

Ну, почти без вложенных. Наш девиз - **без фанатизма** - еще никто не отменял.

```css

/* Хорошо */
.header {...}
.header__title {...}
.header__title__slogan {...}

/* Плохо */
.header .title .slogan{...}
```

"Why?" - спросите Вы. 

1. Да потому, что **title** может быть где-то еще в 150 местах на сайте.  
  И не факт что внутри Header он будет один.  
  Придется переопределять правила... Потом искать где и что поломалось... Нафиг оно нам? 

2. Потому, что если мы поменяем верстку (добавим какие-нибудь обертки или вынесем .slogan из .title), в первом случае
не поломается *ничего*, а во втором - что-нибудь по-любому нужно будет править.


## 3 Пиксели и другие единицы измерения

1. Шрифты лучше делать в **em** или **rem**.
2. Отступы между блоками в px - это нормально.  
   Отступы в тексте лучше делать через **em** или **rem**

## 4 Clearfix

...делается [вот так](https://css-tricks.com/snippets/css/clear-fix/):

```css
.clearfix::after {
  content: "";
  display: table;
  clear: both;
}
```

## 5 " Особенные элементы"
Наша команда, по историческим и рациональным причинам, выделяет 2 "особых категории" классов в верстке.

1. **Container** - этот класс (как правило его имеет тег **DIV**, но фантазия человека на этом не кончается) примечателен тем, что его смысловым назначением является хранение контента. 
На примере все будет куда яснее. 
Пусть на странице имеется блок для описания фототура, в нем фотка, описание, кнопки и так, далее.
Так вот **container** и служит для того, чтобы сгруппировать эти элементы в "единое целое":
```html
<div class="phototour__container">
  <!-- элементы, относящиеся к данному "контейнеру" -->
</div>
```
Конечно, мы могли бы просто сделать так:

```html
<div class="phototour">
  <!-- элементы, относящиеся к данному "контейнеру" -->
</div>
```
Но мы потеряем в семантике, притом в семантике не для браузера конечно, а для людей, которые этот код читают.
Итого, приписка **__container** в конце имени класса дает одно несопоримое преимущество, ежели эту приписку опустить, а именно **восприятие данного тега и его содержимого как некое "единое целое"**
