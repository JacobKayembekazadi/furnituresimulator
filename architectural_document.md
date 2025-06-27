# Interactive 3D Living Room Simulator - Architecture Document

## Table of Contents
1. [Application Overview](#application-overview)
2. [Main Components](#main-components)
3. [Data Models](#data-models)
4. [Core Workflows](#core-workflows)
5. [Technology Stack](#technology-stack)
6. [Architecture Diagrams](#architecture-diagrams)
7. [Performance Considerations](#performance-considerations)
8. [Future Enhancements](#future-enhancements)

## Application Overview

### Purpose
The Interactive 3D Living Room Simulator is a web-based application that provides users with an immersive, ultra-realistic 3D environment for interior design visualization and furniture arrangement. The application enables real-time manipulation of furniture objects within a photorealistic living room setting.

### Key Features
- **Ultra-realistic 3D rendering** with advanced lighting and post-processing effects
- **Interactive furniture manipulation** with drag-and-drop functionality
- **Real-time lighting simulation** including natural window light and artificial lighting
- **Material simulation** with physically-based rendering (PBR) materials
- **Environmental reflections** for enhanced realism
- **Responsive camera controls** for comprehensive scene exploration

### Target Users
- Interior designers and architects
- Homeowners planning room layouts
- Furniture retailers for product visualization
- Students learning 3D design concepts

## Main Components

### Frontend Components

#### 1. Scene Manager
- **Responsibility**: Core 3D scene initialization and management
- **Key Functions**: Scene setup, camera configuration, renderer initialization
- **Dependencies**: Three.js core library

#### 2. Rendering Engine
- **Responsibility**: Visual output generation with advanced effects
- **Key Functions**: Post-processing pipeline, shadow mapping, tone mapping
- **Dependencies**: Three.js post-processing modules

#### 3. Lighting System
- **Responsibility**: Realistic lighting simulation
- **Key Functions**: Multiple light types management, shadow casting
- **Dependencies**: RectAreaLightUniformsLib

#### 4. Material System
- **Responsibility**: Realistic surface appearance
- **Key Functions**: Texture generation, PBR material creation
- **Dependencies**: Canvas API for procedural textures

#### 5. Furniture Factory
- **Responsibility**: 3D furniture object creation and management
- **Key Functions**: Geometry generation, material assignment, positioning
- **Dependencies**: Three.js geometry classes

#### 6. Interaction Controller
- **Responsibility**: User input handling and object manipulation
- **Key Functions**: Drag controls, orbit controls, event management
- **Dependencies**: Three.js control libraries

#### 7. UI Layer
- **Responsibility**: User interface and instructions display
- **Key Functions**: Control instructions, responsive layout
- **Dependencies**: CSS, HTML5

### Backend Components
**Note**: This is a client-side only application with no backend infrastructure.

### Database
**Note**: No persistent data storage is implemented. All scene state is maintained in memory.

### External Integrations
- **Three.js CDN**: Core 3D graphics library
- **Google Fonts**: Typography (Inter font family)

## Data Models

### Scene Data Model
```mermaid
classDiagram
    class Scene {
        +THREE.Scene scene
        +THREE.Color background
        +Object3D[] children
        +add(object)
        +remove(object)
        +traverse(callback)
    }
    
    class Camera {
        +THREE.PerspectiveCamera camera
        +Number fov
        +Number aspect
        +Number near
        +Number far
        +Vector3 position
        +updateProjectionMatrix()
    }
    
    class Renderer {
        +THREE.WebGLRenderer renderer
        +Boolean shadowMapEnabled
        +Number toneMappingExposure
        +setSize(width, height)
        +render(scene, camera)
    }
```

### Furniture Data Model
```mermaid
classDiagram
    class FurnitureObject {
        +THREE.Group group
        +Vector3 position
        +Vector3 rotation
        +Vector3 scale
        +Object userData
        +Boolean castShadow
        +Boolean receiveShadow
        +traverse(callback)
    }
    
    class Sofa {
        +THREE.Mesh base
        +THREE.Mesh[] cushions
        +THREE.Mesh back
        +THREE.Mesh[] arms
        +Material sofaMaterial
    }
    
    class TVStand {
        +THREE.Mesh stand
        +THREE.Mesh tv
        +Material woodMaterial
        +Material screenMaterial
    }
    
    class CoffeeTable {
        +THREE.Mesh top
        +THREE.Mesh[] legs
        +Material glassMaterial
        +Material metalMaterial
    }
    
    class BookShelf {
        +THREE.Mesh shelf
        +THREE.Mesh[] books
        +Material woodMaterial
        +Material[] bookMaterials
    }
    
    FurnitureObject <|-- Sofa
    FurnitureObject <|-- TVStand
    FurnitureObject <|-- CoffeeTable
    FurnitureObject <|-- BookShelf
```

### Material Data Model
```mermaid
classDiagram
    class MaterialSystem {
        +THREE.MeshStandardMaterial floorMaterial
        +THREE.MeshStandardMaterial wallMaterial
        +THREE.MeshStandardMaterial sofaMaterial
        +THREE.MeshStandardMaterial glassMaterial
        +THREE.MeshStandardMaterial metalMaterial
        +createWoodFloorTexture()
        +createFabricTexture()
    }
    
    class Texture {
        +HTMLCanvasElement canvas
        +CanvasRenderingContext2D context
        +Number width
        +Number height
        +Boolean wrapS
        +Boolean wrapT
        +Vector2 repeat
    }
    
    class Material {
        +Color color
        +Number roughness
        +Number metalness
        +Number opacity
        +Boolean transparent
        +Texture map
        +Texture envMap
    }
    
    MaterialSystem --> Texture
    MaterialSystem --> Material
```

### Lighting Data Model
```mermaid
classDiagram
    class LightingSystem {
        +THREE.RectAreaLight windowLight
        +THREE.HemisphereLight fillLight
        +THREE.PointLight lampLight
        +THREE.CubeCamera cubeCamera
        +initializeLights()
        +updateReflections()
    }
    
    class Light {
        +Color color
        +Number intensity
        +Vector3 position
        +Vector3 rotation
        +Boolean castShadow
    }
    
    LightingSystem --> Light
```

## Core Workflows

### Application Initialization Workflow
```mermaid
sequenceDiagram
    participant User
    participant Browser
    participant App
    participant ThreeJS
    participant Scene
    
    User->>Browser: Load index.html
    Browser->>App: Parse HTML/CSS
    Browser->>ThreeJS: Load Three.js libraries
    App->>Scene: Initialize scene
    App->>Scene: Setup camera
    App->>Scene: Configure renderer
    App->>Scene: Create lighting
    App->>Scene: Generate materials
    App->>Scene: Build room geometry
    App->>Scene: Create furniture objects
    App->>Scene: Setup controls
    App->>Scene: Start render loop
    Scene->>User: Display 3D room
```

### Furniture Interaction Workflow
```mermaid
sequenceDiagram
    participant User
    participant DragControls
    participant OrbitControls
    participant FurnitureObject
    participant Renderer
    
    User->>DragControls: Click on furniture
    DragControls->>OrbitControls: Disable orbit controls
    DragControls->>FurnitureObject: Set emissive highlight
    User->>DragControls: Drag furniture
    DragControls->>FurnitureObject: Update position
    DragControls->>FurnitureObject: Maintain Y position
    User->>DragControls: Release furniture
    DragControls->>FurnitureObject: Remove highlight
    DragControls->>OrbitControls: Enable orbit controls
    DragControls->>Renderer: Trigger re-render
```

### Rendering Pipeline Workflow
```mermaid
sequenceDiagram
    participant AnimationLoop
    participant OrbitControls
    participant CubeCamera
    participant Composer
    participant Renderer
    participant Screen
    
    AnimationLoop->>OrbitControls: Update camera
    AnimationLoop->>CubeCamera: Update reflections
    AnimationLoop->>Composer: Begin render pass
    Composer->>Renderer: Execute render pass
    Composer->>Composer: Apply bloom effect
    Composer->>Screen: Output final frame
    AnimationLoop->>AnimationLoop: Request next frame
```

### Texture Generation Workflow
```mermaid
sequenceDiagram
    participant MaterialSystem
    participant Canvas
    participant Context2D
    participant ThreeJS
    
    MaterialSystem->>Canvas: Create canvas element
    MaterialSystem->>Context2D: Get 2D context
    MaterialSystem->>Context2D: Draw base pattern
    MaterialSystem->>Context2D: Add detail lines
    MaterialSystem->>ThreeJS: Create CanvasTexture
    MaterialSystem->>ThreeJS: Configure wrapping
    MaterialSystem->>ThreeJS: Set repeat values
    ThreeJS->>MaterialSystem: Return texture
```

## Technology Stack

### Frontend Technologies

#### Core Libraries
| Technology | Version | Purpose |
|------------|---------|---------|
| **Three.js** | r128 | 3D graphics rendering and scene management |
| **HTML5** | Latest | Document structure and canvas element |
| **CSS3** | Latest | Styling and responsive layout |
| **JavaScript** | ES6+ | Application logic and interaction handling |

#### Three.js Modules
| Module | Purpose |
|--------|---------|
| **OrbitControls** | Camera manipulation and scene navigation |
| **DragControls** | Object selection and movement |
| **EffectComposer** | Post-processing pipeline management |
| **RenderPass** | Basic scene rendering |
| **UnrealBloomPass** | Bloom lighting effects |
| **RectAreaLightUniformsLib** | Advanced lighting calculations |

#### External Dependencies
| Service | Purpose |
|---------|---------|
| **Google Fonts CDN** | Inter font family for UI typography |
| **Three.js CDN** | Library delivery and version management |

### Runtime Environment
- **Browser**: Modern web browsers with WebGL support
- **WebGL**: Hardware-accelerated 3D graphics
- **Canvas API**: 2D texture generation
- **RequestAnimationFrame**: Smooth animation loops

### Development Tools
- **No build process**: Direct HTML/CSS/JS development
- **CDN delivery**: No local dependency management required
- **Hot reload**: Browser refresh for development iterations

## Architecture Diagrams

### High-Level System Architecture
```mermaid
graph TB
    subgraph "Client Browser"
        subgraph "Presentation Layer"
            UI[HTML/CSS UI]
            Instructions[Control Instructions]
        end
        
        subgraph "Application Layer"
            App[Main Application]
            Controls[Interaction Controllers]
            Materials[Material System]
        end
        
        subgraph "3D Graphics Engine"
            Scene[Three.js Scene]
            Renderer[WebGL Renderer]
            Camera[Perspective Camera]
        end
        
        subgraph "Asset Management"
            Textures[Procedural Textures]
            Geometry[Furniture Geometry]
            Lighting[Lighting System]
        end
    end
    
    subgraph "External Services"
        CDN[Three.js CDN]
        Fonts[Google Fonts]
    end
    
    UI --> App
    Instructions --> Controls
    App --> Scene
    Controls --> Camera
    Materials --> Textures
    Scene --> Renderer
    Scene --> Geometry
    Scene --> Lighting
    App --> CDN
    UI --> Fonts
```

### Component Interaction Diagram
```mermaid
graph LR
    subgraph "Core Components"
        SM[Scene Manager]
        RE[Rendering Engine]
        LS[Lighting System]
        MS[Material System]
        FF[Furniture Factory]
        IC[Interaction Controller]
    end
    
    subgraph "Three.js Framework"
        Scene[THREE.Scene]
        Renderer[THREE.WebGLRenderer]
        Camera[THREE.PerspectiveCamera]
        Controls[THREE.Controls]
    end
    
    SM --> Scene
    RE --> Renderer
    RE --> Scene
    LS --> Scene
    MS --> Scene
    FF --> Scene
    IC --> Controls
    IC --> Camera
    
    Scene --> Renderer
    Camera --> Renderer
```

### Data Flow Diagram
```mermaid
flowchart TD
    Start([Application Start]) --> Init[Initialize Three.js Scene]
    Init --> CreateMaterials[Generate Procedural Materials]
    CreateMaterials --> BuildRoom[Create Room Geometry]
    BuildRoom --> CreateFurniture[Generate Furniture Objects]
    CreateFurniture --> SetupLighting[Configure Lighting System]
    SetupLighting --> SetupControls[Initialize User Controls]
    SetupControls --> StartRender[Begin Render Loop]
    
    StartRender --> UpdateControls[Update Camera Controls]
    UpdateControls --> UpdateReflections[Update Environment Maps]
    UpdateReflections --> RenderFrame[Render Frame with Post-Processing]
    RenderFrame --> CheckResize{Window Resized?}
    CheckResize -->|Yes| ResizeRenderer[Resize Renderer & Camera]
    CheckResize -->|No| NextFrame[Request Next Frame]
    ResizeRenderer --> NextFrame
    NextFrame --> UpdateControls
    
    UserInput[User Interaction] --> DragStart[Drag Start Event]
    DragStart --> DisableOrbit[Disable Orbit Controls]
    DisableOrbit --> HighlightObject[Highlight Selected Object]
    HighlightObject --> UpdatePosition[Update Object Position]
    UpdatePosition --> DragEnd{Drag Complete?}
    DragEnd -->|No| UpdatePosition
    DragEnd -->|Yes| EnableOrbit[Enable Orbit Controls]
    EnableOrbit --> RemoveHighlight[Remove Object Highlight]
```

### Scene Hierarchy Diagram
```mermaid
graph TD
    Scene[THREE.Scene] --> Floor[Floor Mesh]
    Scene --> Wall1[Back Wall]
    Scene --> Wall2[Accent Wall]
    Scene --> WindowGroup[Window Group]
    Scene --> Artwork[Wall Art]
    Scene --> Lights[Lighting System]
    Scene --> Furniture[Furniture Objects]
    Scene --> CubeCamera[Reflection Camera]
    
    WindowGroup --> WindowPane[Glass Pane]
    WindowGroup --> WindowFrame[Window Frame]
    
    Lights --> WindowLight[Rect Area Light]
    Lights --> FillLight[Hemisphere Light]
    Lights --> LampLight[Point Light]
    
    Furniture --> Sofa1[Sofa Instance 1]
    Furniture --> Sofa2[Sofa Instance 2]
    Furniture --> TVStand[TV Stand]
    Furniture --> CoffeeTable[Coffee Table]
    Furniture --> BookShelf[Book Shelf]
    
    Sofa1 --> SofaBase[Base Mesh]
    Sofa1 --> SofaCushions[Cushion Meshes]
    Sofa1 --> SofaBack[Back Rest]
    Sofa1 --> SofaArms[Arm Rests]
```

## Performance Considerations

### Rendering Optimization
- **Shadow Map Resolution**: Optimized for balance between quality and performance
- **Post-Processing**: Selective bloom effects with configurable thresholds
- **Geometry Optimization**: Efficient polygon counts for furniture objects
- **Texture Resolution**: Balanced texture sizes for visual quality vs. memory usage

### Memory Management
- **Geometry Reuse**: Shared geometries for similar objects (sofa instances)
- **Material Sharing**: Common materials across multiple objects
- **Texture Caching**: Procedural textures generated once and reused
- **Garbage Collection**: Proper cleanup of Three.js objects when needed

### Browser Compatibility
- **WebGL Support**: Requires modern browsers with WebGL capabilities
- **Hardware Acceleration**: Benefits from dedicated GPU rendering
- **Mobile Considerations**: Responsive design but optimized for desktop interaction

## Future Enhancements

### Planned Features
1. **Furniture Library**: Expandable catalog of furniture types
2. **Room Customization**: Configurable room dimensions and layouts
3. **Material Editor**: User-customizable material properties
4. **Scene Persistence**: Save and load room configurations
5. **VR Support**: Virtual reality integration for immersive experience
6. **Lighting Controls**: User-adjustable lighting scenarios
7. **Collision Detection**: Realistic furniture placement constraints
8. **Physics Integration**: Realistic object interactions and gravity

### Technical Improvements
1. **Performance Optimization**: LOD (Level of Detail) system for complex furniture
2. **Asset Loading**: External 3D model support (GLTF/GLB)
3. **Progressive Enhancement**: Graceful degradation for lower-end devices
4. **Accessibility**: Screen reader support and keyboard navigation
5. **Testing Framework**: Automated testing for 3D scene functionality

### Architecture Evolution
1. **Modular Design**: Plugin architecture for extending functionality
2. **State Management**: Redux-like state management for complex interactions
3. **Component System**: Entity-Component-System architecture for scalability
4. **Backend Integration**: Server-side scene persistence and user accounts
5. **Real-time Collaboration**: Multi-user room design sessions

---

*Document Version: 1.0*  
*Last Updated: June 26, 2025*  
*Architecture Team: Senior Software Architect*
