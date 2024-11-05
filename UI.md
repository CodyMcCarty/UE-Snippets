# Use common UI and MVVM to make Kenshi UI

## Enable these Plugins:
UMG Viewmodel  
Common UI Plugin
ProjectSettings.Engine.GeneralSettings.GameViewportClientClass = CommonGameViewportClient.  
follow [CommonUIQuickStart](https://dev.epicgames.com/documentation/en-us/unreal-engine/common-ui-quickstart-guide-for-unreal-engine)  

NOTES the W_Layout is CommonUserWidget or CommonActivatableWidget:
Leaning towards CAW, it inheirts from CUW.
UE Live, Cropout does CAW.
Narrative does CUW with stacks? and CAW

Make CommonUserWidget W_Layout  Names: UI_Layer_Game-Cropout, PrimaryGameLayout.h, W_OverallUILayout, WBP_PrimaryLayout, WBP_Hud, WBP_NarrativeHUD
Make Common User Widget W_NotificationBox, and CommonActivatableWidget
add SafeZone attach an Overlay, attach 4 CommonActivatableWidgetStacks and W_NotificationBox to the Overlay.
name the CAWs Game_Stack(HUD elements), GameMenu(Inventory, Dialogue, Maps), Menu(Settings), Modal(Confirmation and errors). // lower is on top

Cropout: OvrL; SafeZ; OvrL; Things, CAW 
LyraOvrall: OvrL; 4x CAW
LyraGame: SafeZ; OvrL; CanvasP; Cust ; ComUW
Narritive: Overlay; 3x CAW, W_Notify

CommonActivatableWidgetStack(layer)
Canvas Panel?
LayerStack? CommonActivatableWidgetStack. 

### Performance notes:
:rocket: Create Widget is expensive.  
:rocket: Tick is expensive.  
:rocket: Num of widgets visible is expensive.  
:rocket: Hit testable is expensive. 
:rocket: Nesting is expensive?  
:rocket: Texture size is expensive.  
:rocket: use UI Invalidation to increase performance for widgets that don't change often.    
:rocket: Animation is more expensive and limited than a material instance. Addtionally, changing material params doesn't invalidate the widget.   
:rocket: Show\hide or Create/Remove? see [UEDocs.OptimizationGuidelines.BreakComplexWidgets](https://dev.epicgames.com/documentation/en-us/unreal-engine/optimization-guidelines-for-umg-in-unreal-engine#breakcomplexwidgetsintopiecesthatcanloadatruntime)  
:rocket: Collapsed is more performant than hidden. Hidden layout is still calculated.  
:rocket: if it doesn't need click, HitTestInvisible or SelfHitTestInvisible. TopLevel, Behavior.Visibility.  
:rocket: CanvasPanel(multiple draw calls) and Overlay, to a lesser degree, is expensive.  Use GridPanel with nudge?
:rocket: Spacer is more perfromant than SizeBox(uses mutiple passes). Don't use SizeBox as a spacer.  
:rocket: Soft Ref for characters and actors. check reference chain and size.  
:rocket: Custom Icon font instead of textures. Unicode (0xf286 doens't work) values, FontForge, Fontello, FontAwesome. is not only more performant, it's easier to write text like a text message with emojis sytle. 
:rocket: Consider SMeshWidget for many instances of the same widget. ie. EnemyHealthBars, miniMapIcons, WorldUIs. is complicated. [github.YawLightouse.PerformanceRegardingWidgetComponent](https://github.com/YawLighthouse/UMG-Slate-Compendium?tab=readme-ov-file#perf-widget-components) or [TomLooman Lecture 14 - UMG With C++ & More Framework Extensions](https://courses.tomlooman.com/courses/1320807/lectures/32515407) for an easier approach (read comments for better impl).  

#### Other Tips // todo replace as used
:exclamation: Create base reuseable widgets: button, text, in order to change colors faster.   
:exclamation: Not updating? Save all, Select all widgets, Rclick, AssetActions->Reload.  
:exclamation: Named slot for templating in order to have a container, with a black border, a header, a footer, and content(the name slot). avoids duplication of widget to change a few things and enables mass edits.  
:exclamation: Layers allow showing and hiding widgets in order to avoid having a tighlighly coupled and hard to maintain system of widget telling other widgets to hide/show.  ie. Menu, inventory, pause, dialogue.  
:exclamation: Data into seperate object. MVVM, MVC, MVP type system.  
:exclamation: Bindings run every frame, except the View Model plugin's bindings. Turn off the binding option to avoid confussion.  
