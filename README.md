## Laboratory work VIII

Данная лабораторная работа посвещена изучению средств пакетирования на примере **CPack**

```ShellSession
$ open https://cmake.org/Wiki/CMake:CPackPackageGenerators
```

## Tasks

- [ ] 1. Создать публичный репозиторий с названием **lab08** на сервисе **GitHub**
- [ ] 2. Выполнить инструкцию учебного материала
- [ ] 3. Ознакомиться со ссылками учебного материала
- [ ] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=hurrybomber //создаем переменную с ником на гитхабе
$ export GITHUB_EMAIL=<адрес_почтового_ящика> //переменная с ящиком (гитхаб)
$ alias edit=<nano|vi|vim|subl> //псевдоним для текстового редактора
$ alias gsed=sed # for *-nix system //псевдоним для sed
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace //переходим в каталог
$ pushd . //возвращаем текущий каталог на верхний стек
~/hurrybomber/workspace ~/hurrybomber/workspace

$ source scripts/activate //выполняем скрипты
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab07 projects/lab08 //копируем репозиторий
Cloning into 'projects/lab08'...
remote: Counting objects: 125, done.
remote: Compressing objects: 100% (87/87), done.
remote: Total 125 (delta 38), reused 114 (delta 30), pack-reused 0
Receiving objects: 100% (125/125), 251.90 KiB | 691.00 KiB/s, done.
Resolving deltas: 100% (38/38), done.

$ cd projects/lab08 //переходим в каталог
$ git remote remove origin //управление репозиторием
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab08 //новое имя
```

```ShellSession
$ gsed -i '/project(print)/a\ //записываем в cmake версию String
set(PRINT_VERSION_STRING "v${PRINT_VERSION}")
' CMakeLists.txt
$ gsed -i '/project(print)/a\ //записываем в cmake общую версию
set(PRINT_VERSION \
\${PRINT_VERSION_MAJOR}.\${PRINT_VERSION_MINOR}.\${PRINT_VERSION_PATCH}.\${PRINT_VERSION_TWEAK})
' CMakeLists.txt
$ gsed -i '/project(print)/a\ //записываем в cmake версию Tweak
set(PRINT_VERSION_TWEAK 0)
' CMakeLists.txt
$ gsed -i '/project(print)/a\ //записываем в cmake версию Patch
set(PRINT_VERSION_PATCH 0)
' CMakeLists.txt 
$ gsed -i '/project(print)/a\ //записываем в cmake версию Minor
set(PRINT_VERSION_MINOR 1)
' CMakeLists.txt
$ gsed -i '/project(print)/a\ //записываем в cmake версию Major
set(PRINT_VERSION_MAJOR 0)
' CMakeLists.txt
```

```ShellSession
$ touch DESCRIPTION && edit DESCRIPTION //создаем и открываем файл
$ touch ChangeLog.md //создаем файл
$ export DATE="`LANG=en_US date +'%a %b %d %Y'`" //переменную для параметра языка
$ cat > ChangeLog.md <<EOF //записываем в файл информацию об изменениях и версию
* ${DATE} ${GITHUB_USERNAME} <${GITHUB_EMAIL}> 0.1.0.0 
- Initial RPM release
EOF
```

```ShellSession
$ cat > CPackConfig.cmake <<EOF //записываем в СиМэйк подключаемую библиотеку
include(InstallRequiredSystemLibraries)
EOF
```

```ShellSession
$ cat >> CPackConfig.cmake <<EOF //записываем в cmake параметры сборки
set(CPACK_PACKAGE_CONTACT ${GITHUB_EMAIL})
set(CPACK_PACKAGE_VERSION_MAJOR \${PRINT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR \${PRINT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH \${PRINT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK \${PRINT_VERSION_TWEAK})
set(CPACK_PACKAGE_VERSION \${PRINT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_FILE \${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "static c++ library for printing")
EOF
```

```ShellSession
$ cat >> CPackConfig.cmake <<EOF //записываем в cmake параметры для лицензии и ридми

set(CPACK_RESOURCE_FILE_LICENSE \${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README \${CMAKE_CURRENT_SOURCE_DIR}/README.md)
EOF
```

```ShellSession
$ cat >> CPackConfig.cmake <<EOF //записываем в cmake дополнительные параметры пакетирования (название, лицензия, URL и тд)

set(CPACK_RPM_PACKAGE_NAME "print-devel")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_GROUP "print")
set(CPACK_RPM_PACKAGE_URL "https://github.com/${GITHUB_USERNAME}/lab07")
set(CPACK_RPM_CHANGELOG_FILE \${CMAKE_CURRENT_SOURCE_DIR}/ChangeLog.md)
set(CPACK_RPM_PACKAGE_RELEASE 1)
EOF
```

```ShellSession
$ cat >> CPackConfig.cmake <<EOF //записываем в cmake параметры для Debian

set(CPACK_DEBIAN_PACKAGE_NAME "libprint-dev")
set(CPACK_DEBIAN_PACKAGE_HOMEPAGE "https://${GITHUB_USERNAME}.github.io/lab07")
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)
EOF
```

```ShellSession
$ cat >> CPackConfig.cmake <<EOF //включаем библиотеку CPack

include(CPack)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF //включаем конфигурацию CPack

include(CPackConfig.cmake)
EOF
```

```ShellSession
$ gsed -i 's/lab07/lab08/g' README.md //записываем новую lab
```

```ShellSession
$ git add . //выбираем все содержимое директории
$ git commit -m"added cpack config" //создаем коммит
[master a42826f] added cpack config
5 files changed, 39 insertions(+)
create mode 100644 CPackConfig.cmake
create mode 100644 ChangeLog.md
create mode 100644 DESCRIPTION

$ git push origin master //выгружаем на гитхаб
Counting objects: 108, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (81/81), done.
Writing objects: 100% (108/108), 248.17 KiB | 41.36 MiB/s, done.
Total 108 (delta 22), reused 95 (delta 18)
remote: Resolving deltas: 100% (22/22), done.
To https://github.com/hurrybomber/lab08
* [new branch]      master -> master

```

```ShellSession
$ travis login --auto //лигинимся автоматически на трэвисе
Successfully logged in as hurrybomber!

$ travis enable //подключаем сборку
Detected repository as hurrybomber/lab08, is this correct? |yes| yes
hurrybomber/lab08: enabled :)

```

```ShellSession
$ cmake -H. -B_build //подготоваливаем проект для сборки
-- Configuring done
-- Generating done
-- Build files have been written to: /Users/hurrybomber/workspace/projects/lab08/_build

$ cmake --build _build //собираем в _build
Scanning dependencies of target print
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print

$ cd _build //переходим в _build
$ cpack -G "TGZ" //пакетируем с TGZ (прошла для MAC_OS)
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print
CPack: Create package
CPack: - package: /Users/hurrybomber/workspace/projects/lab08/_build/print-0...-Darwin.tar.gz generated.

$ cpack -G "RPM" //пакетируем с RPM (не прошла)
CPack Error: Cannot initialize CPack generator: RPM
Segmentation fault: 11

$ cpack -G "DEB" //пакетируем с DEB (не прошла)
CPack Error: Cannot initialize CPack generator: DEB
Segmentation fault: 11

$ cpack -G "NSIS" //пакетируем с NSIS (не прошла)
CPack Error: Cannot find NSIS compiler makensis: likely it is not installed, or not in your PATH
CPack Error: Could not read NSIS registry value. This is usually caused by NSIS not being installed. Please install NSIS from http://nsis.sourceforge.net
CPack Error: Cannot initialize the generator NSIS

$ cpack -G "DragNDrop" //пакетируем с DragNDrop (прошла для MAC_OS)
CPack: Create package using DragNDrop
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print
CPack: Create package

CPack: - package: /Users/hurrybomber/workspace/projects/lab08/_build/print-0...-Darwin.dmg generated.

$ cd .. //шаг назад по каталогам
```

```ShellSession
$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ" //выполняем с генератором пакетирования TGZ
-- Configuring done
-- Generating done
-- Build files have been written to: /Users/hurrybomber/workspace/projects/lab08/_build

$ cmake --build _build --target package //собираем проект
[100%] Built target print
Run CPack packaging tool...
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print
CPack: Create package
CPack: - package: /Users/hurrybomber/workspace/projects/lab08/_build/print-0...-Darwin.tar.gz generated.

```

```ShellSession
$ mkdir artifacts //создаем директорию
$ mv _build/*.tar.gz artifacts //перемещаем определенные по параметру файлы в новый каталог
$ tree artifacts //выводим содержимое деревом
artifacts
└── print-0...-Darwin.tar.gz

0 directories, 1 file

```

## Report

```ShellSession
$ popd //возвращаем верхний стэк
$ export LAB_NUMBER=08 //переменная
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER} //копируем репозиторий
$ mkdir reports/lab${LAB_NUMBER} //создаем каталог
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md //копируем
$ cd reports/lab${LAB_NUMBER} //переходим в каталог
$ edit REPORT.md //заполняем отчет
$ gistup -m "lab${LAB_NUMBER}" //выгружаем в гист
```

## Links

- [DMG](https://cmake.org/cmake/help/latest/module/CPackDMG.html)
- [DEB](https://cmake.org/cmake/help/latest/module/CPackDeb.html)
- [RPM](https://cmake.org/cmake/help/latest/module/CPackRPM.html)
- [NSIS](https://cmake.org/cmake/help/latest/module/CPackNSIS.html)

```
Copyright (c) 2017 Братья Вершинины
```
