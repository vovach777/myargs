# myargs
simple arguments parser

# Мотивация
Для простых и быстрых экспериментов потребовались простые решения для парсинга аргументов.
Я попробовал популярные решения и они оказались тяжелыми замысловатыми и перегруженными.
Я решил потратить час времени и написать свой класс парсера аргументов, которые делает только то, что мне нужно.

# Итог
Класс описан в файле хедера. Его  определить глобально, а в main() его надо инициализировать.
Можно создавать группировку опций, чтобы объединять псевдонимы опций. 
Опции с одним названием (или в одной группе) образуют массив значений.
Можно найти исходное название опции, чтобы указать пользователю о ошибке синтаксиса. 

## Умеет
### Короткие опции
Односимвольные параметры. Имеют префикс "-".

```
-a -b -c -d -?
```

У них может быть значение. Есть только один способ как его задать: непосредственно за символом аргумента.

```
-a1 -b2 -cText  -d-1 -I/path
```

### Длинные опции. 
Многосимвольные параметры. Имеют префикс "--":

```
--alpha --beta --help
```

Так же могут иметь значение, но после символа "=":

```
--alpha=1 --bata=2 --help=compact --incluide-dir=/path
```

### Без префикса
Это называется команда или просто аргумент.

## Не умеет
* Получать значения аргументов из следуюего аргумента (этот способ вносит неопреленность. исключён, чтобы сохранить простоту и строгость.

# Развитие
* Нужна валидация.
* Нужена генерация помощи (справки), основанный на валидации. 

Это будет полезно и удобно для реальных программ командной строки.

# Как пользоваться

```c++
#include <iostream>
#include "myargs.hpp"

myargs::Args args;

int main(int argc, char** argv)
{
   args.add_to_group("*i", 'I' );
   args.add_to_group("*i", "include-dir" );
   args.parse(argc,argv);

   std::cout << "arg count: " << args.size() << std::endl;
   //все аргументы и опции
   //аргументы - это параметры без префикс
   //опции - это параметры с префиксом
   for (auto & kv : args)
   {
      std::cout << kv.first << " : " << myargs::str(kv.second) << std::endl;
   }
   // первый аргумент (команда)
   std::cout << args[0] << std::endl;
    // второй аргемнт если есть.
   std::cout <<  (args.size() > 1 ? args[1] : "none" ) << std::endl;
   //опция integer -a  если нет то 0
   std::cout << args.get("a") << std::endl;
   //опция -a если нет то 555
   std::cout << args.get("a",555) << std::endl;
   //опция -b взять и ограничить диаппазон
   std::cout << args.get("b",1,-5,5) << (args.has("b") ? " (by user)" : " (default value is 1)"  )  << std::endl;

   //так получаем строку и задаем строку по умолчанию
   std::cout << args.get("I","/path") << std::endl;
   std::cout << args.get("include-dir","/path/to/dir") << std::endl;
   //так берем список всех опций -Idir1 -Idir2 --include-dir=dir3 --include-dir=dir4
   std::cout << myargs::str( args.all("*i") )<< std::endl;


}
```

```bash
>clang++ example.cpp -o example -std=c++17
>example cmd1 -Idir1 -Idir2 --include-dir=dir3 --include-dir=dir4 -Idir5 cmd2
arg count: 3
 : example.exe, cmd1, cmd2
*i : dir1, dir2, dir3, dir4, dir5
example.exe
cmd1
0
555
1 (default value is 1)
path
path/to/dir
dir1, dir2, dir3, dir4, dir5
```
