# Array.prototype  quick sort

```typescript
declare global {
  interface Array<T> {
    quickSort(compareFn?: (a: T, b: T) => number): T[];
  }
}

Array.prototype.quickSort = function <T>(compareFn?: (a: T, b: T) => number): T[] {
  const defaultCompareFn = (a: T, b: T): number => {
    if (a < b) return -1;
    if (a > b) return 1;
    return 0;
  };

  const compare = compareFn || defaultCompareFn;

  const quickSortRecursive = (arr: T[]): T[] => {
    if (arr.length <= 1) return arr;

    const pivot = arr[0];
    const left = arr.slice(1).filter(item => compare(item, pivot) < 0);
    const right = arr.slice(1).filter(item => compare(item, pivot) >= 0);

    return [...quickSortRecursive(left), pivot, ...quickSortRecursive(right)];
  };

  return quickSortRecursive(this);
};

// 사용 예시
const numbers = [5, 3, 8, 4, 2];
console.log(numbers.quickSort()); // 출력: [2, 3, 4, 5, 8]

const strings = ["banana", "apple", "cherry"];
console.log(strings.quickSort((a, b) => a.localeCompare(b))); // 출력: ["apple", "banana", "cherry"]
```

prototype quick
