# Fresco

## Introduction
>Fresco is a powerful system for displaying images in Android applications.
>
>Fresco takes care of image loading and display, so you don't have to. It will load images from the network, local storage, or local resources, and display a placeholder until the image has arrived. It has two levels of cache; one in memory and another in internal storage.


## Import

The line below is supposed to added to the `build.gradle` of your module.

```Gradle
compile 'com.facebook.fresco:fresco:0.14.1'
```

## SimpleDraweeView
Here I use `SimpleDraweeView` first to show what `Fresco` can do.

### import namespace
First of all, import the namespace `fresco` in `xml`:

```XML
xmlns:fresco="http://schemas.android.com/apk/res-auto"
```

### use widget

```XML
<com.facebook.drawee.view.SimpleDraweeView
    android:id="@+id/sdv_fresco"
    android:layout_width="300dp"
    android:layout_height="300dp"/>
```

It's required to set `layout_width` and `layout_height` with specific number, and `wrap_content` is not permitted here. It is because when the download completes, the layout will be adjusted, and if the size of `placeholderImage` is not the same as the final image, the transition will be very sharp.

You can use `wrap_content`, only when you want to display the image in a 
specific aspect ratio.

```Java
<com.facebook.drawee.view.SimpleDraweeView
    android:id="@+id/sdv_fresco"
    android:layout_width="300dp"
    android:layout_height="wrap_content"
    fresco:viewAspectRatio="1.5"/>		<!-- it means width/height=1.5 -->
```

### initalize Fresco
In the method `onCreate`, before the statement `setContentView()`, add:

```Java
Fresco.initialize(this);
```

### specify uri
`Fresco` accepts uri in large range of formats, but make sure it's an absolute uri.

```Java
mSimpleDraweeView = (SimpleDraweeView) findViewById(R.id.sdv_fresco);
String url = "http://urlc.cn/XR5hy3";
mSimpleDraweeView.setImageURI(url);
```

Now run your app and see what happend!


## Polish widget

### static way
You can do it in a static way, in `xml` file.

```XML
<com.facebook.drawee.view.SimpleDraweeView
    android:id="@+id/sdv_fresco"			
    android:layout_width="300dp"
    android:layout_height="300dp"
    fresco:fadeDuration="3000"
    fresco:placeholderImage="@drawable/icon_retry"
    fresco:roundTopLeft="false"
    fresco:roundedCornerRadius="20dp"/>
```
See more attributes in [official document](https://www.fresco-cn.org/docs/using-drawees-xml.html).

### dynamic way
You can do it in a dynamic way, in `java` file, it relys on `GenericDraweeHierarchy`.

```Java
GenericDraweeHierarchyBuilder builder = new GenericDraweeHierarchyBuilder(getResources());
GenericDraweeHierarchy hierarchy = builder
        .setFadeDuration(1000)
        .setPlaceholderImage(R.drawable.icon_loading)
        .build();

mSimpleDraweeView.setHierarchy(hierarchy);
```

### outcome
<img src="../screenshots/fresco-radius.png" width="250"/>

