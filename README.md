# Сортировки Массивов

## Введение
Сортировка массивов — это фундаментальная операция в информатике, используемая для упорядочивания элементов в определённом порядке. Эффективность выбранного алгоритма сортировки критически влияет на производительность программ, особенно при работе с большими объёмами данных. В этом реферате мы рассмотрим и сравним три различных алгоритма: быструю сортировку (Quick Sort), сортировку слиянием (Merge Sort) и пузырьковую сортировку (Bubble Sort)

## Цель
Сравнить три различных алгоритма: быструю сортировку (Quick Sort), сортировку слиянием (Merge Sort) и пузырьковую сортировку (Bubble Sort). Мы сравним их временную сложность и время работы на массиве, содержащем 1 000 000 чисел. 

## Описание сравнения
Практическое Сравнение Производительности (Массив из 1 000 000 чисел). Для каждого алгоритма, Мы сгенерируем одномерный массив из 1 000 000 случайных целых чисел. Далее отсортируем его с помощью

Для всех примеров будем использовать Java.

## Код Сравнения
```
import java.util.Arrays;
import java.util.Random;

class App {
    private static long measureTime(Runnable task) {
        long startTime = System.nanoTime();
        task.run();
        long endTime = System.nanoTime();
        return (endTime - startTime) / 1_000_000; // Возвращаем в миллисекундах
    }

    public static void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            int pi = partition(arr, low, high);
            quickSort(arr, low, pi - 1);
            quickSort(arr, pi + 1, high);
        }
    }

    private static int partition(int[] arr, int low, int high) {
        int pivotIndex = low + (high - low) / 2;
        int pivot = arr[pivotIndex];

        int temp = arr[pivotIndex];
        arr[pivotIndex] = arr[high];
        arr[high] = temp;

        int i = (low - 1);
        for (int j = low; j < high; j++) {
            if (arr[j] <= pivot) {
                i++;
                temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }

        temp = arr[i + 1];
        arr[i + 1] = arr[high];
        arr[high] = temp;
        return i + 1;
    }

    public static void mergeSort(int[] arr, int l, int r) {
        if (l < r) {
            int m = (l + r) / 2;
            mergeSort(arr, l, m);
            mergeSort(arr, m + 1, r);
            merge(arr, l, m, r);
        }
    }

    private static void merge(int[] arr, int l, int m, int r) {
        int n1 = m - l + 1;
        int n2 = r - m;

        int[] L = new int[n1];
        int[] R = new int[n2];

        for (int i = 0; i < n1; ++i)
            L[i] = arr[l + i];
        for (int j = 0; j < n2; ++j)
            R[j] = arr[m + 1 + j];

        int i = 0, j = 0;
        int k = l;
        while (i < n1 && j < n2) {
            if (L[i] <= R[j]) {
                arr[k] = L[i];
                i++;
            } else {
                arr[k] = R[j];
                j++;
            }
            k++;
        }

        while (i < n1) {
            arr[k] = L[i];
            i++;
            k++;
        }

        while (j < n2) {
            arr[k] = R[j];
            j++;
            k++;
        }
    }

    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        boolean swapped;

        for (int i = 0; i < n - 1; i++) {
            swapped = false;
            for (int j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                }
            }
            if (!swapped) {
                break;
            }
        }
    }

    public static void main(String[] args) {
        int arraySize = 1_000_000;
        Random random = new Random();

        int[] originalArray = new int[arraySize];
        for (int i = 0; i < arraySize; i++) {
            originalArray[i] = random.nextInt(1_000); // Числа от 0 до 999_999
        }

        System.out.println("Начало сравнения сортировок для " + arraySize + " чисел...");
        System.out.println("----------------------------------------------------");

        int[] arrForQuickSort = Arrays.copyOf(originalArray, arraySize);
        long durationQuick = measureTime(() -> quickSort(arrForQuickSort, 0, arrForQuickSort.length - 1));
        System.out.println("Быстрая Сортировка: " + durationQuick + " мс");

        int[] arrForMergeSort = Arrays.copyOf(originalArray, arraySize);
        long durationMerge = measureTime(() -> mergeSort(arrForMergeSort, 0, arrForMergeSort.length - 1));
        System.out.println("Сортировка Слиянием: " + durationMerge + " мс");

        int[] arrForBubbleSort = Arrays.copyOf(originalArray, arraySize);
        long durationBubble = measureTime(() -> bubbleSort(arrForBubbleSort));
        System.out.println("Пузырьковая Сортировка: " + durationBubble + " мс");

        System.out.println("----------------------------------------------------");
    }
}
```

## Реультат работы
```
Начало сравнения сортировок для 1000000 чисел...
----------------------------------------------------
Быстрая Сортировка: 291 мс
Сортировка Слиянием: 88 мс
Пузырьковая Сортировка: 1118287 мс
----------------------------------------------------
```

## Итог

В таблице ниже привел результаты работы:

| Алгоритм               | Временная Сложность | Время работы для N = 1 000 000\* |
| ---------------------- | ------------------- | -------------------------------- |
| Быстрая Сортировка     | O(NlogN)            | 291 мс                           |
| Сортировка Слиянием    | O(NlogN)            | 88 мс                            |
| Пузырьковая Сортировка | O(N\*\*2)           | 1118287 мс                       |

\* - Запуск производился на компьютере с процессором M2 Pro и 32gb ОЗУ


В итоге получилось успешно сравнить три вида сортировки на 1 000 000 параметров. Самой быстрой окзаалась Сортировка Слиянием (88мс), почти столько же времени выполнялась Быстрая Сортировка (291 мс) и самой долгой оказалась Пузырьковая Сортировка (1118287 мс)
