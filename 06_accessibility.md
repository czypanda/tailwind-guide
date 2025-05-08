# Доступность и семантика

Данный раздел относится в меньшей степени к tailwind и является общей ознакомительной рекомендацией.

## Что такое доступность и зачем она нужна?

Доступность (Accessibility, часто сокращается как a11y) - это практика создания веб-приложений, которые могут использоваться людьми с различными ограничениями, включая:

- Визуальные ограничения (слепота, слабое зрение, дальтонизм)
- Моторные ограничения (ограниченная подвижность, тремор)
- Когнитивные ограничения (дислексия, проблемы с памятью)
- Слуховые ограничения (глухота, слабый слух)

### Почему доступность важна?

1. **Юридические требования**:
   - Многие страны имеют законы, требующие доступности веб-сайтов
   - Несоблюдение может привести к судебным искам
   - Особенно важно для государственных и образовательных сайтов

2. **Бизнес-преимущества**:
   - Увеличение аудитории пользователей
   - Улучшение SEO (поисковые системы ценят доступные сайты)
   - Улучшение пользовательского опыта для всех
   - Снижение риска юридических проблем

3. **Технические преимущества**:
   - Более чистая и семантическая разметка
   - Лучшая поддержка различных устройств
   - Упрощенное тестирование и поддержка

### Где применяется доступность?

1. **Веб-приложения**:
   - Корпоративные сайты
   - Электронная коммерция
   - Социальные сети
   - Образовательные платформы

2. **Мобильные приложения**:
   - Нативные приложения
   - Прогрессивные веб-приложения (PWA)
   - Гибридные приложения

3. **Специальные системы**:
   - Банковские системы
   - Медицинские порталы
   - Государственные сервисы

## Основы доступности

### 1. Семантическая разметка

Семантическая разметка - это использование HTML-элементов по их назначению, что помогает скринридерам и другим вспомогательным технологиям правильно интерпретировать контент.

```erb
<!-- ❌ Не рекомендуется -->
<div class="text-xl font-bold">Заголовок</div>

<!-- ✅ Рекомендуется -->
<h1 class="text-xl font-bold">Заголовок</h1>
```

Преимущества семантической разметки:

- Улучшенная навигация для скринридеров
- Лучшее понимание структуры страницы
- Улучшенное SEO
- Более предсказуемое поведение браузера

### 2. ARIA-атрибуты

ARIA (Accessible Rich Internet Applications) - это набор атрибутов, которые помогают сделать веб-контент более доступным для людей с ограниченными возможностями.

```erb
<!-- Кнопка с описанием -->
<button 
  class="btn"
  aria-label="Закрыть модальное окно"
>
  <svg class="w-6 h-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
  </svg>
</button>

<!-- Alert с ролью -->
<div 
  class="alert"
  role="alert"
  aria-live="polite"
>
  Сообщение об успехе
</div>
```

Основные ARIA-атрибуты:

- `aria-label`: Описывает элемент для скринридеров
- `role`: Определяет роль элемента
- `aria-live`: Указывает, как обновления контента должны объявляться
- `aria-expanded`: Показывает состояние раскрытия/сворачивания
- `aria-hidden`: Скрывает элемент от скринридеров

## Доступные компоненты

### 1. Навигация

Доступная навигация должна обеспечивать:

- Четкую структуру
- Клавиатурную навигацию
- Индикацию текущей страницы
- Понятные описания для скринридеров

```erb
<nav aria-label="Основная навигация">
  <ul class="flex space-x-4">
    <li>
      <%= link_to "Главная", 
          root_path, 
          class: "text-gray-700 hover:text-gray-900",
          "aria-current": current_page?(root_path) ? "page" : nil %>
    </li>
    <li>
      <%= link_to "О нас", 
          about_path, 
          class: "text-gray-700 hover:text-gray-900",
          "aria-current": current_page?(about_path) ? "page" : nil %>
    </li>
  </ul>
</nav>
```

### 2. Формы

Доступные формы должны включать:

- Связанные метки
- Описания ошибок
- Валидацию
- Клавиатурную навигацию

```erb
<div class="mb-4">
  <label for="email" class="block text-sm font-medium text-gray-700">
    Email
  </label>
  <input
    type="email"
    id="email"
    name="email"
    class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500"
    required
    aria-required="true"
    aria-describedby="email-error"
  >
  <p id="email-error" class="mt-1 text-sm text-red-600" role="alert">
    <%= @errors[:email] if @errors&.key?(:email) %>
  </p>
</div>
```

### 3. Модальные окна

Доступные модальные окна должны:

- Ловить фокус
- Иметь заголовок
- Закрываться по Escape
- Возвращать фокус на предыдущий элемент

```erb
<div 
  class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"
  role="dialog"
  aria-modal="true"
  aria-labelledby="modal-title"
>
  <div class="fixed inset-0 z-10 overflow-y-auto">
    <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
      <div class="relative transform overflow-hidden rounded-lg bg-white px-4 pb-4 pt-5 text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg sm:p-6">
        <div class="absolute right-0 top-0 hidden pr-4 pt-4 sm:block">
          <button 
            type="button"
            class="rounded-md bg-white text-gray-400 hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
            aria-label="Закрыть"
          >
            <span class="sr-only">Закрыть</span>
            <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>
        </div>
        <div>
          <h3 class="text-lg font-semibold leading-6 text-gray-900" id="modal-title">
            Заголовок модального окна
          </h3>
          <div class="mt-2">
            <p class="text-sm text-gray-500">
              Содержимое модального окна
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
```

## Фокус и клавиатурная навигация

### 1. Стилизация фокуса

Стили фокуса должны быть:

- Видимыми
- Контрастными
- Не нарушающими дизайн
- Последовательными

```erb
<button class="
  px-4 py-2 
  bg-blue-500 text-white 
  rounded-lg
  focus:outline-none 
  focus:ring-2 
  focus:ring-blue-500 
  focus:ring-offset-2
">
  Кнопка
</button>
```

### 2. Управление фокусом

Управление фокусом включает:

- Ловушку фокуса в модальных окнах
- Правильный порядок табуляции
- Возврат фокуса
- Программное управление фокусом

```erb
<div class="relative" x-data="{ open: false }">
  <button
    @click="open = !open"
    class="btn"
    aria-expanded="open"
    aria-controls="dropdown-menu"
  >
    Меню
  </button>
  
  <div
    x-show="open"
    @click.away="open = false"
    class="absolute right-0 mt-2 w-48 rounded-md shadow-lg"
    role="menu"
    aria-orientation="vertical"
    id="dropdown-menu"
  >
    <!-- Содержимое меню -->
  </div>
</div>
```

## Доступные изображения

Изображения должны иметь:

- Альтернативный текст
- Описания для сложных изображений
- Правильную загрузку
- Адаптивность

```erb
<%= image_tag "product.jpg",
    class: "w-full h-auto",
    alt: "Описание продукта",
    loading: "lazy" %>
```

## Практическое задание

Создайте доступную страницу со следующими элементами:

1. Навигационное меню с поддержкой клавиатуры
2. Форму с валидацией и доступными сообщениями об ошибках
3. Модальное окно с правильной семантикой
4. Таблицу с заголовками и описанием
5. Карусель изображений с доступным управлением

Требования:

- Использовать семантическую разметку
- Добавить ARIA-атрибуты где необходимо
- Обеспечить клавиатурную навигацию
- Добавить описания для изображений
- Проверить контрастность цветов

## Дополнительные материалы

1. [WAI-ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/)
2. [Web Content Accessibility Guidelines (WCAG)](https://www.w3.org/WAI/standards-guidelines/wcag/)
3. [A11Y Project](https://www.a11yproject.com/)
4. [Tailwind CSS: Focus Styles](https://tailwindcss.com/docs/hover-focus-and-other-states#focus)
