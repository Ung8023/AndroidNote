### 说明

现在布局已经画好，数据也已经作为"形参"被表示在`Layout`中，那么接下来，就应该对`Binding`，进行操作。根据前面的工作原理图，也就是到了在`Activity或Fragment`中使用**DataBinding**的时候了。

### 创建Binding对象
前面`Layouts`中提到过，可以对`Binding`重新命名，如果没有重新命名，会按照默认规则自动生成，那个类就是我们要使用的类。获取`Binding`对象有集中种情况。

#### 第一种情况(在知道生成的Binding类的情况下)

```java
MyLayoutBinding binding = MyLayoutBinding.inflate(layoutInflater);
MyLayoutBinding binding = MyLayoutBinding.inflate(layoutInflater, viewGroup, false);
```

#### 第二种情况(如果Layout已经通过别的方式渲染完毕)

```java
MyLayoutBinding binding = MyLayoutBinding.bind(viewRoot);
```

#### 第三种情况(不能预先知道Binding,)
通过`DataBindingUtils`进行渲染.

```java
ViewDataBinding binding = DataBindingUtil.inflate(LayoutInflater, layoutId,
    parent, attachToParent);
ViewDataBinding binding = DataBindingUtil.bindTo(viewRoot, layoutId);
```

#### EG(示例):

1. 在Activity中使用:

```java
public class AActivity extends AppCompatActivity {
    ActivityABinding dataBinding;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        dataBinding = DataBindingUtil.setContentView(this, R.layout.activity_a);
    }
}
```

2. 在Fragment中使用:

```java
public class AFragment extends Fragment {
    private FragmentABinding mViewDataBinding;
    
    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        mViewDataBinding = DataBindingUtil.inflate(
                inflater, R.layout.fragment_a, container, false);
        return mViewDataBinding.getRoot();
    }
```

3. 在ListView的Adapter中使用

```java
public class HomeAdapter<T> extends BaseAdapter {

    private List<T> dataList;
    private @LayoutRes int layoutId;
    private int variableId;
    LayoutInflater inflater;

    public HomeAdapter(List<T> dataList, @LayoutRes int layoutId, int resId) {
        this.dataList = dataList;
        this.layoutId = layoutId;
        this.variableId = resId;
    }

    @Override
    public int getCount() {
        return dataList != null ? dataList.size() : 0;
    }

    @Override
    public Object getItem(int position) {
        return dataList.get(position);
    }

    @Override
    public long getItemId(int position) {
        return position;
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        ViewDataBinding dataBinding;
        if (inflater == null) {
            inflater = LayoutInflater.from(parent.getContext());
        }
        if (convertView == null) {
            dataBinding = DataBindingUtil.inflate(inflater, layoutId, parent, false);
        }else{
            dataBinding = DataBindingUtil.getBinding(convertView);
        }
        //variableId  对应的变量名
        dataBinding.setVariable(variableId, dataList.get(position));
        return dataBinding.getRoot();
    }
}
```