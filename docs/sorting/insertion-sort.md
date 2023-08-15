# Insertion Sort

Insertion sort's main concept is to iterate through the array, and for each element, insert it into the correct position in the sorted subarray.

## Implementation

=== "Pseudocode"
    In Pseudocode, we can implement this as follows:

    ```pseudocode
    insertionSort(list) {
	for each element in the list:
		while the current element is less than the previous element:
			swap the current element and the previous element
    }
    ```

=== "Python"
    In Python, we can implement this as follows:

    ```python
    def insertion_sort(arr):
        for i in range(1, len(arr)):
            j = i
            while j > 0 and arr[j] < arr[j - 1]:
                arr[j], arr[j - 1] = arr[j - 1], arr[j]
                j -= 1
    ```

## Runtime Analysis
In order to analyze the runtime of insertion sort, we need to consider the number of elements in the array, $N$ and the number of inversion pairs, $K$.

Because insertion sort iterates through every element in the list, insertion sort must do a minimum of $\Theta(N)$ work.
If there are inversion pairs, insertion sort must swap elements, which takes $\Theta(1)$ time.
This mean for all inversion pairs, insertion sort must do $\Theta(K)$ work.

Therefore, the runtime of insertion sort is $\Theta(N + K)$.

### Best Case
The best case occurs when the array is already sorted. In this case, we do not need to swap any elements, and the runtime is $\Theta(N)$.

### Worst Case
The worst case occurs when the array is sorted in reverse order. In this case, we need to swap every element with the previous element, and the runtime is $\Theta(N^2)$.
