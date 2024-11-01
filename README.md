# Practiceweek3

Q1. 분석
1. 입문 주차와 비교해서 입력 받는 방식의 차이와 공통점
: 공통점은 두 방식이 같은 값을 입력해도 (WASD) 구현되는 것은 똑같다는 것인데, 
차이가 있다면 이전에는 Console을 통해서 스크립트로 일일이 작성해서 이동방식을 
구현해야 했지만 지금은 Input System을 사용해서 키설정을 편하게 할 수 있고,
이동에 대한 코드도 간단하게 작성할 수 있다.

2. CharacterManager와 Player의 역할
: CharacterManager는 캐릭터와 관련된 모든 것들을 통합해서 처리해주는 관리자
역할이라고 한다면 Player는 다른 곳에서 처리된것들을 모아서 사용하는 역할이라고
생각한다.

3. 핵심 로직 분석
- Move : InputAction이 작동중인지에 따라서 Vector2의 값을 정해주는 형식
- CameraLook : 마우스의 위치에 따라서 보는 방향을 정해주는 형식
- IsGrounded : Ray를 땅에 발사해서 grounLayer를 인식하게 하는 형식

4. 각각 Update를 다르게 호출하는 이유
- Move - FixedUpdate는 프레임 기반으로 호출되는 Update와는 달리 일정한 간격으로
호출되기 때문에 물리 효과가 적용되는 Move에서 쓰기 좋다.
- CameraLook - LateUpdate는 모든 Update 함수가 호출된 후 마지막으로 호출되는데
카메라가 촬영하는 대상의 움직임을 업데이트하고 움직여야 하기 때문에 사용된다.

---


Q2. 분석
1. 별도의 UI 스크립트를 만드는 이유
: 코드의 유지보수나 재사용성을 높이기 유용하기 때문이다. UI와 로직을 분리하면 
각자의 역할에 맞는 코드를 작성할 수 있어서 가독성도 높아지고 유지보수도 쉬워진다.
또한 디자인이 바뀌어도 UI 스크립트만 수정하면 되기 때문에 변경사항이 있어도 
적용하는데 용이하다. 협업에서도 UI와 기능을 분리시키면 따로 작업이 가능하기 때문에
필요한 부분만 신경써서 작업하기 좋다.

2. 인터페이스의 특징
: 인터페이스는 추상 메서드들의 집합으로 기능을 구현하지 않고 기능의 틀을 제공한다.
다중 상속 구현이 가능해서 클래스에서 여러 인터페이스를 구현할 수 있다. 인터페이스를
구현하는 클래스는 인터페이스에 정의된 메서드를 반드시 구현해야한다. 이러한 특징
덕분에 유지보수가 쉬우며 기능 확장에도 매우 유용하다.

3. 핵심 로직 분석
UI 스크립트 구조 : UI 스크립트 구조는 실제로 바뀌는 컨디션 기능과 UI에서 표시되는 컨디션을
나누어서 컨디션에서 값이 바뀌면 UI에서 그 값을 받아 표시하게 만들었고, 인벤토리
창과 아이템 슬롯을 분리시켜서 기능을 만들었습니다.

CampFire : damageRate 간격으로 DealDamage를 호출해서 things 리스트에 있는 객체들에게
반복적으로 데미지를 입힙니다. 트리거 범위안에 들어오거나 나가는 객체들을 OnTriggerEnter와
OnTriggerExit로 리스트에 추가하거나 더해서 데미지를 입게 합니다.

DamageIndicator : 플레이어가 데미지를 받을 때마다 Flash가 실행되도록 onTakeDamage에
연결해주고 Flash가 호출되면 붉은색 이미지가 활성화되고 FadeAwat 코루틴을 시작합니다.
FadeAway는 점차 투명도를 줄여서 이미지를 사라지게하는 효과를 가지고 있습니다.

---

Q3. 핵심 로직 분석
- Interaction : 화면 중앙의 Raycast를 통해서 Update로 주기적으로 상호작용 가능한 객체를 탐지하고
객체가 상호작용이 가능하다면 프롬프트 메시지를 화면에 표시합니다. 상호작용 입력이 들어오면
해당 객체와 상호작용을 실행하고, 상호작용 상태를 초기화합니다.

Inventory : 아이템을 획득하면 인벤토리에 추가하고, 인벤토리 상태를 아이템 슬롯 UI가
업데이트 합니다. 선택한 아이템을 사용, 장착, 드랍할수 있고 어떤 종류인가에 따라서
버튼 활성이 다르게 나타납니다. 아이템을 사용하거나 드랍하면 인벤토리의 수량을 줄이고
0이 되면 슬롯에서 제거해서 UI에 반영합니다.
