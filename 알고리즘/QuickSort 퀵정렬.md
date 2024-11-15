# QuickSort 퀵정렬

### 첫번째 요소가 피벗

```typescript
function quickSort(arr: number[]): number[] {
  // 배열의 길이가 1 이하라면 이미 정렬된 상태
  if (arr.length <= 1) {
    return arr;
  }

  // 기준점(pivot)을 배열의 첫 번째 요소로 설정
  const pivot = arr[0];
  const left: number[] = [];
  const right: number[] = [];

  // pivot을 제외한 나머지 요소를 비교하여 분할
  for (let i = 1; i < arr.length; i++) {
    if (arr[i] < pivot) {
      left.push(arr[i]); // pivot보다 작으면 left에 추가
    } else {
      right.push(arr[i]); // pivot보다 크거나 같으면 right에 추가
    }
  }

  // 왼쪽과 오른쪽 배열을 각각 정렬 후 pivot과 병합
  return [...quickSort(left), pivot, ...quickSort(right)];
}

// 테스트
const unsortedArray = [38, 27, 43, 3, 9, 82, 10];
const sortedArray = quickSort(unsortedArray);
console.log("정렬 결과:", sortedArray);
```

# 가운데 요소가 피벗일때

```typescript
function quickSort(arr: number[]): number[] {
  if (arr.length <= 1) return arr;

  // 가운데 요소를 피벗으로 선택
  const mid = Math.floor(arr.length / 2);
  const pivot = arr[mid];

  const left: number[] = [];
  const right: number[] = [];

  // 피벗을 제외한 배열을 비교하여 분할
  for (let i = 0; i < arr.length; i++) {
    if (i !== mid) { // 피벗 요소는 제외
      if (arr[i] < pivot) {
        left.push(arr[i]);
      } else {
        right.push(arr[i]);
      }
    }
  }

  // 왼쪽과 오른쪽 배열을 정렬 후 피벗과 병합
  return [...quickSort(left), pivot, ...quickSort(right)];
}

// 테스트
const unsortedArray = [38, 27, 43, 3, 9, 82, 10];
const sortedArray = quickSort(unsortedArray);
console.log("정렬 결과:", sortedArray);
```

피벗을 무작위로 선택하는 방법이 있다

### 피벗을 가운데로 잡는 이유

피벗을 배열의 **가운데** 요소로 선택하는 이유는 일반적으로 다음과 같습니다:

- 배열이 **이미 정렬된 상태**이거나 **거의 정렬된 상태**일 때, 첫 번째 요소나 마지막 요소를 피벗으로 선택하면, 퀵 정렬이 **최악의 성능**을 보일 수 있습니다. 이때 **중앙값을 피벗으로 선택**하면 최악의 경우를 방지할 수 있습니다.
- 이 방법은 평균적인 성능을 높일 수 있습니다.

### 그러나, 피벗을 가운데로 잡지 않아도 되는 경우

피벗을 **첫 번째 요소**로 선택하거나 **무작위**로 선택하는 것도 가능합니다. 사실, **무작위 피벗 선택**은 퀵 정렬의 평균적인 성능을 더욱 안정적으로 만들어줍니다.

각 피벗 선택 방식에 따른 특성을 살펴보면:

1. **첫 번째 요소**를 피벗으로 선택:
   
   - 일반적인 경우에서 효율적으로 작동할 수 있습니다.
   - 하지만 정렬된 배열이나 역순으로 정렬된 배열에서는 최악의 성능을 보일 수 있습니다.

2. **가운데 요소**를 피벗으로 선택:
   
   - 최악의 성능을 방지하기 위해 자주 사용됩니다. 배열이 정렬된 상태에서 첫 번째 또는 마지막 요소를 피벗으로 사용할 때 발생할 수 있는 비효율성을 줄입니다.

3. **무작위로 피벗을 선택**:
   
   - 랜덤하게 피벗을 선택하면 최악의 경우를 피할 확률이 높아집니다. 이 방법은 퀵 정렬의 성능을 더 균등하게 만들어줍니다.

end. 
