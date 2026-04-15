# MechJourVIS - Mechanical Engineering CAD Assistant

**AI-Powered Voice-First CAD Assistant for Windows 11**

An intelligent, voice-controlled mechanical engineering assistant that integrates with major CAD platforms, provides real-time design analysis, and intelligently sorts components and assemblies.

![Status](https://img.shields.io/badge/Status-Beta-orange)
![Platform](https://img.shields.io/badge/Platform-Windows%2011-blue)
![Python](https://img.shields.io/badge/Python-3.11%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)

## Features

### 🎤 **Voice Control**
- **Natural language commands** for design operations
- Real-time speech recognition with AI understanding
- Voice feedback with engineering-specific terminology
- Hands-free operation while designing

### 🔍 **Real-Time Design Analysis**
- **FEA Quick Analysis** - Stress, strain, deflection preview
- **Tolerance Stack-Up** - Automatic dimensional analysis
- **Material Database** - Properties lookup and comparison
- **Design Rule Checking** - Manufacturability validation
- **Weight & CoG Calculation** - Instant mass properties

### 📦 **Intelligent Part Sorting**
- **Automatic categorization** - By type, material, size
- **BOM Generation** - Bill of Materials with specifications
- **Assembly hierarchy** - Visual part relationships
- **Part library search** - Find similar components
- **Standardization checking** - Identify redundant parts

### 🔗 **CAD Integration**
- ✅ **SolidWorks** - Full API integration
- ✅ **FreeCAD** - Python scripting support
- ✅ **Fusion 360** - Cloud API integration
- ✅ **AutoCAD** - DWG/DXF support
- 📋 **STEP/IGES/STL** - Universal format support

### 💾 **Project Management**
- Design version control
- Revision history tracking
- Collaborative annotations
- Design documentation auto-generation

### 📊 **Advanced Analytics**
- Design performance metrics
- Cost estimation
- Manufacturing time prediction
- Supply chain integration

## Requirements

### System
- **Windows 11** (21H2+)
- **8GB RAM minimum** (16GB recommended)
- **100GB free disk space** (for CAD software)
- **GPU support** (NVIDIA/AMD for 3D rendering)

### Software Dependencies
- Python 3.11+
- Node.js 18+
- Google Chrome (Web Speech API)
- One or more CAD platforms:
  - SolidWorks 2022+
  - FreeCAD 0.21+
  - Fusion 360
  - AutoCAD 2023+

### API Keys Required
- **Anthropic Claude API** - AI brain (https://console.anthropic.com/)
- **Fish Audio API** - Text-to-speech (https://fish.audio/)
- **CAD vendor APIs** - For integration (SolidWorks, Fusion 360, etc.)

## Quick Start

### 1. Clone & Setup
```bash
git clone https://github.com/yourusername/mechjourvis.git
cd mechjourvis
cp .env.example .env
```

### 2. Configure API Keys
Edit `.env`:
```env
ANTHROPIC_API_KEY=your-key-here
FISH_API_KEY=your-key-here
SOLIDWORKS_PATH=C:\Program Files\SolidWorks
FREECAD_PATH=C:\Program Files\FreeCAD
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
cd frontend && npm install && cd ..
```

### 4. Generate SSL Certificates
```bash
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365 -nodes -subj "/CN=localhost"
```

### 5. Start Services
```bash
# Terminal 1: Backend
python server.py

# Terminal 2: Frontend
cd frontend && npm run dev
```

### 6. Open in Chrome
```
https://localhost:5173
```

Click to enable audio → Speak naturally!

## Voice Commands Examples

### Design Analysis
- *"Analyze stress on this part"*
- *"Calculate the center of gravity"*
- *"Check tolerance stack-up for this assembly"*
- *"Is this design manufacturable?"*
- *"Estimate production cost"*

### Part Management
- *"Sort all parts by material"*
- *"Generate a bill of materials"*
- *"Find similar parts in our library"*
- *"Show assembly hierarchy"*
- *"Which parts are redundant?"*

### CAD Operations
- *"Open the latest assembly"*
- *"Create a new part"*
- *"Export as STEP file"*
- *"Compare this to the previous version"*

### Project Management
- *"Start a new design project"*
- *"Save current work"*
- *"Show design history"*
- *"Generate design documentation"*

## Architecture

```
Microphone → Web Speech API → WebSocket → FastAPI Backend
                                            ↓
                                    Claude AI Engine
                                    ↓
                        CAD Integration Layer
                        ├─ SolidWorks API
                        ├─ FreeCAD Python
                        ├─ Fusion 360 API
                        └─ AutoCAD COM
                        ↓
                    Analysis Engines
                    ├─ FEA Quick Solver
                    ├─ Tolerance Analysis
                    ├─ Design Validator
                    └─ Cost Estimator
                        ↓
                    Fish Audio TTS → Speaker
```

## Key Components

| Module | Purpose |
|--------|---------|
| `server.py` | Main FastAPI server & WebSocket handler |
| `cad_handler.py` | CAD file I/O and format conversion |
| `analysis_engine.py` | Real-time analysis computations |
| `part_sorter.py` | Intelligent component classification |
| `design_validator.py` | Manufacturability & design rules |
| `mechanical_utils.py` | Engineering calculations & formulas |
| `integrations/` | CAD platform-specific code |
| `frontend/` | React/Vite UI with Three.js 3D viewer |

## Workflow

1. **Open CAD file** via UI or voice command
2. **MechJourVIS loads** the model and metadata
3. **Real-time analysis** runs automatically:
   - Part categorization
   - Material validation
   - Dimensional checking
4. **Voice interface** awaits commands
5. **User speaks** - "Analyze stress on this part"
6. **AI processes** intent → triggers appropriate analysis
7. **Results displayed** visually + voice summary
8. **Export** analysis, BOM, or documentation

## Configuration

### `config.yaml`
```yaml
cad_platforms:
  solidworks:
    enabled: true
    path: "C:\Program Files\SolidWorks"
  freecad:
    enabled: true
    path: "C:\Program Files\FreeCAD"
  fusion360:
    enabled: false
  autocad:
    enabled: false

analysis:
  fea_enabled: true
  fea_solver: "quick"  # or "detailed"
  tolerance_analysis: true
  cost_estimation: true

voice:
  language: "en-US"
  provider: "fish_audio"
  voice_id: "mechanical_engineer"
```

## API Reference

### WebSocket Messages

**Request: Analyze Part**
```json
{
  "action": "analyze",
  "type": "stress",
  "part_id": "PART_001",
  "material": "Aluminum 6061",
  "load": 500
}
```

**Response: Analysis Results**
```json
{
  "status": "complete",
  "analysis_type": "stress",
  "max_stress": 45.2,
  "safety_factor": 2.8,
  "recommendation": "Design is safe"
}
```

See [API_REFERENCE.md](docs/API_REFERENCE.md) for complete documentation.

## Examples

### Example 1: Analyze an Assembly
```bash
# Voice: "Analyze this assembly for stress"
# System: Loads assembly → runs FEA → returns results
```

### Example 2: Generate BOM
```bash
# Voice: "Create a bill of materials"
# System: Extracts parts → categorizes → generates BOM.xlsx
```

### Example 3: Find Design Issues
```bash
# Voice: "Check for manufacturability issues"
# System: Validates against design rules → highlights problems
```

## Development

### Run Tests
```bash
pytest tests/
```

### Build Frontend
```bash
cd frontend
npm run build
```

### Deploy
```bash
# Production build
python -m build
pip install dist/mechjourvis-*.whl
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| CAD software not detected | Check `config.yaml` paths |
| FEA analysis slow | Enable GPU acceleration in settings |
| Voice not recognized | Check microphone permissions |
| WebSocket fails | Regenerate SSL certificates |

See [SETUP_WINDOWS.md](SETUP_WINDOWS.md) for detailed troubleshooting.

## Contributing

We welcome contributions! Areas of interest:

- 🔧 Additional CAD platform integrations
- 📊 Advanced FEA solvers
- 🤖 Machine learning for design optimization
- 🌐 Web client improvements
- 📚 Documentation enhancements

Please open an issue before submitting large PRs.

## Roadmap

- [ ] **v1.0** - Core CAD integration + basic analysis
- [ ] **v1.5** - Advanced FEA solver integration
- [ ] **v2.0** - Machine learning design optimization
- [ ] **v2.5** - Supply chain integration
- [ ] **v3.0** - Collaborative real-time editing

## License

MIT License - See [LICENSE](LICENSE) for details

## Credits

**Built by:** Mechanical Engineers, for Mechanical Engineers

**Powered by:** 
- [Anthropic Claude](https://anthropic.com) - AI Engine
- [Fish Audio](https://fish.audio) - Voice synthesis
- [Three.js](https://threejs.org) - 3D visualization
- CAD Vendor APIs

## Support

- 📧 Email: support@mechjourvis.dev
- 💬 Discord: [Join Community](https://discord.gg/mechjourvis)
- 🐛 Issues: [GitHub Issues](https://github.com/yourusername/mechjourvis/issues)

---

**Ready to revolutionize your mechanical design workflow? Start MechJourVIS today!** 🚀