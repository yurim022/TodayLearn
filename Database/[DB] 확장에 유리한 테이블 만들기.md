## 확장에 유리한 테이블 만들기

서비스를 진행하다 보면, 초기 요구사항에서 계속해서 고도화하는 요구사항이 생겨서 테이블의 필드가 점점 늘어나게 되곤 한다.
새로운 테이블을 파서 진행하기엔 애매하고, 조인 성능도 걱정이되는데 필드가 너무 많아지는 것 같을때 어떻게 하는것이 좋을까???


### 1. 무작성 필드를 늘린다.

테이블이 가로로 길어진다. 조인은 안해도 되지만, 공간 낭비가 생긴다.
내 경우, 설정값 관련 테이블이여서 이력테이블처럼 양이 방대한 테이블은 아니지만, 매번 비슷한 성질의 새로운 필드가 추가되야 한다면 매번 DB 구조를 바꿔야한다. 


### 2. 필드에 옵션값을 추가한다.

동일한 속성의 필드가 계속 추가되는 것이라면, 해당 필드에 대한 타입을 추가함으로써 추가 column 생성을 막을 수 있다. 
이 경우 데이터가 세로로 늘어나는 것인데 조인보다는 조회해 오는게 더 효과적이므로 이 방법을 선택!!
