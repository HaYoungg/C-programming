# C-programming

# Heap ADT
Queue에서는 어떤 data가 들어오면 순서대로 처리된다. 즉, 줄서기와 같이 data가 들어온 순서대로 정렬되고 빠져나갈 때도 먼저 들어온 data순으로 처리된다. 하지만 Queue에서는 우리가 원하는 data를 다른 특정 data보다 우선적으로 정렬하게 할 순 없다. 이 때 heap구조를 이용하여 우리가 원하는 data에 우선순위를 부여한다면, 그 data의 위치를 이미 정렬된 다른 data들 보다 우선적으로(순서대로가 아닌) 배치할 수 있다. 지금부터 볼 priority heap구조는 고객정보를 다루는알고리즘이다. 
예를 들어 Time sale 기간에 고객들이 줄을 섰다고 가정하자. 일반적인 Queue 구조에서는 줄을 선 고객부터 차례대로 입장할 것이다. 하지만 만약 VIP인 고객이라면 줄을 늦게 서더라도 우선순위를 부여해야할 것이다. 따라서 heap구조를 이용해 고객정보에 우선순위크기를 부여하여 줄을 선 순서와 상관없이 우선순위가 큰 사람부터 입장할 수 있도록 만들것이다. 만약 우선순위가 같은 사람이 줄을 섰다면, 줄을 선 순서대로 입장하게 된다.
# <heap.h> 부터 살펴보자
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
# bool heapDelete(HEAP* heap, void** dataOutPtr)
→보통 heap에서의 삭제는 root노드를 삭제한다. Root를 삭제하고 나면 두 subtree가 연결이 안된 상태이므로 배열의 가장 마지막을 root로 이동시키고 아까 전의 reheapDown 구조를 통해서 다시 완성된 heap구조를 만들어준다.
# <HeapTest.c>
# CUSTOMER구조체 선언과 사용될 함수들
→ CUSTOMER구조체 멤버로 고객 이름(name)과 부여될 우선순위(priority, 값이 클수록 우선순위가 높다.), 그리고 우선순위로부터 계산될 point를 선언하였다.(우선순위가 높은 사람일수록 point가 높다.)
# main함수
→ HEAP구조체 포인터인 prQueue를 선언하고 동적메모리 영역에서 할당받는다. 실제 이 프로그램은 processPQ함수를 통해 실행된다!
# int compareCus(void* cus1, void* cus2)
→compare함수를 통해 두 매개변수 값을 비교해보자.
→ CUSTOMER 구조체인 c1과 c2를 선언한다.
→매개변수로 받은 void포인터형 cus1과 cus2를 CUSTOMER구조체 포인터형으로 형변환을 하고 그게 가리키는 값이 구조체 c1,c2가 되도록 c1,c2를 초기화한다.
→만약 c1구조체의 point멤버 값이 c2구조체의 point멤버값보다 작으면 -1, 같으면 0, 크면 1을 반환하도록 함수를 설정한다.
# void processPQ(HEAP* prQueue)
-option이 ‘e’인 경우
→ 이제 priority heap구조의 작동중심이라 할 수 있는 processPQ함수를 분석해보자!
→ 조금 있다가 볼 menu()함수를 실행하고 난 return값으로 option변수를 초기화한다. 만약
option이 ‘e’였다면 getCus()함수를 실행한다. getCus()함수는 새로 삽입할 고객의 이름과 우선순위
를 입력받고 customer 구조체에 그 값을 저장하여 그 구조체를 반환하는 함수이다.
→ getCus()함수로부터 새로운 정보가 입력된 customer구조체를 생성하였다. 따라서 numCusomer
의 값을 1증가시켜주고 입력받은 priority값으로부터 point값을 계산하여 저장한다.( customer-
>point = customer->priority * 1000 + (1000 - numCustomer)) 옆의 계산식을 보면 priority가 클수
록 point가 크다는 것을 알 수 있다. 또한 만약 priority값이 같을 경우 numCustomer에 따라
point값이 달라진다. 즉 우선순위가 같을 경우 먼저 줄을 선 사람이 더 큰 point값을 가진다.
→ 정보가 잘 저장되었는지 확인하기 위해서 저장된 정보를 출력하는 printf()함수를 써보았다.
→ 고객이름(name), 우선순위(priority), point가 저장된 customer구조체를 heap에 삽입해보자.
heapInsert(HEAP* heap, void* dataPtr)함수를 통해 prQueue HEAP구조체 포인터에 customer 구조
체 포인터가 저장된다. 즉 heap->heapAry[heap->last]=dataPtr 문장을 통해 크기가 20인
heapAry배열의 마지막에 cutomer구조체가 삽입되고, reheapUp을 통하여 point가 큰 고객부터
heap구조로 배열이 정렬된다.
→즉 어떤 고객이 먼저 왔더라도(먼저 삽입됐더라도) point가 큰 고객이 먼저 입장하게 된다. 왜냐
하면 reheapUp을 실행할 때 compareCus가 실행되는데, 이때 point값으로 배열들을 비교하기 때
문이다.
→만약 heapInsert가 제대로 안이루어져 false를 반환했다면, 에러가 났다는 메시지와 함께
eixt(101)로 코드를 종료한다.

-option이 ‘d’인 경우
→option값이 ‘d’이면 heapDelete함수가 실행된다. point크기순으로 정렬된 heapAry에서 root값이
(point 값이 가장큰) 삭제된다.
→delete가 제대로 이루어지지 않으면, 에러메시지를 출력한다.
→만약 delete가 제대로 실행됐다면, 삭제된 customer 이름을 출력하면서 삭제가 잘 되었는지 확
인한다. 그런 후 numCustomer수를 1감소시킨다.
→option이 ‘q’이면 do-while반복문을 종료한다.
