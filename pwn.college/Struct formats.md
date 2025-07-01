
| Format | C Type             | Python type       | Standard size | Notes    |
| ------ | ------------------ | ----------------- | ------------- | -------- |
| `x`    | pad byte           | no value          |               | (7)      |
| `c`    | char               | bytes of length 1 | 1             |          |
| `b`    | signed char        | integer           | 1             | (1), (2) |
| `B`    | unsigned char      | integer           | 1             | (2)      |
| `?`    | _Bool              | bool              | 1             | (1)      |
| `h`    | short              | integer           | 2             | (2)      |
| `H`    | unsigned short     | integer           | 2             | (2)      |
| `i`    | int                | integer           | 4             | (2)      |
| `I`    | unsigned int       | integer           | 4             | (2)      |
| `l`    | long               | integer           | 4             | (2)      |
| `L`    | unsigned long      | integer           | 4             | (2)      |
| `q`    | long long          | integer           | 8             | (2)      |
| `Q`    | unsigned long long | integer           | 8             | (2)      |
| `n`    | `ssize_t`          | integer           |               | (3)      |
| `N`    | `size_t`           | integer           |               | (3)      |
| `e`    | (6)                | float             | 2             | (4)      |
| `f`    | float              | float             | 4             | (4)      |
| `d`    | double             | float             | 8             | (4)      |
| `s`    | char[]             | bytes             |               | (9)      |
| `p`    | char[]             | bytes             |               | (8)      |
| `P`    | void*              | integer           |               | (5)      |
