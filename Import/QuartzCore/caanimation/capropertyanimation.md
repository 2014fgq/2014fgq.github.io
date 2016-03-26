# CAPropertyAnimation
- 父类是CAAnimation


###CAPropertyAnimation
- 是CAAnimation的子类，也是个抽象类，要想创建动画对象，应该使用它的两个子类：
    - CABasicAnimation
    - CAKeyframeAnimation

- 属性说明：keyPath——通过指定CALayer的一个属性名称为keyPath（NSString类型），并且对CALayer的这个属性的值进行修改，达到相应的动画效果。比如，指定@“position”为keyPath，就修改CALayer的position属性的值，以达到平移的动画效果
