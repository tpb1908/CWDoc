 
# Analysis

## Background


### Git

The Git version control system was developed by Linus Torvalds in 2005 and  is designed for nonlinear distributed development, whereby multiple developers work on multiple different tasks concurrently.

Git is used to store snapshots of a project, and store them as unique versions.
Git makes it easy both to rollback changes to a previous state, and to merge the project with changes made to it elsewhere.

#### Storage

Git acts as a content-addressable filesystem.
When content is inserted into the system, the return value is a key which can later be used to retrieve the content.

The key returned is a 40 character (160 bit) SHA-1 checksum of the content and its header.
The probability of a collision occurring across n unique objects is 0.5 * n<sup>2</sup> /2^160.
In order to achieve a 1% probability of collision 1.7x10<sup>23</sup> objects are required, which is highly improbable.

In order to store a file system structure, Git uses tree objects.
Each node in the tree is constructed of four elements:

- The unix file permissions
- A pointer to either a blob or another tree
- The hash of the object pointed to
- The filename


#### Repositories

Each project is stored in a repository.
A repository contains a set of commit objects, and a set of references to commit objects, called heads.

#### Commits

A commit object contains:
- A set of files, describing the project state at the time of the commit
- References to parent commit objects
- An SHA1 checksum which uniquely identifies the commit object

#### Heads

A head is a reference to a commit object.
Each head has a name, with the default name being 'master'.
A repository can contain any number of heads, but at any one time there is only a single current head.

#### Branches

Every branch has a head and every head has a branch. The only difference between the two is that while a branch refers to a head and the entire history of ancestor commits preceding it, a head is used to refer to a single commit object, the most recent commit in a branch.

When the user switches branches their current head pointer is changed to point to the branches head, and all of their working files are rewritten to match those at that head commit.

#### Merging

When work has been completed on one branch, the changes need to be brought into another branch in order to allow others to use the changes.

This is done by the merge command. 
When asked to merge a branch 'changed' into a branch 'other' Git:
- Finds the common ancestor of 'changed' and 'other'
- If the ancestor commit is equal to the head of 'other'
    - Updates 'other' by adding the commits which have been made to 'changed' since the common ancestor
    - Updates the head pointer of 'other' to point to the same commit as 'changed'
- Otherwise a full merge must be performed. This involves:
    - Determining the changes made between the common ancestor and the head of 'changed'
    - Attempt to merge these changes into 'current'
    - If the merge was successful:
        - Create a new commit with two parents 'changed' and 'other'
        - Set 'other' and the working head to point to this commit
    - Otherwise
        - Insert conflict markers into the file
        - Inform the user of the problem without creating a commit


### GitHub

GitHub is hosting service for Git repositories, and the largest host of source code in the world. 

GitHub adds numerous features on top of the Git system.

#### Issues

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

#### Pull requests

A pull request is effectively a subclass of issue.
A pull request is designed to notify users about changes which you have made to a repository.

As a pull request follows the issue model it has the same comments section as an issue.
The primary difference is that a pull request also references any number of commits which can be merged into the repository.

#### Projects

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


#### Integrations

Integrations are designed to extend GitHub's functionality.
An integration can be installed to a users account or to a single repository, allowing it hook access.

When an integration has hook access, it is notified of changes to the repository, which might be a new commit being pushed to the repository or an issue being created.

An integration could be used to automatically tag issues based on keywords in their text.

A more common use of integrations is to notify a continuous integration system.
Continuous integration is a practice where checked in code is verified against an automated build system, allowing the developer to be notified of a problem without manually running tests.

Continuous integrations are often used as checks on pull requests. If a check fails the pull request will not be merged.


### Markdown

Markdown is a markup language with plain text syntax.

Markdown doesn't have a common standard, however there are some universal rules.

```
# Heading level 1

## Sub-heading

### Another deeper heading
 
Paragraphs are separated
by a blank line.

Two spaces at the end of a line leave a  
line break.

Text attributes _italic_, *italic*, __bold__, **bold**, `monospace`.

Horizontal rule:

---

Bullet list:

  * item 1
  * item 2
  * item 3

Numbered list:

  1. item 1
  2. item 2
  3. item 3

A [link](http://example.com).
```

will be formatted as

# Heading level 1

## Sub-heading

### Another deeper heading
 
Paragraphs are separated
by a blank line.

Two spaces at the end of a line leave a  
line break.

Text attributes _italic_, *italic*, __bold__, **bold**, `monospace`.

Horizontal rule:

---

Bullet list:

  * item 1
  * item 2
  * item 3

Numbered list:

  1. item 1
  2. item 2
  3. item 3

A [link](http://example.com).

GitHub uses its own markdown structure with some extra features.

#### Lists

 Dashes can be used to create lists:

```
- Item 1
- Item 2
- Item 3
```

creates

- Item 1
- Item 2
- Item 3

Nested lists are also supported

```
- Item 1
  - Item 1 depth 2
  - Item 2 depth 2
- Item 2 
```

creates

- Item 1
  - Item 1 depth 2
  - Item 2 depth 2
- Item 2 

#### Images

Images can be inserted inline


```
![Image description](image_url)
```

should load and display the image

further, 

```
![Image description](relative_path)
```

should attempt to load the image from the current repository.

#### Blockquotes

Blockquotes are displayed in the same way as a HTML blockquote tag

```
> Some quoted text
> across 
> multiple lines
```

> Some quoted text  
> across  
> multiple lines

#### Strikethroughs

Placing two tildes on either side of a sequence will draw a stirkethrough through it

```
~~text~~
```

will be displayed as

~~text~~

#### References

Issue references

A # followed by the number of an issue within the repository will be shown as a link to that issue

User references

An @ followed by a username will be shown as a link to that user


#### Emoji

Emoji names wrapped in colons are converted to emoji characters

```
:camel:
```

is displayed as the emoji character 🐫


#### Code 

Code can be inserted between triple backquotes 

```

 ``` Language

 Some code

 ```

```

Will display the code without applying any other formatting

``` C
float Q_rsqrt( float number )
{
	long i;
	float x2, y;
	const float threehalfs = 1.5F;

	x2 = number * 0.5F;
	y  = number;
	i  = * ( long * ) &y;                       // evil floating point bit level hacking
	i  = 0x5f3759df - ( i >> 1 );               // what the fuck? 
	y  = * ( float * ) &i;
	y  = y * ( threehalfs - ( x2 * y * y ) );   // 1st iteration
//	y  = y * ( threehalfs - ( x2 * y * y ) );   // 2nd iteration, this can be removed

	return y;
}
```
#### HTML

The HTML tags which correspond to each of the markdown features can be used in place of the markdown characters.

There are also further tags which do not have a markdown equivalance:

The sup and sub tags display text as <sup>super</sup> and <sub>sub</sub> script respectively.

The font tag can be used to choose a text color and face

```<font color="red" face="impact">Formatted text</font>```

gives <font color="red" face="impact">Formatted text</font>


## Android Application Structure

### Activities

The Android developer documentation defines an ```Activity``` as ‘a single, focused thing that the user can do.’.
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

``` XML
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

``` XML
<resources>
        <string name="hint_login_username">Username</string>
</resources>
```

This value can then be referenced elsewhere as 

``` XML
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

``` Java
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

``` Java
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

```Java
@OnClick(R.id.button)
void onButtonClick() {
    //Do something
}
```

rather than 
``` Java
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

![enter image description here](http://imgur.com/GsPWoBA.png)

### RecyclerViews

The ```ListView``` class was added to Android in API level `.
As its name suggests a ```ListView``` is used to show a set of ```Views``` in a scrollable list.

The ```RecyclerView``` was added in API level 22 (February 2015) with the intention of replacing the ```ListView```.

The ```RecyclerView``` brought three key improvements over the ```ListView```

 1. In a ```RecyclerView``` ```Views``` are always reused as the user scrolls. When the ```RecyclerView``` is first drawn, it instantiates as many ```ViewHolder``` instances as are necessary to fill the screen and provide some padding before reusing these ```Views``` by binding the appropriate data to them rather than creating new instances. This reduces memory usage and stops unnecessary ```findViewById``` calls which are costly as they require traversing the ```View``` tree.
 2. Animations are decoupled from the individual ```Views``` and delegated to an ```ItemAnimator``` which provides default animations for common actions such as insertion and removal of items.
 3. The list itself is decoupled from its containing ```ViewGroup``` which allows much more complex  item layouts with ```LayoutManagers``` such as the ```StaggeredGridLayoutManager``` which can ```Views``` of different sizes as in Google Keep (Below).

 ![Google keep](http://imgur.com/WeVtSkc.png)


### Basic storage

While many applications use a variety of database solutions, it is not always necessary or the best solution to store simple values in a full database.

The ```SharedPreferences``` system is a persistent set of key value pairs which can be used to store application information such as settings.

Each 'preference' is a key value set stored under a particular name.
Once a ```SharedPreference``` object has been returned for the set, the key value pairs within the set can be read from and written to.


### Build system

Android uses the Gradle build system.
Gradle converts each of the files in the project to the necessary type to be packaged in an application, as well as performing minification and obfuscation if told to.
Java files are converted to dex files, and XML files are built into the required resource classes.

Gradle is a plugin based system which makes it particularly useful for Android development which development which often contains multiple languages other than Java, such as C and C++ through the native development kit, as well as other Java Virtual Machine based languages such as Kotlin and Scala which can replace Java entirely and provide many useful features not present in versions of Java present on older devices.

A build script for a near empty project may appear as follows

``` gradle
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

``` gradle
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






## Identification of the problem

In July 2012, GitHub released an offical app for Android.

# TODO Split frames from GIF

![GitHub app](https://cloud.githubusercontent.com/assets/391331/6142496/22c87a64-b16c-11e4-8715-b1d8c1d78359.gif)


GitHub later dropped support for their app in favour of a mobile website with severely limited features.
GitHub's mobile website allows viewing most important information about a repository, as well as creating comments on issues and commits, however it has no editing functionality for content which has already been created.
Some areas, such as the projects section, are entirely missing from the mobile website.

The source of the original app has been used to continue support for a number of similar apps, however they have the same limitations as the original app as they are built on a 5 year old codebase.
Further, the maintainers of some of the most used GitHub apps have recently dropped their support.

### Users

According to GitHub's 2016 statistics there are 5.8 million active GitHub users with over 19.4 million repositories.

As GitHub is designed for groups of developers to collaborate it is reasonable to assume that GitHub's users would make use of a method for keeping track of their projects without having access to a computer.


### Proposition and objectives

I will develop a GitHub client for Android to implement the features of the GitHub website.

In order to be more useful than GitHub's mobile website, and other clients, an Android client must implement most of the functionality of the desktop website.


The client must implement the following features:

<ol type="1">
    <li> Sign in 
        <ol type="a"> 
            <li>Allow the user to log in to GitHub with their credentials </li>
            <li>Store the authentication token received from GitHub </li>
        </ol>
    </li>
    <li> Users 
        <ol type="a"> 
            <li>Display available information about a user
                <ol type="i"> 
                    <li> Name </li>
                    <li> Avatar </li>
                    <li> Join date </li>
                    <li> Contributions in graphical form </li>
                    <li> Statistics on contributions </li>
                </ol>
            </li>
            <li> Repositories 
                <ol type="i">
                    <li> List the repositories that the user has created</li>
                    <li> List private repositories for the authenticated user </li>
                    <li> Display the repository primary language </li>
                    <li> Display the last time that the repository was updated </li>
                    <li> Display the number of users that have starred or forked the repository </li>
                    <li> Allow a user to pin a repository to the top of their repository list </li>
                </ol>
            </li>
            <li> Display the list of repositories that a user has starred</li>
            <li> Gists
                <ol type="i">
                    <li>Display the gists that the user has created </li>
                    <li>Display private gists for the authenticated user </li>
                </ol>
            </li>
            <li> Display the users that a user is following </li>
            <li> Display the users that are following a user </li>
            <li> Events 
                <ol type="i">
                    <li> Display the events relevant to a user </li>
                    <li> Display private events for the authenticated user </li>
                </ol>
            </li>
        </ol>
    </li>
    <li>Repositories
        <ol type="a">
            <li> Display information about the repository
                <ol type="i"> 
                    <li>Repository size</li>
                    <li>Number of issues </li>
                    <li>Number of forks </li>
                    <li>Number of stars </li>
                    <li>The repository license type </li>
                    <li>Display the text of the license</li>
                </ol>
             </li>
            <li>Repository files
                <ol type="i">
                    <li>Display the repository file tree</li>
                    <li>Implement viewing the file tree for different branches</li>
                    <li>Display files within the repository</li>
                </ol>
            </li>
            <li>Display the README in a suitable format for a mobile device </li>
            <li>Commits
                <ol type="i">
                    <li>List the commits made to a repository</li>
                    <li>Implement selecting the branch fo which to show commits</li>
                    <li>Display the user that made the commit, and the commit message</li>
                </ol>
            </li>
            <li>Issues
                <ol type="i">
                    <li>List the issues made on a repository</li>
                    <li>Display information about each issue
                        <ol type="1"> 
                            <li>The issue state</li>
                            <li>The issue number</li>
                            <li>The user that opened the user</li>
                            <li>The user that closed the issue (If applicable)</li>
                            <li>The date at which the issue was opened </li>
                            <li>The user(s) assigned to the issue</li>
                            <li>The tag(s) added to the issue </li>
                            <li>The number of comments made on the issue</li>
                        </ol>
                    </li>
                    <li>Implement filtering of the issues list
                        <ol type="1">
                            <li>By state 
                                <ol type="a"> 
                                    <li>Open</li>
                                    <li>Closed</li>
                                    <li>All</li>
                                </ol>
                            </li>
                            <li>By labels </li>
                            <li>By assigned user</li>
                        </ol>
                    </li>
                    <li>Implement searching of the issues list 
                        <ol type="1">
                            <li>Real time searching of the list</li>
                            <li>Fuzzy string matching when searching contents</li>
                        </ol>
                    </li>
                    <li>Implement toggling issue state</li>
                    <li>Implement editing of issues (See issues section)</li>
                    <li>Implement creation of issues (See issues section) </li>
                </ol>
            </li>
            <li>Projects
                <ol type="i">
                    <li>List the projects made on a repository</li>
                    <li> Display information about each project 
                        <ol type="1"> 
                            <li>Name</li>
                            <li>Description</li>
                            <li>State 
                                <ol type="a"> 
                                    <li>Open</li>
                                    <li>Closed</li>
                                </ol>
                            </li>
                            <li>Last date updated</li>
                        </ol>
                    </li>
                </ol>
            </li>
        </ol>
     </li>
     <li>Issues
        <ol type="a">
            <li>Display information about the issue
                <ol type="i">  
                    <li>Title</li>
                    <li>Number</li>
                    <li>State</li>
                    <li>Body</li>
                    <li>User that opened the issue</li>
                    <li>Date that the issue was opened</li>
                    <li> User(s) assigned to the issue </li>
                    <li> Labels added to the issue </li>
                    <li> Display the list of events which have occured on the issue
                        <ol type="1">
                            <li> Closed - When the issue was closed, and the commit if it was closed from a commit message </li>
                            <li> Reopened- When the issue was re-opened, and by whom </li>
                            <li> Subscribed- When a user subscribed to the issue, and who subscribed </li>
                            <li> Merged- When the issue (A pull request) was merged, the commit, and the user that merged the issue </li>
                            <li> Referenced- When the issue was referenced from a commit message, and the commit </li>
                            <li> Mentioned- When a user was mentioned in the body, and the user that was mentioned </li>
                            <li> Assigned- When a user was assigned to the issue, the user that was assigned, and by whom </li>
                            <li> Unassigned- When a user was unassinged from the issue, the user that was unassigned, and by whom </li>
                            <li> Labeled- When a label was added to the issue, and by whom </li>
                            <li> Unlabeled- When a label was removed from the issue, and by whom </li>
                            <li> Milestoned- When the issue was added to a milestone, and by whom </li>
                            <li> Demilestoned- When the issue was added to a milestone, and by whom </li>
                            <li> Renamed- When the issue was renamed, the old and new names, and the user that renamed the issue </li>
                            <li> Locked- When the issue was locked, and by whom </li>
                            <li> Unlocked- When the issue was unlocked, and by whom </li>
                            <li> Head ref deleted- When the pull request's branch was deleted </li>
                            <li> Head ref restored- When the pull request's branch was restored </li>
                            <li> Review requested- When a user was requested to review the pull request, and by whom </li>
                            <li> Review request dismissed- When a review request was dismissed, and by whom </li>
                            <li> Review request removed- When a request for a user to review the pull request was dismissed, and by whom </li>
                            <li> Added to project- When the issue was added a project board </li>
                            <li> Moved columns in project- When the issue was moved between columns in a project board</li>
                            <li> Removed from project- When the issue was removed from a project board </li>
                            <li> Converted note to issue- When the issue was created by conversion of a project note to an issue </li>
                        </ol>
                    </li>
                </ol>
            </li>
            <li> Comments
                <ol type="i"> 
                    <li>Display the list of comments on the issue 
                        <ol type="1">
                            <li>The user that created the comment</li>
                            <li>The date that the comment was created</li>
                            <li>The comment body </li>
                         </ol>
                    </li>
                    <li>Implement editing of comments made by the authenticated user </li>
                    <li>Implement creation of comments made by the authenticated user </li>
                </ol>
            </li>
            <li>Implement editing of
                <ol type="i">
                    <li>The issue title</li>
                    <li>The issue body</li>
                    <li>The issue assignee(s)</li>
                    <li>The issue label(s)</li>
                </ol>
            </li>
         </ol>
     </li>
     <li> Commits
        <ol type="a">
            <li>Display information about the commit
                <ol type="i">
                    <li>The commit message</li>
                    <li>The time that the commmit was created</li>
                    <li>The user that created the commit</li>
                    <li>The number of additions and deletions made in the commit</li>
                </ol>
            </li>
            <li>Diffs
                <ol type="i">
                    <li>List the changed files</li>
                    <li>Display information about the changed files
                        <ol type="1">
                            <li>The file state
                                <ol type="a"> 
                                    <li>Created</li>
                                    <li>Deleted</li>
                                    <li>Modified</li>
                                </ol>
                            </li>
                            <li>The number of additions and deletions</li>
                        </ol>
                    </li>
                    <li>Display and highlight the changed lines of code</li>
                </ol>
            </li>
            <li>Comments
                <ol type="i"> 
                    <li>Display the list of comments on the commit
                        <ol type="1"> 
                            <li>The user that created the comment</li>
                            <li>The date that the comment was created</li>
                            <li>The comment body </li>
                        </ol>
                    </li>
                    <li>Implemented editing of comments made by the authenticated user</li>
                    <li>Implement creation of comments by the authenticated user</li>
                </ol>
            </li>
            <li>Statuses 
                <ol type="i">
                    <li>Display the overall status for a commit</li>
                    <li>Display the integration that created a status</li>
                    <li>Link to the status information</li>
                </ol>
            </li>
        </ol>
     </li>
     <li>Projects
        <ol type="a">
            <li>Display information about each column
                <ol type="i">
                    <li>The column title</li>
                    <li>The last time that the column was updated</li>
                    <li>The number of cards in the column</li>
                </ol>
            </li>
            <li>Implement creation of new columns</li>
            <li>Implement deletion of columns</li>
            <li>Display cards for each column
                <ol type="i"> 
                    <li>For note cards display the note text</li>
                    <li>For issue cards display
                        <ol type="1"> 
                            <li>The issue title</li>
                            <li>The issue body</li>
                            <li>The issue state</li>
                            <li>The issue number</li>
                            <li>The number of comements on the issue</li>
                            <li>The user that opened the issue </li>
                            <li>The user that closed the issue (If applicable)</li>
                            <li>The user(s) that are assigned to the issue</li>
                            <li>The label(s) that are assigned to the issue </li>
                        </ol>
                    </li>
                </ol>
            </li>
            <li>Implement editing note cards text </li>
            <li>Implement deleting note cards </li>
            <li>Implement editing issue cards
                <ol type="i">
                    <li>Implement editing the issue (See issue section) </li>
                    <li>Implement toggling issue state</li>
                    <li>Implement removal of the issue card from the project</li>
                </ol>
            </li>
            <li>Implement creation of new cards
                <ol type="i">
                    <li>Implement creation of note cards with text limited to the correct length</li>
                    <li>Implement creation of new issue cards
                        <ol type="1">
                            <li>Create a new issue</li>
                            <li>Create a new card linked to the issue </li>
                        </ol>
                    </li>
                    <li> Implement creation of issue cards from pre-existing issues </li>
                </ol>
            </li>
            <li>Implement searching of project cards
                <ol type="i">
                    <li>Search text of cards and issues</li>
                    <li>Fuzzy text matching</li>
                    <li>Jump to and highlight the selected search item</li>
                </ol>
            </li>
        </ol>
     </li>
     <li>Link handling
        <ol type="a">
            <li>Handle all links through github.com</li>
            <li>Gracefully reject unsupported links by showing other apps which can handle the link</li>
            <li>Handle username links by opening the user</li>
            <li>Handle repository links and subsections
                <ol type="i">
                    <li>Handle repository links by opening the repository</li>
                    <li>Handle issues links by opening the issues section of the repository</li>
                    <li>Handle projects links by opening the projects section of the repository</li>
                    <li>Handle commits links by opening the commits section of the repository</li>
                </ol>
            </li>
            <li>Direct item links
                <ol type="i"> 
                    <li>Open individual issues from their numbers</li>
                    <li>Open individual files from their paths</li>
                    <li>Projects
                        <ol type="a">
                            <li>Open individual projects from their numbers</li>
                            <li>With a project, jump to a selected card from the id specified in a url</li>
                        </ol>
                    </li>
                </ol>
            </li>
        </ol>
     </li>
     <li>Notifications
        <ol type="a">
            <li>Run a background service to check for new notifications</li>
            <li>Allow the user to disable the notification service</li>
            <li>Only load notifications since the last time that they were loaded</li>
            <li>Display different icons and titles for different notification types
                <ol type="i">
                    <li> Assign notifications, when the user is assigned to an issue </li>
                    <li> Author notifications, when an update occurs on an item which the user created </li>
                    <li> Comment notifications, when an update occurs on a thread which the user commented on </li>
                    <li> Invitation notifications, when the user accepts an invitation to contribute to a repository </li>
                    <li> Manual notifications, when the user has manually subscribed to a thread </li>
                    <li> Mention notifications, when the user has been mentioned in the content of an item </li>
                    <li> State change notifications, when the user changes the state of a thread </li>
                    <li> Subscribed notifications, when a change happens to a repository that the user is subscribed to </li>
                </ol>
            </li>
        </ol>
     </li>
     <li>Markdown
        <ol type="a">
            <li>Parse markdown to an Android usable format </li>
            <li> Implement GitHub markdown specific features
                <ol type="i">
                    <li> Code blocks
                        <ol type="1">
                            Display short code blocks as monospaced text blocks within the text body
                        </ol>
                    <li>Parse strikethrougs and create the correct spans </li>
                    <li>Images
                        <ol type="1">
                            <li>Parse image links</li>
                            <li>Load the images asynchronously</li>
                            <li>Display the images in the text body, maintaining their aspect</li>
                            <li>Cache the images for later use</li>
                            <li>Make image clickable, to show images in a fullscreen dialog</li>
                        </ol>
                    </li>
                    <li>Username mentions
                        <ol type="1">
                            <li>Find username mentions in text body</li>
                            <li>Ensure that the following string matches the GitHub username format</li>
                            <li>If the username is valid, replace the username text with a link</li>
                        </ol>
                    </li>
                    <li>Issue references
                        <ol type="1">
                            <li>Find issue references in text body</li>
                            <li>Ensure that the following string is numeric</li>
                            <li>Replace the issue reference text with an issue link</li>
                        </ol>
                    </li>
                    <li>Emojis
                        <ol type="1">
                            <li>Find emoji name strings in the text body</li>
                            <li>If the emoji name is valid, replace it with the correct unicode character</li>
                        </ol>
                    </li>
                    <li>Checkboxes
                        <ol type="1">
                            <li>Find GitHub style checkboxes in the text body</li>
                            <li>Replace the checkboxes with the correct unicode characters</li>
                        </ol>
                    </li>
                    <li>When displaying background colours, choose a text colour which contrasts the label colour</li>
                </ol>
            </li>
            <li> Link handling
                <ol type="i">
                    <li>Match the same URIs that Android will normally find</li>
                    <li>Attempt to ignore code strings which may match a URI</li>
                </ol>
            </li>
            <li>Format lists with the required indentation levels
                <ol type="i">
                    <li>Support both ordered and unordered lists
                        <ol type="1">
                            <li>Correctly indent lists</li>
                            <li>Handle nested lists</li>
                        </ol>
                    </li>
                    <li>Support ordered list types
                        <ol type="1"> 
                            <li>Numbered lists</li>
                            <li>Alphabetic lists </li>
                            <li>Capitalised alphabetic lists</li>
                            <li>Roman numeral lists</li>
                            <li>Capitalised Roman numeral lists</li>
                        </ol>
                    </li>
                 </ol>
            </li>
        </ol>
     </li>
     <li>Markdown editing
        <ol type="a">
            <li>Implement toggling of a text editor between raw markdown and formatted markdown</li>
            <li>Add utility buttons for markdown features
                <ol type="i">
                    <li>Links</li>
                    <li>Bold text</li>
                    <li>Italic text</li>
                    <li>Strikethrough text</li>
                    <li>List items</li>
                    <li>Numbered list items</li>
                    <li>Quote blocks</li>
                </ol>
            </li>
            <li>Add further utility buttons
                <ol type="i">
                    <li>Images
                        <ol type="1">
                            <li>Allow the user to choose an image image from their device, take a photo, or type a link</li>
                            <li>Upload the image to a hosting service</li>
                            <li>Collect the image link and insert it into the text editor</li>
                        </ol>
                    </li>
                    <li>Code tags</li>
                    <li>Emoji insertion
                        <ol type="1">
                            <li>Display a list of possible emojis</li>
                            <li>Allow searching emojis by their names</li>
                            <li>Insert the chosen emoji text into the text editor</li>
                        </ol>
                    </li>
                    <li>Unicode insertion
                        <ol type="1">
                            <li>Display a list of possible unicode characters</li>
                            <li>Allow searching emojis by their names</li>
                            <li>Insert the chosen emoji into the text editor</li>
                        </ol>
                    </li>
                </ol>
            </li>
        </ol>
     </li>
</ol>

### Limitations

#### Processing power

Unlike the GitHub website, which has the benefit of running on more powerful devices, an Android app must perform the same tasks on a considerably less powerful device, without causing any inconvenience to the user.

One of the key problems will be to ensure that the user interface remains responsive.
By default, all logic will be run on the UI thread, which is usually the only thread which is allowed to modify a view, as only the original thread that created a view hierarchy can touch its views.

In order to stop the UI thread from being blocked by other operations, such as networking or markdown rendering, worker threads should be used to perform the calculations and then call back to the main thread to perform the UI update.

#### Screen size

Due to the limited screen size, and vertical orientation of mobile devices, some content which has been designed to be viewed on much larger landscape oriented screens may not display properly.

While not ideal, some content such as code and repository READMEs can be displayed such that they can be scrolled in two axis, as shown on the GitHub mobile website below:

![Code on GitHub mobile website](http://imgur.com/oJaiBFT.png)

#### API

There are some limitations to the GitHub API.

##### Rate limiting

For non authenticated requests to the GitHub API there is a rate limit of 60 requests per hour.
For content which must be loaded individually, this limit can be easily exceeded.

Requests authenticated by a user sign in have a limit of 5000 requests per hour, an amount which a user is unlikely to exceed.

In order to ensure that users do not consistently hit rate limits, they must sign in rather than being able to use the app anonymously.

##### Lack of endpoints

While the GitHub API provides almost all of the data required to implement the proposed solution there are some features of the GitHub website which cannot be easily duplicated.

Firstly, there is no API endpoint to find a user's pinned repositories.
The client can still replicate this feature by pinning repositories within the app.

Secondly, the API endpoint for contributions works by repository, rather than by user.
One possibility might be to use the search API to find repositories that a user has contributed to, and load the contributions for each of them before calculating the total number of contributions made by the user.
This is an awful idea as it will fail completely if a single request fails, as well as being unacceptably slow due to the number of requests made.

Instead, the contributions image can be loaded with a single request

![Contributions](http://imgur.com/4zp8SG2.png)

As the contributions image is an SVG containing the contributions information, it can be easily parsed to calculate the number of contributions made per day.
        

#### Lack of push API

The GitHub API is purely restful, all data is requested by the client and returned in a JSON format.

As there is no method for GitHub to notify the client of new notifications, the client must repeatedly poll the API for any updates to the user's notifications.

In order to reduce data usage, and to ensure that the impact on rate limiting is reduced, conditional requests can be used.
This is done by passing date-time string under the 'If-Modified-Since' header.
If the data has not been modified, the API will return a 304 Not Modified code and the request will not count against the rate limit.

### Proposed design

As explained in the background section, an Android app is made up of Activities and Fragments.

In order to implement the functionality listed above the app requires multiple activities to mimic the pages on the GitHub website.

The initial Activity should display the same information that a user would see when they navigated to their own GitHub page.

![GitHub user page](http://imgur.com/Szvqa9c.png)

This page can be easily adapted to a mobile layout as it is already paginated.

The root activity layout only needs to contain the navigation interface.
This is the toolbar which displays back and settings navigation, and the viewpager layout which displays the different page names.

![App user activity](http://imgur.com/BPBgFFA.png)

Within the viewpager for this Activity, there will be 6 pages.

The first should display information about the user and their activity

![User info fragment](http://imgur.com/2G9uwEz.png)

The second should list the user's repositories

A single repository is shown as a list item with the repository name, description, stars and forks, and a button to pin the repository.

![Repository viewholder](http://imgur.com/FARbi35.png)

The third lists the user's starred repositories, using the same list item layout as the user's own repositories without the pin button.

The fourth lists the user's Gists 


# Design

## Data structures and sources

Almost all of the data used in the application is acquired through the GitHub API

### Authentication

#### Basic authentication

Requests can be authenticated by sending the user's username and password in the request header, however this is not a an acceptable method of authentication for an Android client.

The first problem is that, while HTTPS requests are encrypted, the request itself is likely to be logged by the Android system, and may pass through another service if the user has a VPN active.

The second problem is that the user's account may require more than a password to authenticate.
GitHub supports two factor authentication.
If the user has activated two factor authentication, the two factor pin must be sent with each request.
This is not a usable experience as the pin changes each minute.

#### OAuth2 authentication

OAuth2 authentication allows applications to request authorization to a user's account without having access to their password.
This method also allows tokens to be limited to specific types of data, and can be revoked by the user.

In order to use the OAuth API, the application must be registered with GitHub.

![Application registration](http://imgur.com/EQu1SYh.png)

The application is registered with information recognisable to the user to ensure their trust when authorizing the application.
The callback URL is the URL which GitHub redirects to once the authorization is complete.

##### Web authentication flow

1. Redirect users to request access

Display the webpage ```https://github.com/login/oauth/authorize```

In order to successfully authenticate we must also pass parameters with the request.
The only required parameter is the client id which was received when the application was registered.

The scope parameter is used to specify what level of access is required to the user's account.

2. Redirect

If the user signs in and accepts the authorization request, GitHub redirects back to your site with a temporary parameter.

The ```code``` parameter has a limited timeframe to be exchanged for an authorization token.

This is done by posting to ```https://github.com/login/oauth/access_token```.

The post request must have three parameters:

1. The client id, which must match that provided when loading the authorization page
2. The client secret
3. The code received in 

If the request is successful the response will be a string containing the access token in the form:

``access_token=some_base_64_string&scope=the_scope_requested_in_step_1``.

3. Once the access token has been received it can be used for authorization by including the authorization header with each request.



##### Implementation of the authorization flow

In order to avoid duplication of values used throughout the GitHub API, I have used a single abstract class to contain the headers and path keys used throughout the project

``` java
package com.tpb.github.data;

import android.content.Context;
import android.support.annotation.Nullable;
import android.support.annotation.StringRes;

import com.androidnetworking.error.ANError;
import com.tpb.github.R;
import com.tpb.github.data.auth.GitHubSession;

import java.util.HashMap;

/**
 * Created by theo on 18/12/16.
 */

public abstract class APIHandler {
    static final String TAG = APIHandler.class.getSimpleName();

    protected static final String GIT_BASE = "https://api.github.com";
    private static final String ACCEPT_HEADER_KEY = "Accept";
    private static final String ACCEPT_HEADER = "application/vnd.github.v3+json";
    private static final String ORGANIZATIONS_PREVIEW_ACCEPT_HEADER = "application/vnd.github.korra-preview";
    private static final String PROJECTS_PREVIEW_ACCEPT_HEADER = "application/vnd.github.inertia-preview+json";
    private static final String REPO_LICENSE_PREVIEW_ACCEPT_HEADER = "application/vnd.github.drax-preview+json";
    private static final String PAGES_PREVIEW_ACCEPT_HEADER = "application/vnd.github.mister-fantastic-preview+json";
    private static final String AUTHORIZATION_HEADER_KEY = "Authorization";
    private static final String AUTHORIZATION_TOKEN_FORMAT = "token %1$s";
    private static GitHubSession mSession;

    protected static final HashMap<String, String> API_AUTH_HEADERS = new HashMap<>();
    static final HashMap<String, String> PROJECTS_API_API_AUTH_HEADERS = new HashMap<>();
    static final HashMap<String, String> ORGANIZATIONS_API_AUTH_HEADERS = new HashMap<>();
    static final HashMap<String, String> LICENSES_API_API_AUTH_HEADERS = new HashMap<>();
    static final HashMap<String, String> PAGES_API_API_AUTH_HEADERS = new HashMap<>();

    protected static final String SEGMENT_USER = "/user";
    static final String SEGMENT_USERS = "/users";
    static final String SEGMENT_REPOS = "/repos";
    static final String SEGMENT_README = "/readme";
    static final String SEGMENT_COLLABORATORS = "/collaborators";
    static final String SEGMENT_LABELS = "/labels";
    static final String SEGMENT_PROJECTS = "/projects";
    static final String SEGMENT_COLUMNS = "/columns";
    static final String SEGMENT_ISSUES = "/issues";
    static final String SEGMENT_PERMISSION = "/permission";
    static final String SEGMENT_CARDS = "/cards";
    static final String SEGMENT_MOVES = "/moves";
    static final String SEGMENT_COMMENTS = "/comments";
    static final String SEGMENT_EVENTS = "/events";
    static final String SEGMENT_STARRED = "/starred";
    static final String SEGMENT_SUBSCRIPTION = "/subscription";
    static final String SEGMENT_MILESTONES = "/milestones";
    static final String SEGMENT_GISTS = "/gists";
    static final String SEGMENT_FOLLOWING = "/following";
    static final String SEGMENT_FOLLOWERS = "/followers";
    static final String SEGMENT_COMMITS = "/commits";
    static final String SEGMENT_NOTIFICATIONS = "/notifications";

    protected APIHandler(Context context) {
        if(mSession == null) {
            mSession = GitHubSession.getSession(context);
            initHeaders();
        }
    }

    protected final void initHeaders() {
        final String accessToken = mSession.getAccessToken();
        API_AUTH_HEADERS.put(AUTHORIZATION_HEADER_KEY,
                String.format(AUTHORIZATION_TOKEN_FORMAT, accessToken)
        );
        API_AUTH_HEADERS.put(ACCEPT_HEADER_KEY, ACCEPT_HEADER);

        ORGANIZATIONS_API_AUTH_HEADERS.put(AUTHORIZATION_HEADER_KEY,
                String.format(AUTHORIZATION_TOKEN_FORMAT, accessToken)
        );
        ORGANIZATIONS_API_AUTH_HEADERS.put(ACCEPT_HEADER_KEY, ORGANIZATIONS_PREVIEW_ACCEPT_HEADER);

        PROJECTS_API_API_AUTH_HEADERS.put(AUTHORIZATION_HEADER_KEY,
                String.format(AUTHORIZATION_TOKEN_FORMAT, accessToken)
        );
        PROJECTS_API_API_AUTH_HEADERS.put(ACCEPT_HEADER_KEY, PROJECTS_PREVIEW_ACCEPT_HEADER);

        LICENSES_API_API_AUTH_HEADERS.put(AUTHORIZATION_HEADER_KEY,
                String.format(AUTHORIZATION_TOKEN_FORMAT, accessToken)
        );
        LICENSES_API_API_AUTH_HEADERS.put(ACCEPT_HEADER_KEY, REPO_LICENSE_PREVIEW_ACCEPT_HEADER);

        PAGES_API_API_AUTH_HEADERS.put(AUTHORIZATION_HEADER_KEY,
                String.format(AUTHORIZATION_TOKEN_FORMAT, accessToken)
        );
        PAGES_API_API_AUTH_HEADERS.put(ACCEPT_HEADER_KEY, PAGES_PREVIEW_ACCEPT_HEADER);
    }

    private static final String CONNECTION_ERROR = "connectionError";

    public static final int HTTP_OK_200 = 200; //OK

    public static final String HTTP_REDIRECT_NEW_LOCATION = "Location";
    public static final int HTTP_301_REDIRECTED = 301; //Should redirect through the value in location

    public static final int HTTP_302_TEMPORARY_REDIRECT = 302; //Redirect for this request only
    public static final int HTTP_307_TEMPORARY_REDIRECT = 307; //Same as above

    private static final int HTTP_BAD_REQUEST_400 = 400; //Bad request problems parsing JSON

    public static final String KEY_MESSAGE = "message";
    private static final String MESSAGE_BAD_CREDENTIALS = "Bad credentials";
    private static final int HTTP_UNAUTHORIZED_401 = 401; //Login required, account locked, permission error

    private static final String MESSAGE_MAX_LOGIN_ATTEMPTS = "Maximum number of login attempts exceeded.";

    public static final String KEY_HEADER_RATE_LIMIT_RESET = "X-RateLimit-Reset";
    private static final String MESSAGE_RATE_LIMIT_START = "API rate limit exceeded";
    private static final String MESSAGE_ABUSE_LIMIT = "You have triggered an abuse detection mechanism";
    private static final int HTTP_FORBIDDEN_403 = 403; //Forbidden server locked or other reasons

    private static final int HTTP_NOT_FOUND_404 = 404;

    private static final int HTTP_NOT_ALLOWED_405 = 405; //Not allowed (managed server)

    private static final int HTTP_409 = 409; //Returned when loading commits for empty repo

    private static final int HTTP_419 = 419; //This function can only be executed with an CL-account

    public static final String ERROR_MESSAGE_UNPROCESSABLE = "Validation Failed";
    public static final String ERROR_MESSAGE_VALIDATION_MISSING = "missing";
    public static final String ERROR_MESSAGE_VALIDATION_MISSING_FIELD = "missing_field";
    public static final String ERROR_MESSAGE_VALIDATION_INVALID = "invalid";
    public static final String ERROR_MESSAGE_VALIDATION_ALREADY_EXISTS = "already_exists";
    private static final String ERROR_MESSAGE_EMPTY_REPOSITORY = "Git Repository is empty.";
    private static final int HTTP_UNPROCESSABLE_422 = 422; // Validation failed

    //600 codes are server codes https://github.com/GleSYS/API/wiki/API-Error-codes#6xx---server

    //700 codes are ip errors https://github.com/GleSYS/API/wiki/API-Error-codes#7xx---ip

    //800 codes are archive codes https://github.com/GleSYS/API/wiki/API-Error-codes#8xx---archive

    //900 domain https://github.com/GleSYS/API/wiki/API-Error-codes#9xx---domain

    //1000 email https://github.com/GleSYS/API/wiki/API-Error-codes#10xx---email

    //1100 livechat https://github.com/GleSYS/API/wiki/API-Error-codes#11xx---livechat

    //1200 invoice https://github.com/GleSYS/API/wiki/API-Error-codes#11xx---livechat

    //1300 glera https://github.com/GleSYS/API/wiki/API-Error-codes#13xx---glera

    //1400 transaction https://github.com/GleSYS/API/wiki/API-Error-codes#14xx---transaction

    //1500 vpn https://github.com/GleSYS/API/wiki/API-Error-codes#15xx---vpn

    public static final int GIT_LOGIN_FAILED_1601 = 1601; //Login failed

    public static final int GIT_LOGIN_FAILED_1602 = 1602; //Login failed unknown

    public static final int GIT_GOOGLE_AUTHENTICATOR_OTP_REQUIRED_1603 = 1603; //Google auth error

    public static final int GIT_YUBIKEY_1604 = 1604; //Yubikey OTP required

    public static final int GIT_NOT_LOGGED_IN_1605 = 1605; //Not logged in as user

    //1700 invite https://github.com/GleSYS/API/wiki/API-Error-codes#17xx---invite

    //1800 test account https://github.com/GleSYS/API/wiki/API-Error-codes#18xx---test-account

    //1900 network https://github.com/GleSYS/API/wiki/API-Error-codes#19xx---network

    static APIError parseError(ANError error) {
        APIError apiError;
        if(CONNECTION_ERROR.equals(error.getErrorDetail())) {
            apiError = APIError.NO_CONNECTION;
        } else {
            switch(error.getErrorCode()) {
                case HTTP_BAD_REQUEST_400:
                    apiError = APIError.BAD_REQUEST;
                    break;
                case HTTP_UNAUTHORIZED_401:
                    apiError = APIError.UNAUTHORIZED;
                    if(error.getErrorBody() != null) {
                        if(error.getErrorBody().contains(MESSAGE_BAD_CREDENTIALS)) {
                            apiError = APIError.BAD_CREDENTIALS;
                        } else if(error.getErrorBody().contains(MESSAGE_MAX_LOGIN_ATTEMPTS)) {
                            apiError = APIError.MAX_LOGIN_ATTEMPTS;
                        }
                    }
                    break;
                case HTTP_FORBIDDEN_403:
                    apiError = APIError.FORBIDDEN;
                    if(error.getErrorBody() != null) {
                        if(error.getErrorBody().contains(MESSAGE_RATE_LIMIT_START)) {
                            apiError = APIError.RATE_LIMIT;
                        } else if(error.getErrorBody().contains(MESSAGE_ABUSE_LIMIT)) {
                            apiError = APIError.ABUSE_LIMIT;
                        }
                    }
                    break;
                case HTTP_NOT_ALLOWED_405:
                    apiError = APIError.NOT_ALLOWED;
                    break;
                case HTTP_NOT_FOUND_404:
                    apiError = APIError.NOT_FOUND;
                    break;
                case HTTP_UNPROCESSABLE_422:
                    apiError = APIError.UNPROCESSABLE;
                    break;
                case HTTP_409:
                    if(error.getErrorBody() != null && error.getErrorBody().contains(
                            ERROR_MESSAGE_EMPTY_REPOSITORY)) {
                        apiError = APIError.EMPTY_REPOSITORY;
                        break;
                    }
                default:
                    apiError = APIError.UNKNOWN;
            }
        }
        apiError.error = error;
        return apiError;
    }

    public enum APIError {
        NO_CONNECTION(R.string.error_no_connection),
        UNAUTHORIZED(R.string.error_unauthorized),
        FORBIDDEN(R.string.error_forbidden),
        NOT_FOUND(R.string.error_not_found),
        UNKNOWN(R.string.error_unknown),
        RATE_LIMIT(R.string.error_rate_limit),
        ABUSE_LIMIT(R.string.error_abuse_limit),
        MAX_LOGIN_ATTEMPTS(R.string.error_max_login_attempts),
        UNPROCESSABLE(R.string.error_unprocessable),
        BAD_CREDENTIALS(R.string.error_bad_credentials),
        NOT_ALLOWED(R.string.error_not_allowed),
        BAD_REQUEST(R.string.error_bad_request),
        EMPTY_REPOSITORY(R.string.error_empty_repository);

        @StringRes
        public final int resId;

        @Nullable ANError error;

        APIError(@StringRes int resId) {
            this.resId = resId;
        }
    }


}

```

The ```APIHandler``` class is mostly static constants:

- GIT_BASE - The base URL for all GitHub API requests
- ACCEPT_HEADER_KEY - The key for the content type header
- ACCEPT_HEADER - The default content type, for JSON results
- ORGANIZATIONS_PREVIEW_ACCEPT_HEADER - A content type header required for some features of the API, specifically requesting collaborators on a repository
- PROJECTS_PREVIEW_API_HEADER - A content type header required for requesting information related to projects
- REPO_LICENSE_PREVIEW_API_HEADER - A content type header required to load information about a repositories license when loading the repository
- PAGES_PREVIEW_ACCEPT_HEADER - A content type header required to load information about a repositories pages when loading the repository
- AUTHORIZATION_HEADER_KEY - A header key for providing the authorization token
- AUTHORIZATION_TOKEN_FORMAT - A format string used when inserting the authorization key into a header

Next we have the headers themselves. 
Asheaders are key value pairs, they are represented as string to string maps.

We then have the ```SEGMENT_``` constants. These are segments of the API paths which are used across numerous different API requests.

The ```APIHandler``` constructor checks if the single instance of ```GitHubSession``` is null, and if so access the singleton session instance before initialising the headers.
Next each header map is initialised with the authorization token header, and their own respective accept headers.


The class which actually stores the authorization information is ```GitHubSession```, which was used once above in ```APIHandler```.

``` java
package com.tpb.github.data.auth;

import android.content.Context;
import android.content.SharedPreferences;
import android.support.annotation.NonNull;

import com.tpb.github.data.models.User;

import org.json.JSONException;
import org.json.JSONObject;

public class GitHubSession {
    private static final String TAG = GitHubSession.class.getSimpleName();

    private static GitHubSession session;
    private final SharedPreferences prefs;

    private static final String SHARED = "GitHub_Preferences";
    private static final String API_LOGIN = "username";
    private static final String API_ID = "id";
    private static final String API_ACCESS_TOKEN = "access_token";
    private static final String INFO_USER = "user_json";

    private GitHubSession(Context context) {
        prefs = context.getSharedPreferences(SHARED, Context.MODE_PRIVATE);
    }

    public static GitHubSession getSession(Context context) {
        if(session == null) session = new GitHubSession(context);
        return session;
    }

    void storeUser(JSONObject json) {
        final SharedPreferences.Editor editor = prefs.edit();
        editor.putString(INFO_USER, json.toString());
        final User user = new User(json);
        editor.putInt(API_ID, user.getId());
        editor.putString(API_LOGIN, user.getLogin());
        editor.apply();
    }

    void storeAccessToken(@NonNull String accessToken) {
        final SharedPreferences.Editor editor = prefs.edit();
        editor.putString(API_ACCESS_TOKEN, accessToken);
        editor.apply();
    }

    public User getUser() {
        try {
            final JSONObject obj = new JSONObject(prefs.getString(INFO_USER, ""));
            return new User(obj);
        } catch(JSONException jse) {
            return null;
        }
    }

    public String getUserLogin() {
        return prefs.getString(API_LOGIN, null);
    }

    public String getAccessToken() {
        return prefs.getString(API_ACCESS_TOKEN, null);
    }

    public boolean hasAccessToken() {
        return getAccessToken() != null;
    }

}
```

```GitHubSession``` is a singleton class which saves and loads the user credentials and authorization token to and from shared preferences.

The private constructor is used to initialise the SharedPreferences instance, this either opens the pre-existing map or creates a new one if it does not exist.

When the user authorizes the app the access token is stored with  ```storeAccessToken(@NonNull String accessToken)```.

Once we have an authorization token, we can load the user's data and store it for later use.

The ```LoginActivity``` consists of two layouts, only one of which is visible at a time. 
 
The first layout is a ```WebView``` which is used to display the user authentication page, and the second is a layout to display the user's information once they have signed in.

#include "app/src/main/java/com/tpb/projects/login/LoginActivity.java"

The ```LoginActivity``` binds four views.

1. The ```WebView``` which displays the login webpage
2. The ```CardView``` which olds the ```WebView``` and user layout
3. The spinning ```ProgressBar``` which is display to indicate progress while the ```WebView``` is loading or the user's information is being loaded.
4. The ```LinearLayout``` which will be filled with views showing the user's information


In the ```onCreate``` method the ```WebView``` is set up not to allow scrolling, to enable JavaScript, and to use a custom client to override page loading.

``` java
mWebView.setVerticalScrollBarEnabled(false);
mWebView.setHorizontalScrollBarEnabled(false);
mWebView.setWebViewClient(new OAuthWebViewClient(OAuthHandler.getListener()));
mWebView.getSettings().setJavaScriptEnabled(true);
mWebView.loadUrl(OAuthHandler.getAuthUrl());
mWebView.setLayoutParams(FILL);
```

The ```OAuthWebViewClient``` extends ```WebViewClient``` and is used to capture the code once the user has logged in, as well as ensuring that the user only navigates through the pages required to log in.

The method ```onPageStarted(WebView view, String url, Bitmap favicon)``` is called whenever a page load begins.
The client checks if the url contains `?code=', and if so,  passes the segment after that point to the ```OAuthHandler''' which then requests the authorization token.

``` java
package com.tpb.github.data.auth;

/**
 * Created by theo on 15/12/16.
 */

import android.content.Context;
import android.util.Log;

import com.androidnetworking.AndroidNetworking;
import com.androidnetworking.error.ANError;
import com.androidnetworking.interfaces.JSONObjectRequestListener;
import com.androidnetworking.interfaces.StringRequestListener;
import com.tpb.github.data.APIHandler;
import com.tpb.github.data.models.User;

import org.json.JSONObject;

public class OAuthHandler extends APIHandler {
    private static final String TAG = OAuthHandler.class.getSimpleName();

    private final GitHubSession mSession;
    private final OAuthAuthenticationListener mListener;
    private final String mAuthUrl;
    private final String mTokenUrl;
    private String mAccessToken;

    private static final String AUTH_URL = "https://gitHub.com/login/oauth/authorize?";
    private static final String TOKEN_URL = "https://gitHub.com/login/oauth/access_token?";
    private static final String SCOPE = "user public_repo repo gist";

    private static final String TOKEN_URL_FORMAT = TOKEN_URL + "client_id=%1$s&client_secret=%2$s&redirect_uri=%3$s";
    private static final String AUTH_URL_FORMAT = AUTH_URL + "client_id=%1$s&scope=%2$s&redirect_uri=%3$s";

    public OAuthHandler(Context context, String clientId, String clientSecret,
                        String callbackUrl,
                        OAuthAuthenticationListener listener) {
        super(context);
        mSession = GitHubSession.getSession(context);
        mTokenUrl = String.format(TOKEN_URL_FORMAT, clientId, clientSecret, callbackUrl);
        mAuthUrl = String.format(AUTH_URL_FORMAT, clientId, SCOPE, callbackUrl);
        mListener = listener;
    }

    public void getAccessToken(final String code) {
        AndroidNetworking.get(mTokenUrl + "&code=" + code)
                         .build()
                         .getAsString(new StringRequestListener() {
                             @Override
                             public void onResponse(String response) {
                                 mAccessToken = response.substring(
                                         response.indexOf("access_token=") + 13,
                                         response.indexOf("&scope")
                                 );
                                 mSession.storeAccessToken(mAccessToken);
                                 initHeaders();
                                 mListener.onSuccess();
                                 fetchUser();
                             }

                             @Override
                             public void onError(ANError anError) {
                                 mListener.onFail(anError.getErrorDetail());
                             }
                         });
    }

    private void fetchUser() {
        AndroidNetworking.get(GIT_BASE + SEGMENT_USER)
                         .addHeaders(API_AUTH_HEADERS)
                         .build()
                         .getAsJSONObject(new JSONObjectRequestListener() {
                             @Override
                             public void onResponse(JSONObject response) {
                                 mSession.storeUser(response);
                                 mListener.userLoaded(mSession.getUser());
                             }

                             @Override
                             public void onError(ANError anError) {
                                 mListener.onFail(anError.getErrorDetail());
                                 Log.e(TAG, "onError: " + anError.getErrorDetail());
                             }
                         });

    }

    public String getAuthUrl() {
        return mAuthUrl;
    }

    public interface OAuthAuthenticationListener {
        void onSuccess();

        void onFail(String error);

        void userLoaded(User user);

    }
}
```

The ```OAuthHandler``` is used to load the authenticated user for the first time.

```getAccessToken(final String code)``` performs a get request to the formatted token url, and parses the response as a string.
The access token is extracted from the string between "access_token=" and "&scope".
Once the access token has been extracted:
- The token is stored with ```GitHubSession```
- The headers are initialised with the token
- The authentication listener (```LoginActivity```) is notified, allowing it to update the ```ProgressBar```
- ```fetchUser()``` is called to load the authenticated user model

```fetchUser()``` performs another request, this to time /user, which loads the authenticated user if provided with an authorization token.
The response is returned as a ```JSONObject```, Java's built in JSON model, and is passed to ```GitHubSession``` where it is stored as a string for later use, and parsed into a ```User``` object.
The authentication listener is then called again, with the ```User``` object.


Prior to the ```WebView``` loading the login page, the ```LoginActivity``` shows the view with a spinning ```ProgressBar```, and once it has finished the login page is displayed

| | |
| --- | --- | 
|![Login Activity loading webpage](http://imgur.com/Iv6xAz7.png) | ![Login Activity login page](http://imgur.com/x2MKOxk.png) |

Once the user has logged in, the GitHub authentication page will show the access which the app is asking for, and ask the user to grant access:

|  |  | 
| --- | --- | 
|![Page 1](http://imgur.com/zmbcpfA.png) | ![Page 2](http://imgur.com/wiieru1.png)

Finally, the user is loaded and their information is displayed.

![User information](http://imgur.com/T4n1feN.png)


### Data models and loading

All of the models from the GitHub API are returned as JSON unless another content type is specified.
Each endpoint returns either a single model, or a single dimensional array of models.

All models used extend the abstract class ```DataModel``` which contains some of the commonly used keys, as well as the object creation date which is used across all models.

``` java
package com.tpb.github.data.models;

/**
 * Created by theo on 15/12/16.
 */

public abstract class DataModel {

    static final String ID = "id";
    static final String NAME = "name";
    static final String CREATED_AT = "created_at";
    static final String UPDATED_AT = "updated_at";
    static final String URL = "url";
    public static final String JSON_NULL = "null";

    long createdAt;

    public abstract long getCreatedAt();


}

```

Networking is split into two classes extending ```APIHandler``` is split across four classes.

#### Loader

The ```Loader``` class is responsible for almost all get requests sent to the GitHub API.

Each method is responsible for load a single model type, and takes the path or filter parameters required to load the model(s) as well as an implementation of a generic loader.

The first interface ```ItemLoader``` is used when loading a single model or value.

``` Java
public interface ItemLoader<T> {

    void loadComplete(T data);

    void loadError(APIError error);

}
```

Any class implementing ```ItemLoader``` must implement ```loadComplete(T data)``` as well as ```loadError(APIError error)```.

Most uses of ```ItemLoader``` load an instance of ```DataModel```.

An example is ```loadIssue(@NonNull final ItemLoader<Issue> loader, String repoFullName, int issueNumber, boolean highPriority)```


``` java
loadIssue(@NonNull final ItemLoader<Issue> loader, String repoFullName, int issueNumber, boolean highPriority) {
        get(GIT_BASE + SEGMENT_REPOS + "/" + repoFullName + SEGMENT_ISSUES + "/" + issueNumber)
                .addHeaders(API_AUTH_HEADERS)
                .setPriority(highPriority ? Priority.HIGH : Priority.MEDIUM)
                .build()
                .getAsJSONObject(new JSONObjectRequestListener() {
                    @Override
                    public void onResponse(JSONObject response) {
                        loader.loadComplete(new Issue(response));
                    }

                    @Override
                    public void onError(ANError anError) {
                        loader.loadError(parseError(anError));
                    }
                });
    }
```


this method is used to load a single ```Issue``` model given a full repository name (user login and repository name) and the issue number.

Some single methods also have prefetching when a null ```ItemLoader``` is passed to them:

``` java
loadProject(@Nullable final ItemLoader<Project> loader, int id) {
        final ANRequest req = get(GIT_BASE + SEGMENT_PROJECTS + "/" + id)
                .addHeaders(PROJECTS_API_API_AUTH_HEADERS)
                .build();
        if(loader == null) {
            req.prefetch();
        } else {
            req.getAsJSONObject(new JSONObjectRequestListener() {
                @Override
                public void onResponse(JSONObject response) {
                    loader.loadComplete(new Project(response));
                }

                @Override
                public void onError(ANError anError) {
                    loader.loadError(parseError(anError));
                }
            });
        }
    }
```

In this case the ```ANRequest``` instance is built and only requested as a ```JSONObject``` when there is an ```ItemLoader``` to deal with the model.
This allows the response to be preloaded before an Activity is started, and only parsed to a ```DataModel``` once a user interface is present to use it.


The ```Loader``` class also contains the ```ListLoader``` interface

```Java
public interface ListLoader<T> {

    void listLoadComplete(List<T> data);

    void listLoadError(APIError error);

}
```

which is used to return objects parsed from a JSONArray as a list of ```DataModels```.

#### Models

Numerous different models are required for use in different parts of the project.


<ol>
    <li> User model:
        <ul> 
            <li> 1. Sign in </li>
            <li> 2. Users </li>
            <li> 3. Repositories </li>
            <li> 4. Issues </li>
            <li> 5. Commits </li>
            <li> 6. Projects </li>
        </ul>
    </li>
    <li> Repository model: 
        <ul>
            <li> 2.b User repositories </li>
            <li> 2.c User starred repositories </li>
            <li> 3. Repositories </li>
        </ul>
    </li>
    <li> Node model
        <ul> 
            <li> 3.b Repository files</li>
        </ul>
      </li>
    <li> Issue and label models: 
        <ul>
            <li> 3.e Repository issues </li>
            <li> 4. Issues </li>
            <li> 6.d.ii Project issue cards </li>
            <li> 6.g Editing project issue cards </li>
            <li> 6.h.ii Creation of new issue cards </li>
            <li> 6.h.iii Creation of new issue cards from existing issues </li>
        </ul>
    </li>
    <li> IssueEvent and MergedModel models:
        <ul>
            <li> 4.a.x Issue events</li>
         </ul>
     </li>
    <li> Gist model: 
        <ul>
            <li>2.d User gists </li>
        </ul>
    </li>
    <li> Project model:
        <ul>
            <li> 3.f Repository projects </li>
            <li> 6. Projects </li>
        </ul>
     </li>
     <li> Column model: 
        <ul>
            <li> 6. Projects </li>
        </ul>
     </li>
     <li> Card model: 
        <ul>
            <li> 6. Projects </li>
        </ul>
     </li>
     <li> Commit model:
        <ul>
            <li> 3.d Repository commits</li>
            <li> 5. Commits </li>
        </ul>
     </li>
     <li> DiffFile model: 
        <ul>
            <li> 5.b Commit diffs </li>
         </ul>
     </li>
    <li> Status and CompleteStatus models 
        <ul>
            <li> 5.d commit statuses </li>
        </ul>
     </li>
     <li> State model: 
        <ul> 
            <li> Issue model </li>
            <li> Project model </li>
            <li> Milestone model </li>
        </ul>
     </li>
     <li>Comment model:
        <ul> 
            <li> 4.b Issue comments</li>
            <li> 5.c Commit comments </li>
        </ul>
      </li>
     <li> Notification model:
        <ul> 8. Notifications</ul>
     </li>
</ol>