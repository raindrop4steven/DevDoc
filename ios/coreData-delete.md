### Show the attributes inspector中View下Clip Subviews作用。

- Opaque ：  如果选择 ，则此视图后的任何内容都不应绘制。
- Clip subviews:如果你的视图有子视图，并且这些子视图并不是完全包含在父视图中，则此复选框将确定子视图的绘制方式，如果选中了ClipSubviews，只有在父视图范围内的子视图被绘制出来。如果未选中Clip Subviews 则全部子视图都将绘制出来，而不管它是否在父视图内部。


### 两个记录点：

1. 在 绘制 拥有多个 subview 的view 的圆角时， 需要勾选 clip Subviews；
2. 在自定义 view 时，声明属性前使用 IBInspectable 可以在xib 中直接赋值；
