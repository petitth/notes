# Some algorithms in C Language

## Insertion sort

```c
/**
 * The insertion sort is efficient for small inputs.
 * If the data is mostly-sorted, it can be great too.
 * For bigger inputs, please consider quick sort or timsort.
 * @param x_array an array of integers
 * @param x_length the size of the array
 * complexity : worst case O (n^2), best case O(n)
 */
void insertion_sort_int(int x_array[], size_t x_length) {
    int i = 1, j, tmp;
    while ((size_t)i < x_length) {
        j = i;
        tmp = x_array[i];

        while(j >= 1 && tmp < x_array[j - 1]) {
            x_array[j] = x_array[j - 1];
            j--;
        }

        x_array[j] = tmp;
        i++;
    }
}
```

## Selection sort

```c
/**
 * The selection sort is efficient on small input, but performs worst than insertion sort.
 * Selection sort is noted for its simplicity and has performance advantages over more complicated algorithms
 * in certain situations, particularly where auxiliary memory is limited.
 * @param x_array an array of integers
 * @param x_length the size of the array
 * complexity : best and worst case O (n^2)
 */
void selection_sort_int(int x_array[], size_t x_length) {
    int temp, lowest;
    for(int i = 0; (size_t)i < x_length - 1; i++) {
        lowest = i;
        for (int j = i+1; (size_t)j < x_length; j++ ) {
            if ( x_array[j] < x_array[lowest]) {
                lowest = j;
            }
        }

        if (lowest != i) {
            temp = x_array[i];
            x_array[i] = x_array[lowest];
            x_array[lowest] = temp;
        }
    }
}
```

## Binary search

```c
/**
 * This function implements the binary search of an integer into a sorted array.
 * @param x_array a sorted array of integers
 * @param begin beginning index
 * @param end ending index
 * @param searched searched integer
 * @return the position of searched value in the array, or -1 if not found
 */
int binary_search_int(int x_array[], int begin, int end, int searched) {
    if (end < begin) {
        return -1;
    } else {
        int middle = begin + ( (end - begin + 1) / 2 );
        if (x_array[middle] == searched) {
            return middle;
        } else if (x_array[middle] < searched) {
            return binary_search_int(x_array, middle + 1, end, searched);
        } else {
            return binary_search_int(x_array, begin, middle - 1, searched);
        }
    }
}
```

## Fibonacci 

```c
int fibonacci(int x) {
    if (x == 0 || x == 1) {
        return x;
    } else {
        return fibonacci(x - 1) + fibonacci( x - 2);
    }
}
```

## Factorial

```c
int factorial(int x) {
    if (x < 1) {
        return 1;
    } else {
        return x * factorial(x - 1);
    }
}
```