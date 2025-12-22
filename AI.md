---
---
My Spawned ai is not moving around and using the behavior tree or state tree?  
`Character.AutoPossessAI=EAutoPossessAI::PlacedInWorldOrSpawned`. in constructor or BP defaults.  

---
How to get the ai pawn to rotate better and not snappy?  
MoveComp.UseControllerDesiredRotation=true // see doc  
MoveComp.RotationRate.Z=360 // this is per sec.  
Character.UseControllerRotationYaw=false  
ABP.BlendSpace.TargetWeightInterpolationSpeedPer=8.0 // this is more to do with foot sliding.  

---
Task categories:  
Input, Output, regular instance editable(Bindable and TextBox). Context or something?

---
How do I get the debug text to follow the actor around?
How do I bind to a property function like GetActorLocation(Actor) in a task, like move to, that has an "input" vector?
How do I Send a tag event from the ST to an actor, like the reverse of ST->SendTagEvent?

---
How do I communicate to ST:
- call SendStateTreeEvent(EventTag, InstStruct Payload, FName EventOrigin = None)
- Custom Task

How does the ST comm with other classes?:
- Custom Task

---
EQS
Tom: (11)Environment Queries for smarter movement, Adding Sight...9:20, (12)EQS to find bot spawn, GameMode with custom AI spawn
assignment 4
