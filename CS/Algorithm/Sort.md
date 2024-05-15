# Sort

|정렬 알고리즘|시간 복잡도|최악 시간복잡도|알고리즘 기법|비고|
|---|---|---|---|---|
|[Bubble Sort](#bubble-sort)|O(n^2)|O(n^2)|비교와 교환|쉬운 코딩|
|[Counting Sort](#counting-sort)|O(n+k)|O(n+k)|비교환 방식|n이 비교적 작을 때만 가능|
|[Selection Sort](#selection-sort)|O(n^2)|O(n^2)|비교와 교환|교환의 횟수가 버블, 삽입 정렬보다 작음|
|[Insertion Sort](#insertion-sort)|O(n^2)|O(n^2)|비교와 교환|n의 개수가 작을 때 효율적|
|[Quick Sort](#quick-sort)|O(nlogn)|O(n^2)|분할 정복|평균적으로 가장 빠름|
|[Merge Sort](#merge-sort합병-정렬)|O(nlogn)|O(nlogn)|분할 정복|연결 리스트의 경우 가장 효율적|


## Bubble Sort
인접한 두개의 원소를 비교하며 자리를 계속 교환하는 정렬 알고리즘
* 교환하며 자리를 이동하는 모습이 물 위로 올라오는 거품 모양과 비슷하다고 하여 버블 정렬이라고 함

![Bubble sort gif](https://i.stack.imgur.com/XNbE0.gif)

### 정렬 과정
1. 첫번째 원소부터 인접한 원소끼리 큰 원소가 오른쪽으로 이동하도록 자리를 교환하며 마지막 자리까지 이동
2. 한 단계가 끝나면 가장 큰 원소가 마지막 자리로 이동
3. 앞선 과정을 반복


```python
N = 6
arr = [7, 2, 5, 3, 1, 5]
for i in range(N-1, 0, -1): # 정렬할 구간의 마지막 인덱스
    for j in range(i): # 비교할 두 원소 중 왼쪽의 인덱스
        if arr[j] > arr[j+1]:
            arr[j], arr[j+1] = arr[j+1], arr[j]
print(arr)
# 결과: [1, 2, 3, 5, 5, 7]
```


## Counting Sort
항목들의 순서를 결정하기 위해 각 항목이 몇개씩 있는지 세는 작업을 하여 선형 시간에 정렬하는 알고리즘

### 제한 사항
정수나 정수로 표현할 수 있는 자료에 대해서만 적용 가능
- 각 항목의 발생 횟수를 기록하기 위해 정수 항목으로 인덱스 되는 배열을 사용하기 때문
- 이를 위해 집합 내의 가장 큰 정수를 알아야 함

![Counting Sort gif1](https://velog.velcdn.com/images%2Fcrosstar1228%2Fpost%2Fd9b38630-27b6-4977-a26a-41008d887593%2Fimg.gif)
![Counting Sort gif2](https://velog.velcdn.com/images%2Fcrosstar1228%2Fpost%2Ff104788c-1e62-47d4-a641-fe2c80aa05f9%2Fimg%20(1).gif)

### 정렬 과정
1. Input Array에서 각 항목들의 횟수를 세고, 정수 항목들로 직접 인덱스 되는 Count Array에 저장
2. 정렬된 배열에서 각 항목의 앞에 위치할 항목의 개수를 반영하기 위해 Count Array의 원소 조정
3. Input Array를 순회하며 항목을 Result Array의 Count Array[항목] 인덱스에 삽입
  - Count Array[항목] -= 1

```python
N = 6 # input_array의 길이
K = 9 # input 의 최댓값
input_array = [9, 2, 4, 5, 1, 3]
count_array = [0] * (K + 1)
result_array = [0] * N
for x in input_array: # 정렬 과정 1번
    count_array[x] += 1

for i in range(1, K+1): # 정렬 과정 2번
    count_array[i] += count_array[i-1]

for i in range(N-1, -1, -1): # 정렬 과정 3번
    # 왜 뒤에서부터 순회함?
    # 원래의 순서를 유지하기 위해서
    # Ex) (2, 1), (2, 2)이고 첫번째 원소를 기준으로 정렬한다면 (2, 1), (2, 2) 순서를 유지할 수 있다.
    # 앞에서부터 순회한다면 (2, 2), (2, 1)로 변경되어 정렬될 것
    count_array[input_array[i]] -= 1
    result_array[count_array[input_array[i]]] = input_array[i]
# 결과: [1, 2, 3, 4, 5, 9]
```

### 시간 복잡도
O(n + k)
- n은 리스트의 길이, k는 정수의 최댓값

## Selection Sort
주어진 자료들 중 가장 작은 값의 원소부터 차례대로 선택하여 위치를 교환하며 정렬하는 알고리즘

![Selection Sort gif](https://miro.medium.com/v2/resize:fit:720/format:webp/1*5WXRN62ddiM_Gcf4GDdCZg.gif)

### 정렬 과정
1. 주어진 리스트에서 최솟값 탐색
2. 최솟값을 리스트의 맨 앞 값과 교환
3. 맨 앞 값을 제외한 리스트에서 앞선 과정 반복
```python
N = 6
arr = [7, 2, 5, 3, 1, 5]
for i in range(N-1):
    minIndex = i
    for j in range(i+1, N):
        if arr[j] < arr[minIndex]:
            minIndex = j
    arr[i], arr[minIndex] = arr[minIndex], arr[i]
# 결과: [1, 2, 3, 5, 5, 7]
```

## Insertion Sort
![Insertion Sort gif](https://i.stack.imgur.com/nL73t.gif)

## Quick Sort
주어진 배열을 두개로 분리하고, 각각을 정렬하는 것

![Quick sort gif](https://engineering.fb.com/wp-content/uploads/2022/07/Hermes-quicksort.gif)

### 정렬 과정
1. 리스트 가운데서 하나의 원소(Pivot)을 고름
2. 피벗 앞에는 피벗보다 값이 작은 모든 원소들이 오고, 피벗 뒤에는 피벗보다 값이 큰 모든 원소들이 오도록 피벗을 기준으로 리스트를 나눔(이렇게 리스트를 둘로 나누는 것을 분할이라고 한다.)
3. 분할된 두개의 작은 리스트에 대해 재귀적으로 이 과정을 반복, 재귀는 리스트의 크기가 0이나 1이 될 때까지 반복


```python
def quickSort(a, begin, end):
    if begin < end:
        p = partition(a, begin, end)
        quickSort(a, begin, p - 1)
        quickSort(a, p + 1, end)


def partition(a, begin, end):
    # pivot = (begin + end) // 2
    pivot = begin # pivot의 위치는 대부분 begin으로 잡는듯
    l = begin
    r = end
    while l <= r:
        while l <= r and a[l] < a[pivot]:
            l += 1  # l보다 왼쪽에 있는 값은 모두 a[pivot]보다 작음
        while l <= r and a[pivot] <= a[r]:
            r -= 1  # l보다 오른쪽에 있는 값은 모두 a[pivot]보다 큼
            # a[pivot]과 a[r]의 값이 같아도 r-=1이 실행되므로 l == r이 될 수 있음
        if l < r:  # 아직도 r이 l보다 오른쪽에 있으면?
            a[l], a[r] = a[r], a[l]  # a[r] <= a[pivot] <= a[l]인 상태이므로 변경
    a[pivot], a[r] = a[r], a[pivot]
    # 현재 l == r, pivot = l + 1인 상태, l + 1 == r이 되도록 변경
    return r

lst = [69, 10, 30, 2, 16, 8, 31, 22]
quickSort(lst, 0, len(lst) - 1)
```

## Merge Sort(합병 정렬)
여러 개의 정렬된 자료의 집합을 병합하여 한 개의 정렬된 집합으로 만드는 방식

### 정렬 과정
1. 자료를 최소 단위의 문제까지 나눔
2. 차례대로 정렬

```python
def merge_sort()

def merge()
```

## 출처

[Bubble Sort, Insertion Sort gif](https://stackoverflow.com/questions/67729819/is-it-bubble-sort-or-insertion-sort)  
[Counting Sort gif](https://velog.io/@crosstar1228/DSsorting3-counting-radix-topological)  
[Selection Sort gif](http://www.xybernetics.com/techtalk/SortingAlgorithmsExplained/SortingAlgorithmsExplained.html)  
[Quick Sort gif](https://engineering.fb.com/2022/07/20/security/hermes-quicksort-to-run-doom/attachment/hermes-quicksort/)