[![Build Status](https://travis-ci.org/DarthBarada/lab05.svg?branch=master)](https://travis-ci.com/DarthBarada/lab05)
## Laboratory work V

Данная лабораторная работа посвещена изучению фреймворков для тестирования на примере **GTest**

```ShellSession
$ open https://github.com/google/googletest
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab05** на сервисе **GitHub**
- [X] 2. Выполнить инструкцию учебного материала
- [X] 3. Ознакомиться со ссылками учебного материала
- [X] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

Настройка окружения
```ShellSession
$ export GITHUB_USERNAME=<имя_пользователя>
$ alias gsed=sed # for *-nix system # Синоним команды gsed
```
Подготовка окружения
```ShellSession
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
~/DarthBarada/workspace ~/DarthBarada/workspace
$ source scripts/activate
```
Получение файлов из lab04
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab04 projects/lab05  # Скачивание из удаленного репозитория в папку lab05
Клонирование в «projects/lab05»…
remote: Enumerating objects: 3021, done.
remote: Counting objects: 100% (3021/3021), done.
remote: Compressing objects: 100% (2312/2312), done.
remote: Total 3021 (delta 531), reused 3005 (delta 523), pack-reused 0
Получение объектов: 100% (3021/3021), 13.49 MiB | 5.09 MiB/s, готово.
Определение изменений: 100% (531/531), готово.
$ cd projects/lab05   # Переход в папку lab05
$ git remote remove origin   # Удаление ссылки на удаленный репозиторий из локального
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab05   # Указание новой ссылки на удаленный репозиторий
```

```ShellSession
$ mkdir third-party   # Создание папки third-party
$ git submodule add https://github.com/google/googletest third-party/gtest  # Скачивание удаленного репозитория в указанную папку
Клонирование в «/home/darthbarada/DarthBarada/workspace/projects/lab05/third-party/gtest»…
remote: Enumerating objects: 16863, done.
remote: Total 16863 (delta 0), reused 0 (delta 0), pack-reused 16863
Получение объектов: 100% (16863/16863), 5.75 MiB | 4.56 MiB/s, готово.
Определение изменений: 100% (12438/12438), готово.
$ cd third-party/gtest && git checkout release-1.8.1 && cd ../..  # Переход в указанную папку, переход в указанную ветку, возврат
Примечание: переход на «release-1.8.1».

Вы сейчас в состоянии «отделённого HEAD». Вы можете осмотреться, сделать
экспериментальные изменения и закоммитить их, также вы можете отменить
изменения любых коммитов в этом состоянии не затрагивая любые ветки и
не переходя на них.

Если вы хотите создать новую ветку и сохранить свои коммиты, то вы
можете сделать это (сейчас или позже) вызвав команду checkout снова,
но с параметром -b. Например:

  git checkout -b <имя-новой-ветки>

HEAD сейчас на 2fe3bd99 Merge pull request #1433 from dsacre/fix-clang-warnings
$ git add third-party/gtest               # Фиксация изменений
$ git commit -m"added gtest framework"    # Комментарий к зафиксированным изменениям
[master 5ec7bb7] added gtest framework
 Committer: darthbarada <darthbarada@localhost.localdomain>
Ваше имя или электронная почта настроены автоматически на основании вашего
имени пользователя и имени машины. Пожалуйста, проверьте, что они 
определены правильно.
Вы можете отключить это уведомление установив их напрямую. Запустите следующую
команду и следуйте инструкциям вашего текстового редактора, для
редактирования вашего файла конфигурации:

    git config --global --edit

После этого, изменить авторство этой коммита можно будет с помощью команды:

    git commit --amend --reset-author

 2 files changed, 4 insertions(+)
 create mode 100644 .gitmodules
 create mode 160000 third-party/gtest
```

```ShellSession
$ gsed -i '/option(BUILD_EXAMPLES "Build examples" OFF)/a\   # Вставить вторую строку после указанной первой строки
option(BUILD_TESTS "Build tests" OFF)
' CMakeLists.txt
$ cat >> CMakeLists.txt <<EOF     # Дописывание в CMakeLists.txt указанного кода

if(BUILD_TESTS)
  enable_testing()
  add_subdirectory(third-party/gtest)
  file(GLOB \${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
  add_executable(check \${\${PROJECT_NAME}_TEST_SOURCES})
  target_link_libraries(check \${PROJECT_NAME} gtest_main)
  add_test(NAME check COMMAND check)
endif()
EOF
```

```ShellSession
$ mkdir tests  # Создание указанной папки tests
# Создание test1.cpp с кодом
$ cat > tests/test1.cpp <<EOF
#include <print.hpp>

#include <gtest/gtest.h>

TEST(Print, InFileStream)
{
  std::string filepath = "file.txt";
  std::string text = "hello";
  std::ofstream out{filepath};

  print(text, out);
  out.close();

  std::string result;
  std::ifstream in{filepath};
  in >> result;

  EXPECT_EQ(result, text);
}
EOF
```

```ShellSession
$ cmake -H. -B_build -DBUILD_TESTS=ON        # Конфигурирование
. -B_build -DBUILD_TESTS=ON
-- The C compiler identification is GNU 8.3.0
-- The CXX compiler identification is GNU 8.3.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Found PythonInterp: /usr/bin/python (found version "2.7.16") 
-- Looking for pthread.h
-- Looking for pthread.h - found
-- Looking for pthread_create
-- Looking for pthread_create - not found
-- Check if compiler accepts -pthread
-- Check if compiler accepts -pthread - yes
-- Found Threads: TRUE  
-- Configuring done
-- Generating done
-- Build files have been written to: /home/darthbarada/DarthBarada/workspace/projects/projects/lab05/_build

$ cmake --build _build                 # Компиляция
Scanning dependencies of target print
[  8%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 16%] Linking CXX static library libprint.a
[ 16%] Built target print
Scanning dependencies of target gtest
[ 25%] Building CXX object third-party/gtest/googlemock/gtest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 33%] Linking CXX static library libgtest.a
[ 33%] Built target gtest
Scanning dependencies of target gtest_main
[ 41%] Building CXX object third-party/gtest/googlemock/gtest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 50%] Linking CXX static library libgtest_main.a
[ 50%] Built target gtest_main
Scanning dependencies of target check
[ 58%] Building CXX object CMakeFiles/check.dir/tests/test1.cpp.o
[ 66%] Linking CXX executable check
[ 66%] Built target check
Scanning dependencies of target gmock
[ 75%] Building CXX object third-party/gtest/googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 83%] Linking CXX static library libgmock.a
[ 83%] Built target gmock
Scanning dependencies of target gmock_main
[ 91%] Building CXX object third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library libgmock_main.a
[100%] Built target gmock_main
$ cmake --build _build --target test      # Компиляция указанной цели
Running tests...
Test project /home/darthbarada/DarthBarada/workspace/projects/projects/lab05/_build
    Start 1: check
1/1 Test #1: check ............................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.02 sec
```

```ShellSession
$ _build/check                      # Выполнение исполняемого файла с тестами
Running main() from /home/darthbarada/DarthBarada/workspace/projects/projects/lab05/third-party/gtest/googletest/src/gtest_main.cc
[==========] Running 1 test from 1 test case.
[----------] Global test environment set-up.
[----------] 1 test from Print
[ RUN      ] Print.InFileStream
[       OK ] Print.InFileStream (0 ms)
[----------] 1 test from Print (0 ms total)

[----------] Global test environment tear-down
[==========] 1 test from 1 test case ran. (0 ms total)
[  PASSED  ] 1 test.
$ cmake --build _build --target test -- ARGS=--verbose        # Компиляция с выводом всей информации
Running tests...
UpdateCTestConfiguration  from :/home/darthbarada/DarthBarada/workspace/projects/projects/lab05/_build/DartConfiguration.tcl
UpdateCTestConfiguration  from :/home/darthbarada/DarthBarada/workspace/projects/projects/lab05/_build/DartConfiguration.tcl
Test project /home/darthbarada/DarthBarada/workspace/projects/projects/lab05/_build
Constructing a list of tests
Done constructing a list of tests
Updating test list for fixtures
Added 0 tests to meet fixture requirements
Checking test dependency graph...
Checking test dependency graph end
test 1
    Start 1: check

1: Test command: /home/darthbarada/DarthBarada/workspace/projects/projects/lab05/_build/check
1: Test timeout computed to be: 10000000
1: Running main() from /home/darthbarada/DarthBarada/workspace/projects/projects/lab05/third-party/gtest/googletest/src/gtest_main.cc
1: [==========] Running 1 test from 1 test case.
1: [----------] Global test environment set-up.
1: [----------] 1 test from Print
1: [ RUN      ] Print.InFileStream
1: [       OK ] Print.InFileStream (0 ms)
1: [----------] 1 test from Print (0 ms total)
1: 
1: [----------] Global test environment tear-down
1: [==========] 1 test from 1 test case ran. (0 ms total)
1: [  PASSED  ] 1 test.
1/1 Test #1: check ............................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.01 sec

```

```ShellSession
$ gsed -i 's/lab04/lab05/g' README.md           # Замена левой строки на правую
$ gsed -i 's/\(DCMAKE_INSTALL_PREFIX=_install\)/\1 -DBUILD_TESTS=ON/' .travis.yml 
$ gsed -i '/cmake --build _build --target install/a\      # Дописывание правой строки после найденной левой строки
- cmake --build _build --target test -- ARGS=--verbose
' .travis.yml
```

```ShellSession
$ travis lint      # Проверка .travis.yml
Warnings for .travis.yml:
[x] value for addons section is empty, dropping
[x] in addons section: unexpected key apt, dropping
```

```ShellSession
$ git add .travis.yml     # Фиксация указанного файла
$ git add tests           # Фиксация указанного файла
$ git add -p               # Фиксация указанного файла.
diff --git a/CMakeLists.txt b/CMakeLists.txt
index 05cc72b..176b7ba 100644
--- a/CMakeLists.txt
+++ b/CMakeLists.txt
@@ -4,6 +4,7 @@ set(CMAKE_CXX_STANDARD 11)
 set(CMAKE_CXX_STANDARD_REQUIRED ON)
 
 option(BUILD_EXAMPLES "Build examples" OFF)
+option(BUILD_TESTS "Build tests" OFF)
 
 project(print)
 
Stage this hunk [y,n,q,a,d,j,J,g,/,e,?]? y
@@ -34,3 +35,12 @@ install(TARGETS print
 
 install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
 install(EXPORT print-config DESTINATION cmake)
+
+if(BUILD_TESTS)
+  enable_testing()
+  add_subdirectory(third-party/gtest)
+  file(GLOB ${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
+  add_executable(check ${${PROJECT_NAME}_TEST_SOURCES})
+  target_link_libraries(check ${PROJECT_NAME} gtest_main)
+  add_test(NAME check COMMAND check)
+endif()

@@ -34,3 +35,12 @@ install(TARGETS print
 
 install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
 install(EXPORT print-config DESTINATION cmake)
+
+if(BUILD_TESTS)
+  enable_testing()
+  add_subdirectory(third-party/gtest)
+  file(GLOB ${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
+  add_executable(check ${${PROJECT_NAME}_TEST_SOURCES})
+  target_link_libraries(check ${PROJECT_NAME} gtest_main)
+  add_test(NAME check COMMAND check)
+endif()
$ git add .   
$ git commit -m"added tests"  # Комментарий изменений
[master ef161e8] added tests
 4 files changed, 35 insertions(+), 5 deletions(-)
 create mode 100644 tests/test1.cpp

$ git push origin master  # Отправка изменений в удаленный репозиторий
Username for 'https://github.com': DarthBarada
Password for 'https://DarthBarada@github.com': 
Перечисление объектов: 3029, готово.
Подсчет объектов: 100% (3029/3029), готово.
При сжатии изменений используется до 4 потоков
Сжатие объектов: 100% (2312/2312), готово.
Запись объектов: 100% (3029/3029), 13.49 MiB | 726.00 KiB/s, готово.
Всего 3029 (изменения 534), повторно использовано 3014 (изменения 529)
remote: Resolving deltas: 100% (534/534), done.
To https://github.com/DarthBarada/lab05
 * [new branch]      master -> master
```

```ShellSession
$ travis login --auto  # Не запустилось, потому что раньше я привязал свой travis к аккаунту c github
$ travis enable # Включение непрерывной интеграции для репозитория
DarthBarada/lab05: enabled :)
```

```ShellSession
$ mkdir artifacts    # Создание директории artifacts
$ sleep 20s && gnome-screenshot --file artifacts/screenshot.png # команда ждёт 20 сек, потом делает снимок с экрана в папку с именем creenshot.png
# for macOS: $ screencapture -T 20 artifacts/screenshot.png
# open https://github.com/${GITHUB_USERNAME}/lab05
```

## Report

```ShellSession
$ popd
$ export LAB_NUMBER=05
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Homework

### Задание
1. Создайте `CMakeList.txt` для библиотеки *banking*.
2. Создайте модульные тесты на классы `Transaction` и `Account`.
    * Используйте mock-объекты.
    * Покрытие кода должно составлять 100%.
3. Настройте сборочную процедуру на **TravisCI**.
4. Настройте [Coveralls.io](https://coveralls.io/).

## Links

- [C++ CI: Travis, CMake, GTest, Coveralls & Appveyor](http://david-grs.github.io/cpp-clang-travis-cmake-gtest-coveralls-appveyor/)
- [Boost.Tests](http://www.boost.org/doc/libs/1_63_0/libs/test/doc/html/)
- [Catch](https://github.com/catchorg/Catch2)

```
Copyright (c) 2015-2019 The ISC Authors
```
