 
## Analysis

### Background


#### Git

#### GitHub

GitHub is hosting service for Git repositories, and the largest host of source code in the world. The Git version control system around which GitHub is based was developed by Linus Torvalds in 2005 and  is designed for nonlinear distributed development, whereby multiple developers work on multiple different tasks concurrently.

GitHub adds numerous features on top of the Git system.

##### Issues

Issues are a tracker system for ‘tasks, enhancements, and bugs’.
Each repository has its own Issues section. Within the issues section of a repository each issue has its own content. The screenshot below shows the issues section for a repository.

![Issues](http://imgur.com/wQieMo5.png)

The page is divided into two sections, open and closed.
An issue which is marked as open is one which has not yet been resolved and is either being investigated or to be investigated later.

When an issue is created, it must have a title explaining its purpose.
It is then assigned an automatically incremented number, and linked to the user who created the issue.

While the purpose of an issue may be clear from its title, larger projects are easier to manager when issues are separated into different categories.
This is achieved through labels. 
A label is a string of text and a background colour which may be applied to an issue, allowing it to be filtered.

Further, each issue may have up to 10 assignees.
An assignee is a user who has been designated to investigate an issue.
Filtering by assignees allows work to be more easily distributed, as well as identifying issues which are not yet under investigation.

In order to facilitate the investigation of an issue, each issue has a comments section.
The comments section consists of a mixed feed of comments and events as shown below

![Issue comments](http://imgur.com/06VkP61.png)

##### Pull requests

A pull request is effectively a subclass of issue.
A pull request is designed to notify users about changes which you have made to a repository.

As a pull request follows the issue model it has the same comments section as an issue.
The primary difference is that a pull request also references any number of commits which can be merged into the repository.

##### Projects

'Projects' were introduced in September 2016.
The aim of projects is to integrate the planning of a project and its features into the development process, as such projects are closely linked with the issue system.
Each repository may contain multiple projects, each of which could be used to plan a particular feature or release.

Each project has title and description, which are displayed in the projects section of a repository page.

![Projects](http://imgur.com/hGecHxh.png)

Since their release projects have been updated to include the same possible states as issues.

A project consists of multiple columns of content.
Each column has a title and can be reordered within the project.


![Project columns](http://imgur.com/9hn7pib.png)

Each item within a column is either a card, which is simply a string of markdown up to 250 characters in length, or a reference to an issue, which displays a link to the issue, its state, labels, and assignees.

#### Markdown








 



## Android Application Structure

### Activities

The Android developer documentation defines an```Activity``` as ‘a single, focused thing that the user can do.’.
The ```Activity``` class is responsible for creating the window in which an application's UI is placed.

When an ```Activity``` is launched, the Android system first calls the ```onCreate``` method, in which the ```Activity``` should create its view tree (if applicable) and perform any setup that it needs to.
Next the ```onStart``` method is called, making the ```Activity``` visible to the user.
Finally, the system calls ```onResume``` when the ```Activity``` enters the foreground. In this state the user can interact with the Activity.

The ```Activity``` will stay in the resumed state until it exits it, or another ```Activity```

![Activity lifecycle](https://developer.android.com/guide/components/images/activity_lifecycle.png)


### Intents

Activities are launched by Intents. 
An ```Intent``` is  ‘an abstract description of an operation to be performed’. ```Intent``` objects are used throughout the Android system to communicate both within apps, and between them.

Intents are messaging objects used to request an action from another application component, within the same app or a different one.

An example of a simple implicit Intent is to launch a messaging application to send a string of text.

```Java
// Create the text message with a string
final Intent sendIntent = new Intent();
sendIntent.setAction(Intent.ACTION_SEND);
sendIntent.putExtra(Intent.EXTRA_TEXT, "Our message");
sendIntent.setType("text/plain");

// Verify that there is an application to handle the Intent
if (sendIntent.resolveActivity(getPackageManager()) != null) {
    startActivity(sendIntent);
}
```

If the user has already chosen a default application for ```Intent.ACTION_SEND``` the Android system will launch the specified Activity, otherwise it will show a chooser dialog.


### Intent handling

In order for any ```Activity``` to be launched it must be declared in the application's manifest, with an item pointing to the ```Activity``` class.

``` XML
<activity android:name=".app.TestActivity" />
```

 Declaring the ```Activity``` in the manifest allows it to be launched, however to be launched from outside of the app the ```Activity``` must specify an Intent filter.
To specify that an```Activity``` is a ```MAIN``` ```Activity```, one which can function as an entry point for the app and does not require being launched with data, the ```MAIN``` intent filter must be registered.
Further, to be shown in the device’s app drawer, allowing the user to easily launch it, the ```Activity``` must register the ```LAUNCHER``` Intent in its Intent filter.

The simplest primary ```Activity```for an app has the following manifest registry:
``` XML
<activity
   android:name=".app.MainActivity">
   <intent-filter>
       <action android:name="android.intent.action.MAIN"/>
       <category android:name="android.intent.category.LAUNCHER"/>
   </intent-filter>
</activity>
```

This allows the app to be launched from the user's home-screen.


### Layouts

Layouts in Android apps are usually defined in XML.

The base class for an Android View is the ```View``` class in the ```android.view``` package.
```View``` directly extends ```Object``` and contains many methods and interfaces for laying out and drawing a the ```View``` on screen.
All individual ```Views```, for example to input text or display an image, extend ```View```. 

However, layouts would be difficult to work with if it were only possible to manipulate individual ```Views```.
A ```ViewGroup``` is a subclass of ```View``` which can contain other ```Views```, called children.
The ```ViewGroup``` class is the base class for all layouts and ```View``` containers.

One of the most commonly used ```ViewGroups``` is ```LinearLayout``` which can align other ```Views``` in a linear fashion either horizontally or vertically. This would be useful for setting out multiple Views one after another on a device screen.
There are also more complicated ```ViewGroups``` such as ```RelativeLayout``` which aligns its children relative to each other and itself.

#### Layout inflation

While ```View``` objects can be, and are, created at run time, it is usually much quicker to write, and more easily understandable to define the layout in a layout resource file.

A layout resource file is an XML file containing the ```View``` classes to be shown on screen and their attributes.

A simple login ```Activity``` might have the following layout

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="match_parent"
              android:layout_height="match_parent">

    <EditText
        android:id="@+id/user_name"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:ems="10"
        android:inputType="textPersonName"
        android:hint="Username"/>

    <EditText
        android:id="@+id/password"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:ems="10"
        android:inputType="textPassword"
        android:hint="Password"/>

    <Button
        android:id="@+id/button"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Login"/>
</LinearLayout>
```

Each ```View``` must have a defined width and height, however other values need not be specified.

While absolute values can be specified, such as the hint and text attributes in the example above, this is not recommended as it is more difficult to refactor and much more difficult to adapt to other languages and locales.

Instead attributes should be specified in the applications values directory.
The values directory is part of the resources directory, which also contains layout files and other resources such as images.
Hint text is specified in a file called strings.xml

```
<resources>
        <string name="hint_login_username">Username</string>
</resources>
```

This value can then be referenced elsewhere as 

```
<EditText
    android:id="@+id/user_name"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:ems="10"
    android:inputType="textPersonName"
    android:hint="@string/hint_login_username"/>
```

However the values are specified, the XML will then be used by the ```LayoutInflater``` class to create a tree of ```View``` objects with the specified attributes.

The attributes are passed as an ```AttributeSet``` object which is usually used to obtain a ```TypedArray``` which contains the resolved attributes.

When the layout above is inflated and set as the root layout in an ```Activity``` it will look like the image below

![Example layout](http://i.imgur.com/Sorbgys.png)



#### Dimensions

The example above used ```match_parent``` and ```wrap_content``` to define the dimensions of each ```View```.
These values refer to constants which, as their names suggest, inform the system to layout the ```Views``` either to fill the available space within their parent ```ViewGroup``` or to the space requested by the instance of ```View```.

In order to properly layout a collection of ```Views```, actual sizes must be specified.

```Views``` can be specified as having a particular dimension in pixels with the px unit, which simply draws the ```View``` across the requested number of pixels.

The three units which directly relate to a physical dimension are as follows
 - in Inches based on the size of the screen
 - pt 72nds of an inch based on the physical size of the screen 
 - mm Millimetres based on the size of the screen

In practice, none of the four units above are particularly useful because device dimensions and resolutions vary greatly.

There are therefore sizes independent of the size and resolution of the device:

 - dp or dip are density independent pixels based on the physical density (dots per inch) of the screen. Density independent pixels are relative to a screen with a density of 160 dots per inch. This allows a ```View``` to maintain its proportions on different resolutions and form factors.
 - sp are scale independent pixels which are similar to dp, but are also scaled with the user's font size. This unit is recommended for use with text in order to respect both the device dimensions and the users preference.

#### View binding

In order to interact with the ```View``` tree and provide a functional application, the application code must have access to the inflated ```View``` objects.
This is achieved by assigning ids to each view.
As shown in the the layout above, every view can have an id attribute. This is a string which must be unique to the current view tree. When the application is built, all of the attributes for each module in the application are collected into a single class named R, with subclasses for each of the resource types.

For a complete application the R class is usually thousands of lines

![Resources](http://imgur.com/DTSTWjv.png)

The id class contains the integer values of all the ids used throughout the app.

In our ```Activity``` we create member variables for the ```Views``` which we need to access.

```
package com.tpb.example;

import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.widget.Button;
import android.widget.EditText;

public class MainActivity extends AppCompatActivity {

    private EditText mUserNameEditor;
    private EditText mPasswordEditor;
    private Button mLoginButton;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```

After calling ```setContentView``` we can assign the inflated ```Views``` to these variables.

```
 setContentView(R.layout.activity_main);
 mUserNameEditor = (EditText) findViewById(R.id.user_name);
 mPasswordEditor = (EditText) findViewById(R.id.password);
 mLoginButton = (Button) findViewById(R.id.button);
```

While this method is part of the Android SDK, and the default way to access the ```View``` tree, it is verbose and quickly becomes difficult to read when accessing complex layouts.
The most common solution to this problem is ButterKnife.

ButterKnife is an annotation processor which generates the necessary lookups from annotations at build time.

The same ```Activity``` class using ButterKnife is as follows

```
@BindView(R.id.user_name) EditText mUserNameEditor;
@BindView(R.id.password) EditText mPasswordEditor;
@BindView(R.id.button) Button mLoginButton;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    ButterKnife.bind(this);
}
```

ButterKnife can also other resources 

```Java
@BindColor(R.color.colorPrimary) int primaryColor;
```

As well as assigning click listeners to ```Views``` 

```
@OnClick(R.id.button)
void onButtonClick() {
    //Do something
}
```

rather than 
```
mLoginButton.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        //Do something
    }
});
```

### Fragments

In all cases, at least one ```Activity``` must be used to display an application, however it is not always necessary, and often adds unnecessary complexity to use a full ```Activity``` for each component of the application.

A ```Fragment``` is 'a behaviour or a portion of user interface in ```Activity```'.
Multiple ```Fragments``` can be combined within an ```Activity``` to produce dynamic layouts on different devices, and allow easy reuse of components across different parts of an application.

A ```Fragment``` has its own lifecycle separate from its parent ```Activity```

![Fragment lifecycle](https://developer.android.com/images/fragment_lifecycle.png)

```Fragments``` are commonly used in ```ViewPagers``` to display multiple related sets of content within the same ```Activity```.

![enter image description here](https://lh3.googleusercontent.com/l3OtlZDW7JlczwHBPsPXJ7OpkXP1MfMH_WV2N017hGHyArpuMyatUYtU84oLHIr1p0g=h900-rw)

### RecyclerViews

The ```ListView``` class was added to Android in API level `.
As its name suggests a ```ListView``` is used to show a set of ```Views``` in a scrollable list.

The ```RecyclerView``` was added in API level 22 (February 2015) with the intention of replacing the ```ListView```.

The ```RecyclerView``` brought three key improvements over the ```ListView```

 1. In a ```RecyclerView``` ```Views``` are always reused as the user scrolls. When the ```RecyclerView``` is first drawn, it instantiates as many ```ViewHolder``` instances as are necessary to fill the screen and provide some padding before reusing these ```Views``` by binding the appropriate data to them rather than creating new instances. This reduces memory usage and stops unnecessary ```findViewById``` calls which are costly as they require traversing the ```View``` tree.
 2. Animations are decoupled from the individual ```Views``` and delegated to an ```ItemAnimator``` which provides default animations for common actions such as insertion and removal of items.
 3. The list itself is decoupled from its containing ```ViewGroup``` which allows much more complex  item layouts with ```LayoutManagers``` such as the ```StaggeredGridLayoutManager``` which can ```Views``` of different sizes as in Google Keep (Below).

 ![Google keep](https://lh3.googleusercontent.com/fRGLrIICjExK5VqM-dL7Q4kmKtcnpVH5nyl3mHYCDKTocxrvQ1rZqLs4zyEyLRb62aNA=h900-rw)

### Build system

Android uses the Gradle build system.
Gradle converts each of the files in the project to the necessary type to be packaged in an application, as well as performing minification and obfuscation if told to.
Java files are converted to dex files, and XML files are built into the required resource classes.

Gradle is a plugin based system which makes it particularly useful for Android development which development which often contains multiple languages other than Java, such as C and C++ through the native development kit, as well as other Java Virtual Machine based languages such as Kotlin and Scala which can replace Java entirely and provide many useful features not present in versions of Java present on older devices.

A build script for a near empty project may appear as follows

```
apply plugin: 'com.android.application'

android {
    compileSdkVersion 25
    buildToolsVersion "25.0.2"
    defaultConfig {
        applicationId "com.tpb.example"
        minSdkVersion 21
        targetSdkVersion 25
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    androidTestCompile('com.android.support.test.espresso:espresso-core:2.2.2', {
        exclude group: 'com.android.support', module: 'support-annotations'
    })
    compile 'com.android.support:appcompat-v7:25.3.0'
    compile 'com.jakewharton:butterknife:8.5.1'
    annotationProcessor 'com.jakewharton:butterknife-compiler:8.5.1'
    testCompile 'junit:junit:4.12'
}
```

This build script includes the Android support library, ButterKnife, and runners for the built in tests.
All applications have a debug build type, however this build script also specifies a release build type which would result in ProGuard being applied to obfuscate the source, and the test dependencies not being included.

More complicated scripts can also be included:
In order to determine when a bug was introduced, it is useful to have a different ```versionCode``` for each change made to the code.
If the Git version control system is being used, this can be achieved by checking the number of commits when the application is built.

This is done by executing a Git command to count the number of revisions.

```
def gitInfo() {
    def commitCount = 0
    def revision = "(unknown revision)"
    try {
        def stdout = new ByteArrayOutputStream()
        exec {
            commandLine 'git', 'rev-list', '--all', '--count'
            standardOutput stdout
        }
        commitCount = stdout.toString().trim().toInteger()
        stdout = new ByteArrayOutputStream()
        exec {
            commandLine 'git', 'rev-list', 'HEAD', '-n', '1'
            standardOutput stdout
        }
        revision = stdout.toString().trim()
    }
    catch (error) {
        println "Error: ${error}"
    }
    return [commitCount: commitCount, revision: revision]
}
```

The ```versionCode``` can then be written ```versionCode gitInfo().commitCount```.

The Android build system will generate an APK (Android Package Kit) which can be installed on a test device.

#### Debugging

Whether running their application on a physical device or an emulator, communication with the device is managed through ADB (Android Debugging Bridge).

Once a device has been connected via ADB the Logcat can be used for simple debugging.
The Logcat is used to print messages and exceptions and is usually used through the ```Log``` class.

The ```Log``` class provides 6 static methods for logging different levels of information.
Each method takes a ```TAG``` string which is used to identify the source of the log, and most methods have overloadings for logging strings as well as ```Throwable``` exceptions.

 - V (Verbose) The highest level of logging, generally used when debugging
 - I (Information) Used to report useful information, usually that an operation has completed successfully
 - D (Debug) Used for debugging purposes only, to track the flow of an application
 - W (Warning) Used to notify of unexpected behaviour
 - E (Error) Used to log an error which is known to have occurred. Often prints a stack trace.
 - WTF (What a terrible failure) Used to report an exception that should never happen.



## My Application Structure


package com.tpb.mdtext;

import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.support.v4.util.ArraySet;

import com.vdurmont.emoji.Emoji;
import com.vdurmont.emoji.EmojiManager;

import org.commonmark.Extension;
import org.commonmark.ext.gfm.strikethrough.Strikethrough;
import org.commonmark.ext.gfm.strikethrough.StrikethroughExtension;
import org.commonmark.ext.gfm.tables.TablesExtension;
import org.commonmark.node.Code;
import org.commonmark.node.FencedCodeBlock;
import org.commonmark.node.Image;
import org.commonmark.node.IndentedCodeBlock;
import org.commonmark.node.Node;
import org.commonmark.parser.Parser;
import org.commonmark.renderer.NodeRenderer;
import org.commonmark.renderer.html.HtmlNodeRendererContext;
import org.commonmark.renderer.html.HtmlNodeRendererFactory;
import org.commonmark.renderer.html.HtmlRenderer;
import org.commonmark.renderer.html.HtmlWriter;

import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.regex.Pattern;

/**
 * Created by theo on 24/02/17.
 */

public class Markdown {

    private Markdown() {
    }

    private static final List<Extension> extensions = Arrays
            .asList(TablesExtension.create(), StrikethroughExtension.create());
    private static final HtmlRenderer renderer =
            HtmlRenderer.builder()
                        .nodeRendererFactory(
                                new HtmlNodeRendererFactory() {
                                    @Override
                                    public NodeRenderer create(HtmlNodeRendererContext context) {
                                        return new BlockRenderer(
                                                context);
                                    }
                                })
                        .extensions(extensions)
                        .build();
    private static final Parser parser = Parser.builder().extensions(extensions).build();

    private static class BlockRenderer implements NodeRenderer {

        private final HtmlWriter html;
        private final HtmlNodeRendererContext context;
        private static final ArraySet<Class<? extends Node>> nodeTypes = new ArraySet<>();

        static { //Nodes to capture
            nodeTypes.add(FencedCodeBlock.class);
            nodeTypes.add(IndentedCodeBlock.class);
            nodeTypes.add(Code.class);
            nodeTypes.add(Strikethrough.class);
            nodeTypes.add(Image.class);
        }

        BlockRenderer(HtmlNodeRendererContext context) {
            this.context = context;
            this.html = context.getWriter();
        }

        @Override
        public Set<Class<? extends Node>> getNodeTypes() {
            return nodeTypes;
        }

        @Override
        public void render(Node node) {
            if(node instanceof FencedCodeBlock) {
                final FencedCodeBlock block = (FencedCodeBlock) node;
                // Greater than 8 lines of code
                if(instancesOf(block.getLiteral(), "\n") > 10) {
                    html.line();
                    html.tag("code");
                    html.raw(String.format("[%1$s]%2$s<br>", block.getInfo(),
                            block.getLiteral().replace(" ", "&nbsp;").replace("\n", "<br>")
                    ));
                    html.tag("/code");
                    html.tag("br");
                    html.line();
                } else {
                    html.tag("inlinecode");
                    if(block.getInfo() != null && !block.getInfo().isEmpty()) {
                        // TODO Highlight string
                    }
                    html.raw(block.getLiteral().replace("\n", "<br>").replace(" ", "&nbsp;"));
                    html.tag("/inlinecode");
                    html.tag("br");
                }
            } else if(node instanceof IndentedCodeBlock) {
                final IndentedCodeBlock block = (IndentedCodeBlock) node;
                html.tag("code");
                html.text(String.format("[%1$s]\u0002%2$s", "",
                        block.getLiteral().replace("\n", "<br>").replace(" ", "&nbsp;")
                ));
                html.tag("/code");
                html.tag("br");
            } else if(node instanceof Code) {
                html.tag("inlinecode");
                html.raw(((Code) node).getLiteral().replace(" ", "&nbsp;"));
                html.tag("/inlinecode");
                html.tag("br");
            } else if(node instanceof Strikethrough) {
                html.line();
                html.tag("s"); //Proper tag
                context.render(node.getFirstChild()); //Fully render the children
                html.tag("/s");
                html.line();
            } else if(node instanceof Image) {
                html.line();
                html.tag("br");
                html.raw("<img src=\"" + ((Image) node).getDestination() + "\">");
                //context.render(node);
                html.line();
            }
        }

        private static int instancesOf(@NonNull String s1, @NonNull String s2) {
            int last = 0;
            int count = 0;
            while(last != -1) {
                last = s1.indexOf(s2, last);
                if(last != -1) {
                    count++;
                    last += s2.length();
                }
            }
            return count;
        }
    }

    private static final Map<String, String> ESCAPE_MAP = new HashMap<>();

    static {
        ESCAPE_MAP.put("#", "&#35;"); //Hashes must be escaped first
        ESCAPE_MAP.put("@", "&#64;"); //Ignore tags and email addresses
        ESCAPE_MAP.put("<", "&#60;"); //Ignore html
        ESCAPE_MAP.put(">", "&#62;");
        ESCAPE_MAP.put("`", "&#96;"); //Code tags in titles
    }

    private static final Pattern ESCAPE_PATTERN = TextUtils.generatePattern(ESCAPE_MAP.keySet());

    /**
     * Escapes characters to stop parser mishandling them
     *
     * @param s The string to escape
     * @return String with #, @, <, and > replaced with their HTML codes
     */
    public static String escape(@Nullable String s) {
        return TextUtils.replace(s, ESCAPE_MAP, ESCAPE_PATTERN);
    }

    public static String fixRelativeLinks(@NonNull String s, String fullRepoName) {
        int next = s.indexOf("<img");
        while(next != -1) {
            int srcStart = s.indexOf("src=\"", next) + "src=\"".length();
            if(srcStart > "src=".length()) {
                int srcEnd = s.indexOf("\"", srcStart);
                if(srcEnd != -1) {
                    final String url = s.substring(srcStart, srcEnd);
                    int offset = -1;
                    if(url.startsWith("./")) offset = 2;
                    else if(url.startsWith("/")) offset = 1;
                    else if(!url.startsWith("http://") && !url.startsWith("https://")) offset = 0;
                    if(offset != -1) {
                        s =
                                s.substring(0, srcStart) +
                                        "https://raw.githubusercontent.com/" +
                                        fullRepoName +
                                        "/master/" + url.substring(offset) +
                                        s.substring(srcEnd);

                    }
                }
            }
            next = s.indexOf("<img", next + 1);
        }
        return s;
    }

    public static String parseMD(@NonNull String s) {
        return renderer.render(parser.parse(s));
    }

    public static String formatMD(@NonNull String s) {
        return formatMD(s, null);
    }

    public static String formatMD(@NonNull String s, @Nullable String fullRepoPath) {
        return formatMD(s, fullRepoPath, true);
    }

    public static String formatMD(@NonNull String s, @Nullable String fullRepoPath, boolean linkUsernames) {
        final StringBuilder builder = new StringBuilder();
        char p = ' ';
        char pp = ' ';
        final char[] chars = ("\n" + s).toCharArray();
        for(int i = 0; i < chars.length; i++) {
            if(linkUsernames && chars[i] == '@' && (p == ' ' || p == '\n')) {
                //Max username length is 39 characters
                //Usernames can be alphanumeric with single hyphens
                i = parseUsername(builder, chars, i);
            } else if(chars[i] == '-' && p == '-' && pp == '-') {
                //Full width bar
                builder.setLength(builder.length() - 2);
                builder.append("<hr></hr>");

            } else if(chars[i] == '#' && (p == ' ' || p == '\n') && fullRepoPath != null) {
                i = parseIssue(builder, chars, i, fullRepoPath);
            } else if(pp == '[' && (p == 'x' || p == 'X') && chars[i] == ']') {
                if(i - 4 >= 0 && chars[i - 4] == '-') {
                    builder.setLength(builder.length() - 4);
                    builder.append("<br>");
                } else {
                    builder.setLength(builder.length() - 2);
                }
                builder.append("\u2611");         //☑ ballot box with check
            } else if(p == '[' && chars[i] == ']') { //Closed box
                if(i - 4 >= 0 && chars[i - 4] == '-') {
                    builder.setLength(builder.length() - 3);
                    builder.append("<br>");
                } else {
                    builder.setLength(builder.length() - 1);
                }
                builder.append("\u2610"); //☐ ballot box
            } else if(pp == '[' && p == ' ' && chars[i] == ']') {//Open box
                if(i - 4 >= 0 && chars[i - 4] == '-') {
                    builder.setLength(builder.length() - 4);
                    builder.append("<br>");
                } else {
                    builder.setLength(builder.length() - 2);
                }
                builder.append("\u2610");
            } else if(chars[i] == '(') {
                builder.append("(");
                i = parseImageLink(builder, chars, i);
            } else if(pp == '`' && p == '`' && chars[i] == '`') {
                //We jump over the code block
                pp = ' ';
                p = ' ';
                int j = i;
                for(; j < chars.length; j++) {
                    builder.append(chars[j]);
                    if(pp == '`' && p == '`' && chars[j] == '`') {
                        i = j;
                        p = ' ';
                        break;
                    } else {
                        pp = p;
                        p = chars[j];
                    }
                }
            } else if(chars[i] == ':') {
                i = parseEmoji(builder, chars, i);
            } else {
                builder.append(chars[i]);
            }
            pp = p;
            p = chars[i];

        }
        return builder.toString();
    }

    /**
     * Formats a username as a link, appends it to a StringBuilder, and returns the position
     * after the username in cs
     *
     * @return The position after the username
     */
    private static int parseUsername(StringBuilder builder, char[] cs, int pos) {
        final StringBuilder nameBuilder = new StringBuilder();
        char p = ' ';
        for(int i = ++pos; i < cs.length; i++) {
            if(((cs[i] >= 'A' && cs[i] <= 'Z') ||
                    (cs[i] >= '0' && cs[i] <= '9') ||
                    (cs[i] >= 'a' && cs[i] <= 'z') ||
                    (cs[i] == '-' && p != '-')) &&
                    i - pos < 38 &&
                    i != cs.length - 1) {
                nameBuilder.append(cs[i]);
                p = cs[i];
                //nameBuilder.length() > 0 stop us linking a single @
            } else if((cs[i] == ' ' || cs[i] == '\n' || cs[i] == '\r' || i == cs.length - 1) && nameBuilder
                    .length() > 0) {
                if(i == cs.length - 1) {
                    nameBuilder.append(cs[i]); //Otherwise we would miss the last char of the name
                }
                builder.append("[@");
                builder.append(nameBuilder.toString());
                builder.append(']');
                builder.append('(');
                builder.append("https://github.com/");
                builder.append(nameBuilder.toString());
                builder.append(')');
                if(i != cs.length - 1) {
                    builder.append(cs[i]); // We still need to append the space or newline
                }
                return i;
            } else {
                builder.append("@");
                return --pos;
            }

        }
        builder.append("@");
        return --pos;
    }

    /**
     * Formats an Issue reference as a link, appends it to a StringBuilder, and returns the
     * position after the Issue reference in cs
     *
     * @return The position after the Issue number
     */
    private static int parseIssue(StringBuilder builder, char[] cs, int pos, String fullRepoPath) {
        final StringBuilder numBuilder = new StringBuilder();
        for(int i = ++pos; i < cs.length; i++) {
            if(cs[i] >= '0' && cs[i] <= '9' && i != cs.length - 1) {
                numBuilder.append(cs[i]);
            } else if(i > pos && (cs[i] == ' ' || cs[i] == '\n' || i == cs.length - 1)) {
                if(i == cs.length - 1) {
                    if(cs[i] >= '0' && cs[i] <= '9') {
                        numBuilder.append(cs[i]);
                    } else if(numBuilder.length() == 0) {
                        builder.append("#");
                        return --pos;
                    }
                }
                builder.append("[#");
                builder.append(numBuilder.toString());
                builder.append("]");
                builder.append("(");
                builder.append("https://github.com/");
                builder.append(fullRepoPath);
                builder.append("/issues/");
                builder.append(numBuilder.toString());
                builder.append(")");
                if(i != cs.length - 1) {
                    builder.append(cs[i]); // We still need to append the space or newline
                }
                return i;
            } else {
                builder.append("#");
                return --pos;
            }
        }
        builder.append("#");
        return --pos;
    }

    /*
    This function fixes positioning of text after images
    The TextView fucks up line spacing if there is text on the same line
    as an image, so if we find an image url we add a newline
     */
    private static int parseImageLink(StringBuilder builder, char[] cs, int pos) {
        for(int i = ++pos; i < cs.length; i++) {
            if(cs[i] == ')') {
                final String link = new String(Arrays.copyOfRange(cs, pos, i));
                final String extension = link.substring(link.lastIndexOf('.') + 1);
                if("png".equals(extension) ||
                        "jpg".equals(extension) ||
                        "gif".equals(extension) ||
                        "bmp".equals(extension) ||
                        "webp".equals(extension)) {
                    builder.append(link);
                    builder.append(") <br><br>");
                } else {
                    builder.append(link);
                    builder.append(")");
                }
                return i;
            }
        }

        return --pos;
    }

    private static int parseEmoji(StringBuilder builder, char[] cs, int pos) {
        final StringBuilder emojiBuilder = new StringBuilder();

        for(int i = pos + 1; i < cs.length; i++) {
            if((cs[i] >= 'A' && cs[i] <= 'Z') ||
                    (cs[i] >= '0' && cs[i] <= '9') ||
                    (cs[i] >= 'a' && cs[i] <= 'z') ||
                    cs[i] == '_' ||
                    cs[i] == '|') {
                emojiBuilder.append(cs[i]);
            } else if(cs[i] == ':') {
                final Emoji eww = EmojiManager.getForAlias(emojiBuilder.toString());
                if(eww == null) break;
                builder.append(eww.getUnicode());
                return i;
            } else {
                break;
            }
        }
        builder.append(cs[pos]);
        return pos;
    }

}
