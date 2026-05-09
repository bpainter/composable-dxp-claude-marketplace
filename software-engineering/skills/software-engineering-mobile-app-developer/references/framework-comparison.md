---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[mobile-app-developer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Mobile Framework Comparison (2025)

## Framework Overview

### React Native
**Best for**: Teams with JavaScript expertise, fast iterations, MVP to scale

**Strengths**:
- Large ecosystem and community
- Code sharing across iOS/Android
- Fast development with hot reload
- Mature third-party libraries
- OTA updates via CodePush

**Trade-offs**:
- Performance overhead vs. native (bridging)
- Platform-specific bugs require debugging
- Bundle size larger than native

**2025 Status**: 121k GitHub stars. Stable with continued investment from community.

### Flutter
**Best for**: Cross-platform with native performance, custom UIs, visual consistency

**Strengths**:
- Excellent performance (direct to GPU rendering)
- Rich widget library and customization
- Single language (Dart) for both platforms
- Strong type system
- Skia graphics engine for consistent visuals

**Trade-offs**:
- Smaller ecosystem than React Native
- Dart is less widely known than JavaScript
- Slightly longer learning curve
- Hot reload can mask state issues

**2025 Status**: 170k GitHub stars. Growing adoption, especially for complex UIs.

## Performance Comparison (2025)

| Metric | React Native | Flutter | Native |
|--------|--------------|---------|--------|
| Startup Time | 1.5-2.5s | 0.8-1.2s | 0.2-0.5s |
| Runtime FPS | 55-60 | 59-60 | 60 |
| App Size | 40-60MB | 30-50MB | 15-30MB |
| Development Speed | ★★★★★ | ★★★★ | ★★★ |
| Platform Access | ★★★★ | ★★★★ | ★★★★★ |

## Decision Matrix

Choose **React Native** if:
- Team has JavaScript/TypeScript skills
- Time-to-market is critical
- You need rapid iteration
- Web and mobile code sharing helps

Choose **Flutter** if:
- Performance is critical
- Complex, custom UI needed
- Team can learn Dart
- Consistency across platforms is priority

Choose **Native** if:
- Maximum performance required
- Deep platform features needed
- You have dedicated iOS/Android teams
- Game or graphics-heavy app

## Hybrid Approach

Modern teams often use:
- **Core logic**: Shared Dart/Kotlin Multiplatform Mobile (KMM) or Flutter
- **UI**: Platform-specific when needed
- **Web**: Flutter Web or React for same codebase
