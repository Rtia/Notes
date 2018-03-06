# [详解Paint的setShader(Shader shader)](http://www.cnblogs.com/tianzhijiexian/p/4298660.html)

**一、概述**

setShader(Shader shader)中传入的自然是shader对象了，shader类是Android在图形变换中非常重要的一个类。Shader在三维软件中我们称之为着色器，其作用是来给图像着色。它有五个子类，像PathEffect一样，它的每个子类都实现了一种Shader。下面来看看文档中的解释：

子类：[BitmapShader](http://www.cnblogs.com/reference/android/graphics/BitmapShader.html), [ComposeShader](http://www.cnblogs.com/reference/android/graphics/ComposeShader.html), [LinearGradient](http://www.cnblogs.com/reference/android/graphics/LinearGradient.html), [RadialGradient](http://www.cnblogs.com/reference/android/graphics/RadialGradient.html), [SweepGradient](http://www.cnblogs.com/reference/android/graphics/SweepGradient.html)

 

**二、BitmapShader**

**2.1 构造方法**

只有有一个含参的构造方法：

> BitmapShader (Bitmap bitmap, Shader.TileMode tileX, Shader.TileMode tileY)

顾名思义，它是给bitmap做处理的类，传入的参数也有bitmap对象。从字面理解，传入的第一个参数是bitmap对象，应该会对bitmap做一定的处理，后面两个常量都是mode（模式），应该是设定处理效果的。理解了这个，我们就可以正式介绍下传入的三个参数了。

> 第一个参数：要处理的bitmap对象
>
> 第二个参数：在X轴处理的效果，Shader.TileMode里有三种模式：CLAMP、MIRROR和REPETA
>
> 第三个参数：在Y轴处理的效果，Shader.TileMode里有三种模式：CLAMP、MIRROR和REPETA

下面我们就来用代码进行各种模式的演示，演示之前自然要准备一个演示图片了：

![img](https://images0.cnblogs.com/blog/651487/201502/241226131891196.png)

 

说明：为了讲解需要，我给这个图片边界PS了几个像素的红色。

 

**2.2 Shader.TileMode.CLAMP**

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        
        Bitmap bitmap = BitmapFactory.decodeResource(mContext.getResources(), R.drawable.kale);
        // 设置shader
        mPaint.setShader(new BitmapShader(bitmap, Shader.TileMode.CLAMP, Shader.TileMode.CLAMP));  
        // 用设置好的画笔绘制一个矩形
        canvas.drawRect(180, 200, 600, 600, mPaint);
    }
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

效果：

![img](https://images0.cnblogs.com/blog/651487/201502/241228197522929.jpg)

从效果看，我们看不明白这是什么东西，只看到了大片的红色区域，左上角漏出了一个原图的小脚。我们索性把绘制区域放大，看看效果会有什么变化。

```
canvas.drawRect(0, 0, 800, 800, mPaint);
```

效果：

![img](https://images0.cnblogs.com/blog/651487/201502/241232477368557.jpg)

这下我们的图片终于完全显示了出来，仔细分析发现图片边界的红边是在的，但是为啥右边、下边都没有呢？因为我们设定的Shader.TileMode.CLAMP会将边缘的一个像素进行拉伸、扩展。所以整个的红色区域其实就是红色边框扩展后的结果。

 

**2.3 Shader.TileMode.MIRROR**

上面的例子我们知道CLAMP模式会拉伸边缘的一个像素来填充，可以说是边缘拉伸模式，那么这个MIRROR模式会有什么作用呢？顾名思义是镜像，那么就来测试一下。测试的代码就是从上面的改动的，仅仅把X轴的模式换成了MIRROR而已。

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        
        Bitmap bitmap = BitmapFactory.decodeResource(mContext.getResources(), R.drawable.kale);
        // 设置shader
        mPaint.setShader(new BitmapShader(bitmap, Shader.TileMode.MIRROR, Shader.TileMode.CLAMP));  
        // 用设置好的画笔绘制一个矩形
        canvas.drawRect(0, 0, 800, 800, mPaint);
    }
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

结果：

![img](https://images0.cnblogs.com/blog/651487/201502/241240258619431.jpg)

可见，在绘制的矩形区域内，X轴方向上出现了镜面翻转，就像翻牌子一样一个个翻开，和复印一样。而Y轴我们还是用的CLAMP，继续是拉伸边缘的红色像素，直到布满画布。

**注意：绘制过程是先采用Y轴模式，再使用X轴模式的。所以是先绘制一幅图片，先采用Y轴模式，向下拉伸了边缘的红色，然后采用X轴模式，将图片和拉伸的红色区域进行镜像翻转，再翻转。**

那么如果我们X,Y都用镜面效果呢？

```
mPaint.setShader(new BitmapShader(bitmap, Shader.TileMode.MIRROR, Shader.TileMode.MIRROR));  
```

![img](https://images0.cnblogs.com/blog/651487/201502/241245290025221.jpg)

 

**2.4 Shader.TileMode.REPEAT**

顾名思义，这个应该是重复模式，和镜像十分类似，但是不是翻转复制，而是平移复制。我们接着把上面镜像的代码的X轴模式进行修改：

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        
        Bitmap bitmap = BitmapFactory.decodeResource(mContext.getResources(), R.drawable.kale);
        // 设置shader
        mPaint.setShader(new BitmapShader(bitmap, Shader.TileMode.REPEAT, Shader.TileMode.MIRROR));  
        // 用设置好的画笔绘制一个矩形
        canvas.drawRect(0, 0, 800, 800, mPaint);
    }
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

结果：

![img](https://images0.cnblogs.com/blog/651487/201502/241249115962884.jpg)

可以明显的看到，第一层的图片都是一个个复制的，竖直方向上的图片是镜面翻转复制的。

**绘制过程：**

先在左上角绘制一个完整的图片，因为Y轴是用了镜像模式，所以翻转了原图，图片倒立了；第二次翻转了第二层第一列的一个图片，所以第三层的图片又变正了。接着，开始把第一列的图片进行平移复制，产生最终的结果。

**扩展：**

我们上面绘制的是一个矩形，绘制圆形也是一样的思路。需要明白的是这个setShader是画笔的属性，你用这个画笔绘制的区域就有这个效果，和你画圆的、方的都没有任何关系，如果你小时候玩过金山画王就能很好的理解了。

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        Bitmap bitmap = BitmapFactory.decodeResource(mContext.getResources(), R.drawable.kale);
        // 设置shader
        mPaint.setShader(new BitmapShader(bitmap, Shader.TileMode.REPEAT, Shader.TileMode.REPEAT));  
        // 用设置好的画笔绘制一个圆形
        //canvas.drawRect(0, 0, 800, 800, mPaint);
        canvas.drawCircle(300, 300, 300, mPaint);
    }
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

结果：

![img](https://images0.cnblogs.com/blog/651487/201502/241316180969845.jpg)

 

**三、LinearGradient**

**3.1 构造方法**

它就是一个线性渐变的处理类，有两个构造方法：

第一个：

> public LinearGradient (float x0, float y0, float x1, float y1, int color0, int color1, Shader.TileMode tile)

这是LinearGradient最简单的一个构造方法，参数虽多其实很好理解（x0，y0）表示渐变的起点坐标而（x1，y1）则表示渐变的终点坐标，这两点都是相对于屏幕坐标系而言的，而color0和color1则表示起点的颜色和终点的颜色。TileMode和上面讲的完全一致，不赘述了。

第二个：

> public LinearGradient (float x0, float y0, float x1, float y1, int[] colors, float[] positions, Shader.TileMode tile)

这个构造方法和第一个类似，坐标都是一样的，但这里的colors和positions都是数组，也就是说我们可以传入多个颜色和颜色的位置，产生更加丰富的渐变效果。

 

**3.2 用代码测试第一个效果**

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        Shader shader = new LinearGradient(0, 0, 400, 400, Color.RED, Color.YELLOW, Shader.TileMode.REPEAT);
        // 设置shader
        mPaint.setShader(shader);  

        canvas.drawRect(0, 0, 400, 400, mPaint);
    }
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

我们的画布是（0，0）-（400，400）的区域，LinearGradient的区域（执行渐变的区域）也是（0，0）-（400，400），开始的颜色是红色，结束的颜色是黄色，模式是重复模式。

结果：

![img](https://images0.cnblogs.com/blog/651487/201502/241335045495908.jpg)

这里我们的重复模式没有起作用，是因为渐变的区域正好等于画布绘制的区域，填充模式使用的前题是，填充的区域小于绘制的区域。就和用图片做桌面一样，如果图片大小大于等于桌面的大小，自然就不会出现平铺拉伸的效果了，如果是用小图做桌面，那么就要看看是怎么一个拉伸法。下面我稍作修改，把渐变的区域缩小一点，看看有什么不一样的效果：

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        Shader shader = new LinearGradient(0, 0, 200, 200, Color.RED, Color.YELLOW, Shader.TileMode.REPEAT);
        // 设置shader
        mPaint.setShader(shader);  
        canvas.drawRect(0, 0, 400, 400, mPaint);
    }
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

![img](https://images0.cnblogs.com/blog/651487/201502/241338284864570.jpg)

 

**3.3 用代码测试第二个效果**

第二个效果是传入不同渐变的颜色，然后设置颜色的显示位置，最后产生绚丽的渐变效果。这里我的渐变区域是小于绘制区域的，设置的模式是镜像模式。

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        Shader shader = new LinearGradient(0, 0, 200, 200, 
                new int[] { Color.RED, Color.YELLOW, Color.GREEN, Color.CYAN, Color.BLUE }, 
                new float[] { 0, 0.1F, 0.5F, 0.7F, 0.8F }, Shader.TileMode.MIRROR);
        // 设置shader
        mPaint.setShader(shader);  
        canvas.drawRect(0, 0, 400, 400, mPaint);
    }
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

colors是一个int型数组，我们用来定义所有渐变的颜色，positions表示的是渐变的相对区域，其取值只有0到1，上面的代码中我们定义了一个[0, 0.1F, 0.5F, 0.7F, 0.8F]，意思就是红色到黄色的渐变起点坐标在整个渐变区域（left, top, right, bottom定义了渐变的区域）的起点，而终点则在渐变区域长度 * 10%的地方，而黄色到绿色呢则从渐变区域10%开始到50%的地方以此类推，其中positions是可以为空的，但模式不能为null。

结果：

![img](https://images0.cnblogs.com/blog/651487/201502/241344015965437.jpg)

当我们的position为null时，颜色是均匀的填充整个渐变区域，显示的比较柔和。

```
Shader shader = new LinearGradient(0, 0, 200, 200, 
                new int[] { Color.RED, Color.YELLOW, Color.GREEN, Color.CYAN, Color.BLUE }, 
                null, Shader.TileMode.MIRROR);
```

结果：

![img](https://images0.cnblogs.com/blog/651487/201502/241347301586053.jpg)

 

**3.4 绘制图片阴影**

线性渐变有什么用呢？我相信美工肯定会给你一个很好的回答，总之它肯定很有用啦。我们下面的例子会演示用线性渐变绘制图片的倒影效果。

![img](https://images0.cnblogs.com/blog/651487/201502/241416140964714.jpg)

**思路：**

绘制倒影肯定要一个原图，然后我们拷贝一张进行矩阵翻转，放在它下面，作为倒影图。倒影图需要渐变消失，但bitmap没有提供渐变消失的效果，所以我们可以去尝试混合模式。既然用到了混合模式，就需要两个图片，一个图片是倒影图，还有一个肯定是渐变图了。倒影的渐变肯定是线性渐变，渐变是从有到无。

接着我们去挑选合适的混合模式，找到了DST_IN。

> ###### PorterDuff.Mode.DST_IN
>
> 计算方式：[Sa * Da, Sa * Dc]；
>
> 说明：只在源图像和目标图像相交的地方绘制目标图像

原图和渐变图相交的地方才绘制目标图像，所以目标图像是我们的倒影图，原图是线性渐变图。

**效果图：**

![img](https://images0.cnblogs.com/blog/651487/201502/241424062215177.jpg)  ![img](https://images0.cnblogs.com/blog/651487/201502/241424242687064.jpg)

**实现代码：**

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        // 为了突出效果，先绘制一个灰色的画布
        canvas.drawColor(Color.GRAY);
        
        int x = 200, y = 200;

        // 获取源图
        Bitmap bitmap = BitmapFactory.decodeResource(getResources(), R.drawable.kale);

        // 实例化一个矩阵对象
        Matrix matrix = new Matrix();
        matrix.setScale(1F, -1F);
        // 产生和原图大小一样的倒影图
        Bitmap refBitmap = Bitmap.createBitmap(bitmap, 0, 0, bitmap.getWidth(), bitmap.getHeight(), matrix, true);

        // 绘制好原图
        canvas.drawBitmap(bitmap, x, y, null);
        // 保存图层。这里保存的图层宽度是原图绘制区域的宽度，高度是原图绘制区域两倍的高度，包含了绘制倒影的区域。
        int sc = canvas.saveLayer(x, y + bitmap.getHeight(), x + bitmap.getWidth(), y + bitmap.getHeight() * 2, null, Canvas.ALL_SAVE_FLAG);
        
        
        
        
        
        // 绘制倒影图片，绘制的区域紧贴原图的底部
        canvas.drawBitmap(refBitmap, x, y + bitmap.getHeight(), null);
        
        // 设置好混合模式
        mPaint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.DST_IN));
        /*
         *  设置线性渐变模式。
         *  这里绘制的高度是原图的1/4，也就说倒影最终的区域也就是原图的1/4
         *  颜色是从Color.BLACK到透明，用于和倒影图做混合模式。
         *  模式是边缘拉伸模式，这里没用到
         */
        mPaint.setShader(new LinearGradient(x, y + bitmap.getHeight(), 
                x, y + bitmap.getHeight() + bitmap.getHeight() / 4, 
                Color.BLACK, Color.TRANSPARENT, Shader.TileMode.CLAMP));  
        // 画一个矩形区域，作为目标图片，用来做混合模式
        canvas.drawRect(x, y + bitmap.getHeight(), x + refBitmap.getWidth(), y + bitmap.getHeight() * 2, mPaint);

        mPaint.setXfermode(null);

        // 回复图层
        canvas.restoreToCount(sc);
    }
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

**四、SweepGradient**

它的意思是梯度渐变，也称之为扫描式渐变，因为其效果有点类似雷达的扫描效果，他也有两个构造方法：

**第一个：**

> SweepGradient(float cx, float cy, int color0, int color1)  

跟LinearGradient差不多，(cx,cy)是远行的坐标，产生从color0到color1的渐变。来一个实例：

```
mPaint.setShader(new SweepGradient(screenX, screenY, Color.RED, Color.YELLOW)); 
```

![img](https://images0.cnblogs.com/blog/651487/201502/241427153308136.jpg)

**第二个：**

> SweepGradient(float cx, float cy, int[] colors, float[] positions)  

实例代码：

```
mPaint.setShader(new SweepGradient(screenX, screenY, new int[] { Color.GREEN, Color.WHITE, Color.GREEN }, null));  
```

![img](https://images0.cnblogs.com/blog/651487/201502/241428323617635.jpg)

 

**五、RadialGradient**

径向渐变，径向渐变说的简单点就是个圆形中心向四周渐变的效果，他也一样有两个构造方法

**第一个：**

> RadialGradient (float centerX, float centerY, float radius, int centerColor, int edgeColor, Shader.TileMode tileMode)  

（centerX，centerY）是圆心的坐标，radius是半径，centerColor是边缘的颜色，edgeColor是外围的颜色，最后是模式。

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        Shader shader = new RadialGradient(200, 200, 200, Color.RED, Color.GREEN, Shader.TileMode.CLAMP);
        // 设置shader
        mPaint.setShader(shader);  
        canvas.drawRect(0, 0, 400, 400, mPaint);
    }
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

结果：

![img](https://images0.cnblogs.com/blog/651487/201502/241438529082789.jpg)

 

**第二个：**

> RadialGradient (float centerX, float centerY, float radius, int[] colors, float[] stops, Shader.TileMode tileMode) 

这个就是添加了多个色彩，设置色彩的位置，没啥特别的。

 

**六、ComposeShader**

就是组合Shader的意思，顾名思义就是两个Shader组合在一起作为一个新Shader，它有两个构造方法：

> ComposeShader (Shader shaderA, Shader shaderB, Xfermode mode)  
>
> ComposeShader (Shader shaderA, Shader shaderB, PorterDuff.Mode mode)  

两个都差不多的，只不过一个指定了只能用PorterDuff的混合模式而另一个只要是Xfermode下的混合模式都没问题！

你可以把两种渐变进行叠加，比如这样：

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);

         Shader shader01 = new RadialGradient(200, 200, 200, Color.RED, Color.GREEN, Shader.TileMode.CLAMP);
         Shader shader02 = new SweepGradient(200, 200, new int[] { Color.GREEN, Color.WHITE, Color.GREEN }, null);
                
        // 设置shader
        mPaint.setShader(new ComposeShader(shader01, shader02, PorterDuff.Mode.DARKEN));  
        
        canvas.drawRect(0, 0, 400, 400, mPaint);
    }
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

**七、Shader和Matrix**

**7.1 前言**

Shader是根据矩阵来绘制的，我们如果没有做任何设置，下面的代码会运行出这样的效果。

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
　　 @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        Bitmap bitmap = BitmapFactory.decodeResource(getResources(), R.drawable.kale);  
          
        // 实例化一个Shader  
        BitmapShader bitmapShader = new BitmapShader(bitmap, Shader.TileMode.CLAMP, Shader.TileMode.CLAMP); // 设置shader
        mPaint.setShader(bitmapShader);  
        
        canvas.drawRect(0, 0, 400, 400, mPaint);
    }
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

![img](https://images0.cnblogs.com/blog/651487/201502/241515020647534.jpg)

如果我们引入矩阵这个类，然后进行矩阵平移，看看能不能改变图片的位置。

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        Bitmap bitmap = BitmapFactory.decodeResource(getResources(), R.drawable.kale);  
          
        // 实例化一个Shader  
        BitmapShader bitmapShader = new BitmapShader(bitmap, Shader.TileMode.CLAMP, Shader.TileMode.CLAMP); 
        
     
        Matrix matrix = new Matrix();
        // 设置矩阵变换  
        matrix.setTranslate(20, 20);  
        // 设置Shader的变换矩阵  
        bitmapShader.setLocalMatrix(matrix);
        
        // 设置shader
        mPaint.setShader(bitmapShader);  
        
        canvas.drawRect(0, 0, 400, 400, mPaint);
    }
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

![img](https://images0.cnblogs.com/blog/651487/201502/241517111899052.jpg)

这说明我们可以改变绘制的初始点，如果没有做任何设定，那么绘制的初始点就是画布的左上角(0,0)。上面的代码我通过变换移改变了图片绘制的初始位置，产生了位置的偏移，效果也符合我们的预期。

如果我们最终绘制的区域和左上角有一段距离，那么就可能会出现文章开头的情形：

![img](https://images0.cnblogs.com/blog/651487/201502/241519293307548.jpg)

 

**7.2 用Matrix进行变换**

好了，现在我们正式引入Matrix。值得说明的是，Matrix可以做自身的各种变换，但：

注：除了平移外，缩放、旋转、错切、透视都是需要一个中心点作为参照的，如果没有平移，默认为图形的[0,0]点，平移只需要指定移动的距离即可，平移操作会改变中心点的位置！非常重要！记牢了！

**仅仅做平移：**

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
        Matrix matrix = new Matrix();
        // 设置矩阵变换  
        matrix.setTranslate(80, 80);  
        // 设置Shader的变换矩阵  
        bitmapShader.setLocalMatrix(matrix); 
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

结果：

![img](https://images0.cnblogs.com/blog/651487/201502/241526101892251.jpg)   平移后→   ![img](https://images0.cnblogs.com/blog/651487/201502/241526560803130.jpg)

 

 

**平移+旋转：**

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
　　　　　Matrix matrix = new Matrix();
        // 设置矩阵变换  
        matrix.setTranslate(80, 80);  
        matrix.setRotate(5);  
        // 设置Shader的变换矩阵  
        bitmapShader.setLocalMatrix(matrix);
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

结果：

![img](https://images0.cnblogs.com/blog/651487/201502/241526101892251.jpg)   平移+旋转后→   ![img](https://images0.cnblogs.com/blog/651487/201502/241530317993727.jpg)

我们发现，平移的效果不见了，仅仅有了旋转。为什么呢？

其实是这样的，在我们new了一个Matrix对象后，这个Matrix对象中已经就为我们封装了一组原始数据：

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
float[]{  
        1, 0, 0  
        0, 1, 0  
        0, 0, 1  
} 
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

我们的setXXX方法执行的操作是把原本Matrix对象中的数据重置，重新设置新的数据，比如：

```
matrix.setTranslate(500, 500);  
```

会变为：

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
float[]{  
        1, 0, 500  
        0, 1, 500  
        0, 0, 1  
}  
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

如果我们再设置了旋转？比如：

```
        matrix.setTranslate(500, 500);  
        matrix.setRotate(5);  

```

这样就会覆盖我们平移的数据，变成了这样：

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
float[]{  
        cos, sin, 0  
        sin, cos, 0  
        0, 0, 1  
} 
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

具体参数值我也就不计算了，我们知道了setxxx方法会重置数据，所以做变换的时候需要多多注意。从这里大家也可以看出Android给我们封装的方法是多么的体贴到位。你只需要setRotate个角度，即可压根就不需要你关心如何去算的。

 

**7.3 preXXX和postXXX方法**

preXXX和postXXX一个是前乘一个是后乘。我们知道Matrix是一个矩阵，而我们设置的参数也是一个矩阵，最终的结果肯定是这两个矩阵相互运算后得出的。

![img](https://images0.cnblogs.com/blog/651487/201502/241545233936207.jpg)

对于Matrix可以这样说：图形的变换实质上就是图形上点的变换，而我们的Matrix的计算也正是基于此，比如点P(x0,y0)经过上面的矩阵变换后会去到P(x,y)的位置。学过矩阵的就知道了，矩阵乘法是不能交换左右位置的，哪个矩阵在前面就很重要了。我们现在再来看这句话：

> preXXX和postXXX一个是前乘一个是后乘.setxxx是设置当前矩阵，不进行运算。

那么具体表现是什么样的呢？，非常简单！

 

**举例01:**

```
matrix.preScale(0.5f, 1);   
matrix.setScale(1, 0.6f);   
matrix.postScale(0.7f, 1);   
matrix.preTranslate(15, 0);  
```

我们来分析一下系统是怎么运算的。为了说明方便，我做如下规定：

> 把上面的代码，变为用两个数字组成的 [x,x] 运算对象。
>
> pre、post表示运算顺序。
>
> 遇到post的矩阵，就把这个矩阵放在已有矩阵后面进行乘法；
>
> 遇到pre，那么就把这个矩阵放到已有矩阵之前进行乘法。

那么，上面代码运算过程如下：

1.  [0.5f,1] * 原始矩阵 = 矩阵01　　（因为是pre，所以设置的值放在原始矩阵之前相乘）
2.  [1,0.6f] -> 矩阵01 = [1,0.6f] = 矩阵02　 （因为是set，所以不会进行运算，直接重置所有值）
3.  矩阵02 * [0.7f,1] = 矩阵03　　  （因为是post，所以把设置的值放在后面相乘）
4.  [15,0] * 矩阵03 = 最终结果　　  （因为是pre，所以把设置值放在矩阵之前相乘）

现在，把上面用等量代换就可以得到运算过程啦：

1. [1,0.6f]->（[0.5f,1]*原始矩阵） = [1,0.6f] = 矩阵02
2. [1,0.6f] * [0.7f,1] = 矩阵03
3. [15,0] * （[1,0.6f] * [0.7f,1]） = 最终结果

可见，计算过程即为：translate (15, 0) -> scale (1, 0.6f) -> scale (0.7f, 1)

因为set会重置数据，所以第一行的[0.5f,1]就没有效果了。

 

**举例02:**

```
matrix.preScale(0.5f, 1);   
matrix.preTranslate(10, 0);  
matrix.postScale(0.7f, 1);    
matrix.postTranslate(15, 0);  
```

上面代码运算过程如下：

1. [0.5f,1] * 原始矩阵 = 矩阵01　 （因为是pre，所以设置的值放在原始矩阵之前相乘）
2. [10,0] * 矩阵01 = 矩阵02　　  （因为是pre，所以不会进行运算，直接重置所有值）
3. 矩阵02 * [0.7f,1] = 矩阵03　　（因为是post，所以把设置的值放在后面相乘）
4. 矩阵03 * [15,0] = 最终结果　　（因为是post，所以把设置值放在矩阵之前相乘）

现在，把上面用等量代换就可以得到运算过程啦：

1. [10,0] * （[0.5f,1] * 原始矩阵） = 矩阵02
2. [10,0] * （[0.5f,1] * 原始矩阵） * [0.7f,1]= 矩阵03
3. （[10,0] * （[0.5f,1] * 原始矩阵） * [0.7f,1]）* [15,0] = 最终结果

其计算过程为：translate (10, 0) -> scale (0.5f, 1) -> scale (0.7f, 1) -> translate (15, 0)

 

有一个方法可以由自己去验证，Matrix有一个getValues方法可以获取当前Matrix的变换浮点数组，也就是我们之前说的矩阵：

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
/* 
 * 新建一个9个单位长度的浮点数组 
 * 因为我们的Matrix矩阵是9个单位长的对吧 
 */  
float[] fs = new float[9];  
  
// 将从matrix中获取到的浮点数组装载进我们的fs里  
matrix.getValues(fs);  
Log.d("Aige", Arrays.toString(fs));// 输出看看呗！  
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

 

本文的大部分内容来自爱哥的博客，本文对原文有少量的删减，全部代码由本人自行验证。记录在此，仅作学习笔记。

参考自：http://blog.csdn.net/aigestudio/article/details/41799811

**From AigeStudio（http://blog.csdn.net/aigestudio）Power by Aige ，尊重原作者，感谢作者的无私分享。**