# 최장증가수열 LIS;Longest Increasing Subsequence

-  최장증가수열(LIS)

  어떤 수열 A={ a<sub>1</sub>, a<sub>2</sub>, ..., a<sub>n</sub> } 에 대해, 배열의 순서를 유지하면서, 크기가 점진적으로 커지는 가장 긴 부분 수열

---

문제 유형

​	i)  DP를 이용하여 길이만을 출력

​	ii) DP를 이용하여 최장증가수열의 값을 출력

​	iii) 이진탐색을 이용하여 길이만을 출력

​	iv) 이진탐색을 이용하여 최장증가수열의 값을 출력 

---

- 가능한 LIS 중에서, 가장 수열의 **길이** 가 긴 것을 찾는 문제

  - Brute-force로 푼다.
    - 부분집합 알고리즘을 활용
    - 수열의 모든 부분집합을 구하여, 각 부분집합이 증가 수열인지 확인한 뒤, 가장 길이가 긴 수열을 찾는다.
    - O(2<sup> n</sup>)의 시간복잡도를 가진다.

  - Dynamic Programming으로  푼다. 
    - LIS를 만족하는 점화식을 찾는다.
    - O(n<sup> 2</sup> )의 시간복잡도를 가진다.
  - 이진 검색을 이용한 DP
    - O(nlogn)의 시간복잡도를 가진다.





#### Dyanmic Programming으로 풀기 O(n<sup> 2</sup> )

숫자 배열 a<sub>1</sub>, a<sub>2</sub>, ..., a<sub>n</sub>에 대해, 

DP[i] 는 a<sub>1</sub>, a<sub>2</sub>, ..., a<sub>i</sub> 에서 LIS의 길이이다.

DP[i] 에 a<sub>i</sub> 가 들어있지 않으면,  DP[i] = DP[i-1]

DP[i] 에 a<sub>i</sub> 가 들어있으면, 