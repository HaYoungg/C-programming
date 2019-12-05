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
