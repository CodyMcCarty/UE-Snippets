# Use common UI and MVVM to make Kenshi UI GameplayMessageSystem????

## Enable these Plugins:
UMG Viewmodel  
Common UI Plugin
ProjectSettings.Engine.GeneralSettings.GameViewportClientClass = CommonGameViewportClient.  
follow [CommonUIQuickStart](https://dev.epicgames.com/documentation/en-us/unreal-engine/common-ui-quickstart-guide-for-unreal-engine)  

Make CommonActivatableWidget W_Layout. Can be a normal UserWidget, CUW(Narritive, Lyra), or CAW(UELive, Cropout)  Names: UI_Layer_Game-Cropout, PrimaryGameLayout.h, W_OverallUILayout, WBP_PrimaryLayout, WBP_Hud, WBP_NarrativeHUD
Make Common User Widget W_NotificationBox
add SafeZone attach an Overlay, attach 4 CommonActivatableWidgetStacks and W_NotificationBox to the Overlay.  
:exclamation: Layers allow showing and hiding widgets in order to avoid having a tighlighly coupled and hard to maintain system of widget telling other widgets to hide/show.  ie. Menu, inventory, pause, dialogue.  
name the CAWSs Game_Stack(HUD elements), GameMenu_Stack(Inventory, Dialogue, Maps), Menu_Stack(Settings), Modal_Stack(Confirmation and errors). // lower is on top
CAWSs and Overlay Fill Horizontal and Vertical. Visibility=NotHitTestableSelf.  TODO: play with Overlay's SafeAreaScale, Game_Stack.RootContentWidgetClass is default WBP?    
:rocket: Hit testable is expensive. if it doesn't need click, HitTestInvisible or SelfHitTestInvisible. TopLevel, Behavior.Visibility.  
:rocket: CanvasPanel(multiple draw calls) and Overlay, to a lesser degree, is expensive.  Use GridPanel with nudge when possible?
In Event Graph make Custom Event for Push and Remove(GetActiveWidget).  Push if !GetActiveWidget == widget.  



Make CommonActivatiableWidget(CAW) Game/      
smaller= Common, 
diff between CommonButtonBase & BoundAction
More on button styles
### Performance notes:
:rocket: Create Widget is expensive.  
:rocket: Tick is expensive.  
:rocket: Num of widgets visible is expensive.  
 
:rocket: Nesting is expensive?  
:rocket: Texture size is expensive.  
:rocket: use UI Invalidation to increase performance for widgets that don't change often.    
:rocket: Animation is more expensive and limited than a material instance. Addtionally, changing material params doesn't invalidate the widget.   
:rocket: Show\hide or Create/Remove? see [UEDocs.OptimizationGuidelines.BreakComplexWidgets](https://dev.epicgames.com/documentation/en-us/unreal-engine/optimization-guidelines-for-umg-in-unreal-engine#breakcomplexwidgetsintopiecesthatcanloadatruntime)  
:rocket: Collapsed is more performant than hidden. Hidden layout is still calculated.  
:rocket: Spacer is more perfromant than SizeBox(uses mutiple passes). Don't use SizeBox as a spacer.  
:rocket: Soft Ref for characters and actors. check reference chain and size.  
:rocket: Custom Icon font instead of textures. Unicode (0xf286 doens't work) values, FontForge, Fontello, FontAwesome. is not only more performant, it's easier to write text like a text message with emojis sytle. 
:rocket: Consider SMeshWidget for many instances of the same widget. ie. EnemyHealthBars, miniMapIcons, WorldUIs. is complicated. [github.YawLightouse.PerformanceRegardingWidgetComponent](https://github.com/YawLighthouse/UMG-Slate-Compendium?tab=readme-ov-file#perf-widget-components) or [TomLooman Lecture 14 - UMG With C++ & More Framework Extensions](https://courses.tomlooman.com/courses/1320807/lectures/32515407) for an easier approach (read comments for better impl).  

#### Other Tips // todo replace as used
:exclamation: Create base reuseable widgets: button, text, in order to change colors faster. Defaults can be set in Project Settings.     
:exclamation: Not updating? Save all, Select all widgets, Rclick, AssetActions->Reload.  
:exclamation: Named slot for templating in order to have a container, with a black border, a header, a footer, and content(the name slot). avoids duplication of widget to change a few things and enables mass edits.  
:exclamation: Data into seperate object. MVVM, MVC, MVP type system.  
:exclamation: Bindings run every frame, except the View Model plugin's bindings. Turn off the binding option to avoid confussion.  
