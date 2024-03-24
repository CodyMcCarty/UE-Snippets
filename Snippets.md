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

Vector Math  
```cpp
// Target - start
FVector Direction = TargetActor->GetActorLocation() - MuzzleLocation;
FRotator MuzzleRotation = Direction.Rotation();
GetWorld()->SpawnActor(ProjectileClass, MuzzleLocation, MuzzleRotation, SpawnParams);
```

use find component instead of cast  
`Comp = OtherActor->FindComponentByClass<USAttributeComponent>();`
`Comp = Cast<USAttributeComponent>(OtherActor->GetComponentByClass(USAttributeComponent::StaticClass()));`

Category="User Options" "User Options - Functions" "- Events"
"Information
AdvancedDisplay

Steam file  
`$ MyGame\Binaries\Win64  echo "480" >> steam_appid.txt`

Get all actors  
`TActorRange<AController>(GetWorld())` > `UGameplayStatics::GetAllActorsOfClass()`

Which Client? they're all Client 0    
```cpp
int32 PlayInEditorID = GPlayInEditorID;  
GEngine->AddOnScreenDebugMessage(-1, 15.f, FColor::Yellow, FString::Printf(TEXT("Client %d OnRep_ReplicatedVar"), PlayInEditorID));
```

binding input to a function in another class  
`EnhancedInputComp->BindAction(Input_Interact, ETriggerEvent::Triggered, this->InteractionComp.Get(), &USInteractionComp::PrimaryInteract);`
