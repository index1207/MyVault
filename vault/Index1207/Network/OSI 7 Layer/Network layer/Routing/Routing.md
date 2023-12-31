___
## 라우팅
[[IP]] 프로토콜에서 출발지부터 목적지까지 데이터그램을 전송하는 것을 라우팅([[Routing]])이라고 한다.

## 라우팅 알고리즘
목적지까지 전송하는 과정을 라우팅 알고리즘이라고 한다. 라우팅 알고리즘은 크게 동적 라우팅(dynamic routing)과 정적 라우팅(static routing) 2가지로 나뉜다.

### 정적 라우팅 알고리즘
정적 라우팅 알고리즘은 초기의 라우터 노드의 정보만을 가지고 목적지에 도달할 수 있을지 경로를 찾는 알고리즘이다. 단점으로는 네트워크의 현재 상황을 반영하지 않고 결정된 경로만을 사용해 느려질 수 있다.
#### 종류
- 최단경로(=다익스트라([[Dijikstra]])) 알고리즘
- 플러딩([[Flooding]]) 알고리즘
### 동적 라우팅 알고리즘
동적 라우팅 알고리즘은 정적 라우팅 알고리즘과 달리 현재 네트워크 상태를 반영해 경로를 변경해가면서 더 빠른 길을 찾는 라우팅하는 알고리즘이다.
#### 종류
- 거리벡터([[Distance Vector]]) 알고리즘
- 연결상태([[Link State]]) 알고리즘