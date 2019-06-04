## Laboratory work III

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

```ShellSession
$ open https://cmake.org/
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab03** на сервисе **GitHub**
- [X] 2. Ознакомиться со ссылками учебного материала
- [X] 3. Выполнить инструкцию учебного материала
- [X] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Введение

```ShellSession
$ export GITHUB_USERNAME=DarthBarada        # Усанавливаем параметры окружения
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .                                   # Сохраняет имя текущего каталога для команды и переходит в другой каталог. 
~/DarthBarada/workspace ~/DarthBarada/workspace
$ source scripts/activate                    # Активируем файл activate
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03 # Копируем репозиторий lab02 с сайта в папку projects/lab03
Cloning into 'projects/lab03'...
remote: Enumerating objects: 19, done.
remote: Counting objects: 100% (19/19), done.
remote: Compressing objects: 100% (14/14), done.
remote: Total 19 (delta 1), reused 13 (delta 0), pack-reused 0
Unpacking objects: 100% (19/19), done.
$ cd projects/lab03                                        # заходим в эту папку 
$ git remote remove origin                                 # Устанавливаем удаленный доступ к моему репозитрию
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git
```

```ShellSession 
# Включаем поддержку с++ 11 года в g++, создаем файл example1, связываем его с print.hpp и запускаем коды
$ g++ -std=c++11 -I./include -c sources/print.cpp
$ ls print.o    # Проверить наличие файла
print.o
$ nm print.o | grep print    # Вывести символы, содержащиеся в объектном файле
0000000000000095 t _GLOBAL__sub_I__Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
0000000000000000 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
0000000000000026 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
$ ar rvs print.a print.o    # Создать архив (статическую библиотеку) 
ar: creating print.a
a - print.o
$ file print.a  # Информация о файле print.a
print.a: current ar archive
$ g++ -std=c++11 -I./include -c examples/example1.cpp
$ ls example1.o
example1.o
$ g++ example1.o print.a -o example1   # Связывание файла и статической библиотеки,вывод в example1
$ ./example1 && echo   # Запустить полученный файл. И напечатать строку
hello
//---------------- Часть 2 --------------------
$ g++ -std=c++11 -I./include -c examples/example2.cpp   # Компиляция source/example2.cpp, используя заголовки из ./include и стандарт C++11
$ nm example2.o
 U __cxa_atexit
                 U __dso_handle
0000000000000000 V DW.ref.__gxx_personality_v0
                 U _GLOBAL_OFFSET_TABLE_
0000000000000163 t _GLOBAL__sub_I_main
                 U __gxx_personality_v0
0000000000000000 T main
                 U __stack_chk_fail
                 U _Unwind_Resume
000000000000011a t _Z41__static_initialization_and_destruction_0ii
                 U _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
                 U _ZNSaIcEC1Ev
                 U _ZNSaIcED1Ev
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEEC1EPKcSt13_Ios_Openmode
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEED1Ev
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEC1EPKcRKS3_
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEED1Ev
                 U _ZNSt8ios_base4InitC1Ev
                 U _ZNSt8ios_base4InitD1Ev
0000000000000000 r _ZStL19piecewise_construct
0000000000000000 b _ZStL8__ioinit
$ g++ example2.o print.a -o example2    # Связывание файла и статической библиотеки,вывод в example2
$ ./example2                            # Исполнение example2
$ cat log.txt && echo                   # выводим содержание log.txt
hello
```

```ShellSession
// Удалем ненужные файлы
$ rm -rf example1.o example2.o print.o
$ rm -rf print.a
$ rm -rf example1 example2
$ rm -rf log.txt
```

```ShellSession
// Настраиваем Cmake
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.4)
project(print)
EOF

$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF

$ cat >> CMakeLists.txt <<EOF
add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
EOF

$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
EOF
Компиляция
$ cmake -H. -B_build
-- The C compiler identification is GNU 8.2.1
-- The CXX compiler identification is GNU 8.2.1
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
-- Configuring done
-- Generating done
-- Build files have been written to: /home/starkiller/Documents/DarthBarada/workspace/projects/lab03/_build
$ cmake --build _build                            # Выполнение поставленных в CMakeLists.txt задач.
Scanning dependencies of target print
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print

$ cat >> CMakeLists.txt <<EOF

add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
EOF

$ cat >> CMakeLists.txt <<EOF

target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```

Билдим в Сmake наши файлы с кодом С++
```
$ cmake --build _build
-- Configuring done
-- Generating done
-- Build files have been written to: /home/starkiller/Documents/DarthBarada/workspace/projects/lab03/_build
[ 33%] Built target print
Scanning dependencies of target example2
[ 50%] Building CXX object CMakeFiles/example2.dir/examples/example2.cpp.o
[ 66%] Linking CXX executable example2
[ 66%] Built target example2
Scanning dependencies of target example1
[ 83%] Building CXX object CMakeFiles/example1.dir/examples/example1.cpp.o
[100%] Linking CXX executable example1
[100%] Built target example1
$ cmake --build _build --target print
[100%] Built target print
$ cmake --build _build --target example1
[ 50%] Built target print
[100%] Built target example1
$ cmake --build _build --target example2
[ 50%] Built target print
[100%] Built target example2
```
Выводим в cmd результаты работ наших кодов
```
$ ls -la _build/libprint.a    # Проверить наличие файла и вывести информацию о нем
-rw-r--r-- 1 darthbarada darthbarada 3118 апр 29 12:33 _build/libprint.a
$ _build/example1 && echo   # Выполнить example1 и вывести перенос строки
hello 
$ _build/example2           # Выполнить example2
$ cat log.txt && echo       # Напечатать log.txt в консоль и вывести перенос строки
hello
$ rm -rf log.txt            # Рекурсивно и без подтверждения удалить log.txt
```
Получение CMakeLists.txt из репозитория
```
$ git clone https://github.com/tp-labs/lab03 tmp    # Скачивание репозитория в директорию tmp
Cloning into 'tmp'...
remote: Enumerating objects: 24, done.
remote: Counting objects: 100% (24/24), done.
remote: Compressing objects: 100% (18/18), done.
remote: Total 67 (delta 4), reused 24 (delta 4), pack-reused 43
Unpacking objects: 100% (67/67), done.
$ mv -f tmp/CMakeLists.txt .   # Перемещение CMakeLists.txt в текущую директорию
$ rm -rf tmp                   # Удаление директории tmp

$ cat CMakeLists.txt  # Вывод CMakeLists.txt в консоль
cmake_minimum_required(VERSION 3.0)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_EXAMPLES "Build examples" OFF)

project(print)

add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)

target_include_directories(print PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

if(BUILD_EXAMPLES)
  file(GLOB EXAMPLE_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/examples/*.cpp")
  foreach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
    get_filename_component(EXAMPLE_NAME ${EXAMPLE_SOURCE} NAME_WE)
    add_executable(${EXAMPLE_NAME} ${EXAMPLE_SOURCE})
    target_link_libraries(${EXAMPLE_NAME} print)
    install(TARGETS ${EXAMPLE_NAME}
      RUNTIME DESTINATION bin
    )
  endforeach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
endif()

install(TARGETS print
    EXPORT print-config
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
install(EXPORT print-config DESTINATION cmake)
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
-- Configuring done
-- Generating done
-- Build files have been written to: /home/starkiller/Documents/DarthBarada/workspace/projects/lab03/_build
$ cmake --build _build --target install
-- Install configuration: ""
-- Installing: /home/starkiller/Documents/DarthBarada/workspace/projects/lab03/_install/lib/libprint.a
-- Installing: /home/starkiller/Documents/DarthBarada/workspace/projects/lab03/_install/include
-- Installing: /home/starkiller/Documents/DarthBarada/workspace/projects/lab03/_install/include/print.hpp
-- Installing: /home/starkiller/Documents/DarthBarada/workspace/projects/lab03/_install/cmake/print-config.cmake
-- Installing: /home/starkiller/Documents/DarthBarada/workspace/projects/lab03/_install/cmake/print-config-noconfig.cmake
$ tree _install                                         # Вывод дерева файлов
_install
├── cmake
│   ├── print-config.cmake
│   └── print-config-noconfig.cmake
├── include
│   └── print.hpp
└── lib
    └── libprint.a

3 directories, 4 files

$ git add CMakeLists.txt    # Фиксация файла CMakeLists.txt  
$ git commit -m"added CMakeLists.txt" # Добавляем коммит
$ git push origin master   # Отправка изменений
Username for 'https://github.com': DarthBarada
Password for 'https://DarthBarada@github.com': 
Enumerating objects: 22, done.
Counting objects: 100% (22/22), done.
Delta compression using up to 4 threads
Compressing objects: 100% (17/17), done.
Writing objects: 100% (22/22), 7.68 KiB | 5.41 MiB/s, done.
Total 22 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), done.
To https://github.com/DarthBarada/lab03.git
 * [new branch]      master -> master
```

## Отправляем отчет

```
$ popd
$ export LAB_NUMBER=03
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
# копирем репозиторий lab03 в tasks соответствующей папки 
$ mkdir reports/lab${LAB_NUMBER} 
#Создаем в отчетах соответсвующую папку лабы и добавляем в неё файл REPORT.md
#который является копией README.md из папки tasks соответствуюей лабы
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md // редактируем файл
$ gistup -m "lab${LAB_NUMBER}"
```

## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

**Удачной стажировки!**

## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2015-2019 The ISC Authors
```
