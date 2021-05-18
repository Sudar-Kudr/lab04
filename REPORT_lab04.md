[![Build Status](https://travis-ci.com/Sudar-Kudr/lab04.svg?branch=main)](https://travis-ci.com/Sudar-Kudr/lab04)
## Laboratory work IV

Данная лабораторная работа посвещена изучению систем непрерывной интеграции на примере сервиса **Travis CI**

```sh
$ open https://travis-ci.org
```

## Tasks

- [ok] 1. Авторизоваться на сервисе **Travis CI** с использованием **GitHub** аккаунта
- [ok] 2. Создать публичный репозиторий с названием **lab04** на сервисе **GitHub**
- [ok] 3. Ознакомиться со ссылками учебного материала
- [ok] 4. Включить интеграцию сервиса **Travis CI** с созданным репозиторием
- [ok] 5. Получить токен для **Travis CLI** с правами **repo** и **user**
- [ok] 6. Получить фрагмент вставки значка сервиса **Travis CI** в формате **Markdown**
- [ok] 7. Выполнить инструкцию учебного материала
- [ok] 8. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```sh
$ export GITHUB_USERNAME=<имя_пользователя>    #присваиваем <имя_пользователя> в переменную GITHUB_USERNAME
$ export GITHUB_TOKEN=<полученный_токен>      #присваиваем <полученный_токен> в переменную GITHUB_TOKEN
```

```sh
$ cd ${GITHUB_USERNAME}/workspace    #спускаемся в workspace
$ pushd .                           #добавляем в стек текущий каталог
$ source scripts/activate          #выполняем скрипт
```

```sh
$ \curl -sSL https://get.rvm.io | bash -s -- --ignore-dotfiles    #скачиваем файл по ссылке | игноририруем скрытые файлы
$ echo "source $HOME/.rvm/scripts/rvm" >> scripts/activate       #записываем файл в скрипт-файл
$ . scripts/activate                                            #выполняем скрипт
$ rvm autolibs disable                                         #отключеним дополнительные действия
$ rvm install ruby-2.4.2                                      #устанавливаем ruby 2.4.2
$ rvm use 2.4.2 --default                                    #используем версию ruby 2.4.2 по умолчанию
$ gem install travis                                        #устанавливаем travis
```

```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab03 projects/lab04   #клонируем репозиторий из lab03 в директорию projects/lab04
$ cd projects/lab04                                                     #переходим директорию projects/lab04
$ git remote remove origin                                             #удаляем старую ссылку репозитория
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab04   #добавляем ссылку репозитория в управление репозиториями
```

```sh
$ cat > .travis.yml <<EOF            #открываем файл и пишем данные языка программирования от EOF до EOF
language: cpp
EOF
```

```sh
$ cat >> .travis.yml <<EOF           #открываем файл .travis.yml и пишем данные скрипта от EOF до EOF

script:
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build
- cmake --build _build --target install
EOF
```

```sh
$ cat >> .travis.yml <<EOF           #открываем файл .travis.yml и пишем данные от EOF до EOF

addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
EOF
```

```sh
$ travis login --github-token ${GITHUB_TOKEN}   #авторизируемся на travis
```

```sh
$ travis lint                         #проверяем .travis.yml для любых проблем, которые он может обнаружить
```

```sh
$ ex -sc '1i|<фрагмент_вставки_значка>' -cx README.md    #вставляем Markdown значок от сервиса Travis CI
```

```sh
$ git add .travis.yml         #добавляем .travis.yml
$ git add README.md            #добавляем README.md
$ git commit -m "added CI"      #закомментируем их
$ git push origin main           #отправляем данные на сервер, в удаленный репозиторий main
```

```sh
$ travis lint          #проверяем .travis.yml для любых проблем, которые он может обнаружить
$ travis accounts       #посмотрим аккаунты и их статус
$ travis sync            #синхронизируем с github
$ travis repos            #посмотрим список репозиториев
$ travis enable           #делаем проект доступным
$ travis whatsup          #посмотрим список последних сборок
$ travis branches        #посмотрим список последних сборок всех веток
$ travis history        #посмотрим историю сборок
$ travis show          #посмотрим сборку
```

## Report

```sh
$ popd                                                                           #удаляем из стека текущий каталог
$ export LAB_NUMBER=04                                                          #присваиваем 04 в переменную LAB_NUMBER
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER} #клонируем из ссылки в директорию (в наше случае-tasks/lab04)
$ mkdir reports/lab${LAB_NUMBER}                                              #создаем в директории reports папку (в нашем случае- lab04)                                      
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md     #спускаемся в директорию (в наше случае- lab04)
$ cd reports/lab${LAB_NUMBER}                                               #копируем из одной директории в другую
$ edit REPORT.md                                                           #редактируем REPORT.md
$ gist REPORT.md                                                          #сохраняем REPORT.md
```

## Homework

Вы продолжаете проходить стажировку в "Formatter Inc." (см [подробности](https://github.com/tp-labs/lab03#Homework)).
В прошлый раз ваше задание заключалось в настройке автоматизированной системы **CMake**.
Сейчас вам требуется настроить систему непрерывной интеграции для библиотек и приложений, с которыми вы работали в [прошлый раз](https://github.com/tp-labs/lab03#Homework). Настройте сборочные процедуры на различных платформах:
* используйте [TravisCI](https://travis-ci.com/) для сборки на операционной системе **Linux** с использованием компиляторов **gcc** и **clang**;
* используйте [AppVeyor](https://www.appveyor.com/) для сборки на операционной системе **Windows**.

1. Создание файла .travis.yml, где определяем язык программирования, опер. систем. и компиляторы.
```sh
$ cat > .travis.yml <<EOF
language: cpp //язык программирования

compiler:     //компиляторы
- gcc
- clang
os:          //опер. систем
 - linux
  
script:
- cd solver_application
- cmake .
- make
- cd ..
- cd hello_world_application
- cmake .
- make

addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
      - mingw-w64
EOF
```

2. В Travis CI появилась ошибка о пути, поэтому мне пришлось удалить несколько файлов в папке hello_world_application и в solver_application, потому что они както связаны с lab03 и это мешало тревису.

3. Запустил Travis CI и появилась ошибка в самом ```.cpp``` файле, поэтому добавил версию компилятора в solver_application/CMakeLists.txt эту строку:
```add_definitions(-std=c++11)```
которая говорит о том, что нужно использовать 11 версию С++.

4. Запустил Travis CI и все стало хорошо


```
Copyright (c) 2015-2021 The ISC Authors
```
