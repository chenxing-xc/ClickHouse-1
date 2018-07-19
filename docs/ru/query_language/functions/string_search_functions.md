# Функции поиска в строках

Во всех функциях, поиск регистрозависимый.
Во всех функциях, подстрока для поиска или регулярное выражение, должно быть константой.

## position(haystack, needle)
Поиск подстроки `needle` в строке `haystack`.
Возвращает позицию (в байтах) найденной подстроки, начиная с 1, или 0, если подстрока не найдена.

Для поиска без учета регистра используйте функцию `positionCaseInsensitive`.

## positionUTF8(haystack, needle)
Так же, как `position`, но позиция возвращается в кодовых точках Unicode. Работает при допущении, что строка содержит набор байт, представляющий текст в кодировке UTF-8. Если допущение не выполнено - то возвращает какой-нибудь результат (не кидает исключение).

Для поиска без учета регистра используйте функцию `positionCaseInsensitiveUTF8`.

## match(haystack, pattern)
Проверка строки на соответствие регулярному выражению pattern. Регулярное выражение re2.
Возвращает 0 (если не соответствует) или 1 (если соответствует).

Обратите внимание, что для экранирования в регулярном выражении, используется символ `\` (обратный слеш). Этот же символ используется для экранирования в строковых литералах. Поэтому, чтобы экранировать символ в регулярном выражении, необходимо написать в строковом литерале \\ (два обратных слеша).

Регулярное выражение работает со строкой как с набором байт. Регулярное выражение не может содержать нулевые байты.
Для шаблонов на поиск подстроки в строке, лучше используйте LIKE или position, так как они работают существенно быстрее.

## extract(haystack, pattern)
Извлечение фрагмента строки по регулярному выражению. Если haystack не соответствует регулярному выражению pattern, то возвращается пустая строка. Если регулярное выражение не содержит subpattern-ов, то вынимается фрагмент, который подпадает под всё регулярное выражение. Иначе вынимается фрагмент, который подпадает под первый subpattern.

## extractAll(haystack, pattern)
Извлечение всех фрагментов строки по регулярному выражению. Если haystack не соответствует регулярному выражению pattern, то возвращается пустая строка. Возвращается массив строк, состоящий из всех соответствий регулярному выражению. В остальном, поведение аналогично функции extract (по прежнему, вынимается первый subpattern, или всё выражение, если subpattern-а нет).

## like(haystack, pattern), оператор haystack LIKE pattern
Проверка строки на соответствие простому регулярному выражению.
Регулярное выражение может содержать метасимволы `%` и `_`.

`%` обозначает любое количество любых байт (в том числе, нулевое количество символов).

`_` обозначает один любой байт.

Для экранирования метасимволов, используется символ `\` (обратный слеш). Смотрите замечание об экранировании в описании функции match.

Для регулярных выражений вида `%needle%` действует более оптимальный код, который работает также быстро, как функция `position`.
Для остальных регулярных выражений, код аналогичен функции match.

## notLike(haystack, pattern), оператор haystack NOT LIKE pattern
То же, что like, но с отрицанием.