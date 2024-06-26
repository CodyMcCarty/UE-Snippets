[Collision Data in UE5: Practical Tips for Managing Collision Settings & Queries | Unreal Fest 2023](https://dev.epicgames.com/community/learning/talks-and-demos/3KWR/unreal-engine-collision-data-in-ue5-practical-tips-for-managing-collision-settings-queries-unreal-fest-2023)

# Basics   
Trace Channels   
- Visibility  
- Camera

Object Channels (Type) can be used for objects.  Object type and Object channel seem to be the same
- WorldStatic
- WorldDynamic
- Pawn
- PhysicsBody
- Vehicle
- Destructible  

Presets (Profile)   
![image](https://github.com/CodyMcCarty/UE-Snippets/assets/68304541/caa5fe36-300b-4782-9be6-a80921abf28b)    


Example of Pawn(CapsuleComp) and CharacterMesh   
![image](https://github.com/CodyMcCarty/UE-Snippets/assets/68304541/82bbc4ce-488c-4bd3-9f94-6a41e7c926df) ![image](https://github.com/CodyMcCarty/UE-Snippets/assets/68304541/fb64a48e-d67c-4fe2-be38-f57d15d47db4)    

![image](https://github.com/CodyMcCarty/UE-Snippets/assets/68304541/6cbc9a2c-fdc1-4051-9fe9-8114104f015d) ![image](https://github.com/CodyMcCarty/UE-Snippets/assets/68304541/7cde1e46-0124-4a15-a453-a4e8710d53af)

### Collision Filtering
Uses least blocking, example of overlap    
![image](https://github.com/CodyMcCarty/UE-Snippets/assets/68304541/cee24fdb-fba2-47d3-81da-3f6ea60ef01d)

# Managing Collision Settings  
Try to avoid making new Trace and object channels.  
Preset name base on what it used for, not responses. eg EnemySightBlocker not NoCollisionExceptVisibility  
Ok to have duplicates eg LargeTree and Wall  
Avoid OverlapAll ?   

# Using Queries Effectively
LineTraceTestByProfile, SweepMultiByObjectType, OverlapBlockingTestByProfile, etc.  

- LineTraceSingleByChannel = ( ECollisionChannel TraceChannel) basic raycast. stops on 1st blocking hit. Channel=Trace's channel, FCollisionResponseParams for filtering, FHitResult contains hit info.  
- LineTraceSingleByProfile = (FName ProfileName) same has LineTraceSingleByChannel, but pick a channel, ie TEXT("PlayerCapsule") (Pawn).  is Preset ?
- LineTraceSingleByObjectType = 1st thing hit with supplied Object type, no collision filtering(responses).  ObjectType = WorldStatic, WorldDynamic, Pawn, PhysicsBody, Destructible, Vehicle

3 parts
#### First:
LineTrace = Raycast    
Sweep = Raycast with a sphere, capsule, box    
Overlap = x  
#### Second:  
Single = 1st blocking hit  
Multi = Stops on 1st blocking, returns all overlaps in Tarray.  
Test = true/false, more preformant.    
#### Third:  
ByChannel(ECollisionChannel TraceChannel) = Visiblity or Camera Channel.    
ByProfile(FName ProfileName) = Profile name is same as Preset ie IgnoreOnlyPawn, CharacterMesh   
ByObjectType() = x  
#### Special
OverlapBlockingTest = x  
OverlapAnyTest = x  

Ignore characters when tracing for IK     
	FCollisionResponseParams ResponseParams;   
	ResponseParams.CollisionResponse.SetResponse(ECC_Pawn, ECR_Ignore);   
	GetWorld()->LineTraceSingleByChannel(Hit, Start, End, Channel, QueryParams, ResponseParams);    
 Mirror CharacterMoveComp::FindFloor

 Movement blockers use capsule colilsion shape and profile.  

Shot through enemies when Enemies have Block response to Weapon Channel and avoid adding new collision channel  
	FCollisionResponseParams ResponseParams;  
	ResponseParams.CollisionResponse.SetResponse(ECC_Pawn, ECR_Overlap);  
	GetWorld()->LineTraceMultiByChannel(Hits, Start, End, Channel, QueryParams, ResponseParams);  
 

 CollisionQueryTestActor  
![image](https://github.com/CodyMcCarty/UE-Snippets/assets/68304541/da2db2c6-a5de-4e7d-8112-7c9eb1d74014)  


# Case Studies 33:00    

#Detailed notes   
ThridPersonTemplate Walls and Floor = Default  
BluePhysicsCubes = PhysicsActor  
Landscape = BlockAll  
NewMap foor = BlockAllDynamic  


| Name | Collision | Object Type | Visibility | Camera | Objects Notes |
| --- | --- | --- | --- | --- | --- |
| NoCollision | NoCollision | WorldStatic | Ignore | Ignore | None, but block all |
| BlockAll | Collision Enabled (Query and Physics) | WorldStatic | Block | Block | BlockAll |
| BlockAllDynamic | Query Only (No Physics Collision) | WorldDynamic | Block | Block | BlockAll |
| IgnoreOnlyPawn | Query Only (No Physics Collision) | WorldDynamic | Block | Block | Ig(Pawn,Vic)BlockRest |
| OverlapOnlyPawn | Query Only (No Physics Collision) | WorldDynamic | Block | Ignore | Olp(Pawn,Vic)BlockRest |
| Pawn | Collision Enabled (Query and Physics) | Pawn | Ignore | Block | BlockAll |
| Spectator | Query Only (No Physics Collision) | Pawn | Ignore | Ignore | Bk(WStatic)IgRest |
| CharacterMesh | Query Only (No Physics Collision) | Pawn | Ignore | Block | Ig(Pawn,Vic)BlockRest |
| PhysicsActor | Collision Enabled (Query and Physics) | PhysicsBody | Block | Block | BlockAll |
| Destructible | Collision Enabled (Query and Physics) | Destructible | Block | Block | BlockAll |
| InvisibleWall | Collision Enabled (Query and Physics) | WorldStatic | Ignore | Block | BlockAll |
| InvisibleWallDynamic | Collision Enabled (Query and Physics) | WorldDynamic | Ignore | Block | BlockAll |
| Trigger | Query Only (No Physics Collision) | WorldDynamic | Ignore | Overlap | OverlapAll |
| OverlapAll | Query Only (No Physics Collision) | WorldS/D | Overlap | Overlap | OverlapAll |
| Ragdoll | Collision Enabled (Query and Physics) | PhysicsBody | Ignore | Block | Ig(Pawn)BlockRest |
| Vehicle | Collision Enabled (Query and Physics) | Vehicle | Block | Block | BlockAll |
| UI | Query Only (No Physics Collision) | WorldDynamic | Block | Overlap | OverlapAll |




