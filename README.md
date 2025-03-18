# PlayerAnimations
- A C++ Template for Creating Player-Animations for MC3DS at an extremely low-level.

## How it works:
- We don't need to overwrite every animation in game if we can add conditionals.
- With this we can gradually move our models over states and time (like BlockBench animations).
- Each cube/body-part has it's own Size, Offset, Rotation, UV Maps, and more.
- We need to set a succession states for each bone at a freed memory page i.e; `0x1E81000`->`0x1E82000`.
- Timers should be considered hard-coded with execution loop rates, such as `16.33ms` or `20-ticks` of the CPU.
- The value we need to set should be stored at the freed memory range with `0x24A` loops, or known as `16.33ms`.
- We cannot time our executions, so animations may not run at 100% speed at all times, but making the thread sleep; breaks the code, so states are required.
### Example of Working:
```cpp
#include "types.h"

namespace CTRPluginFramework
{
  void kickStonesAnim(MenuEntry *entry, u32 *keys){
    u16 animTimer = 0x2256;
    u32 static legOffset = 0xA66EC;
    if (!Process::IsPaused()){
      if (player->IsIdle() && !keys::key.CStick){ // CStick goes offline if player goes offline for ~5min
        for (int i = 1; i < animTimer/i; i++){ // simulate speed up for kicking up
          u32 currentValue = Process::Read(legOffset+0x04)
          Process::Write32(legOffset+0x04, currentValue+0x01); // rotation offset on axis.z
        }
        for (int i = anim; i < animTimer/(i*0.01); i--){ // simulate slow down up for bringing back down
          u32 currentValue = Process::Read(legOffset+0x04)
          Process::Write32(legOffset+0x04, currentValue-0x01); // rotation offset on axis.z
        }
      }
    }
  }
}
```
### Body Information in Skin Defs:
```
Body.Pivot.X = 0x50
Body.Pivot.Y = 0x5C
Body.Pivot.Z = 0x68

Body.Origin.X = 0x98
Body.Origin.Y = 0xA4
Body.Origin.Z = 0xB0

Body.Scale.X = 0xC8
Body.Scale.Y = 0xD4
Body.Scale.Z = 0xE0

Body.UV.X = 0xF8
Body.UV.Y = 0x104
````
