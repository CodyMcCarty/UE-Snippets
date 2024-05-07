


---
Blueprint output pin
```cpp
UFUNCTION(BlueprintCallable)
bool Foo(float& OutDamage);
```

---
Interfaces. [Steve's write up](https://www.stevestreeting.com/2020/11/02/ue4-c---interfaces---hints-n-tips/). 
```cpp
UFUNCTION(BlueprintCallable, BlueprintNativeEvent)
void Interact(APawn* InstigatorPawn);

Interact_Implementation() // bc of BlueprintNativeEvent

if (HitActor->Implements<USGameplayInterface>()) // "U" not "I" 
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
`TActorRange<AController>(GetWorld())` > `(TActorIterator<AMyAICharacter> It(GetWorld()); It; ++It) ter* Bot = *It;` > `UGameplayStatics::GetAllActorsOfClass()`  
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
