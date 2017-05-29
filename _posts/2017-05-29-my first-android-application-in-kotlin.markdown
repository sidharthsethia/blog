---
layout: post
title:  "My first Android application in Kotlin!"
date:   2017-05-29 08:10:46 +0530
categories: jekyll update
---
On May 17, at the Google I/O keynote, the Android team announced first-class support for Kotlin. For Android developers, Kotlin support is a chance to use a modern and powerful language, helping solve common headaches such as runtime exceptions and source code verbosity. Kotlin is easy to get started with and can be gradually introduced into existing projects, which means that your existing skills and technology investments are preserved. Hence, I decided to have my first hands on experience in Kotlin amidst all the buzz around it.

## Kotlin: An Overview

Kotlin is a statically-typed programming language that runs on the Java Virtual Machine and also can be compiled to source code. While the syntax is not compatible with Java, Kotlin is designed to interoperate with Java code and is reliant on Java code from the existing Java Class Library, such as the collections framework. Kotlin is to Java in Android which Swift is to Objective-C in iOS.

Kotlin variable declarations and parameter lists have the data type come after the variable name (and with a colon separator). As in Scala and Groovy, semicolons are optional as a statement terminator.

In addition to the classes and methods (called member functions in Kotlin) of object-oriented programming, Kotlin also supports procedural programming with the use of functions. As in C and C++, the entry point to a Kotlin program is a function named "main", which is passed an array containing any command line arguments. Perl and Unix/Linux shell script-style string interpolation is supported. Type inference is also supported

**Hello, world! example**

{% highlight Kotlin %}

fun main(args : Array<String>) { 
  val scope = "world"
  println("Hello, $scope!")
}

/**prints "Hello, World" to STDOUT*/

{% endhighlight %}

## Kotlin Camera App

I aimed to create a camera app in Kotlin. The app performs and the normal function of capturing images and saving them to Gallery.

### Getting Started

First, we need to install the Kotlin plugin from JetBrains

#### Step 1

Launch Android Studio and click on **Configure** in the window which appears. Then select **Plugins**

![Install Kotlin Plugin]({{site.url}}/{{site.baseurl}}/downloads/s1.png)



#### Step 2

Click on **Install JetBrains plugin..** and then search for **Kotlin** and click on **Install** to install the Kotlin Plugin.

![Install Kotlin Plugin]({{site.url}}/{{site.baseurl}}/downloads/s2.png)



### Configuring Android Studio with Kotlin

I used a camera app built by me as the base on which we will build the Kotlin app for the same. I launched my camera project in Android Studio. Now, I configured Android Studio with Kotlin

#### Step 1

Click on **Tools** in Android Studio. Then click on **Kotlin** and select on **Configure Kotlin in Project** 

![Install Kotlin Plugin]({{site.url}}/{{site.baseurl}}/downloads/s3.png)



#### Step 2

Select **Android with Gradle** as selecting only gradle can create issues when running an Android app.

![Install Kotlin Plugin]({{site.url}}/{{site.baseurl}}/downloads/s4.png)



#### Step 3

I selected **All modules** as I want to make the complete app in Kotlin only. After the configuration is complete, sync the project and we are good to go with Kotlin now.

![Install Kotlin Plugin]({{site.url}}/{{site.baseurl}}/downloads/s4.png)

The gradle files are updated and we can see the dependency for Kotlin being added.

```groovy
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    androidTestCompile('com.android.support.test.espresso:espresso-core:2.2.2', {
        exclude group: 'com.android.support', module: 'support-annotations'
    })
    compile 'com.android.support:appcompat-v7:24.2.1'
    compile 'id.zelory:compressor:1.0.4'
    compile 'com.android.support:design:24.2.1'
    testCompile 'junit:junit:4.12'
    compile "org.jetbrains.kotlin:kotlin-stdlib-jre7:$kotlin_version"
}
```

### Converting Java files to Kotlin

Now, the best part about Kotlin is that our existing java code can be directly converted to Kotlin while maintaining full runtime compatibility and thus we can use our existing Java code in our Kotlin app. 

I converted all my the java classes to Kotlin. While converting, the Translator raises some errors due to null value handling of Kotlin which one needs to take care of. Smart Cast is not always successful.

Lets take the example of my android activity class **ImagePreviewActivity.java**. Open the activity and then click on **Code** in the menu bar. Then select **Convert Java fike to Kotlin File**. The file will be converted to **ImagePreviewActivity.kt**. The translator will then raise any discrepancies in code due to conversion if any.

**ImagePreviewActivity.java snippet**

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_image_preview);

    imagePreview = (ImageView) findViewById(R.id.camera_preview);
    retakeButton = (ImageButton) findViewById(R.id.retake_button);
    rotateButton = (ImageButton) findViewById(R.id.rotate_button);
    doneButton = (ImageButton) findViewById(R.id.done);

    filePath = getIntent().getExtras().getString("FilePath");

    BitmapFactory.Options options = new BitmapFactory.Options();
    bitmap = BitmapFactory.decodeFile(filePath,options);
    imagePreview.setImageBitmap(bitmap);

    retakeButton.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            Intent intent=new Intent();
            intent.putExtra("action","retake");
            setResult(CameraActivity.CAMERA_PREVIEW_ACTIVITY,intent);
            finish();
        }
    });
}
```

**ImagePreviewActivity.kt  snippet**

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_image_preview)

    imagePreview = findViewById(R.id.camera_preview) as ImageView
    retakeButton = findViewById(R.id.retake_button) as ImageButton
    rotateButton = findViewById(R.id.rotate_button) as ImageButton
    doneButton = findViewById(R.id.done) as ImageButton

    filePath = intent.extras.getString("FilePath")

    val options = BitmapFactory.Options()
    bitmap = BitmapFactory.decodeFile(filePath, options)
    imagePreview!!.setImageBitmap(bitmap)

    retakeButton!!.setOnClickListener {
        val intent = Intent()
        intent.putExtra("action", "retake")
        setResult(CameraActivity.CAMERA_PREVIEW_ACTIVITY, intent)
        finish()
    }
}
```

I directly added a Kotlin activity just like we add an Android activity class. Just like the above code snippet, the activty was coded in Kotlin

### Build and Run

After all the activities were added, we normally build and run the code as we do with the java code. The kotlin package adds few KBs to the project but that's negligible.

## Kotlin: Features I liked

#### 100% Java Interoperability

This is another great feature about Kotlin, you can use Java code from a Kotlin file and vice-versa. In the Keddit App we are extending from AppCompatActivity, using the Bundle object in the onCreate method and of course using all the Android SDK to create our App :). Also, we are using other Java libraries like Retrofit, Picasso, etc.

#### Concise

Most of the verbosity of Java was eliminated. Concise code takes less time to write and to read so this will improve your productivity.

#### Null safety

In java, we have to always check for NullPointerException or null values. In Kotlin, it doesnot allow null values unless we specify explicitly.

#### No more findViewById

While there are a number of libraries, such as Butter Knife, that aim to remove the need for `findViewById`s, these libraries still require you to annotate the fields for each View, which can lead to mistakes and still feels like a lot of effort that would be better invested in other areas of your project.  

The Kotlin Android Extensions plugin (which has recently been incorporated into the standard Kotlin plugin) promises to make `findViewById` a thing of the past, offering you the benefits of the aforementioned libraries without the drawback of having to write any additional code or ship an additional runtime.

#### Lambdas

Functional programming is all the rage these days and Kotlin supports lambdas too.

Let’s start with a simple one. Suppose you have a list of integers and you want to remove all the odd elements:

```kotlin
list.filter { it % 2 == 1 }
```

The function here takes the element type (in this case integer), and outputs a boolean. If we break out the filter predicate into an explicit variable, it might be easier to understand:

```kotlin
val predicate: (Int) -> Boolean = { it -> 
    it % 2 == 1
}
list.filter(predicate)
```

This is similar to lambas in Python.

## Conclusion

The app worked perfectly fine in Kotlin. As in this project, majority of the existing code was converted to Kotlin, I look forward to developing Kotlin Apps from scratch and delve deeper into its concepts. Presently, I wont be comfortable bidding farewell to Java however may be after some more experience I choose Kotlin as my core language for android apps. With the growing popularity and support, one can arguably say that Kotlin may become the star performer for Android in future.

### Source

* [KotlinCamera](https://github.com/sidharthsethia/KotlinCamera) (GitHub)

### References

* [Kotlin, the Swift of Android](https://blog.gouline.net/kotlin-the-swift-of-android-d664f8997e7f)

* [Keddit — Part 2: Kotlin Syntax, Null Safety and more in Android](https://medium.com/@juanchosaravia/learn-kotlin-while-developing-an-android-app-part-2-e53317ffcbe9)

* [Coding Functional Android Apps in Kotlin: Getting Started](https://code.tutsplus.com/tutorials/start-developing-android-apps-with-kotlin-part-1--cms-27827)

* [Kotlin (programming language)](https://en.wikipedia.org/wiki/Kotlin_(programming_language))

  ​