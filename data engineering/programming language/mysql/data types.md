# data types

data types in mysql can be broadly categorized into following

## integer

consist of negative, zero and positive integers

| data type | value range | storage consumed |
| --- | --- | --- |
| INT | broad | lower |
| BIGINT | broader | higher |

✨ **use case** : use `BIGINT` to accurately represent exceptionally large numbers

## decimals

include negative, zero and positive decimals

| data type | decimal places | storage consumed |
| --- | --- | --- |
| FLOAT | up to 7 | fixed |
| DOUBLE | up to 15 | fixed but higher |
| DECIMAL | configurable | depends on precision |

✨ **use case** : use `FLOAT` when rounding errors are acceptable

## string

refers to text data which must be enclosed with single quotes

| data type | maximum length | storage consumed |
| --- | --- | --- |
| CHAR | lowest | fixed |
| VARCHAR | medium | depends on length |
| TEXT | highest | depends on length |

✨ **use case** : 

- use `CHAR` when speed is more important than space
- use `VARCHAR` for flexible metadata overhead

## date and time

another data type that requires single quotes around data

| data type | information | storage consumed | value range |
| --- | --- | --- | --- |
| TIME | hour minute second | smallest | fixed |
| DATE | year month date | small | broad |
| DATETIME | date + time | medium | broad |
| TIMESTAMP | date + time + timezone | large | narrow |

✨ **use case** : 

- use `TIMESTAMP` for references across time zones
- timezone is defined by offset `+hh:mm` or `-hh:mm` after date and time
- timestamp without offset are inferred to have same timezone as server

## boolean

no direct support from mysql. however, `TINYINT` is used under the hood where

- 1 represents `TRUE`
- 0 represents `FALSE`

✨ **use case** : data which represents a flag or result of boolean expression