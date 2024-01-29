# Algorithm


### Bubble Sort
설명도 추가할 것
그림도 찾아서 추가할 것
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