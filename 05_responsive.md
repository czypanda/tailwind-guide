# Адаптивный дизайн и Mobile-First подход

## Mobile-First подход

Mobile-First - это методология разработки, при которой сначала создается мобильная версия сайта, а затем добавляются стили для больших экранов. Tailwind CSS полностью поддерживает этот подход.

Если вы точно знаете что приложение будет использоваться в подавляющей большинстве в desktop версии, этим можно пренебречь в той или иной мере. В противном случае следует использовать и поддерживать адаптивную верстку.

### Базовые принципы

1. Начинайте с мобильной версии
2. Добавляйте стили для больших экранов через breakpoints
3. Используйте прогрессивное улучшение

## Breakpoints в Tailwind CSS

Tailwind CSS использует следующие breakpoints по умолчанию:

- `sm`: 640px
- `md`: 768px
- `lg`: 1024px
- `xl`: 1280px
- `2xl`: 1536px

### Пример использования

```html
<!-- Базовый стили для всех размеров экранов -->
<div class="p-4">
  <!-- Контент -->
</div>

<!-- md:p-6 для планшетов и выше, в данном случае начиная с md(768px) размера padding будет 6 а не 4, при этом для размеров md(768px) и ниже будет 4 а не 6 -->
<div class="p-4 md:p-6">
  <!-- Контент -->
</div>

<!-- lg:p-8 для desktop и выше -->
<div class="p-4 md:p-6 lg:p-8">
  <!-- Контент -->
</div>
```

## Адаптивная сетка

### 1. Flexbox

```html
<div class="flex flex-col md:flex-row">
  <div class="w-full md:w-1/2">Колонка 1</div>
  <div class="w-full md:w-1/2">Колонка 2</div>
</div>
```

### 2. Grid

```html
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
  <div>Элемент 1</div>
  <div>Элемент 2</div>
  <div>Элемент 3</div>
</div>
```

## Адаптивная типографика

```html
<h1 class="text-2xl md:text-3xl lg:text-4xl">
  Адаптивный заголовок
</h1>
```

## Скрытие элементов

```html
<!-- Скрыть на mobile -->
<div class="hidden md:block">
  Видно только на планшетах и выше
</div>

<!-- Скрыть на desktop -->
<div class="block md:hidden">
  Видно только на мобильных
</div>
```

## Адаптивные изображения

```html
<img 
  src="image.jpg" 
  class="w-full h-auto"
  alt="Описание изображения"
>
```

## Практические примеры

### 1. Адаптивная навигация

```erb
<%# app/views/shared/_navigation.html.erb %>
<nav class="bg-white shadow-lg">
  <div class="max-w-7xl mx-auto px-4">
    <div class="flex justify-between h-16">
      <!-- Логотип -->
      <div class="flex-shrink-0 flex items-center">
        <%= image_tag "logo.png", class: "h-8 w-auto" %>
      </div>
      
      <!-- Мобильное меню -->
      <div class="flex items-center md:hidden">
        <button class="mobile-menu-button">
          <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
          </svg>
        </button>
      </div>
      
      <!-- Десктопное меню -->
      <div class="hidden md:flex md:items-center md:space-x-8">
        <%= link_to "Главная", root_path, class: "text-gray-700 hover:text-gray-900" %>
        <%= link_to "О нас", about_path, class: "text-gray-700 hover:text-gray-900" %>
        <%= link_to "Контакты", contact_path, class: "text-gray-700 hover:text-gray-900" %>
      </div>
    </div>
  </div>
  
  <!-- Мобильное меню (скрытое) -->
  <div class="mobile-menu hidden md:hidden">
    <div class="px-2 pt-2 pb-3 space-y-1">
      <%= link_to "Главная", root_path, class: "block px-3 py-2 text-gray-700 hover:text-gray-900" %>
      <%= link_to "О нас", about_path, class: "block px-3 py-2 text-gray-700 hover:text-gray-900" %>
      <%= link_to "Контакты", contact_path, class: "block px-3 py-2 text-gray-700 hover:text-gray-900" %>
    </div>
  </div>
</nav>
```

### 2. Адаптивная карточка продукта

```erb
<%# app/views/shared/components/_product_card.html.erb %>
<div class="bg-white rounded-lg shadow-md overflow-hidden">
  <div class="flex flex-col md:flex-row">
    <!-- Изображение -->
    <div class="w-full md:w-1/3">
      <%= image_tag product.image_url, class: "w-full h-48 md:h-full object-cover" %>
    </div>
    
    <!-- Контент -->
    <div class="p-4 md:p-6 flex-1">
      <h3 class="text-lg font-semibold mb-2"><%= product.name %></h3>
      <p class="text-gray-600 mb-4"><%= product.description %></p>
      
      <div class="flex flex-col md:flex-row md:items-center md:justify-between">
        <span class="text-xl font-bold text-gray-900 mb-2 md:mb-0">
          <%= number_to_currency(product.price) %>
        </span>
        
        <%= render 'shared/components/button', variant: 'primary' do %>
          В корзину
        <% end %>
      </div>
    </div>
  </div>
</div>
```

## Дополнительные материалы

1. [Tailwind CSS: Responsive Design](https://tailwindcss.com/docs/responsive-design)
2. [Responsive Images](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images)
3. [Responsive web design basics](https://web.dev/articles/responsive-web-design-basics?hl=en)
