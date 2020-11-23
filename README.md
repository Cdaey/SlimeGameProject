# SlimeGameProject

## Function

- GetInput() : 키보드, 마우스 등 입력을 받는 함수
- LookAround() : 마우스 이동 시 화면 전환 함수
- Move() : 이동 함수
- Run() : 달리기 함수
- Jump() : 점프 함수
- Attack() : 몸통 박치기 함수
- IEnumerator WaitforAttack() : 몸통 박치기에 딜레이를 걸어주는 코루틴 함수
- Skill() : 스킬 사용 조건 함수
- IEnumerator SkillCreate() : Instantiate 함수로 스킬 생성
- Interaction() : 원소를 획득하고 조건에 맞춰 공격력을 증가 시키는 함수
- choiceElement() : 상호 작용한 원소 종류에 따라 원소 프리펩을 스킬에 할당 해주는 함수
- Heal() : 체력을 회복 시키는 함수

## History
### 20.11.11
 -  Blender 프로그램으로 슬라임 몸체, 뼈, 애니메이션(걷기) 제작 
 -  ""Start(), Update() 함수는 존재만으로 자원을 소모하기에 사용하지 않으면 지우게 좋다.""
### 20.11.12
 - 슬라임 Blender 파일 Unity에 임포트 후 걷기 W,A,S,D(방향키) 
### 20.11.13
 - Blender : 달리기, 점프 제작
 - Unity : shift 입력 지속 시간 동안 달리기, Spacebar 입력 시 1번 점프 
### 20.11.14
 - Unity : 점프 중 점프 차단, 점프 중 방향 전환 차단
 - W(앞으로 이동) 입력이 없을 경우 달리기 차단
### 20.11.15
 - Unity : 3인칭 시점 구현, Alt 입력 지속 시간 동안 시점 자유롭게 변환
 - 캐릭터는 시점과 상관없이 Alt 입력 이전의 방향을 기준으로 컨트롤 
### 20.11.16
 - Blender : 원소(불, 얼음, 빛, 풀, 대지) 제작, 회전 애니메이션 제작 
 - Unity : 임포트 후 정상 작동 확인 
### 20.11.17
 - Unity : 캐릭터가 원소에 접촉해 있을 때 상호작용(E 입력) 시 해당 원소 획득
 - 동일한 원소 재획득 시 원소 강화(최대 +5 강화)
 - 원소 강화 시 데미지 증가
 - 가지고 있는 원소와 다른 원소 획득 시 강화 수치 초기화
 
    * 수정 : 회전 애니메이션 제거 -> Unity 내부 스크립트로 회전 구현 - 20.11.16 / Line 1
### 20.11.18
 - Unity : 원소 강화 +2강 이상일 경우 다른 종류의 원소를 먹으면 힐 하는 함수 추가(High risk, high return)
 - 캐릭터가 보고 있는 방향으로 AddForce하여 몸통 박치기 구현 //허공에 몸통 박치기 중 몸통 박치기를 하는 경우를 차단해야함
 
    * 수정 : 원소 강화 +5 강화 시 다른 원소 획득이 불가능한 버그 수정 - 20.11.17 / Line 1 ~ 2
    
    * 수정 : 캐릭터가 먹은 원소를 저장하는 변수에 GameObject가 저장되지 않는 버그 수정 - 20.11.17 / Line 1
       - ""원인 : GameObject 변수(myElement)에 GameObject(nearObj) 할당 시 값에 대한 할당이 아닌 참조이기에
                  원소 획득 후 nearObj에 대한 Destroy 과정에서 myElement 참조까지 Destroy""
       - 해결 : 원소 종류에 따라 Index 부여 후 할당, 비교
       
    * 수정 : 캐릭터의 이동이 없는 상태로 마우스로 몸을 회전시킬 경우 마지막으로 이동 시 바라 봤던 방향으로
             몸통 박치기 하는 버그 수정 - 20.11.18 / Line 2
       - 원인 : 캐릭터가 보는 방향을 할당하는 변수인 slimeForward를 사용 했는데 LookAround() 함수가 아닌
                Move() 함수 내에 있어 이동 입력이 있을 때만 방향이 할당
       - 해결 : slimeForward 할당문을 LookAround()로 이동
      
### 20.11.19
 - 고민 : 캐릭터 상태에 대한 flag가 점점 늘어나 가시성이 떨어지고 관리가 어려움 
    -> FSM(유한 상태 기계), 푸시다운 오토마타 ( 추후 개선 )
 
    * 수정 : 몸통 박치기 중 몸통 박치기가 되는 버그 수정 - 20.11.19 / Line 2
       - 해결 : Coroutine Delay를 통해 몸통 박치기 후 지연 시간 추가
### 20.11.21
 - Unity : 마우스 좌클릭 시 원소 투사체 발사 스킬 사용 구현, 원소와 관련된 색상의 이펙트 사용
 - 투사체가 바닥, 벽과 충돌 시 2초 후 파괴(파괴 전에 투사체와 충돌 시 피격)
 - 투사체가 충돌 후 2초 후 파괴 되는 것을 이용하여 벽에 튕기거나 바닥으로 굴리는 플레이 가능
 - Instatiate 함수는 if문 내부에 정의할 수 없어 Coroutine을 사용
 - 화면 중앙 크로스헤어 적용(화면 중앙에 작은 이미지 4개를 크로스헤어처럼 보이도록 활용)
 - 기본적으로 크로스헤어를 향해 스킬을 발사하지만 중력을 받는 곡사체임으로 오차 발생(화살과 같은 원리)
 ### 20.11.23
 - Unity : 원소 획득 이전에는 스킬 사용 불가, 원소 획득 시 해당 원소의 스킬 발사
    * 수정 : Alt 키 다운 후 스킬 사용 시 캐릭터 정면이 아닌 카메라가 보는 방향으로 스킬 발사하던 버그 수정 - 20.11.21 / Line 1
       - 원인 : 스킬 사용 시 LookAround() 함수의 cameraArm 벡터 변수를 사용하여 항상 카메라가 보는 방향으로 발사
       - 해결 : 조건문을 추가하여 Alt 입력 시 캐릭터의 바로 앞에만 스킬 발사 가능(Alt 미입력 시 위, 아래 자유롭게 발사 가능)
    * 버그 : 일정 각도 이상 땅을 바라보고 스킬 발사 시에 스킬이 바닥을 통과하는 버그 발생 ( 원인 확인중 )
 - 고민 : 모든 게임 원소들을 public으로 선언하여 Inspector에서 전부 할당해주고 switch 구문을 사용하여 번거로움이 있었다.
    -> Resources.Load를 활용하여 런타임 단계에서 에셋의 데이터(프리팹)을 로딩하여 사용할 수 있다. ( 추후 개선 )
    
  
