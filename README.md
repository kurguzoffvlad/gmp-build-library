# ПРОЕКТ - Сборка для библиотеки
## Разработка с использованием [PARCEL](https://ru.parceljs.org/getting_started.html)

## 1.0 Установить NODE - лучше через nvm:
#### Для nvm - если node устанавливалась ранее через nodejs.org - удалить node
#### nvm install 20.10.0
#### nvm use 20.10.0
## 1.1. Другой вариант - Установка node без nvm - npm i node@20.10.0

## 2. Установить PARCEL - npm install -g parcel-bundler
#### ПРИМЕЧАНИЕ - Для разных NODE нужна "своя установка parcel"

## 3. В итоге получаем:
### node 20.10.0 (node -v)
### npm 10.2.3 (npm -v)
### parcel 1.12.5 (parcel --version)


##  [Параметры CLI tsc](https://runebook.dev/ru/docs/typescript/compiler-options)

## Сборки (npm):
### Run `parcel-dev` - для отладки и дебага запускает html файл на [localhost](http://localhost:1234)
#### Работать с файлом - src\lib\index.ts
### Run `LIB-BUILD` - осуществляет сборку для библиотеки ..."чтобы проверить собирается сборка"

### Run `LIB-TO-NPM-PATCH` - ОСНОВНОЕ - собирает проект и публикует с patch версией (последняя цифра)
### Run `LIB-TO-NPM-MINOR` - собирает проект и публикует с minor версией (средняя цифра)
### Run `LIB-TO-NPM-MAJOR` - собирает проект и публикует с major версией (первая цифра)

## Скрипты:
```
    "scripts": {
        "parcel-dev": "npm run _delete && parcel src/index.html",
        "LIB-BUILD": "npm run _delete-declaration-minify && npm run _copy",
        "LIB-TO-NPM-PATCH": "npm run _delete-declaration-minify && npm run _version-patch && npm run _copy-publish",
        "LIB-TO-NPM-MINOR": "npm run _delete-declaration-minify && npm run _version-minor && npm run _copy-publish",
        "LIB-TO-NPM-MAJOR": "npm run _delete-declaration-minify && npm run _version-major && npm run _copy-publish",
        "_minify": "jsmin -o dist/index.js dist/index.js",
        "_delete": "node -e \"const fs = require('fs'); fs.rmSync('dist', { recursive: true, force: true });\"",
        "_copy": "node -e \"const fs = require('fs'); const data = fs.readFileSync('package.json'); fs.writeFileSync('dist/package.json', data);\"",
        "_version-major": "npm --no-git-tag-version version major",
        "_version-minor": "npm --no-git-tag-version version minor",
        "_version-patch": "npm --no-git-tag-version version patch",
        "_delete-declaration-minify": "npm run _delete && tsc --declaration && npm run _minify",
        "_copy-publish": "npm run _copy && npm publish",
        "parcel-build": "parcel build src/index.html"
    },
```

## tsconfig.json
```
{
  "compilerOptions": {
    "noImplicitAny": false,
    "noEmitOnError": true,
    "removeComments": false,
    "sourceMap": true,
    "target": "es5",
    "outDir": "dist",
    "baseUrl": "./src/lib",
    "declaration": true,
    "typeRoots": [] - !!!нужен для корректной сборки,
  },
  "exclude": [
      "./src/parcel/*",
  ]
}
```

## Особенности:
#### "typeRoots": [] в gmp-test\tsconfig.json - нужен для корректной сборки,
#### без "typeRoots": [] "валятся ошибки" при билде
