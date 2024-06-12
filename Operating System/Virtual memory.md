중요한점 1. **메모리 크기는 한정되어 있기 때문에 많은 프로세스가 메모리에 들어갈 수 있도록 효율적으로 할당.**

### **메모리 관리에서 중요한 세가지**

1. Relocation

   프로그램이 메모리상의 어떤 주소 위치에 들어가느냐에 따라 주소를 바꿔주는 작업.

2. Protection

   할당된 번지수 내에서만 조작하기

3. Sharing

   메모리의 특정 영역을 공유할 수 있도록 하기도 함.

### Fixed Partitioning → 실제 프로그램의 크기가 더 작기 때문에 공간 낭비 발생

고정된 공간에 프로그램 할당 : internal fragmentation problem

1. 같은 크기로 메모리를 나눔
2. 크기를 다양하게 메모리를 나눔

### Dynamic Partitioining

1. External fragmentation problem : 프로세스의 크기만큼 공간을 할당하고 해제하는 과정 반복 시 프로세스 사이에 공간이 남음
2. Compaction : 프로세스들을 위로 올려버림. → 하나하나 다 올리려면 메모리 전체 읽기 쓰기를 해야할 수도 있음.

Dynamic Partitioning 공간 중 어디에 넣을것인가에 대한 알고리즘.

1. Best-fit : 내가 들어가고 남는 공간이 제일 작은 곳 → 공간이 너무 많으면 하나하나 다 봐야함. 오래걸림.
2. First-fit : 들어갈 수 있는 처음 공간에 집어넣기
3. Next-fit : 마지막으로 할당된 곳 다음부터 첫 부분에 집어넣기
4. Worst-fit : 내가 들어가고 남는 공간이 제일 큰 곳

### Buddy System

- **Fixed + Dynamic Partitioning**

internal frag 있지만 external은 없다.

Compaction 없다.

메모리 전체 공간 2^u라고 하자.

<img width="610" alt="스크린샷 2024-06-09 오후 9 47 24" src="https://github.com/Cwonseo/tech-interview/assets/77488438/0788b0b1-df34-4a7a-aadf-fc55dcfbd77d">

반씩 나눠가면서 요청한 공간보다 한단계 큰 공간을 할당해준다.

100 → 128(2^7) 할당

자신의 버디가 비게 되면 공간을 합친다.

<img width="585" alt="스크린샷 2024-06-09 오후 9 47 32" src="https://github.com/Cwonseo/tech-interview/assets/77488438/d0dae1e0-4b49-4432-a1bb-58acea795290">

Relocation과 Protection을 어떻게 할지

주소

Logical :

Relative : 기준점을 기준으로 주소를 붙인다. 내 프로그램이 0번지에 들어간다고 예측하고 주소를 붙임.

Physical : 실제 메모리의 주소.

<img width="555" alt="스크린샷 2024-06-09 오후 9 47 41" src="https://github.com/Cwonseo/tech-interview/assets/77488438/d7360d07-02c3-45ad-940b-dd25e18d422f">

### Relocation

base register 값을 더해 realtive Address의 값을 Pysical Address 값으로 만들어낸다

### Protection

메모리 공간 아무데나 액세스하면 안된다.

Bound Register에 마지막 주소값을 넣어둔다. Bound register 값보다 더 큰 값 발견 시 OS Interrupt

위 방식은 연속된 공간에 메모리를 할당하는 시스템에서 사용하는 방식

**메모리에 10개의 프로그램이 올라와있다고 할 때 base register와 bound register의 개수는 몇개가 필요할까?**

### Paging

연속되지 않은 공간에 메모리를 할당

External fragmentation probelm 없음.

internal → 프로세스 하나에 한개 발생 가능성 있음(마지막)

Compaction 필요 없음.

<img width="702" alt="스크린샷 2024-06-09 오후 9 47 47" src="https://github.com/Cwonseo/tech-interview/assets/77488438/93fc5f58-cd82-4bf8-9236-53c8c7795b33">

페이지 : 프로세스의 한 조각

페이지 프레임 : 메모리를 동일한 크기로 나눈 것 중 한 조각으로 페이지가 들어가는 틀

### **paging 방식에서의 relocation**

페이지 테이블을 사용하고 logical address를 사용한다.

페이지테이블 예시

<img width="617" alt="스크린샷 2024-06-09 오후 9 47 53" src="https://github.com/Cwonseo/tech-interview/assets/77488438/11b097e5-ffe2-4cf6-b876-4200cff26630">

**페이지 테이블**

프로세스의 각 페이지마다 그 페이지가 메모리의 몇 번 프레임에 들어가 있는지를 저장하고 있다.

**Logical address**

페이지 번호, 페이지 안에서의 오프셋을 저장. \*오프셋 : 페이지 안에서 몇번째 줄인가

<img width="571" alt="스크린샷 2024-06-09 오후 9 48 00" src="https://github.com/Cwonseo/tech-interview/assets/77488438/faeb4e29-6837-4da9-8be7-ea8f87ba2947">

Relative address가 1502 이고, 한 페이지가 1024이다.

이 때 페이지 번호는 1(1024), Offset은 478임을 알 수 있다. 1024 + 478 = 1502

이 logical address를 기반으로 physical address를 얻는다.

<img width="617" alt="스크린샷 2024-06-09 오후 9 48 06" src="https://github.com/Cwonseo/tech-interview/assets/77488438/43831ef2-5680-4539-b5cd-d04c2130da59">

1번 페이지는 000110번 페이지 프레임에 들어가 있다. 즉, 6번 페이지 프레임에 들어 있다는 의미.

physical address : 들어 있는 페이지 프레임 + offset

### paging 방식에서의 Protection

오프셋은 10비트이기 때문에 페이지를 벗어나는 위치 가리킬 수 없음. → 페이지 번호만 고려하면 된다.

페이지 번호가 페이지 테이블의 마지막 번호보다 큰 경우 프로그램을 멈춰주면 된다.

### Segmentation

관련된 코드들 단위로 프로그램을 나누는 방식. 길이가 제각각이다. 프레임을 나눌 수가 없음.

external fragmentation 문제 발생함.

**프로세스 세그먼트 테이블**에는 프로세스 세그먼트의 길이와 base 주소를 가짐

base 주소 : 프로그램의 시작 주소

세그먼트의 length : protection에 이용

length의 최대 비트 수는 프로세스 세그먼트 하나의 최대 크기의 값이고 physical 주소는 세그먼트 테이블에 있는 base주소에 offset을 더해 만든다.

 <details>
<summary>Q. 페이지와 페이지 프레임은 무엇인가요?</summary>
<div markdown="1">
페이지는 프로세스의 한 조각이고 페이지 프레임은 메모리를 동일한 크기로 나눈 것 중 한 조각으로 페이지가 들어가는 틀이다.
</div>

</details>
