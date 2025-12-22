---
Asserts & EDD, error driven development. Lots of bugs from not assigning vars in BP and this should be done.  
`if(ensureMsgf(thing, TEXT("Behavior Tree is nullptr! Please assign BehaviorTree in your AI Controller."))) {}`  
format is "The Problem! Solution"  
Break point at c++ code. & Logs the file, class, ln., msg. IIRC it should log the call stack as well.  

---
Drag select actors in editor? Ctrl + Alt + LMB

---
DebugCam https://twitter.com/flassari/status/1631925402171056128 


---
Casey's/UE Switch style. 
```cpp
switch (BestPos.X)
{
case aINT32_MAX // collon, but error with github's .md
	{
		BestIndex = INT32_MAX;
	}
	break;
case INDEX_NONE
	{
		BestIndex = INDEX_NONE;
	}
	break;
default
	{
		BestIndex = BestPos.Y * GridSize.X + BestPos.X;
	}
	break;
}
```

---
remove elements from a TArray    
for (auto It = Stacks.CreateIterator(); It; ++It) // is the Unreal way to deal with TArray and is BP friendly.    
for (int32 i = Stacks.Num() - 1; i >= 0; i--) // is faster with 1000+ elements. Stop doing this so much.    

---
Get crash data in packaged game   
Ari's crashing with style 10:00min  
Editor Preferences → General - Experimental → Blueprints → Blueprint Break on Exceptions  
ScriptStackOnWarnings=true // todo find the better way to enable  
https://dev.epicgames.com/community/learning/tutorials/dXl5/advanced-debugging-in-unreal-engine  
{,,UnrealEditor-Engine.dll}GPlayInEditorContextString to watch in Rider  

---
Debug macro to prevent the compiler from optimizing code away   
```cpp
UE_DISABLE_OPTIMIZATION / UE_ENABLE_OPTIMIZATION
```

---
Change the return of a getter  
```cpp
ULyraAbilitySystemComponent* GetLyraAbilitySystemComponent() const;

ULyraAbilitySystemComponent* ULyraAttributeSet::GetLyraAbilitySystemComponent() const
{
	return Cast<ULyraAbilitySystemComponent>(GetOwningAbilitySystemComponent());
}
```

---
Enum to String  
```cpp
// Note, I believe this doesn't work if cooked.
const ECollisionChannel CurrentChannel = static_cast<ECollisionChannel>(i);
FString Str 	= StaticEnum<ECollisionChannel>()->GetValueAsString(static_cast<ECollisionChannel>(i));
Str 		= StaticEnum<ECollisionChannel>()->GetValueAsString(CurrentChannel)

Capsule UMETA(DisplayName = "Capsule"), // add this to use with created enums.
// alternatives
EShape ShapeA = EShape::Capsule;
FString Str 	= StaticEnum<EShape>()->GetValueAsString(ShapeA); 				// EShape::Capsule
FText Text 	= StaticEnum<EShape>()->GetDisplayNameTextByValue(static_cast<int64>(ShapeA)); 	// Capsule
FName Name 	= StaticEnum<EShape>()->GetNameByValue(static_cast<int64>(ShapeA)); 		// EShape::Capsule
FString StrA 	= StaticEnum<EShape>()->GetNameStringByValue(static_cast<int64>(ShapeA)); 	// Capsule
// ect. GetNameByIndex is prolly the most performant if the index is known. use GetIndexByName to get the index
```  

---
Lighting  
SnowWhite={Color=0.85, Metal=0, Rough=0.3}  
CoalBlack{Color=0.04, Metal=0, Rough=0.3}  
127Grey={Color=0.18, Metal=0, Rough=0.3} /** Exposure is not linear, 3.5 4.5 9 18 36 72 95 */   
Chrome={Color=0.9, Metal=1, Rough=0.05}   

---
Blueprint output pin
```cpp
UFUNCTION(BlueprintCallable)
bool Foo(float& OutDamage);
//vs ref input
bool Foo(UPARAM(ref) float& OutDamage);
```

---
Interfaces. [Steve's write up](https://www.stevestreeting.com/2020/11/02/ue4-c---interfaces---hints-n-tips/). 
```cpp
UFUNCTION(BlueprintCallable, BlueprintNativeEvent)
void Interact(APawn* InstigatorPawn);

Interact_Implementation() // bc of BlueprintNativeEvent

if (HitActor->Implements<USGameplayInterface>()) // "U" not "I". without check returns garbage
{
    ISGameplayInterface::Execute_Interact(HitActor, MyPawn); // HitActor calls Interact(MyPawn)
}
```

---
```cpp
void USAttributeComponent::PostEditChangeProperty(FPropertyChangedEvent& PropertyChangedEvent)
{
	Super::PostEditChangeProperty(PropertyChangedEvent);
	if (PropertyChangedEvent.GetPropertyName() == GET_MEMBER_NAME_CHECKED(USAttributeComponent, HealthMax))
	{
		Health = HealthMax; 
	}
}
```

---
Vector Math  
```cpp
// Target - start
FVector Direction = TargetActor->GetActorLocation() - MuzzleLocation;
FRotator MuzzleRotation = Direction.Rotation();
GetWorld()->SpawnActor(ProjectileClass, MuzzleLocation, MuzzleRotation, SpawnParams);
```

---
use find component instead of cast  
```cpp 
Comp = OtherActor->FindComponentByClass<USAttributeComponent>();    
Comp = Cast<USAttributeComponent>(OtherActor->GetComponentByClass(USAttributeComponent::StaticClass()));

// Most Getters have a templated version
Get<t>()
```

---
Category="User Options" "User Options - Functions" "- Events"
"Information
AdvancedDisplay

---
Steam file  
`$ MyGame\Binaries\Win64  echo "480" >> steam_appid.txt`

---
Get all actors  
`TActorRange<AController>(GetWorld()) // Doesn't work with subclasses?` > `for(TActorIterator<AMyAICharacter> It(GetWorld()); It; ++It) ter* Bot = *It;` > `UGameplayStatics::GetAllActorsOfClass()`  
`for (const AMyAICharacter* Bot : TActorRange<AMyAICharacter>(GetWorld()))`

---
Which Client? they're all Client 0    
```cpp
int32 PlayInEditorID = GPlayInEditorID;  
GEngine->AddOnScreenDebugMessage(-1, 15.f, FColor::Yellow, FString::Printf(TEXT("Client %d OnRep_ReplicatedVar"), PlayInEditorID));
```

---
Macro for delegate
```cpp
// Original
AbilitySystemComponent->GetGameplayAttributeValueChangeDelegate(AuraAttributeSet->GetHealthAttribute()).AddLambda(
    [this](const FOnAttributeChangeData& Data)
    {
        OnHealthChanged.Broadcast(Data.NewValue);
    }
);

// Macro
#define BIND_CALLBACK( AttributeName) AbilitySystemComponent->GetGameplayAttributeValueChangeDelegate(AuraAttributeSet->Get##AttributeName##Attribute()).AddLambda( \
    [this](const FOnAttributeChangeData& Data) \
    {\
        On##AttributeName##Changed.Broadcast(Data.NewValue);
    }
);
BIND_CALLBACK(Health);
BIND_CALLBACK(MaxHealth);
BIND_CALLBACK(Mana);
#undef BIND_CALLBACK S
```

---
binding input to a function in another class | Lambda
```cpp
EnhancedInputComp->BindAction(Input_Interact, ETriggerEvent::Triggered, this->InteractionComp.Get(), &USInteractionComp::PrimaryInteract);

// Lambda delegate
MyDelegate.AddLambda([this](const FGameplayTagContainer& Tags)
    {
        // Do something with Tags
    });
```
