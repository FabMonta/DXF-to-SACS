# DXF-to-SACS

A Python utility to convert 3D DXF line-geometry into SACS (Bentley) structural input files, featuring an interactive node-distance quality check and layer-to-group mapping.

# Why?

SACS does not natively allow you to import standard CAD files directly. Traditionally, engineers must manually recreate structural geometry node-by-node inside the software or rely on the built-in parametric wizard, which is heavily limited to standard offshore jacket structures and lacks the flexibility needed for custom or complex topside framing. 

This Python utility eliminates this limitation. It automatically reads an AutoCAD DXF file and generates a compatible SACS input text file (`sacinp.*`), pre-populating all **JOINT** and **MEMBER** cards, saving hours of manual modeling.

---

## 🚀 Features & Version History

### 🔹 V1.2.0 (Current Version)
* **Layer Characterization & Group Mapping:** Automatically reads AutoCAD layer names and maps them directly to SACS **Group** IDs, letting you define structural group properties based on your CAD layering system.

### 🔹 V1.1.3
* **Interactive Proximity Self-Check:** Prompts you to set a geometric distance threshold (e.g., 10mm) and alerts you if nodes are dangerously close to each other, catching potential CAD snapping errors before you run your structural analysis.

### 🔹 V1.1.2
* **Datagen Optimization:** Automatically formats and spaces the input text file for a clean, organized, and perfectly aligned view inside the SACS **Datagen** module.
* **Default Self-Weight:** Automatically appends a standard `DEAD` load block with a gravity multiplier coefficient of `1.0000` acting in the `-Z` direction, eliminating the need to set up basic self-weight cards manually.

### 🔹 Core Capabilities
* **Automatic Scaling:** mm (CAD) → m (SACS).
* **Smart Node Merging:** overlapping endpoints are automatically merged into a single joint.
* **Strict Column Alignment:** fixed-width spacing for `JOINT`, `MEMBER`, and `LOAD` cards, ensuring zero syntax errors upon import.
* **Sequential Node Naming (`J001`, `J002`, ...):** 3-digit padding, cleanly isolating script-generated nodes from any joints you might add manually inside SACS later.

---

## 📐 CAD Drawing Guidelines

To ensure a perfect conversion, please prepare your DXF file following these simple rules:

1. **Use LINE Entities Only:** The script reads standard `LINE` elements. Ensure your wireframe structural model doesn't use polylines, arcs, or 3D solids.
2. **Draw in Millimeters:** The script assumes your CAD workspace is set up in millimeters (1 unit = 1 mm).
3. **Use Object Snap:** Always use `Endpoint` snap when joining lines. This prevents the generation of unwanted duplicate nodes.
4. **DXF Format:** Export or save your AutoCAD file as a `.dxf` file using the **Save As** (`SAVEAS`) command. Any recent DXF version (e.g., AutoCAD 2018 DXF or newer) works perfectly.

---

## 🔒 Security & Trust (EXE vs Python)

Because company PCs often have strict IT policies regarding unknown executable files, you have two ways to run this tool:

### Option A: The Ready-to-Use Executable (Fastest)

1. **Go to Releases:** Navigate to the **Releases** section on the right side of this repository page.
2. **Download the ZIP:** Download the compressed archive **`v1.2.0.zip`**. It contains the executable (`DXF_to_SACS.exe`) and sample CAD files ready for immediate testing.
3. **Extract the Archive:** Unzip the folder anywhere on your computer. 
   * *To run the test:* the `.exe` and the sample DXF files are already placed together.
   * *To run your own model:* copy your custom `.dxf` file into this **same folder** alongside the `.exe`.
4. **Run the Tool:** Double-click the `.exe` file to run the converter.
   * ⚠️ *Note: Windows SmartScreen might flag it as an "unknown publisher" because it lacks a paid digital certificate. You can safely bypass this by clicking **"More info"** and then **"Run anyway"**.*
5. **Output Generation:** Follow the quick on-screen prompts. A fully compatible `sacinp.*` text file will be automatically created in the exact same folder.

### Option B: Run the Python Script (Fully Transparent)

If you prefer not to run pre-compiled binaries on your machine, you can inspect and run the raw Python code directly:

1. Download `DXF_to_SACS.py`.
2. Install the required library via terminal: `pip install ezdxf`
3. Run the script: `python DXF_to_SACS.py`
4. **Build Your Own EXE:** If you prefer the convenience of an executable but want 100% security transparency, you can easily compile the raw script yourself using the `pyinstaller` library:
   ```bash
   pip install pyinstaller
   python -m PyInstaller --onefile --icon=icon.ico DXF_to_SACS.py
   ```

---

## 🧪 Sample Files & Testing

Inside the `example/` folder you'll find a set of ready-to-use DXF files so you can test the script immediately without needing your own CAD model:

* **`sample_frame.dxf`** — a clean structural wireframe with multiple distinct AutoCAD layers, useful for testing the **Layer Characterization & Group Mapping** feature (V1.2.0). Each layer maps to a different SACS Group ID, so you can see the mapping logic in action.
* **A second sample file with an intentional geometry error** — two nodes placed abnormally close together (but not perfectly overlapping/snapped). Use this file to test the **Interactive Proximity Self-Check** (V1.1.3): run the script, set a distance threshold, and confirm the tool correctly flags the suspicious node pair before you proceed to SACS.

💡 **Tip:** Running both sample files is the fastest way to confirm your installation (EXE or Python) is working correctly before feeding in your own project geometry.

---

## ⚠️ Current Limitations
* Supports only `LINE` geometry (Structural wireframes).
* SACS section properties and material groups must still be assigned manually inside SACS after importing the geometry.
* **999-Node Maximum:** due to the `J001`–`J999` sequential nomenclature (3-digit padding), the script currently supports a maximum of 999 unique nodes per DXF file to strictly preserve the SACS card spacing format.

---

## 🤝 Contributing & Feedback

Found a bug, have a feature request, or want to contribute improvements? Feel free to open an **Issue** or submit a **Pull Request** on this repository — feedback from real-world SACS/DXF workflows is always welcome.

---

*Developed by FM – 2026*
