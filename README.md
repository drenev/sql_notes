## Конспект по Oracle SQL
*Этот конспект основан на видеокурсе Заура Трегулова*

### Главы
1. [Введение в SQL](#1)
2. [Знакомство с SELECT](#2)
3. [Selection, операторы, ORDER BY](#3)
4. [SINGLE-ROW функции](#4)
5. [CONVERSION, GENERAL и CONDITIONAL функции](#5)
6. [GROUP функции, ORDER BY, HAVING](#6)
7. [JOIN (объединение)](#7)
8. [SUBQUERY (подзапрос)](#8)
9. [SET операторы (операторы множеств)](#9)
10. [DML команды](#10)
11. [DDL  часть 1. Работа с таблицами](#11)
12. [DDL  часть 2. Понятия CONSTRAINT и INDEX](#12)
13. [DDL  часть 3. VIEW, SYNONYM, SEQUENCE](#13)
14. [Разное](#14)

## <a name="1"></a> Введение в SQL
### Начальные определения
***База данных** - совокупность данных, которые хранятся по определённым правилам и используются для удовлетворения информационных потребностей пользователей*

***Реляционная база данных** - БД, где вся информация хранится в виде таблиц*

***USER** - лицо, которое может подключиться к БД (log on process)*

***DataBase схема** - все объекты в БД, которые принадлежат одному user-у*

Чтобы вызвать таблицу не из своей DB схемы, к ней нужно обратиться по полному имени *schema.table.name*

### SQL команды
Этот урок рассказывает о четырёх основных группах команд
1. **DML** - Data Manipulation Language *работают с данными в таблице*
   - `SELECT` *выборка из таблицы*
   - `INSERT` *вставляет строки в таблицу*
   - `UPDATE` *изменяет данные в талице*
   - `DELETE` *удаляет строки*
   - `MERGE` *объединяет в себе функционал трёх предыдущих команд*
2. **DDL** - Data Definition Language *работают со структурой таблицы*
   - `CREATE` *создаёт таблицу*
   - `ALTER` *добавляет столбец*
   - `DROP` *удаляет объекты*
   - `RENAME` *меняет название столбца*
   - `TRUNCATE` *удаляет все данные из таблицы*
3. **TCL** - Transaction Control Language *контроль версий*
   - `COMMIT`
   - `ROLLBACK`
   - `SAVEPOINT`
4. **DCL** - Data Control Language *контроль доступа в БД*
   - `GRAND` *даёт права*
   - `REVOKE` *забирает права*
### Типы данных и понятие NULL
#### Основные типы данных:
- **INTEGER** *целое число*
- **NUMBER(p,s)** *число с плавающей точкой, где **p** - максимальное количество символов в числе, а **s** - количество символов в дробной части. Может быть целым, если не указать параметры*
- **CHAR(length)** *строка с фиксированной длиной, не может быть короче, чем **length**. **этот тип устарел***
- **VARCHAR2(size)** *строка с переменной длиной (может быть короче, чем size) **использовать вместо CHAR***
- **DATE** *Год, месяц, день, час, минута, секунда*
- **TIMESTAMP(f)** *то же, что DATE, только содержит информацию и о долях секунды (**f** - количество цифр после запятой)*

#### NULL - отсутствие данных
0 или пробел - это не NULL. NULL не занимает место в памяти.
Результат арифметических операций с NULL - всегда NULL

*NULL is NULL*

### DESCRIBE
Команда DESCRIBE позволяет получить служебную информацию о таблице: о колонках и их типах данных

**DESC**RIBE SCHEMA.**TABLE_NAME**

## <a name="2"></a> Знакомство с SELECT
### Три фундаментальные концепции:
- **PROJECTION** *выбор столбцов из таблицы*
- **SELECTION** *выбор строк из таблицы*
- **JOINING** *объединение таблицы*

### Базовый синтаксис:
```SQL
SELECT * FROM table;
```

```SQL
SELECT column(s) FROM table;
```

### *SELECT statement не меняет данные в БД.*
### ОПЕРАТОР *(ключевое слово)* DISTINCT
DISTINCT - выводит уникальные строки из столбца
#### Синтаксис:
```SQL
SELECT DISTINCT column(s) FROM table;
```

Если в команде передать несколько столбцов, то DISTINCT выведет уникальные сочетания этих столбцов

### Выражения. Expressions.
```SQL
SELECT column(s), expression(s) FROM table;
```

С числовыми данными в столбцах можно проводить арифметические операции.

Текстовые данные можно объединять при помощи конкатенации **||**.

### Alias
Alias - альтернативное имя для столбца или целого выражения.
* при помощи ключевого слова **AS**, либо после пробелом после выражения/столбца.*

```SQL
SELECT column(s) as alias, expression(s) alias2 FROM table;
```

### Таблица DUAL
Таблица DUAL - пустая таблица, которую можно использовать для вывода своих выражений.
### Оператор q
Чтобы одинарная кавычка не ломала строку, можно использовать два метода:
- Поставить одинарную кавычку два раза: ```''```
- Использовать оператор Quote (q)

```SQL
SELECT q'<It's my life> from dual;
```
Символ **<** в данном случае используется как delimiter (разделитель). Вместо него можно использовать другой символ, например **$**
## <a name="3"></a> Selection, операторы, ORDER BY
### Концепция SELECTION
SELECTION Restriction - ограничение в выводе строк

Оператор **WHERE** позволяет выбрать строки соответствующие условию *condition*

Изученный синтаксис:
```SQL
SELECT *|{DISTINCT column(s) alias, expressions(s)}
FROM table 
WHERE condition(s);
```
### Математические операторы сравнения
```SQL
=
>
<
>=
<=
!=, <>
```
### Другие операторы сравнения
```SQL
BETWEEN (value1, value2)
IN (list)
IS NULL ()
LIKE (sting, _, %)
```
### Логические операторы
```SQL
AND
OR
NOT
```
### Приоритет операторов
|Приоритетность| Оператор |
| :----------: |------------------|
|1|```()```|
|2|```/ *```|
|3|```+ -```|
|4|&#124;&#124;|
|5|```=, >, <, >=, <=```|
|6|```[NOT] LIKE, IS [NOT] NULL, [NOT] IN```|
|7|```[NOT] BETWEEN```|
|8|```!= <>```|
|9|```NOT```|
|10|```AND```|
|11|```OR```|


### ORDER BY
Сортирует вывод SELECT
```sql
ORDER BY {col(s) | expr(s) | numeric_position}
{ASC|DESC} {NULLS FIRST|LAST};
```
Пример: 
```SQL
SELECT first_name, salary FROM employees
ORDER by salary;
```
Ключ `ASC` - стандартная сортировка от меньшего к большему *(можно не писать)*

Ключ `DESC` - сортировка от большего к меньшему

```NULLS FIRST/LAST``` - определяет положение строк содержащих NULL в отсортированном выводе

Сортировать можно по нескольким строкам.

## <a name="4"></a> SINGLE-ROW функции
### Разновидности функций в SQL:
1. **Single-Row** *(одна строка на вводе, одна строка на выводе)*
   - Character *(функции, работающие с текстом)*
   - Numeric *(с числами)*
   - Date *(с датами)*
   - Conversion *(преобразовывают типы данных)*
   - General *(работают с NULL)*
2. **Multiple-Row** *(много строк на вводе, одна строка на выводе)*
   - про разновидности этих фунций урок пока не посмотрел

### Character functions *(функции, работающие с текстом)*:
   1. Case conversion functions *(работают с регистром символов)*
      - `LOWER(str)` *преобразует в маленький регистр*
      - `UPPER(str)` *преобразует в большой регистр*
      - `INITCAP(str)` *делает первую букву каждого слова заглавной*
   2. Character-manipulation functions (*редактируют содержимое строки*)
      - `CONCAT (str1, str2...)` *то же что и ||*
      - `LENGTH (str)` *возвращает длину строки*
      - `LPAD и RPAD(str, n, p)` *добавляют символ **p** к началу, либо концу строки столько раз, чтобы итоговая длина строки была равна **n***
      - `TRIM({trailing|leading|both} trimstring from str)` *срезает символ с начала|с конца|**и с начала и с конца**(по умолчанию). trimstring - 1 символ, который нужно срезать, необязательный параметр*
      - `INSTR(str, search_string, start_position, Nth_occurence)` *возвращает номер позиции **search_string** в **str**. может начинать поиск с **start_position**(number). **Nth_occurence**(number) позволяет найти второе и последующие появления **search_string** в тексте (по умолчанию функция возвращает номер первого появления)*
      - `SUBSTR(str, start_position, count_of_characters)` *возвращает часть **str**, начиная с **start_position**, длиной в **count_of_characters**, либо до конца строки(по умолчанию). если start_position>LENTH(str), то функция вернёт NULL. если **start_position** отрицательное, то отсчёт будет идти с права на лево*
      - `REPLACE(str, search_item, replacement_item)` *заменяет **search_item** в **str** на **replacement_item**, либо **на пустоту**(по умолчанию)*
   3. Numeric functions *(работают с числами)*
      - `ROUND(n, precision)` *округляет число **n** до **precision** цифр после запятой. если **precision** не указан - округление произойдёт до целого числа. при отрицательном **precision** функция округляет десятки(-1), сотни(-2), и т.д.*
      - `TRUNC(n, precision)` *то же, что и `ROUND`, только не округляет, а отсекает лишние символы*
      - `MOD (dividend, divisor)` *возвращает остаток от деления **dividend** на **divisor**. используется для разделения чётных и нечётных чисел*
   4. Date functions *(работают с датами)*
      - даты хранятся в виде числа, по умолчанию выводятся в том виде, который используется в системе (у меня это DD-Mon-RR, у автора курса DD.MM.RR). узнать свой формат даты можно в служебной таблице `nls_session_parameters`.
      - Если год в формате `RR ∈ [0-49]`, то речь идёт о текущем веке, если `RR ∈ [50-99]` - о предыдущем
      - `SYSDATE` *возвращает время сервера базы данных*
      - Арифметические операции с датами: `Date-Date=Number`, `Date-Number=Date`, `Date+Number=Date`, где **Number** - количество дней, может быть нецелым. Складывать, умножать и делить даты нельзя
      - `MONTHS_BETWEEN(start_date, end_date)` *возвращает количество месяцев между датами (может быть нецелым). чтобы узнать количество дней, нужно умножить на **31**(стандарт **Oracle**)*
      - `ADD_MONTHS(date, number_of_months)` *добавляет **number_of_months** к **date**. при отрицательном **number_of_months**, месяца будут отниматься. **number_of_months** не может быть дробным*
      - `NEXT_DATE(date, day_of_the_week)` *возвращает ближайший **day_of_the_week[1;7]** к **date**. **day_of_the_week** можно указать словом, каким конкретно, зависит от формата дат в системе(можно посмотреть в таблице `nls_session_parameters`)*
      - `LAST_DAY(date)` *возвращает последний день месяца*
      - `ROUND(date, date_precision_format)` *округление для дат. **date_precision_format** может быть `CC`- век; `YYYY`- год; `Q`- четверть, `MM`- месяц; `W`- неделя; `DD`- день; `HH`- час; `MI`- минута. **по умолчанию округляет до дня `DD`***
      - `TRUNC(date, date_precision_format)` *то же, что ROUND, только не округляет, а отсекает дату*