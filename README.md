[![Build Status](https://travis-ci.org/VladislavBurch/lab14_true.svg?branch=master)](https://travis-ci.org/VladislavBurch/lab14_true)

```
$ http://www.boost.org/doc/libs/1_64_0/doc/html/process.html
```

Программа на **C++** для упрощения процесса сборки, установки и упаковки проектов основанных на конфигурационных файлах **CMakeLists.txt**.

Исполняемый файл программы должен иметь название **builder** и поддерживать следующие опции запуска:

```ShellSession
$ ./builder --help
Usage: builder [options]
Allowed options:
  --help                    : выводим вспомогательное сообщение
  --config <Release|Debug>  : указываем конфигурацию сборки (по умолчанию Debug)
  --install                 : добавляем этап установки (в директорию _install)
  --pack                    : добавляем этап упаковки (в архив формата tar.gz)
  --timeout <count>         : указываем время ожидания (в секундах)
```

Примеры запуска программы с описанием эквивалентных запусков процессов программы **CMake**.

```ShellSession
$ ./builder
<=>
$ cmake -H. -B_builds -DCMAKE_INSTALL_PREFIX=_install -DCMAKE_BUILD_TYPE=Debug
$ cmake --build _builds
```

```ShellSession
$ ./builder --config Release
<=>
$ cmake -H. -B_builds -DCMAKE_INSTALL_PREFIX=_install -DCMAKE_BUILD_TYPE=Release
$ cmake --build _builds
```


```ShellSession
$ ./builder --install
<=>
$ cmake -H. -B_builds -DCMAKE_INSTALL_PREFIX=_install -DCMAKE_BUILD_TYPE=Debug
$ cmake --build _builds
$ cmake --build _builds --target install
```

```ShellSession
$ ./builder --pack
<=>
$ cmake -H. -B_builds -DCMAKE_INSTALL_PREFIX=_install -DCMAKE_BUILD_TYPE=Debug
$ cmake --build _builds
$ cmake --build _builds --target package
```

```ShellSession
$ ./builder --install --pack
<=>
$ cmake -H. -B_builds -DCMAKE_INSTALL_PREFIX=_install -DCMAKE_BUILD_TYPE=Debug
$ cmake --build _builds
$ cmake --build _builds --target install
$ cmake --build _builds --target package
```

```ShellSession
$ ./builder --timeout 500
<=>
$ cmake -H. -B_builds -DCMAKE_INSTALL_PREFIX=_install -DCMAKE_BUILD_TYPE=Debug
$ cmake --build _builds
```

### Особенности

1. Для работы с процессами используется библиотека **Boost.Process**.

2. В случае если время ожидания истекает, то программа завершает **все** дочерние запущенные процессы.

3. Этап установки запускается, только в случае успешного завершения процесса сборки.

4. Этап упаковки запускается, только в случаях успешного завершения процесса сборки и 
успешного завершения процесса установки.

5. Стандартные потоки вывода дочерних процессов необходимо перенаправить в стандартный поток 
вывода родительского процесса исполняемого файла **builder**.

```ShellSession
$ cmake --build _build
Scanning dependencies of target builder
[ 50%] Building CXX object CMakeFiles/builder.dir/sources/process.cpp.o
[100%] Linking CXX executable builder
[100%] Built target builder
```

### Ссылки

- [Boost.Process](http://www.highscore.de/boost/process/)
- [Boost.Program_options](http://www.boost.org/doc/libs/1_65_0/doc/html/program_options.html)
