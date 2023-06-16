# Tlab04
## Homework

Вы продолжаете проходить стажировку в "Formatter Inc." (см [подробности](https://github.com/tp-labs/lab03#Homework)).

В прошлый раз ваше задание заключалось в настройке автоматизированной системы **CMake**.

Сейчас вам требуется настроить систему непрерывной интеграции для библиотек и приложений, с которыми вы работали в [прошлый раз](https://github.com/tp-labs/lab03#Homework). Настройте сборочные процедуры:
* используйте [TravisCI](https://travis-ci.com/) для сборки на операционной системе **Linux** с использованием компиляторов **gcc** и **clang**;

После выполнения задания и туториала, выполним домашнее задание. Вся работа проводится после клонирования репозитория и настройки Travis CI. Однако при настройке, он потребовал подключить подписку, хоть она и бесплатная, но там нужно было указывать банковскую карту и место работы. Я не стала это делать, поэтому работа выполненна согласно источникам по правильному написанию .yml файлов для Travis CI.

[![Build Status](https://app.travis-ci.com/arixixix/Tlab04.svg?branch=main)](https://app.travis-ci.com/arixixix/Tlab04)

Создаём необходимую директорию .github/workflows для Travis CI. 
Создаём файл travis.yml и описываем сценарий сборки проекта на bash.

```sh
% mkdir .github          
% mkdir .github/workflows
% touch .github/workflows/travis.yml
% vim .github/workflows/travis.yml
```

travis.yml

```sh
name: Formatter_app

on:
 push:
   branches:
   - master

jobs:
 libraries:

   runs-on: ubuntu-latest

   steps:
   - uses: actions/checkout@v2

   - name: FormatterLib CMake
     shell: bash
     run: |
       cd formatter_lib
       cmake -H. -B_build
       cmake --build _build
   - name: FormatterExLib CMake
     shell: bash
     run: |
       cd formatter_ex_lib
       cmake -H. -B_build
       cmake --build _build

   - name: SolverLib Cmake
     shell: bash
     run: |
       cd solver_lib
       cmake -H. -B_build
       cmake --build _build
 
 Hello_world:

   runs-on: ubuntu-latest

   steps:
   - uses: actions/checkout@v2

   - name: HelloWorldApplication CMake
     shell: bash
     run: |
       mkdir _build
       cd _build
       cmake ..
       cmake --build .
       cd hello_world_application
       ./hello_world

 Solver:
   runs-on: ubuntu-latest

   steps:
   - uses: actions/checkout@v2

   - name: SolverApplication CMake
     shell: bash
     run: |
       mkdir _build2
       cd _build2
       cmake ..
       cmake --build .
       cd solver_application
       ./solve 1 2 3
```
Commit & push


Затем я поняла, что можно и нужно использовать GitHub Actions. Поняв, что файл travis.yml не рабочий, я переписала его, изменив название на build.yml

build.yml

```sh
name: Formatter

on:
  push:
    branches: 
    - main

  workflow_dispatch:

jobs:
  Build-library:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: Build formatter_lib
        shell: bash
        run: |
          cd formatter_lib
          mkdir build_ && cd build_
          cmake ..
          cmake --build .
          
      - name: Build formatter_ex_lib
        shell: bash
        run: |
          cd formatter_ex_lib
          mkdir build_ && cd build_
          cmake ..
          cmake --build .
      - name: Build solver_lib
        shell: bash
        run: |
          cd solver_lib
          mkdir build_ && cd build_
          cmake ..
          cmake --build .
  
  Hello-world:
    runs-on: ubuntu-latest
    needs: [ Build-library ]
    
    steps:
      - uses: actions/checkout@v2
      
      - name: Build Hello_world
        shell: bash
        run: |
          mkdir build1 && cd build1
          cmake ..
          cmake --build .
  
      - name: writer
        shell: bash
        run: |
          cd build1/hello_world_application
          ./hell
  Build-solver:
    runs-on: ubuntu-latest
    needs: [ Build-library ]
    
    steps:
      - uses: actions/checkout@v2
      
      - name: Build solver_application
        shell: bash
        run: |
          mkdir build2 && cd build2
          cmake ..
          cmake --build .
          
      - name: writer
        shell: bash
        run: |
          cd build2/solver_application
          ./solve 1 1 1
```

Работа исправления и удаления всех неправильных файлов проводилась в удалённом репозитории.

Далее проводилась работа по исправлению неточностей в самих файлах CMakeLists.txt для hello_world, основного Мэйк-файла и исправление ошибок в названии файла в solver. (Они возникли, как я поняла из-за того, что я скопировала старую версию ЛР3 у себя на компьютере)

Уже после исправления всех недочётов все тесты выполнились успешно!

[![Formatter](https://github.com/arixixix/Tlab04/actions/workflows/build.yml/badge.svg)](https://github.com/arixixix/Tlab04/actions/workflows/build.yml)





