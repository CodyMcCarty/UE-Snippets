### Controller ?
3 players. ListenServer
```cpp
// APawn::
NotifyControllerChanged()
{
  HasController
  Pie0 Pawn_0
  Pie0 Pawn_1
  Pie0 Pawn_2
  Pie1 Pawn_1
  Pie2 Pawn_2
}
OnRep_Controller()
{
  HasController. called twice. 2nd time with PState
  Pie1 Pawn_1
  Pie2 Pawn_2
}
PossessedBy(AController* NewController)
{
  Pie0 Pawn_0
  Pie0 Pawn_1
  Pie0 Pawn_2
}
BeginPlay()
{
  ControllerUnlikely
}
```

PieId | Pawn#
Pie0 Pawn_0 is the server's local pawn
Pie1 Pawn_1 is client 1's local pawn
Pie1 Pawn_0 is the server's proxy on client 1's machine.

### Is Local Player Controller?
```cpp
// no Pie1 Pawn_!1 && Pie2 Pawn!2
			if (IsLocallyControlled())
			{
				Pie0 Pawn_0
				Pie1 Pawn_1
				Pie2 Pawn_2
			}
			if (IsPlayerControlled())
			{
				Pie0 Pawn_0
				Pie0 Pawn_1
				Pie0 Pawn_2
				Pie1 Pawn_1
				Pie2 Pawn_2
			}
			if (IsBotControlled())
			{
				bIsBotControlled = true;
			}
			if (Controller->IsLocalController())
			{
				Pie0 Pawn_0
				Pie1 Pawn_1
				Pie2 Pawn_2
			}
			if (Controller->IsPlayerController())
			{
				Pie0 Pawn_0
				Pie0 Pawn_1
				Pie0 Pawn_2
				Pie1 Pawn_1
				Pie2 Pawn_2
			}
			if (Controller->IsLocalPlayerController())
			{
				Pie0 Pawn_0
				Pie1 Pawn_1
				Pie2 Pawn_2
			}
			if (GetNetMode() == NM_Client && GetLocalRole() == ROLE_AutonomousProxy)
			{
				Pie1 Pawn_1
				Pie2 Pawn_2
			}
```
Engine Notes:
```cpp
bool AController::IsLocalController() const
{
	const ENetMode NetMode = GetNetMode();

	if (NetMode == NM_Standalone)
	{
		// Not networked.
		return true;
	}
	
	if (NetMode == NM_Client && GetLocalRole() == ROLE_AutonomousProxy)
	{
		// Networked client in control.
		return true;
	}

	if (GetRemoteRole() != ROLE_AutonomousProxy && GetLocalRole() == ROLE_Authority)
	{
		// Local authority in control.
		return true;
	}

	return false;
}
```
