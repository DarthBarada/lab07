[![Build Status](https://travis-ci.org/DarthBarada/lab07.svg?branch=master)](https://travis-ci.com/DarthBarada/lab07)
## Laboratory work VII

Данная лабораторная работа посвещена изучению систем управления пакетами на примере **Hunter**

```ShellSession
$ open https://github.com/ruslo/hunter
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab07** на сервисе **GitHub**
- [X] 2. Выполнить инструкцию учебного материала
- [X] 3. Ознакомиться со ссылками учебного материала
- [X] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=DarthBarada # Устанавливаем значение переменной окружения GITHUB_USERNAME
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace   # Вход в папку workspace 
$ pushd .                           # Сохранение указанной папки
$ source scripts/activate           # Выполнение скрипта activate
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab06 projects/lab07 # Копируем файлы lab06 из удаленного репозитория github в папку projects/lab07 
Клонирование в «projects/lab07»…
remote: Enumerating objects: 71, done.
remote: Counting objects: 100% (71/71), done.
remote: Compressing objects: 100% (39/39), done.
remote: Total 71 (delta 29), reused 63 (delta 25), pack-reused 0
Распаковка объектов: 100% (71/71), готово.
$ cd projects/lab07                                                    # Переход в папку lab07    
$ git remote remove origin                                             # Удаление ссылки на старый удаленный репозиторий
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab07    # Добавление ссылки на новый удаленный репозиторий
```

```ShellSession
$ wget https://github.com/hunter-packages/gate/archive/v0.9.0.tar.gz -O /tmp/gate.tar.gz      # Получение архива с удалённого репозитория и схранение его в папку tmp под именем gate.tar.gz  
--2019-06-05 00:08:42--  https://github.com/hunter-packages/gate/archive/v0.9.0.tar.gz
Распознаётся github.com (github.com)… 140.82.118.4
Подключение к github.com (github.com)|140.82.118.4|:443... соединение установлено.
HTTP-запрос отправлен. Ожидание ответа… 302 Found
Адрес: https://codeload.github.com/hunter-packages/gate/tar.gz/v0.9.0 [переход]
--2019-06-05 00:08:45--  https://codeload.github.com/hunter-packages/gate/tar.gz/v0.9.0
Распознаётся codeload.github.com (codeload.github.com)… 192.30.253.120
Подключение к codeload.github.com (codeload.github.com)|192.30.253.120|:443... соединение установлено.
HTTP-запрос отправлен. Ожидание ответа… 200 OK
Длина: нет данных [application/x-gzip]
Сохранение в: «/tmp/gate.tar.gz»

/tmp/gate.tar.gz        [   <=>              ] 328,41K   611KB/s    за 0,5s    

2019-06-05 00:08:47 (611 KB/s) - «/tmp/gate.tar.gz» сохранён [336289]

$ tar -xf /tmp/gate.tar.gz                            # Распаковка gate.tar.gz
$ mkdir -p cmake                                      # Создание папки cmake
$ mv gate-0.9.0/cmake/HunterGate.cmake cmake          # Перемещение HunterGate.cmake из распакованного архива (gate-0.9.0) в созданную папку cmake
$ rm -rf gate-0.9.0                                   # Удаление gate-0.9.0   
$ gsed -i '/cmake_minimum_required(VERSION 3.4)/a\    # Редактирование CMakeLists.txt
```
### Редактирование CMakeLists.txt
```ShellSession
include("cmake/HunterGate.cmake")
huntergate(
  URL "https://github.com/ruslo/hunter/archive/v0.23.83.tar.gz"
  SHA1 "12dec078717539eb7b03e6d2a17797cba9be9ba9"
)
' CMakeLists.txt
```

```ShellSession
$ git rm -rf third-party/gtest                                      # Удаление папки gtest в папке third-party
rm 'third-party/gtest'
$ gsed -i '/set(PRINT_VERSION_STRING "v\${PRINT_VERSION}")/a\       # Редактирование CMakeLists.txt (установка зависимостей)

hunter_add_package(GTest)
find_package(GTest CONFIG REQUIRED)
' CMakeLists.txt
$ gsed -i 's/add_subdirectory(third-party/gtest)//' CMakeLists.txt
$ gsed -i 's/gtest_main/GTest::main/' CMakeLists.txt
```
---

```ShellSession
$ cmake -H. -B_builds -DBUILD_TESTS=ON            # Конфигурирование
-- [hunter] Initializing Hunter workspace (12dec078717539eb7b03e6d2a17797cba9be9ba9)
-- [hunter]   https://github.com/ruslo/hunter/archive/v0.23.83.tar.gz
-- [hunter]   -> /home/darthbarada/.hunter/_Base/Download/Hunter/0.23.83/12dec07
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
-- [hunter] Calculating Toolchain-SHA1
-- [hunter] Calculating Config-SHA1
-- [hunter] HUNTER_ROOT: /home/darthbarada/.hunter
-- [hunter] [ Hunter-ID: 12dec07 | Toolchain-ID: bb25d6f | Config-ID: 510d5e8 ]
-- [hunter] GTEST_ROOT: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Install (ver.: 1.8.0-hunter-p11)
-- [hunter] Building GTest
loading initial cache file /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/cache.cmake
loading initial cache file /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/args.cmake
-- The C compiler identification is GNU 8.3.0
-- The CXX compiler identification is GNU 8.3.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Build
Scanning dependencies of target GTest-Release
[  6%] Creating directories for 'GTest-Release'
[ 12%] Performing download step (download, verify and extract) for 'GTest-Release'
-- Downloading...
   dst='/home/darthbarada/.hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
   timeout='none'
-- Using src='https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p11.tar.gz'
-- verifying file...
       file='/home/darthbarada/.hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
-- Downloading... done
-- extracting...
     src='/home/darthbarada/.hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
     dst='/home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 18%] No patch step for 'GTest-Release'
[ 25%] No update step for 'GTest-Release'
[ 31%] Performing configure step for 'GTest-Release'
loading initial cache file /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/cache.cmake
loading initial cache file /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/args.cmake
-- The C compiler identification is GNU 8.3.0
-- The CXX compiler identification is GNU 8.3.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
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
-- Build files have been written to: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Build/GTest-Release-prefix/src/GTest-Release-build
[ 37%] Performing build step for 'GTest-Release'
Scanning dependencies of target gtest
[  9%] Building CXX object googlemock/gtest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
Scanning dependencies of target gmock
[ 27%] Building CXX object googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 27%] Building CXX object googlemock/CMakeFiles/gmock.dir/__/googletest/src/gtest-all.cc.o
Scanning dependencies of target gmock_main
[ 36%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/__/googletest/src/gtest-all.cc.o
[ 45%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock-all.cc.o
[ 54%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[ 63%] Linking CXX static library libgtest.a
[ 72%] Linking CXX static library libgmock.a
[ 72%] Built target gtest
[ 72%] Built target gmock
Scanning dependencies of target gtest_main
[ 81%] Building CXX object googlemock/gtest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 90%] Linking CXX static library libgtest_main.a
[ 90%] Built target gtest_main
[100%] Linking CXX static library libgmock_main.a
[100%] Built target gmock_main
[ 43%] Performing install step for 'GTest-Release'
[ 36%] Built target gmock_main
[ 63%] Built target gmock
[ 81%] Built target gtest
[100%] Built target gtest_main
Install the project...
-- Install configuration: "Release"
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/libgmock.a
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/libgmock_main.a
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/internal
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/internal/gmock-internal-utils.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/internal/gmock-port.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/internal/gmock-generated-internal-utils.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/internal/custom
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/internal/custom/gmock-port.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/internal/custom/gmock-matchers.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/internal/custom/gmock-generated-actions.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-generated-matchers.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-cardinalities.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-actions.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-matchers.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-generated-actions.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-more-actions.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-spec-builders.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-generated-nice-strict.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-more-matchers.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-generated-function-mockers.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/cmake/GMock/GMockConfig.cmake
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/cmake/GMock/GMockConfigVersion.cmake
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/cmake/GMock/GMockTargets.cmake
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/cmake/GMock/GMockTargets-release.cmake
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/libgtest.a
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/libgtest_main.a
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-filepath.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-param-util.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-internal.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-linked_ptr.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-string.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-tuple.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-type-util.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-port-arch.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/custom
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/custom/gtest-printers.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/custom/gtest-port.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/custom/gtest.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-death-test-internal.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-param-util-generated.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-port.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest_pred_impl.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest-death-test.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest-typed-test.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest-test-part.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest-param-test.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest-spi.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest-printers.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest_prod.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest-message.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/cmake/GTest/GTestConfig.cmake
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/cmake/GTest/GTestConfigVersion.cmake
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/cmake/GTest/GTestTargets.cmake
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/cmake/GTest/GTestTargets-release.cmake
loading initial cache file /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/args.cmake
[ 50%] Completed 'GTest-Release'
[ 50%] Built target GTest-Release
Scanning dependencies of target GTest-Debug
[ 56%] Creating directories for 'GTest-Debug'
[ 62%] Performing download step (download, verify and extract) for 'GTest-Debug'
-- verifying file...
       file='/home/darthbarada/.hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
-- File already exists and hash match (skip download):
  file='/home/darthbarada/.hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
  SHA1='76c6aec038f7d7258bf5c4f45c4817b34039d285'
-- extracting...
     src='/home/darthbarada/.hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
     dst='/home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 68%] No patch step for 'GTest-Debug'
[ 75%] No update step for 'GTest-Debug'
[ 81%] Performing configure step for 'GTest-Debug'
loading initial cache file /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/cache.cmake
loading initial cache file /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/args.cmake
-- The C compiler identification is GNU 8.3.0
-- The CXX compiler identification is GNU 8.3.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
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
-- Build files have been written to: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Build/GTest-Debug-prefix/src/GTest-Debug-build
[ 87%] Performing build step for 'GTest-Debug'
Scanning dependencies of target gtest
[  9%] Building CXX object googlemock/gtest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
Scanning dependencies of target gmock
Scanning dependencies of target gmock_main
[ 27%] Building CXX object googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 27%] Building CXX object googlemock/CMakeFiles/gmock.dir/__/googletest/src/gtest-all.cc.o
[ 36%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/__/googletest/src/gtest-all.cc.o
[ 45%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock-all.cc.o
[ 54%] Linking CXX static library libgmockd.a
[ 54%] Built target gmock
[ 63%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[ 72%] Linking CXX static library libgtestd.a
[ 72%] Built target gtest
Scanning dependencies of target gtest_main
[ 81%] Building CXX object googlemock/gtest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 90%] Linking CXX static library libgmock_maind.a
[100%] Linking CXX static library libgtest_maind.a
[100%] Built target gtest_main
[100%] Built target gmock_main
[ 93%] Performing install step for 'GTest-Debug'
[ 36%] Built target gmock_main
[ 63%] Built target gmock
[ 81%] Built target gtest
[100%] Built target gtest_main
Install the project...
-- Install configuration: "Debug"
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/libgmockd.a
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/libgmock_maind.a
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/internal
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/internal/gmock-internal-utils.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/internal/gmock-port.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/internal/gmock-generated-internal-utils.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/internal/custom
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/internal/custom/gmock-port.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/internal/custom/gmock-matchers.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/internal/custom/gmock-generated-actions.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-generated-matchers.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-cardinalities.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-actions.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-matchers.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-generated-actions.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-more-actions.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-spec-builders.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-generated-nice-strict.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-more-matchers.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gmock/gmock-generated-function-mockers.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/cmake/GMock/GMockConfig.cmake
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/cmake/GMock/GMockConfigVersion.cmake
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/cmake/GMock/GMockTargets.cmake
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/cmake/GMock/GMockTargets-debug.cmake
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/libgtestd.a
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/libgtest_maind.a
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-filepath.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-param-util.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-internal.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-linked_ptr.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-string.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-tuple.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-type-util.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-port-arch.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/custom
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/custom/gtest-printers.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/custom/gtest-port.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/custom/gtest.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-death-test-internal.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-param-util-generated.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-port.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest_pred_impl.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest-death-test.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest-typed-test.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest-test-part.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest-param-test.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest-spi.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest-printers.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest_prod.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest-message.h
-- Up-to-date: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/include/gtest/gtest.h
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/cmake/GTest/GTestConfig.cmake
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/cmake/GTest/GTestConfigVersion.cmake
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/cmake/GTest/GTestTargets.cmake
-- Installing: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/Install/lib/cmake/GTest/GTestTargets-debug.cmake
loading initial cache file /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest/args.cmake
[100%] Completed 'GTest-Debug'
[100%] Built target GTest-Debug
-- [hunter] Build step successful (dir: /home/darthbarada/.hunter/_Base/12dec07/bb25d6f/510d5e8/Build/GTest)
-- [hunter] Cache saved: /home/darthbarada/.hunter/_Base/Cache/raw/73e3025e35363108ca674a7731d1ffe8e4305aab.tar.bz2
-- Configuring done
-- Generating done
-- Build files have been written to: /home/darthbarada/DarthBarada/workspace/projects/lab07/_builds
$ cmake --build _builds         # Компиляция
Scanning dependencies of target print
[ 25%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 50%] Linking CXX static library libprint.a
[ 50%] Built target print
Scanning dependencies of target check
[ 75%] Building CXX object CMakeFiles/check.dir/tests/test1.cpp.o
[100%] Linking CXX executable check
[100%] Built target check
$ cmake --build _builds --target test   # Компиляция указанной цели test
Running tests...
Test project /home/darthbarada/DarthBarada/workspace/projects/lab07/_builds
    Start 1: check
1/1 Test #1: check ............................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.01 sec
$ ls -la $HOME/.hunter                  # Вывод информации о изменениях в папке .hunter     
итого 12
drwxr-xr-x  3 darthbarada darthbarada 4096 июн  5 00:27 .
drwxr-xr-x 30 darthbarada darthbarada 4096 июн  5 00:27 ..
drwxr-xr-x  6 darthbarada darthbarada 4096 июн  5 00:28 _Base
```

```ShellSession
$ git clone https://github.com/ruslo/hunter $HOME/projects/hunter # Копирование из удалённого репозитроия папки hunter в папку /projects/hunter 
$ export HUNTER_ROOT=$HOME/projects/hunter                        # Создание переменной окружения HUNTER_ROOT c инофрмацией о пути к папке hunter
$ rm -rf _build                                                   # Удаление папки _build
$ cmake -H. -B_builds -DBUILD_TESTS=ON                            # Конфигурирование
-- [hunter] Initializing Hunter workspace (12dec078717539eb7b03e6d2a17797cba9be9ba9)
-- [hunter]   https://github.com/ruslo/hunter/archive/v0.23.83.tar.gz
-- [hunter]   -> /home/darthbarada/projects/hunter/_Base/Download/Hunter/0.23.83/12dec07
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
-- [hunter] Calculating Toolchain-SHA1
-- [hunter] Calculating Config-SHA1
-- [hunter] HUNTER_ROOT: /home/darthbarada/projects/hunter
-- [hunter] [ Hunter-ID: 12dec07 | Toolchain-ID: bb25d6f | Config-ID: b37bb55 ]
-- [hunter] GTEST_ROOT: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Install (ver.: 1.8.0-hunter-p11)
-- [hunter] Building GTest
loading initial cache file /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/cache.cmake
loading initial cache file /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/args.cmake
-- The C compiler identification is GNU 8.3.0
-- The CXX compiler identification is GNU 8.3.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Build
Scanning dependencies of target GTest-Release
[  6%] Creating directories for 'GTest-Release'
[ 12%] Performing download step (download, verify and extract) for 'GTest-Release'
-- Downloading...
   dst='/home/darthbarada/projects/hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
   timeout='none'
-- Using src='https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p11.tar.gz'
-- verifying file...
       file='/home/darthbarada/projects/hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
-- Downloading... done
-- extracting...
     src='/home/darthbarada/projects/hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
     dst='/home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 18%] No patch step for 'GTest-Release'
[ 25%] No update step for 'GTest-Release'
[ 31%] Performing configure step for 'GTest-Release'
loading initial cache file /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/cache.cmake
loading initial cache file /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/args.cmake
-- The C compiler identification is GNU 8.3.0
-- The CXX compiler identification is GNU 8.3.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
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
-- Build files have been written to: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Build/GTest-Release-prefix/src/GTest-Release-build
[ 37%] Performing build step for 'GTest-Release'
Scanning dependencies of target gtest
Scanning dependencies of target gmock_main
Scanning dependencies of target gmock
[  9%] Building CXX object googlemock/gtest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 18%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/__/googletest/src/gtest-all.cc.o
[ 27%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock-all.cc.o
[ 36%] Building CXX object googlemock/CMakeFiles/gmock.dir/__/googletest/src/gtest-all.cc.o
[ 45%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[ 54%] Building CXX object googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 63%] Linking CXX static library libgtest.a
[ 63%] Built target gtest
Scanning dependencies of target gtest_main
[ 72%] Building CXX object googlemock/gtest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 81%] Linking CXX static library libgmock_main.a
[ 81%] Built target gmock_main
[ 90%] Linking CXX static library libgtest_main.a
[ 90%] Built target gtest_main
[100%] Linking CXX static library libgmock.a
[100%] Built target gmock
[ 43%] Performing install step for 'GTest-Release'
[ 36%] Built target gmock_main
[ 63%] Built target gmock
[ 81%] Built target gtest
[100%] Built target gtest_main
Install the project...
-- Install configuration: "Release"
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/libgmock.a
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/libgmock_main.a
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/internal
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/internal/gmock-internal-utils.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/internal/gmock-port.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/internal/gmock-generated-internal-utils.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/internal/custom
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/internal/custom/gmock-port.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/internal/custom/gmock-matchers.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/internal/custom/gmock-generated-actions.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-generated-matchers.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-cardinalities.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-actions.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-matchers.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-generated-actions.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-more-actions.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-spec-builders.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-generated-nice-strict.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-more-matchers.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-generated-function-mockers.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/cmake/GMock/GMockConfig.cmake
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/cmake/GMock/GMockConfigVersion.cmake
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/cmake/GMock/GMockTargets.cmake
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/cmake/GMock/GMockTargets-release.cmake
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/libgtest.a
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/libgtest_main.a
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-filepath.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-param-util.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-internal.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-linked_ptr.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-string.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-tuple.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-type-util.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-port-arch.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/custom
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/custom/gtest-printers.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/custom/gtest-port.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/custom/gtest.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-death-test-internal.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-param-util-generated.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-port.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest_pred_impl.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest-death-test.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest-typed-test.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest-test-part.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest-param-test.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest-spi.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest-printers.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest_prod.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest-message.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/cmake/GTest/GTestConfig.cmake
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/cmake/GTest/GTestConfigVersion.cmake
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/cmake/GTest/GTestTargets.cmake
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/cmake/GTest/GTestTargets-release.cmake
loading initial cache file /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/args.cmake
[ 50%] Completed 'GTest-Release'
[ 50%] Built target GTest-Release
Scanning dependencies of target GTest-Debug
[ 56%] Creating directories for 'GTest-Debug'
[ 62%] Performing download step (download, verify and extract) for 'GTest-Debug'
-- verifying file...
       file='/home/darthbarada/projects/hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
-- File already exists and hash match (skip download):
  file='/home/darthbarada/projects/hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
  SHA1='76c6aec038f7d7258bf5c4f45c4817b34039d285'
-- extracting...
     src='/home/darthbarada/projects/hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
     dst='/home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 68%] No patch step for 'GTest-Debug'
[ 75%] No update step for 'GTest-Debug'
[ 81%] Performing configure step for 'GTest-Debug'
loading initial cache file /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/cache.cmake
loading initial cache file /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/args.cmake
-- The C compiler identification is GNU 8.3.0
-- The CXX compiler identification is GNU 8.3.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
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
-- Build files have been written to: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Build/GTest-Debug-prefix/src/GTest-Debug-build
[ 87%] Performing build step for 'GTest-Debug'
Scanning dependencies of target gtest
[  9%] Building CXX object googlemock/gtest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
Scanning dependencies of target gmock_main
[ 18%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/__/googletest/src/gtest-all.cc.o
Scanning dependencies of target gmock
[ 27%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock-all.cc.o
[ 36%] Building CXX object googlemock/CMakeFiles/gmock.dir/__/googletest/src/gtest-all.cc.o
[ 45%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[ 54%] Building CXX object googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 63%] Linking CXX static library libgmock_maind.a
[ 72%] Linking CXX static library libgtestd.a
[ 72%] Built target gmock_main
[ 72%] Built target gtest
Scanning dependencies of target gtest_main
[ 81%] Building CXX object googlemock/gtest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 90%] Linking CXX static library libgtest_maind.a
[ 90%] Built target gtest_main
[100%] Linking CXX static library libgmockd.a
[100%] Built target gmock
[ 93%] Performing install step for 'GTest-Debug'
[ 36%] Built target gmock_main
[ 63%] Built target gmock
[ 81%] Built target gtest
[100%] Built target gtest_main
Install the project...
-- Install configuration: "Debug"
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/libgmockd.a
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/libgmock_maind.a
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/internal
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/internal/gmock-internal-utils.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/internal/gmock-port.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/internal/gmock-generated-internal-utils.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/internal/custom
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/internal/custom/gmock-port.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/internal/custom/gmock-matchers.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/internal/custom/gmock-generated-actions.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-generated-matchers.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-cardinalities.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-actions.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-matchers.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-generated-actions.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-more-actions.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-spec-builders.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-generated-nice-strict.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-more-matchers.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gmock/gmock-generated-function-mockers.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/cmake/GMock/GMockConfig.cmake
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/cmake/GMock/GMockConfigVersion.cmake
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/cmake/GMock/GMockTargets.cmake
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/cmake/GMock/GMockTargets-debug.cmake
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/libgtestd.a
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/libgtest_maind.a
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-filepath.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-param-util.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-internal.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-linked_ptr.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-string.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-tuple.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-type-util.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-port-arch.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/custom
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/custom/gtest-printers.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/custom/gtest-port.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/custom/gtest.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-death-test-internal.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-param-util-generated.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/internal/gtest-port.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest_pred_impl.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest-death-test.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest-typed-test.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest-test-part.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest-param-test.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest-spi.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest-printers.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest_prod.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest-message.h
-- Up-to-date: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/include/gtest/gtest.h
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/cmake/GTest/GTestConfig.cmake
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/cmake/GTest/GTestConfigVersion.cmake
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/cmake/GTest/GTestTargets.cmake
-- Installing: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/Install/lib/cmake/GTest/GTestTargets-debug.cmake
loading initial cache file /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest/args.cmake
[100%] Completed 'GTest-Debug'
[100%] Built target GTest-Debug
-- [hunter] Build step successful (dir: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Build/GTest)
-- [hunter] Cache saved: /home/darthbarada/projects/hunter/_Base/Cache/raw/b4bf51893ab936c42ed61c54489b8efeba60b3fa.tar.bz2
-- Configuring done
-- Generating done
-- Build files have been written to: /home/darthbarada/DarthBarada/workspace/projects/lab07/_builds
$ cmake --build _builds                                           # Компиляция
Scanning dependencies of target print
[ 25%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 50%] Linking CXX static library libprint.a
[ 50%] Built target print
Scanning dependencies of target check
[ 75%] Building CXX object CMakeFiles/check.dir/tests/test1.cpp.o
[100%] Linking CXX executable check
[100%] Built target check
$ cmake --build _builds --target test       # Компиляция цели test
Running tests...
Test project /home/darthbarada/DarthBarada/workspace/projects/lab07/_builds
    Start 1: check
1/1 Test #1: check ............................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.01 sec
```

````ShellSession
$ cat $HUNTER_ROOT/cmake/configs/default.cmake | grep GTest     # Выводит содержимое default.cmake
  hunter_default_version(GTest VERSION 1.7.0-hunter-6)
  hunter_default_version(GTest VERSION 1.8.0-hunter-p11)
$ cat $HUNTER_ROOT/cmake/projects/GTest/hunter.cmake            # Выводит содержимое hunter.cmake
# Copyright (c) 2013, Ruslan Baratov
# All rights reserved.

# !!! DO NOT PLACE HEADER GUARDS HERE !!!

include(hunter_add_version)
include(hunter_cacheable)
include(hunter_download)
include(hunter_pick_scheme)
include(hunter_cmake_args)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter.tar.gz"
    SHA1
    1ed1c26d11fb592056c1cb912bd3c784afa96eaa
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-1"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-1.tar.gz"
    SHA1
    0cb1dcf75e144ad052d3f1e4923a7773bf9b494f
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-2"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-2.tar.gz"
    SHA1
    e62b2ef70308f63c32c560f7b6e252442eed4d57
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-3"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-3.tar.gz"
    SHA1
    fea7d3020e20f059255484c69755753ccadf6362
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-4"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-4.tar.gz"
    SHA1
    9b439c0c25437a083957b197ac6905662a5d901b
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-5"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-5.tar.gz"
    SHA1
    796804df3facb074087a4d8ba6f652e5ac69ad7f
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-6"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-6.tar.gz"
    SHA1
    64b93147abe287da8fe4e18cfd54ba9297dafb82
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-7"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-7.tar.gz"
    SHA1
    19b5c98747768bcd0622714f2ed40f17aee406b2
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-8"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-8.tar.gz"
    SHA1
    ac4d2215aa1b1d745a096e5e3b2dbe0c0f229ea5
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-9"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-9.tar.gz"
    SHA1
    8a47fe9c4e550f4ed0e2c05388dd291a059223d9
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-10"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-10.tar.gz"
    SHA1
    374e6dbe8619ab467c6b1a0b470a598407b172e9
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-11"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-11.tar.gz"
    SHA1
    c6ae948ca2bea1d734af01b1069491b00933ed31
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p2
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p2.tar.gz"
    SHA1
    93148cb8850abe78b76ed87158fdb6b9c48e38c4
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p5
    URL https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p5.tar.gz
    SHA1 3325aa4fc8b30e665c9f73a60f19387b7db36f85
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p6
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p6.tar.gz"
    SHA1
    f57096bd01c6f8cbef043b312d4d1e82f29648b6
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p7
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p7.tar.gz"
    SHA1
    4fe083a96d7597f7dce6f453dca01e1d94a1e45b
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p8
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p8.tar.gz"
    SHA1
    1cdd396b20c8d29f7ea08baaa49673b1c261f545
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p9
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p9.tar.gz"
    SHA1
    a345f16cb610e0b5dfa7778dc2852b784cfede5b
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p10
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p10.tar.gz"
    SHA1
    1d92c9f51af756410843b13f8c4e4df09e235394
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.8.0-hunter-p11"
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p11.tar.gz"
    SHA1
    76c6aec038f7d7258bf5c4f45c4817b34039d285
)

if(HUNTER_GTest_VERSION VERSION_LESS 1.8.0)
  set(_gtest_license "LICENSE")
else()
  set(_gtest_license "googletest/LICENSE")
endif()

hunter_cmake_args(
    GTest
    CMAKE_ARGS
    HUNTER_INSTALL_LICENSE_FILES=${_gtest_license}
)

hunter_pick_scheme(DEFAULT url_sha1_cmake)
hunter_cacheable(GTest)
hunter_download(PACKAGE_NAME GTest PACKAGE_INTERNAL_DEPS_ID 1)
$ mkdir cmake/Hunter                            # Создание папки Hunter в папке cmake
$ cat > cmake/Hunter/config.cmake <<EOF         # Создание файла config.cmake и редактирование его
hunter_config(GTest VERSION 1.7.0-hunter-9)
EOF
```

```ShellSession
$ git submodule add github.com/ruslo/polly tools/polly  # Копирование подмодуля из github в папку tools/polly
Клонирование в «/home/darthbarada/DarthBarada/workspace/projects/lab07/tools/polly»…
remote: Enumerating objects: 31, done.
remote: Counting objects: 100% (31/31), done.
remote: Compressing objects: 100% (18/18), done.
remote: Total 6136 (delta 15), reused 19 (delta 13), pack-reused 6105
Получение объектов: 100% (6136/6136), 1.57 MiB | 2.86 MiB/s, готово.
Определение изменений: 100% (4196/4196), готово.
$ tools/polly/bin/polly.py --test           # Запуск теста
ython version: 3.7
Build dir: /home/darthbarada/DarthBarada/workspace/projects/lab07/_builds/default
Execute command: [
  `which`
  `cmake`
]

[/home/darthbarada/DarthBarada/workspace/projects/lab07]> "which" "cmake"

/usr/local/bin/cmake
Execute command: [
  `cmake`
  `--version`
]

[/home/darthbarada/DarthBarada/workspace/projects/lab07]> "cmake" "--version"

cmake version 3.14.2

CMake suite maintained and supported by Kitware (kitware.com/cmake).
Execute command: [
  `cmake`
  `-H.`
  `-B/home/darthbarada/DarthBarada/workspace/projects/lab07/_builds/default`
  `-DCMAKE_TOOLCHAIN_FILE=/home/darthbarada/DarthBarada/workspace/projects/lab07/tools/polly/default.cmake`
]

[/home/darthbarada/DarthBarada/workspace/projects/lab07]> "cmake" "-H." "-B/home/darthbarada/DarthBarada/workspace/projects/lab07/_builds/default" "-DCMAKE_TOOLCHAIN_FILE=/home/darthbarada/DarthBarada/workspace/projects/lab07/tools/polly/default.cmake"

-- [polly] Used toolchain: Default
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
-- [hunter] Calculating Toolchain-SHA1
-- [hunter] Calculating Config-SHA1
-- [hunter] HUNTER_ROOT: /home/darthbarada/projects/hunter
-- [hunter] [ Hunter-ID: 12dec07 | Toolchain-ID: bb25d6f | Config-ID: b37bb55 ]
-- [hunter] GTEST_ROOT: /home/darthbarada/projects/hunter/_Base/12dec07/bb25d6f/b37bb55/Install (ver.: 1.8.0-hunter-p11)
-- Configuring done
-- Generating done
-- Build files have been written to: /home/darthbarada/DarthBarada/workspace/projects/lab07/_builds/default
Execute command: [
  `cmake`
  `--build`
  `/home/darthbarada/DarthBarada/workspace/projects/lab07/_builds/default`
  `--`
]

[/home/darthbarada/DarthBarada/workspace/projects/lab07]> "cmake" "--build" "/home/darthbarada/DarthBarada/workspace/projects/lab07/_builds/default" "--"

Scanning dependencies of target print
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print
Run tests
Execute command: [
  `ctest`
]

[/home/darthbarada/DarthBarada/workspace/projects/lab07/_builds/default]> "ctest"

*********************************
No test configuration file found!
*********************************
Usage

  ctest [options]

-
Log saved: /home/darthbarada/DarthBarada/workspace/projects/lab07/_logs/polly/default/log.txt
-
Generate: 0:00:08.784073s
Build: 0:00:01.413524s
Test: 0:00:00.010986s
-
Total: 0:00:10.208887s
-
SUCCESS
```

## Report

```ShellSession
$ popd
$ export LAB_NUMBER=07
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Homework

### Задание
1. Создайте cвой hunter-пакет.

## Links

- [Create Hunter package](https://docs.hunter.sh/en/latest/creating-new/create.html)
- [Custom Hunter config](https://github.com/ruslo/hunter/wiki/example.custom.config.id)
- [Polly](https://github.com/ruslo/polly)

```
Copyright (c) 2015-2019 The ISC Authors
```
