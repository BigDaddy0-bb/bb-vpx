# Dune VPX - Performance Optimization Changelog
> **Optimized by Antigravity / BB**

This patch drastically reduces script overhead, eliminates stuttering, and rebalances the 4K / Surround Sound Feedback (SSF) configurations for high-end cabinet builds.

## 🚀 Performance
- **Guarded Redundant COM Writes**: Modified the 10ms `RealTime_Timer` interval to only write Flipper (`.RotZ`) and Spinner (`.RotY`) angles to the COM interface when the angle has physically changed. This prevents thousands of redundant COM writes per frame.
- **VBScript Hot-Loop Caching**: Rewrote the `RollingUpdate` subroutine to locally cache all ball attributes (`X`, `Y`, `Z`, `VelZ`) into VBScript variables at the top of the loops. This eliminates extremely slow VPX engine reads during per-ball evaluations.
- **Dynamic Ball Shadow Engine Overhaul**: Optimized the `DynamicBSUpdate` engine. Shadow positioning `(bZ + BallSize) / 80` calculations are purely cached per ball instead of being evaluated iteratively. Shadow primitives' `.visible` property is now explicitly guarded to eliminate redundant overwrites.

## 🔊 Audio & Tacile Feedback
- **Hardware SSF Multiplier**: Added an upstream `SSF_Gain = 0.5` global multiplier bounding tactile responses. This automatically cuts cabinet mechanical shaker/slapper intensity (Bumpers and Slingshots) by 50% without altering the 3D Surround `AudioPan` and `AudioFade` physics mapping.
- **Cabinet 4K INI Override**: Packaged a standalone `DUNE_v1.4.ini` setting:
  - Exclusive Fullscreen (`ExclusiveMode=1`) to prevent Windows OS stuttering.
  - Disabled Super-Sampling Anti-Aliasing (`SSAA=0`) to prevent the 780M from scaling at 8K and crushing frames.
  - Enables standalone `B2SWindows=1` EXE offloading for smoother backglass operation on Ryzen hardware.
