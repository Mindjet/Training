# Data-binding-3

## Preface
We all know that using `data-binding` and easily bind data to the widgets, but how about **BINDing a method**, is it possible?

The answer is YES, `data-binding` can do it!

## Trouble

When we use `Glide` to download an image from network and apply it to an `ImageView`, the code is usually like this:

```Java
ImageView imageView = (ImageView)findViewById(R.id.iv_xxx);
Glide.with(getActivity())
        .load(mMovieInfoList.get(position).url.large)
        .thumbnail(0.1f)
        .into(imageView);
```

I can't use `data-binding` above, because it makes me confused that if I use `data-binding`, there will be no `ImageView`, and there will be nothing I can pass to the method `Glide.xxx.into()`.

But the `data-binding` can still do the trick with `BindingAdapter`.

## BindingAdapter

>BindingAdapter is applied to methods that are used to manipulate how values with expressions are set to views. 

The sentence means we can attach method to the view.

Here is an example from [developer.android.com](https://developer.android.com/index.html):

```Java
@BindingAdapter("android:bufferType")
 public static void setBufferType(TextView view, TextView.BufferType bufferType) {
     view.setText(view.getText(), bufferType);
 }
 
//When android:bufferType is used on a TextView, the method setBufferType is called.
```

*You can check the [official document](https://developer.android.com/reference/android/databinding/BindingAdapter.html) to learn more about `BindingAdapter`.*

## Solution
Let's give it a shot.

Firstly, build a method with a random name, but make sure the ANNOTAION `@BindingAdapter` is added.

In the annotation, you can name the namespace and the attribute whatever you like, just make sure they are not too complex to be correctly written down. : )

Then you can do whatever you want in the method, like here, I download a photo from network with `Glide` and set it to the `ImageView`. You can use the data passed by the arguments. 

Technically speaking, the first argument must be a View or its subclass.

```Java
@BindingAdapter("glide:showImage")
public static void setImageByGlide(ImageView view, String imgUrl) {

    Glide.with(view.getContext())
            .load(imgUrl)
            .thumbnail(0.1f)
            .into(view);

}
```

Finally invoke this method in the widget:

```XML
<ImageView
    android:id="@+id/iv_fullscreen"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    glide:showImage="@{imgUrl}"/>
```

Bingo~! Run your app and see what happen.

---

You can check my repos [DataBinding-1](data-binding-1-en.md) and [DataBinding-2](data-binding-2.md) for more about `data-binding`.




