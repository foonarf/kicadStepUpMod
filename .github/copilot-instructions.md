# KiCad StepUp Workbench
KiCad StepUp is a FreeCAD workbench that enables bidirectional ECAD/MCAD collaboration between KiCad EDA and FreeCAD. It provides tools to import KiCad PCBs and footprints into FreeCAD, export to STEP/IGES formats, and synchronize 3D models between both applications.

Always reference these instructions first and fallback to search or bash commands only when you encounter unexpected information that does not match the info here.

## Working Effectively

### Prerequisites and Installation
- Install FreeCAD 0.19, 0.20, 0.21, 0.22, or 1.0 from [FreeCAD Downloads](http://www.freecadweb.org/wiki/Download)
- Install KiCad 5.1, 6.x, 7.x, 8.x, or 9.x
- NEVER try to "build" this project - it is a Python-based FreeCAD workbench, not a traditional compiled application
- Install the workbench using one of these methods:
  - **Recommended**: Use FreeCAD Add-on Manager (Tools → Addon Manager → search "KiCad StepUp")
  - **Manual**: Copy the `kicadStepUpMod` folder to FreeCAD's Mod directory
  - **Known Issue**: On Linux, FreeCAD Add-on Manager may fail for FC >= 1.0.1

### Platform-Specific Notes
- **Linux Snap/Flatpak**: May need 'mount bind' to access KiCad 3D models path
- **Windows**: Use provided .bat scripts in demo folder
- **macOS**: Use provided OSX .sh scripts in demo folder

### Validation and Testing
Since this is a FreeCAD workbench, validation requires the FreeCAD environment:

**NEVER attempt validation without FreeCAD installed** - the code cannot run standalone.

**Installation Verification**:
1. Launch FreeCAD
2. Check if "KiCadStepUp" appears in the workbench list
3. Switch to KiCadStepUp workbench
4. Verify toolbar icons load correctly

**Functional Validation using Demo Files**:
- `demo/demo.kicad_pcb` - Main test PCB file
- `demo/d-pak.kicad_mod` - Sample footprint file
- `demo/demo.FCStd` - FreeCAD test file
- `demo/demo.step` - STEP export sample

**Key Validation Scenarios**:
1. **Import KiCad PCB**: Load `demo/demo.kicad_pcb` - should create 3D PCB representation with components
2. **Import Footprint**: Load `demo/d-pak.kicad_mod` - should create footprint in FreeCAD with pads
3. **Export to STEP**: Export loaded PCB to STEP format - creates .step file for MCAD use
4. **3D Model Loading**: Verify component 3D models load (requires KISYS3DMOD path configuration)
5. **Push/Pull Operations**: Test bidirectional synchronization between FreeCAD and KiCad
6. **Sketcher Integration**: Load board edge sketches and modify PCB outlines
7. **Collision Detection**: Check component clearances and mechanical interference

**Manual Validation Steps**:
1. **Launch FreeCAD**: `freecad` or use desktop launcher
2. **Switch Workbench**: Select "KiCadStepUp" from workbench dropdown
3. **Load Demo PCB**: File → Import → select `demo/demo.kicad_pcb`
   - Should see 3D PCB with green soldermask
   - Components should appear in proper 3D positions
   - Takes 30-60 seconds for complex boards - **NEVER CANCEL**
4. **Verify Tools**: Check toolbar icons are present and clickable
5. **Test Export**: Select PCB → File → Export → STEP format
   - Should generate .step file
   - Process takes 1-2 minutes - **NEVER CANCEL**

**Timing Expectations**:
- Initial workbench loading: 5-10 seconds
- Large PCB import (>1000 components): 2-5 minutes - **NEVER CANCEL**
- STEP export operations: 1-3 minutes - **NEVER CANCEL**
- 3D model loading: varies by model complexity - **NEVER CANCEL**

## Development and Debugging

### Code Structure
- `Init.py` & `InitGui.py`: Workbench initialization and GUI setup
- `kicadStepUptools.py`: Core functionality (21,000+ lines)
- `kicadStepUpCMD.py`: FreeCAD command definitions
- `package.xml`: FreeCAD package metadata
- `Resources/`: Icons, UI files, textures
- `translations/`: Multi-language support files

### Python Code Validation
- Run `python3 -m py_compile Init.py InitGui.py kicadStepUptools.py kicadStepUpCMD.py` to check syntax
- Expect syntax warnings (regex escape sequences) but no compilation errors
- All Python files should compile successfully - warnings are normal and non-critical
- Do not attempt to run Python files directly - they require FreeCAD environment

### Translation Updates
- Change to translations directory: `cd translations/`
- Run `./update_translation.sh` to see usage help
- For updates: `./update_translation.sh -u <locale>` (e.g., `-u es-ES`)
- For releases: `./update_translation.sh -r <locale>`
- Requires Qt tools: `sudo apt-get install qttools5-dev-tools` on Debian/Ubuntu
- Script validates locale codes automatically - case sensitive

### Common Development Tasks
- **Code Changes**: Modify Python files directly - no build step required
- **Testing Changes**: Restart FreeCAD to reload workbench code (changes don't hot-reload)
- **Adding Features**: Follow FreeCAD workbench patterns in `kicadStepUpCMD.py`
- **UI Changes**: Modify files in `Resources/ui/` directory
- **Debugging**: Use FreeCAD Python console for testing code snippets
- **Log Files**: Check FreeCAD Report View for error messages and warnings

### File Import/Export Operations
- **Supported Formats**: .kicad_pcb, .kicad_mod, .step, .iges, .vrml
- **3D Model Paths**: Configure KISYS3DMOD environment variable for KiCad models
- **Large File Handling**: Files >50MB may take 5+ minutes to process - **NEVER CANCEL**
- **Memory Requirements**: Complex PCBs (>2000 components) need 4GB+ RAM

### Troubleshooting Common Issues
- **Workbench Not Loading**: Check Python Console in FreeCAD for import errors
- **Missing 3D Models**: Verify KiCad 3D model library paths in preferences  
- **Slow Performance**: Large PCBs with many components are CPU-intensive
- **Import Failures**: Ensure KiCad files are compatible versions (5.1+ supported)
- **Path Issues**: Use forward slashes (/) in all paths, even on Windows

## Limitations and Known Issues
- **No Headless Operation**: Cannot run without FreeCAD GUI
- **No Traditional Testing**: Functional testing requires manual FreeCAD interaction
- **Platform Dependencies**: Relies on FreeCAD and KiCad installations
- **3D Model Paths**: May require manual configuration for KiCad 3D model libraries
- **Large Files**: PCBs with many components can take several minutes to process

## Key Workflows
1. **ECAD to MCAD**: Import KiCad PCB → modify in FreeCAD → export to STEP
2. **Footprint Creation**: Design in FreeCAD → export as KiCad footprint
3. **3D Model Creation**: Create 3D models → convert to VRML for KiCad
4. **Board Edge Design**: Sketch PCB outline in FreeCAD → push to KiCad
5. **Collision Detection**: Check component interference in 3D space

## Resources
- **Cheat Sheet**: `demo/kicadStepUp-cheat-sheet.pdf`
- **Starter Guide**: `demo/kicadStepUp-starter-Guide.pdf`  
- **Video Tutorials**: Links in README.md
- **Forums**: [KiCad Forum](https://forum.kicad.info/search?q=stepup) | [FreeCAD Forum](https://forum.freecadweb.org/viewtopic.php?f=24&t=14276)

## Development Workflow for Coding Agents

### Making Changes
- **Always validate syntax**: Run `python3 -m py_compile` on modified .py files before committing
- **Test in FreeCAD**: Changes require FreeCAD restart to take effect
- **Check translations**: If modifying user-facing strings, update translation files
- **Update version**: Increment version in `package.xml` for significant changes

### Pre-commit Validation
1. **Syntax Check**: `python3 -m py_compile Init.py InitGui.py kicadStepUptools.py kicadStepUpCMD.py`
2. **File Structure**: Ensure no accidental deletion of core files
3. **Demo Files**: Keep demo directory intact for user testing
4. **Documentation**: Update this file if changing workflow or adding features

### Important Files - Handle with Care
- `Init.py` & `InitGui.py`: Workbench initialization - test thoroughly
- `kicadStepUptools.py`: 21k+ lines of core functionality - make minimal changes
- `package.xml`: FreeCAD package metadata - version changes affect updates
- `demo/`: User testing files - do not modify without good reason

## Common Repository Tasks

### Repository Structure
```
kicadStepUpMod/
├── Init.py & InitGui.py          # Workbench entry points
├── kicadStepUptools.py           # Main functionality
├── kicadStepUpCMD.py             # Command definitions  
├── package.xml                   # FreeCAD package info
├── demo/                         # Example files and launchers
├── Resources/                    # Icons, UI, textures
├── translations/                 # Language files
└── README.md                     # Documentation
```

### File Listings
**Repository root** (`ls -la`):
```
Init.py InitGui.py README.md Resources/ SaveSettings.py 
TBD.txt TranslateUtils.py ZipStepImport.py _DXF_Import.py
changelog.txt commits_num.py constrainator.py demo/
dxf_parser/ exchangePositions.py expTree.py explode.py
fcad_parser/ fps.py hlp.py images/ kicadStepUpCMD.py
kicadStepUptools.py kicad_parser.py ksu_locator.py
makefacedxf.py package.xml readme.html selection2edges.py
step_amend.py test-mb.py tracks.py translations/
utf8test-TBD.txt utf8test.py
```

**Demo directory** (`ls demo/`):
```
Key files:
- demo.kicad_pcb, empty-kv5.kicad_pcb, empty-kv6.kicad_pcb (PCB files)
- d-pak.kicad_mod (footprint file)
- demo.FCStd, demo-sketch.FCStd (FreeCAD files)
- demo.step, demo-fc16.step (STEP export examples)
- launch-kicad_StepUp-Tools.sh/.bat (platform launchers)
- *.pdf files (documentation and guides)
- gifs/ (animated demonstration files)
```