# 병합정렬 mergesort

```typescript
function mergeSort(arr: number[]): number[] {
  // 배열의 크기가 1 이하라면 이미 정렬된 상태
  if (arr.length <= 1) {
    return arr;
  }

  // 중간 지점을 기준으로 배열 분할
  const mid = Math.floor(arr.length / 2);
  const left = arr.slice(0, mid);
  const right = arr.slice(mid);

  // 재귀적으로 분할된 배열 정렬 후 병합
  return merge(mergeSort(left), mergeSort(right));
}

function merge(left: number[], right: number[]): number[] {
  const sortedArray: number[] = [];
  let i = 0; // 왼쪽 배열의 인덱스
  let j = 0; // 오른쪽 배열의 인덱스

  // 두 배열을 비교하면서 작은 값을 병합 배열에 추가
  while (i < left.length && j < right.length) {
    if (left[i] <= right[j]) {
      sortedArray.push(left[i]);
      i++;
    } else {
      sortedArray.push(right[j]);
      j++;
    }
  }

  // 남은 요소들을 병합 배열에 추가
  while (i < left.length) {
    sortedArray.push(left[i]);
    i++;
  }

  while (j < right.length) {
    sortedArray.push(right[j]);
    j++;
  }

  return sortedArray;
}

// 테스트
const unsortedArray = [38, 27, 43, 3, 9, 82, 10];
const sortedArray = mergeSort(unsortedArray);
console.log("정렬 결과:", sortedArray);
```

mergsort
