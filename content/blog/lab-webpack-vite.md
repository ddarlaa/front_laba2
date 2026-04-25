---
title: "Лабораторная работа: Webpack, Luxon, Docker и TypeScript + Vite"
date: 2026-04-25
draft: false
tags: ["webpack", "luxon", "docker", "typescript", "vite", "github pages"]
---

## Часть 1. Сборка с Webpack + запуск в Docker

### Цель
Создать статическую страницу с часами на Luxon, собрать её с помощью Webpack, запустить в Docker-контейнере.

### Инструменты
- Node.js 24 (образ `node:24-alpine`)
- Webpack 5
- Luxon
- Bootstrap (CDN)
- Docker

### Ход работы

1. Инициализирован npm-проект, установлен `luxon` и dev-зависимости `webpack webpack-cli serve`.
2. Создан `src/index.js` с импортом Luxon и обновлением времени каждую секунду.
3. Создан `index.html` с подключением Bootstrap для адаптивности и крупным отображением времени (класс `display-1`).
4. Выполнена сборка командой `npx webpack`. Результат сборки:
<img width="974" height="145" alt="image" src="https://github.com/user-attachments/assets/d4da26e1-d42b-41c7-99fe-7e5d1b23d1b1" />
<img width="974" height="357" alt="image" src="https://github.com/user-attachments/assets/aeead341-f2b2-49e3-9190-661fa39d924b" />

5. Страница проверена локально через `npx serve .`. Вид в браузере:

<img width="974" height="492" alt="image" src="https://github.com/user-attachments/assets/f6f5a22d-1a4b-4240-8c2b-40776c3aab21" />


### Запуск в Docker

Создан `Dockerfile`.
Сборка образа и запуск контейнера:
```
bash
docker build -t webpack-clock .
docker run -p 3000:3000 webpack-clock
```

Результат – приложение доступно на http://localhost:3000:
<img width="974" height="919" alt="image" src="https://github.com/user-attachments/assets/537296c4-b73e-480c-8e03-727d2fd6e0de" />

<img width="974" height="515" alt="image" src="https://github.com/user-attachments/assets/ee99804f-d266-4b84-be9c-3025b8bfdcfb" />
Часть 2. TypeScript + Luxon + Vite и деплой на GitHub Pages
Цель
Освоить связку Vite + TypeScript, автоматизировать деплой статической страницы на GitHub Pages.

Инструменты
Vite 5.x

TypeScript 5.6

Luxon

Bootstrap (CDN)

GitHub Pages (из ветки main, папка docs)
Выполнение
Проект создан с помощью npm init -y, установлены vite, typescript, luxon.

Настроен tsconfig.json для ESNext и Bundler-резолюции.

Компонент часов написан на TypeScript в src/main.ts:

ts
import { DateTime } from "luxon";

const FORMAT = "dd.LL.y HH:mm:ss";
const el = document.getElementById("clock") as HTMLHeadingElement;

function tick() {
  const now = DateTime.now();
  el.textContent = now.toFormat(FORMAT);
}

tick();
setInterval(tick, 1000);
index.html с Bootstrap и модульным подключением /src/main.ts.
vite.config.ts настроен на публикацию в поддиректорию репозитория (base) и вывод в папку docs:

ts
import { defineConfig } from "vite";

const repoName = "front_laba2"; // должно совпадать с именем репозитория

export default defineConfig({
  base: `/${repoName}/`,
  build: { outDir: "docs" }
});
После отправки кода на GitHub и настройки Pages (Source: Deploy from a branch, branch: main, folder: /docs) сработала публикация.
Финальная страница доступна по адресу: ddarlaa.github.io/time

<img width="899" height="373" alt="image" src="https://github.com/user-attachments/assets/d51d9e1b-7b95-4969-aa46-8b0118f17dfb" />

<img width="974" height="491" alt="image" src="https://github.com/user-attachments/assets/828ef4e2-3d91-424c-8cd0-4d0f364f8569" />
