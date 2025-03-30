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
Body.Size.X = 0xC8
Body.Size.Y = 0xD4
Body.Size.Z = 0xE0
Body.UV.X = 0xF8
Body.UV.Y = 0x104

Head.Pivot.X = 0x134
Head.Pivot.Y = 0x140
Head.Pivot.Z = 0x14C
Head.Origin.X = 0x17C
Head.Origin.Y = 0x188
Head.Origin.Z = 0x194
Head.Size.X = 0x1AC
Head.Size.Y = 0x1B8
Head.Size.Z = 0x1C4
Head.UV.X = 0x1DC
Head.UV.Y = 0x1E8

Hat.Pivot.X = 0x218
Hat.Pivot.Y = 0x224
Hat.Pivot.Z = 0x230
Hat.Origin.X = 0x260
Hat.Origin.Y = 0x26C
Hat.Origin.Z = 0x278
Hat.Size.X = 0x290
Hat.Size.Y = 0x29C
Hat.Size.Z = 0x2A8
Hat.UV.X = 0x2C0
Hat.UV.Y = 0x2CC

RightArm.Pivot.X = 0x314
RightArm.Pivot.Y = 0x320
RightArm.Pivot.Z = 0x32C
RightArm.Origin.X = 0x35C
RightArm.Origin.Y = 0x368
RightArm.Origin.Z = 0x374
RightArm.Size.X = 0x38C
RightArm.Size.Y = 0x398
RightArm.Size.Z = 0x3A4
RightArm.UV.X = 0x3BC
RightArm.UV.Y = 0x3C8

LeftArm.Pivot.X = 0x3F8
LeftArm.Pivot.Y = 0x404
LeftArm.Pivot.Z = 0x410
LeftArm.Origin.X = 0x440
LeftArm.Origin.Y = 0x44C
LeftArm.Origin.Z = 0x458
LeftArm.Size.X = 0x470
LeftArm.Size.Y = 0x47C
LeftArm.Size.Z = 0x488
LeftArm.UV.X = 0x4A0
LeftArm.UV.Y = 0x4AC

RightLeg.Pivot.X = 0x4E8
RightLeg.Pivot.Y = 0x4F4
RightLeg.Pivot.Z = 0x500
RightLeg.Origin.X = 0x530
RightLeg.Origin.Y = 0x53C
RightLeg.Origin.Z = 0x548
RightLeg.Size.X = 0x560
RightLeg.Size.Y = 0x56C
RightLeg.Size.Z = 0x578
RightLeg.UV.X = 0x590
RightLeg.UV.Y = 0x59C

LeftLeg.Pivot.X = 0x5CC
LeftLeg.Pivot.Y = 0x5D8
LeftLeg.Pivot.Z = 0x5E4
LeftLeg.Origin.X = 0x614
LeftLeg.Origin.Y = 0x620
LeftLeg.Origin.Z = 0x62C
LeftLeg.Size.X = 0x644
LeftLeg.Size.Y = 0x650
LeftLeg.Size.Z = 0x65C
LeftLeg.UV.X = 0x674
LeftLeg.UV.Y = 0x680

````
