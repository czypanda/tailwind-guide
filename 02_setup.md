# Настройка Tailwind CSS

## Настройка Tailwind css в rails приложениях

С тем как настроить tailwind для rails приложения можно ознакомится в [гайдлайне](../../guidelines/front-end-stack-setup/).

Существуют так же другие способы настройки tailwind css на различных проектах, с ними при желании и необходимости вы можете ознакомится [вот тут](https://tailwindcss.com/docs/installation/using-vite).

## Tailwind CSS конфигурация

### Что такое конфигурация Tailwind CSS?

В Tailwind CSS v4 конфигурация осуществляется через CSS-файлы, что делает настройку более простой и интуитивной. Основные преимущества:

- Отсутствие необходимости в JavaScript-конфигурации
- Более быстрая компиляция
- Лучшая интеграция с современными сборщиками
- Упрощенная поддержка и обновление

Несмотря на то что js конфиг все еще поддерживается, использовать его не рекомендуется, особенно для новых проектов. Поддержка js конфиг как говорят авторы будет убрана в новых версиях.

### Различия между версиями 3 и 4

### Tailwind CSS v3

- Использовал JavaScript-конфигурацию (`tailwind.config.js`)
- Требовал PostCSS для обработки CSS
- Имел более сложную систему настройки тем
- Использовал JIT (Just-In-Time) компиляцию

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    './public/*.html',
    './app/helpers/**/*.rb',
    './app/frontend/**/*.js',
    './app/views/**/*.{erb,html}'
  ],
  theme: {
    colors: {
      'blue': '#1fb6ff',
      'purple': '#7e5bef',
      'pink': '#ff49db',
      'orange': '#ff7849',
      'green': '#13ce66',
      'yellow': '#ffc82c',
      'gray-dark': '#273444',
      'gray': '#8492a6',
      'gray-light': '#d3dce6',
    },
    fontFamily: {
      sans: ['Graphik', 'sans-serif'],
      serif: ['Merriweather', 'serif'],
    },
    extend: {
      spacing: {
        '8xl': '96rem',
        '9xl': '128rem',
      },
      borderRadius: {
        '4xl': '2rem',
      }
    }
  },
  plugins: []
};
```

### Tailwind CSS v4

- CSS-based конфигурация
- Встроенная поддержка Vite
- Упрощенная настройка через CSS-переменные
- Улучшенная производительность
- Более интуитивная система тем

```css
/* app/assets/stylesheets/application.tailwind.css */
@import "tailwindcss";

/* Определение путей для сканирования, с помощью source указываются файла в которых может быть использован tailwind css. */
@source "../../../app/views/**/*.html.erb";
@source "../../../app/views/**/*.rb";
@source "../../../app/helpers/**/*.rb";

/* Пример настройки темы */
@theme {
  /* Цветовая палитра */
  --color-primary-50: #f0f9ff;
  --color-primary-100: #e0f2fe;
  --color-primary-200: #bae6fd;
  --color-primary-300: #7dd3fc;
  --color-primary-400: #38bdf8;
  --color-primary-500: #0ea5e9;
  --color-primary-600: #0284c7;
  --color-primary-700: #0369a1;
  --color-primary-800: #075985;
  --color-primary-900: #0c4a6e;

  /* Типографика */
  --font-family-sans: Inter, system-ui, -apple-system, sans-serif;
  --font-family-serif: Merriweather, Georgia, serif;
  
  /* Размеры */
  --spacing-128: 32rem;
  --spacing-144: 36rem;
  
  /* Скругления */
  --border-radius-4xl: 2rem;
}

/* Подключение плагинов */
@plugin forms;
@plugin typography;

/* Настройка режима тем */
@dark-mode class;

/* Настройка префикса */
@prefix tw-;
```
