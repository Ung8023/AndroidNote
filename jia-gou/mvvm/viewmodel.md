### 说明
ViewModel用来存储和管理UI需要的数据。Google提供了一个ViewModel的实现，如果不喜欢，可以自己写。这里只介绍官方ViewModel。

### 使用ViewModel
在**Module**级别的**build.gradle**中添加：

```java
dependencies {
    ......
    implementation "android.arch.lifecycle:viewmodel:1.1.1"
}
```

#### 创建ViewModel类

1. 继承ViewModel

    ```java
    public class MyViewModel extends ViewModel {
        private MutableLiveData<List<User>> users;
        public LiveData<List<User>> getUsers() {
            if (users == null) {
                users = new MutableLiveData<List<Users>>();
                loadUsers();
            }
            return users;
        }
    
        private void loadUsers() {
            // Do an asynchronous operation to fetch users.
        }
    }
    ```

2. 继承自AndroidViewModel

    ```java
    public class MyViewModel extends AndroidViewModel {
        private MutableLiveData<List<User>> users;
        public LiveData<List<User>> getUsers() {
        if (users == null) {
        users = new MutableLiveData<List<Users>>();
        loadUsers();
        }
        return users;
        }
        
        private void loadUsers() {
        // Do an asynchronous operation to fetch users.
        }
    }
    ```