# ViewModel

The **ViewModel** class is designed to store and manage UI-related data in a lifecycle conscious way. The ViewModel class allows data to survive configuration changes such as screen rotations.


- The Android framework manages the lifecycles of UI controllers, such as activities and fragments. The framework may decide to destroy or re-create a UI controller in response to certain user actions or device events that are completely out of your control.

- If the system destroys or re-creates a UI controller, any transient UI-related data you store in them is lost. For example, your app may include a list of users in one of its activities. When the activity is re-created for a configuration change, the new activity has to re-fetch the list of users. For simple data, the activity can use the onSaveInstanceState() method and restore its data from the bundle in onCreate(), but this approach is only suitable for small amounts of data that can be serialized then deserialized, not for potentially large amounts of data like a list of users or bitmaps.

- Another problem is that UI controllers frequently need to make asynchronous calls that may take some time to return. The UI controller needs to manage these calls and ensure the system cleans them up after it's destroyed to avoid potential memory leaks. This management requires a lot of maintenance, and in the case where the object is re-created for a configuration change, it's a waste of resources since the object may have to reissue calls it has already made.

- UI controllers such as activities and fragments are primarily intended to display UI data, react to user actions, or handle operating system communication, such as permission requests. Requiring UI controllers to also be responsible for loading data from a database or network adds bloat to the class. Assigning excessive responsibility to UI controllers can result in a single class that tries to handle all of an app's work by itself, instead of delegating work to other classes. Assigning excessive responsibility to the UI controllers in this way also makes testing a lot harder.

- It's easier and more efficient to separate out view data ownership from UI controller logic.

### ViewModel Dependency 

```gradle
dependencies {
    def lifecycle_version = "2.2.0"
    def arch_version = "2.1.0"

    // ViewModel
    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:$lifecycle_version"
    }
```
### ViewModel Architecture

![ViewModel Architecture](https://miro.medium.com/max/522/1*3Kr2-5HE0TLZ4eqq8UQCkQ.png)

### Example of ViewModel
Here we have a one beautiful example for to understand the viewmodel concept in easy way.
#### Application name : RandomNumberGenerator

- First create a new androidstudio project and name it as a **RandomNumberGenerator**
- Here you don't need to change the user interface. Default you will get one HelloWorld textview just add the below two attributes
    1.textsize 2.id
- After adding the above two attributes your **activity_main.xml** file looking below format. <br>

##### activity_main.xml
```xml

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FFFFFF"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        android:textSize="39sp"
        android:id="@+id/textview"
        android:textStyle="bold"
        android:textColor="#051A6B"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```



<img src ="https://github.com/mastan511/ViewModel/blob/master/Screenshot_2020-05-06-12-01-34-54_9fbf3a07fbf0be638fbcd6a96785d18f%5B1%5D.png?raw=true" width=200px height=300px>






##### MyViewModel.kt





```kotlin

import androidx.lifecycle.ViewModel

class MyViewModel : ViewModel(){
    var r:Int = 0
    init {
        r = (1..100).random()
    }
}

```

##### MainActivity.kt




```kotlin
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.TextView
import androidx.lifecycle.ViewModelProvider
import kotlin.random.Random

class MainActivity : AppCompatActivity() {

    lateinit var textView: TextView
    lateinit var myViewModel: MyViewModel
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        textView = findViewById(R.id.textview)
        myViewModel = ViewModelProvider(this).get(MyViewModel::class.java)
        textView.text = myViewModel.r.toString()

    }
}



```



