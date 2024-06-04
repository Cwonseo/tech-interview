### **데드락이란**

---

두개 이상의 프로세스가 서로에게 할당받은 자원을 얻지 못해 둘다 작업을 하지 못하는 상태.

_회사 : 일하고 싶으면 경력이 필요해요_

_지원자 : 경력을 쌓으려면 일이 필요해요_

_이처럼 무한정 기다리고 있는 상태를 데드락이라고 볼 수 있다_

### **데드락이 발생할 수 있는 조건 4가지**

---

1. Mutual Exclustion : 자원은 한번에 하나의 프로세스만 사용할 수 있다.
2. Hold-and-Wait : 자원을 할당받은 상태로 자원을 할당 받기를 기다리고 있어야 한다.
3. No Pre-emption : 프로세스로부터 실행 중간에 자원을 뺏어갈 수 없다.
4. Circular Wait(순환 대기) : 아래 그림처럼 순환 형태를 이루어야 한다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/71c9eaf7-e499-4e38-ac99-9e49e0b9c88a/4592e97f-74ae-4400-ba46-98da556d73dc/Untitled.png)

### 자원할당 그래프

---

프로세스와 자원의 관계, 할당 상태를 표현하는 그래프

![스크린샷 2024-06-04 오후 4.24.33.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/71c9eaf7-e499-4e38-ac99-9e49e0b9c88a/7ccb9fcc-879b-4691-bfe1-389f1a0b6590/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-06-04_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.24.33.png)

| 원                           | 프로세스            |
| ---------------------------- | ------------------- |
| 사각형                       | 자원                |
| 사각형 내 점 개수            | 자원의 개수         |
| 자원에서 프로세스로 화살표   | held 자원을 할당    |
| 프로세스에서 자원으로 화살표 | request 자원을 요청 |

### **데드락을 해결하는 방법 3가지**

---

### **Prevent (예방)**

데드락이 발생할 수 있는 조건 4가지중 하나를 충족 못하게 하는 방법이다.

Mutual Exclustion 없애기.

→ 모든 자원을 공유하게 만든다 **사용 불가**

Hold-and-Wait 없애기.

→ 자원이 두개 필요하면 한번에 할당받도록 만든다. **자원 활용률이 낮음**

No Pre-emption 없애기.

→ 중간에 자원을 뺏을 수 있게 한다. **CPU는 가능하나 프린터와 같은 자원에서 사용 불가**

Circular Wait 없애기.

→ 모든 자원에 번호를 붙이고 오름차순으로 자원을 할당 후 프로세스가 번호가 높은 자원만 잡을 수 있도록 한다. **모든 자원에 번호를 붙이는 것 어려움**

### **Avoid (회피)**

프로세스에게 자원을 주기 전에 데드락 가능성이 있는지 검사 후에 할당해주는 방식.

자원을 할당받고 종료할 수 있는 상태라면 자원을 주는 것이다.

안전상태 : 자원을 할당받고 종료할 수 있는 상태

불안정상태 : 자원 할당 시 데드락이 발생할 가능성이 있는 상태

안전 순서열 : 데드락 없이 프로세스들에 자원을 할당할 수 있는 순서

![스크린샷 2024-06-04 오후 4.50.12.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/71c9eaf7-e499-4e38-ac99-9e49e0b9c88a/a0dad201-0fd4-420c-8997-d190a2ad7cbd/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-06-04_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.50.12.png)

Claim Matrix : 각 자원의 최대 요청량

Allocation Matrix : 할당된 각 자원의 양

C-A : 앞으로 더 요청할 수 있는 자원의 양

Resource Vector : 시스템이 가지고 있는 자원의 개수

Availabe Vector : 사용 가능한 자원의 개수

P1 ~ P4까지 돌며 Availabe Vector와 C-A를 비교하여 **안전상태**라면 자원을 할당 후 작업을 끝내 자원을 반환시킨다.

위 방식을 사용하면 P2 → P1 → P3 → P4라는 안전 순서열로 작업을 끝마칠 수 있다.

즉, 시스템을 계속 안전 상태로 유지하여 데드락을 회피하는 방법이고 이 방식을 뱅커스 알고리즘(은행원 알고리즘)이라고도 한다.

제약조건.

1. 프로세스마다 자원의 최대 요청량을 알아야 한다. 그리고 이것이 정확하지 않을 수 있다.
2. 자원의 개수가 고정되어 있어야한다.

### **Detect and Restore (검출)**

일단 데드락이 발생할 때까지 기다리고 조치를 취하는 방식.

**Detect**

![스크린샷 2024-06-04 오후 5.17.34.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/71c9eaf7-e499-4e38-ac99-9e49e0b9c88a/7022c0f6-b047-44d3-bdb5-5eb382e6e899/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-06-04_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.17.34.png)

Request Matrix : 요청이 받아들여지지 않아 자원할당이 안 된 자원의 개수

Allocation Matrix : 각각 프로세스가 가지고 있는 자원의 개수

Availabe Vector : 지금 할당할 수 있는 자원의 개수

Resource Vector : 시스템이 가지고 있는 자원의 개수

데드락이 아닌 것이 확실한 프로세스를 마크한다.

P4 : Hold-and-Wait 만족하지 않음. 마크.

P3 : 자원 할당해줄 수 있음. 마크.

P1, P2는 마크되지 않음. 데드락에 빠졌음.

**Restore**

- 프로세스를 강제 종료시켜 시스템 전체가 데드락에 빠지지 않도록 한다.

1. 데드락에 빠진 프로세스를 모두 종료시키고 다시 시작한다.
2. 중간에 저장된 상태부터 다시 시작한다.
3. 다 종료시키지 않고 하나씩 종료시켜 데드락이 사라지는지 확인한다.

### **식사하는 철학자 문제**

---

원탁에 다섯명의 철학자가 앉아있고 철학자들 사이에 포크가 있는 상황이다.

이 때 두개의 포크를 들어야 음식을 먹을 수 있다.

1. 왼쪽 포크 사용이 가능하면 잡는다.
2. 오른쪽 포크가 사용 가능하면 잡는다.
3. 일정 시간동안 음식을 먹는다.
4. 오른쪽 포크를 내려놓는다.
5. 왼쪽 포크를 내려놓는다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/71c9eaf7-e499-4e38-ac99-9e49e0b9c88a/d75348ac-6300-428f-b0e9-672b2eef73f5/Untitled.png)

위와 같은 순서로 식사를 한다고 가정했을 때

모든 철학자가 왼쪽 포크를 동시에 든다면 아무도 오른쪽 포크를 들 수 없기 때문에 **데드락**이 발생할 수 있다.

### 💡예상 질문

<details>
<summary>Q. 데드락이 무엇인가요?</summary>
<div markdown="1">
데드락이란 두개 이상의 프로세스가 서로 작업을 완료하기 위해 상대가 가지고 있는 자원을 기다리며 무한히 대기하는 상태입니다.
</div>
</details>
<details>
<summary>Q. 데드락의 발생 조건에 대해 말해보세요.</summary>
<div markdown="2">
mutual exclusion, hold-and-wait, non pre-emption, circular wait 총 4가지가 모두 만족되는 상태에서 데드락이 발생할 수 있습니다.

</div>
</details>
<details>
<summary>Q. 데드락이 발생하면 어떻게 해결하나요?</summary>
<div markdown="1">

예방, 회피, 탐지 후 회복이 있다.
이 중 탐지 후 회복을 가장 많이 사용하는데, 자원을 할당시켜주고 데드락이 발생하면 프로세스 강제종료와 같은 방법으로 데드락을 해결할 수 있다.

</div>
</details>
<details>
<summary>Q. 데드락 회피 기법인 Banker’s(은행원) 알고리즘에 대해 설명하세요.</summary>
<div markdown="1">
프로세스에게 자원을 할당했을 때 할당 받고 종료할 수 있는 상태라면 자원을 할당해주고 그렇지 않다면 다른 프로세스를 종료시킨 후 자원이 해제될 때까지 대기했다가 자원을 할당하는 알고리즘이다.
</div>
</details>
<details>
<summary>Q. 데드락(교착상태)와 Starvation(기아)의 차이점에 대해 말해보세요</summary>
<div markdown="1">

데드락은 자원을 기다리며 blocked 상태에 있고 기아는 Ready 상태에 있다. 그리고 데드락은 서로 자원을 점유하기에 영원히 진행할 수 없는 것이고 기아상태는 우선순위에 의해 무기한 대기하는 상태인 것이다.

</div>
</details>
<details>
<summary>Q. 식사하는 철학자 문제에서 데드락을 해결하는 방법이 뭐가 있을까요?</summary>
<div markdown="1">

- 홀수 철학자는 왼쪽 포크부터, 짝수 철학자는 오른쪽 포크부터 기다린다.
  - Circular wait 발생
- 포크를 한번에 두개씩 들 수 있도록 바꾼다.
- semaphore 통해 방에 4명까지 입장할 수 있도록 한다. → 적어도 한명은 두개의 포크를 잡는다.
</div>

</details>