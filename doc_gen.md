 
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
The probability of a collision occurring across n unique objects is 0.5 * n<sup>2</sup> /2<sup>160</sup>.
In order to achieve a 1% probability of collision 1.7x10<sup>23</sup> objects are required, which is effectively impossible.

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

<div style="page-break-after: always;"></div>

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

<div style="page-break-after: always;"></div>

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

<div style="page-break-after: always;"></div>

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

<div style="page-break-after: always;"></div>

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

<div style="page-break-after: always;"></div>

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

<div style="page-break-after: always;"></div>

### Basic storage

While many applications use a variety of database solutions, it is not always necessary or the best solution to store simple values in a full database.

The ```SharedPreferences``` system is a persistent set of key value pairs which can be used to store application information such as settings.

Each 'preference' is a key value set stored under a particular name.
Once a ```SharedPreference``` object has been returned for the set, the key value pairs within the set can be read from and written to.


<div style="page-break-after: always;"></div>

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


<div style="page-break-after: always;"></div>

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

<div style="page-break-after: always;"></div>

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
            <li> Implement following and unfollowing of users </li>
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
                            <li>The reactions to the comment</li>
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
                            <li>The reactions to the comment</li>
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

<div style="page-break-after: always;"></div>

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

<div style="page-break-after: always;"></div>

### Proposed design

As explained in the background section, an Android app is made up of Activities and Fragments.

In order to implement the functionality listed above the app requires multiple activities to mimic the pages on the GitHub website.

#### User information Activity

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

<div style="page-break-after: always;"></div>

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

**APIHandler.java**
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
    private static final String REACTIONS_PREVIEW_ACCEPT_HEADER = "  application/vnd.github.squirrel-girl-preview";
    private static final String AUTHORIZATION_HEADER_KEY = "Authorization";
    private static final String AUTHORIZATION_TOKEN_FORMAT = "token %1$s";
    private static GitHubSession mSession;

    protected static final HashMap<String, String> API_AUTH_HEADERS = new HashMap<>();
    static final HashMap<String, String> PROJECTS_API_AUTH_HEADERS = new HashMap<>();
    static final HashMap<String, String> ORGANIZATIONS_API_AUTH_HEADERS = new HashMap<>();
    static final HashMap<String, String> LICENSES_API_AUTH_HEADERS = new HashMap<>();
    static final HashMap<String, String> PAGES_API_AUTH_HEADERS = new HashMap<>();
    static final HashMap<String, String> REACTIONS_API_PREVIEW_AUTH_HEADERS = new HashMap<>();

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

        PROJECTS_API_AUTH_HEADERS.put(AUTHORIZATION_HEADER_KEY,
                String.format(AUTHORIZATION_TOKEN_FORMAT, accessToken)
        );
        PROJECTS_API_AUTH_HEADERS.put(ACCEPT_HEADER_KEY, PROJECTS_PREVIEW_ACCEPT_HEADER);

        LICENSES_API_AUTH_HEADERS.put(AUTHORIZATION_HEADER_KEY,
                String.format(AUTHORIZATION_TOKEN_FORMAT, accessToken)
        );
        LICENSES_API_AUTH_HEADERS.put(ACCEPT_HEADER_KEY, REPO_LICENSE_PREVIEW_ACCEPT_HEADER);

        PAGES_API_AUTH_HEADERS.put(AUTHORIZATION_HEADER_KEY,
                String.format(AUTHORIZATION_TOKEN_FORMAT, accessToken)
        );
        PAGES_API_AUTH_HEADERS.put(ACCEPT_HEADER_KEY, PAGES_PREVIEW_ACCEPT_HEADER);

        REACTIONS_API_PREVIEW_AUTH_HEADERS.put(AUTHORIZATION_HEADER_KEY,
                String.format(AUTHORIZATION_TOKEN_FORMAT, accessToken)
        );
        REACTIONS_API_PREVIEW_AUTH_HEADERS.put(ACCEPT_HEADER_KEY, REACTIONS_PREVIEW_ACCEPT_HEADER);
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

**GitHubSession.java**
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

**LoginActivity.java**
``` java
package com.tpb.projects.login;

import android.annotation.SuppressLint;
import android.content.Intent;
import android.graphics.Bitmap;
import android.os.Bundle;
import android.os.Handler;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.CardView;
import android.view.View;
import android.view.ViewGroup;
import android.webkit.CookieManager;
import android.webkit.CookieSyncManager;
import android.webkit.WebView;
import android.webkit.WebViewClient;
import android.widget.FrameLayout;
import android.widget.LinearLayout;
import android.widget.ProgressBar;

import com.google.firebase.analytics.FirebaseAnalytics;
import com.tpb.github.data.auth.OAuthHandler;
import com.tpb.github.data.models.User;
import com.tpb.projects.BuildConfig;
import com.tpb.projects.R;
import com.tpb.projects.markdown.Formatter;
import com.tpb.projects.user.UserActivity;
import com.tpb.projects.util.Analytics;
import com.tpb.projects.util.UI;

import butterknife.BindView;
import butterknife.ButterKnife;

import static com.tpb.projects.flow.ProjectsApplication.mAnalytics;

/**
 * A login screen that offers login via email/password.
 */
public class LoginActivity extends AppCompatActivity implements OAuthHandler.OAuthAuthenticationListener {
    private static final String TAG = LoginActivity.class.getSimpleName();
    private boolean mLoginShown = false;

    @BindView(R.id.login_webview) WebView mWebView;
    @BindView(R.id.login_form) CardView mLogin;
    @BindView(R.id.progress_spinner) ProgressBar mSpinner;
    @BindView(R.id.user_details) LinearLayout mUserDetails;

    private Intent mLaunchIntent;

    private static final FrameLayout.LayoutParams FILL = new FrameLayout.LayoutParams(
            ViewGroup.LayoutParams.FILL_PARENT,
            ViewGroup.LayoutParams.FILL_PARENT
    );

    @SuppressLint("SetJavaScriptEnabled")
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login);
        ButterKnife.bind(this);

        final OAuthHandler OAuthHandler = new OAuthHandler(this,
                BuildConfig.GITHUB_CLIENT_ID,
                BuildConfig.GITHUB_CLIENT_SECRET,
                BuildConfig.GITHUB_REDIRECT_URL,
                this
        );

        if(getIntent().hasExtra(Intent.EXTRA_INTENT)) {
            mLaunchIntent = getIntent().getParcelableExtra(Intent.EXTRA_INTENT);
        } else {
            mLaunchIntent = new Intent(LoginActivity.this, UserActivity.class);
        }

        mWebView.setVerticalScrollBarEnabled(false);
        mWebView.setHorizontalScrollBarEnabled(false);
        mWebView.setWebViewClient(new OAuthWebViewClient(OAuthHandler));
        mWebView.getSettings().setJavaScriptEnabled(true);
        mWebView.loadUrl(OAuthHandler.getAuthUrl());
        mWebView.setLayoutParams(FILL);

        mUserDetails.setVisibility(View.GONE);
        UI.expand(mLogin);
    }

    @Override
    public void onSuccess() {
        mWebView.setVisibility(View.GONE);
        mSpinner.setVisibility(View.VISIBLE);
    }

    @Override
    public void onFail(String error) {

    }

    @Override
    public void userLoaded(User user) {
        mSpinner.setVisibility(View.GONE);
        Formatter.displayUser(mUserDetails, user);
        final Bundle bundle = new Bundle();
        bundle.putString(Analytics.TAG_LOGIN, Analytics.VALUE_SUCCESS);
        mAnalytics.logEvent(FirebaseAnalytics.Event.LOGIN, bundle);
        new Handler().postDelayed(() -> {
            CookieSyncManager.createInstance(this);
            final CookieManager cookieManager = CookieManager.getInstance();
            cookieManager.removeAllCookie();
            startActivity(mLaunchIntent);
            overridePendingTransition(R.anim.slide_up, R.anim.none);
            finish();
        }, 1500);
    }

    private void ensureWebViewVisible() {
        if(!mLoginShown) {
            new Handler().postDelayed(() -> {
                mWebView.setVisibility(View.VISIBLE);
                mSpinner.setVisibility(View.GONE);
                mLoginShown = true;
            }, 150);
        }

    }


    private class OAuthWebViewClient extends WebViewClient {
        private final OAuthHandler mAuthHandler;

        OAuthWebViewClient(OAuthHandler handler) {
            mAuthHandler = handler;
        }

        @Override
        public boolean shouldOverrideUrlLoading(WebView view, String url) {
            return !(url.startsWith("https://github.com/login/oauth/authorize") ||
                    url.startsWith("https://github.com/login?") ||
                    url.startsWith("https://github.com/session") ||
                    url.startsWith(BuildConfig.GITHUB_REDIRECT_URL));
        }

        @Override
        public void onPageStarted(WebView view, String url, Bitmap favicon) {
            if(url.contains("?code=")) {
                final String[] parts = url.split("=");
                mAuthHandler.getAccessToken(parts[1]);
            }
            super.onPageStarted(view, url, favicon);
        }

        @Override
        public void onPageFinished(WebView view, String url) {
            ensureWebViewVisible();
            super.onPageFinished(view, url);
        }

    }

}


```

The ```LoginActivity``` binds four views.

1. The ```WebView``` which displays the login webpage
2. The ```CardView``` which olds the ```WebView``` and user layout
3. The spinning ```ProgressBar``` which is display to indicate progress while the ```WebView``` is loading or the user's information is being loaded.
4. The ```LinearLayout``` which will be filled with views showing the user's information


In the ```onCreate``` method the ```WebView``` is set up not to allow scrolling, to enable JavaScript, and to use a client implementation to override page loading.

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

This completes objective 1.a

**OAuthHandler.java**
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

If the user presses the authorize button, the page will be redirected through url containing the path parameter "code".

In on the overriden ```onPageStarted``` method of the ```OAuthWebViewClient``` this results in the ```code``` parameter being passed to the ```OAuthHandler``` through ```fetchAccessToken```.

**LoginActivity.java**
``` java
public void onPageStarted(WebView view, String url, Bitmap favicon) {
            if(url.contains("?code=")) {
                final String[] parts = url.split("=");
                mAuthHandler.getAccessToken(parts[1]);
            }
            super.onPageStarted(view, url, favicon);
        }
```


**OAuthHandler.java**
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

The ```fetchAccessToken``` method performs a get request to the token URL created with the apps client id and secret, adding the code as a path parameter.

On a successful response the access token is split from the returned value and stored through ```GitHubSession```.

This completes objective 1.b

The authorization headers are initialised with a call to ```initHeaders``` and the ```OAuthAuthenticationListener``` (```LoginActivity```) is notified of the success.

Finally, ```fetchUser``` is called.

This method performs a second get request to the Git API,  this time to load the ```User``` model and store the JSON data, as well as notifying the ```OAuthAuthenticationListener``` that the user has been loaded, allowing the user information to be displayed.

![User information](http://imgur.com/T4n1feN.png)




### Data models and loading

All of the models from the GitHub API are returned as JSON unless another content type is specified.
Each endpoint returns either a single model, or a single dimensional array of models.

All models used extend the abstract class ```DataModel``` which contains some of the commonly used keys, as well as the object creation date which is used across all models.

**DataModel.java**
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


**Loader.java**
``` java
loadIssue(@NonNull final ItemLoader<Issue> loader, String repoFullName, int issueNumber, boolean highPriority) {
        get(GIT_BASE + SEGMENT_REPOS + "/" + repoFullName + SEGMENT_ISSUES + "/" + issueNumber)
                .addHeaders(REACTIONS_API_PREVIEW_AUTH_HEADERS)
                .setPriority(highPriority ? Priority.HIGH : Priority.MEDIUM)
                .setTag(loader)
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
        return this;
    }
```


this method is used to load a single ```Issue``` model given a full repository name (user login and repository name) and the issue number.

Some single methods also have prefetching when a null ```ItemLoader``` is passed to them:

**Loader.java**
``` java
loadProject(@Nullable final ItemLoader<Project> loader, int id) {
        final ANRequest req = get(GIT_BASE + SEGMENT_PROJECTS + "/" + id)
                .addHeaders(PROJECTS_API_AUTH_HEADERS)
                .setTag(loader)
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
        return this;
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

The ```Loader``` is a singleton accessed by ```Loader.getLoader```

#### Models

Numerous different models are required to fulfill different objectives

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
            <li> 2.d User gists </li>
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

## Base structure

In order to maintain a cleaner application structure, it is normal to separate repeated logic into a base class or a set of utility classes.

### BaseActivity

In this case ```BaseActivity``` is used to check that there is an authentication token stored, to provide an onClick method for the toolbar back button, cancel network requests,  and to fix a memory leak caused by Android system transitions keeping a reference to the ```DecorView``` which in turn references the ```Activity```.

**BaseActivity.java**
``` java
package com.tpb.projects.common;

import android.content.Intent;
import android.os.Bundle;
import android.support.annotation.Nullable;
import android.support.transition.Transition;
import android.support.transition.TransitionManager;
import android.support.v4.app.Fragment;
import android.support.v7.app.AppCompatActivity;
import android.util.ArrayMap;
import android.view.View;
import android.view.ViewGroup;

import com.androidnetworking.AndroidNetworking;
import com.tpb.github.data.auth.GitHubSession;
import com.tpb.projects.login.LoginActivity;
import com.tpb.projects.notifications.receivers.NotificationEventReceiver;

import java.lang.ref.WeakReference;
import java.lang.reflect.Field;
import java.util.ArrayList;
import java.util.List;

/**
 * Created by theo on 10/03/17.
 */

public abstract class BaseActivity extends AppCompatActivity {

    public boolean mHasAccess = true;
    private final List<WeakReference<Fragment>> mWeakFragments = new ArrayList<>();

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        if(GitHubSession.getSession(this).hasAccessToken()) {
            mHasAccess = true;
            NotificationEventReceiver.setupAlarm(getApplicationContext());
        } else {
            mHasAccess = false;
            final Intent intent = new Intent(BaseActivity.this, LoginActivity.class);
            intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK | Intent.FLAG_ACTIVITY_NEW_TASK);
            if(getIntent() != null && !getIntent().getAction().equals(Intent.ACTION_MAIN)) {
                intent.putExtra(Intent.EXTRA_INTENT, getIntent());
            }
            startActivity(intent);
            finish();
        }

    }

    public void onToolbarBackPressed(View view) {
        onBackPressed();
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        removeActivityFromTransitionManager();
        cancelNetworkRequests();
    }

    private void removeActivityFromTransitionManager() {
        final Class transitionManagerClass = TransitionManager.class;
        try {
            final Field runningTransitionsField = transitionManagerClass
                    .getDeclaredField("sRunningTransitions");
            runningTransitionsField.setAccessible(true);
            //noinspection unchecked
            final ThreadLocal<WeakReference<ArrayMap<ViewGroup, ArrayList<Transition>>>> runningTransitions
                    = (ThreadLocal<WeakReference<ArrayMap<ViewGroup, ArrayList<Transition>>>>)
                    runningTransitionsField.get(transitionManagerClass);
            if(runningTransitions.get() == null || runningTransitions.get().get() == null) {
                return;
            }
            ArrayMap map = runningTransitions.get().get();
            View decorView = getWindow().getDecorView();
            if(map.containsKey(decorView)) {
                map.remove(decorView);
            }
        } catch(Exception ignored) {
        }
    }

    @Override
    public void onAttachFragment (Fragment fragment) {
        mWeakFragments.add(new WeakReference<>(fragment));
    }

    private void cancelNetworkRequests() {
        AndroidNetworking.cancel(this);
        for(WeakReference<Fragment> ref : mWeakFragments) {
            if(ref.get() != null) {
                AndroidNetworking.cancel(ref.get());
            }
        }
    }

}

```

#### onCreate

The ```BaseActivity``` ```onCreate``` method checks that ```GitHubSession``` has an access token, setting the ```mHasAccess``` flag accordingly.

If there is an access token, the notification service (Objective 8) is started.

If there is not an access token the ```mHasAccess``` flag is set to false, allowing any extending ```Activities``` to return from their ```onCreate``` methods without performing any unnecessary initialisation, such as ```View``` inflation.

An ```Intent``` is then created to launch ```LoginActivity```, setting the flags ```Intent.FLAG_ACTIVITY_CLEAR_TASK``` and ```Intent.FLAG_ACTIVITY_NEW_TASK``` which result in the current task, a group of ```Activities``` being destroyed and a new task being created for the ```LoginActivity```.

Next, the method checks if the ```Intent``` which started ```BaseActivity``` is not from the home screen (```ACTION_MAIN```).
If the ```Intent``` is not from the homescreen, the ```Activity``` was launched from a URL which the user likely wants to view once they have signed in. This can be achieved by passing the launch ```Intent``` forward to the ```LoginActivity```.

#### onDestroy

##### Network cancelling

Each network request made uses the calling object, e.g. an implementation of ```ItemLoader``` as a tag.
The ```BaseActivity``` retains a ```WeakReferences``` to each of the ```Fragments``` attached to it, and uses these to cancel network requests as they ```Activity``` is destroyed.

**BaseActivity.java**
``` java
void onAttachFragment (Fragment fragment) {
        mWeakFragments.add(new WeakReference<>(fragment));
    }
```

```onAttachFragment``` adds a ```WeakReference``` to the ```Fragment``` to ```mWeakFragments```, and ```cancelNetworkRequests``` uses these to cancel network requests started by each ```Fragment```.

**BaseActivity.java**
``` java
cancelNetworkRequests();
    }

    private void removeActivityFromTransitionManager() {
        final Class transitionManagerClass = TransitionManager.class;
        try {
            final Field runningTransitionsField = transitionManagerClass
                    .getDeclaredField("sRunningTransitions");
            runningTransitionsField.setAccessible(true);
            //noinspection unchecked
            final ThreadLocal<WeakReference<ArrayMap<ViewGroup, ArrayList<Transition>>>> runningTransitions
                    = (ThreadLocal<WeakReference<ArrayMap<ViewGroup, ArrayList<Transition>>>>)
                    runningTransitionsField.get(transitionManagerClass);
            if(runningTransitions.get() == null || runningTransitions.get().get() == null) {
                return;
            }
            ArrayMap map = runningTransitions.get().get();
            View decorView = getWindow().getDecorView();
            if(map.containsKey(decorView)) {
                map.remove(decorView);
            }
        } catch(Exception ignored) {
        }
    }
```

##### Memory Leak

A bug introduced in Android 5.0, launched in November 2014, which results in a reference to the ```Activity``` being kept in the ```TransitionManager```.
The memory leak can be solved by using reflection to remove the ```Activity``` from the ```TransitionManager``` map.

```
final ThreadLocal<WeakReference<ArrayMap<ViewGroup, ArrayList<Transition>>>> runningTransitions = (ThreadLocal<WeakReference<ArrayMap<ViewGroup, ArrayList<Transition>>>> runningTransitionsField.get(transitionManagerClass);
```

The ```runningTransitionsField``` refers to a ```ThreadLocal``` ```WeakReference``` to a map of ```ViewGroups``` to ```ArrayLists``` of ```Transitions```.

```ThreadLocal``` is a reference to a field, such that each ```Thread``` which access a thread-local variable via the ```TheadLocals``` get and set methods have their own copy of the variable.

A ```WeakReference``` is a reference which is not strong enough to prevent grabage collection.

Finally we have the map which may contain the reference to our ```Activity```'s ```DecorView```.

```removeActivityFromTransitionManager``` checks that both the ```ThreadLocal```'s ```WeakReference``` is not null, and that the ```WeakReference```'s ```ArrayMap``` is not null before checking the ```ArrayMap``` for the ```DecorView``` and removing it if is present.

#### onToolbarBackPressed

```onToolbarBackPressed``` is used solely to reduce clutter in other ```Activity``` classes.
When a ```View``` is declared in XML, its onClick listener can be set with the ```onClick``` attribute.
Rather than adding this method in all of the ```Activities```, it can be added to ```BaseActivity```.

``` XML
<ImageButton
    android:id="@+id/back_button"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:background="@android:color/transparent"
    android:src="@drawable/ic_arrow_back"
    android:onClick="onToolbarBackPressed"/>
```

The back button shown in each ```Toolbar``` then references this method, which calls the ```Activity``` method ```onBackPressed``` to perform the same behaviour as pressing the navigation back key.

<div style="page-break-after: always;"></div>

### NetworkImageView

The ```NetworkImageView``` is a subclass of ```ImageView``` used to asynchronously load images from a given URL and display them once they have finished downloading.

**NetworkImageView.java**
``` java
package com.tpb.projects.common;

import android.content.Context;
import android.content.res.TypedArray;
import android.support.annotation.IdRes;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.support.v7.widget.AppCompatImageView;
import android.text.TextUtils;
import android.util.AttributeSet;
import android.view.ViewGroup;

import com.androidnetworking.error.ANError;
import com.androidnetworking.internal.ANImageLoader;
import com.tpb.projects.R;

/**
 * Created by theo on 01/04/17.
 */

public class NetworkImageView extends AppCompatImageView {

    private String mUrl;

    @IdRes private int mDefaultImageResId;
    @IdRes private int mErrorImageResId;

    private ANImageLoader.ImageContainer mImageContainer;

    public NetworkImageView(Context context) {
        super(context);
    }

    public NetworkImageView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        if(attrs != null) init(attrs, 0);
    }

    public NetworkImageView(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        if(attrs != null) init(attrs, defStyleAttr);
    }

    private void init(AttributeSet attrs, int defStyleAttr) {
        final TypedArray array = getContext()
                .obtainStyledAttributes(attrs, R.styleable.NetworkImageView, defStyleAttr, 0);
        mDefaultImageResId = array
                .getResourceId(R.styleable.NetworkImageView_default_image_resource,
                        R.drawable.ic_avatar_default
                );
        mErrorImageResId = array
                .getResourceId(R.styleable.NetworkImageView_error_image_resource, 0);
        array.recycle();
    }

    public void setImageUrl(@NonNull String url) {
        mUrl = url;
        loadImage(false);
    }

    public void setDefaultImageResId(@IdRes int defaultImage) {
        mDefaultImageResId = defaultImage;
    }

    public void setErrorImageResId(@IdRes int errorImage) {
        mErrorImageResId = errorImage;
    }

    private void loadImage(final boolean isInLayoutPass) {
        final int width = getWidth();
        final int height = getHeight();


        boolean wrapWidth = false, wrapHeight = false;
        if(getLayoutParams() != null) {
            wrapWidth = getLayoutParams().width == ViewGroup.LayoutParams.WRAP_CONTENT;
            wrapHeight = getLayoutParams().height == ViewGroup.LayoutParams.WRAP_CONTENT;
        }

        if(width == 0 && height == 0 && !(wrapWidth && wrapHeight)) {
            //Can't display the image as not size
            return;
        }

        if(TextUtils.isEmpty(mUrl)) {
            if(mImageContainer != null) {
                mImageContainer.cancelRequest();
                mImageContainer = null;
            }
            setDefaultImage();
            return;
        }

        if(mImageContainer != null && mImageContainer.getRequestUrl() != null) {
            if(mImageContainer.getRequestUrl().equals(mUrl)) {
                return;
            } else {
                mImageContainer.cancelRequest();
            }
        }

        final int maxWidth = wrapWidth ? 0 : width;
        final int maxHeight = wrapHeight ? 0 : height;

        final ScaleType scaleType = getScaleType();
        mImageContainer = ANImageLoader.getInstance().get(mUrl,
                new ANImageLoader.ImageListener() {
                    @Override
                    public void onResponse(final ANImageLoader.ImageContainer response,
                                           boolean isImmediate) {
                        if(isImmediate && isInLayoutPass) {
                            post(() -> onResponse(response, false));
                            return;
                        }

                        if(response.getBitmap() != null) {
                            setImageBitmap(response.getBitmap());
                        } else if(mDefaultImageResId != 0) {
                            setDefaultImage();
                        }
                    }

                    @Override
                    public void onError(ANError error) {
                        if(mErrorImageResId != 0) {
                            setImageResource(mErrorImageResId);
                        }
                    }
                }, maxWidth, maxHeight, scaleType
        );

    }

    private void setDefaultImage() {
        if(getDrawable() != null) return; //Drawable has been set manually
        if(mDefaultImageResId != 0) {
            setImageResource(mDefaultImageResId);
        } else {
            setImageBitmap(null);
        }
    }

    @Override
    protected void onLayout(boolean changed, int left, int top, int right, int bottom) {
        super.onLayout(changed, left, top, right, bottom);
        loadImage(true);
    }

    @Override
    protected void drawableStateChanged() {
        super.drawableStateChanged();
        invalidate();
    }
}

```

The ```NetworkImageView``` has three instance variables; the URL to be loaded as well as two resource identifiers for the loading and error states.

When the ```NetworkImageView``` is instantiated it checks the ```AttributeSet``` for resource identifiers set in XML.

**NetworkImageView.java**
``` java
void init(AttributeSet attrs, int defStyleAttr) {
        final TypedArray array = getContext()
                .obtainStyledAttributes(attrs, R.styleable.NetworkImageView, defStyleAttr, 0);
        mDefaultImageResId = array
                .getResourceId(R.styleable.NetworkImageView_default_image_resource,
                        R.drawable.ic_avatar_default
                );
        mErrorImageResId = array
                .getResourceId(R.styleable.NetworkImageView_error_image_resource, 0);
        array.recycle();
    }
```

The ```loadImage``` method is responsible for loading and displaying the image.

**NetworkImageView.java**
``` java
void loadImage(final boolean isInLayoutPass) {
        final int width = getWidth();
        final int height = getHeight();


        boolean wrapWidth = false, wrapHeight = false;
        if(getLayoutParams() != null) {
            wrapWidth = getLayoutParams().width == ViewGroup.LayoutParams.WRAP_CONTENT;
            wrapHeight = getLayoutParams().height == ViewGroup.LayoutParams.WRAP_CONTENT;
        }

        if(width == 0 && height == 0 && !(wrapWidth && wrapHeight)) {
            //Can't display the image as not size
            return;
        }

        if(TextUtils.isEmpty(mUrl)) {
            if(mImageContainer != null) {
                mImageContainer.cancelRequest();
                mImageContainer = null;
            }
            setDefaultImage();
            return;
        }

        if(mImageContainer != null && mImageContainer.getRequestUrl() != null) {
            if(mImageContainer.getRequestUrl().equals(mUrl)) {
                return;
            } else {
                mImageContainer.cancelRequest();
            }
        }

        final int maxWidth = wrapWidth ? 0 : width;
        final int maxHeight = wrapHeight ? 0 : height;

        final ScaleType scaleType = getScaleType();
        mImageContainer = ANImageLoader.getInstance().get(mUrl,
                new ANImageLoader.ImageListener() {
                    @Override
                    public void onResponse(final ANImageLoader.ImageContainer response,
                                           boolean isImmediate) {
                        if(isImmediate && isInLayoutPass) {
                            post(() -> onResponse(response, false));
                            return;
                        }

                        if(response.getBitmap() != null) {
                            setImageBitmap(response.getBitmap());
                        } else if(mDefaultImageResId != 0) {
                            setDefaultImage();
                        }
                    }

                    @Override
                    public void onError(ANError error) {
                        if(mErrorImageResId != 0) {
                            setImageResource(mErrorImageResId);
                        }
                    }
                }, maxWidth, maxHeight, scaleType
        );

    }
```

The first check performed in ```loadImage``` is whether the image can actually be drawn.

The ```View``` width and height are collected, before the ```LayoutParams``` are checked to determine whether the ```NetworkImageView``` is of a fixed size, or should expand to the size of its image.
If the ```NetworkImageView``` has 0 width and height, and doesn't wrap in either direction, then ```loadImage``` returns as it cannnot display the image.

The second check is whether the URL is empty.
If a null or empty url is passed, any current request is cancelled, and the default image is set.

The third check is whether the URL which is currently being loaded, or has been loaded is the same as the URL passed to the ```NetworkImageView```.
If the URL is already being loaded, ```loadImage``` returns, otherwise it cancels the current request in preparation for loading a new image.

Finally, the maximum width and height of the ```NetworkImageView``` are determined and the image is loaded.

The ```onResponse``` callback has two arguments, the response, and the isImmediate flag which indicates whether the response was returned form cache.

If both isImmediate and isInLayoutPass are true, the ```NetworkImageView``` has not yet been properly drawn an the image cannot yet be displayed.
In this case, a ```post``` call is made, which will add a runnable to the UI thread ```MessageQueue``` for execution once all other work on the UI thread is complete.

When the ```NetworkImageView``` is able to set the image, it checks whether the ```Bitmap``` is non null, and sets either the ```Bitmap``` or the default resource accordingly.

<div style="page-break-after: always;"></div>

### ViewSafeFragment

The lifecycle of a ```Fragment``` is different than that of an ```Activity```.

The first method called is ```onAttach``` when the ```Fragment``` is attached to an ```Activity```.
Next, ```onCreate``` is called, which might be used to initialise non view-dependent logic.
Most importantly, ```onCreateView``` is called.

```onCreateView``` returns a ```View``` object and must created the layout for the ```Fragment```.

As the ```Activity``` is created prior to the ```Fragment``` ```View```s being created, the ```Fragment``` may have data to bind before it has to ```Views``` to bind them to.

In order to ensure that the ```Fragment``` doesn't attempt to bind data to a null ```View```, the ```Fragment``` has flag set when the its ```Views``` are created, and set back when its ```Views``` are destroyed.

**ViewSafeFragment.java**
``` java
package com.tpb.projects.common;

import android.support.v4.app.Fragment;

/**
 * Created by theo on 11/04/17.
 */

public class ViewSafeFragment extends Fragment {

    protected boolean mAreViewsValid;

    protected boolean areViewsValid() {
        return mAreViewsValid && getActivity() != null;
    }

    @Override
    public void onDestroyView() {
        super.onDestroyView();
        mAreViewsValid = false;
    }

}

``` 

In a concrete instance of ```ViewSafeFragment``` the mAreViewsValid flag should be set after inflation in ```onCreateView``` and used to check ```View``` validity before performing any binding.

### FabHideScrollListener

A ```FloatingActionButton``` is a button which, as its name suggests, floats over other ```Views```. It is often positioned in the bottom right of a screen to provide a button for the 
primary action.
The floating nature of the button can cause problems when it is displayed over a ```RecyclerView``` as it obscures the bottom most item.

To solve this the ```FloatingActionButton``` should hide when the ```RecyclerView``` scrolls down, and show again when it scrolls back up.

**FabHideScrollListener.java**
``` java
package com.tpb.projects.common.fab;

import android.support.v7.widget.RecyclerView;

/**
 * Created by theo on 25/03/17.
 */

public class FabHideScrollListener extends RecyclerView.OnScrollListener {

    private FloatingActionButton mFab;

    public FabHideScrollListener(FloatingActionButton fab) {
        mFab = fab;
    }

    public void setFab(FloatingActionButton fab) {
        mFab = fab;
    }

    @Override
    public void onScrolled(RecyclerView recyclerView, int dx, int dy) {
        super.onScrolled(recyclerView, dx, dy);
        if(mFab == null) return;
        if(dy > 10) {
            mFab.hide(true);
        } else if(dy < -10) {
            mFab.show(true);
        }
    }
}

```

The ```FabHideScrollListener``` extends  ```RecyclerView.OnScrollListener``` and overrides ```onScrolled```, checking if the change in the y value is sufficient to indicate scrolling.

<div style="page-break-after: always;"></div>


### SimpleTextChangeWatcher

The ```SimpleTextChangeWatcher``` is a simplified abstract implementation of ```TextWatcher``` which forwards the ```onTextChanged``` call to ```textChanged``` without any parameters,
rather than requiring the implementation of ```beforeTextChanged```, ```onTextChanged```, and ```afterTextChanged``` when the logic only needs to know that the text has changed.

**SimpleTextChangeWatcher.java**
``` java
package com.tpb.projects.util.input;

import android.text.Editable;
import android.text.TextWatcher;

/**
 * Created by theo on 24/02/17.
 * Simplified TextWatcher which updates onTextChanged
 */

public abstract class SimpleTextChangeWatcher implements TextWatcher {

    @Override
    public final void beforeTextChanged(CharSequence s, int start, int count, int after) {
    }

    @Override
    public final void afterTextChanged(Editable s) {
    }

    @Override
    public final void onTextChanged(CharSequence s, int start, int before, int count) {
        textChanged();
    }

    /**
     * Called onTextChanged
     */
    public abstract void textChanged();
}

```

<div style="page-break-after: always;"></div>

### KeyBoardVisibilityChecker

A long running gripe with Android's text input system is that there is no standard way to detect whether the keyboard is currently visible, or listen for when its visibility changes.

This functionality is achieved by listening for changes on the ```ViewTreeObserver``` of the root ```View``` of an ```Activity``` and comparing it to the display frame size.

**KeyBoardVisibilityChecker.java**
``` java
package com.tpb.projects.util.input;

import android.graphics.Rect;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.view.View;

/**
 * Created by theo on 21/02/17.
 * Utility for listening for keyboard state
 */

public class KeyBoardVisibilityChecker {

    private boolean mIsKeyboardOpen = false;

    public KeyBoardVisibilityChecker(@NonNull View content) {
        this(content, null);
    }

    public KeyBoardVisibilityChecker(@NonNull View content, @Nullable KeyBoardVisibilityListener listener) {
        content.getViewTreeObserver().addOnGlobalLayoutListener(() -> {
            final Rect r = new Rect();
            content.getWindowVisibleDisplayFrame(r);
            final int screenHeight = content.getRootView().getHeight();

            // r.bottom is the position above soft keypad or device button.
            // if keypad is shown, the r.bottom is smaller than that before.
            final int kbHeight = screenHeight - r.bottom;

            if(kbHeight > screenHeight * 0.15) { // 0.15 ratio is perhaps enough to determine keypad height.
                mIsKeyboardOpen = true;
                if(listener != null) listener.keyboardShown();
            } else {
                mIsKeyboardOpen = false;
                if(listener != null) listener.keyboardHidden();
            }
        });
    }

    public boolean isKeyboardOpen() {
        return mIsKeyboardOpen;
    }

    /**
     * Interface for listening to {@link KeyBoardVisibilityChecker}
     */
    public interface KeyBoardVisibilityListener {

        void keyboardShown();

        void keyboardHidden();

    }

}

```

The ```KeyBoardVisibilityChecker``` adds an ```onGlobalLayoutListener``` which checks the size of the window with ```getWindowVisibleDisplayFrame```.
This method applies the dimensions of "the overall visible display size in which the window this view is attached to has been positioned in" to a given ```Rect```.

When the keyboard is shown, the root content layout is pushed upward and resized. The bottom of the content layout is therefore at the same position as the top of the keyboard.

Next the screen height is found from the height of the root ```View``` returned by the content layout.

If the calculated height is greater than 15% of the screen, it can be assumed that the keyboard is showing.

The ```KeyBoardVisibilityListener``` is an interface which can be used to listen for changes in keyboard visibility.

<div style="page-break-after: always;"></div>

### Utility methods

The ```Util``` class contains numerous utility methods for formatting and finding array indices.

The ```indexOf``` methods are used to find the index of a value within an array of integers, strings, pairs, or a generic type.
Each method performs a linear search for the key item, returning -1 if it does not exist.

The  ```formatBytes``` and ```formatKB``` methods are used to format a number of bytes or kilobytes into a 2 decimal place string representation of the highest unit suffix.

The next method, ```formatDateLocally``` is used to format a date in the expected manner for the device locale.

```isNotNullOrEmpty``` is used to check that a string is not null, not empty, and not a "null" string returned from a ```JSONObject```.

Finally, the ```insertString``` methods are used to insert a string at the currently selected position in an ```EditText```, before moving the cursor to the end of the inserted string, or to a provided offset.

<div style="page-break-after: always;"></div> 

### User Interface utility methods

The ```UI``` class contains numerous helper methods for performing unit conversions as well as helping with ```View``` animations.

**UI.java**
``` java
package com.tpb.projects.util;

import android.animation.ArgbEvaluator;
import android.animation.ObjectAnimator;
import android.app.Activity;
import android.content.Context;
import android.content.Intent;
import android.content.res.Resources;
import android.graphics.drawable.BitmapDrawable;
import android.support.annotation.ColorInt;
import android.support.annotation.Dimension;
import android.support.annotation.NonNull;
import android.support.annotation.Px;
import android.support.v4.util.Pair;
import android.util.TypedValue;
import android.view.View;
import android.view.ViewGroup;
import android.view.Window;
import android.view.WindowManager;
import android.view.animation.Transformation;
import android.widget.ImageView;

import com.tpb.projects.R;
import com.tpb.projects.common.CircularRevealActivity;

/**
 * Created by theo on 16/12/16.
 */

public class UI {
    /**
     * Sets the expansion point for a {@link CircularRevealActivity} Intent
     * to the midpoint of a View
     *
     * @param i    The Intent to launch a {@link CircularRevealActivity} instance
     * @param view The View instance which was clicked
     */
    public static void setViewPositionForIntent(Intent i, View view) {
        final int[] pos = new int[2];
        view.getLocationOnScreen(pos);
        pos[0] += view.getWidth() / 2;
        pos[1] += view.getHeight() / 2;
        i.putExtra(view.getContext().getString(R.string.intent_position_x), pos[0]);
        i.putExtra(view.getContext().getString(R.string.intent_position_y), pos[1]);
    }

    /**
     * @param context Required to get string resource values
     * @param i       The Intent to launch a {@link CircularRevealActivity} instance
     * @param pos     The x and y coordinates of the click
     */
    public static void setClickPositionForIntent(Context context, Intent i, float[] pos) {
        i.putExtra(context.getString(R.string.intent_position_x), (int) pos[0]);
        i.putExtra(context.getString(R.string.intent_position_y), (int) pos[1]);
    }

    public static void expand(final View v) {
        v.measure(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT);
        final int targetHeight = v.getMeasuredHeight();

        // Older versions of android (pre API 21) cancel animations for views with a height of 0.
        v.getLayoutParams().height = 1;
        v.setVisibility(View.VISIBLE);
        final android.view.animation.Animation a = new android.view.animation.Animation() {
            @Override
            protected void applyTransformation(float interpolatedTime, Transformation t) {
                v.getLayoutParams().height = interpolatedTime == 1
                        ? ViewGroup.LayoutParams.WRAP_CONTENT
                        : (int) (targetHeight * interpolatedTime);
                v.requestLayout();
            }

            @Override
            public boolean willChangeBounds() {
                return true;
            }
        };

        // 1dp/ms
        a.setDuration(
                (int) (targetHeight / v.getContext().getResources().getDisplayMetrics().density));
        v.startAnimation(a);
    }

    public static void collapse(final View v) {
        final int initialHeight = v.getMeasuredHeight();

        android.view.animation.Animation a = new android.view.animation.Animation() {
            @Override
            protected void applyTransformation(float interpolatedTime, Transformation t) {
                if(interpolatedTime == 1) {
                    v.setVisibility(View.GONE);
                } else {
                    v.getLayoutParams().height = initialHeight - (int) (initialHeight * interpolatedTime);
                    v.requestLayout();
                }
            }

            @Override
            public boolean willChangeBounds() {
                return true;
            }
        };

        // 1dp/ms
        a.setDuration(
                (int) (initialHeight / v.getContext().getResources().getDisplayMetrics().density));
        v.startAnimation(a);
    }

    /**
     * Fades the background color of a View from original to flash and back
     *
     * @param view     The view to flash
     * @param original The current background color
     * @param flash    The color to fade to
     */
    public static void flashViewBackground(View view, @ColorInt int original, @ColorInt int flash) {
        final ObjectAnimator colorFade = ObjectAnimator.ofObject(
                view,
                "backgroundColor",
                new ArgbEvaluator(),
                original,
                flash
        );
        colorFade.setDuration(300);
        colorFade.setRepeatMode(ObjectAnimator.REVERSE);
        colorFade.setRepeatCount(1);
        colorFade.start();

    }

    @Dimension
    public static float dpFromPx(final float px) {
        return px / Resources.getSystem().getDisplayMetrics().density;
    }

    @Px
    public static int pxFromDp(final float dp) {
        return Math.round(dp * Resources.getSystem().getDisplayMetrics().density);
    }

    @Px
    public static int pxFromDp(final int dp) {
        return (int) (dp * Resources.getSystem().getDisplayMetrics().density);
    }

    @Px
    public static int pxFromSp(final float sp) {
        return (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_SP, sp,
                Resources.getSystem().getDisplayMetrics()
        );
    }

    public static void setStatusBarColor(Window window, @ColorInt int color) {
        // clear FLAG_TRANSLUCENT_STATUS flag:
        window.clearFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS);
        // add FLAG_DRAWS_SYSTEM_BAR_BACKGROUNDS flag to the window
        window.addFlags(WindowManager.LayoutParams.FLAG_DRAWS_SYSTEM_BAR_BACKGROUNDS);
        // finally change the color
        window.setStatusBarColor(color);
    }

    /**
     * Checks whether the device has a navigation bar, and if so returns a pair
     * for {@see ActivityOptionsCompat#makeSceneTransition}
     *
     * @param activity Any activity in which to find the navigation bar
     * @return The navigation bar view view pair, or an empty view pair
     */
    public static Pair<View, String> getSafeNavigationBarTransitionPair(@NonNull Activity activity) {
        final View nav = activity.findViewById(android.R.id.navigationBarBackground);
        return nav == null ?
                new Pair<>(new View(activity), "not_for_transition") :
                new Pair<>(nav, Window.NAVIGATION_BAR_BACKGROUND_TRANSITION_NAME);
    }


    public static void setDrawableForIntent(@NonNull ImageView iv, @NonNull Intent i) {
        if(iv.getDrawable() instanceof BitmapDrawable) {
            i.putExtra(iv.getResources().getString(R.string.intent_drawable),
                    ((BitmapDrawable) iv.getDrawable()).getBitmap()
            );
        }
    }


}

```

The first two methods, ```setViewPositionForIntent``` and ```setClickPositionForIntent``` are used when passing the position of a ```View``` or the position of a click to a new 
```Activity```, allowing it to launch form a point.

The ```expand``` and ```collapse``` methods are used to animate ```Views```.
```expand``` animates a ```View``` from no height to its measured height, and ```collapse``` shrinks a ```View``` from its measured height to no height before hiding it. 

```flashViewBackground``` is a method used to fade the background colour of a ```View``` from its original colour to a highlight colour, and back again.
This method can be used to highlight an important ```View```, such as when jumping to a search result.

The next four methods are used for unit conversion, converting pixels to density independent pixels, as well as converting density independent pixels or scale independent pixels to pixels.

As its name suggest, ```setStatusBarColor``` is used to set the status bar color for a ```Window```, which is required if the ```Activity``` uses a theme with transparency.

Finally, ```getSafeNavigationBarTransitionPair```is a utility for shared element transitions.
When a ```View``` in a ```RecyclerView``` is used in a shared element transition between two ```Activities```, the ```View``` is drawn throught the ```ViewOverlay``` layer, which draws above the software navigation bar.
If a ```View``` is partially below the navigation bar it will jump in elevation to display above the navigation bar, and jump back under it on the return transition.
In order to prevent this jumpy transition, the navigation bar can be added to the transition, resulting in it being drawn in the ```ViewOverlay``` above the transitioning ```View```.

The navigation bar can be found by searching an ```Activity``` for the id ```android.R.id.navigationBarBackground```, however it may not exist.
Many devices, notably Samsung phones, do not use software navigation keys, and the null ```View``` returned from ```findViewById``` would result in a crash.

In order to prevent this, a ```Pair``` containing an empty ```View``` instance with an unused key is returned if the navigation bar does not exist.


<div style="page-break-after: always;"></div>

### Logging

As explained in the analysis, logs are printed with the ```Log``` class and are a useful method of debugging. However, log messages should only be shown in the debug variant of the
application.
The ```Logger``` class wraps the methods in ```Log``` with checks for the ```BuildConfig``` flag ```IS_IN_DEBUG```.

**Logger.java**
``` java
package com.tpb.projects.util;

import android.annotation.SuppressLint;
import android.util.Log;

import java.io.IOException;

import okhttp3.Interceptor;
import okhttp3.Request;
import okhttp3.Response;

/**
 * Created by theo on 11/03/17.
 */

public class Logger {
    private static final boolean DEBUG = com.tpb.projects.BuildConfig.IS_IN_DEBUG;

    public static void logLong(String TAG, String s) {
        if(s.length() > 4000) {
            Log.d(TAG, s.substring(0, 4000));
            logLong(TAG, s.substring(4000));
        } else
            Log.d(TAG, s);
    }

    public static void v(String tag, String msg) {
        if(DEBUG) Log.v(tag, msg);
    }

    public static void v(String tag, String msg, Throwable tr) {
        if(DEBUG) Log.v(tag, msg, tr);
    }

    public static void d(String tag, String msg) {
        if(DEBUG) Log.d(tag, msg);
    }

    public static void d(String tag, String msg, Throwable tr) {
        if(DEBUG) Log.d(tag, msg, tr);
    }

    public static void w(String tag, String msg) {
        if(DEBUG) Log.w(tag, msg);
    }

    public static void w(String tag, String msg, Throwable tr) {
        if(DEBUG) Log.w(tag, msg, tr);
    }

    public static void w(String tag, Throwable tr) {
        if(DEBUG) Log.w(tag, tr);
    }

    public static void i(String tag, String msg) {
        if(DEBUG) Log.i(tag, msg);
    }

    public static void i(String tag, String msg, Throwable tr) {
        if(DEBUG) Log.i(tag, msg, tr);
    }

    public static void e(String tag, String msg) {
        if(DEBUG) Log.e(tag, msg);
    }

    public static void e(String tag, String msg, Throwable tr) {
        if(DEBUG) Log.e(tag, msg, tr);
    }

    public static class LoggingInterceptor implements Interceptor {

        private static final String TAG = LoggingInterceptor.class.getSimpleName();

        @SuppressLint("DefaultLocale")
        @Override
        public Response intercept(Chain chain) throws IOException {
            final Request request = chain.request();
            final long ts = System.nanoTime();
            Logger.i(TAG, String.format("Sending request %s on %s%n%s",
                    request.url(), chain.connection(), request.headers()
            ));

            final Response response = chain.proceed(request);

            Logger.i(TAG, String.format("Received response for %s in %.1fms%n%s",
                    response.request().url(), (System.nanoTime() - ts) / 1e6d, response.headers()
            ));

            return response;
        }
    }

}

```

The ```Logger``` class also contains the ```LoggingInterceptor``` class which is a network interceptor used to log all network request made throughout the app.

The ```LoggingInterceptor``` is added in the ```ProjectsApplication``` class.
It produces two log messages for each call, the first defailts the request being sent, for example a request to the notifications API:
```
Sending request https://api.github.com/notifications on Connection{api.github.com:443, proxy=DIRECT@ hostAddress=api.github.com/192.30.253.116:443 cipherSuite=TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 protocol=http/1.1}
                                       Accept: application/vnd.github.v3+json
                                       Authorization: token an_authorization_token
                                       Cache-Control: no-store
                                       Host: api.github.com
                                       Connection: Keep-Alive
                                       Accept-Encoding: gzip
                                       User-Agent: okhttp/3.6.0
```

and the received response:
```
Received response for https://api.github.com/notifications in 419.7ms
                                       Server: GitHub.com
                                       Date: Tue, 11 Apr 2017 23:36:57 GMT
                                       Content-Type: application/json; charset=utf-8
                                       Content-Length: 2
                                       Status: 200 OK
                                       X-RateLimit-Limit: 5000
                                       X-RateLimit-Remaining: 4959
                                       X-RateLimit-Reset: 1491955996
                                       Cache-Control: private, max-age=60, s-maxage=60
                                       Vary: Accept, Authorization, Cookie, X-GitHub-OTP
                                       ETag: "3ef243829743ac436b782dbd8981e769"
                                       X-Poll-Interval: 60
                                       X-OAuth-Scopes: gist, repo, user
                                       X-Accepted-OAuth-Scopes: notifications, repo
                                       X-OAuth-Client-Id: the_client_id_of_the_app
                                       X-GitHub-Media-Type: github.v3; format=json
                                       Access-Control-Expose-Headers: ETag, Link, X-GitHub-OTP, X-RateLimit-Limit, X-RateLimit-Remaining, X-RateLimit-Reset, X-OAuth-Scopes, X-Accepted-OAuth-Scopes, X-Poll-Interval
                                       Access-Control-Allow-Origin: *
                                       Content-Security-Policy: default-src 'none'
                                       Strict-Transport-Security: max-age=31536000; includeSubdomains; preload
                                       X-Content-Type-Options: nosniff
                                       X-Frame-Options: deny
                                       X-XSS-Protection: 1; mode=block
                                       Vary: Accept-Encoding
                                       X-Served-By: 07ff1c8a09e44b62e277fae50a1b1dc4
                                       X-GitHub-Request-Id: A8DC:2A61:2B933E:372939:58ED6898
```

### Analytics

In order to help debug crashes which happen when not connected to a computer, I have chosen to incorporate Google's FireBase analytics service to log crashes and other events.

If a crash occurs, the information about the crash is exported and can then be to debug the issue later.

Each issue shows the version codes for which it occured, as well as more detailed information about the circumstances of the crash.

![Issue info](http://imgur.com/7yqPZjf.png)

The crash information contains the full stack trace as well as information about the device on which the crash occured.

![Device info](http://imgur.com/tPfFzWS.png)

## Markdown

As GitHub uses Markdown throughout its content, a method for displaying Markdown must be implemented before the creation of the rest of the user interface.

GitHub flavoured Markdown follows the CommonMark specification while extending it with extra features, however these features are not present in the Markdown returned from the GitHub
API.

The simplest way to display Markdown would be to display the Markdown as HTML in a ```WebView```, however the performance of a ```WebView``` is not acceptable when displaying
multiple different sections of text.

Instead, the Markdown must be displayed in a ```TextView```. The ```TextView``` has no native support for Markdown formatting, nor does it have direct support for HTML.
In order to display styled text without applying the styling to the entire text body, spans are used.

### Spans

The ```Spanned``` interface is "the interface for text that has markup objects attached to ranges of it.".

There are three key interfaces and an abstract class for different types of span used in the ```TextView```.

#### CharacterStyle

The abstract class ```CharacterStyle``` is used for spans which affect character level text formatting.

```CharacterStyle``` has three methods.

The first, ```updateDrawState``` takes the ```TextPaint``` used to draw the ```TextView``` in order to allow changes to be made to the text styling.

The second and third methods are used when a single ```CharacterStyle``` needs to be applied to multiple different regions of a ```Spanned```.
The ```wrap``` method takes a ```CharacterStyle``` which will actually perform the text manipulation. When a ```CharacterStyle``` is used, the ```getUnderlying``` method is called, which
should return `this` for instances which actually perform manipulation, or return the wrapping ```CharacterStyle``` for spans which are only placeholders for actual implementations.

Implementations of ```CharacterStyle``` include ```BackgroundColorSpan```, ```ForegroundColorSpan```, and ```UnderLineSpan``` which each change a single parameter on ```TextPaint```.

#### ParagraphStyle

As its name suggest, ```ParagraphStyle``` affects paragraph level text formatting.

The interface has no methods, and is instead used only as a marker to indicate that the span or span interface affects text at a wider level.

Direct span implemenations of ```ParagraphStyle``` include ```BulletSpan```, ```QuoteSpan```, and ```DrawableMarginSpan```, while numerous interfaces such as ```AlignmentSpan``` and ```LeadingMarginSpan``` also extend from it.

#### UpdateAppearance

The ```UpdateAppearance``` interface is for spans which affect character-level text "in a way that modifies their appearance when one is added or removed".

Implementations include ```AbsoluteSizeSpan``` which is used to scae text using either density independent pixels, or an absolute pixel size.

#### UpdateLayout

The ```UpdateLayout``` interface is an extension of ```UpdateApperance``` with the difference being that the modifications made by an implementation of ```UpdateLayout``` are such that a
text layout update is triggered when one is added or removed.

Implementations include ```SuperscriptSpan```, ```SubscriptSpan```, and ```ImageSpan```.


#### ReplacementSpan

An important implementation of ```CharacterStyle``` is ```ReplacementSpan```.

Rather than simply allowing updates to ```TextPaint```, the ```ReplacementSpan``` is able to draw directly to the canvas.

The two abstract methods defined in ```ReplacementSpan``` are ```getSize``` and ```draw```.

```getSize``` takes a ```Paint``` object, the ```CharSequence``` displayed in the ```TextView```, the start and end 
positions of the span within the ```CharSequence```, and the ```FontMetrics``` being used to draw the text. Is is expected to return the width of the span.

```draw``` takes the ```Canvas```, the ```CharSequence``` displayed in the ```TextView```, the start and end positions of the span within the ```CharSequence```, a ```Paint``` object, and
the x, y, top, and bottom positions of the span on the ```Canvas```.



### Implementing GitHub Markdown features

#### General strategy

None of the markings used are more than three characters in length, meaning that at any one time only the previous two characters need be retained.

The formatted markdown is to be appended to a ```StringBuilder``` as the array of characters in the markdown is formatted.
Each format method is to take the character array, current position, and the ```StringBuilder``` and attempt to append the formatted markdown to the ```StringBuilder``` before returning
the new position to continue from in the character array.

**Markdown.java**
``` java
static String formatMD(@NonNull String s, @Nullable String fullRepoPath, boolean linkUsernames) {
        final StringBuilder builder = new StringBuilder();
        char p = ' ';
        char pp = ' ';
        final char[] chars = ("\n" + s).toCharArray();
        for(int i = 0; i < chars.length; i++) {
            if(linkUsernames && chars[i] == '@' && isWhiteSpace(p)) {
                //Max username length is 39 characters
                //Usernames can be alphanumeric with single hyphens
                i = parseUsername(builder, chars, i);
            } else if(chars[i] == '#' && isWhiteSpace(p) && fullRepoPath != null) {
                i = parseIssue(builder, chars, i, fullRepoPath);
            } else if(pp == '[' && (p == 'x' || p == 'X') && chars[i] == ']') {
                builder.setLength(builder.length() - 2);
                builder.append("\u2611");  //☑ ballot box with check
            } else if(p == '[' && chars[i] == ']') { //Closed box
                builder.setLength(builder.length() - 1);
                builder.append("\u2610"); //☐ ballot box
            } else if(pp == '[' && p == ' ' && chars[i] == ']') {//Open box
                builder.setLength(builder.length() - 2);
                builder.append("\u2610");
            } else if(chars[i] == '(') {
                builder.append("(");
                i = parseImageLink(builder, chars, i, fullRepoPath);
            } else if(chars[i] == ':') {
                i = parseEmoji(builder, chars, i);
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
            } else {
                builder.append(chars[i]);
            }
            pp = p;
            p = chars[i];
        }
        return builder.toString();
    }
```

**Markdown.java**
``` java
boolean isWhiteSpace(char c) {
        //Space tab, newline, line tabulation, carriage return, form feed
        return c == ' ' || c == '\t' || c == '\n' || c == '\u000B' || c == '\r' || c == '\u000C';
    }
```

**Markdown.java**
``` java
boolean isLineEnding(char[] cs, int i) {
        return i == cs.length - 1 || cs[i] == '\n' || cs[i] == '\r';
    }
```

```isWhiteSpace``` and ```isLineEnding``` are both utility methods. ```isWhiteSpace``` checks if the character is a space, a tab, a newline, a line tabulation character, a carriage return, or a form feed, while ```isLineEnding``` checks whether the character is a new line or carriage return or the position is the last in the character array.

The ```formatMD``` method the Markdown ```String```, an optional repository path, and a flag for linking usernames.

Within each iteration, the first check is for usernames.
If the current character is the "@" key used for usernames, and the previous character is whitespace the position is jumped with a call to ```parseUsername```.
If the current character is the "#" key used for issues, and the previous character is whitespace, and the repository name is non-null the position is jumped with a call to ```parseIssue```.

The next three checks deal with formatting GitHub's checkbox lists to use ballot characters rather than non-formatted [ ] and [x] sequences.
The first check is for an upper or lowercase "x" contained between two square braces. The last two two characters are removed from the builder and the unicode "ballot box with check" is added.
The second two checks are both for ballot boxes without checks, either written as "[]" or "[ ]".

The next check is for image links, which need to be parsed both in order to deal with links relative to the repository and to add spacing around them as they will be displayed as images.

The next check is for emojis, which are contained between two colons.

If there is a sequence of three backticks, every character from the backticks onward is appended without formatting until the next set of backticks is found.

Finally, if none of the above conditions apply, the character is appended to the builder.

At the end of each iteration the previous and previous previous characters are updated.
#### Username menetions

GitHub usernames are strings of text up to 39 characters in length, containing only alphanumeric characters and hypens.


## User Activity

Once a user has logged in, their account can be displayed.
This is objective 2.

The immediate sub-objectives of objective 2 are written to represent the different components shown when displaying a user in a paged ```Activity``` as described in the proposed design.

Each section will be shown as a ```Fragment``` within a ```ViewPager```.
This makes the ```UserActivity``` layout and class simple, as most logic is kept separate, with each ```Fragment``` only concerned with its own purpose.

**activity_user.xml**
``` xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <android.support.design.widget.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <android.support.v7.widget.Toolbar
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:layout_scrollFlags="scroll|enterAlwaysCollapsed">

            <!--suppress AndroidMissingOnClickHandler -->
            <ImageButton
                android:id="@+id/back_button"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:background="@android:color/transparent"
                android:src="@drawable/ic_arrow_back"
                android:onClick="onToolbarBackPressed"
                android:contentDescription="@string/content_description_back"
                android:layout_marginStart="16dp"/>

            <TextView
                android:id="@+id/title_user"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/title_activity_user"
                android:layout_marginStart="16dp"
                android:layout_marginEnd="16dp"
                android:textAppearance="@android:style/TextAppearance.Material.Title"/>

        </android.support.v7.widget.Toolbar>

        <android.support.design.widget.TabLayout
            android:id="@+id/user_fragment_tablayout"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_gravity="center_horizontal"
            app:layout_scrollFlags="scroll|snap|enterAlways"
            app:tabMode="scrollable"
            app:tabGravity="center">

        </android.support.design.widget.TabLayout>

    </android.support.design.widget.AppBarLayout>

    <android.support.v4.view.ViewPager
        android:id="@+id/user_fragment_viewpager"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">

    </android.support.v4.view.ViewPager>


</android.support.design.widget.CoordinatorLayout>
```

The ```UserActivity``` layout contains an ```AppBarLayout``` allowing the ```Toolbar``` to scroll with the contents of any ```ScrollViews``` within the ```Fragments``` which are contained in the ```ViewPager```.

**UserActivity.java**
``` java
package com.tpb.projects.user;

import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import android.support.annotation.Nullable;
import android.support.design.widget.TabLayout;
import android.support.v4.app.Fragment;
import android.support.v4.app.FragmentManager;
import android.support.v4.app.FragmentPagerAdapter;
import android.support.v4.view.ViewPager;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.widget.TextView;

import com.androidnetworking.AndroidNetworking;
import com.tpb.github.data.APIHandler;
import com.tpb.github.data.Loader;
import com.tpb.github.data.auth.GitHubSession;
import com.tpb.github.data.models.User;
import com.tpb.projects.R;
import com.tpb.projects.common.BaseActivity;
import com.tpb.projects.common.ShortcutDialog;
import com.tpb.projects.user.fragments.UserFollowersFragment;
import com.tpb.projects.user.fragments.UserFollowingFragment;
import com.tpb.projects.user.fragments.UserFragment;
import com.tpb.projects.user.fragments.UserGistsFragment;
import com.tpb.projects.user.fragments.UserInfoFragment;
import com.tpb.projects.user.fragments.UserReposFragment;
import com.tpb.projects.user.fragments.UserStarsFragment;
import com.tpb.projects.util.SettingsActivity;

import butterknife.BindView;
import butterknife.ButterKnife;

/**
 * Created by theo on 10/03/17.
 */

public class UserActivity extends BaseActivity implements Loader.ItemLoader<User> {
    private static final String TAG = UserActivity.class.getSimpleName();
    private static final String URL = "https://github.com/tpb1908/AndroidProjectsClient/blob/master/app/src/main/java/com/tpb/projects/user/UserActivity.java";

    @BindView(R.id.title_user) TextView mTitle;
    @BindView(R.id.user_fragment_tablayout) TabLayout mTabs;
    @BindView(R.id.user_fragment_viewpager) ViewPager mPager;

    private UserFragmentAdapter mAdapter;
    private User mUser;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        if(!mHasAccess) return;
        setTheme(SettingsActivity.Preferences.getPreferences(this).isDarkThemeEnabled()
                ? R.style.AppTheme_Dark : R.style.AppTheme);
        setContentView(R.layout.activity_user);
        ButterKnife.bind(this);
        postponeEnterTransition();

        if(mAdapter == null) mAdapter = new UserFragmentAdapter(getSupportFragmentManager());
        final Loader loader = Loader.getLoader(this);

        if(getIntent() != null && getIntent().hasExtra(getString(R.string.intent_username))) {
            final String user = getIntent().getStringExtra(getString(R.string.intent_username));
            mTitle.setText(user);
            loader.loadUser(this, user);
        } else {
            if(isTaskRoot()) {
                findViewById(R.id.back_button).setVisibility(View.GONE);
            }
            loadComplete(GitHubSession.getSession(this).getUser());
        }
        mTabs.setupWithViewPager(mPager);
        mPager.setAdapter(mAdapter);
        mPager.setOffscreenPageLimit(7);

    }

    @Override
    public void loadComplete(User user) {
        mUser = user;
        mTitle.setText(mUser.getLogin());
        mAdapter.notifyUserLoaded();
    }

    @Override
    public void loadError(APIHandler.APIError error) {

    }

    @Override
    public void onAttachFragment(Fragment fragment) {
        super.onAttachFragment(fragment);
        if(mAdapter == null) mAdapter = new UserFragmentAdapter(getSupportFragmentManager());
        mAdapter.ensureAttached((UserFragment) fragment);
    }

    private class UserFragmentAdapter extends FragmentPagerAdapter {

        private UserFragment[] mFragments = new UserFragment[6];

        UserFragmentAdapter(FragmentManager fm) {
            super(fm);
        }

        @Override
        public Fragment getItem(int position) {
            switch(position) {
                case 0:
                    mFragments[0] = new UserInfoFragment();
                    break;
                case 1:
                    mFragments[1] = new UserReposFragment();
                    break;
                case 2:
                    mFragments[2] = new UserStarsFragment();
                    break;
                case 3:
                    mFragments[3] = new UserGistsFragment();
                    break;
                case 4:
                    mFragments[4] = new UserFollowingFragment();
                    break;
                case 5:
                    mFragments[5] = new UserFollowersFragment();
                    break;
            }
            if(mUser != null) mFragments[position].userLoaded(mUser);
            return mFragments[position];
        }

        void ensureAttached(UserFragment fragment) {
            if(fragment instanceof UserInfoFragment) mFragments[0] = fragment;
            else if(fragment instanceof UserReposFragment) mFragments[1] = fragment;
            else if(fragment instanceof UserStarsFragment) mFragments[2] = fragment;
            else if(fragment instanceof UserGistsFragment) mFragments[3] = fragment;
            else if(fragment instanceof UserFollowingFragment) mFragments[4] = fragment;
            else if(fragment instanceof UserFollowersFragment) mFragments[5] = fragment;
        }

        void notifyUserLoaded() {
            for(UserFragment f : mFragments) {
                if(f != null) f.userLoaded(mUser);
            }
        }

        @Override
        public int getCount() {
            return mFragments.length;
        }

        @Override
        public CharSequence getPageTitle(int position) {
            switch(position) {
                case 0:
                    return getString(R.string.title_user_info_fragment);
                case 1:
                    return getString(R.string.title_user_repos_fragment);
                case 2:
                    return getString(R.string.title_user_stars_fragment);
                case 3:
                    return getString(R.string.title_user_gists_fragment);
                case 4:
                    return getString(R.string.title_user_following_fragment);
                case 5:
                    return getString(R.string.title_user_followers_fragment);
                default:
                    return "Error";
            }
        }
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        //TODO Pass to fragment
        getMenuInflater().inflate(R.menu.menu_activity, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch(item.getItemId()) {
            case R.id.menu_settings:
                startActivity(new Intent(UserActivity.this, SettingsActivity.class));
                break;
            case R.id.menu_source:
                startActivity(new Intent(Intent.ACTION_VIEW, Uri.parse(URL)));
                break;
            case R.id.menu_share:
                final Intent share = new Intent();
                share.setAction(Intent.ACTION_SEND);
                share.putExtra(Intent.EXTRA_TEXT, mUser.getHtmlUrl());
                share.addCategory(Intent.CATEGORY_BROWSABLE);
                share.setType("text/plain");
                startActivity(share);
                break;
            case R.id.menu_save_to_homescreen:
                final ShortcutDialog dialog = new ShortcutDialog();
                final Bundle args = new Bundle();
                args.putInt(getString(R.string.intent_title_res), R.string.title_save_user_shortcut);
                args.putString(getString(R.string.intent_name), mUser.getLogin());

                dialog.setArguments(args);
                dialog.setListener((name, iconFlag) -> {
                    final Intent i = new Intent(getApplicationContext(), UserActivity.class);
                    i.putExtra(getString(R.string.intent_username), mUser.getLogin());

                    final Intent add = new Intent();
                    add.putExtra(Intent.EXTRA_SHORTCUT_INTENT, i);
                    add.putExtra(Intent.EXTRA_SHORTCUT_NAME, name);
                    add.putExtra("duplicate", false);
                    add.putExtra(Intent.EXTRA_SHORTCUT_ICON_RESOURCE, Intent.ShortcutIconResource.fromContext(getApplicationContext(), R.mipmap.ic_launcher));
                    add.setAction("com.android.launcher.action.INSTALL_SHORTCUT");
                    getApplicationContext().sendBroadcast(add);
                });
                dialog.show(getSupportFragmentManager(), TAG);
                break;
        }
        return true;
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        AndroidNetworking.cancelAll();
    }
}

```

**fragment_user_info.xml**
``` xml
<?xml version="1.0" encoding="utf-8"?>

<android.support.v4.widget.SwipeRefreshLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/user_info_refresher"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:layout_behavior="@string/appbar_scrolling_view_behavior">

    <android.support.v4.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                      xmlns:app="http://schemas.android.com/apk/res-auto"
                      android:orientation="vertical"
                      android:layout_width="match_parent"
                      android:layout_height="match_parent">

            <android.support.v7.widget.CardView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="vertical"
                android:layout_margin="8dp">

                <include layout="@layout/shard_user_info"/>

            </android.support.v7.widget.CardView>

            <android.support.v7.widget.CardView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="vertical"
                android:layout_margin="8dp">

                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:layout_marginStart="16dp"
                    android:layout_marginEnd="16dp"
                    android:layout_marginTop="8dp"
                    android:layout_marginBottom="8dp"
                    android:orientation="vertical">

                    <TextView
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/text_user_activity"
                        android:textAppearance="@android:style/TextAppearance.Material.Large"/>

                    <com.tpb.contributionsview.ContributionsView
                        android:id="@+id/user_contributions"
                        android:layout_width="match_parent"
                        android:layout_height="80dp"
                        app:textSize="12sp"
                        app:textColor="?android:textColorPrimary"/>

                    <TextView
                        android:id="@+id/user_contributions_info"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"/>

                </LinearLayout>

            </android.support.v7.widget.CardView>

        </LinearLayout>

    </android.support.v4.widget.NestedScrollView>

</android.support.v4.widget.SwipeRefreshLayout>


```

When it is created, ```UserActivity``` calls the ```super``` method (```BaseActivity```) which performs the access check, allowing ```UserActivity``` to return if the app doesn't have an access token.
If this check passes, ```onCreate``` method continues by checking the theme theme before inflating the ```activity_user``` layout and bindings its ```Views```. 

Once the ```Views``` have been inflated, the ```UserActivity``` can continue by creating the ```FragmentPagerAdapter``` which is responsible for creating each of the ```Fragments```.

The ```UserActivity``` then determines whether it has been started from a link to a user, in which case the ```User``` must be loaded, or if the app is starting from the homescreen to display the authenticated user.

## UserFragment

```UserFragment``` is an abstract class extending ```ViewSafeFragment``` used to add a ```User``` model instance, the ```userLoaded``` method , as well as to save and restore the ```User``` instance when the ```Fragment``` is added to or removed from the back stack.

**UserFragment.java**
``` java
package com.tpb.projects.user.fragments;

import android.os.Bundle;
import android.support.annotation.Nullable;

import com.tpb.github.data.models.User;
import com.tpb.projects.R;
import com.tpb.projects.common.ViewSafeFragment;
import com.tpb.projects.user.UserActivity;

/**
 * Created by theo on 10/03/17.
 */

public abstract class UserFragment extends ViewSafeFragment {

    protected User mUser;

    public abstract void userLoaded(User user);

    protected UserActivity getParent() {
        return (UserActivity) getActivity();
    }

    @Override
    public void onActivityCreated(@Nullable Bundle savedInstanceState) {
        super.onActivityCreated(savedInstanceState);
        if(savedInstanceState != null && savedInstanceState
                .containsKey(getString(R.string.parcel_user))) {
            mUser = savedInstanceState.getParcelable(getString(R.string.parcel_user));
            userLoaded(mUser);
        }
    }

    @Override
    public void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        outState.putParcelable(getString(R.string.parcel_user), mUser);
    }

}

```

### UserInfoFragment

The first ```Fragment``` to be displayed is the ```UserInfoFragment```.
The ```UserInfoFragment``` is to fulfill objective 2.a, displaying information about the user.

The ```UserInfoFragment``` layout has two cards, the first displaying text based information about the user, and the second displaying a graph of the users' contributions.

**fragment_user_info.xml**
``` xml
<?xml version="1.0" encoding="utf-8"?>

<android.support.v4.widget.SwipeRefreshLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/user_info_refresher"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:layout_behavior="@string/appbar_scrolling_view_behavior">

    <android.support.v4.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                      xmlns:app="http://schemas.android.com/apk/res-auto"
                      android:orientation="vertical"
                      android:layout_width="match_parent"
                      android:layout_height="match_parent">

            <android.support.v7.widget.CardView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="vertical"
                android:layout_margin="8dp">

                <include layout="@layout/shard_user_info"/>

            </android.support.v7.widget.CardView>

            <android.support.v7.widget.CardView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="vertical"
                android:layout_margin="8dp">

                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:layout_marginStart="16dp"
                    android:layout_marginEnd="16dp"
                    android:layout_marginTop="8dp"
                    android:layout_marginBottom="8dp"
                    android:orientation="vertical">

                    <TextView
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/text_user_activity"
                        android:textAppearance="@android:style/TextAppearance.Material.Large"/>

                    <com.tpb.contributionsview.ContributionsView
                        android:id="@+id/user_contributions"
                        android:layout_width="match_parent"
                        android:layout_height="80dp"
                        app:textSize="12sp"
                        app:textColor="?android:textColorPrimary"/>

                    <TextView
                        android:id="@+id/user_contributions_info"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"/>

                </LinearLayout>

            </android.support.v7.widget.CardView>

        </LinearLayout>

    </android.support.v4.widget.NestedScrollView>

</android.support.v4.widget.SwipeRefreshLayout>


```

The layout included within the first ```CardView``` is the same layout used in ```LoginActivity``` to display the user's information

**shard_user_info.xml**
``` xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    android:id="@+id/user_details"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:layout_gravity="top"
    xmlns:android="http://schemas.android.com/apk/res/android">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <com.tpb.projects.common.NetworkImageView
            android:id="@+id/user_avatar"
            android:layout_width="40dp"
            android:layout_height="40dp"
            android:layout_margin="16dp"
            android:layout_gravity="center_vertical"
            android:transitionName="@string/transition_user_image"/>

        <TextView
            android:id="@+id/user_login"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center_vertical"
            android:text="@string/text_placeholder"
            android:textAppearance="@android:style/TextAppearance.Material.Title"
            android:transitionName="@string/transition_username"/>

    </LinearLayout>

    <LinearLayout
        android:id="@+id/user_info_layout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:layout_marginEnd="16dp"
        android:layout_marginBottom="8dp"
        android:orientation="vertical">

    </LinearLayout>

</LinearLayout>
```

and contains a ```LinearLayout``` to display the user's avatar and username, as well as another ```LinearLayout``` to display a list of the user's available information.

Once inflated and bound with a ```User```'s information, the layout will be displayed as below.

![UserInfoFragment](http://imgur.com/N7QriPz.png)

#### Animation

In Android 5.0, released November 2014, shared element transitions were introduced.

These transitions allow a ```View``` to be shared between two ```Activities``` via the ```ViewOverlay``` which is drawn on top of all other ```Views```.
A common use if for an ```ImageView``` to animate between two ```Activities```. This works well so long as the ```ImageView``` maintains its aspect ratio.

A common case for the ```UserActivity``` being opened is when the user clicks on an avatar. The avatar can be animated from its original position to its new position in the ```UserActivity```. The user name can also be animated from one ```TextView``` to another.

If the ```Activity``` had a single layout inflated in its ```onCreate``` method, this animation would be simple to perform, and wouldn't require any Java code only XML attributes.
However, the ```NetworkImageView``` displaying the ```User``` avatar is shown in the ```UserInfoFragment``` which is inflated after the ```UserActivity```. Further, the image stored in 
the ```NetworkImageView``` in the launching ```Activity``` was loaded from a URL, and the new ```NetworkImageView``` in the ```UserInfoFragment``` would load the image again, defeating 
the purpose of the transition.

The first problem is solved by postponing the entry transition.
The line after ```View``` binding in ```UserActivity``` is the call ```postponeEnterTransition()``` which, as its name suggests, postpones the entry transition until the 
```startPostponedEnterTransition``` is called.

**UserInfoFragment.java**
``` java
View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        final View view = inflater.inflate(R.layout.fragment_user_info, container, false);
        unbinder = ButterKnife.bind(this, view);
        mRefresher.setRefreshing(true);
        mAvatar.getViewTreeObserver()
               .addOnPreDrawListener(new ViewTreeObserver.OnPreDrawListener() {
                   @Override
                   public boolean onPreDraw() {
                       if(getActivity().getIntent() != null && getActivity().getIntent().hasExtra(
                               getString(R.string.intent_drawable))) {
                           final Bitmap bm = getActivity().getIntent().getParcelableExtra(
                                   getString(R.string.intent_drawable));
                           mUserLogin.setText(getActivity().getIntent().getStringExtra(
                                   getString(R.string.intent_username)));
                           mAvatar.setImageBitmap(bm);
                       }
                       mAvatar.getViewTreeObserver().removeOnPreDrawListener(this);
                       getActivity().startPostponedEnterTransition();
                       return true;
                   }
               });
        mRefresher.setOnRefreshListener(
                () -> Loader.getLoader(getContext()).loadUser(new Loader.ItemLoader<User>() {
                    @Override
                    public void loadComplete(User user) {
                        userLoaded(user);
                    }

                    @Override
                    public void loadError(APIHandler.APIError error) {
                        mRefresher.setRefreshing(false);
                    }
                }, mUser.getLogin())
        );

        mAreViewsValid = true;
        if(mUser != null) userLoaded(mUser);
        return view;
    }
```

Once the ```View``` has been created in ```UserInfoFragment``` we have the ```NetworkImageView``` required to perform the transition.
However, the full layout has not been calculated, so the animation cannot yet be performed.

In order to wait until the layout has been completed, an ```OnPreDrawListener``` is added to the ```ViewTreeObserver``` of the ```NetworkImageView``` mAvatar.

When  ```onPreDraw``` is called, the listener checks that the the ```Intent``` contains the key ```intent_drawable```, and if it is present it sets the ```Bitmap``` of mAvatar 
as well as setting the text of the ```TextView``` mUserLogin.

Due to the nature of ```View``` layouts and drawing, adding a listener to the ```ViewTreeObserver``` is computationally expensive. It is therefore important to remove the 
```onPreDrawListener``` from the ```NetworkImageView```.

Finally, ```startPostponedEnterTransition``` can be called on the ```UserInfoFragment```'s parent ```Activity```.

The second problem, which was assumed to have been solved when setting the drawable above, is to provide the ```ImageView``` in the new ```Activity``` with the avatar to be drawn.
This is done in the ```UI``` utility method ```setDrawableForIntent```

**UI.java**
``` java
static void setDrawableForIntent(@NonNull ImageView iv, @NonNull Intent i) {
        if(iv.getDrawable() instanceof BitmapDrawable) {
            i.putExtra(iv.getResources().getString(R.string.intent_drawable),
                    ((BitmapDrawable) iv.getDrawable()).getBitmap()
            );
        }
    }
```

The method checks that the ```ImageView```'s ```Drawable``` is an instance of ```BitmapDrawable```, which it will be if loaded from a ```Bitmap``` as ```NetworkImageView``` does.
The ```instanceof``` comparator also performs a null check as the language specification defines the result of the operator to be "true if the value of the RelationalExpression is not null and the reference could be cast (§15.16) to the ReferenceType without raising a ClassCastException".

If the ```BitmapDrawable``` is valid, it is added to the ```Intent```.

#### Use of ViewSafeFragment

When a ```UserFragment``` is created, there are two possible orders of events.

The first is that ```onCreateView``` is called, and at some point in the future the network call to load the ```User``` returns, ```userLoaded``` is called and the ```Views``` are bound.
The second is that the ```userLoaded``` is called before ```onCreateView```, which would result in a ```NullPointerException``` if an attempt was made to  bind data to null ```Views```.

This is solved with two checks.

In ```userLoaded``` the first line is ```mUser = user```, assigning the member variable ```mUser```.
The second line is ```if(!mAreViewsValid) return``` which will ensure that no ```Views``` are bound to if they have not yet been created. This deals with the crash which could happen in
the second case, however we must now ensure that the ```Views``` are bound once they have been created.

This is achieved with the second check, the last line in ```onCreateView``` before returning the ```View```.
This check is ```if(mUser != null) userLoaded(mUser)```. As mAreViewsValid has been set to true, this will bind the ```Views``` if the ```User``` was initially loaded before the ```Views``` were created.
This structure is used throughout the different concrete implementations of ```UserFragment```.

#### ContributionsView

Objective 2.a.iv is to display the user's contributions in a graphical form.

As explained in the limitations section, there is no API for loading a user's contributions. Instead this can be achieved by parsing the SVG image of a user's contributions

![Contributions](http://imgur.com/KcQx5U4.png)

This image can be loaded from https://github.com/users/user_name/contributions

##### Loading contributions

Each column in the image is made up of a set of rectangles, with three important attributes.

```XML
<g transform="translate(0, 0)">
          <rect class="day" width="10" height="10" x="13" y="0" fill="#ebedf0" data-count="0" data-date="2016-04-10"></rect>
          <rect class="day" width="10" height="10" x="13" y="12" fill="#ebedf0" data-count="0" data-date="2016-04-11"></rect>
          <rect class="day" width="10" height="10" x="13" y="24" fill="#ebedf0" data-count="0" data-date="2016-04-12"></rect>
          <rect class="day" width="10" height="10" x="13" y="36" fill="#ebedf0" data-count="0" data-date="2016-04-13"></rect>
          <rect class="day" width="10" height="10" x="13" y="48" fill="#ebedf0" data-count="0" data-date="2016-04-14"></rect>
          <rect class="day" width="10" height="10" x="13" y="60" fill="#ebedf0" data-count="0" data-date="2016-04-15"></rect>
          <rect class="day" width="10" height="10" x="13" y="72" fill="#ebedf0" data-count="0" data-date="2016-04-16"></rect>
      </g>
```

Firstly, the ```fill``` attribute provides the hexadecimal colour code used to draw the rectangle, which can be parsed and used to draw the image later.
Secondly, the ```data-count``` attribute provides the number of contributions made for the particular day.
Finally, the ```data-date``` attribute is the date represented by the rectangle in the format yyyy-mm-dd.

In order to load the SVG, a request can be made to download it as a string.

The ```ContributionsLoader``` class is used to load a user's contributions.

**ContributionsLoader.java**
``` java
/*
 * Copyright  2016 Theo Pearson-Bray
 *
 *    Licensed under the Apache License, Version 2.0 (the "License");
 *    you may not use this file except in compliance with the License.
 *    You may obtain a copy of the License at
 *
 *        http://www.apache.org/licenses/LICENSE-2.0
 *
 *    Unless required by applicable law or agreed to in writing, software
 *    distributed under the License is distributed on an "AS IS" BASIS,
 *    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 *    See the License for the specific language governing permissions and
 *    limitations under the License.
 *
 */

package com.tpb.contributionsview;

import android.content.Context;
import android.graphics.Color;
import android.os.Parcel;
import android.os.Parcelable;
import android.support.annotation.ColorInt;
import android.support.annotation.NonNull;

import com.android.volley.Request;
import com.android.volley.Response;
import com.android.volley.VolleyError;
import com.android.volley.toolbox.StringRequest;
import com.android.volley.toolbox.Volley;

import java.lang.ref.WeakReference;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.ArrayList;

/**
 * Created by theo on 12/01/17.
 */

public class ContributionsLoader {

    // Format string for svg path
    private static final String IMAGE_BASE = "https://github.com/users/%s/contributions";

    private final WeakReference<ContributionsRequestListener> mListener;

    ContributionsLoader(@NonNull ContributionsRequestListener listener) {
        mListener = new WeakReference<>(listener);
    }

    void beginRequest(@NonNull Context context, @NonNull String login) {
        final String URL = String.format(IMAGE_BASE, login);
        // Load the svg as a string
        final StringRequest req = new StringRequest(Request.Method.GET, URL,
                new Response.Listener<String>() {
                    @Override
                    public void onResponse(String response) {
                        parse(response);
                    }
                }, new Response.ErrorListener() {
                @Override
                public void onErrorResponse(VolleyError error) {
                    if(mListener.get() != null) mListener.get().onError(error);
                }
            }
        );
        Volley.newRequestQueue(context).add(req);
    }

    private void parse(String response) {
        final ArrayList<ContributionsDay> contributions = new ArrayList<>();
        int first = response.indexOf("<rect");
        int last;
        // Find each rectangle in the image
        while(first != -1) {
            last = response.indexOf("/>", first);
            contributions.add(new ContributionsDay(response.substring(first, last)));
            first = response.indexOf("<rect", last);
        }
        if(mListener.get() != null) mListener.get().onResponse(contributions);

    }

    public static class ContributionsDay implements Parcelable {
        private static final SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        @ColorInt public final int color;
        public long date;
        public final int contributions;

        ContributionsDay(String rect) {
            //rect is <rect class="day" width="10" height="10" x="" y="" fill="#FFFFF" data-count="n" data-date="yyyy-mm-dd"/>
            final int colorIndex = rect.indexOf("fill=\"") + 6;
            color = Color.parseColor(rect.substring(colorIndex, colorIndex + 7));

            final int countIndex = rect.indexOf("data-count=\"") + 12;
            final int countEndIndex = rect.indexOf("\"", countIndex);
            contributions = Integer.parseInt(rect.substring(countIndex, countEndIndex));

            final int dateIndex = rect.indexOf("data-date=\"") + 11;
            try {
                date = sdf.parse(rect.substring(dateIndex, dateIndex + 11)).getTime();
            } catch(ParseException ignored) {}
        }


        @Override
        public String toString() {
            return "ContributionsDay{" +
                    "color=" + color +
                    ", date=" + date +
                    ", contributions=" + contributions +
                    '}';
        }

        @Override
        public int describeContents() {
            return 0;
        }

        @Override
        public void writeToParcel(Parcel dest, int flags) {
            dest.writeInt(this.color);
            dest.writeLong(this.date);
            dest.writeInt(this.contributions);
        }

        protected ContributionsDay(Parcel in) {
            this.color = in.readInt();
            this.date = in.readLong();
            this.contributions = in.readInt();
        }

        public static final Creator<ContributionsDay> CREATOR = new Creator<ContributionsDay>() {
            @Override
            public ContributionsDay createFromParcel(Parcel source) {
                return new ContributionsDay(source);
            }

            @Override
            public ContributionsDay[] newArray(int size) {
                return new ContributionsDay[size];
            }
        };
    }

    interface ContributionsRequestListener {

        void onResponse(ArrayList<ContributionsDay> contributions);

        void onError(VolleyError error);

    }


}

```

The interface ```ContributionsRequestListener``` is used as a callback once the contributions information is loaded.
As such, the ```ContributionsLoader``` constructor takes a ```ContributionsLoader``` and stores a ```WeakReference``` to it.

The ```beginRequest``` method formats the user login and performs a get request to the URL of the SVG image.
If the image is loaded successfully, the ```parse``` method is called.

```parse``` uses two counters, ```first``` and ```last``` to iterate through each substring which is surrounded by the tags ```<rect``` and the closing tag ```/>```.
The ```String``` ```indexOf``` method searches a string for a substring from a given position, 0 in the simpler overloaded method, returning -1 if the substring is not present.

Each time the substring is found, the ```parse``` method adds a new ```ContributionsDay``` object to the contributions ```ArrayList```.

```ContributionsDay``` contains the integer value of the ```fill``` attribute, the long value of the ```data-date``` attribute, and the integer value of the ```data-count``` attribute.

Finally, if the ```ContributionsRequestListener``` is non null, the values are returned to it.

##### Displaying contributions

The ```ContributionsView``` is a descendant of ```View``` used to draw the contributions heatmap.

**ContributionsView.java**
``` java
/*
 * Copyright  2016 Theo Pearson-Bray
 *
 *    Licensed under the Apache License, Version 2.0 (the "License");
 *    you may not use this file except in compliance with the License.
 *    You may obtain a copy of the License at
 *
 *        http://www.apache.org/licenses/LICENSE-2.0
 *
 *    Unless required by applicable law or agreed to in writing, software
 *    distributed under the License is distributed on an "AS IS" BASIS,
 *    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 *    See the License for the specific language governing permissions and
 *    limitations under the License.
 *
 */

package com.tpb.contributionsview;

import android.content.Context;
import android.content.res.TypedArray;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.graphics.Rect;
import android.os.Parcel;
import android.os.Parcelable;
import android.util.AttributeSet;
import android.view.View;
import android.view.ViewGroup;

import com.android.volley.VolleyError;

import java.lang.ref.WeakReference;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Calendar;
import java.util.List;

/**
 * Created by theo on 12/01/17.
 */

public class ContributionsView extends View implements ContributionsLoader.ContributionsRequestListener {

    private static final Calendar mCalendar = Calendar.getInstance();

    private boolean mShouldDisplayMonths;
    private int mTextColor;
    private int mTextSize;
    private int mBackGroundColor;
    private ArrayList<ContributionsLoader.ContributionsDay> mContributions = new ArrayList<>();

    private Paint mDayPainter;
    private Paint mTextPainter;

    private Rect mRect;
    private final Rect mTextBounds = new Rect();

    private WeakReference<ContributionsLoadListener> mListener;

    public ContributionsView(Context context) {
        super(context);
        initView(context, null, 0, 0);
    }

    public ContributionsView(Context context, AttributeSet attrs) {
        super(context, attrs);
        initView(context, attrs, 0, 0);
    }

    public ContributionsView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        initView(context, attrs, defStyleAttr, 0);
    }

    private void initView(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        mRect = new Rect();

        final TypedArray attributes = context.getTheme().obtainStyledAttributes(attrs,
                R.styleable.ContributionsView, defStyleAttr, defStyleRes
        );
        useAttributes(attributes);

        mTextPainter = new Paint(Paint.ANTI_ALIAS_FLAG);
        mDayPainter = new Paint(Paint.ANTI_ALIAS_FLAG);
        mDayPainter.setStyle(Paint.Style.FILL);
    }

    private void useAttributes(TypedArray ta) {
        mShouldDisplayMonths = ta.getBoolean(R.styleable.ContributionsView_showMonths, true);
        mBackGroundColor = ta.getColor(R.styleable.ContributionsView_backgroundColor,
                0xD6E685
        ); //GitHub default color
        mTextColor = ta.getColor(R.styleable.ContributionsView_textColor, Color.BLACK);
        mTextSize = ta.getDimensionPixelSize(R.styleable.ContributionsView_textSize, 7);
        if(ta.getString(R.styleable.ContributionsView_username) != null && !isInEditMode()) {
            loadContributions(ta.getString(R.styleable.ContributionsView_username));
        }
    }

    public void loadContributions(String user) {
        new ContributionsLoader(this).beginRequest(getContext(), user);
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        canvas.getClipBounds(mRect);

        final int w = mRect.width();
        final int h = mRect.height();

        final int hnum = mContributions.size() == 0 ? 52 : (int) Math
                .ceil(mContributions.size() / 7d); //The number of columns to show horizontally

        final float bd = (w / (float) hnum) * 0.9f; //The dimension of a single block
        final float m = (w / (float) hnum) - bd; //The margin around a block

        final float mth = mShouldDisplayMonths ? mTextSize : 0; //Height of month text

        //Draw the background
        mDayPainter.setColor(mBackGroundColor);
        canvas.drawRect(0, (2 * mth), w, h + mth, mDayPainter);
        float x = 0;
        if(mContributions.size() > 0) {
            int dow = getDayOfWeek(mContributions.get(0).date) - 1;
            float y = (dow * (bd + m)) + 2 * mth;
            for(ContributionsLoader.ContributionsDay d : mContributions) {
                mDayPainter.setColor(d.color);
                canvas.drawRect(x, y, x + bd, y + bd, mDayPainter);
                dow = getDayOfWeek(d.date) - 1;
                if(dow == 6) { //We just drew the last day of the week
                    x += bd + m;
                    y = 2 * mth;
                } else {
                    y += bd + m;
                }

            }
        } else {
            int dow = 0;
            float y = 2 * mth;
            mDayPainter.setColor(0xffeeeeee);
            for(int i = 0; i < 364; i++) {
                canvas.drawRect(x, y, x + bd, y + bd, mDayPainter);
                if(dow == 6) { //We just drew the last day of the week
                    x += bd + m;
                    y = 2 * mth;
                    dow = 0;
                } else {
                    y += bd + m;
                    dow++;
                }
            }
        }
        if(mShouldDisplayMonths) {
            mTextPainter.setColor(mTextColor);
            mTextPainter.setTextSize(mth);
            mCalendar.setTimeInMillis(System.currentTimeMillis());
            mCalendar.add(Calendar.MONTH, -12);
            x = 0;
            for(int i = 0; i < 12; i++) {
                final String month = getMonthName(mCalendar.getTimeInMillis());
                mTextPainter.getTextBounds(month, 0, month.length(), mTextBounds);
                if(w > x + mTextBounds.width()) {
                    canvas.drawText(
                            month,
                            x,
                            mth,
                            mTextPainter
                    );
                } else {
                    break;
                }
                mCalendar.add(Calendar.MONTH, 1);
                x += w / 12;
            }
        }
        final ViewGroup.LayoutParams lp = getLayoutParams();
        lp.height = h;
        setLayoutParams(lp);
    }

    private int getDayOfWeek(long stamp) {
        mCalendar.setTimeInMillis(stamp);
        //Day of week is indexed 1 to 7
        return mCalendar.get(Calendar.DAY_OF_WEEK);
    }

    private static final SimpleDateFormat month = new SimpleDateFormat("MMM");

    private String getMonthName(long stamp) {
        return month.format(stamp);
    }

    @Override
    public void onResponse(ArrayList<ContributionsLoader.ContributionsDay> contributions) {
        mContributions = contributions;
        invalidate();
        if(mListener != null && mListener.get() != null) {
            mListener.get().contributionsLoaded(contributions);
        }
    }

    @Override
    public void onError(VolleyError error) {

    }

    public void setListener(ContributionsLoadListener listener) {
        mListener = new WeakReference<>(listener);
    }


    public interface ContributionsLoadListener {

        void contributionsLoaded(List<ContributionsLoader.ContributionsDay> contributions);
    }

    @Override
    protected Parcelable onSaveInstanceState() {
        final Parcelable ss = super.onSaveInstanceState();
        final ContributionsState state = new ContributionsState(ss);
        state.contributions = mContributions.toArray(new ContributionsLoader.ContributionsDay[0]);
        return state;
    }

    @Override
    protected void onRestoreInstanceState(Parcelable state) {
        super.onRestoreInstanceState(state);

        if(!(state instanceof ContributionsState)) {
            super.onRestoreInstanceState(state);
            return;
        }
        final ContributionsState cs = (ContributionsState) state;
        super.onRestoreInstanceState(cs.getSuperState());
        this.mContributions = new ArrayList<>(Arrays.asList(cs.contributions));
        invalidate();
    }

    private static class ContributionsState extends BaseSavedState {
        ContributionsLoader.ContributionsDay[] contributions;

        ContributionsState(Parcel source) {
            super(source);
            this.contributions = source.createTypedArray(ContributionsLoader.ContributionsDay.CREATOR);
        }

        ContributionsState(Parcelable superState) {
            super(superState);
        }

        @Override
        public void writeToParcel(Parcel out, int flags) {
            super.writeToParcel(out, flags);
            out.writeTypedArray(contributions, flags);
        }

        public static final Parcelable.Creator<ContributionsState> CREATOR =
                new Parcelable.Creator<ContributionsState>() {
                    public ContributionsState createFromParcel(Parcel in) {
                        return new ContributionsState(in);
                    }

                    public ContributionsState[] newArray(int size) {
                        return new ContributionsState[size];
                    }
                };
    }

}



```

The ```ContributionsView``` has attributes for its background colour, text colour, text size, and whether months should be displayed. A default user login can also be set via XML.

As the ```View``` is being repeatedly drawn, it is important to ensure that as few objects as possible are allocated in the ```onDraw``` method.
The single ```Paint``` objects for text and day (rectangle) painting are therefore re-used, as are the ```Rectangle``` used to store the ```View``` dimensions and the second ```Rectangle``` used to store text bounds.
The ```Calendar``` used for parsing dates is static and used in all instances of ```ContributionsView```.

The ```ContributionsView``` implements ```ContributionsRequestListener``` and contains the ```loadContributions``` method, which creates an anonymous ```ContributionsLoader``` and
begins loading the contributions for the given user.
In the ```onResponse``` method the ```ContributionsView``` stores the ```ArrayList``` of contributions and calls the ```invalidate``` method, forcing a redraw.
It also checks if its listener has been set, and notifies it of the newly loaded ```ArrayList``` of ```ContributionsDay``` objects.

The ```onDraw``` method needs to deal with two states, either the ```ContributionsView``` has a non empty list of ```ContributionsDays``` or it does not.

**ContributionsView.java**
``` java
void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        canvas.getClipBounds(mRect);

        final int w = mRect.width();
        final int h = mRect.height();

        final int hnum = mContributions.size() == 0 ? 52 : (int) Math
                .ceil(mContributions.size() / 7d); //The number of columns to show horizontally

        final float bd = (w / (float) hnum) * 0.9f; //The dimension of a single block
        final float m = (w / (float) hnum) - bd; //The margin around a block

        final float mth = mShouldDisplayMonths ? mTextSize : 0; //Height of month text

        //Draw the background
        mDayPainter.setColor(mBackGroundColor);
        canvas.drawRect(0, (2 * mth), w, h + mth, mDayPainter);
        float x = 0;
        if(mContributions.size() > 0) {
            int dow = getDayOfWeek(mContributions.get(0).date) - 1;
            float y = (dow * (bd + m)) + 2 * mth;
            for(ContributionsLoader.ContributionsDay d : mContributions) {
                mDayPainter.setColor(d.color);
                canvas.drawRect(x, y, x + bd, y + bd, mDayPainter);
                dow = getDayOfWeek(d.date) - 1;
                if(dow == 6) { //We just drew the last day of the week
                    x += bd + m;
                    y = 2 * mth;
                } else {
                    y += bd + m;
                }

            }
        } else {
            int dow = 0;
            float y = 2 * mth;
            mDayPainter.setColor(0xffeeeeee);
            for(int i = 0; i < 364; i++) {
                canvas.drawRect(x, y, x + bd, y + bd, mDayPainter);
                if(dow == 6) { //We just drew the last day of the week
                    x += bd + m;
                    y = 2 * mth;
                    dow = 0;
                } else {
                    y += bd + m;
                    dow++;
                }
            }
        }
        if(mShouldDisplayMonths) {
            mTextPainter.setColor(mTextColor);
            mTextPainter.setTextSize(mth);
            mCalendar.setTimeInMillis(System.currentTimeMillis());
            mCalendar.add(Calendar.MONTH, -12);
            x = 0;
            for(int i = 0; i < 12; i++) {
                final String month = getMonthName(mCalendar.getTimeInMillis());
                mTextPainter.getTextBounds(month, 0, month.length(), mTextBounds);
                if(w > x + mTextBounds.width()) {
                    canvas.drawText(
                            month,
                            x,
                            mth,
                            mTextPainter
                    );
                } else {
                    break;
                }
                mCalendar.add(Calendar.MONTH, 1);
                x += w / 12;
            }
        }
        final ViewGroup.LayoutParams lp = getLayoutParams();
        lp.height = h;
        setLayoutParams(lp);
    }
```

The ```onDraw``` method begins by measuring the size which it has to draw in.
Next, the number of columns to show horizontally can be calculated, as either the number of contributions split across 7 day weeks, or 52 weeks.

Each rectangle has an area of the view width over the horizontal number of contributions, however it must also account for the margin between each rectangle.
For each rectangular segment of the canvas, 90% of the width will be filled with the block, and the remaining 10% left as a margin.

Next the month text height is set as either the text size, or 0 depenedent on whether month text should be drawn.

The first draw call is to draw the background behind the image. This call draws the background colour behind everything but the space for the month text.

The next draw call is dependent on whether there are contributions to be drawn.
The contributions are drawn based on the day of the week, which is calculated from the value stored in each ```ContributionsDay``` and the ```Calendar```.
The y position is the day of the week multiplied by the total height of a rectangle plus the margin for drawing the month text.

After computing the initial y position, the ```onDraw``` method uses a for-each loop to iterate through each ```ContributionsDay```, setting the colour of mDayPainter, drawing the rectangle, and calculating the day of the week.
If the day of the week is 6, the week has ended; the x position is incremented by the width of rectangle and the y value is reset to the month text margin.
Otherwise, the y value is incremented by the height of a rectangle.

If there are no contributions to draw mDayPainter is set to the default background colour, and 364 rectangles are drawn to fill the 52 weeks with full 7 day weeks.

The final set of draw calls occur if mShouldDisplayMonths is true.
In this case mTextPainter is setup with mTextColor and the month text height, and the calendar is initialised to the current time, before subtracting 12 months to return to the start
of the year.
For each month in the year, the three letter month string is collected from the ```Calendar``` instance and the mTextBounds rectangle is used to store the measured bounds of the text,
ensuring that it doesn't extend past the end of the ```Canvas```.
After each draw call the ```Calendar``` month is incremented, and the x position is incremented by one twelfth of the ```Canvas``` width.

The final call in ```onDraw``` is to set the ```LayoutParams``` of the ```ContributionsView``` to ensure that the ```Canvas``` is drawn across the full width available.

##### Contributions statistics

Returning to the ```UserInfoFragment``` we have the callback for ```contributionsLoaded```.
This method computes numerous statistics about the user:
- Their total number of contributions
- Their average number of contributions per day
- Their average number of contributions per active day
- Their greatest number of contributions per day
- Their longest uninterrupted 'streak' of active days

**UserInfoFragment.java**
``` java
void contributionsLoaded(List<ContributionsLoader.ContributionsDay> contributions) {
        if(!areViewsValid()) return;
        int totalContributions = 0;
        int daysActive = 0;
        int maxContributions = 0;
        int streak = 0;
        int maxStreak = 0;
        for(ContributionsLoader.ContributionsDay gd : contributions) {
            if(gd.contributions > 0) {
                totalContributions += gd.contributions;
                daysActive += 1;
                if(gd.contributions > maxContributions) {
                    maxContributions = gd.contributions;
                }
                streak += 1;
                if(streak > maxStreak) {
                    maxStreak = streak;
                }
            } else {
                streak = 0;
            }
        }
        if(totalContributions > 0) {
            final String info = getResources()
                    .getQuantityString(R.plurals.text_user_contributions, totalContributions, totalContributions) +
                    "\n" +
                    String.format(getString(R.string.text_user_average),
                            (float) totalContributions / contributions.size()
                    ) +
                    "\n" +
                    String.format(getString(R.string.text_user_average_active),
                            ((float) totalContributions / daysActive)
                    ) +
                    "\n" +
                    String.format(getString(R.string.text_user_max_contributions), maxContributions) +
                    "\n" +
                    String.format(getString(R.string.text_user_streak), maxStreak);
            final boolean isEmpty = mContributionsInfo.getText().toString().isEmpty();
            mContributionsInfo.setText(info);
            if(isEmpty) {
                ObjectAnimator.ofInt(
                        mContributionsInfo,
                        "maxLines",
                        0,
                        mContributionsInfo.getLineCount()
                ).setDuration(200).start();
            }
        } else {
            mContributionsInfo.setText(getString(R.string.text_user_no_contributions));
        }
    }
```

5 counters are used for the computation.
Each iteration of the loop checks if any contributions have been made.
If any contributions have been made, the total number of contributions is incremented by the value, the number of active days is incremented, the number of contributions on that day
is checked against the current maximum number of contributions, and streak counter is incremented before being checked against the current maxStreak value.
If no contributions have been made, the streak counter is reset to 0.

Once the computations have been made, the total number of contributions is made.
If it is 0, the "No contributions" string resource is displayed.
Otherwise, a string is built from 5 different format strings to display the information.

Before setting the text of mContributionsInfo, a check is performed to see if it already contains any text.
If the ```TextView``` is empty, then it will have 0 height (other than its margin), and setting its text would cause both it and its parent ```CardView``` to jump in size.
Rather than allowing this, an ```ObjectAnimator``` is used to increment the maxLines count of the ```TextView``` from 0 to the required number over a period of 200 milliseconds.

This completes objectives 2.a.iv and 2.a.v

#### Displaying user information

The level of information which a user provides is not constant. While some provide information about their location, company, email and bio, others provide no information.

The ```displayUser``` method in ```Formatter``` is used in both the ```LoginActivity``` and ```UserInfoLayout``` to bind data to the ```Views``` in an inflated ```shared_user_info```
layout.

**Formatter.java**
``` java
void displayUser(ViewGroup userInfoParent, User user) {
        userInfoParent.setVisibility(View.VISIBLE);

        final NetworkImageView avatar = ButterKnife.findById(userInfoParent, R.id.user_avatar);
        avatar.setImageUrl(user.getAvatarUrl());
        final TextView login = ButterKnife.findById(userInfoParent, R.id.user_login);
        login.setText(user.getLogin());

        final Context context = userInfoParent.getContext();
        final LinearLayout infoList = ButterKnife.findById(userInfoParent, R.id.user_info_layout);
        infoList.removeAllViews();
        TextView tv;
        final LinearLayout.LayoutParams params = new LinearLayout.LayoutParams(
                ViewGroup.LayoutParams.MATCH_PARENT,
                ViewGroup.LayoutParams.WRAP_CONTENT
        );
        params.setMargins(0, UI.pxFromDp(4), 0, UI.pxFromDp(4));
        if(user.getName() != null) {
            tv = getInfoTextView(context, R.drawable.ic_person);
            tv.setText(user.getName());
            infoList.addView(tv, params);
        }
        tv = getInfoTextView(context, R.drawable.ic_date);
        tv.setText(
                String.format(
                        context.getString(R.string.text_user_created_at),
                        Util.formatDateLocally(
                                context,
                                new Date(user.getCreatedAt())
                        )
                )
        );
        infoList.addView(tv, params);
        if(user.getEmail() != null) {
            tv = getInfoTextView(context, R.drawable.ic_email);
            tv.setAutoLinkMask(Linkify.EMAIL_ADDRESSES);
            tv.setText(user.getEmail());
            infoList.addView(tv, params);
        }
        if(user.getBlog() != null) {
            tv = getInfoTextView(context, R.drawable.ic_blog);
            tv.setAutoLinkMask(Linkify.WEB_URLS);
            tv.setText(user.getBlog());
            infoList.addView(tv, params);
        }
        if(user.getCompany() != null) {
            tv = getInfoTextView(context, R.drawable.ic_company);
            tv.setText(user.getCompany());
            infoList.addView(tv, params);
        }
        if(user.getLocation() != null) {
            tv = getInfoTextView(context, R.drawable.ic_location);
            tv.setText(user.getLocation());
            infoList.addView(tv, params);
        }
        if(user.getRepos() > 0) {
            tv = getInfoTextView(context, R.drawable.ic_repo);
            tv.setText(context.getResources().getQuantityString(
                    R.plurals.text_user_repositories,
                    user.getRepos(),
                    user.getRepos()
            ));
            infoList.addView(tv, params);
        }
        if(user.getGists() > 0) {
            tv = getInfoTextView(context, R.drawable.ic_gist);
            tv.setText(context.getResources().getQuantityString(
                    R.plurals.text_user_gists,
                    user.getGists(),
                    user.getGists()
            ));
            infoList.addView(tv, params);
        }
        if(user.getBio() != null) {
            tv = getInfoTextView(context, R.drawable.ic_bio);
            tv.setText(user.getBio());
            infoList.addView(tv, params);
        }
        UI.expand(infoList);
    }
```

The method first finds the ```NetworkImageView``` to display the user's avatar, and the ```TextView``` to display their username.
Once the username and avatar URL have been bound, a ```LayoutParams``` instance is created to ensure that each ```TextView``` uses the same margins.

The ```getInfoTextView``` method takes the ```Context``` required to instantiate a ```View``` and a drawable resource id to display at the start of the ```TextView```.

**Formatter.java**
``` java
static TextView getInfoTextView(Context context, @DrawableRes int drawableRes) {
        final TextView tv = new TextView(context);
        tv.setCompoundDrawablePadding(UI.pxFromDp(4));
        tv.setCompoundDrawablesRelativeWithIntrinsicBounds(drawableRes, 0, 0, 0);
        return tv;
    }
```

```displayUser``` checks each value string value to be displayed, and if it is non null, the string is added to its own row with a corresponding icon.
Numeric values greater than 0 are also displayed, however they require extra formatting.
Correct grammar is achieved using plural strings, string resources with multiple values for different quantities.

Once each ```TextView``` row has been added to the ```LinearLayout```, the ```LinearLayout``` is expanded with the ```UI``` ```expand``` method.

This completes objectives 2.a.i, 2.a.ii, and 2.a.iii

#### Following and unfollowing users

Objective 2.g requires the implementation of following and unfollowing users.
This can be implemented as a ```Button``` below their information, displaying either "follow" if the user is not currently followed, or "unfollow" if they are currently followed.

The ```Loader``` method to check whether a user is followed requires an ```ItemLoader<Boolean>``` and the user's login, while the ```Editor``` methods to follow or unfollow a user 
require an ```UpdateListener<Boolean>```, as well as the user's login.

The ```Button``` should only be shown if the user being displayed is not the authenticated user.

This check if performed in the ```userLoaded``` method:

```java
if(!GitHubSession.getSession(getContext()).getUserLogin().equals(user.getLogin())) {
    new Loader(getContext()).checkIfFollowing(this, user.getLogin());
}
```

The ```updated``` method is used for binding the following information, and ```loadComplete``` pases its return value through to ```updated```.

**UserInfoFragment.java**
``` java
void updated(Boolean isFollowing) {
        if(mFollowButton == null) {
            mFollowButton = new Button(getContext());
            mFollowButton.setBackground(null);
            mFollowButton.setLayoutParams(
                    new LinearLayout.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT,
                            ViewGroup.LayoutParams.WRAP_CONTENT
                    ));
            mUserInfoParent.addView(mFollowButton);
        }

        if(isFollowing) {
            mFollowButton.setText(R.string.text_unfollow_user);
        } else {
            mFollowButton.setText(R.string.text_follow_user);
        }
        mFollowButton.setOnClickListener(v -> {
            mFollowButton.setEnabled(false);
            mRefresher.setRefreshing(true);
            if(isFollowing) {
                Editor.getEditor(getContext()).unfollowUser(UserInfoFragment.this, mUser.getLogin());
            } else {
                Editor.getEditor(getContext()).followUser(UserInfoFragment.this, mUser.getLogin());
            }
        });
        mFollowButton.setEnabled(true);
        mRefresher.setRefreshing(false);
    }
```

```updated``` first checks if the ```Button``` has ben created, creating it if not.
It then sets the appropriate resource string for the button and adds an ```onClickListener```.
The ```onClickListener``` disables the button, to prevent multiple requests, starts the```SwipeRefreshLayout``` to indicate loading, and then calls the ```Editor``` to follow or
unfollow the user.

Finally, the ```Button``` is enabled, and the ```SwipeRefreshLayout``` is stopped.

The ```UserInfoFragment``` completes all objectives in 2.a

### UserReposFragment

The ```UserReposFragment``` is used to display the repositories which a user has contributed to.

As multiple different ```Fragments``` contain only a ```SwipeRefreshLayout``` and a ```RecyclerView``` there is no need to create an individual layout file for each of them.
Instead they can each use the same layout file:

**fragment_recycler.xml**
``` xml
<?xml version="1.0" encoding="utf-8"?>

<android.support.v4.widget.SwipeRefreshLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/fragment_refresher"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:layout_behavior="@string/appbar_scrolling_view_behavior">

    <com.tpb.animatingrecyclerview.AnimatingRecyclerView
        android:id="@+id/fragment_recycler"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_margin="8dp"/>

</android.support.v4.widget.SwipeRefreshLayout>



```

The ```UserReposFragment``` contains very little logic, as it only has to load the ```User``` and then pass this ```User``` to the ```RecyclerView``` adapter which contains logic for
loading the user's repositories and binding them to a list of ```Views```.

**UserReposFragment.java**
``` java
package com.tpb.projects.user.fragments;

import android.content.Intent;
import android.os.Bundle;
import android.support.annotation.Nullable;
import android.support.v4.widget.SwipeRefreshLayout;
import android.support.v7.widget.LinearLayoutManager;
import android.support.v7.widget.RecyclerView;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;

import com.tpb.animatingrecyclerview.AnimatingRecyclerView;
import com.tpb.github.data.Loader;
import com.tpb.github.data.models.Repository;
import com.tpb.github.data.models.State;
import com.tpb.github.data.models.User;
import com.tpb.projects.R;
import com.tpb.projects.common.FixedLinearLayoutManger;
import com.tpb.projects.repo.RepoActivity;
import com.tpb.projects.common.RepositoriesAdapter;

import butterknife.BindView;
import butterknife.ButterKnife;
import butterknife.Unbinder;

/**
 * Created by theo on 10/03/17.
 */

public class UserReposFragment extends UserFragment implements RepositoriesAdapter.RepoOpener {

    private Unbinder unbinder;

    @BindView(R.id.fragment_recycler) AnimatingRecyclerView mRecycler;
    @BindView(R.id.fragment_refresher) SwipeRefreshLayout mRefresher;
    private RepositoriesAdapter mAdapter;

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        final View view = inflater.inflate(R.layout.fragment_recycler, container, false);
        unbinder = ButterKnife.bind(this, view);

        final LinearLayoutManager manager = new FixedLinearLayoutManger(getContext());
        mRecycler.setLayoutManager(manager);
        mRecycler.enableLineDecoration();
        mAdapter = new RepositoriesAdapter(getActivity(), this, mRefresher);
        mRecycler.setAdapter(mAdapter);

        mRecycler.setOnScrollListener(new RecyclerView.OnScrollListener() {
            @Override
            public void onScrolled(RecyclerView recyclerView, int dx, int dy) {
                super.onScrolled(recyclerView, dx, dy);
                if(manager.findFirstVisibleItemPosition() + 20 > manager.getItemCount()) {
                    mAdapter.notifyBottomReached();
                }
            }
        });
        mAreViewsValid = true;
        if(mUser != null) userLoaded(mUser);
        return view;
    }

    @Override
    public void userLoaded(User user) {
        mUser = user;
        if(!areViewsValid()) return;
        mAdapter.setUser(user.getLogin(), false);
    }

    @Override
    public void openRepo(Repository repo) {
        final Intent i = new Intent(getContext(), RepoActivity.class);
        i.putExtra(getString(R.string.intent_repo), repo);
        Loader.getLoader(getContext())
              .loadProjects(null, repo.getFullName())
              .loadIssues(null, repo.getFullName(), State.OPEN, null, null, 0)
              .loadProjects(null, repo.getFullName());
        startActivity(i);
        getActivity().overridePendingTransition(R.anim.slide_up, R.anim.none);
    }

    @Override
    public void onDestroyView() {
        super.onDestroyView();
        unbinder.unbind();
    }
}

```

Following the same ```View``` binding structure as other ```Fragments``` the ```UserRepoFragment``` inflates its layout in ```onCreateView```.

One function of the ```UserReposFragment``` is to listen for the ```RecyclerView``` scrolling.
The GitHub API is paginated, returning a maximum of 30 items per request. In order to load further items a page number must be specified in the request.
The ```RepositoriesAdapter``` tracks its page number when loading ```Repositories``` and is notified that its ```RecyclerView``` is approaching the bottom of its content by the 
```UserReposFragment```  ```OnScrollListener```.

The second function of the ```UserReposFragment``` is to launch the ```RepoActivity``` for displaying a repository when an item in the ```RecyclerView``` is clicked.

```openRepo``` creates the ```Intent``` to launch the ```RepoActivity```, begins preloading the data which wil be used in ```RepoActvitiy```, and then launches ```RepoActivity```.

### RepositoriesAdapter

The ```RepositoriesAdapter``` is used in two places, displaying the repositories that a user contributes to and displaying the repositories that a user has starred.
When displaying a user's repositories, the ```RepositoriesAdapter``` must also support pinning repositories (Objective 2.b.vi).

**RepositoriesAdapter.java**
``` java
package com.tpb.projects.common;

import android.annotation.SuppressLint;
import android.app.Activity;
import android.content.Context;
import android.content.SharedPreferences;
import android.support.v4.widget.SwipeRefreshLayout;
import android.support.v7.widget.RecyclerView;
import android.text.format.DateUtils;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

import com.tpb.github.data.APIHandler;
import com.tpb.github.data.Loader;
import com.tpb.github.data.auth.GitHubSession;
import com.tpb.github.data.models.Repository;
import com.tpb.mdtext.Markdown;
import com.tpb.mdtext.views.MarkdownTextView;
import com.tpb.projects.R;
import com.tpb.projects.flow.IntentHandler;
import com.tpb.projects.util.Util;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

import butterknife.BindView;
import butterknife.ButterKnife;

/**
 * Created by theo on 14/12/16.
 */

public class RepositoriesAdapter extends RecyclerView.Adapter<RepositoriesAdapter.RepoHolder> implements Loader.ListLoader<Repository> {
    private static final String TAG = RepositoriesAdapter.class.getSimpleName();

    private final Loader mLoader;
    private final SwipeRefreshLayout mRefresher;
    private final ArrayList<Repository> mRepos = new ArrayList<>();
    private final String mAuthenticatedUser;
    private String mUser;
    private final RepoPinChecker mPinChecker;
    private final RepoOpener mOpener;
    private final Activity mActivity;
    private int mPage = 1;
    private boolean mIsLoading = false;
    private boolean mMaxPageReached = false;

    private boolean mIsShowingStars = false;

    public RepositoriesAdapter(Activity activity, RepoOpener opener, SwipeRefreshLayout refresher) {
        mActivity = activity;
        mLoader = Loader.getLoader(activity);
        mOpener = opener;
        mRefresher = refresher;
        mRefresher.setRefreshing(true);
        mRefresher.setOnRefreshListener(() -> {
            mPage = 1;
            mMaxPageReached = false;
            final int oldSize = mRepos.size();
            mRepos.clear();
            notifyItemRangeRemoved(0, oldSize);
            loadReposForUser(true);
        });
        mPinChecker = new RepoPinChecker(activity);
        mAuthenticatedUser = GitHubSession.getSession(activity).getUserLogin();
    }

    public void setUser(String user, boolean isShowingStars) {
        mUser = user;
        mIsShowingStars = isShowingStars;
        mRepos.clear();
        loadReposForUser(true);
    }

    public void notifyBottomReached() {
        if(!mIsLoading && !mMaxPageReached) {
            mPage++;
            loadReposForUser(false);
        }
    }

    private void loadReposForUser(boolean resetPage) {
        mIsLoading = true;
        mRefresher.setRefreshing(true);
        if(resetPage) {
            mPage = 1;
            mMaxPageReached = false;
        }
        if(mIsShowingStars) {
            mLoader.loadStarredRepositories(this, mUser, mPage);
        } else if(mUser.equals(mAuthenticatedUser)) { //The session user
            mLoader.loadRepositories(this, mPage);
        } else {
            mLoader.loadRepositories(this, mUser, mPage);
        }
        mPinChecker.setKey(mUser);

    }

    @Override
    public RepoHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        return new RepoHolder(LayoutInflater.from(parent.getContext())
                                            .inflate(R.layout.viewholder_repo, parent, false));
    }

    @SuppressLint("SetTextI18n")
    @Override
    public void onBindViewHolder(RepoHolder holder, int position) {
        final int pos = holder.getAdapterPosition();
        final Repository r = mRepos.get(pos);
        holder.mName.setText(
                (r.getUserLogin().equals(mUser) ? r.getName() : r.getFullName()) + (r
                        .isFork() ? " (Forked) " : "")
        );
        if(Util.isNotNullOrEmpty(r.getLanguage())) {
            holder.mLanguage.setVisibility(View.VISIBLE);
            holder.mLanguage.setText(r.getLanguage());
        } else {
            holder.mLanguage.setVisibility(View.GONE);
        }
        if(Util.isNotNullOrEmpty(r.getDescription())) {
            holder.mDescription.setVisibility(View.VISIBLE);
            holder.mDescription.setMarkdown(Markdown.formatMD(r.getDescription(), r.getFullName()));
        } else {
            holder.mDescription.setVisibility(View.GONE);
        }
        if(mIsShowingStars) {
            holder.mImage.setImageUrl(r.getUserAvatarUrl());
            IntentHandler.addOnClickHandler(mActivity, holder.mImage, r.getUserLogin());
        } else {
            final boolean isPinned = mPinChecker.isPinned(r.getFullName());
            holder.isPinned = isPinned;
            holder.mImage
                    .setImageResource(isPinned ? R.drawable.ic_pinned : R.drawable.ic_not_pinned);
        }
        holder.mForks.setText(Integer.toString(r.getForks()));
        holder.mStars.setText(Integer.toString(r.getStarGazers()));
        holder.mLastUpdated.setText(DateUtils.getRelativeTimeSpanString(r.getUpdatedAt()));
    }

    private void togglePin(int pos) {
        final Repository r = mRepos.get(pos);
        if(mPinChecker.isPinned(r.getFullName())) {
            mRepos.remove(pos);
            final int newPos = mPinChecker.initialPosition(r.getFullName());
            mRepos.add(newPos, r);
            mPinChecker.unpin(r.getFullName());
            notifyItemMoved(pos, newPos);
        } else {
            mRepos.remove(pos);
            mRepos.add(0, r);
            mPinChecker.pin(r.getFullName());
            notifyItemMoved(pos, 0);

        }
    }

    @Override
    public int getItemCount() {
        return mRepos.size();
    }

    @Override
    public void listLoadComplete(List<Repository> repos) {
        mRefresher.setRefreshing(false);
        mIsLoading = false;
        if(repos.size() > 0) {
            int oldLength = mRepos.size();
            if(mIsShowingStars) {
                mRepos.addAll(repos);
            } else {
                insertPinnedRepos(repos);
            }
            notifyItemRangeInserted(oldLength, mRepos.size());

        } else {
            mMaxPageReached = true;
        }
    }

    private void insertPinnedRepos(List<Repository> repos) {
        if(mPage == 1) {
            for(Repository r : repos) {
                if(mPinChecker.isPinned(r.getFullName())) {
                    mRepos.add(0, r);
                } else {
                    mRepos.add(r);
                }
            }
            mPinChecker.setInitialPositions(mRepos);
            ensureLoadOfPinnedRepos();
        } else {
            for(Repository repo : repos) {
                if(!mRepos.contains(repo)) mRepos.add(repo);
            }
            mPinChecker.appendInitialPositions(repos);
        }
    }

    @Override
    public void listLoadError(APIHandler.APIError error) {
        mIsLoading = false;
        mRefresher.setRefreshing(false);
    }

    private void ensureLoadOfPinnedRepos() {
        for(String repo : mPinChecker.findNonLoadedPinnedRepositories()) {
            mLoader.loadRepository(new Loader.ItemLoader<Repository>() {
                @Override
                public void loadComplete(Repository data) {
                    if(!mRepos.contains(data)) {
                        mRepos.add(0, data);
                        mPinChecker.appendPinnedPosition(data.getFullName());
                        notifyItemInserted(0);
                    }
                }

                @Override
                public void loadError(APIHandler.APIError error) {

                }
            }, repo);
        }
    }

    private void openItem(int pos) {
        mOpener.openRepo(mRepos.get(pos));
    }

    class RepoHolder extends RecyclerView.ViewHolder {
        @BindView(R.id.repo_name) TextView mName;
        @BindView(R.id.repo_description) MarkdownTextView mDescription;
        @BindView(R.id.repo_forks) TextView mForks;
        @BindView(R.id.repo_stars) TextView mStars;
        @BindView(R.id.repo_language) TextView mLanguage;
        @BindView(R.id.repo_last_updated) TextView mLastUpdated;
        @BindView(R.id.repo_icon) NetworkImageView mImage;
        private boolean isPinned = false;

        RepoHolder(View view) {
            super(view);
            ButterKnife.bind(this, view);
            view.setOnClickListener(v -> openItem(getAdapterPosition()));
            mDescription.setOnClickListener(v -> openItem(getAdapterPosition()));
            if(!mIsShowingStars) {
                mImage.setOnClickListener((v) -> {
                    togglePin(getAdapterPosition());
                    isPinned = !isPinned;
                    mImage.setImageResource(
                            isPinned ? R.drawable.ic_pinned : R.drawable.ic_not_pinned);
                });
            }
        }

    }

    private class RepoPinChecker {

        private final SharedPreferences prefs;
        private static final String PREFS_KEY = "PINS";
        private String KEY;
        private final ArrayList<String> pins = new ArrayList<>();
        private final ArrayList<String> mInitialPositions = new ArrayList<>();

        RepoPinChecker(Context context) {
            prefs = context.getSharedPreferences(PREFS_KEY, Context.MODE_PRIVATE);
        }

        void setKey(String key) {
            KEY = key;
            final String[] savedPins = Util.stringArrayFromPrefs(prefs.getString(KEY, ""));
            pins.clear();
            pins.addAll(Arrays.asList(savedPins));
        }

        void pin(String path) {
            if(!pins.contains(path)) {
                pins.add(path);
                prefs.edit().putString(KEY, Util.stringArrayForPrefs(pins)).apply();
            }
        }

        void unpin(String path) {
            pins.remove(path);
            prefs.edit().putString(KEY, Util.stringArrayForPrefs(pins)).apply();
        }

        void setInitialPositions(List<Repository> repos) {
            mInitialPositions.clear();
            for(Repository r : repos) mInitialPositions.add(r.getFullName());
        }

        void appendInitialPositions(List<Repository> repos) {
            for(Repository r : repos) mInitialPositions.add(r.getFullName());
        }

        void appendPinnedPosition(String key) {
            mInitialPositions.add(0, key);
        }

        int initialPosition(String key) {
            return mInitialPositions.indexOf(key);
        }

        boolean isPinned(String path) {
            return pins.contains(path);
        }

        List<String> findNonLoadedPinnedRepositories() {
            final List<String> repos = new ArrayList<>();
            for(String pin : pins) {
                if(!mInitialPositions.contains(pin)) repos.add(pin);
            }
            return repos;
        }

    }

    public interface RepoOpener {

        void openRepo(Repository repo);

    }

}

```


