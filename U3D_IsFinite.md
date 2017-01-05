#Uinty3D !IsFinite(outDistanceForSort) ; !IsFinite(outDistanceAlongView)错误的原因

出现 `!IsFinite(outDistanceForSort)` ; `!IsFinite(outDistanceAlongView)`的原因是可能在Transform组件的某处出现isNan的值或负数

快速定位，运行场景，在`Hierarchy`中选择`object`并`setactive(false)`排查是哪个`object`引起的，然后采取措施修正`Transform`里的数值！