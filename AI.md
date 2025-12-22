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
What are the categories in a stt statetree task? 
Input, Output, Context(actor or AIC), regular instance editable(Bindable and TextBox). 

---
How do I get the debug text to follow the actor around?
How do I bind to a property function like GetActorLocation(Actor) in a task, like move to, that has an "input" vector?
How do I Send a tag event from the ST to an actor, like the reverse of ST->SendTagEvent?
Is IGameplayTagAssetInterface used anywhere?
Would I need a cool down in ST, and how?
What's up with Evaluators?
What's up with Global Tasks? Source of info?

---
Is there a gpTag interface to allows better communication between BPs?
How do I use the event payload?
Hows do I comm between ST and other actors?
- gameplay tags
- Custom STTask
  
How do I communicate to ST?
- call SendStateTreeEvent(EventTag, InstStruct Payload, FName EventOrigin = None)


---
EQS
Tom: (11)Environment Queries for smarter movement, Adding Sight...9:20, (12)EQS to find bot spawn, GameMode with custom AI spawn, (15)Assignme4, more...
