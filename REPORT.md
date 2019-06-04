[![Build Status](https://travis-ci.org/DarthBarada/lab06.svg?branch=master)](https://travis-ci.com/DarthBarada/lab06)
## Laboratory work VI

Данная лабораторная работа посвещена изучению средств пакетирования на примере **CPack**

```ShellSession
$ open https://cmake.org/Wiki/CMake:CPackPackageGenerators
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab06** на сервисе **GitHub**
- [X] 2. Выполнить инструкцию учебного материала
- [X] 3. Ознакомиться со ссылками учебного материала
- [X] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
# Устанавливаем параметры окружения 
$ export GITHUB_USERNAME=DarthBarada                  # Инициализация переменной GITHUB_USERNAME
$ export GITHUB_EMAIL=darthbarada@gmail.com
$ alias edit=nano                                     # Устанавливаем синоним команды edit
$ alias gsed=sed # for *-nix system                   # Устанавливаем синоним команды gsed
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace                     # Перемещение в папку workspace
$ pushd .                                             # Сохранение указанной папки
$ source scripts/activate                             # Выполнение скрипта activate
```

```ShellSession
# Копируем файлы из удаленного репозитория github в папку projects/lab06 
$ git clone https://github.com/${GITHUB_USERNAME}/lab05 projects/lab06     
Клонирование в «projects/lab06»…
remote: Enumerating objects: 58, done.
remote: Counting objects: 100% (58/58), done.
remote: Compressing objects: 100% (41/41), done.
remote: Total 58 (delta 23), reused 36 (delta 11), pack-reused 0
Распаковка объектов: 100% (58/58), готово.
$ cd projects/lab06                                                         # Переход в папку lab06            
$ git remote remove origin                                                  # Удаление ссылки на старый удаленный репозиторий
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab06         # Добавление ссылки на новый удаленный репозиторий
```

```ShellSession
Редактирование CMakeLists.txt.
$ gsed -i '/project(print)/a\                                                # Дописывание в указанный файл указанной строки
set(PRINT_VERSION_STRING "v\${PRINT_VERSION}")
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION\
  \${PRINT_VERSION_MAJOR}.\${PRINT_VERSION_MINOR}.\${PRINT_VERSION_PATCH}.\${PRINT_VERSION_TWEAK})
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION_TWEAK 0)
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION_PATCH 0)
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION_MINOR 1)
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION_MAJOR 0)
' CMakeLists.txt
$ git diff                                              # Просмотр отличий локальной версии от последнего коммита
diff --git a/CMakeLists.txt b/CMakeLists.txt
index 176b7ba..eb75e94 100644
--- a/CMakeLists.txt
+++ b/CMakeLists.txt
@@ -7,6 +7,13 @@ option(BUILD_EXAMPLES "Build examples" OFF)
 option(BUILD_TESTS "Build tests" OFF)
 
 project(print)
+set(PRINT_VERSION_MAJOR 0)
+set(PRINT_VERSION_MINOR 1)
+set(PRINT_VERSION_PATCH 0)
+set(PRINT_VERSION_TWEAK 0)
+set(PRINT_VERSION
+  ${PRINT_VERSION_MAJOR}.${PRINT_VERSION_MINOR}.${PRINT_VERSION_PATCH}.${PRINT_VERSION_TWEAK})
+set(PRINT_VERSION_STRING "v${PRINT_VERSION}")
 
 add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)

```

```ShellSession
$ touch DESCRIPTION && edit DESCRIPTION                               # Создание и редактирование DESCRIPTION
$ touch ChangeLog.md                                                  # Создание ChangeLog.md
$ export DATE="`LANG=en_US date +'%a %b %d %Y'`"                      # Инициализация переменной DATE
$ cat > ChangeLog.md <<EOF                                            # Редактирование ChangeLog.md
* ${DATE} ${GITHUB_USERNAME} <${GITHUB_EMAIL}> 0.1.0.0
- Initial RPM release
EOF
```
### Редактирование CPackConfig.cmake
```ShellSession
$ cat > CPackConfig.cmake <<EOF                                      # Добавление в CPackConfig.cmake указанной строки
include(InstallRequiredSystemLibraries)
EOF
```

```ShellSession
# Дописывание переменных (почта, версия, описание (краткое, полное))
$ cat >> CPackConfig.cmake <<EOF
set(CPACK_PACKAGE_CONTACT ${GITHUB_EMAIL})
set(CPACK_PACKAGE_VERSION_MAJOR \${PRINT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR \${PRINT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH \${PRINT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK \${PRINT_VERSION_TWEAK})
set(CPACK_PACKAGE_VERSION \${PRINT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_FILE \${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "static C++ library for printing")
EOF
```

```ShellSession
# Дополнение конфигурации переменными с путями к файлам лицензии и README
$ cat >> CPackConfig.cmake <<EOF

set(CPACK_RESOURCE_FILE_LICENSE \${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README \${CMAKE_CURRENT_SOURCE_DIR}/README.md)
EOF
```

```ShellSession
# Добавление информации о RPM
$ cat >> CPackConfig.cmake <<EOF

set(CPACK_RPM_PACKAGE_NAME "print-devel")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_GROUP "print")
set(CPACK_RPM_CHANGELOG_FILE \${CMAKE_CURRENT_SOURCE_DIR}/ChangeLog.md)
set(CPACK_RPM_PACKAGE_RELEASE 1)
EOF
```

```ShellSession
# Добавление информации о DEB
$ cat >> CPackConfig.cmake <<EOF

set(CPACK_DEBIAN_PACKAGE_NAME "libprint-dev")
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)
EOF
```

```ShellSession
# Подключение CPack в CMake конфиг
$ cat >> CPackConfig.cmake <<EOF

include(CPack)
EOF
```
---

```ShellSession
# Подключение дополнительной конфигурации
$ cat >> CMakeLists.txt <<EOF             # Добавление в конец CMakeLists.txt указанной строки

include(CPackConfig.cmake)
EOF
```

```ShellSession
# Редактирование README.md
$ gsed -i 's/lab05/lab06/g' README.md
```

```ShellSession
$ git add .                               # Фиксация изменений     
$ git commit -m"added cpack config"       # Коммит зафиксированных изменений
[master a2bc132] added cpack config
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

 5 files changed, 42 insertions(+), 7 deletions(-)
 create mode 100644 CPackConfig.cmake
 create mode 100644 ChangeLog.md
 create mode 100644 DESCRIPTION

$ git tag v0.1.0.0                       # Установка тэга
$ git push origin master --tags          # Отправка изменений
Username for 'https://github.com': DarthBarada
Password for 'https://DarthBarada@github.com': 
Перечисление объектов: 65, готово.
Подсчет объектов: 100% (65/65), готово.
При сжатии изменений используется до 4 потоков
Сжатие объектов: 100% (58/58), готово.
Запись объектов: 100% (65/65), 23.48 KiB | 2.61 MiB/s, готово.
Всего 65 (изменения 25), повторно использовано 0 (изменения 0)
remote: Resolving deltas: 100% (25/25), done.
To https://github.com/DarthBarada/lab06
 * [new branch]      master -> master
 * [new tag]         v0.1.0.0 -> v0.1.0.0

```

```ShellSession
# Подключаем наш репозиторий к travis
$ travis login --auto   # По умолчанию я подключил свой аккаунт к travis, поэтоу данная строка не нужна
$ travis enable    
Detected repository as DarthBarada/lab06, is this correct? |yes| yes
DarthBarada/lab06: enabled :)
```

```ShellSession
$ cmake -H. -B_build            # Сборка CMakeLists.txt в директорию _build
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
-- Configuring done
-- Generating done
-- Build files have been written to: /home/darthbarada/DarthBarada/workspace/projects/lab06/_build
$ cmake --build _build          # Компиляция в директории _build
Scanning dependencies of target print
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print
$ cd _build                     # Вход в папку _build
$ cpack -G "TGZ"                # Упаковка с использованием CPack
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print
CPack: Create package
CPack: - package: /home/darthbarada/DarthBarada/workspace/projects/lab06/_build/print-0.1.0.0-Linux.tar.gz generated.
$ cd ..                         # Выход из папки
```

```ShellSession
$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ"        # Сборка CMake в папку _build, используя распаковку
-- Configuring done
-- Generating done
-- Build files have been written to: /home/darthbarada/DarthBarada/workspace/projects/lab06/_build
$ cmake --build _build --target package             # Компиляция цели package
[100%] Built target print
Run CPack packaging tool...
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print
CPack: Create package
CPack: - package: /home/darthbarada/DarthBarada/workspace/projects/lab06/_build/print-0.1.0.0-Linux.tar.gz generated.
```

```ShellSession
$ mkdir artifacts                  # Создание папки artifacts
$ mv _build/*.tar.gz artifacts     # Перемещение собранного пакета
$ tree artifacts                   # Отображения дерева указанной папки
artifacts
└── print-0.1.0.0-Linux.tar.gz

0 directories, 1 file
```

## Report

```ShellSession
$ popd
$ export LAB_NUMBER=06
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Homework

После того, как вы настроили взаимодействие с системой непрерывной интеграции,</br>
обеспечив автоматическую сборку и тестирование ваших изменений, стоит задуматься</br>
о создание пакетов для измениний, которые помечаются тэгами (см. вкладку [releases](https://github.com/tp-labs/lab06/releases)).</br>
Пакет должен содержать приложение _solver_ из [предыдущего задания](https://github.com/tp-labs/lab03#задание-1)
Таким образом, каждый новый релиз будет состоять из следующих компонентов:
- архивы с файлами исходного кода (`.tar.gz`, `.zip`)
- пакеты с бинарным файлом _solver_ (`.deb`, `.rpm`, `.msi`, `.dmg`)

В качестве подсказки:
```bash
$ cat .travis.yml
os: osx
script:
...
- cpack -G DragNDrop # dmg

$ cat .travis.yml
os: linux
script:
...
- cpack -G DEB # deb

$ cat .travis.yml
os: linux
addons:
  apt:
    packages:
    - rpm
script:
...
- cpack -G RPM # rpm

$ cat appveyor.yml
platform:
- x86
- x64
build_script:
...
- cpack -G WIX # msi
```

Для этого нужно добавить ветвление в конфигурационные файлы для **CI** со следующей логикой:</br>
если **commit** помечен тэгом, то необходимо собрать пакеты (`DEB, RPM, WIX, DragNDrop, ...`) </br>
и разместить их на сервисе **GitHub**. (см. пример для [Travi CI](https://docs.travis-ci.com/user/deployment/releases))</br>

## Links

- [DMG](https://cmake.org/cmake/help/latest/module/CPackDMG.html)
- [DEB](https://cmake.org/cmake/help/latest/module/CPackDeb.html)
- [RPM](https://cmake.org/cmake/help/latest/module/CPackRPM.html)
- [NSIS](https://cmake.org/cmake/help/latest/module/CPackNSIS.html)

```
Copyright (c) 2015-2019 The ISC Authors
```
