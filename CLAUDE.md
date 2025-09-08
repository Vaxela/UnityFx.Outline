# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

UnityFx.Outline is a Unity package that provides configurable per-object and per-camera outline effects for Unity projects. It supports multiple render pipelines including Built-in RP, Universal Render Pipeline (URP), High Definition Render Pipeline (HDRP), and Post-processing Stack v2.

## Commands

### Testing
```bash
# Open Unity and use Test Runner window in the editor
# Tests are located in Outline.Core/Assets/Tests/Editor/Scripts/
# Run tests through Unity's Test Runner: Window > General > Test Runner
```

### Publishing Packages
```bash
# Publish packages to npm registry
./NpmPublish.bat
```

### Unity Operations
```bash
# Open each Unity project individually:
# - Outline.Core (main core package)
# - Outline.URP (Universal Render Pipeline integration) 
# - Outline.HDRP (High Definition Render Pipeline integration)
# - Outline.PostProcessing (Post-processing Stack v2 integration)
```

## Architecture

### Multi-Package Structure
The project uses a modular architecture with separate Unity packages for different render pipelines:

- **Core Package** (`Outline.Core/Packages/UnityFx.Outline/`): Contains the main outline implementation
- **URP Package** (`Outline.URP/Packages/UnityFx.Outline.URP/`): Universal Render Pipeline integration
- **HDRP Package** (`Outline.HDRP/Packages/UnityFx.Outline.HDRP/`): High Definition Render Pipeline integration  
- **PostProcessing Package** (`Outline.PostProcessing/Packages/UnityFx.Outline.PostProcessing/`): Post-processing Stack v2 integration

### Key Components

#### Core Classes (Outline.Core/Packages/UnityFx.Outline/Runtime/Scripts/)
- **OutlineEffect**: MonoBehaviour for per-camera outlines, attaches to cameras
- **OutlineBehaviour**: MonoBehaviour for per-object outlines, attaches to GameObjects
- **OutlineRenderer**: Low-level rendering helper using CommandBuffers
- **OutlineLayer**: Groups GameObjects with shared outline settings
- **OutlineLayerCollection**: Collection of OutlineLayers that can be shared between effects
- **OutlineSettings**: ScriptableObject for reusable outline configurations
- **OutlineResources**: Contains shader and material references needed for rendering

#### Rendering System
- Uses Unity CommandBuffers for efficient GPU rendering
- Supports both solid and blurred (Gaussian) outline modes
- MaterialPropertyBlock-based rendering for performance
- Procedural geometry rendering on SM3.5+ platforms
- Support for depth testing and alpha testing

#### Shader Architecture (Runtime/Shaders/)
- **Outline.shader**: Multi-pass shader for outline rendering
- **OutlineColor.shader**: Color-only outline variant
- HLSL-based implementation compatible with different render pipelines

### Unity Project Structure
Each render pipeline has its own Unity project:
- Separate `ProjectSettings/` configurations
- Individual `Packages/manifest.json` dependencies
- Dedicated example scenes and test assets
- Pipeline-specific settings (URP assets, HDRP settings, etc.)

### Testing Framework
- Unit tests using NUnit framework
- Located in `Outline.Core/Assets/Tests/Editor/Scripts/`
- Tests for core components: OutlineBehaviour, OutlineLayer, OutlineSettings
- Editor-only tests using Unity Test Framework

### Package Dependencies
- **Core**: Requires Unity 2018.4+, no external dependencies
- **URP**: Depends on core + `com.unity.render-pipelines.universal` 7.0.0+
- **HDRP**: Depends on core + HDRP packages (version TBD)
- **PostProcessing**: Depends on core + Post-processing Stack v2

## Development Notes

### Render Pipeline Support
When working with render pipeline specific code:
- Built-in RP: Use `OutlineEffect` and `OutlineBehaviour` directly
- URP: Use `OutlineFeature` added to renderer features
- HDRP: Implementation pending
- Post-processing v2: Use as post-processing effect override

### Performance Considerations
- Uses MaterialPropertyBlocks to avoid material instantiation
- Command buffer rendering for GPU efficiency  
- Procedural geometry on supported platforms
- No external dependencies to minimize overhead

### Platform Compatibility
- Supports Windows/Mac standalone, Android, iOS, WebGL
- Mobile-optimized shaders with unroll statements for WebGL 1.0
- XR compatible (Multi Pass, Single Pass Instanced)

### Version Management
- Uses Semantic Versioning (SemVer)
- Current core version: 0.8.5
- Each package maintains independent versioning
- NPM packages published to npmjs.com under @unityfx scope