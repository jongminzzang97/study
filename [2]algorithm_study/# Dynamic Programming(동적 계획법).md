# Dynamic Programming(동적 계획법) - 1


## 들어가기 
    학교에서 알고리즘 수업을 들었을 때는 배우지 않앗던 것 같다.
    
    어떻게 공부할까 찾아보다가 좋은 강의를 발견했다.
    https://www.youtube.com/watch?v=ln7AfppN7mY&list=PL52K_8WQO5oUuH06MLOrah4h05TZ4n38l
    
    처음에 자료구조를 공부할 때고 참고했던 교수님이신데, 엄청 도움이 많이 되었다.
    
    여기서 다른 부분 말고 동적 프로그래밍 부분만 들을 것이다.
    내용을 이해하고 내가 이해한 것을 정리해보겠다.
    일단 재귀함수 및 컴퓨터에 대한 어느 정도의 이해를 바탕으로 작성한다.



## 피보나치 수열
    1, 1, 2, 3, 5, 8 ...
    이 대표적인 예시를 통해 동적프로그래밍을 이해해 볼 수 있다.

    피보나치 수열은 익숙하다. 특히 재귀 함수를 배울 때 다뤄보게 된다.
```python
def fibo(k):
    if k == 1 or k == 2:
        return 1
    else:
        return fibo(k-2) + fibo(k-1)
```
    간단한 코드이고.
    재귀에서는 어떻게 함수가 재귀적으로 사용하는지와 종료조건은 무엇인지가 중요하다.
    
    재귀 함수는 직관적이며 작성하기 편하다. 그러나 꽤나 큰 단점도 존재한다.

    1. 함수의 호출이 반복된다.
        함수의 호출은 loop을 한번 반복하거나 값을 계산하는 정도의 간단한 일처리라고 할 수는 없다.
        그런 부분에서의 오버헤드가 발생한다.
        또한 재귀함수의 트리가 그려지는 구조를 생각하면 메모리도 많이 차지하게 된다.

    2. 같은 값을 여러번 계산된다.
        이것 또한 트리를 그려보면 쉽게 알 수 있다
        위의 함수를 예로 들면
        fibo(5) = fibo(4) + fibo(3)
                = fibo(3) + fibo(2) + fibo(3)
        이므로 fibo(3)의 트리는 중복되지만 값을 매번 호출해서 계산하게 된다.
<br/>

    이 조금 개선하면 다음과 같은 코드를 작성해볼 수 있다.
    
```python
def fibo(k):
    f = [-1 for _ in range(k)]
    if k == 1 or k == 2:
        return 1
    elif f[k] > -1:
        return f[k]
    else:
        f[k] = fibo[k-1] + fibo[k-2]
        return f[k]    

# 만약 함수가 여러번 사용 될 것 같으면 배열을 밖에 선언하면 된다.
```
    재귀 함수는 분명 이용되고 있다.
    하지만 만약 내가 계산한 값이 배열에 존재한다면 그 값을 계산할 필요가 없다.

    위에서 어떤 문제점이 해결되었는지 보자.

    1. 함수의 호출이 반복된다. 
        함수의 호출이 반복되는가? 반복된다.
        함수 호출의 측면에서는 딱히 나아진 부분이 없다.
        트리를 그리면 동일하게 나올 것이다.
    
    2. 같은 값이 여러 번 계산된다.
        이것은 해결 되었는가? 해결되었다.
        f[n] = fibo(k-1) + fibo(k-2) 이 계산을 매번 하는 대신에,
        배열에 값이 존재한다면 바로 불러와서 함수가 일찍 종료하게 된다.
    
    따라서 재귀를 사용하였지만 계산을 덜하기 때문에 분명히 더 효율적이다.
    그리고 강의에서는 이런 방법을 Memoization이라고 하였다.
    (실제로 존재하는 말은 아니라고 하셨다.)

<br/>
<br/>

    조금 더 좋은 방법은 없을까?
    
    만약 컴퓨터를 이용하지 않고 피보나치 수열의 10번째 수를 구하라 하면 어떻게 할 것인가?
    그냥 처음부터 10 번째까지 모두 구해볼 것이다.
    재귀를 이용하지 않는다.
    이것이 정답에 근접하다. 함수로 나타내면 다음과 같다.

```python
def fibo(k):
    f = [-1 for _ in range(k)]
    f[0] = 1
    f[1] = 1

    for i in range(2, k):
        print("dd")
        f[i] = f[i-1] + f[i-2]
  
    return f[k-1]
# k가 1일 때는 예외처리를 해줘야하지만 생략한다.
```

    위 코드를 보면 함수의 호출이 반복되지도 않고, 같은 값이 여러번 계산될 필요도 없다.
    필요한 값을 저장하고 불러오면서 계산에 이용한다.


<br/>
<br/>
<br/>

## Combination
    피보나치에 이어서 대표적으로 등장하는 예제이다.
    combination(n, k) = combination(n-1, k-1) + combination(n-1, k)을 이용한다.
    (이 식이 어떻게 나오는지 생각해 보는 것도 좋다.)

    이번엔 따로 설명없이 코드만 작성했다.
    코드를 직접 작성해보거나 작성된 것을 보면서 각각이 더떻게 다른지 확인해보자.
    (직접 작성해보는 것이 매우 도움이 된다)

    Dynamic programming에서 중요한 것은 처음 채워지는게 어디이고
    어떤 순서로 채워 나갈 수 있는 지이다.
    
    recursive 에서의 종료조건이 처음 채울 수 있는 부분과 같은 역할을 한다.
    (그렇기에 중요하다고 생각해도 좋다.)
    그리고 그 채워진 부분으로 나머지를 어떻게 완성시킬지가 당연히 중요할 수밖에 없다.

    

```python
# use recursive 
def combi(n, k):

    if n == k or k == 0:
        return 1
    
    else :
        return combi(n-1, k-1) + combi(n-1, k)
            

# use Memoization
#c = [[-1 for _ in range(j+1)] for j in range (n)]
# 리스트 c는 초기값을 -1로 초기화해둠
def combi(n, k):
    
    if c[n][k] > -1:
        return c[n][k]
    
    elif n == k or k == 0 :
        c[n][k] = 1
        return 1
          
    else :      
        c[n][k] = combi(n-1, k-1) + combi(n-1, k)
        return c[n][k]


# using Dynamic programming 
#c = [[-1 for _ in range(j+1)] for j in range (n)]
def combi(n, k): 
    c[0][0] = 1
    for i in range(n+1):
        for j in range(i):
            
            if j == 0 or i == j:
                c[i][j] = 1  
            else :
                c[i][j] = c[i-1][j-1] + c[i-1][j]
                
    return c[n][k]  
    
```

<br/>
<br/>
<br/>


## 최단경로

    6 7 12 5
    5 3 11 18
    7 17 3 3
    8 14 14 9
    
    좌상단(0,0)에서 우하단(3,3)까지 이동할 때 방문한 칸에 있는 정수들의 합이 최소가 되도록하라.


```python
# 모든 문제에서 지도는 M이라는 2차원 리스트에 들어있다고 하자.

# use recursive
def short(i, j):

    if i == 0 and j == 0:
        return M[0][0]
    elif i == 0:
        return short(i, j-1) + M[0][j]
    elif j == 0:
        return short(i-1, j) + M[i][0]
    else:
        return min(short(i, j-1), short(i-1, j)) + M[i][j]
    


# use Memoization
# 새로운 배열을 그 지점까지의 최단 경로의 값으로 저장할 필요가 있다.
# 미리 S라는 배열의 값을 -1로 초기화
def short(i, j):
    
    if i == 0 and j == 0 :
        S[0][0] = M[0][0]
    
    elif i == 0:
        S[i][j] = short(i, j-1) + M[0][j]

    elif j == 0:
        S[i][j] = short(i-1, j) + M[i][0]
    
    elif S[i][j] == -1:
        S[i][j] = min(short(i, j-1), short(i-1, j)) + M[i][j]
    
    return S[i][j]

    
# using Dynamic programming 
# 재귀 함수가 이용되지 않는다.
def short(i, j):
    
    S[0][0] = M[0][0]
        
    for x in range(1, i+1):
        S[0][x] = S[0][x-1] + M[0][x]
    for y in range(1, j+1):
        S[y][0] = S[y-1][0] + M[y][0]
        
    for x in range(1, i+1):
        for y in range(1, j+1):
            S[x][y] = min(S[x-1][y], S[x][y-1]) + M[x][y]
            
   
    return S[i][j]


```