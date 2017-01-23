# MultiImageViewLayout（朋友圈 图片列表）
## 一：前提：吐槽一下，可以忽略
#### 最近写了一个类似朋友圈的项目，拿到项目工程导入后傻眼了，之前的人三个月写了什么啊，气死了，显示几张图片的布局竟然用了一个Imageview和两个GridView控制显隐来达到，大小什么的显示问题多多，界面整体刷新还不是局部的，各种错误数据，等，此处省略十万字。。。。。于是开始了重写朋友圈之路，二手的东西真心不好接，先让我哭一会( ▼-▼ )。

## 二：说明

#### 我是将朋友圈分成了几个独立模块单独自定义的View，通过回调完成交互，耦合性算是非常低了，主要有以下及部分：多图展示区分单张图自适应，两列，三列显示，多图片数量没具体要求，最好别超过9个。 自定义布局用的LinearLayout的ddView等方法

1.评论布局（自定义TextView出门右转，处理了很多点击事件，改改就行了）

[Lu_comment_TextView](https://github.com/hnsugar/Lu_comment_TextView/)

[Lu_PingLunLayout](https://github.com/hnsugar/lu_pinglunlayout/)

2.点赞布局（原理和评论的自定义TextView一样，都是用的SpannableString）

[PraiseTextView](https://github.com/hnsugar/PraiseTextView/)
 
 
3.图片列表（当前工程是源码，理论上没有数量限制，和listView配合使用也很好，缓存也自己处理了）

[MultiImageViewLayout](https://github.com/hnsugar/MultiImageViewLayout/)

我也是找第三方例子不好找，于是自己写了一个，我和同事还打算做一套IM系统，app和后台都要做，我们自己定义sdk，我们还要封装H5，类似hbuilder如果有什么问题，可以联系我，

QQ:1264957104

![](https://github.com/hnsugar/MultiImageViewLayout/blob/master/jdfw.gif)
![](https://github.com/hnsugar/MultiImageViewLayout/blob/master/picture.png)
![](http://i.imgur.com/BDFkB82.png)
## 使用方法
#### 代码：  
##### 主要是 mMultiImageViewLayout.setList(mStrings)和 mMultiImageViewLayout.setOnItemClickListener


public class MainActivity extends Activity {
    public static final String[] PHOTOS = {
            "http://f.hiphotos.baidu.com/image/pic/item/faf2b2119313b07e97f760d908d7912396dd8c9c.jpg",
            "http://g.hiphotos.baidu.com/image/pic/item/4b90f603738da977c76ab6fab451f8198718e39e.jpg",
            "http://e.hiphotos.baidu.com/image/pic/item/902397dda144ad343de8b756d4a20cf430ad858f.jpg",
            "http://a.hiphotos.baidu.com/image/pic/item/a6efce1b9d16fdfa0fbc1ebfb68f8c5495ee7b8b.jpg",
            "http://b.hiphotos.baidu.com/image/pic/item/a71ea8d3fd1f4134e61e0f90211f95cad1c85e36.jpg",
            "http://c.hiphotos.baidu.com/image/pic/item/7dd98d1001e939011b9c86d07fec54e737d19645.jpg",
            "http://f.hiphotos.baidu.com/image/pic/item/f11f3a292df5e0fecc3e83ef586034a85edf723d.jpg",
            "http://pica.nipic.com/2007-10-17/20071017111345564_2.jpg",
            "http://pic4.nipic.com/20091101/3672704_160309066949_2.jpg",
            "http://pic4.nipic.com/20091203/1295091_123813163959_2.jpg",
            "http://pic2.ooopic.com/11/79/98/31bOOOPICb1_1024.jpg"};
    private List<String> mStrings = new ArrayList<String>();
    private MultiImageViewLayout mMultiImageViewLayout;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        for (int i = 0; i < PHOTOS.length; i++) {
            mStrings.add(PHOTOS[i]);
        }


        mMultiImageViewLayout = (MultiImageViewLayout) findViewById(R.id.multiimage);
        mMultiImageViewLayout.setList(mStrings);
         mMultiImageViewLayout.setOnItemClickListener(new MultiImageViewLayout.OnItemClickListener() {
            @Override
            public void onItemClick(View view, int PressImagePosition, float PressX, float PressY) {
                System.out.println("view = [" + view + "], PressImagePosition = [" + PressImagePosition + "], PressX = [" + PressX + "], PressY = [" + PressY + "]");
            }

            @Override
            public void onItemLongClick(View view, int PressImagePosition, float PressX, float PressY) {
                System.out.println("view = [" + view + "], PressImagePosition = [" + PressImagePosition + "], PressX = [" + PressX + "], PressY = [" + PressY + "]");
            }

        });
    }
}


#### 布局
<?xml version="1.0" encoding="utf-8"?>
<!--
  ~ Copyright (c) 2016. Lu Jianchao
  -->

<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.example.multiimageviewlayout.MainActivity">

    <com.example.multiimageviewlayout.MultiImageViewLayout
        android:layout_width="match_parent"
        android:id="@+id/multiimage"
        android:layout_height="wrap_content"></com.example.multiimageviewlayout.MultiImageViewLayout>
</RelativeLayout>

#### 资源
##### 使用的是Glide加载的图片，默认Glide会调用ImageView的Settag方法，所以我使用了两个参数的settag方法，保存了图片位置，Glide是可以改默认设置的，有兴趣的可以搜一下。如果使用ImageLoader等其他的，这个就不需要了，根据情况改吧。
<resources>
  <item type="id" name="FriendLife_Position"/>
</resources>
