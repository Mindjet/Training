# MVC/MVP/MVVM

## MVC
`Model-View-Controller`, aka `MVC`, is the most common framework in software development.

<img src="screenshot/mvc.png"/>

Take developing Android as an example. The `layout.xml` represents the `View`, the Java Bean stands for the `Model`, and `Activity` plays as `Controller` as it can `setOnClickListener` to the `View`, and change the `Model` directly.

Here comes 2 problem, however. One is that as `View`, `layout.xml` can hardly control itself, like it can't change its background. The other is that while `View` can self-control, the `Activity` play role as `View` and `Controller`, which makes it very heavy and inflexible.

There is another deadly problem. `View` and `Model` are both visible to each other and are coupled, which makes it hard to develop, test, and maintain.

That's why `MVP` and `MVVM` were born.

## MVP
As an advanced version of `MVC`, `MVP` has fixed many problems. 

The `View` is the same as the `View` in `MVC`, but `Activity` and `Fragment` aren't `Controller` anymore, they are pure `View` now. All the communications of `View` and `Model` are now done through `Presenter`.

The `View` and `Controller` are discoupled now, but it seems that the `View` and the `Presenter` are coupled. In fact, thay are not. They are communicating through `Interface`.

The best way to manage the `View` is `Fragment` playing the `View`, and `Activity` playing a controller(Presenter) to create or remove `Fragment`.

## MVVM