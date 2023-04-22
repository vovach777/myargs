#myargs
simple arguments parser

# Motivation
For simple and quick experiments, simple solutions for parsing arguments were required.
I tried popular solutions and they turned out to be heavy, convoluted and overloaded.
I decided to take an hour and write my own argument parser class that does just what I need.

# Total
The class is described in the header file.
No comparisons and checks! Use immediately what the user asked.
## Can
### Short options
Single character options. They are prefixed with "-".

```
-a -b -c -d -?
```

They may have meaning. There is only one way to set it: directly after the argument symbol.

```
-a1 -b2 -cText -d-1 -I/path
```

### Long options.
Multi-character options. They are prefixed with "--":

```
--alpha --beta --help
```

They can also have a value, but after the "=" symbol:

```
--alpha=1 --bata=2 --help=compact -incluide-dir=/path
```

### No prefix
It's called a command or just an argument.

## Can not
* Get the values of the arguments of their next argument (this option introduces vagueness and I excluded it for the sake of simplicity and rigor.
* Combine short and long options and one entity. Definitely need to figure out how to do it.

# Development
* Requires validation.
* Need to generate help (help) based on validation.

It will be useful and convenient for real command line programs.

# How to use

```c++
#include <iostream>
#include "myargs.hpp"

int main(int argc, char** argv)
{
    myargs::Args args(argc,argv);

    std::cout << "cmd_nb:" << args.cmd_nb << std::endl;
    //all arguments and options
    //arguments are parameters without prefix
    //options are prefixed options
    for (auto & kv : args)
    {
       std::cout << kv.first << " : " << kv.second << std::endl;
    }
    // first argument (command)
    std::cout << args[0] << std::endl;
     // second argument if any.
    std::cout << (args.cmd_nb > 1 ? args[1] : "none" ) << std::endl;
    //option integer -a if not then 0
    std::cout << args.get("a") << std::endl;
    //option -a if not then 555
    std::cout << args.get<555>("a") << std::endl;
    //option -b take and limit the range
    std::cout << args.get<1,-5,5>("b") << (args.has("b") ? " (by user)" : " (default value is 1)" ) << std::endl;

    //this is how we get the string and set the default string
    std::cout << args.get("I","/path") << std::endl;
    std::cout << args.get("include-dir","/path/to/dir") << std::endl;
    // direct access to unordered_map parameters
    std::cout << args.m.contains("f") << std::endl;

}
```

conclusion

```bush
>testargs -I/root -a777 -b1000 --include-dir=/usr/local
cmd_nb:1
include-dir : /usr/local
b : 1000
a : 777
I : /root
%0 : testargs.exe
testargs.exe
none
777
777
5 (by user)
/root
/usr/local
0
```

# myargs
simple arguments parser

# Мотивация
Для простых и быстрых экспериментов потребовались простые решения для парсинга аргументов.
Я попробовал популярные решения и они оказались тяжелыми замысловатыми и перегруженными.
Я решил потратить час времени и написать свой класс парсера аргументов, которые делает только то, что мне нужно.

# Итог
Класс описан в файле хедера.
Никаких сопоставлений и проверок! Используй сразу то, что задал пользователь.
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
--alpha=1 --bata=2 --help=compact -incluide-dir=/path
```

### Без префикса
Это называется команда или просто аргумент.

## Не умеет
* Получать значения аргументов их следуюего аргемента (этот свособ вносит неопреленность и я его исключил в угоду простоте и строгости.
* Объединять короткие и длинные опции и одну сущность.  Определенно, нужно придумать как это сделать.

# Развитие
* Нужна валидация.
* Нужена генерация помощи (справки), основанный на валидации. 

Это будет полезно и удобно для реальных программ командной строки.

# Как пользоваться

```c++
#include <iostream>
#include "myargs.hpp"

int main(int argc, char** argv)
{
   myargs::Args args(argc,argv);

   std::cout << "cmd_nb:" << args.cmd_nb << std::endl;
   //все аргументы и опции
   //аргументы - это параметры без префикс
   //опции - это параметры с префиксом
   for (auto & kv : args)
   {
      std::cout << kv.first << " : " << kv.second << std::endl;
   }
   // первый аргумент (команда)
   std::cout << args[0] << std::endl;
    // второй аргемнт если есть.
   std::cout <<  (args.cmd_nb > 1 ? args[1] : "none" ) << std::endl;
   //опция integer -a  если нет то 0
   std::cout << args.get("a") << std::endl;
   //опция -a если нет то 555
   std::cout << args.get<555>("a") << std::endl;
   //опция -b взять и ограничить диаппазон
   std::cout << args.get<1,-5,5>("b") <<  (args.has("b") ? " (by user)" : " (default value is 1)"  )  << std::endl;

   //так получаем строку и задаем строку по умолчанию
   std::cout << args.get("I","/path") << std::endl;
   std::cout << args.get("include-dir","/path/to/dir") << std::endl;
   //прямой доступ unordered_map параметров
   std::cout << args.m.contains("f") << std::endl;

}
```

вывод

```bush
>testargs -I/root -a777 -b1000 --include-dir=/usr/local
cmd_nb:1
include-dir : /usr/local
b : 1000
a : 777
I : /root
%0 : testargs.exe
testargs.exe
none
777
777
5 (by user)
/root
/usr/local
0
```
