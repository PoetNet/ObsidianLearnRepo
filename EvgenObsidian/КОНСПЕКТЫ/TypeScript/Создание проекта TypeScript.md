Давайте составим первый программный код на TypeScript c использованием установленных инструментов. Сначала поработаем с консолью и Node.js.

### "Hello, World!" в node.js

Наша первая программа будет традиционно выводить строку `"Hello, World!"` на консоль. Заодно мы проверим всю цепочку компиляции кода TypeScript. Для этого:

- Создадим новую папку для хранения кода, например, `helloworld`, непосредственно из редактора VS Code.

![](https://ucarecdn.com/f41291c8-f361-4614-ab4e-87abb311f004/)

- Создадим новый файл TypeScript с именем `app.ts`. Расширение файла TypeScript — `.ts`.

![](https://ucarecdn.com/4832aba1-5369-4061-aa7e-4cf5c7736fb6/)

- Напишем в файле `app.ts` следующий код:

```
let message: string = 'Hello, World!';
console.log(message);
```

![](https://ucarecdn.com/4dc936de-e18c-49e4-b62d-eeac6d94ed5e/)

- Сохраним отредактированный файл с помощью сочетания клавиш `Ctrl+S`
- Запустим новый терминал в VS Code с помощью сочетания клавиш `` Ctrl+` `` или через меню `Terminal > New Terminal`

![](https://ucarecdn.com/57f52838-2b57-4692-af24-a2cf89c2e846/)

- ​Введем в `Терминале` команду `tsc app.ts`, чтобы скомпилировать файл `app.ts`

> Если при запуске мы получили ошибку вида "tsc : Невозможно загрузить файл tsc.ps1, так как выполнение сценариев отключено в этой систем.". Это часто проявляется под Windows. Для решения проблемы необходимо запустить от имени администратора PowerShell и в открывшемся терминале написать команду `Set-ExecutionPolicy RemoteSigned.`
> 
>  Это изменит политику выполнения и компиляция станет возможной.

Если все в порядке, вы увидите файл `app.js`, который  сгенерировал компилятор TypeScript.

![](https://ucarecdn.com/51b3075d-7723-4f3c-aa82-e5ec57aae414/)

Чтобы запустить файл `app.js` в `node.js`, используем следующую команду:

```shell
node app.js
```

![](https://ucarecdn.com/80c44fae-211d-4a04-9078-2df9203a8812/)

Можно немного сократить количество манипуляций, благодаря установленному модулю `ts-node`, и использовать только одну команду для компиляции и выполнения файла TypeScript:

```shell
ts-node app.ts
```

![](https://ucarecdn.com/bf82fd15-efe6-4600-9851-1f1f09e713ff/)

### Программа "Hello, World!" в веб-браузере

Рассмотрим еще один способ настройки среды разработки - когда мы используем браузер для рендеринга кода html + javascript. И для этого нам понадобится установленный плагин `Live Server`.

Создадим новый файл с именем `index.html`  в том же каталоге `helloworld` и и добавим в него код:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TypeScript: Hello, World!</title>
</head>
<body>
    <script src="app.js"></script>
</body>
</html>
```

![](https://ucarecdn.com/146dc1d9-885c-4ac5-a13e-d0ace2c2bc57/)

Дополним код `app.ts` :

```TypeScript
let message: string = 'Hello, World!';
let heading = document.createElement('h1');
heading.textContent = message;
document.body.appendChild(heading);
```

Сохраним отредактированный файл `Ctrl+S`

Скомпилируем файл `app.ts`:

```shell
tsc app.ts
```

Откроем `Live Server` из `VS Code`, щелкнув правой кнопкой мыши на файле `index.html` и выбрав опцию `Open with Live Server`:

![](https://ucarecdn.com/70da5731-3c96-46ed-b92b-1ef66169d61e/)

`Live Server` откроет `index.html` в новой вкладке браузера по умолчанию.

![](https://ucarecdn.com/e0504189-fa7a-443e-942a-30af45366053/)

Чтобы внести какие-то изменения в программный код - редактируем файл `app.ts`. Например:

```
let message: string = 'Hello, TypeScript!';
let heading = document.createElement('h1');
heading.textContent = message;
document.body.appendChild(heading);
```

Скомпилируем файл `app.ts`:

```
tsc app.ts
```

Компилятор TypeScript сгенерирует новый файл `app.js`, а `Live Server` автоматически загрузит его в веб-браузер.

![](https://ucarecdn.com/0ac761f4-1612-47a9-90f2-54a8c13ac888/)

Обратите внимание, что `app.js` является выходным файлом файла `app.ts`, поэтому никогда не следует напрямую изменять код в `app.js`, иначе потеряются изменения после перекомпиляции файла `app.ts`.
