# Локальный CSS

Когда у тега `<style>` есть атрибут `scoped`, то его CSS будет применяться только к элементам текущего компонента. Это похоже на инкапсуляцию стилей в Shadow DOM. Ими можно пользоваться с некоторыми оговорками, но не требуется никаких полифиллов. Это достигается за счёт использования PostCSS для преобразования следующего:

``` html
<style scoped>
.example {
  color: red;
}
</style>

<template>
  <div class="example">hi</div>
</template>
```

В что-то подобное:

``` html
<style>
.example[data-v-f3f3eg9] {
  color: red;
}
</style>

<template>
  <div class="example" data-v-f3f3eg9>hi</div>
</template>
```

#### Примечания

1. Вы можете использовать в компоненте локальные и глобальные стили одновременно:

  ``` html
  <style>
  /* глобальные стили */
  </style>

  <style scoped>
  /* локальные стили */
  </style>
  ```

2. Корневой тег компонента потомка будет попадать под область видимости родительского локального CSS и своего локального CSS.

3. Partials не затрагиваются локальными стилями.

4. **Локальные стили не устраняют необходимость классов**. Из-за того как браузеры рендерят различные CSS-селекторы, `p { color: red }` может быть в разы медленнее при использовании в локальных стилях (например, когда комбинируется с селектором по атрибуту). Если же вы используете классы или ID, такие как `.example { color: red }`, тогда вы практически полностью исключаете ухудшение производительности. [Вот пример](http://stevesouders.com/efws/css-selectors/csscreate.php) где вы можете проверить разницу самостоятельно.

5. **Будьте внимательны с селекторами потомков в рекурсивных компонентах!** Для CSS-правила с селектором `.a .b`, если элемент, который соответствует `.a` содержит рекурсивный компонент потомок, тогда все `.b` в этом компоненте потомке будут также соответствовать правилу.
