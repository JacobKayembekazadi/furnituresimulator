# 🏠 Interactive 3D Living Room Simulator

[![Demo](https://img.shields.io/badge/Demo-Live-brightgreen)](https://jacobkayembekazadi.github.io/furnituresimulator/)
[![Three.js](https://img.shields.io/badge/Three.js-r128-blue)](https://threejs.org/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

An ultra-realistic, interactive 3D living room simulator built with Three.js. Experience photorealistic rendering, advanced lighting effects, and intuitive furniture manipulation in real-time.

![Furniture Simulator Preview](https://via.placeholder.com/800x400/1a1a1a/ffffff?text=3D+Living+Room+Simulator)

## ✨ Features

### 🎨 **Ultra-Realistic Rendering**
- **Physically Based Rendering (PBR)** materials
- **Real-time lighting simulation** with multiple light sources
- **Advanced post-processing** with bloom effects
- **Environmental reflections** and shadows
- **High-quality procedural textures**

### 🎮 **Interactive Experience**
- **Drag-and-drop furniture** positioning
- **Smooth camera controls** (orbit, zoom, pan)
- **Real-time material updates**
- **Responsive design** for all devices

### 🛋️ **Furniture Collection**
- Multiple sofa configurations
- Glass coffee table with metal legs
- Wooden TV stand with mounted display
- Bookshelf with randomized books
- Expandable furniture library

### 🏠 **Room Environment**
- Realistic room proportions
- Natural window lighting
- Accent wall with metallic finish
- Wood floor with procedural textures
- Wall-mounted artwork

## 🚀 Quick Start

### Online Demo
Visit the live demo: [https://jacobkayembekazadi.github.io/furnituresimulator/](https://jacobkayembekazadi.github.io/furnituresimulator/)

### Local Development
```bash
# Clone the repository
git clone https://github.com/JacobKayembekazadi/furnituresimulator.git

# Navigate to the project directory
cd furnituresimulator

# Open index.html in your browser
# No build process required!
```

## 🎮 Controls

| Action | Control |
|--------|---------|
| **Rotate Camera** | Left-click + drag |
| **Zoom In/Out** | Mouse wheel |
| **Pan Camera** | Right-click + drag |
| **Move Furniture** | Click + drag furniture objects |

## 🏗️ Architecture

### Technology Stack
- **Three.js r128** - 3D graphics engine
- **WebGL** - Hardware-accelerated rendering
- **HTML5 Canvas** - Procedural texture generation
- **ES6+ JavaScript** - Modern application logic

### Core Components
- **Scene Manager** - 3D scene initialization and management
- **Rendering Engine** - Post-processing and visual effects
- **Lighting System** - Multi-source lighting simulation
- **Material System** - PBR material creation and management
- **Furniture Factory** - 3D object generation and positioning
- **Interaction Controller** - User input and object manipulation

For detailed architecture information, see [`architectural_document.md`](architectural_document.md).

## 📁 Project Structure

```
furnituresimulator/
├── index.html                    # Main application file
├── architectural_document.md     # Comprehensive architecture guide
└── README.md                    # Project documentation
```

## 🎨 Customization

### Adding New Furniture
```javascript
function createCustomFurniture() {
    const furniture = new THREE.Group();
    // Add your custom geometry and materials
    return furniture;
}

// Add to draggable objects
const customItem = createCustomFurniture();
draggableObjects.push(customItem);
```

### Modifying Materials
```javascript
// Create custom materials
const customMaterial = new THREE.MeshStandardMaterial({
    color: 0xff6b6b,
    roughness: 0.5,
    metalness: 0.3
});
```

### Adjusting Lighting
```javascript
// Modify existing lights
windowLight.intensity = 3.0;
lampLight.color.setHex(0xffa500);

// Add new light sources
const newLight = new THREE.DirectionalLight(0xffffff, 1);
scene.add(newLight);
```

## 🚀 Performance

### Optimizations Included
- **Shadow map optimization** for realistic shadows without performance loss
- **Geometry reuse** for multiple furniture instances
- **Efficient texture management** with procedural generation
- **Smart rendering pipeline** with selective post-processing

### Browser Requirements
- **WebGL support** (modern browsers)
- **Hardware acceleration** recommended
- **Minimum 2GB RAM** for optimal performance

## 🔮 Roadmap

### Near Term
- [ ] **Furniture Library Expansion** - Add chairs, tables, decorations
- [ ] **Room Customization** - Configurable wall colors and textures
- [ ] **Save/Load Functionality** - Persist room configurations
- [ ] **Mobile Optimization** - Enhanced touch controls

### Long Term
- [ ] **VR Support** - Virtual reality integration
- [ ] **Physics Simulation** - Realistic collision detection
- [ ] **Multiplayer Mode** - Collaborative room design
- [ ] **AR Integration** - Augmented reality preview

## 🤝 Contributing

Contributions are welcome! Please read our contributing guidelines:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Three.js Team** - For the incredible 3D graphics library
- **WebGL Community** - For pushing the boundaries of web graphics
- **Open Source Contributors** - For inspiration and code examples

## 📞 Contact

**Jacob Kayembe Kazadi**
- GitHub: [@JacobKayembekazadi](https://github.com/JacobKayembekazadi)
- Project Link: [https://github.com/JacobKayembekazadi/furnituresimulator](https://github.com/JacobKayembekazadi/furnituresimulator)

---

⭐ **Star this repository if you found it helpful!**
