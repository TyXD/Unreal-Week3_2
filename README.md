# [내일배움캠프 사전캠프] Unreal C++로 AI 구성하기.

## 1. 오늘 학습 키워드

>### 블랙보드

>### Behavior Tree

>### Custom Node

## 2. 오늘 학습 한 내용을 나만의 언어로 정리하기

**Unreal Engine의 AI 시스템**에서 중요한 두 가지 요소인 **블랙보드(Blackboard)**와 AI **에셋(Behavior Tree)**가 있다.

이 두 가지는 AI 시스템을 구성하는 핵심적인 역할을 가지고 있다.

**블랙보드**는 **AI의 기억 저장소**로, AI 캐릭터가 목표 위치나 특정 상태와 같은 데이터(변수)를 저장하고 관리하는 역할을 하고 있다.

**Behavior Tree**는 AI의 **의사결정 구조**를 시각적으로 설계할 수 있는 도구로, 블랙보드에 저장된 데이터를 기반으로 AI의 행동패턴을 결정한다.

>### 네비매쉬 바운드 볼륨 생성

Unreal Engine에서 AI가 길을 찾기위한 방법중 하나가 바로 네비게이션 메시라는 땅을 그래프 그리듯 찾아가는 방법이다.

이 방법으로 길을 찾기 위해 해당 땅을 지정하고자 사용하는 것이 **네비매시 바운드 볼륨**으로 해당 영역은 AI가 그래프로 인지 할 수 있는 땅으로 구성된.

>### 블랙보드 생성하기

**블랙보드는 Unreal Behavior Tree에서 사용되는 자료들을 선언해두는 역할을 맡고 있다.**

C++에서 header(.h) 파일에 기본적인 자료형을 선언해서 사용과 유사한 형태로, 필요한 자료형을 설정해둘 수 있다.

- 콘텐츠 브라우저에서 “콘텐츠/AI” 경로상에 우클릭하여 **“인공지능 > 블랙보드”** 를 선택하여 생성한 뒤, “BB_AI”로 파일명을 설정해준다.

- BB_AI 파일을 더블 클릭하여 열어서, 데이터 내용을 확인할 수 있다.

기본으로 생성 된 SelfActor의 경우, AI가 소속 된 Actor를 나타낸다.

- “새 키” 버튼을 클릭하면 새로운 데이터를 추가해줄 수 있다.
- 이번 예시에서는 임의에 이동정보를 저장하는 벡터 자료형을 저장하고 이름(키 이름)을 TargetPosition으로 설정을 해 다른 코드에서 사용할 준비를 했다.

>### Behavior Tree

**Behavior Tree는 AI에 행동패턴을 설정하는 역할을 맡고 있다.** 

블랙보드에 선언된 데이터를 기반으로, 이동 / 공격 / 순찰 등 다양한 행동패턴을 설정하는 자료이다.

**Behavior Tree의 경우,** 단어 의미대로 행동(Behavior)을 나무(Tree) 구조로 작성이 가능한 구조이다.

- 콘텐츠 브라우저에서 “콘텐츠/AI” 경로상에 우클릭하여 **“인공지능 > 비헤이비어 트리”** 를 선택하여 생성한 뒤, “BT_AI”로 파일명을 설정해준다.

- BT_AI 파일을 더블 클릭하여 열여서, 데이터의 내용을 확인할 수 있다.

우측 **Behavior Tree**상에 만들어둔 BB_AI 인지 확인하고, 만약 다른 파일이 들어있다면 변경한다.

- 우클릭을 하여 새로운 노드들을 추가할 수 있다. 이번엔 Behavior Tree에서 대표적으로 많이 사용되는 **Selector**와 **Sequence**에 대해서 공부해 보았다.

1. **Selector** : C++에서 if / if else / else 구조와 유사한 형태입니다.

   코드 구조로 비유하면 다음과 같습니다. 만약 true로 return 되는 값이 있다면,

   해당 node를 진행하고, 아니라면 다음 node가 선택되어 진행합니다.

```cpp
//1번째 선택지
if(n > 5)
{
    return true;
}
//2번째 선택지
else if(n > 3)
{
		return true;
}
//아닌 경우
else
{
		return false;
}

```


- **Sequence** : 연결 된 Node들을 **순서대로 실행**한다.
- 실행 순서는 Node에 우측 상단을 보면 숫자를 통해 확인 할 수 있다.

<img width="916" height="829" alt="Image" src="https://github.com/user-attachments/assets/83a43946-60f6-4735-b6dd-98b6d1f04f07" />

이 두개의 영역 박스 종류는 Behavior Tree안에서 드래그를 하여 먼저 진행되는 순서를 변경할 수 있다.

>### Custom Node

Unreal에서 기본적으로 To Move 와 Wait Node를 제공해주긴 하지만, 적을 찾는다던가, 적을 공격하는 AI를 만들기 위해선 해당 기능을 구현해야한다.

C++를 통해 적을 찾아 임의의 위치로 이동하는 AI**Custom Node**과 적이 일정 시야내에 있으면 적을 향해 사격하는 AI **Custom Node**를 만들어 보았다.

- C++클래스 경로상에 우클릭하여 C++ 추가 혹은 상단 툴을 클릭한 뒤 C++ 추가 버튼을 클릭해준다.

- Custom Node를 생성하기 위하여 “모든 클래스”를 선택한 뒤, “BTTaskNode” 를 부모 클래스로 선택한다.

- 적을 발견하면 사격하는 “BTT_Attack”과 랜덤한 위치로 이동하는 “BTT_Move”를 각각 클래스명을 설정하고, AI 폴더를 경로로 설정 한 뒤 클래스들을 작성하였다.

- 파일 구조는 다음과 같습니다

**BTT_Attack.h**

```cpp
#pragma once

#include "CoreMinimal.h"
#include "BehaviorTree/BTTaskNode.h"
#include "BTT_Attack.generated.h"

/**
 * 
 */
UCLASS()
class BASIS_API UBTT_Attack : public UBTTaskNode
{
	GENERATED_BODY()

public:

	UBTT_Attack();
	virtual EBTNodeResult::Type ExecuteTask(UBehaviorTreeComponent& BTC, uint8* NodeMemory) override;

	UPROPERTY(EditAnywhere)
	float AttackRange;
	
	UPROPERTY(EditAnywhere)
	float SightRange;

	UPROPERTY(EditAnywhere)
	float SightAngle;
};

```

**BTT_Move.h**

```cpp

#pragma once

#include "CoreMinimal.h"
#include "BehaviorTree/BTTaskNode.h"
#include "BTT_Move.generated.h"

/**
 * 
 */
UCLASS()
class BASIS_API UBTT_Move : public UBTTaskNode
{
	GENERATED_BODY()
	
public:
	UBTT_Move();
	virtual EBTNodeResult::Type ExecuteTask(UBehaviorTreeComponent& BTC, uint8* NodeMemory) override;
};

```

- ExecuteTask 는 노드가 실행 될 때, 호출되는 함수로서 **EBTNodeResult 결과를 반환**합니다. 반환 결과에 따라서 **Selector에 분기가 나뉘어집니다.**

**BTT_Attack.cpp**

```cpp
#include "BTT_Attack.h"
#include "CharacterBase.h"
#include "PlayerBase.h"
#include "BehaviorTree/BlackboardComponent.h"
#include "AIController.h"
#include "Kismet/KismetMathLibrary.h"

UBTT_Attack::UBTT_Attack()
{
	NodeName = TEXT("Attack");
}

EBTNodeResult::Type UBTT_Attack::ExecuteTask(UBehaviorTreeComponent& BTC, uint8* NodeMemory)
{
	/*
	1. 시야내에 있어야한다.
	2. 적을 볼때 가림막이 없어야 한다.
	3. 공격 범위 내에 있어야 한다.
	*/

	AAIController* Controller = Cast<AAIController>(BTC.GetOwner());
	if (!IsValid(Controller))
	{
		return EBTNodeResult::Failed;
	}

	ACharacterBase* CB = Cast<ACharacterBase>(Controller->GetPawn());

	if (!IsValid(CB))
	{
		return EBTNodeResult::Failed;
	}

	UWorld* World = GetWorld();
	if (!IsValid(World))
	{
		return EBTNodeResult::Failed;
	}

	APlayerController* PC = World->GetFirstPlayerController();
	if (!IsValid(PC))
	{
		return EBTNodeResult::Failed;
	}

	APlayerBase* PB = Cast<APlayerBase>(PC->GetPawn());
	if (!IsValid(PB))
	{
		return EBTNodeResult::Failed;
	}

	FVector Distance = PB->GetActorLocation() - CB->GetActorLocation();
	if (Distance.Length() > SightRange)
	{
		return EBTNodeResult::Failed;
	}

	FVector AIForward = CB->GetActorForwardVector();

	Distance.Normalize();

	AIForward.Normalize();

	float DotResult = AIForward.Dot(Distance);

	float AngleBetweenVector = FMath::Acos(DotResult);

	if (SightAngle < AngleBetweenVector)
	{
		return EBTNodeResult::Failed;
	}


	FCollisionQueryParams QueryParams;
	QueryParams.AddIgnoredActor(CB);

	FHitResult HitResult;
	World->LineTraceSingleByChannel(HitResult, CB->GetActorLocation(), PB->GetActorLocation(), ECollisionChannel::ECC_Camera, QueryParams);

	if (!HitResult.bBlockingHit)
	{
		return EBTNodeResult::Failed;
	}

	if (HitResult.GetActor() != PB)
	{
		return EBTNodeResult::Failed;
	}

	UBlackboardComponent* BBC = BTC.GetBlackboardComponent();
	if (!IsValid(BBC))
	{
		return EBTNodeResult::Failed;
	}
	BBC->SetValueAsVector(TEXT("TargetPosition"), PB->GetActorLocation());

	Distance = PB->GetActorLocation() - CB->GetActorLocation();
	if(Distance.Length() > AttackRange)
	{
		return EBTNodeResult::Failed;
	}
	
	BTC.GetAIOwner()->StopMovement();

	FRotator Rot = UKismetMathLibrary::FindLookAtRotation(CB->GetActorLocation(), PB->GetActorLocation());
	CB->SetActorRotation(Rot);

	CB->Attack();

	return EBTNodeResult::Succeeded;
}

```


**BTT_Move.cpp**

```cpp

#include "BTT_Move.h"
#include "BehaviorTree/BlackboardComponent.h"
#include "AIController.h"
#include "NavigationSystem.h"

UBTT_Move::UBTT_Move()
{
	NodeName = TEXT("Move");

}

EBTNodeResult::Type UBTT_Move::ExecuteTask(UBehaviorTreeComponent& BTC, uint8* NodeMemory)
{
	UBlackboardComponent* BBC = BTC.GetBlackboardComponent();
	if (!IsValid(BBC))
	{
		return EBTNodeResult::Failed;
	}

	FVector TargetPosition = BBC->GetValueAsVector(TEXT("TargetPosition"));

	AAIController* AIC = BTC.GetAIOwner();
	if (!IsValid(AIC))
	{
		return EBTNodeResult::Failed;
	}
	APawn* Pawn = AIC->GetPawn();
	if (!IsValid(Pawn))
	{
		return EBTNodeResult::Failed;
	}

	if((TargetPosition - Pawn->GetActorLocation()).Length()< 100)
	{	
		UNavigationSystemV1* NavSystem = UNavigationSystemV1::GetNavigationSystem(GetWorld());

		FNavLocation LOC;
		NavSystem->GetRandomPoint(LOC);

		BBC->SetValueAsVector(TEXT("TargetPosition"), LOC.Location);
	}
	AIC->MoveToLocation(TargetPosition);
	return EBTNodeResult::Succeeded;

}

```

NavigationSystem을 코드에서 활용하기 위해서는 Build.cs 파일에서 `NavigationSystem` 추가해야 코드에서 정상적으로 인용이 가능하다.

- 코드 작성을 완료한 뒤, Unreal 상에서 **Behavior Tree** 에서 “BTT_Move”와 “BTT_Attack”을 검색하면, 위에서 만든 CustomNode들이 추가 된 것을 알 수 있다.

- 설정한 TargetPosition으로 이동하기 위하여, BTT_Move 노드를 추가하여, Sequence로 순서대로 재생하게 연결 한 뒤, 블랙보드 키 값을 “TargetPosition” 으로 설정해주면 이동한다.


## 3. 내일 학습 할 것은 무엇인지

남은 UI를 추가하고 적 AI가 자동으로 소환되게 만들어 TPS 게임의 틀을 잡을 예정이다.

#내일배움캠프 #사전캠프 #TIL 
