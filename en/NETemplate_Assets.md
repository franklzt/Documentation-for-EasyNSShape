# EasyNSShape — `NETemplate` Asset Reference

**NETemplate**

The `Content/NETemplate/` folder holds the **base templates and driver modules** that every shape is
built from. It is not a shape itself — it is the scaffolding: two emitter templates, two system
templates, and two module scripts that read the NDI formula and write the shape vector. A third
driver module, `NMS_Transform` (in `Content/Common/`), turns that vector into the final particle
position.

| Asset | Type | Role |
|---|---|---|
| `NE_2DShapeTemplate` | Niagara Emitter | Base 2D emitter template. |
| `NE_3DShapeTemplate` | Niagara Emitter | Base 3D emitter template. |
| `NE_2DShapeTemplate_System` | Niagara System | System hosting the 2D emitter template. |
| `NE_3DShapeTemplate_System` | Niagara System | System hosting the 3D emitter template. |
| `NMS_2DShapeVector` | Niagara Module Script | Reads the 2D dynamic input, writes `float2`. |
| `NMS_ShapeVector` | Niagara Module Script | Reads the 3D dynamic input, writes `float3`. |
| `NMS_Transform` | Niagara Module Script | Turns the shape vector into the final particle position (scale / rotate / offset). Lives in `Content/Common/`. |

> The per-shape emitters (`NE_2DShapeTemplate_<Shape>` / `NE_3DShapeTemplate_<Shape>`) live in
> `Content/NEBridge/NE/` and are **built from** these templates. The per-shape NDI scripts live in
> `Content/NDI/`. See [`NDI_NE_Shapes.md`](NDI_NE_Shapes.md) for the full catalogue.

---

## `NMS_2DShapeVector`

| Field | Value |
|---|---|
| Path | `/EasyNSShape/NETemplate/NMS_2DShapeVector` |
| Type | Niagara Module Script (function script) |
| Valid stages | `particle_spawn`, `particle_update` |
| Category | `Default` |
| Module usage bitmask | `250` |

| Input | Type | Notes |
|---|---|---|
| `Module.FormularXY` | Dynamic Input (Vector2D) | The per-shape 2D formula — the linked `NDI_2D_<Shape>` script. |
| `Module.ZValue` | float | Optional Z offset applied to the 2D result. |

**Behaviour**: samples the linked dynamic input at `InputT` and writes the resulting `float2`
(plus `ZValue`) as the particle position.

---

## `NMS_ShapeVector`

| Field | Value |
|---|---|
| Path | `/EasyNSShape/NETemplate/NMS_ShapeVector` |
| Type | Niagara Module Script (function script) |
| Valid stages | `particle_spawn`, `particle_update` |
| Category | `Default` |
| Module usage bitmask | `250` |

| Input | Type | Notes |
|---|---|---|
| `Module.Formular` | Dynamic Input (Vector3D) | The per-shape 3D formula — the linked `NDI_3D_<Shape>` script. |

**Behaviour**: samples the linked dynamic input at `InputT` and writes the resulting `float3`
as the particle position.

> The browser subsystem finds the formula by looking for a function-call node named
> `NMS_2DShapeVector` (input pin ending in `FormularXY`) or `NMS_ShapeVector` (pin ending in
> `Formular`), then reading the linked dynamic input's `CustomHlsl` text.

---

## `NMS_Transform`

| Field | Value |
|---|---|
| Path | `/EasyNSShape/Common/NMS_Transform` |
| Type | Niagara Module Script (function script) |
| Valid stages | `particle_spawn`, `particle_update` |
| Category | `Default` |
| Library visibility | `Library` |

The third driver module. `NMS_2DShapeVector` / `NMS_ShapeVector` produce a **shape point**;
`NMS_Transform` converts that point into the **final particle position** by applying scale, rotation
and offset, then writes it to the parameter map. Both emitter templates call it.

| Input | Type | Notes |
|---|---|---|
| `Module.ShapeVector` | Vector | The shape point from `NMS_2DShapeVector` / `NMS_ShapeVector`. |
| `Module.ShapeTangent` | Vector | Tangent at the shape point (ribbon / tangent alignment). |
| `Module.ShapeNormal` | Vector | Normal at the shape point. |
| `Module.Offset` | Vector | Translation applied to the shape. |
| `Module.Offset Coordinate Space` | `ENiagara_OffsetMode` | Space the offset is applied in. |
| `Module.Non Uniform Scale` | Vector | Per-axis scale. |
| `Module.Apply Owner Scale` | bool | Factor the owner's scale into the result. |
| `Module.Rotation Matrix` | Matrix | Rotation used when the rotation mode is Matrix. |
| `Module.Rotation Quaternion` | Quaternion | Rotation used when the rotation mode is Quaternion. |
| `Module.Invert Rotation Quaternion` | bool | Invert the incoming quaternion. |
| `Module.Custom Transform Matrix` | Matrix | Full transform matrix, used by the custom-matrix path. |

| Output | Type | Notes |
|---|---|---|
| `OutPosition` | Vector | Final particle position. |
| `OutVector` | Vector | Transformed vector for the attributes path. |

| Switch | Options |
|---|---|
| `Apply` | `None`, `Apply To Particle Position`, `Apply To Attributes` |
| `Custom Matrix` | `Default`, `Custom Full Transform Matrix Position`, `Custom Full Transform Matrix Shape Attributes (Normal, etc)` |
| `Transform Order` | `Scale / Offset / Rotate`, `Scale / Rotate / Offset` (`ENiagara_TransformOrder`) |
| `Transform Method` | stack transforms vs. full custom matrix (`ENiagara_TransformType`) |
| `Scale Mode` / `Offset Mode` / `Rotation Mode` | `ENiagara_ScaleMode` / `ENiagara_OffsetMode` / `ENiagara_RotationMode` |
| `Rotation Angle Type` | `ENiagara_AngleInput` |
| `Additional Yaw / Pitch / Roll` | extra Euler rotation on top of the main rotation |
| `Additional Quaternion Rotation` | extra quaternion rotation on top of the main rotation |

**Behaviour**: a stack of scale → rotate → offset operations. Scale is always applied first,
because skewing is not supported. `Transform Order` decides whether the offset happens before or
after rotation: offset-then-rotate keeps the rotation predictable, rotate-then-offset keeps the
offset direction predictable. When `Custom Matrix` is set, the stack is skipped and the full matrix
is used instead.

---

## `NE_2DShapeTemplate`

| Field | Value |
|---|---|
| Path | `/EasyNSShape/NETemplate/NE_2DShapeTemplate` |
| Type | Niagara Emitter |
| Role | Base 2D emitter: spawns particles, drives `NMS_2DShapeVector`, renders the curve. |
| Notes | Uses `Emitter.InterpolatedSpawn`. Per-shape emitters are created from it. |

---

## `NE_3DShapeTemplate`

| Field | Value |
|---|---|
| Path | `/EasyNSShape/NETemplate/NE_3DShapeTemplate` |
| Type | Niagara Emitter |
| Role | Base 3D emitter: spawns particles, drives `NMS_ShapeVector`, renders the curve. |
| Notes | Uses `Emitter.InterpolatedSpawn`. Per-shape emitters are created from it. |

---

## `NE_2DShapeTemplate_System`

| Field | Value |
|---|---|
| Path | `/EasyNSShape/NETemplate/NE_2DShapeTemplate_System` |
| Type | Niagara System |
| Role | Minimal system that hosts a 2D emitter; used as the starting point for `NS_2D_<Shape>`. |

---

## `NE_3DShapeTemplate_System`

| Field | Value |
|---|---|
| Path | `/EasyNSShape/NETemplate/NE_3DShapeTemplate_System` |
| Type | Niagara System |
| Role | Minimal system that hosts a 3D emitter; used as the starting point for `NS_3D_<Shape>`. |

---

## Node graphs

These three node graphs are the **main nodes of the whole system** — the two emitter templates and
the shared transform module. Every per-shape asset is a copy of one of these graphs with a different
NDI linked in.

### `NE_2DShapeTemplate`

![NE_2DShapeTemplate node graph](../TemplateNode/NE_2DShapeTemplate.jpg)

The 2D emitter template graph. It spawns a particle burst, evaluates the linked 2D NDI through
`NMS_2DShapeVector`, transforms the result with `NMS_Transform`, and renders the curve with a sprite
renderer. Its default dynamic input is `NDI_2D_ArchimedeanSpiral`.

### `NE_3DShapeTemplate`

![NE_3DShapeTemplate node graph](../TemplateNode/NE_3DShapeTemplate.jpg)

The 3D emitter template graph. Identical in structure to the 2D template, but drives
`NMS_ShapeVector` (returning `float3`) instead of `NMS_2DShapeVector`. Its default dynamic input is
`NDI_3D_AstroidExtrude`.

### `NMS_Transform`

![NMS_Transform node graph](../TemplateNode/NMS_Transform.jpg)

The transform graph. Takes `Module.ShapeVector` (+ tangent / normal) and emits `OutPosition` /
`OutVector` after the scale → rotate → offset stack described in the section above. It is the only
module shared by both emitter templates.

### Module stack

Both emitter templates contain the same module stack — they differ only in which vector module they
call and which NDI is linked.

| Stage | Module | Notes |
|---|---|---|
| Emitter Spawn | `EmitterState` | Emitter lifecycle. |
| Emitter Update | `SpawnBurst_Instantaneous` | One instantaneous particle burst per loop. |
| Particle Spawn | `InitializeParticle` | Standard particle initialisation. |
| Particle Spawn | `NMS_2DShapeVector` *(2D)* / `NMS_ShapeVector` *(3D)* | Evaluates the linked NDI at `InputT`. |
| Particle Spawn | `NMS_Transform` | Shape point → final particle position. |
| Particle Update | `ParticleState` | Lifetime / state handling. |

**Renderer**: `NiagaraSpriteRendererProperties` with `/Niagara/DefaultAssets/DefaultSpriteMaterial`.
Both templates have `Emitter.InterpolatedSpawn` enabled.

---

## How the templates relate

```
NE_2DShapeTemplate ──▶ NE_2DShapeTemplate_<Shape> ──▶ NS_2D_<Shape>
        (template)              (per-shape NE)             (per-shape NS)
              │
              ├─ NMS_2DShapeVector ◀── FormularXY ◀── NDI_2D_<Shape> ◀── NFS2D_<Shape>
              └─ NMS_Transform ──▶ OutPosition  (final particle position)

NE_3DShapeTemplate ──▶ NE_3DShapeTemplate_<Shape> ──▶ NS_3D_<Shape>
        (template)              (per-shape NE)             (per-shape NS)
              │
              ├─ NMS_ShapeVector ◀──── Formular ──── NDI_3D_<Shape> ◀── NFS3D_<Shape>
              └─ NMS_Transform ──▶ OutPosition  (final particle position)
```
