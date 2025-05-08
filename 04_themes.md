
# Темы в tailwind css

Настройка тем осуществляется с помощью директивы `@theme`, внутри которой определяется настройка темы, шрифты, отступы, цвета и т д.
Значения или же токены определяются через переменные внутри директивы theme.

Например, возьмем конечный класс `bg-mint-500`. Для того чтобы изменить значения для цвета `mint-500`, вам нужно определить переменную с соответствующим значением:

```css
@theme {
  --color-mint: {
    500: #0ea5e9;
  }
}
```

Более подробно о конвенциях и возможных значениях вы можете прочитать [вот тут](https://tailwindcss.com/docs/theme#theme-variable-namespaces)

Ниже приведены другие примеры

## Базовые примеры

### Цветовая палитра

```css
@theme {
  /* Основные цвета */
  --color-primary: {
    50: #f0f9ff;
    100: #e0f2fe;
    200: #bae6fd;
    300: #7dd3fc;
    400: #38bdf8;
    500: #0ea5e9;
    600: #0284c7;
    700: #0369a1;
    800: #075985;
    900: #0c4a6e;
  }

  /* Дополнительные цвета */
  --color-secondary: {
    50: #f5f3ff;
    100: #ede9fe;
    /* ... */
  }
}
```

### Typography

```css
@theme {
  /* Настройки Typography */
  --typography-default: {
    max-width: 65ch;
    color: inherit;
    
    a: {
      color: inherit;
      text-decoration: underline;
      font-weight: 500;
    }
  }
}
```

### Breakpoints

```css
@theme {
  /* Breakpoints */
  --breakpoint-sm: 640px;
  --breakpoint-md: 768px;
  --breakpoint-lg: 1024px;
  --breakpoint-xl: 1280px;
  --breakpoint-2xl: 1536px;
  
  /* Пользовательские breakpoints */
  --breakpoint-tablet: 960px;
  --breakpoint-desktop: 1280px;
}
```

## Dark mode

Tailwind по умолчанию предоставляет dark mode тему по мимо основной светлой.

### Определение темы

Tailwind поддерживает несколько способов определения текущей темы:

1. **Системные настройки** (по умолчанию):

   ```css
   @dark-mode media;
   ```

   В этом случае тема определяется на основе системных настроек пользователя (`prefers-color-scheme`). Если пользователь установил темную тему в системе, будет использоваться dark mode.

2. **Класс на элементе**:

   ```css
   @dark-mode class;
   ```

   В этом случае тема определяется наличием класса `dark` на элементе (обычно на `<html>` или `<body>`). Это позволяет переключать тему программно.

3. **Комбинированный подход**:

   ```css
   @dark-mode media class;
   ```

   Позволяет использовать оба подхода одновременно - сначала проверяются системные настройки, но пользователь может переопределить их через класс.

### Пример переключения темы

Для переключения темы в режиме `class` необходимо добавить/удалить класс `dark` на корневом элементе. Вот пример реализации на JavaScript:

```javascript
// Переключение темы
function toggleTheme() {
  if (document.documentElement.classList.contains('dark')) {
    document.documentElement.classList.remove('dark');
    localStorage.setItem('theme', 'light');
  } else {
    document.documentElement.classList.add('dark');
    localStorage.setItem('theme', 'dark');
  }
}

// Инициализация темы при загрузке страницы
function initTheme() {
  const savedTheme = localStorage.getItem('theme');
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
  
  if (savedTheme === 'dark' || (!savedTheme && prefersDark)) {
    document.documentElement.classList.add('dark');
  }
}

// Инициализация при загрузке
initTheme();

// Слушатель изменений системной темы
window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', (e) => {
  if (!localStorage.getItem('theme')) {
    if (e.matches) {
      document.documentElement.classList.add('dark');
    } else {
      document.documentElement.classList.remove('dark');
    }
  }
});
```

### Использование в разметке

Классы которые должны быть применены для dark темы помечаются префиксом `dark:`

```html
<div class="bg-white dark:bg-gray-800">
  <p class="text-gray-500 dark:text-gray-400 mt-2 text-sm">
    The Zero Gravity Pen can be used to write in any orientation, including upside-down. It even works in outer space.
  </p>
</div>
```

Где:

- `bg-white` - фон для основной темы
- `dark:bg-gray-800` - фон для темной темы
- `text-gray-500` - цвет текста для основной темы
- `dark:text-gray-400` - цвет текста для темной темы

### Кастомизация тем

Вы можете определить собственные значения для темной темы:

```css
@theme {
  /* Основные цвета */
  --color-primary: {
    50: #f0f9ff;
    100: #e0f2fe;
    /* ... */
  }

  /* Темная тема */
  --color-primary-dark: {
    50: #0c4a6e;
    100: #075985;
    /* ... */
  }
}
```

### Групповые модификаторы

Для применения стилей к нескольким элементам в темной теме можно использовать групповые модификаторы:

```html
<div class="group">
  <div class="bg-white group-dark:bg-gray-800">
    <p class="text-gray-500 group-dark:text-gray-400">
      Текст, который меняет цвет в темной теме
    </p>
  </div>
</div>
```

## Создание дополнительных тем

Tailwind позволяет создавать неограниченное количество пользовательских тем помимо стандартных светлой и темной.
Допустим вам нужно добавить дополнительные темы по мимо основных чтобы пользователь переключался между ними или же у вас платформа для создания интернет-магазинов как shopify, где пользователь может выбрать из списка подходящую для его магазина тему, которая в свою очередь может иметь несколько вариантов, e.g светлую или темную.

### Создание дополнительной пользовательской темы

1. **Определение темы в конфигурации**:

```css
@theme {
  /* Основная тема (светлая) */
  --color-primary: {
    50: #f0f9ff;
    100: #e0f2fe;
    /* ... */
  }

  /* Темная тема */
  --color-primary-dark: {
    50: #0c4a6e;
    100: #075985;
    /* ... */
  }

  /* Пользовательская тема (например, "летняя") */
  --color-primary-summer: {
    50: #f0fdf4;
    100: #dcfce7;
    200: #bbf7d0;
    300: #86efac;
    400: #4ade80;
    500: #22c55e;
    600: #16a34a;
    700: #15803d;
    800: #166534;
    900: #14532d;
  }
}
```

1. **Активация темы**:

Для активации пользовательской темы используется класс на корневом элементе:

```html
<html class="theme-summer">
  <!-- Содержимое страницы -->
</html>
```

1. **Использование в разметке**:

```html
<div class="bg-primary-500 theme-summer:bg-primary-summer-500">
  <p class="text-gray-900 theme-summer:text-green-900">
    Текст, который меняет цвет в летней теме
  </p>
</div>
```

#### Переключение между темами

Для переключения между темами можно использовать следующий JavaScript код:

```javascript
// Переключение темы
function setTheme(themeName) {
  // Удаляем все классы тем
  document.documentElement.classList.remove(
    'theme-summer',
    'theme-autumn',
    'theme-winter',
    'theme-spring',
    'theme-brand',
    'theme-partner',
    'theme-holiday',
    'theme-high-contrast'
  );
  
  // Добавляем выбранную тему
  if (themeName) {
    document.documentElement.classList.add(`theme-${themeName}`);
  }
  
  // Сохраняем выбор пользователя
  localStorage.setItem('theme', themeName);
}

// Инициализация темы при загрузке
function initTheme() {
  const savedTheme = localStorage.getItem('theme');
  if (savedTheme) {
    setTheme(savedTheme);
  }
}

// Инициализация при загрузке
initTheme();
```

### Разделение тем на отдельные файлы

Для лучшей организации кода и поддержки, темы можно разделить на отдельные файлы. Это особенно полезно при работе с большим количеством тем которые могут иметь несколько вариантов в свою очередь, как было упомянуто выше - кейс с платформой для создания сайтов.

1. **Создание отдельных файлов тем**:

```css
/* app/assets/stylesheets/themes/summer.css */
@theme summer {
  --color-primary: {
    50: #f0fdf4;
    100: #dcfce7;
    200: #bbf7d0;
    300: #86efac;
    400: #4ade80;
    500: #22c55e;
    600: #16a34a;
    700: #15803d;
    800: #166534;
    900: #14532d;
  }
}

/* app/assets/stylesheets/themes/autumn.css */
@theme autumn {
  --color-primary: {
    50: #fff7ed;
    100: #ffedd5;
    200: #fed7aa;
    300: #fdba74;
    400: #fb923c;
    500: #f97316;
    600: #ea580c;
    700: #c2410c;
    800: #9a3412;
    900: #7c2d12;
  }
}
```

1. **Импорт тем в основной файл**:

```css
/* app/assets/stylesheets/application.tailwind.css */
@import "tailwindcss";
@import "themes/summer";
@import "themes/autumn";

/* Основная конфигурация */
@source "../../../app/views/**/*.html.erb";
@source "../../../app/views/**/*.rb";
@source "../../../app/helpers/**/*.rb";
```

1. **Использование в разметке**:

```html
<div class="bg-primary-500 theme-summer:bg-primary-500 theme-autumn:bg-primary-500">
  <p class="text-gray-900 theme-summer:text-green-900 theme-autumn:text-orange-900">
    Текст, который меняет цвет в зависимости от темы
  </p>
</div>
```

1. **Условная загрузка тем**:

Для оптимизации производительности можно загружать темы только когда они нужны.

В этом случае можно определять общие настройки в `application.css` а настройки темы в отдельной файле, например:

```erb
<!-- app/views/layouts/application.html.erb -->
<head>
  <!-- Общие стили -->
  <%= vite_stylesheet_tag 'application' %>

  <!-- Тема магазина -->
    <%= vite_stylesheet_tag "themes/#{current_theme}" %>
</head>
```

Где `current_theme` название текущей темы, которое может хранится в бд или где то еще

### Дополнительные материалы

1. [Dark mode](https://tailwindcss.com/docs/dark-mode)
2. [Theme](https://tailwindcss.com/docs/theme)
