## `memset()` can only set memory block in the unit of `char` (byte).
If people want to set inital values for each element in an array, and the size of the element is bigger than 1 byte (8bits, the size of char), then the set value can be incorrect.
```c
uint32_t array[100];
memset(array, 1, sizeof(uint32_t)* 100);
```
The value of each element in `array` is `16843009`, which is `00000001000000010000000100000001` in binary.

## A lot of the time in OJ (Oneline Judge) systems, multiple round of inputs and outputs are required. In C, when the used functions are different in terms of reading individual variables and a single line of string (including blanks). 

The following code shows the code snippet reading individual variables and whole lines.

```c
while (scanf("%s",str) != EOF) {
  do_something(str);
}
```
```c
while (gets(str)) {
  do_something(str);
}
```
