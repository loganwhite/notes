## `memset()` can only set memory block in the unit of `char` (byte).
If people want to set inital values for each element in an array, and the size of the element is bigger than 1 byte (8bits, the size of char), then the set value can be incorrect.
```c
uint32_t array[100];
memset(array, 1, sizeof(uint32_t)* 100);
```
The value of each element in `array` is `16843009`, which is `00000001000000010000000100000001` in binary.
