# EasyNSShape — NDI / NE 形状参考

EasyNSShape 形状库 — NDI / NE 参考手册


本文件是 `EasyNSShape` 插件全部形状的完整参考。每个形状由一个 **NDI** 动态输入脚本（参数方程）和一个消费它的 **NE** 发射器组成。第 1 节说明管线，之后是每个形状的详细条目。

图例：**中文** = 中文名称，**Formula** = NDI `CustomHlsl` 节点中的 HLSL 内容，**NDI** = 动态输入脚本，**NE** = 发射器，**NS** = 系统，**NFS** = 源函数脚本。

## 管线

```
formula (HLSL)  ->  NDI dynamic input  ->  NMS driver module  ->  NE emitter  ->  NS system
公式(HLSL)          NDI 动态输入            NMS 驱动模块          NE 发射器      NS 系统
```

| 资源类型 | 目录 | Unreal 类型 | 作用 |
|---|---|---|---|
| NDI（动态输入） | `/EasyNSShape/NDI/` | Niagara Script | 求值 `f(InputT)` 的 `CustomHlsl` 节点；**每个形状一个**。 |
| NFS（源函数） | `/EasyNSShape/NFS/` | Niagara Function Script | NDI 所依据的源数学函数。 |
| NMS（驱动模块） | `/EasyNSShape/NETemplate/` | Niagara Module Script | `NMS_2DShapeVector`（输入 `FormularXY`、`ZValue`）/ `NMS_ShapeVector`（输入 `Formular`）。 |
| NE（发射器） | `/EasyNSShape/NEBridge/NE/` | Niagara Emitter | 沿形状生成粒子；**每个形状一个**。 |
| NS（系统） | `/EasyNSShape/NEBridge/NS/` | Niagara System | 包裹一个 NE 的开箱即用系统；**每个形状一个**。 |

所有 NDI 都接受单个浮点参数 **`Module.InputT`**（曲线参数），返回 `float2`（2D）或 `float3`（3D）位置。


## 2D 形状索引 (89)

| 中文 | Formula (HLSL) | NDI | NE | NS | NFS |
|---|---|---|---|---|---|
| 阿基米德螺线 | `OutVector2D = float2(InputT * cos(InputT), InputT * sin(InputT))` | `NDI_2D_ArchimedeanSpiral` | `NE_2DShapeTemplate_ArchimedeanSpiral` | `NS_2D_ArchimedeanSpiral` | `NFS2D_ArchimedeanSpiral` |
| 星形线 | `float2(pow(cos(InputT), 3.0), pow(sin(InputT), 3.0))` | `NDI_2D_Astroid` | `NE_2DShapeTemplate_Astroid` | `NS_2D_Astroid` | `NFS2D_Astroid` |
| 心形线 | `float2(cosh(InputT), InputT)` | `NDI_2D_Cardioid` | `NE_2DShapeTemplate_Cardioid` | `NS_2D_Cardioid` | `NFS2D_Cardioid` |
| 悬链线 | `float2(cosh(InputT), InputT)` | `NDI_2D_Catenary` | `NE_2DShapeTemplate_Catenary` | `NS_2D_Catenary` | `NFS2D_Catenary` |
| 悬链线 2 | `float2(InputT, cosh(InputT))` | `NDI_2D_Catenary2` | `NE_2DShapeTemplate_Catenary2` | `NS_2D_Catenary2` | `NFS2D_Catenary2` |
| 圆 | `float2(cos(InputT), sin(InputT))` | `NDI_2D_Circle` | `NE_2DShapeTemplate_Circle` | `NS_2D_Circle` | `NFS2D_Circle` |
| 圆 2 | `float2(1.0/(1.0+InputT*InputT), InputT/(1.0+InputT*InputT))` | `NDI_2D_Circle2` | `NE_2DShapeTemplate_Circle2` | `NS_2D_Circle2` | `NFS2D_Circle2` |
| 圆心圆 | `float2(2.0*InputT*InputT/(1.0+InputT*InputT), 2.0*InputT/(1.0+InputT*InputT))` | `NDI_2D_CircleCenter` | `NE_2DShapeTemplate_CircleCenter` | `NS_2D_CircleCenter` | `NFS2D_CircleCenter` |
| 非线性圆 | `float2(sin(InputT*InputT), cos(InputT*InputT))` | `NDI_2D_CircleNonlinear` | `NE_2DShapeTemplate_CircleNonlinear` | `NS_2D_CircleNonlinear` | `NFS2D_CircleNonlinear` |
| 半径圆 | `float2(sin(InputT) + cos(InputT), sin(InputT) - cos(InputT))` | `NDI_2D_CircleRadius` | `NE_2DShapeTemplate_CircleRadius` | `NS_2D_CircleRadius` | `NFS2D_CircleRadius` |
| 三次曲线 | `float2(InputT*InputT*InputT-3.0*InputT, 3.0*InputT*InputT-3.0)` | `NDI_2D_CubicCurve` | `NE_2DShapeTemplate_CubicCurve` | `NS_2D_CubicCurve` | `NFS2D_CubicCurve` |
| 三次曲线 2 | `float2(InputT*InputT+InputT, InputT*InputT*InputT+InputT)` | `NDI_2D_CubicCurve2` | `NE_2DShapeTemplate_CubicCurve2` | `NS_2D_CubicCurve2` | `NFS2D_CubicCurve2` |
| 三次曲线 3 | `float2(InputT*InputT-InputT, InputT*InputT*InputT-InputT*InputT)` | `NDI_2D_CubicCurve3` | `NE_2DShapeTemplate_CubicCurve3` | `NS_2D_CubicCurve3` | `NFS2D_CubicCurve3` |
| 三次曲线 4 | `float2(InputT * InputT, InputT * InputT * InputT - InputT)` | `NDI_2D_CubicCurve4` | `NE_2DShapeTemplate_CubicCurve4` | `NS_2D_CubicCurve4` | `NFS2D_CubicCurve4` |
| 三次曲线 5 | `float2(InputT * InputT + 2.0, InputT * InputT * InputT + 3.0 * InputT)` | `NDI_2D_CubicCurve5` | `NE_2DShapeTemplate_CubicCurve5` | `NS_2D_CubicCurve5` | `NFS2D_CubicCurve5` |
| 三次曲线 6 | `float2(InputT * InputT * InputT - InputT, InputT * InputT + 1.0)` | `NDI_2D_CubicCurve6` | `NE_2DShapeTemplate_CubicCurve6` | `NS_2D_CubicCurve6` | `NFS2D_CubicCurve6` |
| 三次曲线 7 | `float2(InputT * InputT, InputT * InputT * InputT + InputT * InputT)` | `NDI_2D_CubicCurve7` | `NE_2DShapeTemplate_CubicCurve7` | `NS_2D_CubicCurve7` | `NFS2D_CubicCurve7` |
| 三次曲线 8 | `float2(InputT * InputT * InputT + InputT, InputT * InputT - InputT)` | `NDI_2D_CubicCurve8` | `NE_2DShapeTemplate_CubicCurve8` | `NS_2D_CubicCurve8` | `NFS2D_CubicCurve8` |
| 三次抛物线 | `float2(InputT, InputT * InputT * InputT)` | `NDI_2D_CubicParabola` | `NE_2DShapeTemplate_CubicParabola` | `NS_2D_CubicParabola` | `NFS2D_CubicParabola` |
| 摆线 | `float2(InputT-sin(InputT), 1.0-cos(InputT))` | `NDI_2D_Cycloid` | `NE_2DShapeTemplate_Cycloid` | `NS_2D_Cycloid` | `NFS2D_Cycloid` |
| 摆线 2 | `float2(InputT+sin(InputT), 1.0+cos(InputT))` | `NDI_2D_Cycloid2` | `NE_2DShapeTemplate_Cycloid2` | `NS_2D_Cycloid2` | `NFS2D_Cycloid2` |
| 交换摆线 | `float2(1.0-cos(InputT), InputT-sin(InputT))` | `NDI_2D_CycloidSwapped` | `NE_2DShapeTemplate_CycloidSwapped` | `NS_2D_CycloidSwapped` | `NFS2D_CycloidSwapped` |
| 三角星形线 | `float2(2.0*cos(InputT)-cos(2.0*InputT), 2.0*sin(InputT)-sin(2.0*InputT))` | `NDI_2D_Deltoid` | `NE_2DShapeTemplate_Deltoid` | `NS_2D_Deltoid` | `NFS2D_Deltoid` |
| 椭圆 | `float2(cos(InputT), sin(InputT))` | `NDI_2D_Ellipse` | `NE_2DShapeTemplate_Ellipse` | `NS_2D_Ellipse` | `NFS2D_Ellipse` |
| 等距曲线 | `float2(InputT+cos(InputT), InputT+sin(InputT))` | `NDI_2D_Equidistant` | `NE_2DShapeTemplate_Equidistant` | `NS_2D_Equidistant` | `NFS2D_Equidistant` |
| 指数平方曲线 | `float2(exp(InputT), InputT*InputT)` | `NDI_2D_ExpSquare` | `NE_2DShapeTemplate_ExpSquare` | `NS_2D_ExpSquare` | `NFS2D_ExpSquare` |
| 笛卡尔叶形线 | `float2(3.0*InputT/(1.0+InputT*InputT*InputT), 3.0*InputT*InputT/(1.0+InputT*InputT*InputT))` | `NDI_2D_FoliumDescartes` | `NE_2DShapeTemplate_FoliumDescartes` | `NS_2D_FoliumDescartes` | `NFS2D_FoliumDescartes` |
| 双曲余弦-正弦曲线 | `float2(cosh(InputT), sinh(InputT))` | `NDI_2D_HyperbolaCoshSinh` | `NE_2DShapeTemplate_HyperbolaCoshSinh` | `NS_2D_HyperbolaCoshSinh` | `NFS2D_HyperbolaCoshSinh` |
| 指数双曲线 | `float2(exp(InputT), exp(-InputT))` | `NDI_2D_HyperbolaExp` | `NE_2DShapeTemplate_HyperbolaExp` | `NS_2D_HyperbolaExp` | `NFS2D_HyperbolaExp` |
| 指数双曲线 2 | `float2(exp(InputT * InputT), exp(-InputT * InputT))` | `NDI_2D_HyperbolaExp2` | `NE_2DShapeTemplate_HyperbolaExp2` | `NS_2D_HyperbolaExp2` | `NFS2D_HyperbolaExp2` |
| 正割-正切双曲线 | `float2(1.0/cos(InputT), tan(InputT))` | `NDI_2D_HyperbolaSecTan` | `NE_2DShapeTemplate_HyperbolaSecTan` | `NS_2D_HyperbolaSecTan` | `NFS2D_HyperbolaSecTan` |
| 双曲正弦-余弦曲线 | `float2(sinh(InputT), cosh(InputT))` | `NDI_2D_HyperbolaSinhCosh` | `NE_2DShapeTemplate_HyperbolaSinhCosh` | `NS_2D_HyperbolaSinhCosh` | `NFS2D_HyperbolaSinhCosh` |
| 正切-正割双曲线 | `float2(tan(InputT), 1.0 / cos(InputT))` | `NDI_2D_HyperbolaTanSec` | `NE_2DShapeTemplate_HyperbolaTanSec` | `NS_2D_HyperbolaTanSec` | `NFS2D_HyperbolaTanSec` |
| 圆的渐开线 | `float2(cos(InputT)+InputT*sin(InputT), sin(InputT)-InputT*cos(InputT))` | `NDI_2D_InvoluteCircle` | `NE_2DShapeTemplate_InvoluteCircle` | `NS_2D_InvoluteCircle` | `NFS2D_InvoluteCircle` |
| 线段 | `float2(asin(InputT), acos(InputT))` | `NDI_2D_LineSegment` | `NE_2DShapeTemplate_LineSegment` | `NS_2D_LineSegment` | `NFS2D_LineSegment` |
| 利萨茹曲线 1:2 | `float2(sin(InputT), sin(2.0*InputT))` | `NDI_2D_Lissajous12` | `NE_2DShapeTemplate_Lissajous12` | `NS_2D_Lissajous12` | `NFS2D_Lissajous12` |
| 利萨茹曲线 1:2 (余弦) | `float2(sin(InputT), cos(2.0*InputT))` | `NDI_2D_Lissajous12Cos` | `NE_2DShapeTemplate_Lissajous12Cos` | `NS_2D_Lissajous12Cos` | `NFS2D_Lissajous12Cos` |
| 利萨茹曲线 1:3 | `float2(sin(InputT), cos(3.0*InputT))` | `NDI_2D_Lissajous13` | `NE_2DShapeTemplate_Lissajous13` | `NS_2D_Lissajous13` | `NFS2D_Lissajous13` |
| 利萨茹曲线 2:1 | `float2(sin(2.0*InputT), sin(InputT))` | `NDI_2D_Lissajous21` | `NE_2DShapeTemplate_Lissajous21` | `NS_2D_Lissajous21` | `NFS2D_Lissajous21` |
| 利萨茹曲线 2:3 | `float2(cos(2.0*InputT), cos(3.0*InputT))` | `NDI_2D_Lissajous23` | `NE_2DShapeTemplate_Lissajous23` | `NS_2D_Lissajous23` | `NFS2D_Lissajous23` |
| 利萨茹曲线 2:3 变体 B | `float2(2.0*sin(InputT), sin(3.0*InputT))` | `NDI_2D_Lissajous23b` | `NE_2DShapeTemplate_Lissajous23b` | `NS_2D_Lissajous23b` | `NFS2D_Lissajous23b` |
| 利萨茹曲线 2:3 变体 C | `float2(sin(2.0*InputT), sin(3.0*InputT))` | `NDI_2D_Lissajous23c` | `NE_2DShapeTemplate_Lissajous23c` | `NS_2D_Lissajous23c` | `NFS2D_Lissajous23c` |
| 利萨茹曲线 2:3 变体 D | `float2(cos(2.0 * InputT), sin(3.0 * InputT))` | `NDI_2D_Lissajous23d` | `NE_2DShapeTemplate_Lissajous23d` | `NS_2D_Lissajous23d` | `NFS2D_Lissajous23d` |
| 利萨茹曲线 3:2 | `float2(sin(3.0*InputT), sin(2.0*InputT))` | `NDI_2D_Lissajous32` | `NE_2DShapeTemplate_Lissajous32` | `NS_2D_Lissajous32` | `NFS2D_Lissajous32` |
| 利萨茹曲线 3:4 | `float2(cos(3.0 * InputT), sin(4.0 * InputT))` | `NDI_2D_Lissajous34` | `NE_2DShapeTemplate_Lissajous34` | `NS_2D_Lissajous34` | `NFS2D_Lissajous34` |
| 利萨茹曲线 4:5 | `float2(cos(4.0*InputT), sin(5.0*InputT))` | `NDI_2D_Lissajous45` | `NE_2DShapeTemplate_Lissajous45` | `NS_2D_Lissajous45` | `NFS2D_Lissajous45` |
| 利萨茹曲线 5:7 | `float2(sin(5.0 * InputT), cos(7.0 * InputT))` | `NDI_2D_Lissajous57` | `NE_2DShapeTemplate_Lissajous57` | `NS_2D_Lissajous57` | `NFS2D_Lissajous57` |
| 对数螺线 | `float2(exp(InputT) * cos(InputT), exp(InputT) * sin(InputT))` | `NDI_2D_LogarithmicSpiral` | `NE_2DShapeTemplate_LogarithmicSpiral` | `NS_2D_LogarithmicSpiral` | `NFS2D_LogarithmicSpiral` |
| 对数曲线 | `float2(log(InputT), InputT)` | `NDI_2D_LogCurve` | `NE_2DShapeTemplate_LogCurve` | `NS_2D_LogCurve` | `NFS2D_LogCurve` |
| 对数抛物线 | `float2(InputT*InputT, log(InputT))` | `NDI_2D_LogParabola` | `NE_2DShapeTemplate_LogParabola` | `NS_2D_LogParabola` | `NFS2D_LogParabola` |
| 对数正割曲线 | `float2(log(InputT * InputT + 1.0), atan(InputT))` | `NDI_2D_LogSecant` | `NE_2DShapeTemplate_LogSecant` | `NS_2D_LogSecant` | `NFS2D_LogSecant` |
| 抛物线 | `float2(InputT, InputT * InputT)` | `NDI_2D_Parabola` | `NE_2DShapeTemplate_Parabola` | `NS_2D_Parabola` | `NFS2D_Parabola` |
| 高次抛物线 | `float2(pow(InputT, 5.0), pow(InputT, 10.0))` | `NDI_2D_ParabolaHigh` | `NE_2DShapeTemplate_ParabolaHigh` | `NS_2D_ParabolaHigh` | `NFS2D_ParabolaHigh` |
| 有理抛物线 | `float2(InputT + 1.0 / InputT, InputT * InputT + 1.0 / (InputT * InputT))` | `NDI_2D_ParabolaRational` | `NE_2DShapeTemplate_ParabolaRational` | `NS_2D_ParabolaRational` | `NFS2D_ParabolaRational` |
| 平移抛物线 | `float2(InputT + 1.0, (InputT + 1.0) * (InputT + 1.0))` | `NDI_2D_ParabolaShifted` | `NE_2DShapeTemplate_ParabolaShifted` | `NS_2D_ParabolaShifted` | `NFS2D_ParabolaShifted` |
| 抛物线 V2 | `float2(InputT*InputT, InputT*InputT-2.0*InputT)` | `NDI_2D_ParabolaV2` | `NE_2DShapeTemplate_ParabolaV2` | `NS_2D_ParabolaV2` | `NFS2D_ParabolaV2` |
| 抛物线 V3 | `float2(InputT, InputT*InputT-2.0*InputT)` | `NDI_2D_ParabolaV3` | `NE_2DShapeTemplate_ParabolaV3` | `NS_2D_ParabolaV3` | `NFS2D_ParabolaV3` |
| 抛物弧线 | `float2(cos(InputT), cos(2.0*InputT))` | `NDI_2D_ParabolicArc` | `NE_2DShapeTemplate_ParabolicArc` | `NS_2D_ParabolicArc` | `NFS2D_ParabolicArc` |
| 幂曲线 | `float2(sqrt(InputT), InputT*InputT)` | `NDI_2D_PowerCurve` | `NE_2DShapeTemplate_PowerCurve` | `NS_2D_PowerCurve` | `NFS2D_PowerCurve` |
| 幂曲线 2 | `float2(InputT*InputT*InputT, sqrt(InputT))` | `NDI_2D_PowerCurve2` | `NE_2DShapeTemplate_PowerCurve2` | `NS_2D_PowerCurve2` | `NFS2D_PowerCurve2` |
| 幂曲线 3 | `float2(pow(InputT, 4.0), pow(InputT, 6.0))` | `NDI_2D_PowerCurve3` | `NE_2DShapeTemplate_PowerCurve3` | `NS_2D_PowerCurve3` | `NFS2D_PowerCurve3` |
| 四次曲线 | `float2(InputT * InputT * InputT - 3.0 * InputT, pow(InputT, 4.0) - 2.0 * InputT * InputT)` | `NDI_2D_QuarticCurve` | `NE_2DShapeTemplate_QuarticCurve` | `NS_2D_QuarticCurve` | `NFS2D_QuarticCurve` |
| 四次曲线 2 | `float2(pow(InputT, 4.0) - InputT * InputT, InputT * InputT * InputT)` | `NDI_2D_QuarticCurve2` | `NE_2DShapeTemplate_QuarticCurve2` | `NS_2D_QuarticCurve2` | `NFS2D_QuarticCurve2` |
| 四次抛物线 | `float2(InputT * InputT, pow(InputT, 4.0))` | `NDI_2D_QuarticParabola` | `NE_2DShapeTemplate_QuarticParabola` | `NS_2D_QuarticParabola` | `NFS2D_QuarticParabola` |
| 有理曲线 | `float2(InputT/(1.0+InputT*InputT), InputT*InputT/(1.0+InputT*InputT))` | `NDI_2D_RationalCurve` | `NE_2DShapeTemplate_RationalCurve` | `NS_2D_RationalCurve` | `NFS2D_RationalCurve` |
| 有理曲线 2 | `float2(InputT*InputT/(1.0+InputT*InputT), InputT/(1.0+InputT*InputT))` | `NDI_2D_RationalCurve2` | `NE_2DShapeTemplate_RationalCurve2` | `NS_2D_RationalCurve2` | `NFS2D_RationalCurve2` |
| 有理曲线 3 | `float2(InputT*InputT+1.0/InputT, InputT*InputT-1.0/InputT)` | `NDI_2D_RationalCurve3` | `NE_2DShapeTemplate_RationalCurve3` | `NS_2D_RationalCurve3` | `NFS2D_RationalCurve3` |
| 有理曲线 4 | `float2(InputT*InputT/(1.0+InputT*InputT), InputT*InputT*InputT/(1.0+InputT*InputT))` | `NDI_2D_RationalCurve4` | `NE_2DShapeTemplate_RationalCurve4` | `NS_2D_RationalCurve4` | `NFS2D_RationalCurve4` |
| 有理曲线 5 | `float2((InputT*InputT*InputT-1.0)/(InputT*InputT+1.0), (InputT*InputT*InputT+1.0)/(InputT*InputT+1.0))` | `NDI_2D_RationalCurve5` | `NE_2DShapeTemplate_RationalCurve5` | `NS_2D_RationalCurve5` | `NFS2D_RationalCurve5` |
| 有理曲线 6 | `float2(InputT * InputT + 1.0 / (InputT * InputT), InputT * InputT * InputT + 1.0 / (InputT * InputT * InputT))` | `NDI_2D_RationalCurve6` | `NE_2DShapeTemplate_RationalCurve6` | `NS_2D_RationalCurve6` | `NFS2D_RationalCurve6` |
| 直角双曲线 | `float2(InputT, 1.0/InputT)` | `NDI_2D_RectangularHyperbola` | `NE_2DShapeTemplate_RectangularHyperbola` | `NS_2D_RectangularHyperbola` | `NFS2D_RectangularHyperbola` |
| 直角双曲线 2 | `float2(InputT+1.0/InputT, InputT-1.0/InputT)` | `NDI_2D_RectHyperbola2` | `NE_2DShapeTemplate_RectHyperbola2` | `NS_2D_RectHyperbola2` | `NFS2D_RectHyperbola2` |
| 直角双曲线 3 | `float2((InputT * InputT + 1.0) / InputT, (InputT * InputT - 1.0) / InputT)` | `NDI_2D_RectHyperbola3` | `NE_2DShapeTemplate_RectHyperbola3` | `NS_2D_RectHyperbola3` | `NFS2D_RectHyperbola3` |
| 旋转椭圆 | `float2(sin(InputT), sin(InputT + 1.0472))` | `NDI_2D_RotatedEllipse` | `NE_2DShapeTemplate_RotatedEllipse` | `NS_2D_RotatedEllipse` | `NFS2D_RotatedEllipse` |
| 半立方抛物线 | `float2(InputT * InputT, InputT * InputT * InputT)` | `NDI_2D_SemicubicalParabola` | `NE_2DShapeTemplate_SemicubicalParabola` | `NS_2D_SemicubicalParabola` | `NFS2D_SemicubicalParabola` |
| 半立方抛物线 2 | `float2(InputT*InputT*InputT, InputT*InputT)` | `NDI_2D_SemicubicalParabola2` | `NE_2DShapeTemplate_SemicubicalParabola2` | `NS_2D_SemicubicalParabola2` | `NFS2D_SemicubicalParabola2` |
| 正弦平方曲线 | `float2(cos(InputT), sin(InputT)*sin(InputT))` | `NDI_2D_SineSquared` | `NE_2DShapeTemplate_SineSquared` | `NS_2D_SineSquared` | `NFS2D_SineSquared` |
| 螺线曲线 | `float2(cos(3.14159 * InputT), sin(3.14159 * InputT * InputT))` | `NDI_2D_SpiralCurve` | `NE_2DShapeTemplate_SpiralCurve` | `NS_2D_SpiralCurve` | `NFS2D_SpiralCurve` |
| 曳物线 | `float2(cos(InputT)+log(tan(InputT*0.5)), sin(InputT))` | `NDI_2D_Tractrix` | `NE_2DShapeTemplate_Tractrix` | `NS_2D_Tractrix` | `NFS2D_Tractrix` |
| 曳物线 2 | `float2(1.0/cosh(InputT), InputT-tanh(InputT))` | `NDI_2D_Tractrix2` | `NE_2DShapeTemplate_Tractrix2` | `NS_2D_Tractrix2` | `NFS2D_Tractrix2` |
| 曳物线 3 | `float2(InputT - tanh(InputT), 1.0 / cosh(InputT))` | `NDI_2D_Tractrix3` | `NE_2DShapeTemplate_Tractrix3` | `NS_2D_Tractrix3` | `NFS2D_Tractrix3` |
| 次摆线 | `float2(InputT+sin(InputT), 1.0+cos(InputT))` | `NDI_2D_Trochoid` | `NE_2DShapeTemplate_Trochoid` | `NS_2D_Trochoid` | `NFS2D_Trochoid` |
| 次摆线 2 | `float2(2.0*InputT-sin(InputT), 2.0-cos(InputT))` | `NDI_2D_Trochoid2` | `NE_2DShapeTemplate_Trochoid2` | `NS_2D_Trochoid2` | `NFS2D_Trochoid2` |
| 单位圆 | `float2((1.0-InputT*InputT)/(1.0+InputT*InputT), 2.0*InputT/(1.0+InputT*InputT))` | `NDI_2D_UnitCircle` | `NE_2DShapeTemplate_UnitCircle` | `NS_2D_UnitCircle` | `NFS2D_UnitCircle` |
| 单位圆 3 | `float2((InputT * InputT - 1.0) / (InputT * InputT + 1.0), 2.0 * InputT / (InputT * InputT + 1.0))` | `NDI_2D_UnitCircle3` | `NE_2DShapeTemplate_UnitCircle3` | `NS_2D_UnitCircle3` | `NFS2D_UnitCircle3` |
| 单位圆 4 | `float2(2.0 * InputT / (InputT * InputT + 1.0), (InputT * InputT - 1.0) / (InputT * InputT + 1.0))` | `NDI_2D_UnitCircle4` | `NE_2DShapeTemplate_UnitCircle4` | `NS_2D_UnitCircle4` | `NFS2D_UnitCircle4` |
| 双曲单位圆 | `float2(tanh(InputT), 1.0 / cosh(InputT))` | `NDI_2D_UnitCircleHyperbolic` | `NE_2DShapeTemplate_UnitCircleHyperbolic` | `NS_2D_UnitCircleHyperbolic` | `NFS2D_UnitCircleHyperbolic` |
| 阿涅西箕舌线 | `float2(InputT, 1.0 / (1.0 + InputT * InputT))` | `NDI_2D_WitchAgnesi` | `NE_2DShapeTemplate_WitchAgnesi` | `NS_2D_WitchAgnesi` | `NFS2D_WitchAgnesi` |
| 阿涅西箕舌线变体 | `float2(InputT / (InputT * InputT + 1.0), (InputT * InputT - 1.0) / (InputT * InputT + 1.0))` | `NDI_2D_WitchAgnesiVariant` | `NE_2DShapeTemplate_WitchAgnesiVariant` | `NS_2D_WitchAgnesiVariant` | `NFS2D_WitchAgnesiVariant` |

## 3D 形状索引 (27)

| 中文 | Formula (HLSL) | NDI | NE | NS | NFS |
|---|---|---|---|---|---|
| 星形线拉伸体 | `float3(pow(cos(InputT),3.0), pow(sin(InputT),3.0), InputT)` | `NDI_3D_AstroidExtrude` | `NE_3DShapeTemplate_AstroidExtrude` | `NS_3D_AstroidExtrude` | `NFS3D_AstroidExtrude` |
| 圆柱螺旋线 | `float3(cos(InputT), sin(InputT), InputT)` | `NDI_3D_CircularHelix` | `NE_3DShapeTemplate_CircularHelix` | `NS_3D_CircularHelix` | `NFS3D_CircularHelix` |
| 克利福德环面纽结 | `float3((2.0+cos(2.0*InputT))*cos(3.0*InputT), (2.0+cos(2.0*InputT))*sin(3.0*InputT), sin(2.0*InputT))` | `NDI_3D_CliffordTorusKnot` | `NE_3DShapeTemplate_CliffordTorusKnot` | `NS_3D_CliffordTorusKnot` | `NFS3D_CliffordTorusKnot` |
| 圆锥螺旋线 | `float3(InputT * cos(InputT), InputT * sin(InputT), InputT)` | `NDI_3D_ConicalHelix` | `NE_3DShapeTemplate_ConicalHelix` | `NS_3D_ConicalHelix` | `NFS3D_ConicalHelix` |
| 三次螺旋线 | `float3(InputT, InputT*InputT, InputT*InputT*InputT)` | `NDI_3D_CubicHelix` | `NE_3DShapeTemplate_CubicHelix` | `NS_3D_CubicHelix` | `NFS3D_CubicHelix` |
| 摆线螺旋 | `float3(cos(InputT)*sin(InputT), sin(InputT)*sin(InputT), cos(InputT))` | `NDI_3D_CycloidalHelix` | `NE_3DShapeTemplate_CycloidalHelix` | `NS_3D_CycloidalHelix` | `NFS3D_CycloidalHelix` |
| 蛋形螺旋线 | `float3(cos(InputT)*cos(InputT), cos(InputT)*sin(InputT), sin(InputT))` | `NDI_3D_EggHelix` | `NE_3DShapeTemplate_EggHelix` | `NS_3D_EggHelix` | `NFS3D_EggHelix` |
| 八字结 | `float3((2.0+cos(2.0*InputT))*cos(3.0*InputT), (2.0+cos(2.0*InputT))*sin(3.0*InputT), sin(4.0*InputT))` | `NDI_3D_FigureEightKnot` | `NE_3DShapeTemplate_FigureEightKnot` | `NS_3D_FigureEightKnot` | `NFS3D_FigureEightKnot` |
| 球面大圆 | `float3(cos(InputT*0.5)*cos(InputT), cos(InputT*0.5)*sin(InputT), sin(InputT*0.5))` | `NDI_3D_GreatCircleSphere` | `NE_3DShapeTemplate_GreatCircleSphere` | `NS_3D_GreatCircleSphere` | `NFS3D_GreatCircleSphere` |
| 双曲螺旋线 | `float3(cosh(InputT)*cos(InputT), cosh(InputT)*sin(InputT), sinh(InputT))` | `NDI_3D_HyperbolicHelix` | `NE_3DShapeTemplate_HyperbolicHelix` | `NS_3D_HyperbolicHelix` | `NFS3D_HyperbolicHelix` |
| 双曲螺线柱面 | `float3(cos(InputT), sin(InputT), InputT/(InputT*InputT+1.0))` | `NDI_3D_HyperbolicSpiralCylinder` | `NE_3DShapeTemplate_HyperbolicSpiralCylinder` | `NS_3D_HyperbolicSpiralCylinder` | `NFS3D_HyperbolicSpiralCylinder` |
| 渐开线柱面 | `float3(cos(InputT)+InputT*sin(InputT), sin(InputT)-InputT*cos(InputT), InputT)` | `NDI_3D_InvoluteCylinder` | `NE_3DShapeTemplate_InvoluteCylinder` | `NS_3D_InvoluteCylinder` | `NFS3D_InvoluteCylinder` |
| 三维直线 | `float3(InputT, InputT*0.5, InputT*0.3)` | `NDI_3D_Line3D` | `NE_3DShapeTemplate_Line3D` | `NS_3D_Line3D` | `NFS3D_Line3D` |
| 利萨茹柱面 | `float3(cos(InputT), sin(InputT), cos(2.0*InputT))` | `NDI_3D_LissajousCylinder` | `NE_3DShapeTemplate_LissajousCylinder` | `NS_3D_LissajousCylinder` | `NFS3D_LissajousCylinder` |
| 莫比乌斯中心线 | `float3(cos(InputT), sin(InputT), 0.0)` | `NDI_3D_MobiusCenterLine` | `NE_3DShapeTemplate_MobiusCenterLine` | `NS_3D_MobiusCenterLine` | `NFS3D_MobiusCenterLine` |
| 抛物螺旋线 | `float3(InputT*cos(InputT), InputT*sin(InputT), InputT*InputT)` | `NDI_3D_ParabolicHelix` | `NE_3DShapeTemplate_ParabolicHelix` | `NS_3D_ParabolicHelix` | `NFS3D_ParabolicHelix` |
| 四次空间曲线 | `float3(pow(InputT,4.0)-2.0*InputT*InputT, InputT*InputT*InputT, InputT*InputT)` | `NDI_3D_QuarticSpace` | `NE_3DShapeTemplate_QuarticSpace` | `NS_3D_QuarticSpace` | `NFS3D_QuarticSpace` |
| 正弦余弦空间曲线 | `float3(InputT, sin(InputT), cos(InputT))` | `NDI_3D_SineCosineSpace` | `NE_3DShapeTemplate_SineCosineSpace` | `NS_3D_SineCosineSpace` | `NFS3D_SineCosineSpace` |
| 正弦抛物线 | `float3(InputT, InputT*InputT, sin(InputT))` | `NDI_3D_SineParabolic` | `NE_3DShapeTemplate_SineParabolic` | `NS_3D_SineParabolic` | `NFS3D_SineParabolic` |
| 正弦柱面 | `float3(cos(InputT), sin(InputT), sin(InputT))` | `NDI_3D_SinusoidalCylinder` | `NE_3DShapeTemplate_SinusoidalCylinder` | `NS_3D_SinusoidalCylinder` | `NFS3D_SinusoidalCylinder` |
| 倾斜螺旋线 | `float3(cos(InputT), sin(InputT), InputT*InputT)` | `NDI_3D_SlantedHelix` | `NE_3DShapeTemplate_SlantedHelix` | `NS_3D_SlantedHelix` | `NFS3D_SlantedHelix` |
| 球面螺旋线 | `float3(cos(InputT)*sin(InputT), sin(InputT)*sin(InputT), cos(InputT))` | `NDI_3D_SphericalHelix` | `NE_3DShapeTemplate_SphericalHelix` | `NS_3D_SphericalHelix` | `NFS3D_SphericalHelix` |
| 环面纽结 (5,2) | `float3(cos(3.0*InputT)*cos(2.0*InputT), cos(3.0*InputT)*sin(2.0*InputT), sin(3.0*InputT))` | `NDI_3D_ToroidalKnot52` | `NE_3DShapeTemplate_ToroidalKnot52` | `NS_3D_ToroidalKnot52` | `NFS3D_ToroidalKnot52` |
| 环面纽结 (5,2) 变体 | `float3((2.0+cos(2.0*InputT))*cos(5.0*InputT), (2.0+cos(2.0*InputT))*sin(5.0*InputT), sin(2.0*InputT))` | `NDI_3D_ToroidalKnot52b` | `NE_3DShapeTemplate_ToroidalKnot52b` | `NS_3D_ToroidalKnot52b` | `NFS3D_ToroidalKnot52b` |
| 三叶结 | `float3(sin(InputT)+2.0*sin(2.0*InputT), cos(InputT)-2.0*cos(2.0*InputT), -sin(3.0*InputT))` | `NDI_3D_TrefoilKnot` | `NE_3DShapeTemplate_TrefoilKnot` | `NS_3D_TrefoilKnot` | `NFS3D_TrefoilKnot` |
| 变半径螺旋线 | `float3((1.0+0.5*InputT)*cos(InputT), (1.0+0.5*InputT)*sin(InputT), InputT*0.3)` | `NDI_3D_VariableRadiusHelix` | `NE_3DShapeTemplate_VariableRadiusHelix` | `NS_3D_VariableRadiusHelix` | `NFS3D_VariableRadiusHelix` |
| 维维亚尼曲线 | `float3(cos(InputT)*cos(InputT), cos(InputT)*sin(InputT), sin(InputT))` | `NDI_3D_Viviani` | `NE_3DShapeTemplate_Viviani` | `NS_3D_Viviani` | `NFS3D_Viviani` |

## 3D 特型形状索引 (12)


多参数 3D 形状。相比单参数的 `NDI_3D_*`，这些形状额外暴露 `InputU`/`InputV`、`InputA`…`InputC`、`InputR`/`InputRho`/`InputN`、`InputP0`…`InputP3` 等输入，可独立驱动半径、扭转率、控制点等参数。资源位于 `Special/` 子目录。

| 中文 | Formula (HLSL) | NDI | NE | NS | NFS |
|---|---|---|---|---|---|
| 圆柱螺旋线 (3参数) | `float3(InputU*cos(InputT), InputU*sin(InputT), InputV*InputT)` | `NDI_3P_3D_CircularHelix` | `NE_3P_3DShapeTemplate_CircularHelix` | `NS_3P_3DShapeTemplate_CircularHelix` | `NFS_3P_3D_CircularHelix` |
| 摆线螺旋 (3参数) | `float3(InputU*(InputT-sin(InputT)), InputU*(1.0-cos(InputT)), InputV*InputT)` | `NDI_3P_3D_CycloidalHelix` | `NE_3P_3DShapeTemplate_CycloidalHelix` | `NS_3P_3DShapeTemplate_CycloidalHelix` | `NFS_3P_3D_CycloidalHelix` |
| 渐开线柱面 (3参数) | `float3(InputU*(cos(InputT)+InputT*sin(InputT)), InputU*(sin(InputT)-InputT*cos(InputT)), InputV*InputT)` | `NDI_3P_3D_InvoluteCylinder` | `NE_3P_3DShapeTemplate_InvoluteCylinder` | `NS_3P_3DShapeTemplate_InvoluteCylinder` | `NFS_3P_3D_InvoluteCylinder` |
| 倾斜螺旋线 (3参数) | `float3(InputU*cos(InputT), InputU*sin(InputT), InputV*InputT*InputT)` | `NDI_3P_3D_SlantedHelix` | `NE_3P_3DShapeTemplate_SlantedHelix` | `NS_3P_3DShapeTemplate_SlantedHelix` | `NFS_3P_3D_SlantedHelix` |
| 球面螺旋线 (3参数) | `float3(InputU*cos(InputT)*sin(InputV*InputT), InputU*sin(InputT)*sin(InputV*InputT), InputU*cos(InputV*InputT))` | `NDI_3P_3D_SphericalHelix` | `NE_3P_3DShapeTemplate_SphericalHelix` | `NS_3P_3DShapeTemplate_SphericalHelix` | `NFS_3P_3D_SphericalHelix` |
| 蛋形螺旋线 (4参数) | `float3((InputA+InputB*cos(InputT))*cos(InputT), (InputA+InputB*cos(InputT))*sin(InputT), InputC*sin(InputT))` | `NDI_4P_3D_EggHelix` | `NE_4P_3DShapeTemplate_EggHelix` | `NS_4P_3DShapeTemplate_EggHelix` | `NFS_4P_3D_EggHelix` |
| 正弦柱面 (4参数) | `float3(InputR*cos(InputT), InputR*sin(InputT), InputA*sin(InputK*InputT))` | `NDI_4P_3D_SinusoidalCylinder` | `NE_4P_3DShapeTemplate_SinusoidalCylinder` | `NS_4P_3DShapeTemplate_SinusoidalCylinder` | `NFS_4P_3D_SinusoidalCylinder` |
| 倾斜椭圆螺旋线 (4参数) | `float3(InputA*cos(InputT), InputB*sin(InputT), InputC*InputT*sin(InputT))` | `NDI_4P_3D_TiltedEllipticHelix` | `NE_4P_3DShapeTemplate_TiltedEllipticHelix` | `NS_4P_3DShapeTemplate_TiltedEllipticHelix` | `NFS_4P_3D_TiltedEllipticHelix` |
| 环形螺旋线 (4参数) | `float3((InputR+InputRho*cos(InputN*InputT))*cos(InputT), (InputR+InputRho*cos(InputN*InputT))*sin(InputT), InputRho*sin(InputN*InputT))` | `NDI_4P_3D_ToroidalHelix` | `NE_4P_3DShapeTemplate_ToroidalHelix` | `NS_4P_3DShapeTemplate_ToroidalHelix` | `NFS_4P_3D_ToroidalHelix` |
| 变半径螺旋线 (4参数) | `float3((InputA+InputB*InputT)*cos(InputT), (InputA+InputB*InputT)*sin(InputT), InputC*InputT)` | `NDI_4P_3D_VariableRadiusHelix` | `NE_4P_3DShapeTemplate_VariableRadiusHelix` | `NS_4P_3DShapeTemplate_VariableRadiusHelix` | `NFS_4P_3D_VariableRadiusHelix` |
| 三次贝塞尔曲线 (5参数) | `pow(1.0-InputT,3.0)*InputP0 + 3.0*pow(1.0-InputT,2.0)*InputT*InputP1 + 3.0*(1.0-InputT)*InputT*InputT*InputP2 + InputT*InputT*InputT*InputP3` | `NDI_5P_3D_CubicBezier` | `NE_5P_3DShapeTemplate_CubicBezier` | `NS_5P_3DShapeTemplate_CubicBezier` | `NFS_5P_3D_CubicBezier` |
| 三维直线 (7参数) | `float3(InputX0+InputA*InputT, InputY0+InputB*InputT, InputZ0+InputC*InputT)` | `NDI_7P_3D_Line3D` | `NE_7P_3DShapeTemplate_Line3D` | `NS_7P_3DShapeTemplate_Line3D` | `NFS_7P_3D_Line3D` |

## 2D 形状详情 (89)

### NDI_2D_ArchimedeanSpiral — 阿基米德螺线

![阿基米德螺线](../Thumbs/NS_2DShapeTemplate_ArchimedeanSpiral.png)

类型**: 2D parametric curve
公式**: `OutVector2D = float2(InputT * cos(InputT), InputT * sin(InputT))`
说明**: Archimedean spiral — radius grows linearly with the angle.
动态输入**: `/EasyNSShape/NDI/NDI_2D_ArchimedeanSpiral`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_ArchimedeanSpiral`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_ArchimedeanSpiral`
源函数**: `/EasyNSShape/NFS/NFS2D_ArchimedeanSpiral`
快照**: [NS_2DShapeTemplate_ArchimedeanSpiral.png](../Thumbs/NS_2DShapeTemplate_ArchimedeanSpiral.png)
视频**: [NS_2DShapeTemplate_ArchimedeanSpiral.mp4](../Videos/NS_2DShapeTemplate_ArchimedeanSpiral.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Astroid — 星形线

![星形线](../Thumbs/NS_2DShapeTemplate_Astroid.png)

类型**: 2D parametric curve
公式**: `float2(pow(cos(InputT), 3.0), pow(sin(InputT), 3.0))`
说明**: Four-cusped hypocycloid (star curve).
动态输入**: `/EasyNSShape/NDI/NDI_2D_Astroid`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Astroid`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Astroid`
源函数**: `/EasyNSShape/NFS/NFS2D_Astroid`
快照**: [NS_2DShapeTemplate_Astroid.png](../Thumbs/NS_2DShapeTemplate_Astroid.png)
视频**: [NS_2DShapeTemplate_Astroid.mp4](../Videos/NS_2DShapeTemplate_Astroid.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Cardioid — 心形线

![心形线](../Thumbs/NS_2DShapeTemplate_Cardioid.png)

类型**: 2D parametric curve
公式**: `float2(cosh(InputT), InputT)`
说明**: Cardioid (heart-shaped curve).
动态输入**: `/EasyNSShape/NDI/NDI_2D_Cardioid`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Cardioid`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Cardioid`
源函数**: `/EasyNSShape/NFS/NFS2D_Cardioid`
快照**: [NS_2DShapeTemplate_Cardioid.png](../Thumbs/NS_2DShapeTemplate_Cardioid.png)
视频**: [NS_2DShapeTemplate_Cardioid.mp4](../Videos/NS_2DShapeTemplate_Cardioid.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Catenary — 悬链线

![悬链线](../Thumbs/NS_2DShapeTemplate_Catenary.png)

类型**: 2D parametric curve
公式**: `float2(cosh(InputT), InputT)`
说明**: Catenary — the shape of a hanging chain (cosh).
动态输入**: `/EasyNSShape/NDI/NDI_2D_Catenary`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Catenary`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Catenary`
源函数**: `/EasyNSShape/NFS/NFS2D_Catenary`
快照**: [NS_2DShapeTemplate_Catenary.png](../Thumbs/NS_2DShapeTemplate_Catenary.png)
视频**: [NS_2DShapeTemplate_Catenary.mp4](../Videos/NS_2DShapeTemplate_Catenary.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Catenary2 — 悬链线 2

![悬链线 2](../Thumbs/NS_2DShapeTemplate_Catenary2.png)

类型**: 2D parametric curve
公式**: `float2(InputT, cosh(InputT))`
说明**: Catenary with the axes swapped.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Catenary2`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Catenary2`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Catenary2`
源函数**: `/EasyNSShape/NFS/NFS2D_Catenary2`
快照**: [NS_2DShapeTemplate_Catenary2.png](../Thumbs/NS_2DShapeTemplate_Catenary2.png)
视频**: [NS_2DShapeTemplate_Catenary2.mp4](../Videos/NS_2DShapeTemplate_Catenary2.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Circle — 圆

![圆](../Thumbs/NS_2DShapeTemplate_Circle.png)

类型**: 2D parametric curve
公式**: `float2(cos(InputT), sin(InputT))`
说明**: Unit circle traced by cos/sin.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Circle`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Circle`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Circle`
源函数**: `/EasyNSShape/NFS/NFS2D_Circle`
快照**: [NS_2DShapeTemplate_Circle.png](../Thumbs/NS_2DShapeTemplate_Circle.png)
视频**: [NS_2DShapeTemplate_Circle.mp4](../Videos/NS_2DShapeTemplate_Circle.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Circle2 — 圆 2

![圆 2](../Thumbs/NS_2DShapeTemplate_Circle2.png)

类型**: 2D parametric curve
公式**: `float2(1.0/(1.0+InputT*InputT), InputT/(1.0+InputT*InputT))`
说明**: Rational-parameterised circle.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Circle2`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Circle2`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Circle2`
源函数**: `/EasyNSShape/NFS/NFS2D_Circle2`
快照**: [NS_2DShapeTemplate_Circle2.png](../Thumbs/NS_2DShapeTemplate_Circle2.png)
视频**: [NS_2DShapeTemplate_Circle2.mp4](../Videos/NS_2DShapeTemplate_Circle2.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_CircleCenter — 圆心圆

![圆心圆](../Thumbs/NS_2DShapeTemplate_CircleCenter.png)

类型**: 2D parametric curve
公式**: `float2(2.0*InputT*InputT/(1.0+InputT*InputT), 2.0*InputT/(1.0+InputT*InputT))`
说明**: Circle whose parameterisation is centred on the origin.
动态输入**: `/EasyNSShape/NDI/NDI_2D_CircleCenter`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_CircleCenter`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_CircleCenter`
源函数**: `/EasyNSShape/NFS/NFS2D_CircleCenter`
快照**: [NS_2DShapeTemplate_CircleCenter.png](../Thumbs/NS_2DShapeTemplate_CircleCenter.png)
视频**: [NS_2DShapeTemplate_CircleCenter.mp4](../Videos/NS_2DShapeTemplate_CircleCenter.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_CircleNonlinear — 非线性圆

![非线性圆](../Thumbs/NS_2DShapeTemplate_CircleNonlinear.png)

类型**: 2D parametric curve
公式**: `float2(sin(InputT*InputT), cos(InputT*InputT))`
说明**: Circle with a non-linear (quadratic) angle sweep.
动态输入**: `/EasyNSShape/NDI/NDI_2D_CircleNonlinear`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_CircleNonlinear`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_CircleNonlinear`
源函数**: `/EasyNSShape/NFS/NFS2D_CircleNonlinear`
快照**: [NS_2DShapeTemplate_CircleNonlinear.png](../Thumbs/NS_2DShapeTemplate_CircleNonlinear.png)
视频**: [NS_2DShapeTemplate_CircleNonlinear.mp4](../Videos/NS_2DShapeTemplate_CircleNonlinear.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_CircleRadius — 半径圆

![半径圆](../Thumbs/NS_2DShapeTemplate_CircleRadius.png)

类型**: 2D parametric curve
公式**: `float2(sin(InputT) + cos(InputT), sin(InputT) - cos(InputT))`
说明**: Circle with a varying radius.
动态输入**: `/EasyNSShape/NDI/NDI_2D_CircleRadius`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_CircleRadius`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_CircleRadius`
源函数**: `/EasyNSShape/NFS/NFS2D_CircleRadius`
快照**: [NS_2DShapeTemplate_CircleRadius.png](../Thumbs/NS_2DShapeTemplate_CircleRadius.png)
视频**: [NS_2DShapeTemplate_CircleRadius.mp4](../Videos/NS_2DShapeTemplate_CircleRadius.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_CubicCurve — 三次曲线

![三次曲线](../Thumbs/NS_2DShapeTemplate_CubicCurve.png)

类型**: 2D parametric curve
公式**: `float2(InputT*InputT*InputT-3.0*InputT, 3.0*InputT*InputT-3.0)`
说明**: Cubic curve, standard form.
动态输入**: `/EasyNSShape/NDI/NDI_2D_CubicCurve`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_CubicCurve`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_CubicCurve`
源函数**: `/EasyNSShape/NFS/NFS2D_CubicCurve`
快照**: [NS_2DShapeTemplate_CubicCurve.png](../Thumbs/NS_2DShapeTemplate_CubicCurve.png)
视频**: [NS_2DShapeTemplate_CubicCurve.mp4](../Videos/NS_2DShapeTemplate_CubicCurve.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_CubicCurve2 — 三次曲线 2

![三次曲线 2](../Thumbs/NS_2DShapeTemplate_CubicCurve2.png)

类型**: 2D parametric curve
公式**: `float2(InputT*InputT+InputT, InputT*InputT*InputT+InputT)`
说明**: Cubic curve, variant 2.
动态输入**: `/EasyNSShape/NDI/NDI_2D_CubicCurve2`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_CubicCurve2`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_CubicCurve2`
源函数**: `/EasyNSShape/NFS/NFS2D_CubicCurve2`
快照**: [NS_2DShapeTemplate_CubicCurve2.png](../Thumbs/NS_2DShapeTemplate_CubicCurve2.png)
视频**: [NS_2DShapeTemplate_CubicCurve2.mp4](../Videos/NS_2DShapeTemplate_CubicCurve2.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_CubicCurve3 — 三次曲线 3

![三次曲线 3](../Thumbs/NS_2DShapeTemplate_CubicCurve3.png)

类型**: 2D parametric curve
公式**: `float2(InputT*InputT-InputT, InputT*InputT*InputT-InputT*InputT)`
说明**: Cubic curve, variant 3.
动态输入**: `/EasyNSShape/NDI/NDI_2D_CubicCurve3`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_CubicCurve3`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_CubicCurve3`
源函数**: `/EasyNSShape/NFS/NFS2D_CubicCurve3`
快照**: [NS_2DShapeTemplate_CubicCurve3.png](../Thumbs/NS_2DShapeTemplate_CubicCurve3.png)
视频**: [NS_2DShapeTemplate_CubicCurve3.mp4](../Videos/NS_2DShapeTemplate_CubicCurve3.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_CubicCurve4 — 三次曲线 4

![三次曲线 4](../Thumbs/NS_2DShapeTemplate_CubicCurve4.png)

类型**: 2D parametric curve
公式**: `float2(InputT * InputT, InputT * InputT * InputT - InputT)`
说明**: Cubic curve, variant 4.
动态输入**: `/EasyNSShape/NDI/NDI_2D_CubicCurve4`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_CubicCurve4`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_CubicCurve4`
源函数**: `/EasyNSShape/NFS/NFS2D_CubicCurve4`
快照**: [NS_2DShapeTemplate_CubicCurve4.png](../Thumbs/NS_2DShapeTemplate_CubicCurve4.png)
视频**: [NS_2DShapeTemplate_CubicCurve4.mp4](../Videos/NS_2DShapeTemplate_CubicCurve4.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_CubicCurve5 — 三次曲线 5

![三次曲线 5](../Thumbs/NS_2DShapeTemplate_CubicCurve5.png)

类型**: 2D parametric curve
公式**: `float2(InputT * InputT + 2.0, InputT * InputT * InputT + 3.0 * InputT)`
说明**: Cubic curve, variant 5.
动态输入**: `/EasyNSShape/NDI/NDI_2D_CubicCurve5`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_CubicCurve5`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_CubicCurve5`
源函数**: `/EasyNSShape/NFS/NFS2D_CubicCurve5`
快照**: [NS_2DShapeTemplate_CubicCurve5.png](../Thumbs/NS_2DShapeTemplate_CubicCurve5.png)
视频**: [NS_2DShapeTemplate_CubicCurve5.mp4](../Videos/NS_2DShapeTemplate_CubicCurve5.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_CubicCurve6 — 三次曲线 6

![三次曲线 6](../Thumbs/NS_2DShapeTemplate_CubicCurve6.png)

类型**: 2D parametric curve
公式**: `float2(InputT * InputT * InputT - InputT, InputT * InputT + 1.0)`
说明**: Cubic curve, variant 6.
动态输入**: `/EasyNSShape/NDI/NDI_2D_CubicCurve6`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_CubicCurve6`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_CubicCurve6`
源函数**: `/EasyNSShape/NFS/NFS2D_CubicCurve6`
快照**: [NS_2DShapeTemplate_CubicCurve6.png](../Thumbs/NS_2DShapeTemplate_CubicCurve6.png)
视频**: [NS_2DShapeTemplate_CubicCurve6.mp4](../Videos/NS_2DShapeTemplate_CubicCurve6.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_CubicCurve7 — 三次曲线 7

![三次曲线 7](../Thumbs/NS_2DShapeTemplate_CubicCurve7.png)

类型**: 2D parametric curve
公式**: `float2(InputT * InputT, InputT * InputT * InputT + InputT * InputT)`
说明**: Cubic curve, variant 7.
动态输入**: `/EasyNSShape/NDI/NDI_2D_CubicCurve7`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_CubicCurve7`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_CubicCurve7`
源函数**: `/EasyNSShape/NFS/NFS2D_CubicCurve7`
快照**: [NS_2DShapeTemplate_CubicCurve7.png](../Thumbs/NS_2DShapeTemplate_CubicCurve7.png)
视频**: [NS_2DShapeTemplate_CubicCurve7.mp4](../Videos/NS_2DShapeTemplate_CubicCurve7.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_CubicCurve8 — 三次曲线 8

![三次曲线 8](../Thumbs/NS_2DShapeTemplate_CubicCurve8.png)

类型**: 2D parametric curve
公式**: `float2(InputT * InputT * InputT + InputT, InputT * InputT - InputT)`
说明**: Cubic curve, variant 8.
动态输入**: `/EasyNSShape/NDI/NDI_2D_CubicCurve8`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_CubicCurve8`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_CubicCurve8`
源函数**: `/EasyNSShape/NFS/NFS2D_CubicCurve8`
快照**: [NS_2DShapeTemplate_CubicCurve8.png](../Thumbs/NS_2DShapeTemplate_CubicCurve8.png)
视频**: [NS_2DShapeTemplate_CubicCurve8.mp4](../Videos/NS_2DShapeTemplate_CubicCurve8.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_CubicParabola — 三次抛物线

![三次抛物线](../Thumbs/NS_2DShapeTemplate_CubicParabola.png)

类型**: 2D parametric curve
公式**: `float2(InputT, InputT * InputT * InputT)`
说明**: Cubic parabola (t, t^3).
动态输入**: `/EasyNSShape/NDI/NDI_2D_CubicParabola`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_CubicParabola`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_CubicParabola`
源函数**: `/EasyNSShape/NFS/NFS2D_CubicParabola`
快照**: [NS_2DShapeTemplate_CubicParabola.png](../Thumbs/NS_2DShapeTemplate_CubicParabola.png)
视频**: [NS_2DShapeTemplate_CubicParabola.mp4](../Videos/NS_2DShapeTemplate_CubicParabola.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Cycloid — 摆线

![摆线](../Thumbs/NS_2DShapeTemplate_Cycloid.png)

类型**: 2D parametric curve
公式**: `float2(InputT-sin(InputT), 1.0-cos(InputT))`
说明**: Cycloid — path of a point on a rolling circle.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Cycloid`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Cycloid`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Cycloid`
源函数**: `/EasyNSShape/NFS/NFS2D_Cycloid`
快照**: [NS_2DShapeTemplate_Cycloid.png](../Thumbs/NS_2DShapeTemplate_Cycloid.png)
视频**: [NS_2DShapeTemplate_Cycloid.mp4](../Videos/NS_2DShapeTemplate_Cycloid.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Cycloid2 — 摆线 2

![摆线 2](../Thumbs/NS_2DShapeTemplate_Cycloid2.png)

类型**: 2D parametric curve
公式**: `float2(InputT+sin(InputT), 1.0+cos(InputT))`
说明**: Cycloid, variant with added sin/cos terms.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Cycloid2`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Cycloid2`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Cycloid2`
源函数**: `/EasyNSShape/NFS/NFS2D_Cycloid2`
快照**: [NS_2DShapeTemplate_Cycloid2.png](../Thumbs/NS_2DShapeTemplate_Cycloid2.png)
视频**: [NS_2DShapeTemplate_Cycloid2.mp4](../Videos/NS_2DShapeTemplate_Cycloid2.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_CycloidSwapped — 交换摆线

![交换摆线](../Thumbs/NS_2DShapeTemplate_CycloidSwapped.png)

类型**: 2D parametric curve
公式**: `float2(1.0-cos(InputT), InputT-sin(InputT))`
说明**: Cycloid with the axes swapped.
动态输入**: `/EasyNSShape/NDI/NDI_2D_CycloidSwapped`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_CycloidSwapped`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_CycloidSwapped`
源函数**: `/EasyNSShape/NFS/NFS2D_CycloidSwapped`
快照**: [NS_2DShapeTemplate_CycloidSwapped.png](../Thumbs/NS_2DShapeTemplate_CycloidSwapped.png)
视频**: [NS_2DShapeTemplate_CycloidSwapped.mp4](../Videos/NS_2DShapeTemplate_CycloidSwapped.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Deltoid — 三角星形线

![三角星形线](../Thumbs/NS_2DShapeTemplate_Deltoid.png)

类型**: 2D parametric curve
公式**: `float2(2.0*cos(InputT)-cos(2.0*InputT), 2.0*sin(InputT)-sin(2.0*InputT))`
说明**: Three-cusped hypocycloid (deltoid).
动态输入**: `/EasyNSShape/NDI/NDI_2D_Deltoid`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Deltoid`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Deltoid`
源函数**: `/EasyNSShape/NFS/NFS2D_Deltoid`
快照**: [NS_2DShapeTemplate_Deltoid.png](../Thumbs/NS_2DShapeTemplate_Deltoid.png)
视频**: [NS_2DShapeTemplate_Deltoid.mp4](../Videos/NS_2DShapeTemplate_Deltoid.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Ellipse — 椭圆

![椭圆](../Thumbs/NS_2DShapeTemplate_Ellipse.png)

类型**: 2D parametric curve
公式**: `float2(cos(InputT), sin(InputT))`
说明**: Ellipse (here an axis-aligned unit circle parameterisation).
动态输入**: `/EasyNSShape/NDI/NDI_2D_Ellipse`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Ellipse`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Ellipse`
源函数**: `/EasyNSShape/NFS/NFS2D_Ellipse`
快照**: [NS_2DShapeTemplate_Ellipse.png](../Thumbs/NS_2DShapeTemplate_Ellipse.png)
视频**: [NS_2DShapeTemplate_Ellipse.mp4](../Videos/NS_2DShapeTemplate_Ellipse.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Equidistant — 等距曲线

![等距曲线](../Thumbs/NS_2DShapeTemplate_Equidistant.png)

类型**: 2D parametric curve
公式**: `float2(InputT+cos(InputT), InputT+sin(InputT))`
说明**: Equidistant curve (t + cos t, t + sin t).
动态输入**: `/EasyNSShape/NDI/NDI_2D_Equidistant`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Equidistant`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Equidistant`
源函数**: `/EasyNSShape/NFS/NFS2D_Equidistant`
快照**: [NS_2DShapeTemplate_Equidistant.png](../Thumbs/NS_2DShapeTemplate_Equidistant.png)
视频**: [NS_2DShapeTemplate_Equidistant.mp4](../Videos/NS_2DShapeTemplate_Equidistant.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_ExpSquare — 指数平方曲线

![指数平方曲线](../Thumbs/NS_2DShapeTemplate_ExpSquare.png)

类型**: 2D parametric curve
公式**: `float2(exp(InputT), InputT*InputT)`
说明**: Exponential vs. square curve.
动态输入**: `/EasyNSShape/NDI/NDI_2D_ExpSquare`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_ExpSquare`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_ExpSquare`
源函数**: `/EasyNSShape/NFS/NFS2D_ExpSquare`
快照**: [NS_2DShapeTemplate_ExpSquare.png](../Thumbs/NS_2DShapeTemplate_ExpSquare.png)
视频**: [NS_2DShapeTemplate_ExpSquare.mp4](../Videos/NS_2DShapeTemplate_ExpSquare.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_FoliumDescartes — 笛卡尔叶形线

![笛卡尔叶形线](../Thumbs/NS_2DShapeTemplate_FoliumDescartes.png)

类型**: 2D parametric curve
公式**: `float2(3.0*InputT/(1.0+InputT*InputT*InputT), 3.0*InputT*InputT/(1.0+InputT*InputT*InputT))`
说明**: Folium of Descartes.
动态输入**: `/EasyNSShape/NDI/NDI_2D_FoliumDescartes`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_FoliumDescartes`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_FoliumDescartes`
源函数**: `/EasyNSShape/NFS/NFS2D_FoliumDescartes`
快照**: [NS_2DShapeTemplate_FoliumDescartes.png](../Thumbs/NS_2DShapeTemplate_FoliumDescartes.png)
视频**: [NS_2DShapeTemplate_FoliumDescartes.mp4](../Videos/NS_2DShapeTemplate_FoliumDescartes.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_HyperbolaCoshSinh — 双曲余弦-正弦曲线

![双曲余弦-正弦曲线](../Thumbs/NS_2DShapeTemplate_HyperbolaCoshSinh.png)

类型**: 2D parametric curve
公式**: `float2(cosh(InputT), sinh(InputT))`
说明**: Hyperbolic curve using cosh/sinh.
动态输入**: `/EasyNSShape/NDI/NDI_2D_HyperbolaCoshSinh`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_HyperbolaCoshSinh`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_HyperbolaCoshSinh`
源函数**: `/EasyNSShape/NFS/NFS2D_HyperbolaCoshSinh`
快照**: [NS_2DShapeTemplate_HyperbolaCoshSinh.png](../Thumbs/NS_2DShapeTemplate_HyperbolaCoshSinh.png)
视频**: [NS_2DShapeTemplate_HyperbolaCoshSinh.mp4](../Videos/NS_2DShapeTemplate_HyperbolaCoshSinh.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_HyperbolaExp — 指数双曲线

![指数双曲线](../Thumbs/NS_2DShapeTemplate_HyperbolaExp.png)

类型**: 2D parametric curve
公式**: `float2(exp(InputT), exp(-InputT))`
说明**: Hyperbola built from exp(t) and exp(-t).
动态输入**: `/EasyNSShape/NDI/NDI_2D_HyperbolaExp`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_HyperbolaExp`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_HyperbolaExp`
源函数**: `/EasyNSShape/NFS/NFS2D_HyperbolaExp`
快照**: [NS_2DShapeTemplate_HyperbolaExp.png](../Thumbs/NS_2DShapeTemplate_HyperbolaExp.png)
视频**: [NS_2DShapeTemplate_HyperbolaExp.mp4](../Videos/NS_2DShapeTemplate_HyperbolaExp.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_HyperbolaExp2 — 指数双曲线 2

![指数双曲线 2](../Thumbs/NS_2DShapeTemplate_HyperbolaExp2.png)

类型**: 2D parametric curve
公式**: `float2(exp(InputT * InputT), exp(-InputT * InputT))`
说明**: Hyperbola built from exp(t^2) and exp(-t^2).
动态输入**: `/EasyNSShape/NDI/NDI_2D_HyperbolaExp2`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_HyperbolaExp2`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_HyperbolaExp2`
源函数**: `/EasyNSShape/NFS/NFS2D_HyperbolaExp2`
快照**: [NS_2DShapeTemplate_HyperbolaExp2.png](../Thumbs/NS_2DShapeTemplate_HyperbolaExp2.png)
视频**: [NS_2DShapeTemplate_HyperbolaExp2.mp4](../Videos/NS_2DShapeTemplate_HyperbolaExp2.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_HyperbolaSecTan — 正割-正切双曲线

![正割-正切双曲线](../Thumbs/NS_2DShapeTemplate_HyperbolaSecTan.png)

类型**: 2D parametric curve
公式**: `float2(1.0/cos(InputT), tan(InputT))`
说明**: Hyperbolic curve using sec/tan.
动态输入**: `/EasyNSShape/NDI/NDI_2D_HyperbolaSecTan`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_HyperbolaSecTan`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_HyperbolaSecTan`
源函数**: `/EasyNSShape/NFS/NFS2D_HyperbolaSecTan`
快照**: [NS_2DShapeTemplate_HyperbolaSecTan.png](../Thumbs/NS_2DShapeTemplate_HyperbolaSecTan.png)
视频**: [NS_2DShapeTemplate_HyperbolaSecTan.mp4](../Videos/NS_2DShapeTemplate_HyperbolaSecTan.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_HyperbolaSinhCosh — 双曲正弦-余弦曲线

![双曲正弦-余弦曲线](../Thumbs/NS_2DShapeTemplate_HyperbolaSinhCosh.png)

类型**: 2D parametric curve
公式**: `float2(sinh(InputT), cosh(InputT))`
说明**: Hyperbolic curve using sinh/cosh.
动态输入**: `/EasyNSShape/NDI/NDI_2D_HyperbolaSinhCosh`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_HyperbolaSinhCosh`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_HyperbolaSinhCosh`
源函数**: `/EasyNSShape/NFS/NFS2D_HyperbolaSinhCosh`
快照**: [NS_2DShapeTemplate_HyperbolaSinhCosh.png](../Thumbs/NS_2DShapeTemplate_HyperbolaSinhCosh.png)
视频**: [NS_2DShapeTemplate_HyperbolaSinhCosh.mp4](../Videos/NS_2DShapeTemplate_HyperbolaSinhCosh.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_HyperbolaTanSec — 正切-正割双曲线

![正切-正割双曲线](../Thumbs/NS_2DShapeTemplate_HyperbolaTanSec.png)

类型**: 2D parametric curve
公式**: `float2(tan(InputT), 1.0 / cos(InputT))`
说明**: Hyperbolic curve using tan/sec.
动态输入**: `/EasyNSShape/NDI/NDI_2D_HyperbolaTanSec`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_HyperbolaTanSec`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_HyperbolaTanSec`
源函数**: `/EasyNSShape/NFS/NFS2D_HyperbolaTanSec`
快照**: [NS_2DShapeTemplate_HyperbolaTanSec.png](../Thumbs/NS_2DShapeTemplate_HyperbolaTanSec.png)
视频**: [NS_2DShapeTemplate_HyperbolaTanSec.mp4](../Videos/NS_2DShapeTemplate_HyperbolaTanSec.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_InvoluteCircle — 圆的渐开线

![圆的渐开线](../Thumbs/NS_2DShapeTemplate_InvoluteCircle.png)

类型**: 2D parametric curve
公式**: `float2(cos(InputT)+InputT*sin(InputT), sin(InputT)-InputT*cos(InputT))`
说明**: Involute of a circle.
动态输入**: `/EasyNSShape/NDI/NDI_2D_InvoluteCircle`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_InvoluteCircle`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_InvoluteCircle`
源函数**: `/EasyNSShape/NFS/NFS2D_InvoluteCircle`
快照**: [NS_2DShapeTemplate_InvoluteCircle.png](../Thumbs/NS_2DShapeTemplate_InvoluteCircle.png)
视频**: [NS_2DShapeTemplate_InvoluteCircle.mp4](../Videos/NS_2DShapeTemplate_InvoluteCircle.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_LineSegment — 线段

![线段](../Thumbs/NS_2DShapeTemplate_LineSegment.png)

类型**: 2D parametric curve
公式**: `float2(asin(InputT), acos(InputT))`
说明**: Line segment (asin/acos parameterisation).
动态输入**: `/EasyNSShape/NDI/NDI_2D_LineSegment`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_LineSegment`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_LineSegment`
源函数**: `/EasyNSShape/NFS/NFS2D_LineSegment`
快照**: [NS_2DShapeTemplate_LineSegment.png](../Thumbs/NS_2DShapeTemplate_LineSegment.png)
视频**: [NS_2DShapeTemplate_LineSegment.mp4](../Videos/NS_2DShapeTemplate_LineSegment.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Lissajous12 — 利萨茹曲线 1:2

![利萨茹曲线 1:2](../Thumbs/NS_2DShapeTemplate_Lissajous12.png)

类型**: 2D parametric curve
公式**: `float2(sin(InputT), sin(2.0*InputT))`
说明**: Lissajous figure, frequency ratio 1:2.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Lissajous12`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Lissajous12`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Lissajous12`
源函数**: `/EasyNSShape/NFS/NFS2D_Lissajous12`
快照**: [NS_2DShapeTemplate_Lissajous12.png](../Thumbs/NS_2DShapeTemplate_Lissajous12.png)
视频**: [NS_2DShapeTemplate_Lissajous12.mp4](../Videos/NS_2DShapeTemplate_Lissajous12.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Lissajous12Cos — 利萨茹曲线 1:2 (余弦)

![利萨茹曲线 1:2 (余弦)](../Thumbs/NS_2DShapeTemplate_Lissajous12Cos.png)

类型**: 2D parametric curve
公式**: `float2(sin(InputT), cos(2.0*InputT))`
说明**: Lissajous figure 1:2 using cosine.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Lissajous12Cos`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Lissajous12Cos`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Lissajous12Cos`
源函数**: `/EasyNSShape/NFS/NFS2D_Lissajous12Cos`
快照**: [NS_2DShapeTemplate_Lissajous12Cos.png](../Thumbs/NS_2DShapeTemplate_Lissajous12Cos.png)
视频**: [NS_2DShapeTemplate_Lissajous12Cos.mp4](../Videos/NS_2DShapeTemplate_Lissajous12Cos.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Lissajous13 — 利萨茹曲线 1:3

![利萨茹曲线 1:3](../Thumbs/NS_2DShapeTemplate_Lissajous13.png)

类型**: 2D parametric curve
公式**: `float2(sin(InputT), cos(3.0*InputT))`
说明**: Lissajous figure, frequency ratio 1:3.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Lissajous13`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Lissajous13`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Lissajous13`
源函数**: `/EasyNSShape/NFS/NFS2D_Lissajous13`
快照**: [NS_2DShapeTemplate_Lissajous13.png](../Thumbs/NS_2DShapeTemplate_Lissajous13.png)
视频**: [NS_2DShapeTemplate_Lissajous13.mp4](../Videos/NS_2DShapeTemplate_Lissajous13.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Lissajous21 — 利萨茹曲线 2:1

![利萨茹曲线 2:1](../Thumbs/NS_2DShapeTemplate_Lissajous21.png)

类型**: 2D parametric curve
公式**: `float2(sin(2.0*InputT), sin(InputT))`
说明**: Lissajous figure, frequency ratio 2:1.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Lissajous21`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Lissajous21`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Lissajous21`
源函数**: `/EasyNSShape/NFS/NFS2D_Lissajous21`
快照**: [NS_2DShapeTemplate_Lissajous21.png](../Thumbs/NS_2DShapeTemplate_Lissajous21.png)
视频**: [NS_2DShapeTemplate_Lissajous21.mp4](../Videos/NS_2DShapeTemplate_Lissajous21.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Lissajous23 — 利萨茹曲线 2:3

![利萨茹曲线 2:3](../Thumbs/NS_2DShapeTemplate_Lissajous23.png)

类型**: 2D parametric curve
公式**: `float2(cos(2.0*InputT), cos(3.0*InputT))`
说明**: Lissajous figure, frequency ratio 2:3.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Lissajous23`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Lissajous23`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Lissajous23`
源函数**: `/EasyNSShape/NFS/NFS2D_Lissajous23`
快照**: [NS_2DShapeTemplate_Lissajous23.png](../Thumbs/NS_2DShapeTemplate_Lissajous23.png)
视频**: [NS_2DShapeTemplate_Lissajous23.mp4](../Videos/NS_2DShapeTemplate_Lissajous23.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Lissajous23b — 利萨茹曲线 2:3 变体 B

![利萨茹曲线 2:3 变体 B](../Thumbs/NS_2DShapeTemplate_Lissajous23b.png)

类型**: 2D parametric curve
公式**: `float2(2.0*sin(InputT), sin(3.0*InputT))`
说明**: Lissajous figure 2:3, variant B.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Lissajous23b`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Lissajous23b`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Lissajous23b`
源函数**: `/EasyNSShape/NFS/NFS2D_Lissajous23b`
快照**: [NS_2DShapeTemplate_Lissajous23b.png](../Thumbs/NS_2DShapeTemplate_Lissajous23b.png)
视频**: [NS_2DShapeTemplate_Lissajous23b.mp4](../Videos/NS_2DShapeTemplate_Lissajous23b.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Lissajous23c — 利萨茹曲线 2:3 变体 C

![利萨茹曲线 2:3 变体 C](../Thumbs/NS_2DShapeTemplate_Lissajous23c.png)

类型**: 2D parametric curve
公式**: `float2(sin(2.0*InputT), sin(3.0*InputT))`
说明**: Lissajous figure 2:3, variant C.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Lissajous23c`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Lissajous23c`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Lissajous23c`
源函数**: `/EasyNSShape/NFS/NFS2D_Lissajous23c`
快照**: [NS_2DShapeTemplate_Lissajous23c.png](../Thumbs/NS_2DShapeTemplate_Lissajous23c.png)
视频**: [NS_2DShapeTemplate_Lissajous23c.mp4](../Videos/NS_2DShapeTemplate_Lissajous23c.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Lissajous23d — 利萨茹曲线 2:3 变体 D

![利萨茹曲线 2:3 变体 D](../Thumbs/NS_2DShapeTemplate_Lissajous23d.png)

类型**: 2D parametric curve
公式**: `float2(cos(2.0 * InputT), sin(3.0 * InputT))`
说明**: Lissajous figure 2:3, variant D.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Lissajous23d`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Lissajous23d`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Lissajous23d`
源函数**: `/EasyNSShape/NFS/NFS2D_Lissajous23d`
快照**: [NS_2DShapeTemplate_Lissajous23d.png](../Thumbs/NS_2DShapeTemplate_Lissajous23d.png)
视频**: [NS_2DShapeTemplate_Lissajous23d.mp4](../Videos/NS_2DShapeTemplate_Lissajous23d.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Lissajous32 — 利萨茹曲线 3:2

![利萨茹曲线 3:2](../Thumbs/NS_2DShapeTemplate_Lissajous32.png)

类型**: 2D parametric curve
公式**: `float2(sin(3.0*InputT), sin(2.0*InputT))`
说明**: Lissajous figure, frequency ratio 3:2.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Lissajous32`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Lissajous32`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Lissajous32`
源函数**: `/EasyNSShape/NFS/NFS2D_Lissajous32`
快照**: [NS_2DShapeTemplate_Lissajous32.png](../Thumbs/NS_2DShapeTemplate_Lissajous32.png)
视频**: [NS_2DShapeTemplate_Lissajous32.mp4](../Videos/NS_2DShapeTemplate_Lissajous32.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Lissajous34 — 利萨茹曲线 3:4

![利萨茹曲线 3:4](../Thumbs/NS_2DShapeTemplate_Lissajous34.png)

类型**: 2D parametric curve
公式**: `float2(cos(3.0 * InputT), sin(4.0 * InputT))`
说明**: Lissajous figure, frequency ratio 3:4.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Lissajous34`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Lissajous34`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Lissajous34`
源函数**: `/EasyNSShape/NFS/NFS2D_Lissajous34`
快照**: [NS_2DShapeTemplate_Lissajous34.png](../Thumbs/NS_2DShapeTemplate_Lissajous34.png)
视频**: [NS_2DShapeTemplate_Lissajous34.mp4](../Videos/NS_2DShapeTemplate_Lissajous34.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Lissajous45 — 利萨茹曲线 4:5

![利萨茹曲线 4:5](../Thumbs/NS_2DShapeTemplate_Lissajous45.png)

类型**: 2D parametric curve
公式**: `float2(cos(4.0*InputT), sin(5.0*InputT))`
说明**: Lissajous figure, frequency ratio 4:5.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Lissajous45`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Lissajous45`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Lissajous45`
源函数**: `/EasyNSShape/NFS/NFS2D_Lissajous45`
快照**: [NS_2DShapeTemplate_Lissajous45.png](../Thumbs/NS_2DShapeTemplate_Lissajous45.png)
视频**: [NS_2DShapeTemplate_Lissajous45.mp4](../Videos/NS_2DShapeTemplate_Lissajous45.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Lissajous57 — 利萨茹曲线 5:7

![利萨茹曲线 5:7](../Thumbs/NS_2DShapeTemplate_Lissajous57.png)

类型**: 2D parametric curve
公式**: `float2(sin(5.0 * InputT), cos(7.0 * InputT))`
说明**: Lissajous figure, frequency ratio 5:7.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Lissajous57`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Lissajous57`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Lissajous57`
源函数**: `/EasyNSShape/NFS/NFS2D_Lissajous57`
快照**: [NS_2DShapeTemplate_Lissajous57.png](../Thumbs/NS_2DShapeTemplate_Lissajous57.png)
视频**: [NS_2DShapeTemplate_Lissajous57.mp4](../Videos/NS_2DShapeTemplate_Lissajous57.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_LogarithmicSpiral — 对数螺线

![对数螺线](../Thumbs/NS_2DShapeTemplate_LogarithmicSpiral.png)

类型**: 2D parametric curve
公式**: `float2(exp(InputT) * cos(InputT), exp(InputT) * sin(InputT))`
说明**: Logarithmic (equiangular) spiral.
动态输入**: `/EasyNSShape/NDI/NDI_2D_LogarithmicSpiral`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_LogarithmicSpiral`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_LogarithmicSpiral`
源函数**: `/EasyNSShape/NFS/NFS2D_LogarithmicSpiral`
快照**: [NS_2DShapeTemplate_LogarithmicSpiral.png](../Thumbs/NS_2DShapeTemplate_LogarithmicSpiral.png)
视频**: [NS_2DShapeTemplate_LogarithmicSpiral.mp4](../Videos/NS_2DShapeTemplate_LogarithmicSpiral.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_LogCurve — 对数曲线

![对数曲线](../Thumbs/NS_2DShapeTemplate_LogCurve.png)

类型**: 2D parametric curve
公式**: `float2(log(InputT), InputT)`
说明**: Logarithmic curve (log t, t).
动态输入**: `/EasyNSShape/NDI/NDI_2D_LogCurve`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_LogCurve`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_LogCurve`
源函数**: `/EasyNSShape/NFS/NFS2D_LogCurve`
快照**: [NS_2DShapeTemplate_LogCurve.png](../Thumbs/NS_2DShapeTemplate_LogCurve.png)
视频**: [NS_2DShapeTemplate_LogCurve.mp4](../Videos/NS_2DShapeTemplate_LogCurve.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_LogParabola — 对数抛物线

![对数抛物线](../Thumbs/NS_2DShapeTemplate_LogParabola.png)

类型**: 2D parametric curve
公式**: `float2(InputT*InputT, log(InputT))`
说明**: Log vs. parabolic curve (t^2, log t).
动态输入**: `/EasyNSShape/NDI/NDI_2D_LogParabola`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_LogParabola`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_LogParabola`
源函数**: `/EasyNSShape/NFS/NFS2D_LogParabola`
快照**: [NS_2DShapeTemplate_LogParabola.png](../Thumbs/NS_2DShapeTemplate_LogParabola.png)
视频**: [NS_2DShapeTemplate_LogParabola.mp4](../Videos/NS_2DShapeTemplate_LogParabola.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_LogSecant — 对数正割曲线

![对数正割曲线](../Thumbs/NS_2DShapeTemplate_LogSecant.png)

类型**: 2D parametric curve
公式**: `float2(log(InputT * InputT + 1.0), atan(InputT))`
说明**: Log/arctangent curve.
动态输入**: `/EasyNSShape/NDI/NDI_2D_LogSecant`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_LogSecant`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_LogSecant`
源函数**: `/EasyNSShape/NFS/NFS2D_LogSecant`
快照**: [NS_2DShapeTemplate_LogSecant.png](../Thumbs/NS_2DShapeTemplate_LogSecant.png)
视频**: [NS_2DShapeTemplate_LogSecant.mp4](../Videos/NS_2DShapeTemplate_LogSecant.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Parabola — 抛物线

![抛物线](../Thumbs/NS_2DShapeTemplate_Parabola.png)

类型**: 2D parametric curve
公式**: `float2(InputT, InputT * InputT)`
说明**: Parabola (t, t^2).
动态输入**: `/EasyNSShape/NDI/NDI_2D_Parabola`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Parabola`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Parabola`
源函数**: `/EasyNSShape/NFS/NFS2D_Parabola`
快照**: [NS_2DShapeTemplate_Parabola.png](../Thumbs/NS_2DShapeTemplate_Parabola.png)
视频**: [NS_2DShapeTemplate_Parabola.mp4](../Videos/NS_2DShapeTemplate_Parabola.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_ParabolaHigh — 高次抛物线

![高次抛物线](../Thumbs/NS_2DShapeTemplate_ParabolaHigh.png)

类型**: 2D parametric curve
公式**: `float2(pow(InputT, 5.0), pow(InputT, 10.0))`
说明**: High-order parabola (t^5, t^10).
动态输入**: `/EasyNSShape/NDI/NDI_2D_ParabolaHigh`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_ParabolaHigh`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_ParabolaHigh`
源函数**: `/EasyNSShape/NFS/NFS2D_ParabolaHigh`
快照**: [NS_2DShapeTemplate_ParabolaHigh.png](../Thumbs/NS_2DShapeTemplate_ParabolaHigh.png)
视频**: [NS_2DShapeTemplate_ParabolaHigh.mp4](../Videos/NS_2DShapeTemplate_ParabolaHigh.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_ParabolaRational — 有理抛物线

![有理抛物线](../Thumbs/NS_2DShapeTemplate_ParabolaRational.png)

类型**: 2D parametric curve
公式**: `float2(InputT + 1.0 / InputT, InputT * InputT + 1.0 / (InputT * InputT))`
说明**: Rational parabola with 1/t terms.
动态输入**: `/EasyNSShape/NDI/NDI_2D_ParabolaRational`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_ParabolaRational`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_ParabolaRational`
源函数**: `/EasyNSShape/NFS/NFS2D_ParabolaRational`
快照**: [NS_2DShapeTemplate_ParabolaRational.png](../Thumbs/NS_2DShapeTemplate_ParabolaRational.png)
视频**: [NS_2DShapeTemplate_ParabolaRational.mp4](../Videos/NS_2DShapeTemplate_ParabolaRational.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_ParabolaShifted — 平移抛物线

![平移抛物线](../Thumbs/NS_2DShapeTemplate_ParabolaShifted.png)

类型**: 2D parametric curve
公式**: `float2(InputT + 1.0, (InputT + 1.0) * (InputT + 1.0))`
说明**: Parabola shifted by +1 on both axes.
动态输入**: `/EasyNSShape/NDI/NDI_2D_ParabolaShifted`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_ParabolaShifted`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_ParabolaShifted`
源函数**: `/EasyNSShape/NFS/NFS2D_ParabolaShifted`
快照**: [NS_2DShapeTemplate_ParabolaShifted.png](../Thumbs/NS_2DShapeTemplate_ParabolaShifted.png)
视频**: [NS_2DShapeTemplate_ParabolaShifted.mp4](../Videos/NS_2DShapeTemplate_ParabolaShifted.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_ParabolaV2 — 抛物线 V2

![抛物线 V2](../Thumbs/NS_2DShapeTemplate_ParabolaV2.png)

类型**: 2D parametric curve
公式**: `float2(InputT*InputT, InputT*InputT-2.0*InputT)`
说明**: Parabola, variant 2.
动态输入**: `/EasyNSShape/NDI/NDI_2D_ParabolaV2`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_ParabolaV2`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_ParabolaV2`
源函数**: `/EasyNSShape/NFS/NFS2D_ParabolaV2`
快照**: [NS_2DShapeTemplate_ParabolaV2.png](../Thumbs/NS_2DShapeTemplate_ParabolaV2.png)
视频**: [NS_2DShapeTemplate_ParabolaV2.mp4](../Videos/NS_2DShapeTemplate_ParabolaV2.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_ParabolaV3 — 抛物线 V3

![抛物线 V3](../Thumbs/NS_2DShapeTemplate_ParabolaV3.png)

类型**: 2D parametric curve
公式**: `float2(InputT, InputT*InputT-2.0*InputT)`
说明**: Parabola, variant 3.
动态输入**: `/EasyNSShape/NDI/NDI_2D_ParabolaV3`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_ParabolaV3`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_ParabolaV3`
源函数**: `/EasyNSShape/NFS/NFS2D_ParabolaV3`
快照**: [NS_2DShapeTemplate_ParabolaV3.png](../Thumbs/NS_2DShapeTemplate_ParabolaV3.png)
视频**: [NS_2DShapeTemplate_ParabolaV3.mp4](../Videos/NS_2DShapeTemplate_ParabolaV3.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_ParabolicArc — 抛物弧线

![抛物弧线](../Thumbs/NS_2DShapeTemplate_ParabolicArc.png)

类型**: 2D parametric curve
公式**: `float2(cos(InputT), cos(2.0*InputT))`
说明**: Parabolic arc (cos t, cos 2t).
动态输入**: `/EasyNSShape/NDI/NDI_2D_ParabolicArc`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_ParabolicArc`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_ParabolicArc`
源函数**: `/EasyNSShape/NFS/NFS2D_ParabolicArc`
快照**: [NS_2DShapeTemplate_ParabolicArc.png](../Thumbs/NS_2DShapeTemplate_ParabolicArc.png)
视频**: [NS_2DShapeTemplate_ParabolicArc.mp4](../Videos/NS_2DShapeTemplate_ParabolicArc.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_PowerCurve — 幂曲线

![幂曲线](../Thumbs/NS_2DShapeTemplate_PowerCurve.png)

类型**: 2D parametric curve
公式**: `float2(sqrt(InputT), InputT*InputT)`
说明**: Power curve (sqrt t, t^2).
动态输入**: `/EasyNSShape/NDI/NDI_2D_PowerCurve`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_PowerCurve`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_PowerCurve`
源函数**: `/EasyNSShape/NFS/NFS2D_PowerCurve`
快照**: [NS_2DShapeTemplate_PowerCurve.png](../Thumbs/NS_2DShapeTemplate_PowerCurve.png)
视频**: [NS_2DShapeTemplate_PowerCurve.mp4](../Videos/NS_2DShapeTemplate_PowerCurve.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_PowerCurve2 — 幂曲线 2

![幂曲线 2](../Thumbs/NS_2DShapeTemplate_PowerCurve2.png)

类型**: 2D parametric curve
公式**: `float2(InputT*InputT*InputT, sqrt(InputT))`
说明**: Power curve (t^3, sqrt t).
动态输入**: `/EasyNSShape/NDI/NDI_2D_PowerCurve2`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_PowerCurve2`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_PowerCurve2`
源函数**: `/EasyNSShape/NFS/NFS2D_PowerCurve2`
快照**: [NS_2DShapeTemplate_PowerCurve2.png](../Thumbs/NS_2DShapeTemplate_PowerCurve2.png)
视频**: [NS_2DShapeTemplate_PowerCurve2.mp4](../Videos/NS_2DShapeTemplate_PowerCurve2.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_PowerCurve3 — 幂曲线 3

![幂曲线 3](../Thumbs/NS_2DShapeTemplate_PowerCurve3.png)

类型**: 2D parametric curve
公式**: `float2(pow(InputT, 4.0), pow(InputT, 6.0))`
说明**: Power curve (t^4, t^6).
动态输入**: `/EasyNSShape/NDI/NDI_2D_PowerCurve3`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_PowerCurve3`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_PowerCurve3`
源函数**: `/EasyNSShape/NFS/NFS2D_PowerCurve3`
快照**: [NS_2DShapeTemplate_PowerCurve3.png](../Thumbs/NS_2DShapeTemplate_PowerCurve3.png)
视频**: [NS_2DShapeTemplate_PowerCurve3.mp4](../Videos/NS_2DShapeTemplate_PowerCurve3.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_QuarticCurve — 四次曲线

![四次曲线](../Thumbs/NS_2DShapeTemplate_QuarticCurve.png)

类型**: 2D parametric curve
公式**: `float2(InputT * InputT * InputT - 3.0 * InputT, pow(InputT, 4.0) - 2.0 * InputT * InputT)`
说明**: Quartic curve.
动态输入**: `/EasyNSShape/NDI/NDI_2D_QuarticCurve`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_QuarticCurve`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_QuarticCurve`
源函数**: `/EasyNSShape/NFS/NFS2D_QuarticCurve`
快照**: [NS_2DShapeTemplate_QuarticCurve.png](../Thumbs/NS_2DShapeTemplate_QuarticCurve.png)
视频**: [NS_2DShapeTemplate_QuarticCurve.mp4](../Videos/NS_2DShapeTemplate_QuarticCurve.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_QuarticCurve2 — 四次曲线 2

![四次曲线 2](../Thumbs/NS_2DShapeTemplate_QuarticCurve2.png)

类型**: 2D parametric curve
公式**: `float2(pow(InputT, 4.0) - InputT * InputT, InputT * InputT * InputT)`
说明**: Quartic curve, variant 2.
动态输入**: `/EasyNSShape/NDI/NDI_2D_QuarticCurve2`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_QuarticCurve2`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_QuarticCurve2`
源函数**: `/EasyNSShape/NFS/NFS2D_QuarticCurve2`
快照**: [NS_2DShapeTemplate_QuarticCurve2.png](../Thumbs/NS_2DShapeTemplate_QuarticCurve2.png)
视频**: [NS_2DShapeTemplate_QuarticCurve2.mp4](../Videos/NS_2DShapeTemplate_QuarticCurve2.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_QuarticParabola — 四次抛物线

![四次抛物线](../Thumbs/NS_2DShapeTemplate_QuarticParabola.png)

类型**: 2D parametric curve
公式**: `float2(InputT * InputT, pow(InputT, 4.0))`
说明**: Quartic parabola (t^2, t^4).
动态输入**: `/EasyNSShape/NDI/NDI_2D_QuarticParabola`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_QuarticParabola`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_QuarticParabola`
源函数**: `/EasyNSShape/NFS/NFS2D_QuarticParabola`
快照**: [NS_2DShapeTemplate_QuarticParabola.png](../Thumbs/NS_2DShapeTemplate_QuarticParabola.png)
视频**: [NS_2DShapeTemplate_QuarticParabola.mp4](../Videos/NS_2DShapeTemplate_QuarticParabola.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_RationalCurve — 有理曲线

![有理曲线](../Thumbs/NS_2DShapeTemplate_RationalCurve.png)

类型**: 2D parametric curve
公式**: `float2(InputT/(1.0+InputT*InputT), InputT*InputT/(1.0+InputT*InputT))`
说明**: Rational curve.
动态输入**: `/EasyNSShape/NDI/NDI_2D_RationalCurve`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_RationalCurve`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_RationalCurve`
源函数**: `/EasyNSShape/NFS/NFS2D_RationalCurve`
快照**: [NS_2DShapeTemplate_RationalCurve.png](../Thumbs/NS_2DShapeTemplate_RationalCurve.png)
视频**: [NS_2DShapeTemplate_RationalCurve.mp4](../Videos/NS_2DShapeTemplate_RationalCurve.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_RationalCurve2 — 有理曲线 2

![有理曲线 2](../Thumbs/NS_2DShapeTemplate_RationalCurve2.png)

类型**: 2D parametric curve
公式**: `float2(InputT*InputT/(1.0+InputT*InputT), InputT/(1.0+InputT*InputT))`
说明**: Rational curve, variant 2.
动态输入**: `/EasyNSShape/NDI/NDI_2D_RationalCurve2`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_RationalCurve2`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_RationalCurve2`
源函数**: `/EasyNSShape/NFS/NFS2D_RationalCurve2`
快照**: [NS_2DShapeTemplate_RationalCurve2.png](../Thumbs/NS_2DShapeTemplate_RationalCurve2.png)
视频**: [NS_2DShapeTemplate_RationalCurve2.mp4](../Videos/NS_2DShapeTemplate_RationalCurve2.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_RationalCurve3 — 有理曲线 3

![有理曲线 3](../Thumbs/NS_2DShapeTemplate_RationalCurve3.png)

类型**: 2D parametric curve
公式**: `float2(InputT*InputT+1.0/InputT, InputT*InputT-1.0/InputT)`
说明**: Rational curve, variant 3.
动态输入**: `/EasyNSShape/NDI/NDI_2D_RationalCurve3`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_RationalCurve3`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_RationalCurve3`
源函数**: `/EasyNSShape/NFS/NFS2D_RationalCurve3`
快照**: [NS_2DShapeTemplate_RationalCurve3.png](../Thumbs/NS_2DShapeTemplate_RationalCurve3.png)
视频**: [NS_2DShapeTemplate_RationalCurve3.mp4](../Videos/NS_2DShapeTemplate_RationalCurve3.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_RationalCurve4 — 有理曲线 4

![有理曲线 4](../Thumbs/NS_2DShapeTemplate_RationalCurve4.png)

类型**: 2D parametric curve
公式**: `float2(InputT*InputT/(1.0+InputT*InputT), InputT*InputT*InputT/(1.0+InputT*InputT))`
说明**: Rational curve, variant 4.
动态输入**: `/EasyNSShape/NDI/NDI_2D_RationalCurve4`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_RationalCurve4`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_RationalCurve4`
源函数**: `/EasyNSShape/NFS/NFS2D_RationalCurve4`
快照**: [NS_2DShapeTemplate_RationalCurve4.png](../Thumbs/NS_2DShapeTemplate_RationalCurve4.png)
视频**: [NS_2DShapeTemplate_RationalCurve4.mp4](../Videos/NS_2DShapeTemplate_RationalCurve4.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_RationalCurve5 — 有理曲线 5

![有理曲线 5](../Thumbs/NS_2DShapeTemplate_RationalCurve5.png)

类型**: 2D parametric curve
公式**: `float2((InputT*InputT*InputT-1.0)/(InputT*InputT+1.0), (InputT*InputT*InputT+1.0)/(InputT*InputT+1.0))`
说明**: Rational curve, variant 5.
动态输入**: `/EasyNSShape/NDI/NDI_2D_RationalCurve5`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_RationalCurve5`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_RationalCurve5`
源函数**: `/EasyNSShape/NFS/NFS2D_RationalCurve5`
快照**: [NS_2DShapeTemplate_RationalCurve5.png](../Thumbs/NS_2DShapeTemplate_RationalCurve5.png)
视频**: [NS_2DShapeTemplate_RationalCurve5.mp4](../Videos/NS_2DShapeTemplate_RationalCurve5.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_RationalCurve6 — 有理曲线 6

![有理曲线 6](../Thumbs/NS_2DShapeTemplate_RationalCurve6.png)

类型**: 2D parametric curve
公式**: `float2(InputT * InputT + 1.0 / (InputT * InputT), InputT * InputT * InputT + 1.0 / (InputT * InputT * InputT))`
说明**: Rational curve, variant 6.
动态输入**: `/EasyNSShape/NDI/NDI_2D_RationalCurve6`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_RationalCurve6`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_RationalCurve6`
源函数**: `/EasyNSShape/NFS/NFS2D_RationalCurve6`
快照**: [NS_2DShapeTemplate_RationalCurve6.png](../Thumbs/NS_2DShapeTemplate_RationalCurve6.png)
视频**: [NS_2DShapeTemplate_RationalCurve6.mp4](../Videos/NS_2DShapeTemplate_RationalCurve6.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_RectangularHyperbola — 直角双曲线

![直角双曲线](../Thumbs/NS_2DShapeTemplate_RectangularHyperbola.png)

类型**: 2D parametric curve
公式**: `float2(InputT, 1.0/InputT)`
说明**: Rectangular hyperbola (t, 1/t).
动态输入**: `/EasyNSShape/NDI/NDI_2D_RectangularHyperbola`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_RectangularHyperbola`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_RectangularHyperbola`
源函数**: `/EasyNSShape/NFS/NFS2D_RectangularHyperbola`
快照**: [NS_2DShapeTemplate_RectangularHyperbola.png](../Thumbs/NS_2DShapeTemplate_RectangularHyperbola.png)
视频**: [NS_2DShapeTemplate_RectangularHyperbola.mp4](../Videos/NS_2DShapeTemplate_RectangularHyperbola.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_RectHyperbola2 — 直角双曲线 2

![直角双曲线 2](../Thumbs/NS_2DShapeTemplate_RectHyperbola2.png)

类型**: 2D parametric curve
公式**: `float2(InputT+1.0/InputT, InputT-1.0/InputT)`
说明**: Rectangular hyperbola, variant 2.
动态输入**: `/EasyNSShape/NDI/NDI_2D_RectHyperbola2`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_RectHyperbola2`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_RectHyperbola2`
源函数**: `/EasyNSShape/NFS/NFS2D_RectHyperbola2`
快照**: [NS_2DShapeTemplate_RectHyperbola2.png](../Thumbs/NS_2DShapeTemplate_RectHyperbola2.png)
视频**: [NS_2DShapeTemplate_RectHyperbola2.mp4](../Videos/NS_2DShapeTemplate_RectHyperbola2.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_RectHyperbola3 — 直角双曲线 3

![直角双曲线 3](../Thumbs/NS_2DShapeTemplate_RectHyperbola3.png)

类型**: 2D parametric curve
公式**: `float2((InputT * InputT + 1.0) / InputT, (InputT * InputT - 1.0) / InputT)`
说明**: Rectangular hyperbola, variant 3.
动态输入**: `/EasyNSShape/NDI/NDI_2D_RectHyperbola3`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_RectHyperbola3`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_RectHyperbola3`
源函数**: `/EasyNSShape/NFS/NFS2D_RectHyperbola3`
快照**: [NS_2DShapeTemplate_RectHyperbola3.png](../Thumbs/NS_2DShapeTemplate_RectHyperbola3.png)
视频**: [NS_2DShapeTemplate_RectHyperbola3.mp4](../Videos/NS_2DShapeTemplate_RectHyperbola3.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_RotatedEllipse — 旋转椭圆

![旋转椭圆](../Thumbs/NS_2DShapeTemplate_RotatedEllipse.png)

类型**: 2D parametric curve
公式**: `float2(sin(InputT), sin(InputT + 1.0472))`
说明**: Rotated ellipse (sin t, sin(t + 60°)).
动态输入**: `/EasyNSShape/NDI/NDI_2D_RotatedEllipse`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_RotatedEllipse`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_RotatedEllipse`
源函数**: `/EasyNSShape/NFS/NFS2D_RotatedEllipse`
快照**: [NS_2DShapeTemplate_RotatedEllipse.png](../Thumbs/NS_2DShapeTemplate_RotatedEllipse.png)
视频**: [NS_2DShapeTemplate_RotatedEllipse.mp4](../Videos/NS_2DShapeTemplate_RotatedEllipse.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_SemicubicalParabola — 半立方抛物线

![半立方抛物线](../Thumbs/NS_2DShapeTemplate_SemicubicalParabola.png)

类型**: 2D parametric curve
公式**: `float2(InputT * InputT, InputT * InputT * InputT)`
说明**: Semicubical parabola (t^2, t^3).
动态输入**: `/EasyNSShape/NDI/NDI_2D_SemicubicalParabola`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_SemicubicalParabola`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_SemicubicalParabola`
源函数**: `/EasyNSShape/NFS/NFS2D_SemicubicalParabola`
快照**: [NS_2DShapeTemplate_SemicubicalParabola.png](../Thumbs/NS_2DShapeTemplate_SemicubicalParabola.png)
视频**: [NS_2DShapeTemplate_SemicubicalParabola.mp4](../Videos/NS_2DShapeTemplate_SemicubicalParabola.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_SemicubicalParabola2 — 半立方抛物线 2

![半立方抛物线 2](../Thumbs/NS_2DShapeTemplate_SemicubicalParabola2.png)

类型**: 2D parametric curve
公式**: `float2(InputT*InputT*InputT, InputT*InputT)`
说明**: Semicubical parabola, axes swapped.
动态输入**: `/EasyNSShape/NDI/NDI_2D_SemicubicalParabola2`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_SemicubicalParabola2`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_SemicubicalParabola2`
源函数**: `/EasyNSShape/NFS/NFS2D_SemicubicalParabola2`
快照**: [NS_2DShapeTemplate_SemicubicalParabola2.png](../Thumbs/NS_2DShapeTemplate_SemicubicalParabola2.png)
视频**: [NS_2DShapeTemplate_SemicubicalParabola2.mp4](../Videos/NS_2DShapeTemplate_SemicubicalParabola2.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_SineSquared — 正弦平方曲线

![正弦平方曲线](../Thumbs/NS_2DShapeTemplate_SineSquared.png)

类型**: 2D parametric curve
公式**: `float2(cos(InputT), sin(InputT)*sin(InputT))`
说明**: Sine-squared curve (cos t, sin²t).
动态输入**: `/EasyNSShape/NDI/NDI_2D_SineSquared`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_SineSquared`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_SineSquared`
源函数**: `/EasyNSShape/NFS/NFS2D_SineSquared`
快照**: [NS_2DShapeTemplate_SineSquared.png](../Thumbs/NS_2DShapeTemplate_SineSquared.png)
视频**: [NS_2DShapeTemplate_SineSquared.mp4](../Videos/NS_2DShapeTemplate_SineSquared.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_SpiralCurve — 螺线曲线

![螺线曲线](../Thumbs/NS_2DShapeTemplate_SpiralCurve.png)

类型**: 2D parametric curve
公式**: `float2(cos(3.14159 * InputT), sin(3.14159 * InputT * InputT))`
说明**: Spiral curve (cos πt, sin πt²).
动态输入**: `/EasyNSShape/NDI/NDI_2D_SpiralCurve`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_SpiralCurve`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_SpiralCurve`
源函数**: `/EasyNSShape/NFS/NFS2D_SpiralCurve`
快照**: [NS_2DShapeTemplate_SpiralCurve.png](../Thumbs/NS_2DShapeTemplate_SpiralCurve.png)
视频**: [NS_2DShapeTemplate_SpiralCurve.mp4](../Videos/NS_2DShapeTemplate_SpiralCurve.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Tractrix — 曳物线

![曳物线](../Thumbs/NS_2DShapeTemplate_Tractrix.png)

类型**: 2D parametric curve
公式**: `float2(cos(InputT)+log(tan(InputT*0.5)), sin(InputT))`
说明**: Tractrix (pursuit curve).
动态输入**: `/EasyNSShape/NDI/NDI_2D_Tractrix`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Tractrix`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Tractrix`
源函数**: `/EasyNSShape/NFS/NFS2D_Tractrix`
快照**: [NS_2DShapeTemplate_Tractrix.png](../Thumbs/NS_2DShapeTemplate_Tractrix.png)
视频**: [NS_2DShapeTemplate_Tractrix.mp4](../Videos/NS_2DShapeTemplate_Tractrix.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Tractrix2 — 曳物线 2

![曳物线 2](../Thumbs/NS_2DShapeTemplate_Tractrix2.png)

类型**: 2D parametric curve
公式**: `float2(1.0/cosh(InputT), InputT-tanh(InputT))`
说明**: Tractrix, variant 2.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Tractrix2`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Tractrix2`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Tractrix2`
源函数**: `/EasyNSShape/NFS/NFS2D_Tractrix2`
快照**: [NS_2DShapeTemplate_Tractrix2.png](../Thumbs/NS_2DShapeTemplate_Tractrix2.png)
视频**: [NS_2DShapeTemplate_Tractrix2.mp4](../Videos/NS_2DShapeTemplate_Tractrix2.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Tractrix3 — 曳物线 3

![曳物线 3](../Thumbs/NS_2DShapeTemplate_Tractrix3.png)

类型**: 2D parametric curve
公式**: `float2(InputT - tanh(InputT), 1.0 / cosh(InputT))`
说明**: Tractrix, variant 3.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Tractrix3`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Tractrix3`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Tractrix3`
源函数**: `/EasyNSShape/NFS/NFS2D_Tractrix3`
快照**: [NS_2DShapeTemplate_Tractrix3.png](../Thumbs/NS_2DShapeTemplate_Tractrix3.png)
视频**: [NS_2DShapeTemplate_Tractrix3.mp4](../Videos/NS_2DShapeTemplate_Tractrix3.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Trochoid — 次摆线

![次摆线](../Thumbs/NS_2DShapeTemplate_Trochoid.png)

类型**: 2D parametric curve
公式**: `float2(InputT+sin(InputT), 1.0+cos(InputT))`
说明**: Trochoid (extended cycloid).
动态输入**: `/EasyNSShape/NDI/NDI_2D_Trochoid`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Trochoid`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Trochoid`
源函数**: `/EasyNSShape/NFS/NFS2D_Trochoid`
快照**: [NS_2DShapeTemplate_Trochoid.png](../Thumbs/NS_2DShapeTemplate_Trochoid.png)
视频**: [NS_2DShapeTemplate_Trochoid.mp4](../Videos/NS_2DShapeTemplate_Trochoid.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_Trochoid2 — 次摆线 2

![次摆线 2](../Thumbs/NS_2DShapeTemplate_Trochoid2.png)

类型**: 2D parametric curve
公式**: `float2(2.0*InputT-sin(InputT), 2.0-cos(InputT))`
说明**: Trochoid, variant 2.
动态输入**: `/EasyNSShape/NDI/NDI_2D_Trochoid2`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Trochoid2`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_Trochoid2`
源函数**: `/EasyNSShape/NFS/NFS2D_Trochoid2`
快照**: [NS_2DShapeTemplate_Trochoid2.png](../Thumbs/NS_2DShapeTemplate_Trochoid2.png)
视频**: [NS_2DShapeTemplate_Trochoid2.mp4](../Videos/NS_2DShapeTemplate_Trochoid2.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_UnitCircle — 单位圆

![单位圆](../Thumbs/NS_2DShapeTemplate_UnitCircle.png)

类型**: 2D parametric curve
公式**: `float2((1.0-InputT*InputT)/(1.0+InputT*InputT), 2.0*InputT/(1.0+InputT*InputT))`
说明**: Rational unit circle.
动态输入**: `/EasyNSShape/NDI/NDI_2D_UnitCircle`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_UnitCircle`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_UnitCircle`
源函数**: `/EasyNSShape/NFS/NFS2D_UnitCircle`
快照**: [NS_2DShapeTemplate_UnitCircle.png](../Thumbs/NS_2DShapeTemplate_UnitCircle.png)
视频**: [NS_2DShapeTemplate_UnitCircle.mp4](../Videos/NS_2DShapeTemplate_UnitCircle.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_UnitCircle3 — 单位圆 3

![单位圆 3](../Thumbs/NS_2DShapeTemplate_UnitCircle3.png)

类型**: 2D parametric curve
公式**: `float2((InputT * InputT - 1.0) / (InputT * InputT + 1.0), 2.0 * InputT / (InputT * InputT + 1.0))`
说明**: Rational unit circle, variant 3.
动态输入**: `/EasyNSShape/NDI/NDI_2D_UnitCircle3`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_UnitCircle3`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_UnitCircle3`
源函数**: `/EasyNSShape/NFS/NFS2D_UnitCircle3`
快照**: [NS_2DShapeTemplate_UnitCircle3.png](../Thumbs/NS_2DShapeTemplate_UnitCircle3.png)
视频**: [NS_2DShapeTemplate_UnitCircle3.mp4](../Videos/NS_2DShapeTemplate_UnitCircle3.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_UnitCircle4 — 单位圆 4

![单位圆 4](../Thumbs/NS_2DShapeTemplate_UnitCircle4.png)

类型**: 2D parametric curve
公式**: `float2(2.0 * InputT / (InputT * InputT + 1.0), (InputT * InputT - 1.0) / (InputT * InputT + 1.0))`
说明**: Rational unit circle, variant 4.
动态输入**: `/EasyNSShape/NDI/NDI_2D_UnitCircle4`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_UnitCircle4`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_UnitCircle4`
源函数**: `/EasyNSShape/NFS/NFS2D_UnitCircle4`
快照**: [NS_2DShapeTemplate_UnitCircle4.png](../Thumbs/NS_2DShapeTemplate_UnitCircle4.png)
视频**: [NS_2DShapeTemplate_UnitCircle4.mp4](../Videos/NS_2DShapeTemplate_UnitCircle4.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_UnitCircleHyperbolic — 双曲单位圆

![双曲单位圆](../Thumbs/NS_2DShapeTemplate_UnitCircleHyperbolic.png)

类型**: 2D parametric curve
公式**: `float2(tanh(InputT), 1.0 / cosh(InputT))`
说明**: Hyperbolic unit circle (tanh, sech).
动态输入**: `/EasyNSShape/NDI/NDI_2D_UnitCircleHyperbolic`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_UnitCircleHyperbolic`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_UnitCircleHyperbolic`
源函数**: `/EasyNSShape/NFS/NFS2D_UnitCircleHyperbolic`
快照**: [NS_2DShapeTemplate_UnitCircleHyperbolic.png](../Thumbs/NS_2DShapeTemplate_UnitCircleHyperbolic.png)
视频**: [NS_2DShapeTemplate_UnitCircleHyperbolic.mp4](../Videos/NS_2DShapeTemplate_UnitCircleHyperbolic.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_WitchAgnesi — 阿涅西箕舌线

![阿涅西箕舌线](../Thumbs/NS_2DShapeTemplate_WitchAgnesi.png)

类型**: 2D parametric curve
公式**: `float2(InputT, 1.0 / (1.0 + InputT * InputT))`
说明**: Witch of Agnesi.
动态输入**: `/EasyNSShape/NDI/NDI_2D_WitchAgnesi`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_WitchAgnesi`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_WitchAgnesi`
源函数**: `/EasyNSShape/NFS/NFS2D_WitchAgnesi`
快照**: [NS_2DShapeTemplate_WitchAgnesi.png](../Thumbs/NS_2DShapeTemplate_WitchAgnesi.png)
视频**: [NS_2DShapeTemplate_WitchAgnesi.mp4](../Videos/NS_2DShapeTemplate_WitchAgnesi.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

### NDI_2D_WitchAgnesiVariant — 阿涅西箕舌线变体

![阿涅西箕舌线变体](../Thumbs/NS_2DShapeTemplate_WitchAgnesiVariant.png)

类型**: 2D parametric curve
公式**: `float2(InputT / (InputT * InputT + 1.0), (InputT * InputT - 1.0) / (InputT * InputT + 1.0))`
说明**: Witch of Agnesi, variant.
动态输入**: `/EasyNSShape/NDI/NDI_2D_WitchAgnesiVariant`
发射器**: `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_WitchAgnesiVariant`
系统**: `/EasyNSShape/NEBridge/NS/NS_2D_WitchAgnesiVariant`
源函数**: `/EasyNSShape/NFS/NFS2D_WitchAgnesiVariant`
快照**: [NS_2DShapeTemplate_WitchAgnesiVariant.png](../Thumbs/NS_2DShapeTemplate_WitchAgnesiVariant.png)
视频**: [NS_2DShapeTemplate_WitchAgnesiVariant.mp4](../Videos/NS_2DShapeTemplate_WitchAgnesiVariant.mp4)
输入**: `Module.InputT` (float)
输出**: `float2`

## 3D 形状详情 (27)

### NDI_3D_AstroidExtrude — 星形线拉伸体

![星形线拉伸体](../Thumbs/NS_3DShapeTemplate_AstroidExtrude.png)

类型**: 3D parametric curve
公式**: `float3(pow(cos(InputT),3.0), pow(sin(InputT),3.0), InputT)`
说明**: Astroid extruded along Z.
动态输入**: `/EasyNSShape/NDI/NDI_3D_AstroidExtrude`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_AstroidExtrude`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_AstroidExtrude`
源函数**: `/EasyNSShape/NFS/NFS3D_AstroidExtrude`
快照**: [NS_3DShapeTemplate_AstroidExtrude.png](../Thumbs/NS_3DShapeTemplate_AstroidExtrude.png)
视频**: [NS_3DShapeTemplate_AstroidExtrude.mp4](../Videos/NS_3DShapeTemplate_AstroidExtrude.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_CircularHelix — 圆柱螺旋线

![圆柱螺旋线](../Thumbs/NS_3DShapeTemplate_CircularHelix.png)

类型**: 3D parametric curve
公式**: `float3(cos(InputT), sin(InputT), InputT)`
说明**: Circular helix (constant radius).
动态输入**: `/EasyNSShape/NDI/NDI_3D_CircularHelix`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_CircularHelix`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_CircularHelix`
源函数**: `/EasyNSShape/NFS/NFS3D_CircularHelix`
快照**: [NS_3DShapeTemplate_CircularHelix.png](../Thumbs/NS_3DShapeTemplate_CircularHelix.png)
视频**: [NS_3DShapeTemplate_CircularHelix.mp4](../Videos/NS_3DShapeTemplate_CircularHelix.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_CliffordTorusKnot — 克利福德环面纽结

![克利福德环面纽结](../Thumbs/NS_3DShapeTemplate_CliffordTorusKnot.png)

类型**: 3D parametric curve
公式**: `float3((2.0+cos(2.0*InputT))*cos(3.0*InputT), (2.0+cos(2.0*InputT))*sin(3.0*InputT), sin(2.0*InputT))`
说明**: Clifford torus knot.
动态输入**: `/EasyNSShape/NDI/NDI_3D_CliffordTorusKnot`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_CliffordTorusKnot`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_CliffordTorusKnot`
源函数**: `/EasyNSShape/NFS/NFS3D_CliffordTorusKnot`
快照**: [NS_3DShapeTemplate_CliffordTorusKnot.png](../Thumbs/NS_3DShapeTemplate_CliffordTorusKnot.png)
视频**: [NS_3DShapeTemplate_CliffordTorusKnot.mp4](../Videos/NS_3DShapeTemplate_CliffordTorusKnot.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_ConicalHelix — 圆锥螺旋线

![圆锥螺旋线](../Thumbs/NS_3DShapeTemplate_ConicalHelix.png)

类型**: 3D parametric curve
公式**: `float3(InputT * cos(InputT), InputT * sin(InputT), InputT)`
说明**: Conical helix — radius grows with t.
动态输入**: `/EasyNSShape/NDI/NDI_3D_ConicalHelix`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_ConicalHelix`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_ConicalHelix`
源函数**: `/EasyNSShape/NFS/NFS3D_ConicalHelix`
快照**: [NS_3DShapeTemplate_ConicalHelix.png](../Thumbs/NS_3DShapeTemplate_ConicalHelix.png)
视频**: [NS_3DShapeTemplate_ConicalHelix.mp4](../Videos/NS_3DShapeTemplate_ConicalHelix.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_CubicHelix — 三次螺旋线

![三次螺旋线](../Thumbs/NS_3DShapeTemplate_CubicHelix.png)

类型**: 3D parametric curve
公式**: `float3(InputT, InputT*InputT, InputT*InputT*InputT)`
说明**: Cubic helix (t, t², t³).
动态输入**: `/EasyNSShape/NDI/NDI_3D_CubicHelix`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_CubicHelix`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_CubicHelix`
源函数**: `/EasyNSShape/NFS/NFS3D_CubicHelix`
快照**: [NS_3DShapeTemplate_CubicHelix.png](../Thumbs/NS_3DShapeTemplate_CubicHelix.png)
视频**: [NS_3DShapeTemplate_CubicHelix.mp4](../Videos/NS_3DShapeTemplate_CubicHelix.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_CycloidalHelix — 摆线螺旋

![摆线螺旋](../Thumbs/NS_3DShapeTemplate_CycloidalHelix.png)

类型**: 3D parametric curve
公式**: `float3(cos(InputT)*sin(InputT), sin(InputT)*sin(InputT), cos(InputT))`
说明**: Cycloidal helix.
动态输入**: `/EasyNSShape/NDI/NDI_3D_CycloidalHelix`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_CycloidalHelix`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_CycloidalHelix`
源函数**: `/EasyNSShape/NFS/NFS3D_CycloidalHelix`
快照**: [NS_3DShapeTemplate_CycloidalHelix.png](../Thumbs/NS_3DShapeTemplate_CycloidalHelix.png)
视频**: [NS_3DShapeTemplate_CycloidalHelix.mp4](../Videos/NS_3DShapeTemplate_CycloidalHelix.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_EggHelix — 蛋形螺旋线

![蛋形螺旋线](../Thumbs/NS_3DShapeTemplate_EggHelix.png)

类型**: 3D parametric curve
公式**: `float3(cos(InputT)*cos(InputT), cos(InputT)*sin(InputT), sin(InputT))`
说明**: Egg-shaped helix.
动态输入**: `/EasyNSShape/NDI/NDI_3D_EggHelix`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_EggHelix`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_EggHelix`
源函数**: `/EasyNSShape/NFS/NFS3D_EggHelix`
快照**: [NS_3DShapeTemplate_EggHelix.png](../Thumbs/NS_3DShapeTemplate_EggHelix.png)
视频**: [NS_3DShapeTemplate_EggHelix.mp4](../Videos/NS_3DShapeTemplate_EggHelix.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_FigureEightKnot — 八字结

![八字结](../Thumbs/NS_3DShapeTemplate_FigureEightKnot.png)

类型**: 3D parametric curve
公式**: `float3((2.0+cos(2.0*InputT))*cos(3.0*InputT), (2.0+cos(2.0*InputT))*sin(3.0*InputT), sin(4.0*InputT))`
说明**: Figure-eight knot.
动态输入**: `/EasyNSShape/NDI/NDI_3D_FigureEightKnot`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_FigureEightKnot`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_FigureEightKnot`
源函数**: `/EasyNSShape/NFS/NFS3D_FigureEightKnot`
快照**: [NS_3DShapeTemplate_FigureEightKnot.png](../Thumbs/NS_3DShapeTemplate_FigureEightKnot.png)
视频**: [NS_3DShapeTemplate_FigureEightKnot.mp4](../Videos/NS_3DShapeTemplate_FigureEightKnot.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_GreatCircleSphere — 球面大圆

![球面大圆](../Thumbs/NS_3DShapeTemplate_GreatCircleSphere.png)

类型**: 3D parametric curve
公式**: `float3(cos(InputT*0.5)*cos(InputT), cos(InputT*0.5)*sin(InputT), sin(InputT*0.5))`
说明**: Great circle on a sphere.
动态输入**: `/EasyNSShape/NDI/NDI_3D_GreatCircleSphere`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_GreatCircleSphere`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_GreatCircleSphere`
源函数**: `/EasyNSShape/NFS/NFS3D_GreatCircleSphere`
快照**: [NS_3DShapeTemplate_GreatCircleSphere.png](../Thumbs/NS_3DShapeTemplate_GreatCircleSphere.png)
视频**: [NS_3DShapeTemplate_GreatCircleSphere.mp4](../Videos/NS_3DShapeTemplate_GreatCircleSphere.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_HyperbolicHelix — 双曲螺旋线

![双曲螺旋线](../Thumbs/NS_3DShapeTemplate_HyperbolicHelix.png)

类型**: 3D parametric curve
公式**: `float3(cosh(InputT)*cos(InputT), cosh(InputT)*sin(InputT), sinh(InputT))`
说明**: Hyperbolic helix (cosh/sinh).
动态输入**: `/EasyNSShape/NDI/NDI_3D_HyperbolicHelix`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_HyperbolicHelix`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_HyperbolicHelix`
源函数**: `/EasyNSShape/NFS/NFS3D_HyperbolicHelix`
快照**: [NS_3DShapeTemplate_HyperbolicHelix.png](../Thumbs/NS_3DShapeTemplate_HyperbolicHelix.png)
视频**: [NS_3DShapeTemplate_HyperbolicHelix.mp4](../Videos/NS_3DShapeTemplate_HyperbolicHelix.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_HyperbolicSpiralCylinder — 双曲螺线柱面

![双曲螺线柱面](../Thumbs/NS_3DShapeTemplate_HyperbolicSpiralCylinder.png)

类型**: 3D parametric curve
公式**: `float3(cos(InputT), sin(InputT), InputT/(InputT*InputT+1.0))`
说明**: Hyperbolic spiral wrapped on a cylinder.
动态输入**: `/EasyNSShape/NDI/NDI_3D_HyperbolicSpiralCylinder`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_HyperbolicSpiralCylinder`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_HyperbolicSpiralCylinder`
源函数**: `/EasyNSShape/NFS/NFS3D_HyperbolicSpiralCylinder`
快照**: [NS_3DShapeTemplate_HyperbolicSpiralCylinder.png](../Thumbs/NS_3DShapeTemplate_HyperbolicSpiralCylinder.png)
视频**: [NS_3DShapeTemplate_HyperbolicSpiralCylinder.mp4](../Videos/NS_3DShapeTemplate_HyperbolicSpiralCylinder.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_InvoluteCylinder — 渐开线柱面

![渐开线柱面](../Thumbs/NS_3DShapeTemplate_InvoluteCylinder.png)

类型**: 3D parametric curve
公式**: `float3(cos(InputT)+InputT*sin(InputT), sin(InputT)-InputT*cos(InputT), InputT)`
说明**: Involute wrapped on a cylinder.
动态输入**: `/EasyNSShape/NDI/NDI_3D_InvoluteCylinder`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_InvoluteCylinder`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_InvoluteCylinder`
源函数**: `/EasyNSShape/NFS/NFS3D_InvoluteCylinder`
快照**: [NS_3DShapeTemplate_InvoluteCylinder.png](../Thumbs/NS_3DShapeTemplate_InvoluteCylinder.png)
视频**: [NS_3DShapeTemplate_InvoluteCylinder.mp4](../Videos/NS_3DShapeTemplate_InvoluteCylinder.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_Line3D — 三维直线

![三维直线](../Thumbs/NS_3DShapeTemplate_Line3D.png)

类型**: 3D parametric curve
公式**: `float3(InputT, InputT*0.5, InputT*0.3)`
说明**: Straight 3D line.
动态输入**: `/EasyNSShape/NDI/NDI_3D_Line3D`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_Line3D`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_Line3D`
源函数**: `/EasyNSShape/NFS/NFS3D_Line3D`
快照**: [NS_3DShapeTemplate_Line3D.png](../Thumbs/NS_3DShapeTemplate_Line3D.png)
视频**: [NS_3DShapeTemplate_Line3D.mp4](../Videos/NS_3DShapeTemplate_Line3D.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_LissajousCylinder — 利萨茹柱面

![利萨茹柱面](../Thumbs/NS_3DShapeTemplate_LissajousCylinder.png)

类型**: 3D parametric curve
公式**: `float3(cos(InputT), sin(InputT), cos(2.0*InputT))`
说明**: Lissajous figure wrapped on a cylinder.
动态输入**: `/EasyNSShape/NDI/NDI_3D_LissajousCylinder`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_LissajousCylinder`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_LissajousCylinder`
源函数**: `/EasyNSShape/NFS/NFS3D_LissajousCylinder`
快照**: [NS_3DShapeTemplate_LissajousCylinder.png](../Thumbs/NS_3DShapeTemplate_LissajousCylinder.png)
视频**: [NS_3DShapeTemplate_LissajousCylinder.mp4](../Videos/NS_3DShapeTemplate_LissajousCylinder.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_MobiusCenterLine — 莫比乌斯中心线

![莫比乌斯中心线](../Thumbs/NS_3DShapeTemplate_MobiusCenterLine.png)

类型**: 3D parametric curve
公式**: `float3(cos(InputT), sin(InputT), 0.0)`
说明**: Centre line of a Mobius strip.
动态输入**: `/EasyNSShape/NDI/NDI_3D_MobiusCenterLine`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_MobiusCenterLine`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_MobiusCenterLine`
源函数**: `/EasyNSShape/NFS/NFS3D_MobiusCenterLine`
快照**: [NS_3DShapeTemplate_MobiusCenterLine.png](../Thumbs/NS_3DShapeTemplate_MobiusCenterLine.png)
视频**: [NS_3DShapeTemplate_MobiusCenterLine.mp4](../Videos/NS_3DShapeTemplate_MobiusCenterLine.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_ParabolicHelix — 抛物螺旋线

![抛物螺旋线](../Thumbs/NS_3DShapeTemplate_ParabolicHelix.png)

类型**: 3D parametric curve
公式**: `float3(InputT*cos(InputT), InputT*sin(InputT), InputT*InputT)`
说明**: Parabolic helix — radius and height grow with t.
动态输入**: `/EasyNSShape/NDI/NDI_3D_ParabolicHelix`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_ParabolicHelix`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_ParabolicHelix`
源函数**: `/EasyNSShape/NFS/NFS3D_ParabolicHelix`
快照**: [NS_3DShapeTemplate_ParabolicHelix.png](../Thumbs/NS_3DShapeTemplate_ParabolicHelix.png)
视频**: [NS_3DShapeTemplate_ParabolicHelix.mp4](../Videos/NS_3DShapeTemplate_ParabolicHelix.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_QuarticSpace — 四次空间曲线

![四次空间曲线](../Thumbs/NS_3DShapeTemplate_QuarticSpace.png)

类型**: 3D parametric curve
公式**: `float3(pow(InputT,4.0)-2.0*InputT*InputT, InputT*InputT*InputT, InputT*InputT)`
说明**: Quartic space curve.
动态输入**: `/EasyNSShape/NDI/NDI_3D_QuarticSpace`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_QuarticSpace`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_QuarticSpace`
源函数**: `/EasyNSShape/NFS/NFS3D_QuarticSpace`
快照**: [NS_3DShapeTemplate_QuarticSpace.png](../Thumbs/NS_3DShapeTemplate_QuarticSpace.png)
视频**: [NS_3DShapeTemplate_QuarticSpace.mp4](../Videos/NS_3DShapeTemplate_QuarticSpace.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_SineCosineSpace — 正弦余弦空间曲线

![正弦余弦空间曲线](../Thumbs/NS_3DShapeTemplate_SineCosineSpace.png)

类型**: 3D parametric curve
公式**: `float3(InputT, sin(InputT), cos(InputT))`
说明**: Sine/cosine space curve.
动态输入**: `/EasyNSShape/NDI/NDI_3D_SineCosineSpace`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_SineCosineSpace`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_SineCosineSpace`
源函数**: `/EasyNSShape/NFS/NFS3D_SineCosineSpace`
快照**: [NS_3DShapeTemplate_SineCosineSpace.png](../Thumbs/NS_3DShapeTemplate_SineCosineSpace.png)
视频**: [NS_3DShapeTemplate_SineCosineSpace.mp4](../Videos/NS_3DShapeTemplate_SineCosineSpace.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_SineParabolic — 正弦抛物线

![正弦抛物线](../Thumbs/NS_3DShapeTemplate_SineParabolic.png)

类型**: 3D parametric curve
公式**: `float3(InputT, InputT*InputT, sin(InputT))`
说明**: Sine/parabolic space curve.
动态输入**: `/EasyNSShape/NDI/NDI_3D_SineParabolic`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_SineParabolic`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_SineParabolic`
源函数**: `/EasyNSShape/NFS/NFS3D_SineParabolic`
快照**: [NS_3DShapeTemplate_SineParabolic.png](../Thumbs/NS_3DShapeTemplate_SineParabolic.png)
视频**: [NS_3DShapeTemplate_SineParabolic.mp4](../Videos/NS_3DShapeTemplate_SineParabolic.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_SinusoidalCylinder — 正弦柱面

![正弦柱面](../Thumbs/NS_3DShapeTemplate_SinusoidalCylinder.png)

类型**: 3D parametric curve
公式**: `float3(cos(InputT), sin(InputT), sin(InputT))`
说明**: Sinusoidal cylinder.
动态输入**: `/EasyNSShape/NDI/NDI_3D_SinusoidalCylinder`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_SinusoidalCylinder`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_SinusoidalCylinder`
源函数**: `/EasyNSShape/NFS/NFS3D_SinusoidalCylinder`
快照**: [NS_3DShapeTemplate_SinusoidalCylinder.png](../Thumbs/NS_3DShapeTemplate_SinusoidalCylinder.png)
视频**: [NS_3DShapeTemplate_SinusoidalCylinder.mp4](../Videos/NS_3DShapeTemplate_SinusoidalCylinder.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_SlantedHelix — 倾斜螺旋线

![倾斜螺旋线](../Thumbs/NS_3DShapeTemplate_SlantedHelix.png)

类型**: 3D parametric curve
公式**: `float3(cos(InputT), sin(InputT), InputT*InputT)`
说明**: Helix with a quadratic Z rise.
动态输入**: `/EasyNSShape/NDI/NDI_3D_SlantedHelix`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_SlantedHelix`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_SlantedHelix`
源函数**: `/EasyNSShape/NFS/NFS3D_SlantedHelix`
快照**: [NS_3DShapeTemplate_SlantedHelix.png](../Thumbs/NS_3DShapeTemplate_SlantedHelix.png)
视频**: [NS_3DShapeTemplate_SlantedHelix.mp4](../Videos/NS_3DShapeTemplate_SlantedHelix.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_SphericalHelix — 球面螺旋线

![球面螺旋线](../Thumbs/NS_3DShapeTemplate_SphericalHelix.png)

类型**: 3D parametric curve
公式**: `float3(cos(InputT)*sin(InputT), sin(InputT)*sin(InputT), cos(InputT))`
说明**: Spherical helix.
动态输入**: `/EasyNSShape/NDI/NDI_3D_SphericalHelix`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_SphericalHelix`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_SphericalHelix`
源函数**: `/EasyNSShape/NFS/NFS3D_SphericalHelix`
快照**: [NS_3DShapeTemplate_SphericalHelix.png](../Thumbs/NS_3DShapeTemplate_SphericalHelix.png)
视频**: [NS_3DShapeTemplate_SphericalHelix.mp4](../Videos/NS_3DShapeTemplate_SphericalHelix.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_ToroidalKnot52 — 环面纽结 (5,2)

![环面纽结 (5,2)](../Thumbs/NS_3DShapeTemplate_ToroidalKnot52.png)

类型**: 3D parametric curve
公式**: `float3(cos(3.0*InputT)*cos(2.0*InputT), cos(3.0*InputT)*sin(2.0*InputT), sin(3.0*InputT))`
说明**: Torus knot with p=5, q=2.
动态输入**: `/EasyNSShape/NDI/NDI_3D_ToroidalKnot52`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_ToroidalKnot52`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_ToroidalKnot52`
源函数**: `/EasyNSShape/NFS/NFS3D_ToroidalKnot52`
快照**: [NS_3DShapeTemplate_ToroidalKnot52.png](../Thumbs/NS_3DShapeTemplate_ToroidalKnot52.png)
视频**: [NS_3DShapeTemplate_ToroidalKnot52.mp4](../Videos/NS_3DShapeTemplate_ToroidalKnot52.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_ToroidalKnot52b — 环面纽结 (5,2) 变体

![环面纽结 (5,2) 变体](../Thumbs/NS_3DShapeTemplate_ToroidalKnot52b.png)

类型**: 3D parametric curve
公式**: `float3((2.0+cos(2.0*InputT))*cos(5.0*InputT), (2.0+cos(2.0*InputT))*sin(5.0*InputT), sin(2.0*InputT))`
说明**: Torus knot (5,2), variant.
动态输入**: `/EasyNSShape/NDI/NDI_3D_ToroidalKnot52b`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_ToroidalKnot52b`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_ToroidalKnot52b`
源函数**: `/EasyNSShape/NFS/NFS3D_ToroidalKnot52b`
快照**: [NS_3DShapeTemplate_ToroidalKnot52b.png](../Thumbs/NS_3DShapeTemplate_ToroidalKnot52b.png)
视频**: [NS_3DShapeTemplate_ToroidalKnot52b.mp4](../Videos/NS_3DShapeTemplate_ToroidalKnot52b.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_TrefoilKnot — 三叶结

![三叶结](../Thumbs/NS_3DShapeTemplate_TrefoilKnot.png)

类型**: 3D parametric curve
公式**: `float3(sin(InputT)+2.0*sin(2.0*InputT), cos(InputT)-2.0*cos(2.0*InputT), -sin(3.0*InputT))`
说明**: Trefoil knot.
动态输入**: `/EasyNSShape/NDI/NDI_3D_TrefoilKnot`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_TrefoilKnot`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_TrefoilKnot`
源函数**: `/EasyNSShape/NFS/NFS3D_TrefoilKnot`
快照**: [NS_3DShapeTemplate_TrefoilKnot.png](../Thumbs/NS_3DShapeTemplate_TrefoilKnot.png)
视频**: [NS_3DShapeTemplate_TrefoilKnot.mp4](../Videos/NS_3DShapeTemplate_TrefoilKnot.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_VariableRadiusHelix — 变半径螺旋线

![变半径螺旋线](../Thumbs/NS_3DShapeTemplate_VariableRadiusHelix.png)

类型**: 3D parametric curve
公式**: `float3((1.0+0.5*InputT)*cos(InputT), (1.0+0.5*InputT)*sin(InputT), InputT*0.3)`
说明**: Helix whose radius grows with t.
动态输入**: `/EasyNSShape/NDI/NDI_3D_VariableRadiusHelix`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_VariableRadiusHelix`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_VariableRadiusHelix`
源函数**: `/EasyNSShape/NFS/NFS3D_VariableRadiusHelix`
快照**: [NS_3DShapeTemplate_VariableRadiusHelix.png](../Thumbs/NS_3DShapeTemplate_VariableRadiusHelix.png)
视频**: [NS_3DShapeTemplate_VariableRadiusHelix.mp4](../Videos/NS_3DShapeTemplate_VariableRadiusHelix.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

### NDI_3D_Viviani — 维维亚尼曲线

![维维亚尼曲线](../Thumbs/NS_3DShapeTemplate_Viviani.png)

类型**: 3D parametric curve
公式**: `float3(cos(InputT)*cos(InputT), cos(InputT)*sin(InputT), sin(InputT))`
说明**: Viviani curve (sphere ∩ cylinder).
动态输入**: `/EasyNSShape/NDI/NDI_3D_Viviani`
发射器**: `/EasyNSShape/NEBridge/NE/NE_3DShapeTemplate_Viviani`
系统**: `/EasyNSShape/NEBridge/NS/NS_3D_Viviani`
源函数**: `/EasyNSShape/NFS/NFS3D_Viviani`
快照**: [NS_3DShapeTemplate_Viviani.png](../Thumbs/NS_3DShapeTemplate_Viviani.png)
视频**: [NS_3DShapeTemplate_Viviani.mp4](../Videos/NS_3DShapeTemplate_Viviani.mp4)
输入**: `Module.InputT` (float)
输出**: `float3`

## 3D 特型形状详情 (12)

Full-resolution snapshots have not been captured for these 12 shapes yet — only the 240×135 picture (`Thumbs/`) and video exist for each. 本组 12 个形状尚未生成全分辨率快照，目前每个形状仅提供 240×135 图片（`Thumbs/`）与视频。

### NDI_3P_3D_CircularHelix — 圆柱螺旋线 (3参数)

![圆柱螺旋线 (3参数)](../Thumbs/NS_3P_3DShapeTemplate_CircularHelix.png)

类型**: 3D parametric curve (3 parameters)
公式**: `float3(InputU*cos(InputT), InputU*sin(InputT), InputV*InputT)`
说明**: 3D circular helix — x=r*cos(t), y=r*sin(t), z=c*t.
动态输入**: `/EasyNSShape/NDI/Special/NDI_3P_3D_CircularHelix`
发射器**: `/EasyNSShape/NEBridge/Special/NE_3P_3DShapeTemplate_CircularHelix`
系统**: `/EasyNSShape/NEBridge/Special/NS_3P_3DShapeTemplate_CircularHelix`
源函数**: `/EasyNSShape/NFS/NFS_3P_3D_CircularHelix`
快照**: [NS_3P_3DShapeTemplate_CircularHelix.png](../Thumbs/NS_3P_3DShapeTemplate_CircularHelix.png)
视频**: [NS_3P_3DShapeTemplate_CircularHelix.mp4](../Videos/NS_3P_3DShapeTemplate_CircularHelix.mp4)
输入**: `Module.InputT` (float, t), `Module.InputU` (float, r), `Module.InputV` (float, c)
输出**: `float3`

### NDI_3P_3D_CycloidalHelix — 摆线螺旋 (3参数)

![摆线螺旋 (3参数)](../Thumbs/NS_3P_3DShapeTemplate_CycloidalHelix.png)

类型**: 3D parametric curve (3 parameters)
公式**: `float3(InputU*(InputT-sin(InputT)), InputU*(1.0-cos(InputT)), InputV*InputT)`
说明**: 3D cycloidal helix — x=a*(t-sin(t)), y=a*(1-cos(t)), z=b*t.
动态输入**: `/EasyNSShape/NDI/Special/NDI_3P_3D_CycloidalHelix`
发射器**: `/EasyNSShape/NEBridge/Special/NE_3P_3DShapeTemplate_CycloidalHelix`
系统**: `/EasyNSShape/NEBridge/Special/NS_3P_3DShapeTemplate_CycloidalHelix`
源函数**: `/EasyNSShape/NFS/NFS_3P_3D_CycloidalHelix`
快照**: [NS_3P_3DShapeTemplate_CycloidalHelix.png](../Thumbs/NS_3P_3DShapeTemplate_CycloidalHelix.png)
视频**: [NS_3P_3DShapeTemplate_CycloidalHelix.mp4](../Videos/NS_3P_3DShapeTemplate_CycloidalHelix.mp4)
输入**: `Module.InputT` (float, t), `Module.InputU` (float, a), `Module.InputV` (float, b)
输出**: `float3`

### NDI_3P_3D_InvoluteCylinder — 渐开线柱面 (3参数)

![渐开线柱面 (3参数)](../Thumbs/NS_3P_3DShapeTemplate_InvoluteCylinder.png)

类型**: 3D parametric curve (3 parameters)
公式**: `float3(InputU*(cos(InputT)+InputT*sin(InputT)), InputU*(sin(InputT)-InputT*cos(InputT)), InputV*InputT)`
说明**: 3D involute on cylinder — x=R*(cos(t)+t*sin(t)), y=R*(sin(t)-t*cos(t)), z=c*t.
动态输入**: `/EasyNSShape/NDI/Special/NDI_3P_3D_InvoluteCylinder`
发射器**: `/EasyNSShape/NEBridge/Special/NE_3P_3DShapeTemplate_InvoluteCylinder`
系统**: `/EasyNSShape/NEBridge/Special/NS_3P_3DShapeTemplate_InvoluteCylinder`
源函数**: `/EasyNSShape/NFS/NFS_3P_3D_InvoluteCylinder`
快照**: [NS_3P_3DShapeTemplate_InvoluteCylinder.png](../Thumbs/NS_3P_3DShapeTemplate_InvoluteCylinder.png)
视频**: [NS_3P_3DShapeTemplate_InvoluteCylinder.mp4](../Videos/NS_3P_3DShapeTemplate_InvoluteCylinder.mp4)
输入**: `Module.InputT` (float, t), `Module.InputU` (float, R), `Module.InputV` (float, c)
输出**: `float3`

### NDI_3P_3D_SlantedHelix — 倾斜螺旋线 (3参数)

![倾斜螺旋线 (3参数)](../Thumbs/NS_3P_3DShapeTemplate_SlantedHelix.png)

类型**: 3D parametric curve (3 parameters)
公式**: `float3(InputU*cos(InputT), InputU*sin(InputT), InputV*InputT*InputT)`
说明**: 3D slanted helix — x=a*cos(t), y=a*sin(t), z=b*t^2.
动态输入**: `/EasyNSShape/NDI/Special/NDI_3P_3D_SlantedHelix`
发射器**: `/EasyNSShape/NEBridge/Special/NE_3P_3DShapeTemplate_SlantedHelix`
系统**: `/EasyNSShape/NEBridge/Special/NS_3P_3DShapeTemplate_SlantedHelix`
源函数**: `/EasyNSShape/NFS/NFS_3P_3D_SlantedHelix`
快照**: [NS_3P_3DShapeTemplate_SlantedHelix.png](../Thumbs/NS_3P_3DShapeTemplate_SlantedHelix.png)
视频**: [NS_3P_3DShapeTemplate_SlantedHelix.mp4](../Videos/NS_3P_3DShapeTemplate_SlantedHelix.mp4)
输入**: `Module.InputT` (float, t), `Module.InputU` (float, a), `Module.InputV` (float, b)
输出**: `float3`

### NDI_3P_3D_SphericalHelix — 球面螺旋线 (3参数)

![球面螺旋线 (3参数)](../Thumbs/NS_3P_3DShapeTemplate_SphericalHelix.png)

类型**: 3D parametric curve (3 parameters)
公式**: `float3(InputU*cos(InputT)*sin(InputV*InputT), InputU*sin(InputT)*sin(InputV*InputT), InputU*cos(InputV*InputT))`
说明**: 3D spherical helix — x=a*cos(t)*sin(c*t), y=a*sin(t)*sin(c*t), z=a*cos(c*t).
动态输入**: `/EasyNSShape/NDI/Special/NDI_3P_3D_SphericalHelix`
发射器**: `/EasyNSShape/NEBridge/Special/NE_3P_3DShapeTemplate_SphericalHelix`
系统**: `/EasyNSShape/NEBridge/Special/NS_3P_3DShapeTemplate_SphericalHelix`
源函数**: `/EasyNSShape/NFS/NFS_3P_3D_SphericalHelix`
快照**: [NS_3P_3DShapeTemplate_SphericalHelix.png](../Thumbs/NS_3P_3DShapeTemplate_SphericalHelix.png)
视频**: [NS_3P_3DShapeTemplate_SphericalHelix.mp4](../Videos/NS_3P_3DShapeTemplate_SphericalHelix.mp4)
输入**: `Module.InputT` (float, t), `Module.InputU` (float, a), `Module.InputV` (float, c)
输出**: `float3`

### NDI_4P_3D_EggHelix — 蛋形螺旋线 (4参数)

![蛋形螺旋线 (4参数)](../Thumbs/NS_4P_3DShapeTemplate_EggHelix.png)

类型**: 3D parametric curve (4 parameters)
公式**: `float3((InputA+InputB*cos(InputT))*cos(InputT), (InputA+InputB*cos(InputT))*sin(InputT), InputC*sin(InputT))`
说明**: 3D egg-shaped helix — x=(a+b*cos(t))*cos(t), y=(a+b*cos(t))*sin(t), z=c*sin(t).
动态输入**: `/EasyNSShape/NDI/Special/NDI_4P_3D_EggHelix`
发射器**: `/EasyNSShape/NEBridge/Special/NE_4P_3DShapeTemplate_EggHelix`
系统**: `/EasyNSShape/NEBridge/Special/NS_4P_3DShapeTemplate_EggHelix`
源函数**: `/EasyNSShape/NFS/NFS_4P_3D_EggHelix`
快照**: [NS_4P_3DShapeTemplate_EggHelix.png](../Thumbs/NS_4P_3DShapeTemplate_EggHelix.png)
视频**: [NS_4P_3DShapeTemplate_EggHelix.mp4](../Videos/NS_4P_3DShapeTemplate_EggHelix.mp4)
输入**: `Module.InputT` (float, t), `Module.InputA` (float, a), `Module.InputB` (float, b), `Module.InputC` (float, c)
输出**: `float3`

### NDI_4P_3D_SinusoidalCylinder — 正弦柱面 (4参数)

![正弦柱面 (4参数)](../Thumbs/NS_4P_3DShapeTemplate_SinusoidalCylinder.png)

类型**: 3D parametric curve (4 parameters)
公式**: `float3(InputR*cos(InputT), InputR*sin(InputT), InputA*sin(InputK*InputT))`
说明**: 3D sinusoidal curve on cylinder — x=R*cos(t), y=R*sin(t), z=A*sin(k*t).
动态输入**: `/EasyNSShape/NDI/Special/NDI_4P_3D_SinusoidalCylinder`
发射器**: `/EasyNSShape/NEBridge/Special/NE_4P_3DShapeTemplate_SinusoidalCylinder`
系统**: `/EasyNSShape/NEBridge/Special/NS_4P_3DShapeTemplate_SinusoidalCylinder`
源函数**: `/EasyNSShape/NFS/NFS_4P_3D_SinusoidalCylinder`
快照**: [NS_4P_3DShapeTemplate_SinusoidalCylinder.png](../Thumbs/NS_4P_3DShapeTemplate_SinusoidalCylinder.png)
视频**: [NS_4P_3DShapeTemplate_SinusoidalCylinder.mp4](../Videos/NS_4P_3DShapeTemplate_SinusoidalCylinder.mp4)
输入**: `Module.InputT` (float, t), `Module.InputR` (float, R), `Module.InputA` (float, A), `Module.InputK` (float, k)
输出**: `float3`

### NDI_4P_3D_TiltedEllipticHelix — 倾斜椭圆螺旋线 (4参数)

![倾斜椭圆螺旋线 (4参数)](../Thumbs/NS_4P_3DShapeTemplate_TiltedEllipticHelix.png)

类型**: 3D parametric curve (4 parameters)
公式**: `float3(InputA*cos(InputT), InputB*sin(InputT), InputC*InputT*sin(InputT))`
说明**: 3D tilted elliptic helix — x=a*cos(t), y=b*sin(t), z=c*t*sin(t).
动态输入**: `/EasyNSShape/NDI/Special/NDI_4P_3D_TiltedEllipticHelix`
发射器**: `/EasyNSShape/NEBridge/Special/NE_4P_3DShapeTemplate_TiltedEllipticHelix`
系统**: `/EasyNSShape/NEBridge/Special/NS_4P_3DShapeTemplate_TiltedEllipticHelix`
源函数**: `/EasyNSShape/NFS/NFS_4P_3D_TiltedEllipticHelix`
快照**: [NS_4P_3DShapeTemplate_TiltedEllipticHelix.png](../Thumbs/NS_4P_3DShapeTemplate_TiltedEllipticHelix.png)
视频**: [NS_4P_3DShapeTemplate_TiltedEllipticHelix.mp4](../Videos/NS_4P_3DShapeTemplate_TiltedEllipticHelix.mp4)
输入**: `Module.InputT` (float, t), `Module.InputA` (float, a), `Module.InputB` (float, b), `Module.InputC` (float, c)
输出**: `float3`

### NDI_4P_3D_ToroidalHelix — 环形螺旋线 (4参数)

![环形螺旋线 (4参数)](../Thumbs/NS_4P_3DShapeTemplate_ToroidalHelix.png)

类型**: 3D parametric curve (4 parameters)
公式**: `float3((InputR+InputRho*cos(InputN*InputT))*cos(InputT), (InputR+InputRho*cos(InputN*InputT))*sin(InputT), InputRho*sin(InputN*InputT))`
说明**: 3D toroidal helix — x=(R+r*cos(n*t))*cos(t), y=(R+r*cos(n*t))*sin(t), z=r*sin(n*t).
动态输入**: `/EasyNSShape/NDI/Special/NDI_4P_3D_ToroidalHelix`
发射器**: `/EasyNSShape/NEBridge/Special/NE_4P_3DShapeTemplate_ToroidalHelix`
系统**: `/EasyNSShape/NEBridge/Special/NS_4P_3DShapeTemplate_ToroidalHelix`
源函数**: `/EasyNSShape/NFS/NFS_4P_3D_ToroidalHelix`
快照**: [NS_4P_3DShapeTemplate_ToroidalHelix.png](../Thumbs/NS_4P_3DShapeTemplate_ToroidalHelix.png)
视频**: [NS_4P_3DShapeTemplate_ToroidalHelix.mp4](../Videos/NS_4P_3DShapeTemplate_ToroidalHelix.mp4)
输入**: `Module.InputT` (float, t), `Module.InputR` (float, R), `Module.InputRho` (float, r), `Module.InputN` (float, n)
输出**: `float3`

### NDI_4P_3D_VariableRadiusHelix — 变半径螺旋线 (4参数)

![变半径螺旋线 (4参数)](../Thumbs/NS_4P_3DShapeTemplate_VariableRadiusHelix.png)

类型**: 3D parametric curve (4 parameters)
公式**: `float3((InputA+InputB*InputT)*cos(InputT), (InputA+InputB*InputT)*sin(InputT), InputC*InputT)`
说明**: 3D variable-radius helix — x=(a+b*t)*cos(t), y=(a+b*t)*sin(t), z=c*t.
动态输入**: `/EasyNSShape/NDI/Special/NDI_4P_3D_VariableRadiusHelix`
发射器**: `/EasyNSShape/NEBridge/Special/NE_4P_3DShapeTemplate_VariableRadiusHelix`
系统**: `/EasyNSShape/NEBridge/Special/NS_4P_3DShapeTemplate_VariableRadiusHelix`
源函数**: `/EasyNSShape/NFS/NFS_4P_3D_VariableRadiusHelix`
快照**: [NS_4P_3DShapeTemplate_VariableRadiusHelix.png](../Thumbs/NS_4P_3DShapeTemplate_VariableRadiusHelix.png)
视频**: [NS_4P_3DShapeTemplate_VariableRadiusHelix.mp4](../Videos/NS_4P_3DShapeTemplate_VariableRadiusHelix.mp4)
输入**: `Module.InputT` (float, t), `Module.InputA` (float, a), `Module.InputB` (float, b), `Module.InputC` (float, c)
输出**: `float3`

### NDI_5P_3D_CubicBezier — 三次贝塞尔曲线 (5参数)

![三次贝塞尔曲线 (5参数)](../Thumbs/NS_5P_3DShapeTemplate_CubicBezier.png)

类型**: 3D parametric curve (5 parameters)
公式**: `pow(1.0-InputT,3.0)*InputP0 + 3.0*pow(1.0-InputT,2.0)*InputT*InputP1 + 3.0*(1.0-InputT)*InputT*InputT*InputP2 + InputT*InputT*InputT*InputP3`
说明**: 3D cubic Bezier curve — P(t)=(1-t)^3*P0+3*(1-t)^2*t*P1+3*(1-t)*t^2*P2+t^3*P3.
动态输入**: `/EasyNSShape/NDI/Special/NDI_5P_3D_CubicBezier`
发射器**: `/EasyNSShape/NEBridge/Special/NE_5P_3DShapeTemplate_CubicBezier`
系统**: `/EasyNSShape/NEBridge/Special/NS_5P_3DShapeTemplate_CubicBezier`
源函数**: `/EasyNSShape/NFS/NFS_5P_3D_CubicBezier`
快照**: [NS_5P_3DShapeTemplate_CubicBezier.png](../Thumbs/NS_5P_3DShapeTemplate_CubicBezier.png)
视频**: [NS_5P_3DShapeTemplate_CubicBezier.mp4](../Videos/NS_5P_3DShapeTemplate_CubicBezier.mp4)
输入**: `Module.InputT` (float, t), `Module.InputP0`…`Module.InputP3` (float3, control points (vec3))
输出**: `float3`

### NDI_7P_3D_Line3D — 三维直线 (7参数)

![三维直线 (7参数)](../Thumbs/NS_7P_3DShapeTemplate_Line3D.png)

类型**: 3D parametric curve (7 parameters)
公式**: `float3(InputX0+InputA*InputT, InputY0+InputB*InputT, InputZ0+InputC*InputT)`
说明**: 3D line — x=x0+a*t, y=y0+b*t, z=z0+c*t.
动态输入**: `/EasyNSShape/NDI/Special/NDI_7P_3D_Line3D`
发射器**: `/EasyNSShape/NEBridge/Special/NE_7P_3DShapeTemplate_Line3D`
系统**: `/EasyNSShape/NEBridge/Special/NS_7P_3DShapeTemplate_Line3D`
源函数**: `/EasyNSShape/NFS/NFS_7P_3D_Line3D`
快照**: [NS_7P_3DShapeTemplate_Line3D.png](../Thumbs/NS_7P_3DShapeTemplate_Line3D.png)
视频**: [NS_7P_3DShapeTemplate_Line3D.mp4](../Videos/NS_7P_3DShapeTemplate_Line3D.mp4)
输入**: `Module.InputT` (float, t), `Module.InputX0` (float, x0), `Module.InputY0` (float, y0), `Module.InputZ0` (float, z0), `Module.InputA` (float, a), `Module.InputB` (float, b), `Module.InputC` (float, c)
输出**: `float3`

## 资源路径约定

| Asset type | Example |
|---|---|
| NDI | `/EasyNSShape/NDI/NDI_2D_Circle` |
| NFS | `/EasyNSShape/NFS/NFS2D_Circle` |
| NE | `/EasyNSShape/NEBridge/NE/NE_2DShapeTemplate_Circle` |
| NS | `/EasyNSShape/NEBridge/NS/NS_2D_Circle` |
| NMS | `/EasyNSShape/NETemplate/NMS_2DShapeVector` |

3D 变体仅在 `NDI_3D_` / `NE_3DShapeTemplate_` / `NS_3D_` / `NFS3D_` 前缀后使用相同后缀。
