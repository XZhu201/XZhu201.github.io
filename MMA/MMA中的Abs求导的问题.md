# 我在MMA符号计算求导结果中出现了Abs‘[***]的情况

### 1. 求教了群里面的大神，一句话点明了原因：

> chy(785644070) 15:41:16
> Abs不能求导，改成RealAbs

### 2. 我在网上也搜了一下，确实是这样的问题：

https://community.wolfram.com/groups/-/m/t/384859

其中以下的回复把出现问题的原因也讨论了：

> The reason why you are encountering this issue, I think, is that the derivative of the absolute value generally does not exist in the complex plane, though it does exist on the real line--or any other fixed curve in the complex plane since that determines explicitly the directions of the differentiation. See
>
> http://mathworld.wolfram.com/AbsoluteValue.html
>
> and see the discussion in the paragraphs surrounding equation 4. The return of Abs'[...] unevaluated is a sign of this. When you make use of ComplexExpand, it assumes all parameters are real, and so it Evaluates Abs'[z] assuming that z is real, which it isn't in your case.
>
> Thus the correct way to go about your computation is to evaluate
>
> ```mma
> ComplexExpand[Rho1]
> ```
>
> before performing any differentiation. This then leads to your value of 2/5 as expected.

### 3. 我实际的解决方法

上面第二条中回复建议先用`ComplexExpand`来解决。其实归根结底就是要让MMA知道绝对值的自变量只是一个实数。所以我可以在求导之前，用`FullSimplify`配合`Assumptions->x \[Element] Reals `化简表达式，然后再求导。

这个方法也许行得通，但不普遍适用。我想上面第二条中介绍的方法应该是普遍适用的。