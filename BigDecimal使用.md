---
title: BigDecimal使用
---

### 名词解释
1. 向上取整
2. 向下取整
3. 四舍五入

### API说明

1. RoundingMode.CEILING：取右边最近的整数
2. RoundingMode.DOWN：去掉小数部分取整，也就是正数取左边，负数取右边，相当于向原点靠近的方向取整
3. RoundingMode.FLOOR：取左边最近的正数
4. RoundingMode.HALF_DOWN:五舍六入，负数先取绝对值再五舍六入再负数
5. RoundingMode.HALF_UP:四舍五入，负数原理同上
6. RoundingMode.HALF_EVEN:这个比较绕，整数位若是奇数则四舍五入，若是偶数则五舍六入