## 그래프 탐색(깊이 우선 탄샘 - DFS)

<br>

### DFS 깊이 우선 탑색(Depth First Search)

<br>

`"더 나아갈 길이 보이지 않을 때까지 깊이 들어간다."` 를 원칙으로 그래프 내의 정점을 방문하는 알고리즘이다.   

미로 찾기처럼 그래프의 정점을 타고 깊이 들어가다 더 이상 방문해왔던 정점 말고는 다른 이웃을 갖고 있지 않은 정점을 만나면, 뒤로 돌아와 다른 경로로 뻗어있는 정점을 타고 방문을 재개하는 방식으로 동작한다.   

루트 노트(혹은 다른 임의의 노드)에서 시작해서 다음 분기(branch)로 넘어가기 전에 해당 분기를 완벽하게 탑색하는 방법     

> 사용하는 경우: 모든 노드를 방문하고자 하는 경우에 이 방법을 선택한다.(완전 탐색 알고리즘에 자주 이용됨.)   

<br>

### DFS의 특징   

1. 자기 자신을 호출하는 순환 알고리즘의 형태   
2. 트리 순회(전위, 중위, 후위 순회)는 모두 DFS의 한 종류   
3. 그래프 탐색의 경우 어떤 노드를 방문했었는지의 여부를 반드시 검사해야함(안하면 무한루프에 빠질 위험이 있음)   

<br>

### DFS의 수행 과정

<br>

<img src="img/img07.png">

<br>

1. a노드(시작 노드)를 방문한다.
   - 방문한 노드는 방문했다고 표시한다.   

2. a와 인접한 노드들을 차례로 순회한다.   
   - a와 인접한 노드가 없다면 종료한다.   

3. a와 이웃한 노드 b를 방문했다면, a와 인접한 또 다른 노드를 방문하기 전에 b의 이웃 노드들을 전부 방문해야 한다.   
   - b를 시작 정점으로 DFS를 다시 시작하여 b의 이웃 노드들을 방문한다.(단계 1 다시)   

4. b의 분기를 전부 완벽하게 탐색했다면 다시 a에 인접한 정점들 중에서 아직 방문이 안된 정점을 찾는다.   
   - 즉, b의 분기를 전부 완벽하게 탐색한 뒤에야 a의 다른 이웃 노드를 방문한 수 있다는 뜻이다.   
   - 아직 방문이 안된 정점이 없으면 종료한다.   
   - 있으면 다시 그 정점을 시작 정점으로 DFS를 시작한다.    

<br>

### 구현 방법 2가지   

<br>

1. 순환 호출 이용(재귀)
2. 명시적인 스택 사용 - 인접한 정점들을 스텍에 저장하였다가 다시 꺼내어 작업   

<br>

### 그래프 표현하기

<br>

정접의 개수, 간선의 개수, 탐색을 시작할 정점을 번호로 입력받는다.   

노드 방문 여부를 검사하기 위한 boolean 배열도 선언한다.   

<br>

```Java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
StringTokenizer st = new StringTokenizer(br.readLine());

int vertext = Integer.parseInt(st.nextToken());             // 정점의 개수
int edge = Integer.parseInt(st.nextToken());                // 간선의 개수 
int vertextStartNumber = Ineteger.parseInt(st.nextToken())  // 탐색을 시작할 정점의 번호
```

<br>

### DFS 인접 리스트로 구현   

<br>

LinkedList에 정점과 간선을 넣어 인접 리스트를 만든다.   

<br>

```Java
List<Integer>[] list = new LinkedList[vertext + 1];

for (int i = 0; i <= vertext; i++) {
    list = new LinkedList<Integer>();
}

// 두 정점 사이에 여러 개의 간선이 존재할 수 있다.
// 입력으로 주어지는 간선은 양방향이다.  
while (edge-- > 0) {
    st = new StringTokenizer(br.readLine());

    int vertext01 = Integer.parseInt(st.nextToken());
    int vertext02 = Integer.parseInt(st.nextToken());

    list[vertext01].add(vertext02);
    list[vertext02].add(vertext01);
}

for (int i = 1; i <= vertext; i++) {  // 방문 순서를 위해 오름차순 정렬
    Collections.sort(list[i]);
}

System.out.println("DFS - 인접 리스트");
dfs_list(vertextStartNumer, list, c);
```

<br>

### 1-1. 재귀를 이용해 DFS 구현   

<br>

처음 DFS 함수를 호출하게 되면 int vertextStartNumber에 탐색을 시작할 정점의 번호가 들어있고, 이 시작 정점부터 DFS 탐색을 시작한다.   

정점을 방문하면 visited[vertextStartNumber] 에 true 값을 넣어 방문했다고 표시한 뒤 정점을 출력한다.   

현재 정점과 인접한 정점들을 찾기위해 인접 리스트에 Iterator를 이용해 접근해 이접 정점을 탐색한다.   

점점 vertextStartNumber의 인접리스트를 순회하며 아직 방문하지 않은 visited가 false인 정점을 찾느다.   

인접한 정점 nextVertex를 찾는다면 nextVertex부터 다시 DFS를 한다.   

더 이상 방문하지 않은 정점이 없을 때까지 탐색을 하며 DFS를 하는 것이다.   

<br>

```Java
// DFS - 인접리스트 - 재귀로 구현
public static void dfs_list(int vertexStartNumber, LinkedList<Integer>[] list, boolean[] visited) {
    visited[vertextStartNumber] = true;            //정점 방문 표시
    System.out.println(vertextStartNumber + " ");  // 정점 출력

    Iterator<Integer> iterator = list[vertextStartNumber].listIterator();  // 정점 인접리스트 순회
    while(iterater.hasNext()) {
        int nextVertex = iterator.next();
        if (!visited[nextVertex]) {  // 방문하지 않은 정점이라면
            dfs_list(nextVertex, list, visited); // 다시 DFS 재귀 호출
        }
    }
}
```