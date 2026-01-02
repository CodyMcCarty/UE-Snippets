---
The usual setup is for leaf states to contain the action tasks, and intermediate states to have tasks for configuring or claiming resources.

---
Try to shoot for fewer states and a 100s tasks. View task as composing behavior. Many small tasks

---
My Spawned ai is not moving around and using the behavior tree or state tree?  
`Character.AutoPossessAI=EAutoPossessAI::PlacedInWorldOrSpawned`. in constructor or BP defaults.  

---
How to get the ai pawn to rotate better and not snappy?  
MoveComp.UseControllerDesiredRotation=true // see doc  
MoveComp.RotationRate.Z=360 // this is per sec.  
OrientRotationToMovement = false  
Character.UseControllerRotationYaw=false  
AIC.SetControlRotationFromPawnOrientation=false  
ABP.BlendSpace.TargetWeightInterpolationSpeedPer=8.0 // this is more to do with foot sliding.  

---
What are the categories in a stt statetree task? 
Input, Output, Context(actor or AIC), Parameter?, regular instance editable(Bindable and TextBox. not clear if it's an input or output. input & output require a binding).  

---
How do I get the debug text to follow the actor around? Bind it's ReferencedActor.  

---
Hows do I comm between ST and actors?
- gameplay tags
- Task & Custom STTask
- EQS Task
- call SendStateTreeEvent(EventTag, InstStruct Payload, FName EventOrigin = None)
  - Payload: MakeInstancedStruct, Enter Conditions to use data. Transistions to transistion
- FStateTreePropertyRef to Read&Write [First 60 ST](https://dev.epicgames.com/community/learning/tutorials/lwnR/unreal-engine-your-first-60-minutes-with-statetree).  

---
Evaluator can be used to get some other actor from the world. Then that can be an input in a task. STQuckStart. Like Resolver from MVVM  
Has TreeStart, Stop, and Tick. Doens't run when needed, runs like begin play.  
Evaluator - These have largely been phased out in favor of using Global Tasks. Global Tasks handle the same uses as Evaluators. [First 60 ST](https://dev.epicgames.com/community/learning/tutorials/lwnR/unreal-engine-your-first-60-minutes-with-statetree).  

---
How do I debug ST?  
Open RewindDebugger to see what state is selected and info about it (looks great) in the Rewind debugger details(a second panel).  
There's a break on Enter/Exit & bEnable in the top right drop down, or right click state or task  
Conditions can force eval to true.  

---
EQS
Tom: (11)Environment Queries for smarter movement, Adding Sight...9:20, (12)EQS to find bot spawn, GameMode with custom AI spawn, (15)Assignme4, more...

---
How do I bind to a property function like GetActorLocation(Actor) in a task, like move to, that has an "input" vector?  
How do I Send a tag event from the ST to an actor, like the reverse of ST->SendTagEvent?  
Is IGameplayTagAssetInterface used anywhere?  
Would I need a cool down in ST, and how?  
What's up with Evaluators?  
What's up with Global Tasks? Source of info?  
I want to avoid BP_AIC & BP_Bot in the schema and use Epic's framework. Can I use Comp or interface?  
Task Any & All complete: try making a task where all have to compete, this should remove a state. There's a toggle completion to avoid traps.  
Delegates?:
  Delegate.  Time of day changes to night, ST reacts and goes to sleep.  
    There is a Trigger on delegate. Maybe an evaluator's TreeStarted to get the delegate? like begin play
  ST broadcast a delegate and AIC reacts I guess.
