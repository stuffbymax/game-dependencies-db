
## 🎮 Game Compatibility Helper JSON

This JSON file serves as a **reference database for fixing compatibility issues** with popular PC games, particularly when launching older titles or resolving common runtime errors. It provides detailed metadata for each game including:

### ✅ What It Contains

For each game entry, the JSON includes:

* **Game Name & Cover Image**
  A visual and name identifier for easy reference.

* **DirectX Version**
  Indicates the required DirectX version (e.g., DirectX 9.0c, 11, or 12).

* **Visual C++ Redistributables (`vcredist`)**
  Lists the Microsoft VC++ versions needed (e.g., 2008, 2015–2022).

* **.NET Framework Version**
  Specifies the required version of the .NET Framework.

* **Critical DLL Dependencies**
  Lists DLL files that the game depends on. These are often the root cause of startup errors.

* **Download Links**
  Direct links to official Microsoft sources for:

  * DirectX Redistributables
  * VC++ Redistributables
  * .NET Framework installers

* **Common Fixes**
  A list of practical steps users can follow to resolve compatibility problems (e.g., running as administrator, using compatibility mode, disabling overlays).

### 🧩 Purpose

This JSON is designed to help users:

* Troubleshoot and fix game launch issues.
* Quickly locate and download missing dependencies.
* Set up classic or modern games on Windows systems more reliably.

### 🛠 Example Use Cases

* Building a **troubleshooting assistant tool** or launcher that reads this JSON and checks for missing components.
* Creating a **mod manager or patch installer** that ensures all runtimes are correctly installed.
* Publishing a **website or app** that recommends fixes based on known game compatibility problems.

---
