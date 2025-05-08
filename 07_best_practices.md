# Лучшие практики работы с Tailwind CSS

## 1. Организация кода

Общие требования и рекомендации к организации кода касательно фронтенда в rails приложениях вы можете найти [вот тут](../../standarts/organizing-rails-views.md) и [вот тут](../../guidelines/views/README.md).

## 2. Работа с классами

### Избегайте @apply

Директива `@apply` может показаться удобной, но она противоречит философии Tailwind и создает проблемы:

❌ Не рекомендуется:

```css

.btn {
  @apply px-4 py-2 bg-blue-500 text-white rounded;
}
```

Проблемы с `@apply`:

- Создает дублирование кода
- Усложняет отладку
- Увеличивает размер CSS
- Нарушает принцип атомарности
- Будет убран из tailwind в будущем со слов разработчиков

✅ Рекомендуется:

Создавать отдельные partial или view components или react/vue/svelte components вместо этого и затем использовать их

```erb
<%= render 'shared/components/button', variant: 'primary' do %>
  Кнопка
<% end %>
```

### Группировка классов

Для улучшения читаемости и поддержки:

```erb
<%
  button_classes = [
    # Размеры
    'px-4',
    'py-2',
    
    # Цвета
    'bg-blue-500',
    'text-white',
    
    # Оформление
    'rounded',
    
    # Состояния
    'hover:bg-blue-600',
    'focus:ring-2',
    'focus:ring-blue-500'
  ].join(' ')
%>

<button class="<%= button_classes %>">
  Кнопка
</button>
```

Преимущества группировки:

- Логическая организация классов
- Легкое добавление/удаление стилей
- Улучшенная читаемость
- Простое тестирование

## 3. Создание переиспользуемых компонентов

Как уже было упомянуто выше вместо apply стоит создавать отдельные компоненты. Создание локального UI kit на основе tailwind и rails partials/helper, partials/view components, react components и т д может показаться сложной задачей, но это не так, особенно учитывая ai code gen инструменты. В дальнейшем это позволит избежать проблем с поддержкой.

## 4. Оптимизация производительности

В Tailwind CSS v4 с Vite оптимизация происходит автоматически, то есть из всех возможных утилитарных классов в конечный бандл добавляется лишь те что используются.

TODO: Проверить это

**Пример Конфигурация в Vite**:

   ```javascript
   // vite.config.js
   import { defineConfig } from 'vite'
   import tailwindcss from '@tailwindcss/vite'

   export default defineConfig({
     plugins: [
       tailwindcss({
         // Пути к файлам для анализа
         content: [
           './app/views/**/*.erb',
           './app/helpers/**/*.rb',
           './app/javascript/**/*.js'
         ]
       })
     ]
   })
   ```

## 5. Конфигурация тем

Группируйте переменные по их назначению, по аналогии с группировкой классов.

Используйте отдельные файлы и или различные директивы для тем(`@theme my-custom-theme`) в случае использования нескольких тем по мимом дефолтных(light dark).

В случае если вам нужно переопределить дефолтные значения и для основной темы и для темной группируйте переменные по сначала по теме, а затем уже назначению:

```css

```css
@theme {
  /* Основная тема (светлая) */
  --color-primary: {
    50: #f0f9ff;
    100: #e0f2fe;
    /* ... */
  }

  /*  все остальное что касается основной темы */ 

  /* Темная тема */
  --color-primary-dark: {
    50: #0c4a6e;
    100: #075985;
    /* ... */
  }

  /*  все остальное что касается темной темы */ 
}
```

## Дополнительные материалы

1. [Tailwind CSS Best Practices](https://tailwindcss.com/docs/optimizing-for-production)
2. [Accessibility Guidelines](https://www.w3.org/WAI/standards-guidelines/wcag/)
3. [Component Testing](https://guides.rubyonrails.org/testing.html#system-testing)
4. [Rails View Components](https://viewcomponent.org/)
