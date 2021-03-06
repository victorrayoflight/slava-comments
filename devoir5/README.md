# Создание команды, которая возвращает баннер с сообщением

## Интро

Давай попробуем разобраться, заранее выбросив из головыЖ

- я хочу, но нет времени учится програмированию
- препод говнюк, ничего нас не учит, напридумывал задач блин
- вообще програмирование сложно, не думаю что я смогу когда-нибудь что-то сделать
- я ничего не понимаю что тут вообще надо сделать, не знаю с чего подойти

Все это негатив, мешающий нормально жить. Медведи на велосипеде научились ездить,
неужели люди тупее и не могут запрограмировать банальные вещи?

## Задание

Пример банера что надо вывести:

~~~
*******************
*******************
***   Bonsoir   ***
*******************
*******************
~~~

Давай невдаваясь в подробное задание, посмотрем на вывод,
и попытаемся понять что нада сделать.

1) Надо вывести 4 строчки одинакового содержания и длины. Что для этого надо?
Надо знать каким символом заполнять и сколько раз печатать символ.
Для этого нам понадобится 2 аргумента (параметра).
Вспоминаем что символ должен настраиватся,
а длина состоит из длины слова что надо вывести + 12 символов (по 6 слева и справа). 

2) Строка по середине содержит слово, заданое аргумнетом из конмадной строки,
по краям имеющее два вида обрамление.
Одно это птсоряющийся все тот же симовл что в дргуих 4 стркоах,
значит будем использовать тот же аргумент, в вот количество повторений этого символа это новый аргумент.
Другое обрамление это повторяющиеся пробелы. Количество повторений тоже должно настраиваться,
и по заданию оно отличается от количестов здездочек, значит это другой аргумент.

Из пункта (1) можно увидеть что нам нужно повторить одно и тоже 4 раза (напечатать 4 одинаковые линии).
Чтоб не писать один и тот же кусок кода 4 раза, вроде
~~~bash
echo "Hello"
echo "Hello"
echo "Hello"
echo "Hello"
~~~
выделют повторяющийся код в функцию и потом просто вызывают эту функцию каждый раз когда надо выполнить кид из нее
~~~bash
say_hello () {
  echo "Hello"
}
say_hello
say_hello
say_hello
say_hello
~~~
Код выше сделает тоже самое, напичает 4 раза "Hello".

Вернемся к заданию, нам надо 4 линии повторяющихся символов.
Помним для повторений одного и того же используется цикл `for`.
Итак подведем итоги: нужна функция которая будет использовать 2 аргумента и цикл `for`
Напишим функцию:
~~~bash
repeat_char () {
  # Первым аргументом функции будет повторяющийся символ
  CHAR="$1"
  # Вторым аргументом функции будет кол-во повторений
  LENGTH=$2
  # В эту переменную будем конкатинировать (не зняю как на рус., на анг. concatenation, т.е. объединение нескольких строк)
  # Зачем она нам нужно, почему сразу не печатать, потому что `echo` выводит строчку,
  # т.е. второй раз `echo` напечатает на новой строке, а нам надо символы в одной строке
  STR=""
  # Цикл повторений от одного до указанного LENGTH с шагом +1
  for (( i=1; i<=${LENGTH}; i++ )) ; do
    # Объденеям предудщую строку с очередным симолом
    STR="${STR}${CHAR}"
  done

  # Строка готова печатаем
  echo "${STR}"
}
~~~ 

Как я уже сказал, основное предназначение функций, это недублирование повторяющийся кода.
Но функции можно создавать и для одиночного вызова, иногда код который выделен в функцию
выглядит сложно внутри другого кода, в отличии от вызова фукнции.
Так почему бы не выделить функцию и для строки по середине банера

~~~bash
print_message () {
  # Слово на баннере, первым аргументом функции, т.к. должна быть возможность это указать при вызове функции и скрипта
  MESSAGE="$1"
  # Символ по бокам, указывается аргументом, т.к. он настраеваемый
  MARGIN_CHAR="$2"
  # Количество повторении обрамления, также настраивается третим аргументом функции
  MARGIN_LENGTH=$3
  # Символ разделитель обрамления и слова. Используется пробле, без возможности настраить.
  PADDING_CHAR=" "
  # Количество пробелов в разделителе.
  PADDING_LENGTH=$4
  # В эту переменную будем объеденять всю линию
  STR=""

  # Генерируем левый край
  for (( i=1; i<=${MARGIN_LENGTH}; i++ )) ; do
    STR="${STR}${MARGIN_CHAR}"
  done

  # Генерируем левый разделитель
  for (( i=1; i<=${PADDING_LENGTH}; i++ )) ; do
    STR="${STR}${PADDING_CHAR}"
  done

  # Добавляем заголовок
  STR="${STR}${MESSAGE}"

  # Добавляем правый разделитель
  for (( i=1; i<=${PADDING_LENGTH}; i++ )) ; do
    STR="${STR}${PADDING_CHAR}"
  done

  # Добавляем правый край
  for (( i=1; i<=${MARGIN_LENGTH}; i++ )) ; do
    STR="${STR}${MARGIN_CHAR}"
  done

  # Печатаем результат
  echo "${STR}"
}
~~~ 

Пробуем, вызываем функции в нужно порядке и передаем нужные аргументы

~~~bash
repeat_char "+" 20
repeat_char "+" 20
print_message "Bonsoir" "+" 3 3
repeat_char "+" 20
repeat_char "+" 20
~~~ 

Уверен ты заметил повторящийся код в функции `print_message`.
Что он делает? Тоже самое что функция `repeat_char`,
так давай изменим `print_message` чтоб избавится от повторяющего кода.

~~~bash
print_message () {
  # Слово на баннере, первым аргументом функции, т.к. должна быть возможность это указать при вызове функции и скрипта
  MESSAGE="$1"
  # Символ по бокам, указывается аргументом, т.к. он настраеваемый
  MARGIN_CHAR="$2"
  # Количество повторении обрамления, также настраивается третим аргументом функции
  MARGIN_LENGTH=$3
  # Символ разделитель обрамления и слова. Используется пробле, без возможности настраить.
  PADDING_CHAR=" "
  # Количество пробелов в разделителе.
  PADDING_LENGTH=$4

  # Генерируем край
  MARGIN="$( repeat_char "${MARGIN_CHAR}" ${MARGIN_LENGTH} )"
  # Генерируем разделитель
  PADDING="$( repeat_char "${PADDING_CHAR}" ${PADDING_LENGTH} )"
  
  # Объеденяем все в нужную строку
  STR=${MARGIN}${PADDING}${MESSAGE}${PADDING}${MARGIN}

  # Печатаем результат
  echo "${STR}"
}
~~~ 

Итак у нас есть функции для генерации потовряющихся символов и функция генерации строки с заголовокм банера,
уже можно напечатать все 5 строк. Но при генерации все строки должны иметь одинаковую длину.
Для этого нам надо подсчитать количество букв в заголовке банера, сложить с длиной краев и разделителя.
Еще одна функция для этого куска кода?

~~~bash
print_banner() {
  # Слово на баннере, первым аргументом функции, т.к. должна быть возможность это указать при вызове функции и скрипта
  MESSAGE="$1"
  # Символ по бокам, указывается аргументом, т.к. он настраеваемый
  MARGIN_CHAR="$2"
  # Количество повторении обрамления, также настраивается третим аргументом функции
  MARGIN_LENGTH=$3
  # Количество пробелов в разделителе.
  PADDING_LENGTH=$4

  # Вычисляем длину заголовка, в баше это делает диез после фигурной скобки 
  LABEL_LENGTH=${#MESSAGE}

  # Вычисляем длину линий сверху/снизу
  # Матемитические операии делаются в баше при помощи двух круглых скобок
  BORDER_LENGTH=$(( LABEL_LENGTH + MARGIN_LENGTH * 2 + PADDING_LENGTH * 2 ))

  # Печатаем баннер
  repeat_char "${MARGIN_CHAR}" ${BORDER_LENGTH}
  repeat_char "${MARGIN_CHAR}" ${BORDER_LENGTH}
  print_message "${MESSAGE}" "${MARGIN_CHAR}" ${MARGIN_LENGTH} ${PADDING_LENGTH}
  repeat_char "${MARGIN_CHAR}" ${BORDER_LENGTH}
  repeat_char "${MARGIN_CHAR}" ${BORDER_LENGTH}
}
~~~ 

Пробуем

~~~bash
# Звоздочка в одинарных ковычках, т.к. звездочка это один из нескольких специальных символов в баше
# и для того чтоб баш не юзал его, то надо его использовать в одинарных ковычках
print_banner "Bonsoir" '*' 3 3
~~~

Все банер готов. Осталось только считать настройки с аргументов при запуске скрипта.
Но для начало можно было обратить внимание что `print_message` и `print_banner` очень похожи,
большинство аргументов теже и т.д. Это подсказывает нам что их надо оъеденить в одну функцию,
т.к. две отдельные не имеют смысла и только усложняют чтение и пониамние кода.
Это процесс написания кода, а потом, когда он готов, улучшения его, перенписывание, называется *Refactoring*. 

Итак уберем использование `print_message` из `print_banner`

~~~bash
print_banner () {
  # Слово на баннере, первым аргументом функции, т.к. должна быть возможность это указать при вызове функции и скрипта
  MESSAGE=$1
  # Символ по бокам, указывается аргументом, т.к. он настраеваемый
  MARGIN_CHAR=$2
  # Количество повторении обрамления, также настраивается третим аргументом функции
  MARGIN_LENGTH=$3
  # Количество пробелов в разделителе.
  PADDING_LENGTH=$4
  # Символ разделитель обрамления и слова. Используется пробле, без возможности настраить.
  PADDING_CHAR=" "

  # Генерируем край
  MARGIN="$( repeat_char "${MARGIN_CHAR}" ${MARGIN_LENGTH} )"
  # Генерируем разделитель
  PADDING="$( repeat_char "${PADDING_CHAR}" ${PADDING_LENGTH} )"

  # Вычисляем длину заголовка, в баше это делает диез после фигурной скобки
  LABEL_LENGTH=${#MESSAGE}

  # Вычисляем длину линий сверху/снизу
  # Матемитические операии делаются в баше при помощи двух круглых скобок
  BORDER_LENGTH=$(( LABEL_LENGTH + MARGIN_LENGTH * 2 + PADDING_LENGTH * 2 ))

  # Печатаем баннер
  repeat_char "${MARGIN_CHAR}" ${BORDER_LENGTH}
  repeat_char "${MARGIN_CHAR}" ${BORDER_LENGTH}
  echo "${MARGIN}${PADDING}${MESSAGE}${PADDING}${MARGIN}"
  repeat_char "${MARGIN_CHAR}" ${BORDER_LENGTH}
  repeat_char "${MARGIN_CHAR}" ${BORDER_LENGTH}
}
~~~

Проверяем

~~~bash
print_banner "Bonsoir" '*' 3 3
~~~

Работает так же. Отлично. Теперь надо считать аргумента скрипта чтоб передать их в функцию `print_banner`.

По ссылке что я тебе дал, да и из задания ты должен был заметить что есть два вида аргументов у скриптаю:
- ключи: выглядят как буквы, перед которыми ставится тире (`-h`, `-mt` и т.д.). Они служат для управления сценариями.
- параметры: обычные буквы (`Bonsoir`, `123` и т.д.). Они служат для передачи в скрипт данные. 

Ключи командной строки могут быть сами по себе. Например `-h`, есть почти во всех скриптах,
запускает сценарий выдачи помощи по команде.

Могут быть ключи за которым идут параметры, от одного и более параметра.
Используется для включения определенного сценарии, а параметры уточняют настроки этого сценария.
Например: `-f long`, `-f long pretty`. Ты мог это видеть читая help других команд.
Иногда используют другой формат для ключей с параметрами, например `-format=long`.
Все зависит как их обработать в скрипте, можно даже поддерживать оба для удобства.
Иногда (ты может встречал), ключи имеют короткую и длинную форму одновременно,
например `-h, --help`, `-f, --format`.

Часто скрипт имеет и ключи (с парамтрами для ключа или без) и параметры одновременно,
обычно используют специальную комбинацию символов `--` указать где заканчиваются ключи и начинаются параметра.
Может быть другая комбинация, это зависит как обрабатываются аргументы командной строки в саммо скрипте,
для интерпритатора bash все что передано при запуске скрипта это одна большая строка.
Это специальная комбинацию символов помогает правильно обработать введенные данные в командную строку.

Например `script.sh -f foo bar`, как понять `foo` отдельным параметром, или параметром для ключя `-f`?
А `bar` это отдельный параметр или тоже относится к `-f`? Ведь ключи могут требовать несколько параметров для себя.

А вот уже `script.sh -f foo -- bar`, так уже более понятно, `foo` это параметр для ключа `-f`,
а `bar` уже отдельный параметр.

Если я правильно выразился ты должен был понять что вот например это:

- `script.sh bar -- -f foo` 
- `script.sh -f foo -- bar -- -b baz`

Так тоже может быть, есть разделитель `--`, который указывает скрипту где начинаются ключи, где параметры.
Но так никто не делает, уже устоявшиеся **best practice**, что сначало идут ключи, потом разделитель и потом параметры,
чтоб обработчик аругментов командной строки не стал монстром в миллион сртрочек кода.

Как я уже упоминал чуть раньше, интерпритатор пониает все переданные аргументы командной строки как одну большую строку,
обработать которую нужно самомстоятельно в скрипте. Для этого есть системные аргументы:

- `$*` - так можно прочитать всю строку
- `$@` - так можно прочитать всю строку разбитую на части по побелам.
- `$1`, `$2`, `${10}` - это первый, второй и десятый аргумент из того же списка разделенного пробелами.
- `$#` - количество аргументов

На примере при запуске `script.sh -f long -- Bonsoir`

~~~bash
# Выдаст `-f long -- Bonsoir`
echo $*
# Выдаст тоже `-f long -- Bonsoir`
echo $@
# Выдаст 4 (интересно, ты их может в примере насчитать?)
echo $#

# Но выдаст все туже одну строку
# -f long -- Bonsoir
for param in "$*" ; do
  echo ${param}
done

# А это уже распечатает 4 строки
# -f
# long
# --
# Bonsoir
for param in "$@" ; do
  echo ${param}
done
~~~ 

Интересно после прочтения этого ты сможешь без проверки сказать что будет в `$3` и того же примера `-f long -- Bonsoir`?

Если ты ответил `--` то отлично.

Вернемся к заданию, из командной строки мы можем получить:
`-h` - ключ включающий сценарий помощи 
`-mt car` - ключ и параметр для него, для символа обрамления
`-mg nombre` - ключ и параметр для него, кол-во для контура
`-ep nombre` -  ключ и параметр для него, кол-во для разделителя
`message` - заголовок банера, оыбчный параметр, без ключа. Как говорили выше чтоб отделить его от ключей будем использовать `--`

Все это усложняется тем что ключе могут передваться в разном порядке (ну кроме `message` который должен быть после ключей).
Другими словами исользовать `$1`, `$2` не очень нам поможет.
Что делать?

Верно, будем испольщовать цикл, и цикически передвигаться по каждому аргументу команной строки, проверяя что с ним делать.
Для цикла мы знаем что есть `for`, `while` (разницу и предназначение помним?)
Для проверок у нас `if`, `case` (разницу и предназначение помним?).

Для начало определим переменные и их значения по умолчанию

~~~bash
# Заголовок банера. Переменная куда запишем то что пришло из командной строки
MESSAGE=""
# Символ для обрамления. Переменная куда запишем то что пришло из командной строки. По умолчанию "*".
# Помним для одинарныые ковычки для спец символов.
MARGIN_CHAR='*'
# Длина обрамления. Переменная куда запишем то что пришло из командной строки. По умолчанию 3.
MARGIN_LENGTH=3
# Длина отступа между обрамлением и заголовком. Переменная куда запишем то что пришло из командной строки. По умолчанию 3.
PADDING_LENGTH=3
~~~

Уже можно попробовать

~~~bash
print_banner "${MESSAGE}" "${MARGIN_CHAR}" ${MARGIN_LENGTH} ${PADDING_LENGTH}
~~~

Но нам надо обработать аргументы командной строки. Как говорили будем использовать цикл с проверками внутри.

Какой цикл выбрать? `for` или `while`? У каждого есть свое предназначение, один удобен в одной задаче, другой в другой.
А в некоторых задачах вообще нельзя использовать тот или иной.

Возьмем `for`.

~~~bash
for PARAM in $@ ; do
  # тут будут проверки ${PARAM}
  # если ${PARAM} это -h то нужно вывести справку
  # если ${PARAM} это -mt то нужно записать следующий за ним параметр в MARGIN_LENGTH
done
~~~

Но стоп, как на музнать следующий параметр? У нас есть только текущий.
Мы не знаем следующий. Это трудности, значит надо попробовать `while`.

Циклу `while` нужна условие при котором цикл прекращается. Что это омжет быть?
Ранее мы сказали что для обработки командной строки, мы должны пройти по всем аргументам.
Если перефразировать, то нам нужно вытаскивать из аргументов один, проверять его,
тем самым список оставшихся уменьшается пока не законцится.
Вот и условие для `while` - пока есть хоть один аргумент.

Тут ты наверное подофигел от всех этих выводов. Это приходит с опытом, причем не надо годы.
После этого задания, который вам дал препод ты уже должен это запомнить как делается.
Тем более что обработка аргументов командной строки делается чуть ли ни в каждой скрипте.
Так что препод не зря вам это дает.

Итак, цикл `while`. Берем первый аргумент, делаем проверки, после как проверили его, выбрасили из списка аргументов.
Команда `shift` выбрасывает $1 и передвигает аргумента: $2 становится $1, $3 становится $2 и так далее.

~~~bash
while [ -n "$1" ] ; do
  # тут будут проверки $1
  
  # после как $1 проверили выбрасим его и повторим цикл
  shift
done
~~~

Проверка $1 можно сделать `if` или `case`.
Т.к. у нас может быть `-h`, `-mt`, `-mg`, `-ep`, `--`, ух это 5 условий.
Писать пять `if`? Верно нет, для этого лучше подхожит `case`.


~~~bash
# Обрабатываем ключи
while [ -n "$1" ] ; do
  # тут будут проверки $1
  case "$1" in
    # При `--` завершаем цикл, т.к. этот параметр означает что закончились ключи и дальше остаются только параметры.
    # завершения цикл при помощи команды `break`.
    # Внимание сейчас в аргументах `-- другие параметры`, а нам `--` не нужен, делаем `shift` до `break`,
    # чтобы выбрасить его.
    --) shift ; break ;;
    # При `-h` показать справку. Можно праму тут написать echo "bla bla", а можно вынести в функцию.
    # exit при этом сценарии сразу, не выполняя дальше скрипт.
    -h) show_help ; exit ;;
    # Если $1 равен `-mt`, то следующий $2 будет символ который нам нужен в MARGIN_CHAR.
    # После того как мы использовали $1 и $2 они нам не нужны больше, т.е. shift их оба из аргументов.
    -mt) MARGIN_CHAR=$2 ; shift ; shift ;;
    # Если $1 равен `-mg`, то следующий $2 будет цифра который нам нужен в PADDING_LENGTH.
    # После того как мы использовали $1 и $2 они нам не нужны больше, т.е. shift их оба из аргументов.
    -mg) PADDING_LENGTH=$2 ; shift ; shift ;;
    # Если $1 равен `-ep`, то следующий $2 будет цифра который нам нужен в MARGIN_LENGTH.
    # После того как мы использовали $1 и $2 они нам не нужны больше, т.е. shift их оба из аргументов.
    -ep) MARGIN_LENGTH=$2 ; shift ; shift ;;
    # Если ничего из этого, значит это message который не был разделен `--` либо другой неизвестный ключ.
    *)
      # Проверяем не является ли $1 ключе (нет ли тире в начале). Если это не известный ключ показываем ошибку и выходм из программы.
      assert_not_parameter $1 || exit 1
      # Если же нет, то там message, завершаем цикл т.к. дальше проверок не нужно.
      break ;;
  esac
done


# Обрабатываем параметры после ключей.
# Ключи мы уже все выбрасили при помомощи shift в while,
# т.е. в аргументах командной строки (`$@`, `$*`) остались толко параметры.
# По условию задания все это является сообщением для баннера.
MESSAGE=$*
~~~

Теперь мы обработали ключи и параметры команной строки, все настройки записали в нужные переменные,
можем вызвать нашу функцию `print_banner` с нужными аргументами

~~~bash
print_banner "${MESSAGE}" "${MARGIN_CHAR}" ${MARGIN_LENGTH} ${PADDING_LENGTH}
~~~

Пробуем: `./banniere.sh -mt '+' -mg 20 -ep 10  -- Bonsoir Victor`

~~~
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++          Bonsoir Victor          ++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
~~~

Мы использовали функцию `show_help`, надо бы ее добавить

~~~bash
show_help () {
  echo "banniere [paramètres mot-clefs] message"
  echo ""
  echo "-h         : affiche ce message"
  echo "-mt car    : caractère du motif du contour"
  echo "-mg nombre : marge entre le message et le contour"
  echo "-ep nombre : épaisseur du motif"
  echo "--         : pour indiquer la fin des paramètres positionnels"
  echo "message : le message à afficher; doit être le dernier paramètre"
}
~~~

Пробуем: `./banniere.sh -h`

Мы использовали функцию `assert_not_parameter`, ой чуть ниже в части с ваилидацией.

Скрипт в принципе работает. Правда золотое праивло програмирования, даже я бы сказал закон:
"То что приходит от пользователя надо проверить".

Проверяя какие данные вводит пользователь, можно увидеть что нужно проверить:
- `-mt` имеет длину в один символ
- `-mg` и `-ep` это цифры больше 0
- `message` слово длинее 0

Предлагаю создать функции для каждой проверки, тем более что на ичсло надо проверить две переменные.

~~~bash
assert_single_char () {
  SUBJECT=$1
  VALUE=$2
  if (( ${#VALUE} != 1 )) ; then
    # Показываем ошибку
    echo "doit retourner une erreur: ${SUBJECT}"
    echo ""
    show_help
    # Используем return чтоб вернуть код ошибки
    return 1
  fi
}

assert_positive_number () {
  SUBJECT=$1
  VALUE=$2
  if (( VALUE <= 0 )) ; then
    # Показываем ошибку и выходим из программы
    echo "doit retourner une erreur: ${SUBJECT}"
    echo ""
    show_help
    # Используем return чтоб вернуть код ошибки
    return 1
  fi
}

assert_length () {
  SUBJECT=$1
  VALUE=$2
  if (( ${#VALUE} < 1 )) ; then
    # Показываем ошибку и выходим из программы
    echo "doit retourner une erreur: ${SUBJECT}"
    echo ""
    show_help
    # Используем return чтоб вернуть код ошибки
    return 1
  fi
}

assert_not_parameter () {
  VALUE=$1
  if [[ ${VALUE:0:1} = "-" ]] ; then
    # Показываем ошибку и выходим из программы
    echo "doit retourner une erreur: paramètre ${VALUE} invalide"
    echo ""
    show_help
    # Используем return чтоб вернуть код ошибки
    return 1
  fi
}
~~~

Проверяем пользовательские данные

~~~bash
assert_single_char "caractère du motif du contour" "${MARGIN_CHAR}" || exit 1
assert_positive_number "marge invalide" ${MARGIN_LENGTH} || exit 1
assert_positive_number "épaisseur invalide" ${PADDING_LENGTH} || exit 1
assert_length "message" "${MESSAGE}" || exit 1
~~~

Вот и все. Если я ничего не пропустил из задания.
