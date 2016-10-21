# RecyclerView-4
更多关于`RecyclerView`：  
- [RecyclerView(介绍与简单使用)](recycler-view-1.md)  
- [RecyclerView(布局)](recycler-view-2.md)  
- [RecyclerView(监听器)](recycler-view-3.md)

这一次主要是关于`ItemTouchHelper`实现拖拽、滑动的效果。

## ItemTouchHelper

>This is a utility class to add swipe to dismiss and drag & drop support to RecyclerView.

>It works with a RecyclerView and a Callback class, which configures what type of interactions are enabled and also receives events when user performs these actions.  
>
><p style="text-align:right">——Android Developer</p>

上面那句话是`Google`官方的介绍。

>ItemTouchHelper是一个工具类，协助RecyclerView来实现滑动删除、拖拽和摆放。
>
>ItemTouchHelper需要RecyclerView和Callback类。Callback类定义了被允许的交互类型，在用户进行这些交互操作时接受交互事件。

翻译过来差不多就是这样。  

`ItemTouchHelper`的实例化需要`ItemTouchHelper.Callback`作为参数：

```
mItemTouchHelper = new ItemTouchHelper(ItemTouchHelper.Callback callback);
```

## ItemTouchHelper.Callback

`ItemTouchHelper.Callback`是一个静态抽象类，一般重写的方法有下列几个：

### getMovementFlags
该方法规定了`item`拖拽和滑动的方向。

```
public int getMovementFlags(RecyclerView recyclerView, RecyclerView.ViewHolder viewHolder) {

    int dragFlags = 0, swipeFlags = 0;
    if (recyclerView.getLayoutManager() instanceof LinearLayoutManager) {

        dragFlags = ItemTouchHelper.UP | ItemTouchHelper.DOWN;
        swipeFlags = ItemTouchHelper.START | ItemTouchHelper.END;

    } else if (recyclerView.getLayoutManager() instanceof StaggeredGridLayoutManager) {

        dragFlags = ItemTouchHelper.UP | ItemTouchHelper.DOWN | ItemTouchHelper.LEFT | ItemTouchHelper.RIGHT;
        swipeFlags = ItemTouchHelper.START | ItemTouchHelper.END;
    }

    return makeMovementFlags(dragFlags, swipeFlags);
}
```

`dragFlags`和`swipeFlags`分别代表拖拽的方向和滑动的方向。  

在该方法中，根据`LayoutManager`的类型来设置`dragFlags`和`swipeFlags`。当`LayoutManager`为`LinearLayoutManager`时，设置`dragFlags`为上下滑动和`swipeFlags`为左右滑动。

方法返回类型为`int`，