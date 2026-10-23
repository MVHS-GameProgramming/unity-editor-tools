# MVHS Unity Editor Tools

A collection of Unity Editor tools to support group projects and collaborative game development workflows. These tools handle project setup, asset validation, and asset distribution for teams using Git.

All tools live under `Assets/Editor/MVHS/` and share a common configuration through `AssetPipelineConfig.cs`. They are accessed from the Unity menu at **Tools > MVHS**.

## Installation

1. Download the latest `.unitypackage` from the shared class Google Drive folder
2. Open your Unity project
3. Go to **Assets > Import Package > Custom Package** and select the downloaded `.unitypackage`
4. Import all files — the tools will be placed in `Assets/Editor/MVHS/` automatically
5. The tools are now available under **Tools > MVHS**

---

## Tools

### Pre-PR Checklist

**Menu:** Tools > MVHS > Pre-PR Checklist

The main workflow tool — complete all four steps before opening a Pull Request. Steps must be done in order; each one unlocks the next.

1. **Run Project Scan** — verifies all assets in `Assets/Project/` are in the correct folder. Fix any violations in Asset Structure Validator before continuing.
2. **Generate Manifest** — scans for binary assets and holds the manifest in memory. Nothing is written to disk yet.
3. **Export Package** — exports all binary assets as a `.unitypackage` file. Save it somewhere you can find it.
4. **Upload to Shared Drive** — upload the `.unitypackage` to the shared class Google Drive folder, then paste the share link into the checklist and click **Save URL**. This writes the manifest and records the Drive URL so teammates can find the package.

After all four steps are complete, commit the manifest and `package_urls.json`, then open your Pull Request.

---

### Asset Pipeline Manager

**Menu:** Tools > MVHS > Asset Pipeline Manager

Shows the status of all asset folders and helps teammates import missing binary assets.

**Folder Status** displays each project folder with one of three states:
- `●` Has assets on disk — good to go
- `⬇` Manifest says files should be here but they are missing — needs import
- `○` No assets yet — nobody has added anything here

**Import** lists each manifest that has missing files, newest first. For each one it shows who exported it, how many files are missing, and a **Open Drive** button that opens the Google Drive link directly. Download the `.unitypackage` from Drive and import it using **Import Package from File…**, or drag it into the Project window.

Click **Refresh** after importing to update the folder status.

**Startup check:** When Unity opens the project, `AssetPipelineStartupCheck` automatically reads all manifests. If any folders are missing assets that a manifest expects, a warning dialog appears before any work is saved. Folders that are empty because no one has added assets there yet are ignored — only folders where a manifest expects files are flagged.

---

### Asset Structure Validator

**Menu:** Tools > MVHS > Asset Structure Validator

Scans scene and GameObject dependencies to verify that assets are in the correct project folders.

**Tabs:**

- **Scene Dependency Viewer** — scans all loaded scenes and checks every referenced asset against the validation rules
- **GameObject Dependency Viewer** — scans a single selected GameObject and its dependencies
- **Rules** — displays the rules for where each asset type must be placed

Assets that fail validation can be moved to the correct folder directly from the tool. **Do not move assets from outside Unity** — always use Unity or this tool to move assets so `.meta` files stay in sync.

---

### Project Directory Creator

**Menu:** Tools > MVHS > Utilities > Project Directory Creator

Creates the standard project folder structure used for all Unity projects in the game programming course. Only missing folders are created — running it on an existing project is safe. This should be the first tool run when setting up a new project.

---

## Workflow

### Project lead (initial setup)

1. Create a new Unity project
2. Import the MVHS tools `.unitypackage` from the shared class Google Drive folder
3. Run **Tools > MVHS > Utilities > Project Directory Creator**
4. Import assets into the appropriate project folders
5. Run **Asset Structure Validator** to verify placement
6. Run **Pre-PR Checklist** to export the package and record the Drive URL
7. Commit and push to Git (the repo contains scripts, scenes, prefabs, and manifests — not binary assets)

### Teammates (after cloning)

1. Clone the repository — the MVHS tools are already in the repo under `Assets/Editor/MVHS/`
2. Open Unity — the startup check warns if binary assets are missing
3. Open **Tools > MVHS > Asset Pipeline Manager**
4. Click **Open Drive** next to the package that has missing files
5. Download the `.unitypackage` from Google Drive and import it using **Import Package from File…**
6. All asset references reconnect automatically

### When new assets are added (before a Pull Request)

1. Add assets to the correct project folders
2. Run **Asset Structure Validator** to verify placement
3. Open **Tools > MVHS > Pre-PR Checklist** and complete all four steps
4. Commit the manifest and `package_urls.json`
5. Open your Pull Request
