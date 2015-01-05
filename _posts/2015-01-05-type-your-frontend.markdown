---
layout: post
title:  "Типизируйте ваш фронтенд. Часть 1. Готовим инструментарий"
date:   2015-01-05 12:00
languages: TypeScript
categories:
---

Для начала скажу пару слов о том, для кого я пишу эту статью. Эта статья для тех, кто хотел бы попробовать написать свой новый (или переделать текущий) проект на TypeScript.  Для начала вы можете прочить вводную [статью](http://panferov.me/2014/11/30/typescript.html) о том,  зачем нужен TypeScript.

Я предполагаю, что это будет Single Page Application на ReactJS, сборка которого осуществляется при помощи Grunt. Также я предполагаю, что вы уже имеете опыт в разработке одного или нескольких подобных проектов без использования TypeScript.

Тем не менее, этот цикл статей также будет полезен и для новичков, так как он описывает технологии и способы, которые можно применять даже в своем первом учебном проекте.. Но я не буду слишком уж углубляться в детали и объяснения, простите меня за это заранее. 

Для начала работы с TypeScript в вашем следующем проекте, вам потребуется установить и настроить несколько полезных инструментов. Этот этап традиционно считается самым скучным в подобного рода статьях, но обойтись без него крайне сложно, поэтому давайте расчехлим нашу консоль и запасемся терпением.

## Файловая структура проекта

Тут я просто приведу предполагаемую структуру проекта, чтобы новичкам было с чего начать.

~~~
.
├── Gruntfile.js
├── LICENSE
├── README.md
├── bower.json
├── bower_components
├── node_modules
├── dist
│   ├── тут будут содержаться уже собранные файлы вашего приложения
├── index.html
├── package.json
├── specs
│   ├── сюда вы можете положить тесты
├── src
│   ├── app
|       ├── run.ts
│   ├── fake
│   ├── locales
│   ├── styles
│   └── vendor
├── tsd.json
└── typings
    ├── файлы DefinitelyTyped
~~~

## TypeScript

В первую очередь, вам понадобиться компилятор TypeScript последней доступной публичной версии. 

~~~bash
npm install -g typescript
~~~

Я сам предпочитаю master-ветку TypeScript, так как там в первую очередь появляются фиксы и новая функциональность. Для установки TypeScript из мастер-ветки выполните:

~~~bash
git clone https://github.com/Microsoft/TypeScript.git
cd TypeScript
jake LKG
npm link
~~~

## Grunt

Существует несколько готовых инструментов для интеграции TypeScript и Grunt, например [grunt-ts](https://github.com/TypeStrong/grunt-ts). У них есть довольно подробный мануал по настройке и я не буду повторять его здесь. 

Сам же я пользуюсь своим велосипедом, который я вставил прямо в Gruntfile:

~~~js
function executeTsc(args, resolve) {
    grunt.util.spawn({
        cmd: 'tsc',
        args: args
    }, function (error, result, code) {
        var ret = {
            code: code,
            // New TypeScript compiler uses stdout for user code errors. Old one used stderr.
            output: result.stdout || result.stderr
        };
        resolve(ret);
    });
}

grunt.registerTask('ts:dev', 'Build application', function () {
    var done = this.async();
    executeTsc(["--target", "ES5",
        "--sourceMap",
        "--declaration",
        "--noImplicitAny",
        "--preserveConstEnums",
        "--module", "commonjs",
        "--out", "dist/app.js",
        "src/app/run.ts"
    ], function (ret) {
        grunt.log.writeln(ret.output);

        if (ret.code != 0) {
            grunt.fail.fatal("TSC error", ret.code);
        }

        done()
    })
});
~~~

Он позволяет довольно гибко настраивать все, что мне нужно и избавляет от лишней зависимости.  

## Система сборки

В моем текущем проекте я не использую сторонние системы сборки, такие как [require-js](http://requirejs.org/) или [browserify](http://browserify.org/). ~~В свое время я намучился с ними~~. Вместо этого я использую внутренний сборщик TypeScript и склеиваю библиотеки при помощи [grunt-contrib-concat](https://github.com/gruntjs/grunt-contrib-concat).  Я очень жду наивную поддержку es6-modules в TypeScript, после чего с радостью перейду на нее.

Тем не менее, вы можете воспользоваться любимой системой сборки в вашем проекте, TypeScript совместим как с AMD, так и с CommonJS модулями. Почитать об этом можно в [документации](http://www.typescriptlang.org/Handbook#modules) к TypeScript.

Остальные настройке в вашем Gruntfile остаются на ваше усмотрение. Я рекомендую настроить connect, watch и sass.

## DefinitelyTyped

Следующим шагом в ознакомлении с миром TypeScript обязательно должно стать знакомство с проектом [DefinitelyTyped](http://definitelytyped.org/). Этот проект представляет из себя огромный набор `.d.ts` файлов для большинства известных или не очень известных JavaScript библиотек. В этих файлах содержится информация о типах, которую TypeScript может использовать для проверок и поиска ошибок в вашем приложении.

Используйте [TypeScript Definition manager](http://definitelytyped.org/tsd/) для загрузки нужных вам файлов из общего набора.

## Заключение

Я надеюсь что все эти настройки не слишком утомили вас и вы продолжите чтение следующих частей этого цикла, которые будут гораздо интереснее, уверяю вас. 

Я буду дописывать эту статью по ходу цикла, если выяснится, что я что-то забыл или написал неправильно. 

До скорой встречи, и да пребудет с вами сила.