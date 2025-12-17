# MVVM
- Disable old school property binding that ticks
- UE's is not standard MVVM. It's an Unrealism.
- Make small, concise & reusable VMs (hard to do) think about debugging and test data. Composistion over inheirtance. e.g. Don't make a PlayerVM that has a BasicVM & HealthVM. Instead the widget uses a PlayerBasicVM & PlayerHealthVM & multiple other VMs.
- No ptrs to players or widget. Think easy to sterilize. Data only with lots of state and avoid direct comms. e.g. State = NotConnected, Requesting, Connecting, Failed, Connected
- Self supporting. e.g. Health is only float in VM. View makes it a 75% progress bar or 6/8 hearts or some material.
- Prefer Fetch(resolver, manual?) over setter. Fetch is in direction of gameplay doesn't know about UI. UMG can go get VM. Manual want a specific VM instance. Resolver Global GetOrCreate, one instance.
- Some say KISS. some say good place to house complicated bits together and leave Game code in Game and View code in UI.
- Function Prefix: CF_ for conversion. VM_ for setter, updater, _Event to bind to.   Viewmodel category if they can't be made generic/reuseable in BPFL.
  - Custom funcs can take in two MV vars.
- Style VM?
- SelectionVM for UI navigation.

# UMG
- Consider stack box instead of verticle or horizontal box.
- Use Global Invalidation or Invalidation panel 26:00[20 umg](https://dev.epicgames.com/community/learning/talks-and-demos/MPZq/unreal-engine-20-things-you-should-be-using-in-unreal-motion-graphics-umg-unreal-fest-gold-coast-2024) with some debugging
  - Choose materials over W_. Materials run on highly parallelised GPU, reduces batches, no layout invalidation, reduces num of W_.
- Use less widgets. 31:00[20 umg](https://dev.epicgames.com/community/learning/talks-and-demos/MPZq/unreal-engine-20-things-you-should-be-using-in-unreal-motion-graphics-umg-unreal-fest-gold-coast-2024)
- Some W_ have 2 prepasses. ScaleBox, SizeBox, RichText(sometimes), Text(with AutoWrap, wrap at pixel size=multi line.
  - e.g. if heiarchy is Overlay->SizeBox->ScaleBox->Text, replacing that with Overlay->Overlay->Overlay->Text goes from 500us to 25us.
- Templating: Use material functions to create an "interface" between materials. i.e. params have the same name across different materials so the BP logic remains the same.
- Dynamic sizing & Size to content: wrap box, scroll box, marquee scrolling (ticker tap) in CommonText 12:00[20 umg](https://dev.epicgames.com/community/learning/talks-and-demos/MPZq/unreal-engine-20-things-you-should-be-using-in-unreal-motion-graphics-umg-unreal-fest-gold-coast-2024).
- Materials over textures have many benefits (cons being time to setup) [x-playform UI dev](https://youtu.be/u06GAVxyIag?si=6rf2w0-dtQFTWtFh&t=1486). Many materials are avalible in UI Material Lab and Lyra 

# Common UI
- Com Visual Attachment doesn't take up layout space (notification, controller button)
- Common Button & Text are amazing.
- Lyra Cross-platform UI Development | Tech Talk | State of Unreal 2022. Mostly about common UI than cross platform. stacks, queues, short consise explinations, input routing, .  
