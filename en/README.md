# EasyNSShape — Plugin Documentation

**EasyNSShape**

A Niagara-based library that turns **parametric math formulas into GPU particle shapes**.
Each shape is a single HLSL expression `f(t) -> position`, wrapped as a Niagara *dynamic input*,
then consumed by a reusable emitter and system. The result is a browsable catalogue of
2D curves and 3D curves that render as Niagara point/ribbon effects.



### Shape gallery

All 128 shapes on two 8×8 contact sheets — one frame per shape, name burned into each tile.

[![All 128 EasyNSShape shapes — sheet 1 of 2, tiles 1–64](../AllShapes_Sheet01.png)](../AllShapes_Sheet01.png)

*Sheet 1 — tiles 1–64: 2D curves `ArchimedeanSpiral` → `UnitCircleHyperbolic`.*

[![All 128 EasyNSShape shapes — sheet 2 of 2, tiles 65–128](../AllShapes_Sheet02.png)](../AllShapes_Sheet02.png)

*Sheet 2 — tiles 65–128: `WitchAgnesi` → the 3D set → the 12 multi-parameter shapes → `7P Line3D`.*

Full merged video: [`AllShapes.mp4`](../AllShapes.mp4) — 2:47, 1920×1080, 88 MB.

Watch online: https://youtu.be/sP-1jeiqB6A · https://www.bilibili.com/video/BV1GMhB6gEUE/

### Documentation index

| File | Contents |
|---|---|
| [`README.md`](README.md) | This file — plugin overview, pipeline, workflow, browser, CVars. |
| [`NETemplate_Assets.md`](NETemplate_Assets.md) | Detailed reference for the `NETemplate` template/module assets + the 3 main node graphs. |
| [`TemplateNode/`](../TemplateNode) | Node-graph screenshots of the 3 main nodes (`NE_2DShapeTemplate`, `NE_3DShapeTemplate`, `NMS_Transform`). |
| [`NDI_NE_Shapes.md`](NDI_NE_Shapes.md) | Index + per-shape details for all 128 shapes (picture + video link). |
| [`Thumbs/`](../Thumbs) | 128 shape pictures (`<NS asset>.png`, 240×135, name burned in) embedded in `NDI_NE_Shapes.md` and used to build the contact sheets. |
| [`Videos/`](../Videos) | 128 shape videos (`<NS asset>.mp4`, 1920×1080); requires the FrameCapture plugin to produce new ones. |
| [`AllShapes_Sheet01.png`](../AllShapes_Sheet01.png) · [`AllShapes_Sheet02.png`](../AllShapes_Sheet02.png) | 8×8 contact sheets (1920×1080) covering all 128 shapes. |

---

## 1. Requirements

| Requirement | Notes |
|---|---|
| Unreal Engine 5.8 | Custom build; plugin targets `BuildSettingsVersion.V7`, `IncludeOrderVersion = Unreal5_8`. |
| Niagara plugin | Core dependency — shapes are Niagara emitters/systems. |
| FrameCapture plugin (**optional**) | Only needed for video recording. When absent the module still builds and the browser widget hides its **Record** button. |
| Slate / UMG / UnrealEd | Editor module (`EasyNSShapeEditor`) hosts the shape browser widget. |


---

## 2. Modules

| Module | Type | Loading phase | Purpose |
|---|---|---|---|
| `EasyNSShape` | Runtime | `Default` | Runtime helper code for the shape assets. |
| `EasyNSShapeEditor` | Editor | `PostEngineInit` | Shape browser subsystem + UMG widget, snapshot/record tooling. |

---

## 3. Asset taxonomy

| Prefix | Folder | Unreal type | Role |
|---|---|---|---|
| `NFS` | `Content/NFS/` | Niagara Function Script | Source math function (the formula as a graph). |
| `NDI` | `Content/NDI/` | Niagara Script (Dynamic Input) | Wraps the formula as a `CustomHlsl` node; **one per shape**. |
| `NMS` | `Content/NETemplate/` | Niagara Module Script | Generic driver that samples the NDI and writes the shape vector. |
| `NE` | `Content/NEBridge/NE/` | Niagara Emitter | Emitter that spawns particles along the shape; **one per shape**. |
| `NS` | `Content/NEBridge/NS/` | Niagara System | Ready-to-drop system wrapping one NE; **one per shape**. |
| `NETemplate` | `Content/NETemplate/` | Mixed | The base templates (`NE_2DShapeTemplate`, `NE_3DShapeTemplate`) and driver modules (`NMS_2DShapeVector`, `NMS_ShapeVector`). |


The 12 multi-parameter 3D shapes (`3P` / `4P` / `5P` / `7P`) ship under `Special/` sub-folders:
`Content/NDI/Special/`, `Content/NEBridge/Special/` (both `NE_` and `NS_`), with their `NFS` scripts in
`Content/NFS/` under the `NFS_<nP>_3D_<Shape>` prefix. They expose extra inputs (`InputU`/`InputV`,
`InputA`…`InputC`, `InputR`/`InputRho`/`InputN`, `InputP0`…`InputP3`) beyond `InputT`.


### The `Content/NETemplate/` folder

| Asset | Type | Role |
|---|---|---|
| `NE_2DShapeTemplate` | Niagara Emitter | Base 2D emitter template; the per-shape `NE_2DShapeTemplate_<Shape>` emitters are built from it. |
| `NE_3DShapeTemplate` | Niagara Emitter | Base 3D emitter template; the per-shape `NE_3DShapeTemplate_<Shape>` emitters are built from it. |
| `NE_2DShapeTemplate_System` | Niagara System | System template that hosts a 2D emitter. |
| `NE_3DShapeTemplate_System` | Niagara System | System template that hosts a 3D emitter. |
| `NMS_2DShapeVector` | Niagara Module Script | Reads the 2D dynamic input and writes `OutVector2D`. |
| `NMS_ShapeVector` | Niagara Module Script | Reads the 3D dynamic input and writes the shape vector. |

---

## 4. Data flow

```
        formula (HLSL)                       per-shape
  ┌──────────────────────┐        ┌──────────────────────────┐
  │ NFS2D_<Shape>        │        │ NDI_2D_<Shape>           │
  │ (function script)    │───────▶│ CustomHlsl: f(InputT)    │
  └──────────────────────┘        └────────────┬─────────────┘
                                               │ linked to "FormularXY" / "Formular"
                                               ▼
                              ┌────────────────────────────────┐
                              │ NMS_2DShapeVector              │  (NMS_ShapeVector for 3D)
                              │ driver module script           │
                              └───────────────┬────────────────┘
                                              ▼
                              ┌────────────────────────────────┐
                              │ NE_2DShapeTemplate_<Shape>     │  emitter
                              │  → NS_2D_<Shape>               │  system
                              └────────────────────────────────┘
```

- `NMS_2DShapeVector` exposes a `FormularXY` input; `NMS_ShapeVector` exposes `Formular`.
  The browser subsystem reads the linked dynamic input's `CustomHlsl` text to display the formula.
- 2D shapes are viewed from straight above; 3D shapes from a `(-300,-300,250)`-style offset orbit.

---

## 5. Authoring a new shape

1. Write the formula as HLSL returning a `float2` (2D) or `float3` (3D), using `InputT` as the parameter.
2. Create the **NDI** dynamic input, e.g. `NDI_2D_MyCurve`, and set its `CustomHlsl` to the formula.
3. Create the **NE** emitter `NE_2DShapeTemplate_MyCurve` from `NE_2DShapeTemplate` and link the NDI
   to the `FormularXY` input of `NMS_2DShapeVector`.
4. Create the **NS** system `NS_2D_MyCurve` wrapping that emitter.
5. Drop the system into the demo map so the browser lists it, then regenerate the catalogue with
   `NDI_NE_Shapes.md` (see *Shape reference*).

---

## 6. Shape browser

`UEasyNSShapeBrowserSubsystem` (editor module) parks every `ANiagaraActor` on one spot and shows one
shape at a time, framed by an auto-created focus camera. `UEasyNSShapeBrowserWidget` is the on-screen
HUD: **[Prev] [Snapshot] [AutoRotate] [Record] [Next]** with the current shape name and formula.


| Control | Effect |
|---|---|
| `Prev` / `Next` |Cycle through shapes|
| `Snapshot` |Manual capture → `NSShapeDocs/ScreenShots/<Name>_fix.png`|
| `AutoRotate` |Toggle `EasyNSShape.CameraOrbit`|
| `Record` |Start/stop a video via FrameCapture — **only shown when FrameCapture is installed**|

### Console variables & commands

| Name | Default | Purpose |
|---|---|---|
| `EasyNSShape.AutoCapture` | `0` | Walk every shape and write one snapshot each. |
| `EasyNSShape.AutoRecord` | `0` | With AutoCapture: record video instead of snapshots. |
| `EasyNSShape.CameraOrbit` | `0` | Auto-orbit the focus camera. |
| `EasyNSShape.CameraOrbitSpeed` | `20` | Orbit speed, degrees/second. |
| `EasyNSShape.CameraMouseSensitivity` | `0.25` | Degrees of orbit per pixel of drag. |
| `EasyNSShape.Snapshot` | — | Console command: manual snapshot of the current shape. |

`EasyNSShape.AutoCapture` must be set **before** entering PIE: the subsystem arms the pass in
`OnWorldBeginPlay`, so a command issued during PIE does not start a new pass.

---

## 7. Optional recording

Video recording is implemented through the third-party **FrameCapture** plugin. The dependency is
optional at build time:

- `EasyNSShapeEditor.Build.cs` looks for `Plugins/FrameCapture/FrameCapture.uplugin` (project or engine)
  and checks the `.uproject` for `"Enabled": false`; only then does it define
  `EASYNS_WITH_FRAMECAPTURE=1` and link the module.
- When the plugin is missing or disabled, `StartVideoRecording()` returns `false` with a warning, and
  `UEasyNSShapeBrowserWidget` never creates the **Record** button.


---

## 8. Output

| Path | Contents |
|---|---|
| `NSShapeDocs/ScreenShots/` | One PNG per shape (auto pass) + `<Name>_fix.png` (manual). |
| `NSShapeDocs/ScreenShots/Thumbnails/` | 256×256 thumbnails (center-cropped). |
| `NSShapeDocs/Videos/` | Recorded `.mp4` files (requires FrameCapture). |
| `NSShapeDocs/Thumbs/` | 240×135 picture per video, one per shape (offline ffmpeg pass). |
| `NSShapeDocs/AllShapes_Sheet01.png` · `02` | 8×8 contact sheets (1920×1080) built from `Thumbs/`. |
