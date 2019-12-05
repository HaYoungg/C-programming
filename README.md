# C-programming

# Heap ADT
Queue에서는 어떤 data가 들어오면 순서대로 처리된다. 즉, 줄서기와 같이 data가 들어온 순서대로 정렬되고 빠져나갈 때도 먼저 들어온 data순으로 처리된다. 하지만 Queue에서는 우리가 원하는 data를 다른 특정 data보다 우선적으로 정렬하게 할 순 없다. 이 때 heap구조를 이용하여 우리가 원하는 data에 우선순위를 부여한다면, 그 data의 위치를 이미 정렬된 다른 data들 보다 우선적으로(순서대로가 아닌) 배치할 수 있다. 지금부터 볼 priority heap구조는 고객정보를 다루는알고리즘이다. 
예를 들어 Time sale 기간에 고객들이 줄을 섰다고 가정하자. 일반적인 Queue 구조에서는 줄을 선 고객부터 차례대로 입장할 것이다. 하지만 만약 VIP인 고객이라면 줄을 늦게 서더라도 우선순위를 부여해야할 것이다. 따라서 heap구조를 이용해 고객정보에 우선순위크기를 부여하여 줄을 선 순서와 상관없이 우선순위가 큰 사람부터 입장할 수 있도록 만들것이다. 만약 우선순위가 같은 사람이 줄을 섰다면, 줄을 선 순서대로 입장하게 된다.
# HEAP 구조체 선언
→heap 구조체 멤버로 heapAry, last, size,compare함수, maxSize를 선언한다.
# HEAP* heapCreate(int maxSize, int(*compare)(void* argu1, void* argu2))
→동적 메모리 영역에 HEAP구조체 포인터를 할당한다.
→heap 구조체를 초기화하고 heapAry에는 calloc을 사용하여 할당과 동시에 초기화를 한다. heap->maxSize는 할당공간의 개수를 의미하고 sizeof(void*)는 할당 공간의 크기를 의미한다.
# void reheapUp(HEAP* heap, int childLoc)
→reheapUp함수의 매개변수로 선언된 childLoc이 만약 제 값이 있다면 if문을 실행하게 된다. 이때 이중포인터인 heapAry는 구조체포인터 HEAP멤버의 heapAry와 같다.
→parent는 childnode의 윗단계인 node를 의미한다. Parent에 (childLoc-1)/2한 주소값을 준다.
→heapAry[childLoc]:childLoc이 배열의 위치를 의미하므로 heapAry[childLoc]은 그 위치의 data값이다.
마찬가지로 heapAry[parent] int형 parent의 배열 위치에 해당하는 data값을 의미한다.
→compare함수를 통해 두 data를 비교했을 때 childLoc에 저장된 data값이 더 크다면(즉,childnode값이 더 크면) 현재 parentnode 값은 저장하고 parentnode 자리에 childenode값을 넣어주어 두 data의 위치를 바꾼다.
→이렇게 함으로써 첫번째 reheapUp이 이루어지고 재귀함수 구조를 통해서 처음 childLoc에 있던 data값은 다른 data들과의 크기비교를 통해 heap 구조에서 자기자리에 맞는 위치에 배열될 것이다.
# void reheapDown(HEAP* heap, int root)
→last는 int형 변수로 배열의 마지막 위치를 나타낸다.
→leftData = heap->heapAry[root * 2 + 1] : 만약 root*2+1의 위치가 배열의 마지막 값보다 같거나 작으면 leftData는 배열의 root*2+1위치에 있는 값으로 저장된다.
→rightData = heap->heapAry[root * 2 + 2] : 만약 root*2+2의 위치가 배열의 마지막 값보다 같거나 작으면 rightData는 배열의 root*2+2의 위치에 있는 값으로 저장된다.
→if ((!rightData) || heap->compare(leftData, rightData) > 0) : compare함수는 통해 아까전에 저장했던 leftData값과 rightData값을 비교했을 때 leftData가 더크면 1을 반환하고 rightData가 더 크면 -1을 반환한다. 지금 if 문에서는 만약 leftData가 더 크다면을 가정한다.
→만약 leftData값이 더 크면 largeLoc에는 root*2+1의 배열 위치값을 저장하고 rightData값이 더크면 largeLoc에는 root*2+2의 배열 위치값을 저장한다.
