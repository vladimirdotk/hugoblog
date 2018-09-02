---
title: "Nodejs. Вспомнить всё. Основы."
date: 2018-08-31T22:07:04+03:00
---

<img class="blog-pic" src="https://nodejs.org/static/images/logos/nodejs-new-pantone-black.png" />

Node или Node.js — программная платформа, основанная на движке V8 (транслирующем JavaScript в машинный код), превращающая JavaScript из узкоспециализированного языка в язык общего назначения. Решил посмотреть несколько курсов по Node.js, вспомнить основные моменты, которые зачастую забываются...в основном потому, что не очень часто, если не сказать редко, используются. Речь пойдет не только о самой "ноде", но и об окружении и сопутствующих библиотеках.
<!--more-->

### Server
Простейший сервер на "ноде":
{{< highlight js >}}
const {Server} = require('http');

const server = new Server((req, res) => {
  res.end("hello world");
});

server.listen(3000);
{{< /highlight >}}


### Eslint
Для единого стиля и подсказок по нему в редакторе как правило используют `eslint`. Наиболее популярная конфигурация - от `airbinb`. Установить её можно следующим образом:

{{< highlight sh >}}
npx install-peerdeps --dev eslint-config-airbnb-base
{{< /highlight >}}

После чего, в конфигурационный файл `.eslintrc` добавить:
{{< highlight json >}}
"extends": "airbnb-base"
{{< /highlight >}}

### Require

Где ищет require?

{{< highlight js >}}
require('some-module')
{{< /highlight >}}

+ встроенные модули
+ node_modules (в проекте, затем выше)
+ переменная окружения `NODE_PATH`

### Hotreload

Для hotreload'а используют модуль `nodemon`, установленный глобально:

{{< highlight js >}}
nodemon index.js
{{< /highlight >}}

### Отладка

Для отладки в `vscode` достаточно зайти в меню `отладка`, добавить новый отладчик `Node.js`, указать в открывшемся конфигурационном файле стартовый скрипт, сохранить файл, поставить точку(-и) останова, выполнить запрос к своем серверу. 

### Порядок выполнения кода

{{< highlight js >}}

fs.open(__filename, 'F', (err, fd) => {
  console.log('fs.open');
}) // 4

setImmediate(() => {
  console.log('immediate');
}) // 5

process.nextTick(() => {
  console.log('next tick');
}) // 2

new Promise(resolve => {
  resolve('promise')
}).then(console.log); // 3

console.log('some text') // 1
{{< /highlight >}}