 
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

Placing two tildes on either side of a sequence will draw a strikethrough through it

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

Code can be inserted between triple backticks

\``` Language

 Some code

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

The ```ListView``` class was added to Android in API level 1.
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

In July 2012, GitHub released an official app for Android.

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
            <li> Display available information about a user
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
                    <li>Implement selecting the branch for which to show commits</li>
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
                    <li> Display the list of events which have occurred on the issue
                        <ol type="1">
                            <li> Closed - When the issue was closed, and the commit if it was closed from a commit message </li>
                            <li> Reopened- When the issue was re-opened, and by whom </li>
                            <li> Subscribed- When a user subscribed to the issue, and who subscribed </li>
                            <li> Merged- When the issue (A pull request) was merged, the commit, and the user that merged the issue </li>
                            <li> Referenced- When the issue was referenced from a commit message, and the commit </li>
                            <li> Mentioned- When a user was mentioned in the body, and the user that was mentioned </li>
                            <li> Assigned- When a user was assigned to the issue, the user that was assigned, and by whom </li>
                            <li> Unassigned- When a user was unassigned from the issue, the user that was unassigned, and by whom </li>
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
                    <li>The time that the commit was created</li>
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
                            <li>The number of comments on the issue</li>
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
                            <li>With a project, jump to a selected card from the id specified in a URL</li>
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
                            <li>Display short code blocks as monospaced text blocks within the text body</li>
                            <li>Display large code blocks as placeholders for a separate view</li>
                        </ol>
                    <li>Parse strikethroughs and create the correct spans </li>
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
                    <li>Display tables as placeholders for a separate view</li>
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
                            <li>Allow searching characters by their names</li>
                            <li>Insert the chosen characters into the text editor</li>
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

The fourth lists the user's Gists, with each item showing the Gist title and description.

The fifth lists the users that the authenticated user is following.

The sixth lists the users that are following the authenticated user.

Each user listed in the forth of fifth ```Fragments``` is displayed with their avatar and login.

![User viewholder](http://imgur.com/dGpUaJJ.png)

<div style="page-break-after: always;"></div>

#### Repository information Activity

The repository information ```Activity``` is also split into multiple ```Fragments```

The first ```Fragment``` should display information about the repository, as well as the users related to it.

![Repo info fragment](http://imgur.com/Efb2VjU.png)

Once populated with data this layout will show the user that created the repository, the size of the repository, the number of issues, the number of forks, the number of stars, and
the license.

If there are users collaborating on or contributing to the repository they will be shown in a ```HorizontalScrollView```.

The second ```Fragment``` should display the repository's README file.

The third ```Fragment``` should display the commits made to the repository, and allow selecting the branch for which to view commits.

![Repo commit fragment](http://imgur.com/Mp26ebe.png)


The fourth ```Fragment``` should display the issues on a repository, and allow filtering as well as searching the issues.

![Repo issues fragment](http://imgur.com/E5tVyok.png)

Each list item shows the issue state, the user that created the issue, issue information and an overflow button for performing actions on the issue.

The fifth and final ```Fragment``` displays each of the projects associated with the repository.

![Repo projects fragment](http://imgur.com/xbmrCIz.png)

Each list item displays the project name, its description, the last time it was updated, its state, and an overflow button for performing actions on the project.

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

The ```code``` parameter has a limited time frame to be exchanged for an authorization token.

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
As headers are key value pairs, they are represented as string to string maps.

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
The client checks if the URL contains `?code=', and if so,  passes the segment after that point to the ```OAuthHandler``` which then requests the authorization token.

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

```getAccessToken(final String code)``` performs a get request to the formatted token URL, and parses the response as a string.
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

If the user presses the authorize button, the page will be redirected through URL containing the path parameter "code".

In on the overridden ```onPageStarted``` method of the ```OAuthWebViewClient``` this results in the ```code``` parameter being passed to the ```OAuthHandler``` through ```fetchAccessToken```.

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
public Loader loadIssue(@NonNull final ItemLoader<Issue> loader, String repoFullName, int issueNumber, boolean highPriority) {
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
public Loader loadProject(@Nullable final ItemLoader<Project> loader, int id) {
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
This allows the response to be pre-loaded before an Activity is started, and only parsed to a ```DataModel``` once a user interface is present to use it.


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
public void onAttachFragment (Fragment fragment) {
        mWeakFragments.add(new WeakReference<>(fragment));
    }
```

```onAttachFragment``` adds a ```WeakReference``` to the ```Fragment``` to ```mWeakFragments```, and ```cancelNetworkRequests``` uses these to cancel network requests started by each ```Fragment```.

**BaseActivity.java**
``` java
private void cancelNetworkRequests() {
        AndroidNetworking.cancel(this);
        for(WeakReference<Fragment> ref : mWeakFragments) {
            if(ref.get() != null) {
                AndroidNetworking.cancel(ref.get());
            }
        }
    }
```

##### Memory Leak

A bug introduced in Android 5.0, launched in November 2014, which results in a reference to the ```Activity``` being kept in the ```TransitionManager```.
The memory leak can be solved by using reflection to remove the ```Activity``` from the ```TransitionManager``` map.

``` java
final ThreadLocal<WeakReference<ArrayMap<ViewGroup, ArrayList<Transition>>>> runningTransitions = (ThreadLocal<WeakReference<ArrayMap<ViewGroup, ArrayList<Transition>>>> runningTransitionsField.get(transitionManagerClass);
```

The ```runningTransitionsField``` refers to a ```ThreadLocal``` ```WeakReference``` to a map of ```ViewGroups``` to ```ArrayLists``` of ```Transitions```.

```ThreadLocal``` is a reference to a field, such that each ```Thread``` which access a thread-local variable via the ```TheadLocals``` get and set methods have their own copy of the variable.

A ```WeakReference``` is a reference which is not strong enough to prevent garbage collection.

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
```

The ```loadImage``` method is responsible for loading and displaying the image.

**NetworkImageView.java**
``` java
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
```

The first check performed in ```loadImage``` is whether the image can actually be drawn.

The ```View``` width and height are collected, before the ```LayoutParams``` are checked to determine whether the ```NetworkImageView``` is of a fixed size, or should expand to the size of its image.
If the ```NetworkImageView``` has 0 width and height, and doesn't wrap in either direction, then ```loadImage``` returns as it cannot display the image.

The second check is whether the URL is empty.
If a null or empty URL is passed, any current request is cancelled, and the default image is set.

The third check is whether the URL which is currently being loaded, or has been loaded is the same as the URL passed to the ```NetworkImageView```.
If the URL is already being loaded, ```loadImage``` returns, otherwise it cancels the current request in preparation for loading a new image.

Finally, the maximum width and height of the ```NetworkImageView``` are determined and the image is loaded.

The ```onResponse``` callback has two arguments, the response, and the isImmediate flag which indicates whether the response was returned form cache.

If both isImmediate and isInLayoutPass are true, the ```NetworkImageView``` has not yet been properly drawn an the image cannot yet be displayed.
In this case, a ```post``` call is made, which will add a runnable to the UI thread ```MessageQueue``` for execution once all other work on the UI thread is complete.

When the ```NetworkImageView``` is able to set the image, it checks whether the ```Bitmap``` is non null, and sets either the ```Bitmap``` or the default resource accordingly.

The ```NetworkImageView``` is used to display user avatars, in objectives 2.i.b, 2.v, 2.vi, 3.iv.c, 3.v.b.3, 3.v.b.4, 4.i.e, 4.i.i, 4.ii.a, 5.i.c, 5.iii.a.1, and 6.iv.b.6.


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

    private UI() {}

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
This method can be used to highlight an important ```View```, such as when jumping to a search result, such as in objective 6.ix.c.

The next four methods are used for unit conversion, converting pixels to density independent pixels, as well as converting density independent pixels or scale independent pixels to pixels.

As its name suggest, ```setStatusBarColor``` is used to set the status bar color for a ```Window```, which is required if the ```Activity``` uses a theme with transparency.

Finally, ```getSafeNavigationBarTransitionPair```is a utility for shared element transitions.
When a ```View``` in a ```RecyclerView``` is used in a shared element transition between two ```Activities```, the ```View``` is drawn through the ```ViewOverlay``` layer, which draws above the software navigation bar.
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
It produces two log messages for each call, the first details the request being sent, for example a request to the notifications API:
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

Each issue shows the version codes for which it occurred, as well as more detailed information about the circumstances of the crash.

![Issue info](http://imgur.com/7yqPZjf.png)

The crash information contains the full stack trace as well as information about the device on which the crash occurred.

![Device info](http://imgur.com/tPfFzWS.png)

<div style="page-break-after: always;"></div>

## Link handling

In order to receieve ```Intents``` when a user attempts to open a link to GitHub, the application must register an intent filter in its manifest.

The intent filter system allows specifying a host and a scheme to capture. It also allows specifying a path pattern to match the path against.

Unfortunately the pattern matching system is very limited.

An asterisk, "\*", matches a sequence of 0 or more occurences of the character immediately preceding it.
A period, ".", followed by an asterisk, "\*", matches any sequence of 0 or more characters.

This is of no use when matching GitHub URLs.

Instead, the application must match all GitHub URLs and reject those which it cannot handle by allowing the user to choose another application.

The manifest entry for the ```Interceptor``` ```Activity``` is therefore

``` XML
<activity
    android:name=".flow.Interceptor"
    android:theme="@android:style/Theme.NoDisplay">
    <intent-filter>
        <action android:name="android.intent.action.VIEW"/>

        <data
            android:host="github.com"
            android:scheme="http"/>
        <data
            android:host="github.com"
            android:scheme="https"/>

        <category android:name="android.intent.category.DEFAULT"/>
        <category android:name="android.intent.category.BROWSABLE"/>
    </intent-filter>
</activity>
```

The NoDisplay theme is specified, as the ```Activity``` should not display any content. The ```Interceptor``` ```Activity``` should also ensure that it calls ```finish``` before it exits
the ```onCreate``` method.

The intent filter specifies both schema for github.com, and adds the DEFAULT category, allowing the application to be chosen as the default for a particular URL, and the BROWSABLE 
category which allosws the application to be started by a web-browser.

**Interceptor.java**
``` java
package com.tpb.projects.flow;

import android.app.Activity;
import android.content.Intent;
import android.content.pm.PackageManager;
import android.content.pm.ResolveInfo;
import android.os.Bundle;
import android.os.Parcelable;

import com.tpb.projects.R;
import com.tpb.projects.commits.CommitActivity;
import com.tpb.projects.issues.IssueActivity;
import com.tpb.projects.milestones.MilestonesActivity;
import com.tpb.projects.project.ProjectActivity;
import com.tpb.projects.repo.RepoActivity;
import com.tpb.projects.repo.content.ContentActivity;
import com.tpb.projects.repo.content.FileActivity;
import com.tpb.projects.user.UserActivity;

import java.util.ArrayList;
import java.util.List;

/**
 * Created by theo on 01/01/17.
 */

public class Interceptor extends Activity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        if(getIntent().getAction().equals(Intent.ACTION_VIEW) &&
                getIntent().getData() != null &&
                "github.com".equals(getIntent().getData().getHost())) {
            final List<String> segments = getIntent().getData().getPathSegments();
            if(segments.size() == 0) {
               fail();
            } else if(segments.size() == 1) {
                final Intent u = new Intent(Interceptor.this, UserActivity.class);
                u.putExtra(getString(R.string.intent_username), segments.get(0));
                startActivity(u);
                overridePendingTransition(R.anim.slide_up, R.anim.none);
                finish();
            } else {
                final Intent i = new Intent();
                i.putExtra(getString(R.string.intent_repo), segments.get(0) + "/" + segments.get(1));
                switch(segments.size()) {
                    case 2: //Repo
                        i.setClass(Interceptor.this, RepoActivity.class);
                        break;
                    case 3:
                        if("projects".equals(segments.get(2))) {
                            i.setClass(Interceptor.this, RepoActivity.class);
                            i.putExtra(getString(R.string.intent_pager_page),
                                    RepoActivity.PAGE_PROJECTS
                            );
                        } else if("issues".equals(segments.get(2))) {
                            i.setClass(Interceptor.this, RepoActivity.class);
                            i.putExtra(getString(R.string.intent_pager_page),
                                    RepoActivity.PAGE_ISSUES
                            );
                        } else if("milestones".equals(segments.get(2))) {
                            i.setClass(Interceptor.this, MilestonesActivity.class);
                        } else if("commits".equals(segments.get(2))) {
                            i.setClass(Interceptor.this, RepoActivity.class);
                            i.putExtra(getString(R.string.intent_pager_page),
                                    RepoActivity.PAGE_COMMITS
                            );
                        }
                        break;
                    case 4:
                        if("projects".equals(segments.get(2))) {
                            i.setClass(Interceptor.this, ProjectActivity.class);
                            i.putExtra(getString(R.string.intent_project_number),
                                    safelyExtractInt(segments.get(3))
                            );
                            final String path = getIntent().getDataString();
                            final StringBuilder id = new StringBuilder();
                            for(int j = path
                                    .indexOf('#', path.indexOf(segments.get(3))) + 6; j < path
                                    .length(); j++) {
                                if(path.charAt(j) >= '0' && path.charAt(j) <= '9') {
                                    id.append(path.charAt(j));
                                }
                            }
                            final int cardId = safelyExtractInt(id.toString());
                            if(cardId != -1) i.putExtra(getString(R.string.intent_card_id), cardId);
                        } else if("issues".equals(segments.get(2))) {
                            i.setClass(Interceptor.this, IssueActivity.class);
                            i.putExtra(getString(R.string.intent_issue_number),
                                    safelyExtractInt(segments.get(3))
                            );
                        } else if("milestone".equals(segments.get(2))) {
                            i.setClass(Interceptor.this, MilestonesActivity.class);
                            i.putExtra(getString(R.string.intent_milestone_number),
                                    safelyExtractInt(segments.get(3))
                            );
                        } else if("commit".equals(segments.get(2))) {
                            i.setClass(Interceptor.this, CommitActivity.class);
                            i.putExtra(getString(R.string.intent_commit_sha), segments.get(3));
                        }
                        break;
                    default:
                        if("tree".equals(segments.get(2))) {
                            i.setClass(Interceptor.this, ContentActivity.class);
                            final StringBuilder path = new StringBuilder();
                            for(int j = 3; j < segments.size(); j++) {
                                path.append(segments.get(j));
                                path.append('/');
                            }
                            i.putExtra(getString(R.string.intent_path), path.toString());
                        } else if("blob".equals(segments.get(2))) {
                            i.setClass(Interceptor.this, FileActivity.class);
                            final StringBuilder path = new StringBuilder();
                            for(int j = 2; j < segments.size(); j++) {
                                path.append('/');
                                path.append(segments.get(j));
                            }
                            i.putExtra(getString(R.string.intent_blob_path), path.toString());
                        }
                }
                if(i.getComponent() != null && i.getComponent().getClassName() != null) {
                    startActivity(i);
                    overridePendingTransition(R.anim.slide_up, R.anim.none);
                    finish();
                } else {
                    fail();
                }
            }
        } else {
            fail();
        }
    }

    private static int safelyExtractInt(String possibleInt) {
        try {
            return Integer.parseInt(possibleInt.replace("\\s+", ""));
        } catch(NumberFormatException nfe) {
            return -1;
        }
    }


    private void fail() {
        try {
            startActivity(generateFailIntentWithoutApp());
        } catch(Exception e) {
            e.printStackTrace();
        } finally {
            finish();
        }
    }


    private Intent generateFailIntentWithoutApp() {
        try {
            final Intent intent = new Intent(getIntent().getAction());
            intent.setData(getIntent().getData());
            intent.addCategory(Intent.CATEGORY_BROWSABLE);
            intent.addCategory(Intent.CATEGORY_DEFAULT);

            final List<ResolveInfo> resolvedInfo = getPackageManager()
                    .queryIntentActivities(intent, PackageManager.MATCH_DEFAULT_ONLY);

            if(!resolvedInfo.isEmpty()) {
                final List<Intent> targetedShareIntents = new ArrayList<>();
                for(ResolveInfo resolveInfo : resolvedInfo) {
                    final String packageName = resolveInfo.activityInfo.packageName;
                    if(!packageName.equals(getPackageName())) {
                        final Intent targetedShareIntent = new Intent(getIntent().getAction());
                        targetedShareIntent.setData(getIntent().getData());
                        targetedShareIntent.setPackage(packageName);
                        targetedShareIntents.add(targetedShareIntent);
                    }
                }
                final Intent chooserIntent = Intent.createChooser(targetedShareIntents.remove(0),
                        getString(R.string.text_interceptor_open_with)
                );
                if(targetedShareIntents.size() > 0) {
                    chooserIntent.putExtra(Intent.EXTRA_INITIAL_INTENTS, targetedShareIntents
                            .toArray(new Parcelable[targetedShareIntents.size()]));
                }
                return chooserIntent;
            }
        } catch(Exception ignored) {
        }
        return null;
    }

}


```

### Possible paths

When onCreate is called, the first check is that the ```Intent``` action is ACTION_VIEW, the ```Intent``` has data, and the data host is github.com, if it is not ```fail``` is called.

If the URL is from GitHub, the ```List``` of path segments are extracted.

If the segments size is 0, the URL is github.com, and the ```Interceptor``` fails because it has not information to parse.

If the segments size is 1, the only possibility is a user account, so an ```Intent``` for the ```UserActivity``` is created and the first segment is added as an extra under the "username"
key. The ```Intent``` is then started and ```Interceptor``` finishes.

If there is more than one segment, there are many more possibilities.
Each of these possibilities contains a path to a repository. The URL is of the form "github.com/user/repository/...".

A new ```Intent``` is created, and the repository path is added to the ```Intent``` extras under the "repo" key.

A switch statement over the size of the segments ```List``` is then used to determine the ```Intent``` class.

2- The class is set to the repository ```Activity```
3- The URL could be to a repository's projects, issues, milestones, or commits.
The third item in the ```List``` is checked, the class is set, and a page number is added to the ```Intent``` extras under the "page" key, allowing the ```Activity``` to scroll to a 
particular page on launch.
4- The URL could be a particular project, issue, milestone, or commit
    - Projects
        A projects URL should contain the integer value of the project's if as the fourth item in the ```List```.
        It may also contain a URL parameter for the id of the card reference in the project, this is an integer value after the string "#card-".
        Both of these values are added to the ```Intent``` as extras and the class is set to the Project ```Activity```.
    - Issues 
        A path with a third segment of "issues" and a length of 4 refers to a particular issue.
        The id of the issue is extracted from the fourth segment and added to the ```Intent```, the class is set to the issue ```Activity```.
    - Milestone
        The milestone id is extracted in the same way as an issue id, and the ```Activity``` class is set accordingly.
    - Commit
        The commit path does not contain a numeric id, but instead contains a SHA hash which is added as an extra to the ```Intent``` after the commit ```Activity``` is set.
Other-
Any values greater than 5 items refers to a file or directory in the repository files.

If the third path segment is "tree", the URL refers to a directory within a project. In this case the tree path is built and added to the ```Intent``` as an extra.
If the third path segment is "blob", the URl refers to an individual file within the project. The blob path is built and added to the ```Intent``` as an extra.


Outside of the switch, the method checks that the component class has been set on the ```Intent```.
If it has, the ```Intent``` is launched and ```Interceptor``` finishes.
Otherwise the ```fail``` method is called.

### Showing a chooser

In the event that the ```Interceptor``` fails to determine an ```Activity``` to handle a particular URL, it should suggest other applications which might be able to handle the URL, objective 7.ii.

In order to open a link in another app a chooser dialog should be shown.
This is done by building an implicit ```Intent```, not declaring the class to handle the ```Intent``` but allowing the user to choose from the available applications.

This is handled in ```generateFailIntentWithoutApp```.
The method creates a new ```Intent```, with the same action as the ```Intent``` that launched ```Interceptor```.
It then continues to add the data that was launched with ```Interceptor``` to the new ```Intent```, and add the BROWSABLE and DEFAULT categories.

Next a ```List``` of ```ResolveInfo``` objects is collected, each of which contains information about an application which could handle the new ```Intent```.

If the ```List``` is not empty, a new ```List``` of ```Intents``` is created, and a new ```Intent``` is created for each of the applications, using their package names, if the package name is not the name of this app.

Finally, the chooser ```Intent``` is created using the first ```Intent``` in the targetedShareIntents ```List```, and the rest of the ```Intents``` are added to the chooser.

<div style="page-break-after: always;"></div>

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

Direct span implementations of ```ParagraphStyle``` include ```BulletSpan```, ```QuoteSpan```, and ```DrawableMarginSpan```, while numerous interfaces such as ```AlignmentSpan``` and ```LeadingMarginSpan``` also extend from it.

#### UpdateAppearance

The ```UpdateAppearance``` interface is for spans which affect character-level text "in a way that modifies their appearance when one is added or removed".

Implementations include ```AbsoluteSizeSpan``` which is used to scale text using either density independent pixels, or an absolute pixel size.

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
public static String formatMD(@NonNull String s, @Nullable String fullRepoPath, boolean linkUsernames) {
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
            } else if(chars[i] == '(' && fullRepoPath != null) {
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
private static boolean isWhiteSpace(char c) {
        //Space tab, newline, line tabulation, carriage return, form feed
        return c == ' ' || c == '\t' || c == '\n' || c == '\u000B' || c == '\r' || c == '\u000C';
    }
```

**Markdown.java**
``` java
private static boolean isLineEnding(char[] cs, int i) {
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

This fulfills objective 9.ii.g.

The next check is for image links, which need to be parsed both in order to deal with links relative to the repository and to add spacing around them as they will be displayed as images.

The next check is for emojis, which are contained between two colons.

If there is a sequence of three backticks, every character from the backticks onward is appended without formatting until the next set of backticks is found.

Finally, if none of the above conditions apply, the character is appended to the builder.

At the end of each iteration the previous and previous previous characters are updated.

#### Username mentions

GitHub usernames are strings of text up to 39 characters in length, containing only alphanumeric characters and hypens.

**Markdown.java**
``` java
private static int parseUsername(StringBuilder builder, char[] cs, int pos) {
        final StringBuilder nameBuilder = new StringBuilder();
        char p = ' ';
        for(int i = pos + 1; i < cs.length; i++) {
            if(((cs[i] >= 'A' && cs[i] <= 'Z') ||
                    (cs[i] >= 'a' && cs[i] <= 'z') ||
                    (cs[i] >= '0' && cs[i] <= '9') ||
                    (cs[i] == '-' && p != '-')) &&
                    i - pos <= 39 &&
                    i != cs.length - 1) {
                nameBuilder.append(cs[i]);
                p = cs[i];
                //nameBuilder.length() > 0 stop us linking a single @
            } else if((isWhiteSpace(cs[i]) || i == cs.length - 1) && i - pos > 1) {
                if(i == cs.length - 1) {
                    nameBuilder.append(cs[i]); //Otherwise we would miss the last char of the name
                }
                builder.append("[@");
                builder.append(nameBuilder.toString());
                builder.append("](https://github.com/");
                builder.append(nameBuilder.toString());
                builder.append(')');
                if(i != cs.length - 1) {
                    builder.append(cs[i]); // We still need to append the space or newline
                }
                return i;
            } else {
                break;
            }

        }
        builder.append("@");
        return pos;
    }
```

```parseUsername``` iterates through the character array from the position after the "@".
If the character is alphanumeric or the character is "-" and the previous character is not, and the name limit has not been exceeded, the character is appended to the `nameBuilder` and
the previous character is set.
If the character is not a valid part of the name there are two possibilities. Either the name should be matched, or the name is not valid.

If the character is whitespace, or we are at the end of the character array the name is valid if it is also of non-zero length.
If the name is valid the name link is appended to the `StringBuilder` used for the formatted markdown.
Aside from appending the link, there are two other cases which must be dealt with.
If the break point for the name is the end of the character array, then the last character in the array must be added.
Second, if the break point is not the end of the character array, then the whitespace character must be added.
Once the name has been added, the counter position is returned.

If the name is not valid, the loop breaks, "@" character is appended, and the original position is returned.

This method completes objective 9.ii.d.

#### Issue links

GitHub issue links are hashes, "#", followed by integer strings.

**Markdown.java**
``` java
private static int parseIssue(StringBuilder builder, char[] cs, int pos, String fullRepoPath) {
        final StringBuilder numBuilder = new StringBuilder();
        for(int i = pos + 1; i < cs.length; i++) {
            if(cs[i] >= '0' && cs[i] <= '9' && i != cs.length - 1) {
                numBuilder.append(cs[i]);
            } else if((isWhiteSpace(cs[i]) || isLineEnding(cs, i)))  {
                if(i == cs.length - 1) {
                    if(cs[i] >= '0' && cs[i] <= '9') {
                        numBuilder.append(cs[i]);
                    } else if(!isWhiteSpace(cs[i])){
                        break;
                    }
                }
                builder.append("[#");
                builder.append(numBuilder.toString());
                builder.append("](https://github.com/");
                builder.append(fullRepoPath);
                builder.append("/issues/");
                builder.append(numBuilder.toString());
                builder.append(")");
                if(i != cs.length - 1) {
                    builder.append(cs[i]); // We still need to append the whitespace
                }
                return i;
            } else {
                break;
            }
        }
        builder.append("#");
        return pos;
    }
```

```parseIssue``` checks if each character is a numeric value, adding it to numBuilder if so.
If the character is instead whitespace or a line ending the issue link may be valid.
If we are at the end of the character array the final character must be checked for validity, and added to numBuilder, otherwise the loop breaks.
The link is built, and if the counter is not at the end of the array the original whitespace is appended.
If the character was not a valid issue link, the hash, "#", is appended and the original index is returned.

This method completes objective 9.ii.e.

#### Relative links

When Markdown is rendered in a GitHub repository, links can be relative to the repository.
In order to load content from these links they need to be changed to a full link including the repository path.

**Markdown.java**
``` java
private static String concatenateRawContentUrl(String url, String fullRepoName) {
        if(url.startsWith("http://") ||url.startsWith("https://")) return url;
        int offset = 0;
        if(url.startsWith("./")) offset = 2;
        else if(url.startsWith("/")) offset = 1;
        return "https://raw.githubusercontent.com/" + fullRepoName + "/master/" + url
                    .substring(offset);
    }
```

A relative URL can be only a file name or it can start with either "/" or "./" specifying a path in the repository.

If the URL begins with "http://" or "https://" it is assumed to be valid and returned.
Otherwise, the offset is calculated and the URL is added as the file path in a link to githubusercontent.

The ```concatenateRawContentUrl``` function is used when parsing image links, as well as when checking links in a repository README.

#### Image links

Image links are checked both to ensure that they are not relative, and to add spacing around each image so that it does not interfere with the text line spacing.

**Markdown.java**
``` java
private static int parseImageLink(StringBuilder builder, char[] cs, int pos, String fullRepoPath) {
        for(int i = pos + 1; i < cs.length; i++) {
            if(cs[i] == ')') {
                final String link = new String(Arrays.copyOfRange(cs, pos + 1, i));
                final String extension = link.substring(link.lastIndexOf('.') + 1);
                if("png".equalsIgnoreCase(extension) ||
                        "jpg".equalsIgnoreCase(extension) ||
                        "gif".equalsIgnoreCase(extension) ||
                        "bmp".equalsIgnoreCase(extension) ||
                        "webp".equalsIgnoreCase(extension)) {
                    if(TextUtils.isValidURL(link)) {
                        builder.append(link);
                    } else {
                        builder.append(concatenateRawContentUrl(link, fullRepoPath));
                    }
                    builder.append(") <br><br>");
                } else {
                    builder.append(link);
                    builder.append(")");
                }
                return i;
            } else if(isWhiteSpace(cs[i])) {
                break;
            }
        }

        return pos;
    }
```

Recalling that a markdown image link is formatted as ```![Description](link)```, ```parseImageLink``` must check the text between the opening bracket, "(", and the closing bracket ")".

If whitespace is found, the function can break early.
When the closing bracket is found, the extension can be checked for any of the image extensions supported by Android.
If the URL is already valid, it is added to the builder. Otherwise the concatenated URL is added.
Finally, the closing bracket and breaks are added.

If the URL does not end with an image extension, it is just added to the builder.

This method completes objective 9.ii.c.1.

#### Emoji

Emoji are added to GitHub Markdown by specifying their alias between two colons. For example ":smile\:" should be rendered as 😄.

##### Loading Emoji

In order to parse each alias to a unicode string, and later allow searching, a table of emojis is required.
I used the emoji json file used in GitHub's gemoji, a Ruby gem for "character information about native emoji".
After stripping the unicode version, ios version, and fitzpatrick information from the file, and minifying it I reduced it from 298kb to 139kb.

Each Emoji contains its description, aliases, tags, and unicode string.

**Emoji.java**
``` java
package com.tpb.mdtext.emoji;

import java.util.Collections;
import java.util.List;

/**
 * Created by theo on 16/04/17.
 */

public class Emoji {

    private final String description;
    private final List<String> aliases;
    private final List<String> tags;
    private final String unicode;

    Emoji(String unicode, String description, List<String> aliases, List<String> tags) {
        this.unicode = unicode;
        this.description = description;
        this.aliases = Collections.unmodifiableList(aliases);
        this.tags = Collections.unmodifiableList(tags);
    }

    public String getDescription() {
        return description;
    }

    public List<String> getAliases() {
        return aliases;
    }

    public List<String> getTags() {
        return tags;
    }

    public String getUnicode() {
        return unicode;
    }

    @Override
    public boolean equals(Object o) {
        return o instanceof Emoji && unicode.equals(((Emoji) o).unicode);
    }

    @Override
    public int hashCode() {
        return unicode.hashCode();
    }

    @Override
    public String toString() {
        return "Emoji{" +
                "description='" + description + '\'' +
                ", aliases=" + aliases +
                ", tags=" + tags +
                ", unicode='" + unicode + '\'' +
                '}';
    }
}

```

The emoji are loaded from the resource directory and added to a master list of emojis as well as maps for aliases and tags.

**EmojiLoader.java**
``` java
package com.tpb.mdtext.emoji;

import android.content.res.AssetManager;
import android.support.annotation.NonNull;

import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Set;

/**
 * Created by theo on 16/04/17.
 */

public class EmojiLoader {

    private static final Map<String, Emoji> ALIAS_MAP = new HashMap<>();
    private static final Map<String, Set<Emoji>> TAG_MAP = new HashMap<>();
    private static final List<Emoji> EMOJIS  = new ArrayList<>();

    public static void loadEmojis(AssetManager assets) {
        if(EMOJIS.size() > 0) return;
        try {
            final JSONArray JSON = new JSONArray(inputStreamToString(assets.open("json/emojis.json")));
            for(int i = 0; i < JSON.length(); i++) {
                final Emoji emoji = buildEmojiFromJSON(JSON.getJSONObject(i));
                if(emoji != null) {
                    EMOJIS.add(emoji);
                    for(String tag : emoji.getTags()) {
                        if(TAG_MAP.get(tag) == null) TAG_MAP.put(tag, new HashSet<Emoji>());
                        TAG_MAP.get(tag).add(emoji);
                    }
                    for(String alias : emoji.getAliases()) {
                        ALIAS_MAP.put(alias, emoji);
                    }
                }
            }
        } catch(Exception ignored) {}
    }

    private static String inputStreamToString(InputStream is) throws IOException {
        final BufferedReader reader = new BufferedReader(  new InputStreamReader(is));
        String line;
        final StringBuilder builder = new StringBuilder();
        while((line =  reader.readLine()) != null){
            builder.append(line);
        }
        is.close();
        return builder.toString();
    }

    private static Emoji buildEmojiFromJSON(JSONObject json) throws JSONException {
        if (!json.has("emoji")) {
            return null;
        }
        String emoji  = json.getString("emoji");
        String description = null;
        if (json.has("description")) {
            description = json.getString("description");
        }

        List<String> aliases = JSONArrayToStringList(json.getJSONArray("aliases"));
        List<String> tags = JSONArrayToStringList(json.getJSONArray("tags"));
        return new Emoji(emoji, description, aliases, tags);
    }

    private static List<String> JSONArrayToStringList(JSONArray array) throws JSONException {
        final List<String> strings = new ArrayList<>(array.length());
        for (int i = 0; i < array.length(); i++) {
            strings.add(array.getString(i));
        }
        return strings;
    }

    public static List<Emoji> getAllEmoji() {
        return EMOJIS;
    }

    public static Set<Emoji> getEmojiForTag(@NonNull String tag) {
        return TAG_MAP.get(tag);
    }

    public static Emoji getEmojiForAlias(@NonNull String alias) {
        if(alias.startsWith(":")) alias = alias.substring(1, alias.length());
        if(alias.endsWith(":")) alias = alias.substring(0, alias.length() - 1);
        return ALIAS_MAP.get(alias);
    }

}

```

The json file is opened as an ```InputStream``` which is then converted to a ```String``` which can be read as a ```JSONArray``` for conversion to ```Emoji``` objects.
For each ```Emoji``` successfully created the object is added to EMOJIS, added to ALIAS_MAP, and added to a ```HashSet``` of ```Emojis``` in TAG_MAP.

The two ```HashMaps``` can later be used to retrieve ```Emojis``` by their tags or aliases.

```getEmojiForAlias``` also checks whether the alias is in the ":alias:" format and strips the colons before searching.

##### Displaying Emoji

**Markdown.java**
``` java
private static int parseEmoji(StringBuilder builder, char[] cs, int pos) {
        final StringBuilder emojiBuilder = new StringBuilder();

        for(int i = pos + 1; i < cs.length; i++) {
            if((cs[i] >= 'A' && cs[i] <= 'Z') ||
                    (cs[i] >= '0' && cs[i] <= '9') ||
                    (cs[i] >= 'a' && cs[i] <= 'z') ||
                    cs[i] == '_') {
                emojiBuilder.append(cs[i]);
            } else if(cs[i] == ':') {
                final Emoji eww = EmojiLoader.getEmojiForAlias(emojiBuilder.toString());
                if(eww == null) break;
                builder.append(eww.getUnicode());
                return i;
            } else {
                break;
            }
        }
        builder.append(":");
        return pos;
    }
```

```parseEmoji``` iterates through the character array after a colon, adding each alphanumeric or underscore character to a ```StringBuilder```.

If another colon is reached, the ```Emoji``` is loaded from ```EmojiLoader``` and if it is non null, the emoji unicode string is added to the parsed text.
Otherwise the colon is appended and the original position is returned.

This method completes objective 9.ii.f.

### Text Utilities and Android Regex patterns

#### Linkify

```Linkify``` is Android's utility for adding clickable links to text. It can match URLs, email addresses, phone numbers, and map addresses.
There is no need to match phone numbers and map addresses in GitHub Markdown, however URLs and email addresses should be linked.

The Android regex for URLs is quite large.
It contains the IANA (Internet Assigned Numbers Authority) top level domain names, as well as the characters in the RFC 3987 specification which extends upon the Uniform Resource Identifier scheme.


The ```Pattern``` class also contains the WORD_BOUNDARY regex which is "(?:\b|$|^)".

The regex begins with "?:". This begins a non-capturing group, which matches a pattern, but does not include it in the result.
The "\b" character is an anchor which matches any word boundary position. A word boundary is any of the three following positions
- Before the first character in the string, if the first character is a word character.
- After the last character in the string, if the last character is a word character.
- Between two characters in the string, where one is a word character and the other is not a word character.

The dollar, "$", is used to match the position before the first newline in a string.

Finally, the hat, "^", is used to match the beginning of a string.

This pattern is used to ensure that invalid domain names such as ".coma" do not match as valid domain names such as ".com"

The ```Pattern``` used for autolinking web URLs is  ""(" + WEB_URL_WITH_PROTOCOL + "|" + WEB_URL_WITHOUT_PROTOCOL + ")"" which matches any URL with or without a protocol.

This ```Pattern``` is likely to cause problems with code in the Markdown.

For example, "System.out.println()" would be matched as a URL.

This problem can be fixed by modifying the regex to required that the character after the URL is whitespace or a line ending, making the regex
""(" + WEB_URL_WITH_PROTOCOL + "|" + WEB_URL_WITHOUT_PROTOCOL + ")($|\\s)")"

The regex patterns, for autolinking URLs and email addresses can then be combined to a single pattern "AUTOLINK_WEB_URL + "|" + AUTOLINK_EMAIL_ADDRESS"

These patterns are included in the ```MDPattern``` class as they are private in the ```Pattern``` source class.

**MDPattern.java**
``` java
package com.tpb.mdtext;

import java.util.regex.Pattern;

/**
 * Created by theo on 05/03/17.
 */

public class MDPattern {

    /**
     * Regular expression to match all IANA top-level domains.
     * <p>
     * List accurate as of 2015/11/24.  List taken from:
     * http://data.iana.org/TLD/tlds-alpha-by-domain.txt
     * This pattern is auto-generated by frameworks/ex/common/tools/make-iana-tld-pattern.py
     *
     * @hide
     */
    private static final String IANA_TOP_LEVEL_DOMAINS =
            "(?:"
                    + "(?:aaa|aarp|abb|abbott|abogado|academy|accenture|accountant|accountants|aco|active"
                    + "|actor|ads|adult|aeg|aero|afl|agency|aig|airforce|airtel|allfinanz|alsace|amica|amsterdam"
                    + "|android|apartments|app|apple|aquarelle|aramco|archi|army|arpa|arte|asia|associates"
                    + "|attorney|auction|audio|auto|autos|axa|azure|a[cdefgilmoqrstuwxz])"
                    + "|(?:band|bank|bar|barcelona|barclaycard|barclays|bargains|bauhaus|bayern|bbc|bbva"
                    + "|bcn|beats|beer|bentley|berlin|best|bet|bharti|bible|bid|bike|bing|bingo|bio|biz|black"
                    + "|blackfriday|bloomberg|blue|bms|bmw|bnl|bnpparibas|boats|bom|bond|boo|boots|boutique"
                    + "|bradesco|bridgestone|broadway|broker|brother|brussels|budapest|build|builders|business"
                    + "|buzz|bzh|b[abdefghijmnorstvwyz])"
                    + "|(?:cab|cafe|cal|camera|camp|cancerresearch|canon|capetown|capital|car|caravan|cards"
                    + "|care|career|careers|cars|cartier|casa|cash|casino|cat|catering|cba|cbn|ceb|center|ceo"
                    + "|cern|cfa|cfd|chanel|channel|chat|cheap|chloe|christmas|chrome|church|cipriani|cisco"
                    + "|citic|city|cityeats|claims|cleaning|click|clinic|clothing|cloud|club|clubmed|coach"
                    + "|codes|coffee|college|cologne|com|commbank|community|company|computer|comsec|condos"
                    + "|construction|consulting|contractors|cooking|cool|coop|corsica|country|coupons|courses"
                    + "|credit|creditcard|creditunion|cricket|crown|crs|cruises|csc|cuisinella|cymru|cyou|c[acdfghiklmnoruvwxyz])"
                    + "|(?:dabur|dad|dance|date|dating|datsun|day|dclk|deals|degree|delivery|dell|delta"
                    + "|democrat|dental|dentist|desi|design|dev|diamonds|diet|digital|direct|directory|discount"
                    + "|dnp|docs|dog|doha|domains|doosan|download|drive|durban|dvag|d[ejkmoz])"
                    + "|(?:earth|eat|edu|education|email|emerck|energy|engineer|engineering|enterprises"
                    + "|epson|equipment|erni|esq|estate|eurovision|eus|events|everbank|exchange|expert|exposed"
                    + "|express|e[cegrstu])"
                    + "|(?:fage|fail|fairwinds|faith|family|fan|fans|farm|fashion|feedback|ferrero|film"
                    + "|final|finance|financial|firmdale|fish|fishing|fit|fitness|flights|florist|flowers|flsmidth"
                    + "|fly|foo|football|forex|forsale|forum|foundation|frl|frogans|fund|furniture|futbol|fyi"
                    + "|f[ijkmor])"
                    + "|(?:gal|gallery|game|garden|gbiz|gdn|gea|gent|genting|ggee|gift|gifts|gives|giving"
                    + "|glass|gle|global|globo|gmail|gmo|gmx|gold|goldpoint|golf|goo|goog|google|gop|gov|grainger"
                    + "|graphics|gratis|green|gripe|group|gucci|guge|guide|guitars|guru|g[abdefghilmnpqrstuwy])"
                    + "|(?:hamburg|hangout|haus|healthcare|help|here|hermes|hiphop|hitachi|hiv|hockey|holdings"
                    + "|holiday|homedepot|homes|honda|horse|host|hosting|hoteles|hotmail|house|how|hsbc|hyundai"
                    + "|h[kmnrtu])"
                    + "|(?:ibm|icbc|ice|icu|ifm|iinet|immo|immobilien|industries|infiniti|info|ing|ink|institute"
                    + "|insure|int|international|investments|ipiranga|irish|ist|istanbul|itau|iwc|i[delmnoqrst])"
                    + "|(?:jaguar|java|jcb|jetzt|jewelry|jlc|jll|jobs|joburg|jprs|juegos|j[emop])"
                    + "|(?:kaufen|kddi|kia|kim|kinder|kitchen|kiwi|koeln|komatsu|krd|kred|kyoto|k[eghimnprwyz])"
                    + "|(?:lacaixa|lancaster|land|landrover|lasalle|lat|latrobe|law|lawyer|lds|lease|leclerc"
                    + "|legal|lexus|lgbt|liaison|lidl|life|lifestyle|lighting|limited|limo|linde|link|live"
                    + "|lixil|loan|loans|lol|london|lotte|lotto|love|ltd|ltda|lupin|luxe|luxury|l[abcikrstuvy])"
                    + "|(?:madrid|maif|maison|man|management|mango|market|marketing|markets|marriott|mba"
                    + "|media|meet|melbourne|meme|memorial|men|menu|meo|miami|microsoft|mil|mini|mma|mobi|moda"
                    + "|moe|moi|mom|monash|money|montblanc|mormon|mortgage|moscow|motorcycles|mov|movie|movistar"
                    + "|mtn|mtpc|mtr|museum|mutuelle|m[acdeghklmnopqrstuvwxyz])"
                    + "|(?:nadex|nagoya|name|navy|nec|net|netbank|network|neustar|new|news|nexus|ngo|nhk"
                    + "|nico|ninja|nissan|nokia|nra|nrw|ntt|nyc|n[acefgilopruz])"
                    + "|(?:obi|office|okinawa|omega|one|ong|onl|online|ooo|oracle|orange|org|organic|osaka"
                    + "|otsuka|ovh|om)"
                    + "|(?:page|panerai|paris|partners|parts|party|pet|pharmacy|philips|photo|photography"
                    + "|photos|physio|piaget|pics|pictet|pictures|ping|pink|pizza|place|play|playstation|plumbing"
                    + "|plus|pohl|poker|porn|post|praxi|press|pro|prod|productions|prof|properties|property"
                    + "|protection|pub|p[aefghklmnrstwy])"
                    + "|(?:qpon|quebec|qa)"
                    + "|(?:racing|realtor|realty|recipes|red|redstone|rehab|reise|reisen|reit|ren|rent|rentals"
                    + "|repair|report|republican|rest|restaurant|review|reviews|rich|ricoh|rio|rip|rocher|rocks"
                    + "|rodeo|rsvp|ruhr|run|rwe|ryukyu|r[eosuw])"
                    + "|(?:saarland|sakura|sale|samsung|sandvik|sandvikcoromant|sanofi|sap|sapo|sarl|saxo"
                    + "|sbs|sca|scb|schmidt|scholarships|school|schule|schwarz|science|scor|scot|seat|security"
                    + "|seek|sener|services|seven|sew|sex|sexy|shiksha|shoes|show|shriram|singles|site|ski"
                    + "|sky|skype|sncf|soccer|social|software|sohu|solar|solutions|sony|soy|space|spiegel|spreadbetting"
                    + "|srl|stada|starhub|statoil|stc|stcgroup|stockholm|studio|study|style|sucks|supplies"
                    + "|supply|support|surf|surgery|suzuki|swatch|swiss|sydney|systems|s[abcdeghijklmnortuvxyz])"
                    + "|(?:tab|taipei|tatamotors|tatar|tattoo|tax|taxi|team|tech|technology|tel|telefonica"
                    + "|temasek|tennis|thd|theater|theatre|tickets|tienda|tips|tires|tirol|today|tokyo|tools"
                    + "|top|toray|toshiba|tours|town|toyota|toys|trade|trading|training|travel|trust|tui|t[cdfghjklmnortvwz])"
                    + "|(?:ubs|university|uno|uol|u[agksyz])"
                    + "|(?:vacations|vana|vegas|ventures|versicherung|vet|viajes|video|villas|vin|virgin"
                    + "|vision|vista|vistaprint|viva|vlaanderen|vodka|vote|voting|voto|voyage|v[aceginu])"
                    + "|(?:wales|walter|wang|watch|webcam|website|wed|wedding|weir|whoswho|wien|wiki|williamhill"
                    + "|win|windows|wine|wme|work|works|world|wtc|wtf|w[fs])"
                    + "|(?:\u03b5\u03bb|\u0431\u0435\u043b|\u0434\u0435\u0442\u0438|\u043a\u043e\u043c|\u043c\u043a\u0434"
                    + "|\u043c\u043e\u043d|\u043c\u043e\u0441\u043a\u0432\u0430|\u043e\u043d\u043b\u0430\u0439\u043d"
                    + "|\u043e\u0440\u0433|\u0440\u0443\u0441|\u0440\u0444|\u0441\u0430\u0439\u0442|\u0441\u0440\u0431"
                    + "|\u0443\u043a\u0440|\u049b\u0430\u0437|\u0570\u0561\u0575|\u05e7\u05d5\u05dd|\u0627\u0631\u0627\u0645\u0643\u0648"
                    + "|\u0627\u0644\u0627\u0631\u062f\u0646|\u0627\u0644\u062c\u0632\u0627\u0626\u0631|\u0627\u0644\u0633\u0639\u0648\u062f\u064a\u0629"
                    + "|\u0627\u0644\u0645\u063a\u0631\u0628|\u0627\u0645\u0627\u0631\u0627\u062a|\u0627\u06cc\u0631\u0627\u0646"
                    + "|\u0628\u0627\u0632\u0627\u0631|\u0628\u06be\u0627\u0631\u062a|\u062a\u0648\u0646\u0633"
                    + "|\u0633\u0648\u062f\u0627\u0646|\u0633\u0648\u0631\u064a\u0629|\u0634\u0628\u0643\u0629"
                    + "|\u0639\u0631\u0627\u0642|\u0639\u0645\u0627\u0646|\u0641\u0644\u0633\u0637\u064a\u0646"
                    + "|\u0642\u0637\u0631|\u0643\u0648\u0645|\u0645\u0635\u0631|\u0645\u0644\u064a\u0633\u064a\u0627"
                    + "|\u0645\u0648\u0642\u0639|\u0915\u0949\u092e|\u0928\u0947\u091f|\u092d\u093e\u0930\u0924"
                    + "|\u0938\u0902\u0917\u0920\u0928|\u09ad\u09be\u09b0\u09a4|\u0a2d\u0a3e\u0a30\u0a24|\u0aad\u0abe\u0ab0\u0aa4"
                    + "|\u0b87\u0ba8\u0bcd\u0ba4\u0bbf\u0baf\u0bbe|\u0b87\u0bb2\u0b99\u0bcd\u0b95\u0bc8|\u0b9a\u0bbf\u0b99\u0bcd\u0b95\u0baa\u0bcd\u0baa\u0bc2\u0bb0\u0bcd"
                    + "|\u0c2d\u0c3e\u0c30\u0c24\u0c4d|\u0dbd\u0d82\u0d9a\u0dcf|\u0e04\u0e2d\u0e21|\u0e44\u0e17\u0e22"
                    + "|\u10d2\u10d4|\u307f\u3093\u306a|\u30b0\u30fc\u30b0\u30eb|\u30b3\u30e0|\u4e16\u754c"
                    + "|\u4e2d\u4fe1|\u4e2d\u56fd|\u4e2d\u570b|\u4e2d\u6587\u7f51|\u4f01\u4e1a|\u4f5b\u5c71"
                    + "|\u4fe1\u606f|\u5065\u5eb7|\u516b\u5366|\u516c\u53f8|\u516c\u76ca|\u53f0\u6e7e|\u53f0\u7063"
                    + "|\u5546\u57ce|\u5546\u5e97|\u5546\u6807|\u5728\u7ebf|\u5927\u62ff|\u5a31\u4e50|\u5de5\u884c"
                    + "|\u5e7f\u4e1c|\u6148\u5584|\u6211\u7231\u4f60|\u624b\u673a|\u653f\u52a1|\u653f\u5e9c"
                    + "|\u65b0\u52a0\u5761|\u65b0\u95fb|\u65f6\u5c1a|\u673a\u6784|\u6de1\u9a6c\u9521|\u6e38\u620f"
                    + "|\u70b9\u770b|\u79fb\u52a8|\u7ec4\u7ec7\u673a\u6784|\u7f51\u5740|\u7f51\u5e97|\u7f51\u7edc"
                    + "|\u8c37\u6b4c|\u96c6\u56e2|\u98de\u5229\u6d66|\u9910\u5385|\u9999\u6e2f|\ub2f7\ub137"
                    + "|\ub2f7\ucef4|\uc0bc\uc131|\ud55c\uad6d|xbox"
                    + "|xerox|xin|xn\\-\\-11b4c3d|xn\\-\\-1qqw23a|xn\\-\\-30rr7y|xn\\-\\-3bst00m|xn\\-\\-3ds443g"
                    + "|xn\\-\\-3e0b707e|xn\\-\\-3pxu8k|xn\\-\\-42c2d9a|xn\\-\\-45brj9c|xn\\-\\-45q11c|xn\\-\\-4gbrim"
                    + "|xn\\-\\-55qw42g|xn\\-\\-55qx5d|xn\\-\\-6frz82g|xn\\-\\-6qq986b3xl|xn\\-\\-80adxhks"
                    + "|xn\\-\\-80ao21a|xn\\-\\-80asehdb|xn\\-\\-80aswg|xn\\-\\-90a3ac|xn\\-\\-90ais|xn\\-\\-9dbq2a"
                    + "|xn\\-\\-9et52u|xn\\-\\-b4w605ferd|xn\\-\\-c1avg|xn\\-\\-c2br7g|xn\\-\\-cg4bki|xn\\-\\-clchc0ea0b2g2a9gcd"
                    + "|xn\\-\\-czr694b|xn\\-\\-czrs0t|xn\\-\\-czru2d|xn\\-\\-d1acj3b|xn\\-\\-d1alf|xn\\-\\-efvy88h"
                    + "|xn\\-\\-estv75g|xn\\-\\-fhbei|xn\\-\\-fiq228c5hs|xn\\-\\-fiq64b|xn\\-\\-fiqs8s|xn\\-\\-fiqz9s"
                    + "|xn\\-\\-fjq720a|xn\\-\\-flw351e|xn\\-\\-fpcrj9c3d|xn\\-\\-fzc2c9e2c|xn\\-\\-gecrj9c"
                    + "|xn\\-\\-h2brj9c|xn\\-\\-hxt814e|xn\\-\\-i1b6b1a6a2e|xn\\-\\-imr513n|xn\\-\\-io0a7i"
                    + "|xn\\-\\-j1aef|xn\\-\\-j1amh|xn\\-\\-j6w193g|xn\\-\\-kcrx77d1x4a|xn\\-\\-kprw13d|xn\\-\\-kpry57d"
                    + "|xn\\-\\-kput3i|xn\\-\\-l1acc|xn\\-\\-lgbbat1ad8j|xn\\-\\-mgb9awbf|xn\\-\\-mgba3a3ejt"
                    + "|xn\\-\\-mgba3a4f16a|xn\\-\\-mgbaam7a8h|xn\\-\\-mgbab2bd|xn\\-\\-mgbayh7gpa|xn\\-\\-mgbbh1a71e"
                    + "|xn\\-\\-mgbc0a9azcg|xn\\-\\-mgberp4a5d4ar|xn\\-\\-mgbpl2fh|xn\\-\\-mgbtx2b|xn\\-\\-mgbx4cd0ab"
                    + "|xn\\-\\-mk1bu44c|xn\\-\\-mxtq1m|xn\\-\\-ngbc5azd|xn\\-\\-node|xn\\-\\-nqv7f|xn\\-\\-nqv7fs00ema"
                    + "|xn\\-\\-nyqy26a|xn\\-\\-o3cw4h|xn\\-\\-ogbpf8fl|xn\\-\\-p1acf|xn\\-\\-p1ai|xn\\-\\-pgbs0dh"
                    + "|xn\\-\\-pssy2u|xn\\-\\-q9jyb4c|xn\\-\\-qcka1pmc|xn\\-\\-qxam|xn\\-\\-rhqv96g|xn\\-\\-s9brj9c"
                    + "|xn\\-\\-ses554g|xn\\-\\-t60b56a|xn\\-\\-tckwe|xn\\-\\-unup4y|xn\\-\\-vermgensberater\\-ctb"
                    + "|xn\\-\\-vermgensberatung\\-pwb|xn\\-\\-vhquv|xn\\-\\-vuq861b|xn\\-\\-wgbh1c|xn\\-\\-wgbl6a"
                    + "|xn\\-\\-xhq521b|xn\\-\\-xkc2al3hye2a|xn\\-\\-xkc2dl3a5ee0h|xn\\-\\-y9a3aq|xn\\-\\-yfro4i67o"
                    + "|xn\\-\\-ygbi2ammx|xn\\-\\-zfr164b|xperia|xxx|xyz)"
                    + "|(?:yachts|yamaxun|yandex|yodobashi|yoga|yokohama|youtube|y[et])"
                    + "|(?:zara|zip|zone|zuerich|z[amw]))";


    private static final Pattern IP_ADDRESS
            = Pattern.compile(
            "((25[0-5]|2[0-4][0-9]|[0-1][0-9]{2}|[1-9][0-9]|[1-9])\\.(25[0-5]|2[0-4]"
                    + "[0-9]|[0-1][0-9]{2}|[1-9][0-9]|[1-9]|0)\\.(25[0-5]|2[0-4][0-9]|[0-1]"
                    + "[0-9]{2}|[1-9][0-9]|[1-9]|0)\\.(25[0-5]|2[0-4][0-9]|[0-1][0-9]{2}"
                    + "|[1-9][0-9]|[0-9]))");

    /**
     * Valid UCS characters defined in RFC 3987. Excludes space characters.
     */
    private static final String UCS_CHAR = "[" +
            "\u00A0-\uD7FF" +
            "\uF900-\uFDCF" +
            "\uFDF0-\uFFEF" +
            "\uD800\uDC00-\uD83F\uDFFD" +
            "\uD840\uDC00-\uD87F\uDFFD" +
            "\uD880\uDC00-\uD8BF\uDFFD" +
            "\uD8C0\uDC00-\uD8FF\uDFFD" +
            "\uD900\uDC00-\uD93F\uDFFD" +
            "\uD940\uDC00-\uD97F\uDFFD" +
            "\uD980\uDC00-\uD9BF\uDFFD" +
            "\uD9C0\uDC00-\uD9FF\uDFFD" +
            "\uDA00\uDC00-\uDA3F\uDFFD" +
            "\uDA40\uDC00-\uDA7F\uDFFD" +
            "\uDA80\uDC00-\uDABF\uDFFD" +
            "\uDAC0\uDC00-\uDAFF\uDFFD" +
            "\uDB00\uDC00-\uDB3F\uDFFD" +
            "\uDB44\uDC00-\uDB7F\uDFFD" +
            "&&[^\u00A0[\u2000-\u200A]\u2028\u2029\u202F\u3000]]";

    /**
     * Valid characters for IRI label defined in RFC 3987.
     */
    private static final String LABEL_CHAR = "a-zA-Z0-9" + UCS_CHAR;

    /**
     * Valid characters for IRI TLD defined in RFC 3987.
     */
    private static final String TLD_CHAR = "a-zA-Z" + UCS_CHAR;

    /**
     * RFC 1035 Section 2.3.4 limits the labels to a maximum 63 octets.
     */
    private static final String IRI_LABEL =
            "[" + LABEL_CHAR + "](?:[" + LABEL_CHAR + "\\-]{0,61}[" + LABEL_CHAR + "]){0,1}";

    /**
     * RFC 3492 references RFC 1034 and limits Punycode algorithm output to 63 characters.
     */
    private static final String PUNYCODE_TLD = "xn\\-\\-[\\w\\-]{0,58}\\w";

    private static final String TLD = "(" + PUNYCODE_TLD + "|" + "[" + TLD_CHAR + "]{2,63}" + ")";

    private static final String HOST_NAME = "(" + IRI_LABEL + "\\.)+" + TLD;

    private static final String PROTOCOL = "(?i:http|https|rtsp):\\/\\/";

    /* A word boundary or end of input.  This is to stop foo.sure from matching as foo.su */
    private static final String WORD_BOUNDARY = "(?:\\b|$|^)";

    private static final String USER_INFO = "(?:[a-zA-Z0-9\\$\\-\\_\\.\\+\\!\\*\\'\\(\\)"
            + "\\,\\;\\?\\&\\=]|(?:\\%[a-fA-F0-9]{2})){1,64}(?:\\:(?:[a-zA-Z0-9\\$\\-\\_"
            + "\\.\\+\\!\\*\\'\\(\\)\\,\\;\\?\\&\\=]|(?:\\%[a-fA-F0-9]{2})){1,25})?\\@";

    private static final String PORT_NUMBER = "\\:\\d{1,5}";

    private static final String PATH_AND_QUERY = "\\/(?:(?:[" + LABEL_CHAR
            + "\\;\\/\\?\\:\\@\\&\\=\\#\\~"  // plus optional query params
            + "\\-\\.\\+\\!\\*\\'\\(\\)\\,\\_])|(?:\\%[a-fA-F0-9]{2}))*";

    /**
     * Regular expression that matches known TLDs and punycode TLDs
     */
    private static final String STRICT_TLD = "(?:" +
            IANA_TOP_LEVEL_DOMAINS + "|" + PUNYCODE_TLD + ")";

    /**
     * Regular expression that matches host names using {@link #STRICT_TLD}
     */
    private static final String STRICT_HOST_NAME = "(?:(?:" + IRI_LABEL + "\\.)+"
            + STRICT_TLD + ")";

    /**
     * Regular expression that matches domain names using either {@link #STRICT_HOST_NAME} or
     * {@link #IP_ADDRESS}
     */
    private static final Pattern STRICT_DOMAIN_NAME
            = Pattern.compile("(?:" + STRICT_HOST_NAME + "|" + IP_ADDRESS + ")");

    /**
     * Regular expression that matches domain names without a TLD
     */
    private static final String RELAXED_DOMAIN_NAME =
            "(?:" + "(?:" + IRI_LABEL + "(?:\\.(?=\\S))" + "?)+" + "|" + IP_ADDRESS + ")";

    /**
     * Regular expression to match strings that do not start with a supported protocol. The TLDs
     * are expected to be one of the known TLDs.
     */
    private static final String WEB_URL_WITHOUT_PROTOCOL = "("
            + WORD_BOUNDARY
            + "(?<!:\\/\\/)"
            + "("
            + "(?:" + STRICT_DOMAIN_NAME + ")"
            + "(?:" + PORT_NUMBER + ")?"
            + ")"
            + "(?:" + PATH_AND_QUERY + ")?"
            + WORD_BOUNDARY
            + ")";

    /**
     * Regular expression to match strings that start with a supported protocol. Rules for domain
     * names and TLDs are more relaxed. TLDs are optional.
     */
    private static final String WEB_URL_WITH_PROTOCOL = "("
            + WORD_BOUNDARY
            + "(?:"
            + "(?:" + PROTOCOL + "(?:" + USER_INFO + ")?" + ")"
            + "(?:" + RELAXED_DOMAIN_NAME + ")?"
            + "(?:" + PORT_NUMBER + ")?"
            + ")"
            + "(?:" + PATH_AND_QUERY + ")?"
            + WORD_BOUNDARY
            + ")";

    /**
     * Regular expression pattern to match IRIs. If a string starts with http(s):// the expression
     * tries to match the URL structure with a relaxed rule for TLDs. If the string does not start
     * with http(s):// the TLDs are expected to be one of the known TLDs.
     *
     * @hide
     */
    static final Pattern AUTOLINK_WEB_URL = Pattern.compile(
            "(" + WEB_URL_WITH_PROTOCOL + "|" + WEB_URL_WITHOUT_PROTOCOL + ")($|\\s)");

    /**
     * Regular expression for valid email characters. Does not include some of the valid characters
     * defined in RFC5321: #&~!^`{}/=$*?|
     */
    private static final String EMAIL_CHAR = LABEL_CHAR + "\\+\\-_%'";

    /**
     * Regular expression for local part of an email address. RFC5321 section 4.5.3.1.1 limits
     * the local part to be at most 64 octets.
     */
    private static final String EMAIL_ADDRESS_LOCAL_PART =
            "[" + EMAIL_CHAR + "]" + "(?:[" + EMAIL_CHAR + "\\.]{1,62}[" + EMAIL_CHAR + "])?";

    /**
     * Regular expression for the domain part of an email address. RFC5321 section 4.5.3.1.2 limits
     * the domain to be at most 255 octets.
     */
    private static final String EMAIL_ADDRESS_DOMAIN =
            "(?=.{1,255}(?:\\s|$|^))" + HOST_NAME;

    /**
     * Regular expression pattern to match email addresses. It excludes double quoted local parts
     * and the special characters #&~!^`{}/=$*?| that are included in RFC5321.
     *
     * @hide
     */
    public static final Pattern AUTOLINK_EMAIL_ADDRESS = Pattern.compile("(" + WORD_BOUNDARY +
            "(?:" + EMAIL_ADDRESS_LOCAL_PART + "@" + EMAIL_ADDRESS_DOMAIN + ")" +
            WORD_BOUNDARY + ")"
    );


    public static final Pattern SPACED_MATCH_PATTERN = Pattern
            .compile(AUTOLINK_WEB_URL + "|" + AUTOLINK_EMAIL_ADDRESS);

    private MDPattern() {
    }

}

```

#### Adding links

Having defined the regex for matching URLs and emails, the ```addLinks``` method can be explained.

**TextUtils.java**
``` java
public static boolean addLinks(@NonNull Spannable spannable) {
        boolean hasMatches = false;
        final Matcher m = MDPattern.SPACED_MATCH_PATTERN.matcher(spannable);
        while(m.find()) {
            spannable.setSpan(
                    new CleanURLSpan(m.group(0)),
                    m.start(),
                    m.end(),
                    Spanned.SPAN_EXCLUSIVE_EXCLUSIVE
            );
            hasMatches = true;
        }

        return hasMatches;
    }
```

The method uses a matcher from the SPACED_MATCH_PATTERN and sets a ```CleanURLSpan```, a subclass of ```URLSpan``` to be explained later, across the indices of the match.

The ```Spanned.SPAN_EXCLUSIVE_EXCLUSIVE``` flag means that if a character or another span is inserted at either end of the ```CleanURLSpan``` it will not be viewed as part of the ```CleanURLSpan```.

This method completes objective 9.iii.

#### Multiple pattern matching and string escaping

In order not to attempt to display HTML tags in titles, and to replace HTML tags in order to stop Android capturing them, multiple string replace calls must be used.

Rather than calling the replace method multiple times, each incurring a full traversal of the string, multiple matches can be compiled into a single pattern.

**TextUtils.java**
``` java
static Pattern generatePattern(@NonNull Set<String> keys) {
        final StringBuilder b = new StringBuilder();
        int i = 0;
        for(String s : keys) {
            b.append(REGEX_ESCAPE_CHARS.matcher(s).replaceAll("\\\\$0"));
            if(++i != keys.size()) b.append('|');
        }
        return Pattern.compile(b.toString());
    }
```

```generateKeys``` takes a ```Set``` of strings and builds an or separated pattern from the strings.

In order to ensure that the strings themselves are only matched as text, they must be escaped.

Each match key is escaped with the "[\\<\\(\\[\\{\\\\\\^\\-\\=\\$\\!\\|\\]\\}\\)‌​\\?\\*\\+\\.\\>]" pattern, which matches regex control characters and allows them to be replaced with their escaped form.

Once a valid pattern has been generated, a single ```Matcher``` can be used to replace a set of key value pairs from a ```Map```.

**TextUtils.java**
``` java
static String replace(@Nullable String s, Map<String, String> replacements, Pattern pattern) {
        if(s == null) return null;
        final StringBuffer buffer = new StringBuffer();
        final Matcher matcher = pattern.matcher(s);
        while(matcher.find()) {
            matcher.appendReplacement(buffer, replacements.get(matcher.group()));
        }
        matcher.appendTail(buffer);
        return buffer.toString();
    }
```

In each iteration of the while loop ```appendReplacement``` appends all of the text between the previous match and the current one.
Finally, the ```appendTail``` call appends any text after the final match.

#### Background colour selection

When displaying text on a coloured background, as will happen when displaying labels, it is important to ensure that the foreground text is a suitable colour to ensure legibility.

In order to determine the background colour, the relative luminance is used.

For linear RGB values, the relative luminance is given by Y = 0.2126R + 0.7152G + 0.0722B. This formula shows that green contributes most to the human perception of luminosity, and blue the least.

In order to use this formula, an inverse gamma function must be applied to the RGB values to account for the non-linear relationship between the intensity of the primary colours and the actual values stored.

The numeric approximation to the function is given below

![Inverse gamma function](http://imgur.com/tyOWS3j.png)

Each RGB value is then used in the relative luminance formula to determine whether to use a light or dark text colour.

**TextUtils.java**
``` java
public static int getTextColorForBackground(int bg) {
        double r = Color.red(bg) / 255d;
        if(r <= 0.04045) {
            r = r / 12.92;
        } else {
            r = Math.pow((r + 0.055) / 1.055, 2.4);
        }
        double g = Color.green(bg) / 255d;
        if(g <= 0.04045) {
            g = g / 12.92;
        } else {
            g = Math.pow((g + 0.055) / 1.055, 2.4);
        }
        double b = Color.blue(bg) / 255d;
        if(b <= 0.04045) {
            b = b / 12.92;
        } else {
            b = Math.pow((b + 0.055) / 1.055, 2.4);
        }
        return (0.2126 * r + 0.7152 * g + 0.0722 * b) > 0.35 ? Color.BLACK : Color.WHITE;
    }
```

This method completes objective 9.ii.h.

#### Other utility methods

**TextUtils.java**
``` java
package com.tpb.mdtext;

import android.graphics.Color;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.text.Spannable;
import android.text.Spanned;

import com.tpb.mdtext.views.spans.CleanURLSpan;

import java.util.Map;
import java.util.Set;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

/**
 * Created by theo on 21/03/17.
 */

public class TextUtils {

    private TextUtils() {}

    private static final Pattern REGEX_ESCAPE_CHARS =
            Pattern.compile("[\\<\\(\\[\\{\\\\\\^\\-\\=\\$\\!\\|\\]\\}\\)‌​\\?\\*\\+\\.\\>]");

    static String replace(@Nullable String s, Map<String, String> replacements) {
        return replace(s, replacements, generatePattern(replacements.keySet()));
    }

    static String replace(@Nullable String s, Map<String, String> replacements, Pattern pattern) {
        if(s == null) return null;
        final StringBuffer buffer = new StringBuffer();
        final Matcher matcher = pattern.matcher(s);
        while(matcher.find()) {
            matcher.appendReplacement(buffer, replacements.get(matcher.group()));
        }
        matcher.appendTail(buffer);
        return buffer.toString();
    }

    static Pattern generatePattern(@NonNull Set<String> keys) {
        final StringBuilder b = new StringBuilder();
        int i = 0;
        for(String s : keys) {
            b.append(REGEX_ESCAPE_CHARS.matcher(s).replaceAll("\\\\$0"));
            if(++i != keys.size()) b.append('|');
        }
        return Pattern.compile(b.toString());
    }

    public static boolean addLinks(@NonNull Spannable spannable) {
        boolean hasMatches = false;
        final Matcher m = MDPattern.SPACED_MATCH_PATTERN.matcher(spannable);
        while(m.find()) {
            spannable.setSpan(
                    new CleanURLSpan(m.group(0)),
                    m.start(),
                    m.end(),
                    Spanned.SPAN_EXCLUSIVE_EXCLUSIVE
            );
            hasMatches = true;
        }

        return hasMatches;
    }

    public static int getTextColorForBackground(int bg) {
        double r = Color.red(bg) / 255d;
        if(r <= 0.04045) {
            r = r / 12.92;
        } else {
            r = Math.pow((r + 0.055) / 1.055, 2.4);
        }
        double g = Color.green(bg) / 255d;
        if(g <= 0.04045) {
            g = g / 12.92;
        } else {
            g = Math.pow((g + 0.055) / 1.055, 2.4);
        }
        double b = Color.blue(bg) / 255d;
        if(b <= 0.04045) {
            b = b / 12.92;
        } else {
            b = Math.pow((b + 0.055) / 1.055, 2.4);
        }
        return (0.2126 * r + 0.7152 * g + 0.0722 * b) > 0.35 ? Color.BLACK : Color.WHITE;
    }

    public static boolean isValidURL(String possible) {
        return MDPattern.AUTOLINK_WEB_URL.matcher(possible).matches();
    }

    public static String capitaliseFirst(String s) {
        if(s == null || s.length() == 0) return s;
        return s.substring(0, 1).toUpperCase() + s.substring(1).toLowerCase();
    }

    /**
     * Counts the instances of a string within another string
     *
     * @param s The string to search
     * @param sub The string to count instances of
     * @return The number of instances of s2 in s1
     */
    public static int instancesOf(@NonNull String s, @NonNull String sub) {
        if(s.length() == 0 || sub.length() == 0 || sub.length() > s.length()) return 0;
        int last = 0;
        int count = 0;
        while(last != -1) {
            last = s.indexOf(sub, last);
            if(last != -1) {
                count++;
                last++;
            }
        }
        return count;
    }

    public static boolean isInteger(String s) {
        return isInteger(s, 10);
    }

    public static boolean isInteger(String s, int radix) {
        if(s.isEmpty()) return false;
        for(int i = 0; i < s.length(); i++) {
            if(i == 0 && s.charAt(i) == '-') {
                if(s.length() == 1) return false;
                else continue;
            }
            if(Character.digit(s.charAt(i), radix) < 0) return false;
        }
        return true;
    }

}

```

The four other methods in ```TextUtils``` are ```isValidUrl```, ```capitaliseFirst```, ```instancesOf```, and ```isInteger```.

```isValidURL``` matches a string against the AUTOLINK_WEB_URL pattern.

```capitaliseFirst``` capitalises the first character of a string.

```instancesOf``` determines the number of instances of a substring in another.
It first performs a check for whether either of the strings are empty, or if the substring is larger than the string to be searched.
Otherwise, ```instancesOf``` searches the string for the substring, each time searching from the index after the previous position found.

```isInteger``` is used to determine whether a ```String``` is a an integer in a given base.
It first checks if the string is empty.
If the string is not empty the method iterates through each character in the string.
If the first character of the string is a minus, "-", and the string is longer than one character, the string may still be valid.
Otherwise, each iteration checks the character for its numeric value in the given base with the ```Character``` class, which will return -1 if the character has no numeric value in the given base.

<div style="page-break-after: always;"></div>

### Displaying README files

While most Markdown strings will be relatively short and simple, many README files are quite complex as they are used to explain the repository purpose and how one might go about using it.

The GitHub mobile website displays READMEs inline with code made horizontally scrollable.

|  |  |
| --- | --- | 
| ![GitHub site README](http://imgur.com/cYWejcn.png) | ![Site README Code block](http://imgur.com/kqo9lS1.png) |

In order to implement exactly the same styling, the same CSS as used in the GitHub website can be used. A dark theme is also used, with the colour scheme changed.

In order to ensure that the Markdown is rendered in exactly the same manner, the GitHub Markdown API is used.
This API takes a Markdown string and renders it to HTML. It also takes an optional context argument, specifying the repository which the Markdown is being rendered for.

#### JavaScript interface methods

In order to interface between Java code used in the application and the JavaScript used in a ```WebView``` JavaScriptInterface methods are used. 

These methods are annotated with the @JavascriptInterface annotations, which makes them accessible from JavaScript run in the ```WebView``` through an interface set on the ```WebView```.

In order to call JavaScript functions from the Java code the ```evaulateJavascript``` function is called, which evaulates a string of JavaScript.

**MarkdownWebView.java**
``` java
private void init() {
        setWebViewClient(new WebViewClient() {
            public void onPageFinished(WebView view, String url) {
                evaluateJavascript(previewText, null);

            }
        });
        addJavascriptInterface(this, "TouchIntercept");
        if(darkTheme) {
            loadUrl("file:///android_asset/html/md_preview_dark.html");
        } else {
            loadUrl("file:///android_asset/html/md_preview.html");
        }

        getSettings().setJavaScriptEnabled(true);
        getSettings().setAllowUniversalAccessFromFileURLs(true);

        if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
            getSettings().setMixedContentMode(WebSettings.MIXED_CONTENT_ALWAYS_ALLOW);
        }
    }
```

The ```init``` method sets a custom ```WebView``` client which evalutates the Javascript string used to load the rendered Markdown into the WebView once the page has finished loading.
It also adds a Javascript interface called "TouchIntercept" to the ```WebView```.

The two files which can be loaded into the ```WebView``` are md_preview and md_preview_dark.

``` HTML
<!doctype html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <script src="js/highlight.pack.js"></script>
    <script src="js/md_preview.js"></script>

    <link rel="stylesheet" href="css/github.css"/>

    <style>
            img { max-width: 100%; }
    </style>

</head>
<body>
    <div class="container" id="preview"></div>
</body>
</html>
```

Each specifies the stylesheet to use, and loads both the Javascript used to display the Markdown and the HighlightJS library used by GitHub to highlight code.

The body contains a single div element which is used to display the Markdown once it has been loaded.

#### Touch interception

There are two problems with the use of the ```WebView``` to display the README, both relating to its use within a ```SwipeRefreshLayout``` which is itself within a ```ViewPager``` with an ```AppBar``` layout
behaviour.

##### Vertical scrolling

Each of the ```Fragments``` used throughout the app uses a ```SwipeRefreshLayout``` in order to allow the user to drag down from the top of a scrolling layout in order to refresh its contents.
Each of the scrolling children inside the ```SwipeRefreshLayout``` has a layout behaviour specified, which should result in the ```ToolBar```, and ```TabLayout``` if applicable, being hidden as the user scrolls
down.
This scrolling behaviour only occurs when the scrolling child implements ```NestedScrollingChild``` which the ```WebView``` does not.

Most of the methods in ```NestedScrollingChild``` are forward through to a ```NestedScrollingChildHelper``` which deals with the interpretation of the touch events.

**MarkdownWebView.java**
``` java
@Override
public boolean onTouchEvent(MotionEvent ev) {
    if(mInterceptTouchEvent && getParent() != null) {
        getParent().requestDisallowInterceptTouchEvent(mInterceptTouchEvent);
        return super.onTouchEvent(ev);
    }

    boolean rv = false;

    MotionEvent event = MotionEvent.obtain(ev);
    final int action = MotionEventCompat.getActionMasked(event);
    if(action == MotionEvent.ACTION_DOWN) {
        mNestedOffsetY = 0;
    }
    int eventY = (int) event.getY();
    event.offsetLocation(0, mNestedOffsetY);
    switch(action) {
        case MotionEvent.ACTION_MOVE:
            int deltaY = mLastY - eventY;
            // NestedPreScroll
            if(dispatchNestedPreScroll(0, deltaY, mScrollConsumed, mScrollOffset)) {
                deltaY -= mScrollConsumed[1];
                mLastY = eventY - mScrollOffset[1];
                event.offsetLocation(0, -mScrollOffset[1]);
                mNestedOffsetY += mScrollOffset[1];
            }
            rv = super.onTouchEvent(event);

            // NestedScroll
            if(dispatchNestedScroll(0, mScrollOffset[1], 0, deltaY, mScrollOffset)) {
                event.offsetLocation(0, mScrollOffset[1]);
                mNestedOffsetY += mScrollOffset[1];
                mLastY -= mScrollOffset[1];
            }
            break;
        case MotionEvent.ACTION_DOWN:
            rv = super.onTouchEvent(event);
            mLastY = eventY;
            // start NestedScroll
            startNestedScroll(ViewCompat.SCROLL_AXIS_VERTICAL);
            break;
        case MotionEvent.ACTION_UP:
        case MotionEvent.ACTION_CANCEL:
            rv = super.onTouchEvent(event);
            // end NestedScroll
            stopNestedScroll();
            break;
    }
    return rv;
}

// Nested Scroll implements
@Override
public void setNestedScrollingEnabled(boolean enabled) {
    mChildHelper.setNestedScrollingEnabled(enabled);
}

@Override
public boolean isNestedScrollingEnabled() {
    return mChildHelper.isNestedScrollingEnabled();
}

@Override
public boolean startNestedScroll(int axes) {
    return mChildHelper.startNestedScroll(axes);
}

@Override
public void stopNestedScroll() {
    mChildHelper.stopNestedScroll();
}

@Override
public boolean hasNestedScrollingParent() {
    return mChildHelper.hasNestedScrollingParent();
}

@Override
public boolean dispatchNestedScroll(int dxConsumed, int dyConsumed, int dxUnconsumed, int dyUnconsumed,
                                    int[] offsetInWindow) {
    return mChildHelper.dispatchNestedScroll(dxConsumed, dyConsumed, dxUnconsumed, dyUnconsumed,
            offsetInWindow
    );
}

@Override
public boolean dispatchNestedPreScroll(int dx, int dy, int[] consumed, int[] offsetInWindow) {
    return mChildHelper.dispatchNestedPreScroll(dx, dy, consumed, offsetInWindow);
}

@Override
public boolean dispatchNestedFling(float velocityX, float velocityY, boolean consumed) {
    return mChildHelper.dispatchNestedFling(velocityX, velocityY, consumed);
}

@Override
public boolean dispatchNestedPreFling(float velocityX, float velocityY) {
    return mChildHelper.dispatchNestedPreFling(velocityX, velocityY);
}
```

The ```onTouchEvent``` forwards each ```MotionEvent``` through the helper in order to receive the scrolling offsets required to scroll multiple views in sync.


##### Horizontal scrolling

The second problem occurs when displaying code blocks.
When a code block overflows horizontally, as happens often on vertical mobile screens, it is expected to scroll horizontally.
This causes a problem when the ```WebView``` is displayed in a ```ViewPager``` because the horizontal touch movements are intercepted by the ```ViewPager``` and result in the entire ```Fragment``` being scrolled
horizontally.

These events can be can be overridden by calling ```requestDisallowInterceptTouchEvent``` on the ```WebView```'s parent, however we must only call this method for touch events which are on the code blocks, 
otherwise all events will be intercepted by the ```WebView``` and the user will not be able to exit the ```Fragment```.

This is achieved by adding touch listeners in Javascript and then notifying the ```WebView``` through Javascript interface methods

**md_preview.js**
``` js
function touchStart(event) {
    TouchIntercept.beginTouchIntercept();
}

function touchEnd(event) {
    TouchIntercept.endTouchIntercept();
}

function preview(md_html) {
    if(md_html == "") {
        return false;
    }
    document.getElementById("preview").innerHTML = md_html.replace(/\\n/g, "\n")
    var codes = document.getElementsByClassName('code');
    for(var i = 0; i < codes.length; i++) {
        codes[i].style.display = 'block';
        codes[i].style.wordWrap = 'normal';
        codes[i].style.overflowX = 'scroll';
        if(!codes[i].innerHTML.includes("license")) {
            hljs.highlightBlock(codes[i]);
        }
        codes[i].addEventListener("touchstart", touchStart, false);
        codes[i].addEventListener("touchend", touchEnd, false)
    }

    var pres = document.getElementsByTagName('pre');
    for(var i = 0; i < pres.length; i++) {
        pres[i].addEventListener("touchstart", touchStart, false);
        pres[i].addEventListener("touchend", touchEnd, false)
    }
    var tables = document.getElementsByTagName('table');
    for(var i = 0; i < tables.length; i++) {
        tables[i].addEventListener("touchstart", touchStart, false);
        tables[i].addEventListener("touchend", touchEnd, false)
    }
}
```

The Javascript above first sets the inner HTML of the preview div mentioned earlier, and then searches for each of the elements which scroll.

The style on each code element is set such that it scrolls on any overflow in x, and if it is not displaying a license it is highlighted.

All code, pre, and table elements are assigned event listeners for the "touchstart" and "touchend" events which call the Java methods in the ```WebView```.

The two Java methods are ```beginTouchIntercept``` and ```endTouchIntercept``` which set the mInterceptTouchEvent flag.

**MarkdownWebView.java**
``` java
@JavascriptInterface
public void beginTouchIntercept() {
    mInterceptTouchEvent = true;
}

@JavascriptInterface
public void endTouchIntercept() {
    mInterceptTouchEvent = false;
}

```

In the ```onTouchEvent``` method shown in the section above, the flag is checked:

``` java
if(mInterceptTouchEvent && getParent() != null) {
    getParent().requestDisallowInterceptTouchEvent(mInterceptTouchEvent);
    return super.onTouchEvent(ev);
}
```

and the event is intercepted if the user is touching a code, pre, or table element.

<div style="page-break-after: always;"></div>

### Displaying short Markdown sections

In order to display GitHub Markdown formatted text in the Android ```TextView```, custom spans are required.

#### CleanURLSpan

This span type was reference earlier.
It is used to display links to web addresses and email addresses.

**CleanURLSpan.java**
``` java
package com.tpb.mdtext.views.spans;

import android.content.Intent;
import android.graphics.Typeface;
import android.net.Uri;
import android.os.Parcel;
import android.support.annotation.Nullable;
import android.text.TextPaint;
import android.text.style.URLSpan;
import android.view.View;

import com.tpb.mdtext.MDPattern;
import com.tpb.mdtext.handlers.LinkClickHandler;

/**
 * Created by theo on 27/02/17.
 */

public class CleanURLSpan extends URLSpan {
    private LinkClickHandler mHandler;

    public CleanURLSpan(String url) {
        super(ensureValidURL(url));
    }

    public CleanURLSpan(String url, LinkClickHandler handler) {
        super(ensureValidURL(url));
        mHandler = handler;
    }

    @Override
    public void onClick(View widget) {
        if(mHandler == null) {
            final Intent i = new Intent(Intent.ACTION_VIEW);
            i.setData(Uri.parse(getURL()));
            widget.getContext().startActivity(i);
        } else {
            mHandler.onClick(getURL());
        }
    }

    private static String ensureValidURL(@Nullable String url) {
        if(url == null) return null;

        if(MDPattern.AUTOLINK_EMAIL_ADDRESS.matcher(url).matches()) {
            return "mailto:" + url;
        } else if(!url.startsWith("https://") && !url.startsWith("http://")) {
            return "http://" + url;
        }
        return url;
    }

    @Override
    public void updateDrawState(TextPaint ds) {
        super.updateDrawState(ds);
        // Links are bold without underline
        ds.setUnderlineText(false);
        ds.setTypeface(Typeface.DEFAULT_BOLD);
    }

    @Override
    public int describeContents() {
        return 0;
    }

    @Override
    public void writeToParcel(Parcel dest, int flags) {
    }

    CleanURLSpan(Parcel in) {
        super(in);
    }

    public static final Creator<CleanURLSpan> CREATOR = new Creator<CleanURLSpan>() {
        @Override
        public CleanURLSpan createFromParcel(Parcel source) {
            return new CleanURLSpan(source);
        }

        @Override
        public CleanURLSpan[] newArray(int size) {
            return new CleanURLSpan[size];
        }
    };
}

```

The ```CleanURLSpan``` uses the ```LinkClickHandler``` interface, which provides an onClick method for a URL

 **LinkClickHandler.java**
``` java
package com.tpb.mdtext.handlers;

/**
 * Created by theo on 27/02/17.
 */

public interface LinkClickHandler {

    void onClick(String url);

}

```

This interface is used to allow capturing all link clicks which occur in a ```TextView```.

The ```ensureValidURL``` method checks if the link is an email address, formatting it accordingly. If the link is a web address, the correct protocl is prefixed.

The ```CleanURLSpan``` overrides ```updateDrawState``` to remove the underline usually displayed on links, and to use a bold typeface.

#### HorizontalRuleSpan

The ```HorizontalRuleSpan``` solves the problem of drawing a line across the ```TextView```.

As trivial as this problem sounds, it cannot be achieved with any string of text.
While a line could be drawn more easily with a box drawing character, specifically U+2500 which draws lines with no gap ──── or the thicker U+2501 ━━━━, these characters would not span
the full width of the ```TextView``` without guesswork, approximations, and some luck.

Once a layout pass has been completed, the number of characters per line of a ```TextView``` can be calculated by repeatedly measuring a string with the ```TextView```'s ```Paint```.

``` java
private static boolean isTextTooLong(TextView tv, String text) {
    final float textWidth = tv.getPaint().measureText(text);
    return (textWidth >= tv.getMeasuredWidth ());
}

private static boolean findCharactersPerLine(TextView tv) {
    String s = "";
    while(!isTextTooLong(tv, s)) {
        s += " ";
    }
    return s.length()
}

```
This method has numerous problems:

First, it relies on the ```TextView``` using a monospace font.

Second, it requires a layout pass to have been completed.
This means that in order to display the horizontal rule, the ```TextView``` would have to:
1. Check for horizontal spans when its text is set
2. Add a listener for its layout call 
3. Within this listener, calculate the maximum number of characters which can be displayed per line
4. Replace each horizontal rule placeholder with a new string of the correct length
5. Redraw the entire ```TextView```, and ensure that there isn't an infinite loop of redrawing the ```TextView```

Clearly this is not a reasonable way to display horizontal rules in a ```TextView```.

Instead, a ```ReplacementSpan``` can be used to draw a line across the ```Canvas```.

**HorizontalRuleSpan.java**
``` java
package com.tpb.mdtext.views.spans;

import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.graphics.RectF;
import android.support.annotation.NonNull;
import android.text.style.ReplacementSpan;

/**
 * Created by theo on 02/03/17.
 */

public class HorizontalRuleSpan extends ReplacementSpan {

    private RectF mRectF;

    public HorizontalRuleSpan() {
        mRectF = new RectF();
    }

    @Override
    public int getSize(@NonNull Paint paint, CharSequence text, int start, int end, Paint.FontMetricsInt fm) {
        return 0;
    }

    @Override
    public void draw(@NonNull Canvas canvas, CharSequence text, int start, int end, float x, int top, int y, int bottom, @NonNull Paint paint) {
        final int mid = (top + bottom) / 2;
        final int quarter = (bottom - top) / 4;
        paint.setColor(Color.GRAY);
        mRectF.left = x;
        mRectF.top = mid - quarter;
        mRectF.right = x + canvas.getWidth();
        mRectF.bottom = mid + quarter;
        canvas.drawRect(mRectF, paint);
        final int eighth = quarter / 2;
        paint.setColor(Color.LTGRAY);
        mRectF.left += eighth;
        mRectF.right -= eighth;
        mRectF.top += eighth;
        mRectF.bottom -= eighth;
        canvas.drawRect(mRectF, paint);
    }
}

```

The draw method in ```HorizontalRuleSpan``` draws a bordered rectangle by drawing two rectangles.

First, it calculates the mid-point of the space available to it (a single line).
Second, it calculates one quarter of the height of the space available to it.
Third, it assigns the given x position, and the calculated mid-point and quarter height to a ```RectF``` object in order to draw a rectangle across the canvas between the upper and lower
quartiles of the line. This makes the total area covered half of the available line.

The second rectangle to be drawn fills half of the vertical space within the first rectangle.
One eighth of the height is calculated as half of the quarter, and the bounds of the rectangle are changed to give the new rectangle a border of this size.
The ```Paint``` colour is then changed to light grey and the new rectangle is drawn.

![HorizontalRuleSpan](http://imgur.com/JAzWw7o.png)

The only caveat to this method is that if the span it must be ensured that there is an empty line for the ```HorizontalRuleSpan``` to fill.

This span completes objective 9.ii.b

#### QuoteSpan

Android already includes a span for quotes, however it only draws a line to the start of the text and is not configurable, instead using blue (0, 255, 0).

**QuoteSpan.java**
``` java
package com.tpb.mdtext.views.spans;

import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.text.Layout;
import android.text.TextPaint;
import android.text.style.CharacterStyle;
import android.text.style.LeadingMarginSpan;

/**
 * Created by theo on 20/03/17.
 */

public class QuoteSpan extends CharacterStyle implements LeadingMarginSpan {
    private static final int STRIPE_WIDTH = 4;
    private static final int GAP_WIDTH = 8;
    private final int mColor;

    public QuoteSpan() {
        super();
        mColor = Color.WHITE;
    }

    public QuoteSpan(int color) {
        super();
        mColor = color;
    }

    public int describeContents() {
        return 0;
    }

    @Override
    public void updateDrawState(TextPaint tp) {
        tp.setAlpha((tp.getColor() & 0x00FFFFFF) > 0x800000 ? 179 : 138);
    }

    @Override
    public int getLeadingMargin(boolean first) {
        return STRIPE_WIDTH + GAP_WIDTH;
    }

    @Override
    public void drawLeadingMargin(Canvas c, Paint p, int x, int dir,
                                  int top, int baseline, int bottom,
                                  CharSequence text, int start, int end,
                                  boolean first, Layout layout) {
        final Paint.Style style = p.getStyle();
        final int color = p.getColor();
        p.setStyle(Paint.Style.FILL);
        p.setColor(mColor);
        c.drawRect(x, top, x + dir * STRIPE_WIDTH, bottom, p);
        p.setStyle(style);
        p.setColor(color);
    }
}
```

```QuoteSpan``` extends ```CharacterStyle```, allowing it to set modify the ```TextPaint```, as well as implementing ```LeadingMarginSpan``` in order to draw the quote line.

The ```QuoteSpan``` follows the material guidelines for secondary text, which specify using an opacity of 54% for secondary text using a dark text on light backgrounds, and using 70%
opacity for secondary text using a white text on light backgrounds.

Each Android colour is stored as an integer with alpha, red, green, and blue occupying each byte.
The alpha value is stripped from the colour by and-ing it with 00FFFFFF, and it is then compared to the middle colour value to approximate whether it is a light or dark colour.

In ```drawLeadingMarginSpan``` the original ```Style``` and colour are saved, and a rectangle is drawn across the whole vertical space of the line, and across 4 pixels horizontally. 
The original style and colour are then saved.

<div style="page-break-after: always;"></div>

#### ListNumberSpan

```ListNumberSpan``` implements ```LeadingMarginSpan``` and is used to draw the keys in an ordered list.

HTML ordered lists can specify four types of keys.
- Numbers, indexed from 1
- Letters, indexed from a
- Capital letters, indexed from A
- Roman numerals, indexed from i
- Capital Roman numerals, indexed from I

The ```ListNumberSpan``` needs to specify the margin for for list indentation, as well as drawing the list item number.

**ListNumberSpan.java**
``` java
package com.tpb.mdtext.views.spans;

import android.graphics.Canvas;
import android.graphics.Paint;
import android.support.annotation.NonNull;
import android.text.Layout;
import android.text.Spanned;
import android.text.TextPaint;
import android.text.style.LeadingMarginSpan;

import com.tpb.mdtext.TextUtils;

import java.util.TreeMap;

/**
 * Created by theo on 02/03/17.
 */

public class ListNumberSpan implements LeadingMarginSpan {
    private final String mNumber;
    private final int mTextWidth;

    public ListNumberSpan(TextPaint textPaint, int number, ListType type) {
        mNumber = ListType.getFormattedNumber(number + type.start, type).concat(". ");
        mTextWidth = (int) textPaint.measureText(mNumber);
    }

    @Override
    public int getLeadingMargin(boolean first) {
        return mTextWidth;
    }

    @Override
    public void drawLeadingMargin(Canvas c, Paint p, int x, int dir, int top, int baseline,
                                  int bottom, CharSequence text, int start, int end,
                                  boolean first, Layout l) {
        //Check if we are at the correct depth to draw text rather than just spacing
        if(text instanceof Spanned) {
            if(((Spanned) text).getSpanStart(this) == start) {
                c.drawText(mNumber, x, baseline, p);
            }
        }
    }

    public enum ListType {

        NUMBER,
        LETTER,
        LETTER_CAP,
        ROMAN,
        ROMAN_CAP;

        int start = 0;

        public static ListType fromString(@NonNull String val) {
            if(val.isEmpty()) return NUMBER;
            if(TextUtils.isInteger(val)) {
                final ListType num = NUMBER;
                num.start = Integer.parseInt(val) - 1;
                return num;
            } else {
                switch(val.charAt(0)) {
                    case 'a': return LETTER;
                    case 'A': return LETTER_CAP;
                    case 'i': return ROMAN;
                    case 'I': return ROMAN_CAP;
                    default: return NUMBER;
                }
            }
        }

        public static String getFormattedNumber(int num, ListType type) {
            switch(type) {
                case LETTER:
                    return getLetter(num);
                case LETTER_CAP:
                    return getLetter(num).toUpperCase();
                case ROMAN:
                    return getRoman(num);
                case ROMAN_CAP:
                    return getRoman(num).toUpperCase();
                default:
                    return Integer.toString(num);
            }
        }

        private static String getLetter(int num) {
            final StringBuilder builder = new StringBuilder();
            while(num-- > 0) { //1 = a, not 0 = a
                final int rmdr = num % 26;
                builder.append((char) (rmdr + 'a'));
                num = (num - rmdr) / 26;
            }
            return builder.reverse().toString();
        }

        private static TreeMap<Integer, String> map = new TreeMap<>();

        static {
            map.put(1000, "m");
            map.put(900, "cm");
            map.put(500, "d");
            map.put(400, "cd");
            map.put(100, "c");
            map.put(90, "xc");
            map.put(50, "l");
            map.put(40, "xl");
            map.put(10, "x");
            map.put(9, "xi");
            map.put(5, "v");
            map.put(4, "iv");
            map.put(1, "i");
        }

        private static String getRoman(int num) {
            final int l = map.floorKey(num);
            if(l == num) {
                return map.get(num);
            }
            return map.get(l) + getRoman(num - l);
        }

    }

}

```

The ```ListNumberSpan``` constructor takes a ```TextPaint```, which is used to calculate the width of the list item text, the number to display, and the ```ListType``` enum.

The string mNumber is calculated with ```getFormattedNumber``` and concatenated with ". ".
The width of mNumber is then calculated.

In ```getLeadingMargin``` the text width calculated in the constructor is returned.

```drawLeadingMargin``` has to check if the start position is the start position of the span, in order to ensure that only the span at the deepest indentation level draws its text.
First, it is checked that the text is an instance of ```Spanned```, as the position cannot be found otherwise.
If the text is an instance, the span start position returned from the cast ```Spanned``` is checked against the start position passed to ```drawLeadingMargin```.
If these values are the same, the number text is drawn with the parameters passed to ```drawLeadingMargin```.

##### Calculating list number formats

The ```fromString``` method in ```ListType``` is used to convert the type parameter in an ordered list tag to a ```ListType``` enum.
If the string is empty, NUMBER is returned as the default type.
If the string is an integer, the integer value is parsed to become the starting index for the list, and NUMBER is returned.
Otherwise, the method switches on the first character in the string:
- If it is 'a', LETTER is returned
- If it is 'A' LETTER_CAP is returned
- If it is 'i' ROMAN is returned
- If it is 'I' ROMAN_CAP is returned
- If it is none of the above, NUMBER is returned

Once the ```ListType``` has been calculated, it needs to be formatted into a string.
This is done in ```getFormattedNumber``` which takes an integer and a ```ListType```.

If the type is LETTER, ```getLetter``` is returned.
``` java
private static String getLetter(int num) {
    final StringBuilder builder = new StringBuilder();
    while(num-- > 0) { //1 = a, not 0 = a
        final int rmdr = num % 26;
        builder.append((char) (rmdr + 'a'));
        num = (num - rmdr) / 26;
    }
    return builder.reverse().toString();
}
```
This method converts an integer value to a base 26 string.
It does this by computing the remainder of dividing the number by 26, and offsetting this by the numeric value of the character \'a' (97).
The remainder is then subtracted from the number, and it is divided by 26.
This process repeats until the reversed form of the string has been generated.
The ```StringBuilder``` is then reversed and its value is returned.

If the type is LETTER_CAP, ```getLetter``` is called, and its return value shifted to uppercase.

If the type is ROMAN, ```getRoman``` is called.

```getRoman``` uses a ```TreeMap``` to store the unique roman numeral values of different integers.
The ```TreeMap``` maintains the order of the mapped pairs.

The greatest key less than or equal to the given key is found with ```floorKey```.
If this value is the same as the number, there is a direct match and the string is returned.
Otherwise, the string at the lowest key is concatenated with a recursive call for the value of the number minus the lowest key value.


``` java
 private static TreeMap<Integer, String> map = new TreeMap<>();

static {
    map.put(1000, "m");
    map.put(900, "cm");
    map.put(500, "d");
    map.put(400, "cd");
    map.put(100, "c");
    map.put(90, "xc");
    map.put(50, "l");
    map.put(40, "xl");
    map.put(10, "x");
    map.put(9, "xi");
    map.put(5, "v");
    map.put(4, "iv");
    map.put(1, "i");
}

private static String getRoman(int num) {
    final int l = map.floorKey(num);
    if(l == num) {
        return map.get(num);
    }
    return map.get(l) + getRoman(num - l);
}
```

Example:

Converting 46 to roman numerals.

The first call calls map.floorKey(46), which returns 40.
As 40 != 46, the function returns map.get(40) + getRoman(46 - 40).
The recursive call getRoman(6) calls map.floorKey(6), which returns 5.
As 5 != 6, the function returns map.get(5) + getRoman(6 - 5).
The recursive call getRoman(1) calls map.floorKey(1) which returns 1.
As 1 == 1, the function returns map.get(1).

As there are no further recursive calls, the function returns map.get(40) + map.get(5) + map.get(1).
This evaluates to "xl" + "v" + "i", which is correct.

This span completes objetive 9.iv.b

#### RoundedBackgroundEndSpan

GitHub labels are shown with coloured backgrounds which have rounded corners.

![GitHub labels](http://imgur.com/JTcPrjA.png)

The background colour can be achieved using a ```BackgroundColorSpan```, which sets the background colour of the ```TextPaint```.
However, this draws a rectangular block of colour behind the segment of text, and doesn't allow drawing any other shape.

The problem can be solved by drawing the spans separately, adding a ```ReplacementSpan``` at each end of the ```BackgroundColorSpan``` which draws the rounded corners.


**RoundedBackgroundEndSpan.java**
``` java
package com.tpb.mdtext.views.spans;

import android.graphics.Canvas;
import android.graphics.Paint;
import android.graphics.RectF;
import android.support.annotation.NonNull;
import android.text.style.ReplacementSpan;

/**
 * Created by theo on 28/03/17.
 */

public class RoundedBackgroundEndSpan extends ReplacementSpan {

    private int mCharacterWidth;
    private final int mBgColor;
    private final boolean mIsEndSpan;

    public RoundedBackgroundEndSpan(int bgColor, boolean isEndSpan) {
        mBgColor = bgColor;
        mIsEndSpan = isEndSpan;
    }

    @Override
    public int getSize(@NonNull Paint paint, CharSequence text, int start, int end, Paint.FontMetricsInt fm) {
        mCharacterWidth = (int) paint.measureText("t");
        return mCharacterWidth;
    }

    @Override
    public void draw(@NonNull Canvas canvas, CharSequence text, int start, int end, float x, int top, int y, int bottom, @NonNull Paint paint) {
        RectF rect =  new RectF(x, top, x + mCharacterWidth, bottom);
        paint.setColor(mBgColor);
        canvas.drawRoundRect(rect, paint.getTextSize() / 6, paint.getTextSize() / 6, paint);
        if(mIsEndSpan) {
            rect = new RectF(x, top, (x + x + mCharacterWidth) / 2, bottom);
        } else {
            rect = new RectF((x + x + mCharacterWidth) / 2, top, x + mCharacterWidth, bottom);
        }
        canvas.drawRect(rect, paint);
    }
}

```

The ```RoundedBackgroundEndSpan``` will either be drawn at the start or the end of a ```BackgroundColorSpan```. The direction in which it draws is controlled by mIsEndSpan, which is passed to the constructor along with the background colour to use.

The ```RoundedBackgroundEndSpan``` uses one character of space, which is measured in ```getSize```.

The drawing takes place with two calls.
The CSS on the GitHub website uses a corner size of one sixth the text size, which can be duplicated when painting the rounded rectangle.
The rounded rectangle is drawn across the measured character width.
At this point, the rounded section of the end span has been drawn, but it is rounded on both sides which would leave a gap between the rounded rectangle and the text.

The next draw call is dependent on whether the span is at the start or end of the ```BackgroundColorSpan```.
If the span is at the start of the ```BackgroundColorSpan```, it needs to fill in the space after itself and before the ```BackgroundColorSpan``` whereas if it is after the 
```BackgroundColorSpan``` it needs to fill in the space after the ```BackgroundColorSpan``` and before itself.

If the span is at the end of the ```BackgroundColorSpan``` the rectangle is created from the given x position (which is the end of the ```BackgroundColorSpan```), to the x position 
halfway between the start and end of the span.
If the span is at the start of the ```BackgroundColorSpan``` the rectangle is created from the halfway position to the end of the span.

The rectangle is then drawn.

#### InlineCodeSpan

The ```InlineCodeSpan``` is used to draw short sections of code within the ```TextView```.

**InlineCodeSpan.java**
``` java
package com.tpb.mdtext.views.spans;

import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.graphics.Typeface;
import android.support.annotation.IntRange;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.text.TextPaint;
import android.text.style.ReplacementSpan;

/**
 * Created by theo on 22/03/17.
 */

public class InlineCodeSpan extends ReplacementSpan {
    private final float mTextSize;

    private float mPadding;


    public InlineCodeSpan(float textSize) {
        mTextSize = textSize;
    }

    @Override
    public void updateDrawState(TextPaint tp) {
        tp.setTextSize(mTextSize);
        tp.setTypeface(Typeface.MONOSPACE);
    }

    @Override
    public int getSize(@NonNull Paint paint,
                       CharSequence text,
                       @IntRange(from = 0) int start,
                       @IntRange(from = 0) int end,
                       @Nullable Paint.FontMetricsInt fm) {
        mPadding = paint.measureText("c");
        return  (int) (paint.measureText(text, start, end) + mPadding * 2);
    }

    @Override
    public void draw(@NonNull Canvas canvas,
                     CharSequence text,
                     @IntRange(from = 0) int start,
                     @IntRange(from = 0) int end,
                     float x,
                     int top,
                     int y,
                     int bottom,
                     @NonNull Paint paint) {
        canvas.drawText(text, start, end, x + mPadding, y, paint);
        paint.setColor(Color.GRAY);
        paint.setAlpha(50);
        final int leading = paint.getFontMetricsInt().leading;
        canvas.drawRect((int) x, top - leading, (int) x + canvas.getWidth(), bottom + leading, paint);
    }


}
```

```InlineCodeSpan``` extends ```ReplacementSpan``` and is used to draw short segments of code.
The ```InlineCodeSpan``` sets the typeface to monospace in ```updateDrawState``` as well as setting the font size to a value provided in the constructor.

In ```getSize```, the padding size is computed as the width of a character before the width is returned as the measured width of the text plus two times the padding.

Finally, in ```draw``` the ```InlineCodeSpan``` first draws the text, offset by the padding computed earlier, and then draws the code background.
The background is an opaque grey rectangle which fills the full width of the ```TextView```.

![InlineCodeSpan](http://imgur.com/vP5nytU.png)

This span completes objective 9.ii.a.1

#### Dealing with more complex content

As explained earlier, some content, notably large code segments and tables, which is usually displayed on the desktop is not well suited for small vertical displays.
As such, it would no t be sensible to display this ocntent directly in the ```TextView```.

Instead, placeholders in the ```TextView``` can be used to link to a more suitable method for displaying the content.

##### CodeSpan

The first problem to be dealt with is larger blocks of code.

As was written in the markdown section of the background information, code blocks are written

\``` Language

 Some code

\```


where the "Language" string is optional.

A ```CodeSpan``` needs to be a large enough item in the ```TextView``` that it can be easily clicked.
It must also be obvious to the user that the span should be clicked.

As such, the span is styled like a button.

**CodeSpan.java**
``` java
package com.tpb.mdtext.views.spans;

import android.content.Context;
import android.graphics.Bitmap;
import android.graphics.Canvas;
import android.graphics.Paint;
import android.graphics.PorterDuff;
import android.graphics.PorterDuffColorFilter;
import android.graphics.RectF;
import android.graphics.drawable.Drawable;
import android.support.annotation.IntRange;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.text.style.ReplacementSpan;

import com.tpb.mdtext.TextUtils;
import com.tpb.mdtext.handlers.CodeClickHandler;

import org.sufficientlysecure.htmltextview.R;

import java.lang.ref.WeakReference;

/**
 * Created by theo on 06/03/17.
 */

public class CodeSpan extends ReplacementSpan implements WrappingClickableSpan.WrappedClickableSpan {
    private WeakReference<CodeClickHandler> mHandler;
    private String mCode;
    private String mLanguage;
    private static String mLanguageFormatString = "%1$s code";
    private static String mNoLanguageString = "Code";
    private static Bitmap mCodeBM;
    private PorterDuffColorFilter mBMFilter;
    private int mBaseOffset = 5;

    public CodeSpan(String code, CodeClickHandler handler) {
        setCode(code);
        mHandler = new WeakReference<>(handler);
    }

    public void setCode(String code) {
        final int ls = code.indexOf('[');
        final int le = code.indexOf(']');
        if(ls != -1 && le != -1 && le - ls > 0 && le < code.indexOf("\n")) {
            mLanguage = TextUtils.capitaliseFirst(code.substring(ls + 1, le));
            mCode = code.substring(le + 1);
        } else {
            mCode = code;
        }
    }

    @Override
    public int getSize(@NonNull Paint paint, CharSequence text, @IntRange(from = 0) int start, @IntRange(from = 0) int end, @Nullable Paint.FontMetricsInt fm) {
        mBaseOffset = (int) paint.measureText("c");
        return 0;
    }

    @Override
    public void draw(@NonNull Canvas canvas, CharSequence text, @IntRange(from = 0) int start, @IntRange(from = 0) int end, float x, int top, int y, int bottom, @NonNull Paint paint) {
        paint.setTextSize(paint.getTextSize() - 1);
        final int textHeight = paint.getFontMetricsInt().descent - paint.getFontMetricsInt().ascent;

        int offset = mBaseOffset;
        if(mCodeBM != null) offset += mCodeBM.getWidth();

        final int textStart = top + textHeight / 4;

        if(mLanguage != null && !mLanguage.isEmpty()) {
            canvas.drawText(String.format(mLanguageFormatString, mLanguage), x + mBaseOffset + offset, textStart,
                    paint
            );
        } else {
            canvas.drawText(mNoLanguageString, x + mBaseOffset + offset, textStart, paint);
        }

        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeWidth(mBaseOffset / 4);
        canvas.drawRoundRect(new RectF(x, top + top - bottom, x + canvas.getWidth(), bottom), 7, 7,
                paint
        );

        if(mCodeBM != null) {
            if(mBMFilter == null) mBMFilter = new PorterDuffColorFilter(paint.getColor(), PorterDuff.Mode.SRC_IN);
            paint.setColorFilter(mBMFilter);
            canvas.drawBitmap(mCodeBM, x + mBaseOffset, textStart - textHeight, paint);
        }
    }

    public void onClick() {
        if(mHandler.get() != null) mHandler.get().codeClicked(mCode, mLanguage);
    }

    public static void initialise(Context context) {
        final Drawable drawable = context.getResources().getDrawable(R.drawable.ic_code);
        final Bitmap bitmap = Bitmap.createBitmap(drawable.getIntrinsicWidth(),
                drawable.getIntrinsicHeight(), Bitmap.Config.ARGB_8888
        );
        final Canvas canvas = new Canvas(bitmap);
        drawable.setBounds(0, 0, canvas.getWidth(), canvas.getHeight());
        drawable.draw(canvas);
        mCodeBM = bitmap;
        mLanguageFormatString = context.getString(R.string.code_span_language_format);
        mNoLanguageString = context.getString(R.string.code_span_no_language);
    }

    public static boolean isInitialised() {
        return mCodeBM != null;
    }

}

```

The ```CodeSpan``` is more complicated than the other spans because it also draws a ```BitMap``` image and deals with being clicked.

mHandler is a ```WeakReference``` to a ```CodeClickHandler```, which is called when the span is clicked.
The code string and language are stored when the span is created, and the format strings are used to format the displayed text dependent on whether or not the language has been set.

```setCode``` is used to check if a language has been embedded in the code string.
While it has not been explained yet, when the code is parsed it will begin with two square brackets followed by a newline, with the language between the two brackets.
If the brackets exist, and are before the first index of a newline, the language is extracted. 

```getSize``` measures the size of a single character, and then returns 0, as the span is not drawing any text in the normal manner.

###### Drawing

```draw``` is used to draw a button shape, the "button" text, and the ```BitMap```.
First, the text size is reduced, as the text is to be drawn between the bounds of the "button".
Next, the text height is computed from the ```Paint``` font metrics.

The offset is used when drawing text to ensure that it is correctly aligned in the horizontal.
If the ```BitMap``` is non-null, its width is added to the offset.

The vertical start position of the text is computed as the top position, plus a quarter of the text height.

If the language is non-null and non-empty, the formatted string containing the language is drawn.
Otherwise, the no-language string is drawn.

In order to draw the "button" shape, the ```Paint``` style is changed to stroke, and its width set to one quarter of the base offset, which is one character width.
A round rectangle is then drawn across two lines of vertical space, and the entire width of the canvas.

Finally, if the ```BitMap``` has been initialised, a colour filter is set to ensure that it is the same colour as the text, and it is drawn at the start of the span, at a vertical position
the same as the start of the text.

###### Initialisation

As the ```CodeSpan``` has no access to a ```Context``` instance, it cannot load items from resources.
This is done with a static initialiser which is called in the custom ```TextView``` written for displaying markdown.
This allows the ```BitMap``` to be loaded and the correct strings to be loaded for a particular language.

![CodeSpan](http://imgur.com/vC8m9BG.png)

This span completes objective 9.ii.a.2

##### TableSpan

The ```TableSpan``` is structured in a very similar manner to ```CodeSpan``` as it also draws a "button" style span across two lines, and draws a bitmap if it has been initialised.

**TableSpan.java**
``` java
package com.tpb.mdtext.views.spans;

import android.content.Context;
import android.graphics.Bitmap;
import android.graphics.Canvas;
import android.graphics.Paint;
import android.graphics.PorterDuff;
import android.graphics.PorterDuffColorFilter;
import android.graphics.RectF;
import android.graphics.drawable.Drawable;
import android.support.annotation.IntRange;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.text.style.ReplacementSpan;

import com.tpb.mdtext.handlers.TableClickHandler;

import org.sufficientlysecure.htmltextview.R;

import java.lang.ref.WeakReference;

/**
 * Created by theo on 08/04/17.
 */

public class TableSpan extends ReplacementSpan implements WrappingClickableSpan.WrappedClickableSpan {

    private WeakReference<TableClickHandler> mHandler;
    private static String mTableString = "Table";
    private static Bitmap mTableBM;
    private PorterDuffColorFilter mBMFilter;
    private String mHtml;
    private int mBaseOffset = 7;

    public TableSpan(String html, TableClickHandler handler) {
        mHtml = html;
        mHandler = new WeakReference<>(handler);
    }

    @Override
    public int getSize(@NonNull Paint paint, CharSequence text, @IntRange(from = 0) int start, @IntRange(from = 0) int end, @Nullable Paint.FontMetricsInt fm) {
        mBaseOffset = (int) paint.measureText("c");
        return 0;
    }

    @Override
    public void draw(@NonNull Canvas canvas, CharSequence text, @IntRange(from = 0) int start, @IntRange(from = 0) int end, float x, int top, int y, int bottom, @NonNull Paint paint) {
        paint.setTextSize(paint.getTextSize() - 1);
        final int textHeight = paint.getFontMetricsInt().descent - paint.getFontMetricsInt().ascent;

        int offset = mBaseOffset;
        if(mTableBM != null) offset += mTableBM.getWidth();

        final int textStart = top + textHeight / 4;

        canvas.drawText(mTableString, x + mBaseOffset + offset, textStart, paint);

        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeWidth(mBaseOffset / 4);
        canvas.drawRoundRect(new RectF(x, top + top - bottom, x + canvas.getWidth(), bottom), 7, 7,
                paint
        );

        if(mTableBM != null) {
            if(mBMFilter == null)
                mBMFilter = new PorterDuffColorFilter(paint.getColor(), PorterDuff.Mode.SRC_IN);
            paint.setColorFilter(mBMFilter);
            canvas.drawBitmap(mTableBM, x + mBaseOffset, textStart - textHeight, paint);
        }
    }


    public void onClick() {
        if(mHandler.get() != null) mHandler.get().onClick(mHtml);
    }

    public static void initialise(Context context) {
        final Drawable drawable = context.getResources().getDrawable(R.drawable.ic_table);
        final Bitmap bitmap = Bitmap.createBitmap(drawable.getIntrinsicWidth(),
                drawable.getIntrinsicHeight(), Bitmap.Config.ARGB_8888
        );
        final Canvas canvas = new Canvas(bitmap);
        drawable.setBounds(0, 0, canvas.getWidth(), canvas.getHeight());
        drawable.draw(canvas);
        mTableBM = bitmap;
        mTableString = context.getString(R.string.table_span);
    }

    public static boolean isInitialised() {
        return mTableBM != null;
    }

}

```

The key differences are that ```TableSpan``` only ever draws a constant string, and that it takes a ```TableClickHandler``` which only takes one parameter, the table HTML, rather than
code and a language.

![Table span](http://imgur.com/NDA1ydi.png)

This span completes objective 9.ii.i

##### ClickableImageSpan

```ClickableImageSpan``` extends ```ImageSpan``` and is used to implement click listening for the ```ImageClickHandler```.
It also ensures that the actual drawable is returned from a ```URLDrawable```, which will be explained in the "Image loading and caching" section.

**ClickableImageSpan.java**
``` java
package com.tpb.mdtext.views.spans;

import android.graphics.drawable.Drawable;
import android.text.style.ImageSpan;

import com.tpb.mdtext.handlers.ImageClickHandler;
import com.tpb.mdtext.imagegetter.HttpImageGetter;

import java.lang.ref.WeakReference;

/**
 * Created by theo on 22/04/17.
 */

public class ClickableImageSpan extends ImageSpan implements WrappingClickableSpan.WrappedClickableSpan {

    private WeakReference<ImageClickHandler> mImageClickHandler;

    public ClickableImageSpan(Drawable d, ImageClickHandler handler) {
        super(d);
        mImageClickHandler = new WeakReference<>(handler);
    }

    @Override
    public Drawable getDrawable() {
        if(super.getDrawable() instanceof HttpImageGetter.URLDrawable && ((HttpImageGetter.URLDrawable) super.getDrawable()).getDrawable() != null) {
            return ((HttpImageGetter.URLDrawable) super.getDrawable()).getDrawable();
        }
        return super.getDrawable();
    }

    @Override
    public void onClick() {
        if(mImageClickHandler.get() != null) {
            mImageClickHandler.get().imageClicked(getDrawable());
        }
    }
}

```

This span completes objective 9.ii.c.3, as well as 9.ii.c.5 once click handling is explained below.

##### Handling clicks

```ReplacementSpans``` have not click listeners. 
Clicks on individual spans in a  ```TextView``` are handled by ```ClickableSpans``` and the ```MovementMethod```.

```ClickableSpan``` is an abstract class extending ```CharacterStyle``` and implementing ```UpdateAppearance```. It sets the ```TextPaint``` colour to the link colour, and underlines the
clickable content. ```ClickableSpan``` has the abstract method ```onClick``` which takes the ```View``` which was clicked.

The calling of ```onClick``` is handled by the ```MovementMethod```.
A ```MovementMethod``` "provides cursor positioning, scrolling and text selection functionality in a ```TextView```.".
The ```TextView``` delegates handling of key events and touches to the movement method for content navigation.

###### Handling clicks on ReplacementSpans

As ```ClickableSpan``` is an abstract class, ```ReplacementSpans``` cannot directly implement the ```onClick``` method.
Instead, their click handling must be handled by a ```ClickableSpan``` which is set across the same subsection of text as the ```ReplacementSpan```.

**WrappingClickableSpan.java**
``` java
package com.tpb.mdtext.views.spans;

import android.support.annotation.NonNull;
import android.text.style.ClickableSpan;
import android.view.View;

/**
 * Created by theo on 11/04/17.
 */

public class WrappingClickableSpan extends ClickableSpan {

    private final WrappedClickableSpan mWrappedClickableSpan;

    public WrappingClickableSpan(@NonNull WrappedClickableSpan child) {
        mWrappedClickableSpan = child;
    }

    @Override
    public void onClick(View widget) {
        mWrappedClickableSpan.onClick();
    }

    public interface WrappedClickableSpan {

        void onClick();

    }

}

```

As was shown above, ```CodeSpan```, ```TableSpan``` and ```ImageSpan``` implement ```WrappedClickableSpan``` which allows touch events on a parent ```ClickableSpan``` to be forwarded to the 
```ReplacementSpan```.

###### Stopping the onClickHandler

The problem with having clickable elements in the ```TextView``` is that it interferes with any click listeners set on the ```TextView``` itself.

This problem is solved with a custom ```MovementMethod``` and ```TextView```.

**ClickableMovementMethod.java**
``` java
package com.tpb.mdtext;

import android.text.Layout;
import android.text.Selection;
import android.text.Spannable;
import android.text.method.LinkMovementMethod;
import android.text.method.Touch;
import android.text.style.ClickableSpan;
import android.view.MotionEvent;
import android.widget.TextView;

import com.tpb.mdtext.views.MarkdownTextView;

/**
 * Copied from http://stackoverflow.com/questions/8558732
 */
public class ClickableMovementMethod extends LinkMovementMethod {

    private static ClickableMovementMethod instance;

    private ClickableMovementMethod() {}

    public static ClickableMovementMethod getInstance() {
        if(instance == null) instance = new ClickableMovementMethod();
        return instance;
    }

    @Override
    public boolean onTouchEvent(final TextView widget, final Spannable buffer, MotionEvent event) {
        final int action = event.getAction();

        if(action == MotionEvent.ACTION_UP || action == MotionEvent.ACTION_DOWN) {

            int x = (int) event.getX();
            int y = (int) event.getY();

            x -= widget.getTotalPaddingLeft();
            y -= widget.getTotalPaddingTop();

            x += widget.getScrollX();
            y += widget.getScrollY();

            final Layout layout = widget.getLayout();
            final int line = layout.getLineForVertical(y);
            final int off = layout.getOffsetForHorizontal(line, x);

            final ClickableSpan[] clickable = buffer.getSpans(off, off, ClickableSpan.class);

            if(clickable.length != 0) {
                if(action == MotionEvent.ACTION_UP) {
                    clickable[0].onClick(widget);
                    triggerSpanHit(widget);
                }
                return true;
            } else {
                Selection.removeSelection(buffer);
                Touch.onTouchEvent(widget, buffer, event);
                return false;
            }
        }

        return Touch.onTouchEvent(widget, buffer, event);
    }

    private void triggerSpanHit(TextView widget) {
        if(widget instanceof MarkdownTextView) {
            ((MarkdownTextView) widget).setSpanHit();
        }
    }

}

```

The ```ClickableMovementMethod``` extends ```LinkMovementMethod``` and overrides ```onTouchEvent``` to deal with all clickable spans, as well as notifying the ```TextView```.

If the event acction is an up or down movement, the event is captured.

The x and y positions are collected from the event, and then offset by both the padding and scroll position of the ```TextView```.
The ```TextView``` layout is then used to calculate the line of text, and the offset within the line for the click position.

Using this offset, the array of ```ClickableSpans``` present at this position is found from the buffer.

If the array is not empty, and the event type is up (The end of a click) the ```ClickableSpan``` onClick method is called, and the span hit is triggered on the ```TextView```. Returning
true results in further events being passed to the ```MovementMethod```.

If the array is empty, the selection is removed, the touch event is triggered, and false is returned so that no further events in this chain are passed to the ```MovementMethod```.

Within the ```TextView```, ```setSpanHit``` is used to set a flag for triggering click events.

Usually, to handle click events for a ```View```, one would call ```setOnClickListener``` which would then be called when the ```TextView``` is clicked.
The problem with this is that the ```OnClickListener``` would recieve span click events.

To solve this problem, the ```TextView``` itself implements ```OnClickListener```.

The ```TextView``` also overrides ```setOnClickListener```. In this method it stores the ```onClickListener```.
In ```onClick```, it checks if the span hit flag is false, and the listener is non null, and if both of these are true it forwards the click to the listener.
It then sets the span hit flag back to false.

#### MarkdownTextView

```MarkdownTextView``` is the ```TextView``` descendent used for displaying markdown.
It handles click handling for links, images, tables, and code, as well as dealing with background parsing of content and caching of parsed content. 

##### Handlers

There are four handler interfaces used for click events on different items in the ```MarkdownTextView```.
There are also three default implementations of these interfaces.

**CodeClickHandler.java**
``` java
package com.tpb.mdtext.handlers;

import android.support.annotation.Nullable;

/**
 * Created by theo on 27/02/17.
 */

public interface CodeClickHandler {

    void codeClicked(String code, @Nullable String language);

}

```

**ImageClickHandler.java**
``` java
package com.tpb.mdtext.handlers;

import android.graphics.drawable.Drawable;

/**
 * Created by theo on 27/02/17.
 */

public interface ImageClickHandler {

    void imageClicked(Drawable drawable);

}

```

**LinkClickHandler.java**
``` java
package com.tpb.mdtext.handlers;

/**
 * Created by theo on 27/02/17.
 */

public interface LinkClickHandler {

    void onClick(String url);

}

```

**TableClickHandler.java**
``` java
package com.tpb.mdtext.handlers;

/**
 * Created by theo on 08/04/17.
 */

public interface TableClickHandler {

    public void onClick(String html);

}

```

The default implementations of ```CodeClickHandler```, ```ImageClickHandler``` and ```TableClickHandler``` are all dialogs used to show the content over a larger area.
They can be replaced with any other implementation of their respective interfaces.

**CodeDialog.java**
``` java
package com.tpb.mdtext.dialogs;

import android.app.AlertDialog;
import android.app.Dialog;
import android.content.Context;
import android.support.annotation.Nullable;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.view.Window;

import com.pddstudio.highlightjs.HighlightJsView;
import com.pddstudio.highlightjs.models.Language;
import com.pddstudio.highlightjs.models.Theme;
import com.tpb.mdtext.handlers.CodeClickHandler;

import org.sufficientlysecure.htmltextview.R;

/**
 * Created by theo on 27/02/17.
 */

public class CodeDialog implements CodeClickHandler {

    private Context mContext;

    public CodeDialog(Context context) {
        mContext = context;
    }

    @Override
    public void codeClicked(String code, @Nullable String language) {
        final AlertDialog.Builder builder = new AlertDialog.Builder(mContext);
        final LayoutInflater inflater = LayoutInflater.from(mContext);

        final View view = inflater.inflate(R.layout.dialog_code, null);

        builder.setView(view);

        final HighlightJsView wv = (HighlightJsView) view.findViewById(R.id.dialog_highlight_view);
        wv.setTheme(Theme.ANDROID_STUDIO);

        if(language != null) wv.setHighlightLanguage(getLanguage(language));
        wv.setSource(code);
        final Dialog dialog = builder.create();

        dialog.getWindow()
              .setLayout(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT);
        dialog.getWindow().requestFeature(Window.FEATURE_NO_TITLE);
        dialog.show();
    }

    private static Language getLanguage(String lang) {
        for(Language l : Language.values()) {
            if(l.toString().equalsIgnoreCase(lang)) return l;
        }
        return Language.AUTO_DETECT;
    }
}

```

The ```CodeDialog``` creates a dialog to display a ```HighlightJsView```, which is a ```WebView``` with the highlightjs library embedded.
It also attempts to find the correct language for highlighting the code.

This completes objective 9.ii.a.2

**ImageDialog.java**
``` java
package com.tpb.mdtext.dialogs;

import android.app.AlertDialog;
import android.app.Dialog;
import android.content.Context;
import android.graphics.Color;
import android.graphics.drawable.ColorDrawable;
import android.graphics.drawable.Drawable;
import android.view.LayoutInflater;
import android.view.View;
import android.view.WindowManager;

import com.tpb.mdtext.handlers.ImageClickHandler;

import org.sufficientlysecure.htmltextview.R;

/**
 * Created by theo on 27/02/17.
 */

public class ImageDialog implements ImageClickHandler {

    private final Context mContext;

    public ImageDialog(Context context) {
        mContext = context;
    }

    @Override
    public void imageClicked(Drawable drawable) {
        final AlertDialog.Builder builder = new AlertDialog.Builder(mContext);
        final LayoutInflater inflater = LayoutInflater.from(mContext);

        final View view = inflater.inflate(R.layout.dialog_image, null);

        builder.setView(view);

        final FillingImageView fiv = (FillingImageView) view.findViewById(R.id.dialog_imageview);
        fiv.setImageDrawable(drawable);

        final Dialog dialog = builder.create();
        dialog.getWindow().setBackgroundDrawable(new ColorDrawable(Color.TRANSPARENT));
        dialog.getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN,
                WindowManager.LayoutParams.FLAG_FULLSCREEN
        );
        dialog.getWindow().setLayout(WindowManager.LayoutParams.MATCH_PARENT,
                WindowManager.LayoutParams.MATCH_PARENT
        );

        fiv.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                dialog.dismiss();

            }
        });

        dialog.show();

    }

}

```

The ```ImageDialog``` is used to show an image across the entire screen, while maintaining its aspect ratio.

It uses a ```FillingImageView``` which overrides the ```onMeasure``` method of ```ImageView``` to ensure that the image aspect ratio is maintained.

This completes objective 9.ii.c.5

**FillingImageView.java**
``` java
package com.tpb.mdtext.dialogs;

import android.content.Context;
import android.graphics.drawable.Drawable;
import android.support.annotation.Nullable;
import android.support.v7.widget.AppCompatImageView;
import android.util.AttributeSet;

/**
 * Created by theo on 31/01/17.
 */

public class FillingImageView extends AppCompatImageView {

    public FillingImageView(Context context) {
        super(context);
    }

    public FillingImageView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
    }


    public FillingImageView(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        try {
            final Drawable drawable = getDrawable();

            if(drawable == null) {
                setMeasuredDimension(0, 0);
            } else {
                final float imageSideRatio = (float) drawable.getIntrinsicWidth() / (float) drawable
                        .getIntrinsicHeight(); //Image aspect ratio
                final float viewSideRatio = (float) MeasureSpec
                        .getSize(widthMeasureSpec) / (float) MeasureSpec
                        .getSize(heightMeasureSpec); //Aspect ratio of parent
                if(imageSideRatio >= viewSideRatio) {
                    // Image is wider than the display (ratio)
                    final int width = MeasureSpec.getSize(widthMeasureSpec);
                    final int height = (int) (width / imageSideRatio);
                    setMeasuredDimension(width, height);
                } else {
                    // Image is taller than the display (ratio)
                    final int height = MeasureSpec.getSize(heightMeasureSpec);
                    final int width = (int) (height * imageSideRatio);
                    setMeasuredDimension(width, height);
                }
            }
        } catch(Exception e) {
            super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        }
    }

}

```

The method calculates the aspect ratio of the image, and the aspect ratio of the parent ```View```.

If the image has a wider ratio than the display ratio, then the width is calculated as the available width of the ```ImageView```, and the height is calculated as the ratio of this width
matching the ratio calculated earlier.

If the image has a taller ratio than the display ratio, then the height is calculated as the available height of the ```ImageView```, and the width is calculated as the ratio of this 
height matching the ratio calculated earlier.

**TableDialog.java**
``` java
package com.tpb.mdtext.dialogs;

import android.app.AlertDialog;
import android.app.Dialog;
import android.content.Context;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.view.Window;

import com.tpb.mdtext.handlers.TableClickHandler;
import com.tpb.mdtext.webview.MarkdownWebView;

import org.sufficientlysecure.htmltextview.R;

/**
 * Created by theo on 08/04/17.
 */

public class TableDialog implements TableClickHandler {

    private Context mContext;

    public TableDialog(Context context) {
        mContext = context;
    }

    @Override
    public void onClick(String html) {
        final AlertDialog.Builder builder = new AlertDialog.Builder(mContext);
        final LayoutInflater inflater = LayoutInflater.from(mContext);

        final View view = inflater.inflate(R.layout.dialog_table, null);

        builder.setView(view);

        final MarkdownWebView wv = (MarkdownWebView) view.findViewById(R.id.dialog_web_view);
        wv.enableDarkTheme();
        wv.setMarkdown(html);
        final Dialog dialog = builder.create();

        dialog.getWindow()
              .setLayout(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT);
        dialog.getWindow().requestFeature(Window.FEATURE_NO_TITLE);
        dialog.show();
    }

}

```

The ```TableDialog``` uses the ```MarkdownWebView``` created earlier to display the HTML of the table in a ```WebView``` using the GitHub style CSS.

This dialog completes objective 9.ii.i

##### Image loading and caching 

The ```HttpImageGetter``` implements ```Html.ImageGetter``` which is used when retreiving images for img tags.

As well as loading images, the ```HttpImageGetter``` also caches them, which is especially useful when editing markdown segments containing images or in a comment feed where multiple users may use the same image, as it stops the image being reloaded
each time the editor is toggled from raw to formatted markdown.

**HttpImageGetter.java**
``` java
package com.tpb.mdtext.imagegetter;

import android.content.res.Resources;
import android.graphics.Canvas;
import android.graphics.drawable.BitmapDrawable;
import android.graphics.drawable.Drawable;
import android.os.AsyncTask;
import android.support.annotation.Nullable;
import android.support.v4.util.Pair;
import android.text.Html.ImageGetter;
import android.view.View;
import android.widget.TextView;

import java.io.IOException;
import java.io.InputStream;
import java.lang.ref.WeakReference;
import java.net.URI;
import java.net.URL;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;

public class HttpImageGetter implements ImageGetter {

    private static final HashMap<String, Pair<Drawable, Long>> cache = new HashMap<>();

    private final TextView mContainer;

    public HttpImageGetter(TextView container) {
        this.mContainer = container;
    }


    public Drawable getDrawable(String source) {
        final URLDrawable ud = new URLDrawable();
        new ImageGetterAsyncTask(ud, this, mContainer).execute(source);
        return ud;
    }

    private static class ImageGetterAsyncTask extends AsyncTask<String, Void, Drawable> {
        private final WeakReference<URLDrawable> mDrawableReference;
        private final WeakReference<HttpImageGetter> mGetterReference;
        private final WeakReference<View> mContainerReference;
        private final WeakReference<Resources> mResources;
        private String mSource;
        private float mScale;

        ImageGetterAsyncTask(URLDrawable d, HttpImageGetter imageGetter, View container) {
            this.mDrawableReference = new WeakReference<>(d);
            this.mGetterReference = new WeakReference<>(imageGetter);
            this.mContainerReference = new WeakReference<>(container);
            this.mResources = new WeakReference<>(container.getResources());
        }

        @Override
        protected Drawable doInBackground(String... params) {
            mSource = params[0];
            synchronized(cache) {
                Map.Entry<String, Pair<Drawable, Long>> entry;
                final Iterator<Map.Entry<String, Pair<Drawable, Long>>> it = cache.entrySet().iterator();
                for(; it.hasNext(); ) {
                    entry = it.next();
                    if(System.currentTimeMillis() > entry.getValue().second + 60000) {
                        it.remove();
                    }
                }

                if(cache.containsKey(mSource)) {
                    if(System.currentTimeMillis() > cache.get(mSource).second + 45000) {
                        // The drawable is still being accessed, so we update it
                        fetchDrawable(mResources.get(), mSource);
                    }
                    return cache.get(mSource).first.getConstantState().newDrawable();
                } else if(mResources.get() != null) {
                    return fetchDrawable(mResources.get(), mSource);
                }
            }


            return null;
        }

        @Override
        protected void onPostExecute(Drawable result) {
            final URLDrawable urlDrawable = mDrawableReference.get();
            final HttpImageGetter imageGetter = mGetterReference.get();
            // We exist outside of the lifespan of the view
            if(result == null || urlDrawable == null || imageGetter == null) return;


            // Scale is set here as drawable may be cached and view may have changed
            setDrawableScale(result);

            // Set the correct bound according to the result from HTTP call
            urlDrawable.setBounds(0, 0, (int) (result.getIntrinsicWidth() * mScale),
                    (int) (result.getIntrinsicHeight() * mScale)
            );

            // Change the reference of the current urlDrawable to the result from the HTTP call
            urlDrawable.mDrawable = result;

            // redraw the image by invalidating the container
            imageGetter.mContainer.invalidate();
            // re-set text to fix images overlapping text
            imageGetter.mContainer.setText(imageGetter.mContainer.getText());
        }

        @Nullable
        private Drawable fetchDrawable(Resources res, String urlString) {
            try {
                final InputStream is = fetch(urlString);
                final Drawable drawable = new BitmapDrawable(res, is);
                synchronized(cache) {
                    cache.put(mSource, Pair.create(drawable, System.currentTimeMillis()));
                }
                return drawable;
            } catch(Exception e) {
                return null;
            }
        }

        private void setDrawableScale(Drawable drawable) {
            mScale = getScale(drawable);
            drawable.setBounds(0, 0, (int) (drawable.getIntrinsicWidth() * mScale),
                    (int) (drawable.getIntrinsicHeight() * mScale)
            );
        }

        private float getScale(Drawable drawable) {
            final View container = mContainerReference.get();
            if(container == null) {
                return 1f;
            }
            float maxWidth = container.getWidth();
            float originalDrawableWidth = drawable.getIntrinsicWidth();
            return maxWidth / originalDrawableWidth;
        }

        @Nullable
        private InputStream fetch(String urlString) throws IOException {
            URL url;
            final HttpImageGetter imageGetter = mGetterReference.get();
            if(imageGetter == null) {
                return null;
            }
            url = URI.create(urlString).toURL();


            return (InputStream) url.getContent();
        }
    }

    @SuppressWarnings("deprecation")
    public class URLDrawable extends BitmapDrawable {
        Drawable mDrawable;

        @Override
        public void draw(Canvas canvas) {
            if(mDrawable != null) {
                mDrawable.draw(canvas);
            }
        }

        public Drawable getDrawable() {
            return mDrawable;
        }

    }
} 

```

The ```HttpImageGetter``` is constructed with a reference to the ```TextView``` for which it is loading  images.

The ```URLDrawable``` is used in order to only draw the ```Drawable``` to the canvas once it has been loaded.

When ```getDrawable``` is called, ```HttpImageGetter``` creates a new ```URLDrawable```, then creates a new ```ImageGetterAsyncTask``` and then returns the ```URLDrawable```.

The ```ImageGetterAsyncTask``` then begins loading the image in the background.
The ```ImageGetterAsyncTask``` takes a ```URLDrawable```, the ```HTTPImageGetter``` and the containing ```View```, and stores ```WeakReferences``` to each of them as well as a 
```WeakReference``` to the ```Resources``` from the container ```View```.

When an ```AsyncTask``` is executed, first two methods are executed on the main thread, and the third is executed in the background.
The first on the main thread is ```onPreExecute```, which is not needed here as there is no setup process.
Next, ```doInBackground``` is called, which performs work on another thread. Finally, ```onPostExecute``` is called on the main thread, with the result from ```doInBackground```.

In ```doInBackground``` the source is first extracted from the first parameter.
Next the cache is checked for the source. This must be synchronised as the cache may be accessed from multiple threads at once.
The cache is a ```Map``` and must be accessed with an ```Iterator``` to allow removal at the same time.

The ```Map``` contains pairs of strings, the source URLs, and pairs of the drawable and the last time that it was updated.
If the drawable was last updated over a minute ago, it is removed from the cache.

Once the cache has been cleaned, the cache is checked for the current key, which is reloaded if it was last accessed more than 45 seconds ago.
The ```Drawable``` is returned with ```getConstantState().newDrawable()```. This ensures that each ```Drawable``` displayed has the correct aspect ratio for the ```TextView``` in which it
is displayed, otherwise only the last mutation would take effect.

Otherwise, the ```Resources``` reference is checked, and if it is non-null, the ```Drawable``` is loaded.

```fetchDrawable``` creates an ```InputStream``` by calling ```fetch```.
```fetch``` checks if the ```HttpImageGetter``` still exists, and if so creates a ```URL``` from which to load the content.
```fetchDrawable``` then creates a ```BitmapDrawable``` from this ```InputStream``` and the ```Resources``` object which is used to determine specifics about how the ```Drawable``` should
be displayed.
The ```Drawable``` is then added to the cache and returned.

In ```onPostExecute``` the validity of the ```HttpImageGetter```, result and ```URLDrawable``` are checked, and if they are valid the ```Drawable``` is scaled.
```setDrawableScale``` calls ```getScale``` which returns the scale of the ```View``` maximum width to the ```Drawable``` width.
```setDrawableScale``` then sets the bounds of the ```Drawable``` to the scaled values of its original width and height, maintaining its aspect ratio while filling the full width of the
containing ```View```.

Once the scale of the result ```Drawable``` has been set, the scale of the ```URLDrawable``` is also set, and the ```URLDrawable``` drawable is changed for the result ```Drawable```.
If the ```DrawableCatcher``` is non-null, the drawable and URL are passed to it.

Finally, in order to draw the ```URLDrawable``` the container is invalidated and its text is set to its current text to force a layout refresh.

This completes objective 9.ii.c.1, 9.ii.c.2, and 9.ii.c.4.

###### Loading images from Assets and Resources

While they are not currently used in this project, some may want to load images from the device itself. Either those included in the assets directory, or in resources.

This can be handled with the ```AssetsImageGetter``` and ```ResImageGetter```.

**AssetsImageGetter.java**
``` java
package com.tpb.mdtext.imagegetter;

import android.content.Context;
import android.graphics.drawable.Drawable;
import android.text.Html;
import android.widget.TextView;

import java.io.IOException;
import java.io.InputStream;

/**
 * Assets Image Getter
 * <p>
 * Load image from assets folder
 *
 * @author <a href="mailto:daniel@passos.me">Daniel Passos</a>
 */
class AssetsImageGetter implements Html.ImageGetter {

    private final Context context;

    public AssetsImageGetter(Context context) {
        this.context = context;
    }

    public AssetsImageGetter(TextView textView) {
        this.context = textView.getContext();
    }

    @Override
    public Drawable getDrawable(String source) {

        try {
            final InputStream inputStream = context.getAssets().open(source);
            final Drawable d = Drawable.createFromStream(inputStream, null);
            d.setBounds(0, 0, d.getIntrinsicWidth(), d.getIntrinsicHeight());
            return d;
        } catch(IOException e) {
            return null;
        }

    }

}

```

The ```AssetsImageGetter``` creates an ```InputStream``` from an assets path and loads the drawable from it.

**ResImageGetter.java**
``` java
package com.tpb.mdtext.imagegetter;

import android.content.Context;
import android.graphics.drawable.Drawable;
import android.text.Html;
import android.widget.TextView;

/**
 * Copied from http://stackoverflow.com/a/22298833
 */
class ResImageGetter implements Html.ImageGetter {
    private final TextView container;

    public ResImageGetter(TextView textView) {
        this.container = textView;
    }

    public Drawable getDrawable(String source) {
        final Context context = container.getContext();
        int id = context.getResources().getIdentifier(source, "drawable", context.getPackageName());

        if(id == 0) {
            //Drawable not in this package, might be somewhere else
            id = context.getResources().getIdentifier(source, "drawable", "android");
        }

        if(id == 0) {
            return null;
        } else {
            final Drawable d = context.getResources().getDrawable(id);
            d.setBounds(0, 0, d.getIntrinsicWidth(), d.getIntrinsicHeight());
            return d;
        }
    }

}
```

The ```ResImageGetter``` attempts to load a drawable from a resource name, checking the current package as well as the built in drawables.

##### MarkdownTextView

```MarkdownTextView``` is used to implement each of the individual objectives described above. It handles backgrround parsing of the markdown as well as dealing with click handling and
the caching of content.

The ```MarkdownTextView``` contains a ```LinkClickHandler```, ```ImageClickHandler```, ```TableClickHandler```, and ```CodeClickHandler```.

**MarkdownTextView.java**
``` java
package com.tpb.mdtext.views;

import android.content.Context;
import android.graphics.Color;
import android.os.Build;
import android.os.Handler;
import android.os.Looper;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.support.annotation.RawRes;
import android.support.v7.widget.AppCompatTextView;
import android.text.Html;
import android.text.SpannableString;
import android.text.Spanned;
import android.util.AttributeSet;
import android.util.Log;
import android.view.MotionEvent;
import android.view.View;

import com.tpb.mdtext.ClickableMovementMethod;
import com.tpb.mdtext.HtmlTagHandler;
import com.tpb.mdtext.Markdown;
import com.tpb.mdtext.SpanCache;
import com.tpb.mdtext.TextUtils;
import com.tpb.mdtext.dialogs.CodeDialog;
import com.tpb.mdtext.dialogs.ImageDialog;
import com.tpb.mdtext.dialogs.TableDialog;
import com.tpb.mdtext.handlers.CodeClickHandler;
import com.tpb.mdtext.handlers.ImageClickHandler;
import com.tpb.mdtext.handlers.LinkClickHandler;
import com.tpb.mdtext.handlers.TableClickHandler;
import com.tpb.mdtext.views.spans.CodeSpan;
import com.tpb.mdtext.views.spans.TableSpan;

import java.io.InputStream;
import java.lang.ref.WeakReference;
import java.util.Scanner;


public class MarkdownTextView extends AppCompatTextView implements View.OnClickListener {

    public static final String TAG = MarkdownTextView.class.getSimpleName();

    @Nullable private LinkClickHandler mLinkHandler;
    @Nullable private ImageClickHandler mImageClickHandler;
    @Nullable private TableClickHandler mTableHandler;
    @Nullable private CodeClickHandler mCodeHandler;
    @Nullable private Handler mParseHandler;

    private boolean mSpanHit = false;
    private OnClickListener mOnClickListener;
    private float[] mLastClickPosition = new float[] {-1, -1};

    private WeakReference<SpanCache> mSpanCache;

    public MarkdownTextView(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
        init(context);
    }

    public MarkdownTextView(Context context, AttributeSet attrs) {
        super(context, attrs);
        init(context);
    }

    public MarkdownTextView(Context context) {
        super(context);
        init(context);
    }

    private void init(Context context) {
        if(!CodeSpan.isInitialised()) CodeSpan.initialise(context);
        if(!TableSpan.isInitialised()) TableSpan.initialise(context);
        setDefaultHandlers(context);
        setTextIsSelectable(true);
        setCursorVisible(false);
        setClickable(true);
        setTextColor(Color.WHITE);
    }

    public void setParseHandler(@Nullable Handler parseHandler) {
        mParseHandler = parseHandler;
    }

    public void setLinkClickHandler(LinkClickHandler handler) {
        mLinkHandler = handler;
    }

    public void setImageHandler(ImageClickHandler imageHandler) {
        mImageClickHandler = imageHandler;
    }

    public void setCodeClickHandler(CodeClickHandler handler) {
        mCodeHandler = handler;
    }

    public void setTableClickHandler(TableClickHandler handler) {
        mTableHandler = handler;
    }

    public void setDefaultHandlers(Context context) {
        setCodeClickHandler(new CodeDialog(context));
        setImageHandler(new ImageDialog(context));
        setTableClickHandler(new TableDialog(context));
    }

    public void setMarkdown(@NonNull String markdown) {
        setMarkdown(markdown, null, null);
    }

    public void setMarkdown(@RawRes int resId) {
        setMarkdown(resId, null);
    }

    private void setMarkdown(@RawRes int resId, @Nullable Html.ImageGetter imageGetter) {
        final InputStream inputStreamText = getContext().getResources().openRawResource(resId);
        setMarkdown(convertStreamToString(inputStreamText), imageGetter, null);
    }

    public void setMarkdown(@NonNull final String markdown, @Nullable final Html.ImageGetter imageGetter, @Nullable SpanCache cache) {
        mSpanCache = new WeakReference<>(cache);
        //If we have a handler use it
        if(mParseHandler != null) {
            mParseHandler.post(new Runnable() {
                @Override
                public void run() {
                    parseAndSetMd(markdown, imageGetter);
                }
            });
        } else {
            parseAndSetMd(markdown, imageGetter);
        }
    }

    private void parseAndSetMd(@NonNull String markdown, @Nullable final Html.ImageGetter imageGetter) {
        // Override tags to stop Html.fromHtml destroying some of them
        markdown = HtmlTagHandler.overrideTags(Markdown.parseMD(markdown));

        final HtmlTagHandler htmlTagHandler = new HtmlTagHandler(this,
                imageGetter,  mLinkHandler, mImageClickHandler, mCodeHandler, mTableHandler
        );
        try {
            final Spanned text;
            if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
                text = removeHtmlBottomPadding(
                        Html.fromHtml(markdown, Html.FROM_HTML_MODE_LEGACY, imageGetter,
                                htmlTagHandler
                        ));
            } else {
                text = removeHtmlBottomPadding(
                        Html.fromHtml(markdown, imageGetter, htmlTagHandler));
            }

            // Convert to a buffer to allow editing
            final SpannableString buffer = new SpannableString(text);

            //Add links for emails and web-urls
            TextUtils.addLinks(buffer);
            if(Looper.myLooper() == Looper.getMainLooper()) {
                setMarkdownText(buffer);
            } else {
                //Post back on UI thread
                MarkdownTextView.this.postDelayed(new Runnable() {
                    @Override
                    public void run() {
                        setMarkdownText(buffer);
                    }
                }, 17);
            }
        } catch(Exception e) {
            Log.e("TextView", "WTF", e);
            markdown = "Error parsing markdown\n\n\n" + Html.fromHtml(Html.escapeHtml(markdown));
            setText(markdown);
        }
    }

    private void setMarkdownText(SpannableString buffer) {
        setText(buffer);
        if(!(getMovementMethod() instanceof ClickableMovementMethod)) {
            setMovementMethod(ClickableMovementMethod.getInstance());
        }
        if(mSpanCache != null && mSpanCache.get() != null) {
            mSpanCache.get().cache(buffer);
            mSpanCache = null;
        }
    }

    @NonNull
    private static String convertStreamToString(@NonNull InputStream is) {
        final Scanner s = new Scanner(is).useDelimiter("\\A");
        return s.hasNext() ? s.next() : "";
    }

    private static Spanned removeHtmlBottomPadding(Spanned text) {
        while(text.length() > 0 && text.charAt(text.length() - 1) == '\n') {
            text = (Spanned) text.subSequence(0, text.length() - 1);
        }
        return text;
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        if(event.getAction() == MotionEvent.ACTION_DOWN) {
            mLastClickPosition[0] = event.getRawX();
            mLastClickPosition[1] = event.getRawY();
            if(hasSelection()) clearFocus();
        } else if(event.getAction() == MotionEvent.ACTION_UP) {
            mSpanHit = false;
        }
        return super.onTouchEvent(event);
    }

    public float[] getLastClickPosition() {
        if(mLastClickPosition[0] == -1) {
            // If we haven't been clicked yet, get the centre of the view
            final int[] pos = new int[2];
            getLocationOnScreen(pos);
            mLastClickPosition[0] = pos[0] + getWidth() / 2;
            mLastClickPosition[1] = pos[1] + getHeight() / 2;
        }
        return mLastClickPosition;
    }

    public void setSpanHit() {
        mSpanHit = true;
    }

    @Override
    public void onClick(View v) {
        if(!mSpanHit && mOnClickListener != null) mOnClickListener.onClick(v);
    }

    @Override
    public void setOnClickListener(@Nullable OnClickListener l) {
        mOnClickListener = l;
        super.setOnClickListener(this);
    }

    @Override
    protected boolean getDefaultEditable() {
        return Build.VERSION.SDK_INT >= Build.VERSION_CODES.N || super.getDefaultEditable();
    }

    @Override
    public boolean isSuggestionsEnabled() {
        return false;
    }
}

```

Each of the constructors calls ```init``` which checks for the initialisation of ```CodeSpan``` and ```TableSpan```, creates the default handlers, enables text selection and clicking, 
and sets the text colour.

After this are setters for each of the handlers.

The raw markdown text can be set from a string or a resource id.
Each of these methods can also take an ```ImageGeter```.

The ```setMarkdown``` method which takes an a ```SpanCacher``` creates a ```WeakReference``` to the ```SpanCacher``` and then checks if there is a ```Handler``` to perform parsing on 
another thread.
If the handler exists, the call to ```parseAndSetMd``` is run on the ```Handler```, otherwise it is called on the UI thread.

In ```parseAndSetMd``` the ```HtmlTagHandler``` is created to parse HTML to spans.
The span text is then overriden to stop the built in ```Html.fromHtml``` capturing but ignoring numerous tags.

If the Android version is APi 25 or greater, the LEGACY flag is used to ensure the styling is the same as on previous versions.
In either case, ```removeHtmlBottomPadding``` is called, as ```Html.fromHtml``` adds two newlines to the end of the parsed HTML.

In order to add the URL and email address links, the ```Spanned``` object must be converted to an ```Editable```.
The links are then added, and the text can be set.

If the current ```Looper``` is the main ```Looper```, then the code is running on the UI thread and the text can be set immediately.
Otherwise, it must be posted on the ```TextView``` itself so that it runs on the UI thread.

```setMarkdownText``` sets the text, and ensures that the movement method is a ```ClickableMovementMethod```.
Finally, if a ```SpanCacher``` exists, it is passed the ```SpannableString```.

The remaining methods are used to handle clicking on the ```TextView```.
```onTouchEvent``` captures all touch events.
If the event action is down the position is stored for use in animations, and any selection is cleared.
If the event is up, the span hit flag is reset.

```setSpanHit``` is used to set the span hit flag, as described in ```ClickableMovementMethod```.

```onClick``` is used to forward the click events to the actual ```OnClickListener``` if it exists and a span has not been hit in the same click event.

```setOnClickListener``` is overriden to store the ```OnClickListener``` and ensure that click events are passed to the ```MarkdownTextView``` itself.

```getDefaultEditable``` checks the Android version, returning true if the SDK version is 25 or above, otherwise returning the default value.
This fixes a problem introduced in SDK 25 where span priority is not respected, which can cause problems when displaying indented content, such as lists.

```isSuggestionEnabled``` is overriden to ensure that suggestions are never shown on the ```MarkdownTextView```, even though it may appear to be editable.

#### MarkdownEditText

The ```MarkdownEditText``` is very similar to ```MarkdownTextView``` except that it extends ```EditText```, a subclass of ```TextView```, does not include support for processing on another
thread, and adds support for toggling between and editable and non-editable state.

**MarkdownEditText.java**
``` java
package com.tpb.mdtext.views;

import android.content.Context;
import android.os.Build;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.support.annotation.RawRes;
import android.support.v7.widget.AppCompatEditText;
import android.text.Editable;
import android.text.Html;
import android.text.SpannableString;
import android.text.SpannableStringBuilder;
import android.text.Spanned;
import android.util.AttributeSet;

import com.tpb.mdtext.ClickableMovementMethod;
import com.tpb.mdtext.HtmlTagHandler;
import com.tpb.mdtext.Markdown;
import com.tpb.mdtext.TextUtils;
import com.tpb.mdtext.dialogs.CodeDialog;
import com.tpb.mdtext.dialogs.ImageDialog;
import com.tpb.mdtext.dialogs.TableDialog;
import com.tpb.mdtext.handlers.CodeClickHandler;
import com.tpb.mdtext.handlers.ImageClickHandler;
import com.tpb.mdtext.handlers.LinkClickHandler;
import com.tpb.mdtext.handlers.TableClickHandler;
import com.tpb.mdtext.views.spans.CodeSpan;
import com.tpb.mdtext.views.spans.TableSpan;

import java.io.InputStream;
import java.util.Scanner;


/**
 * Created by theo on 27/02/17.
 */

public class MarkdownEditText extends AppCompatEditText {

    public static final String TAG = MarkdownEditText.class.getSimpleName();

    private boolean mIsEditing = true;
    private Editable mSavedText = new SpannableStringBuilder();
    @Nullable private LinkClickHandler mLinkHandler;
    @Nullable private ImageClickHandler mImageClickHandler;
    @Nullable private TableClickHandler mTableHandler;
    @Nullable private CodeClickHandler mCodeHandler;


    public MarkdownEditText(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
        init(context);
    }

    public MarkdownEditText(Context context, AttributeSet attrs) {
        super(context, attrs);
        init(context);
    }

    public MarkdownEditText(Context context) {
        super(context);
        init(context);
    }

    private void init(Context context ) {
        if(!CodeSpan.isInitialised()) CodeSpan.initialise(context);
        if(!TableSpan.isInitialised()) TableSpan.initialise(context);
        setDefaultHandlers(context);
        setPadding(0, getPaddingTop(), 0, getPaddingBottom());
    }

    public void setLinkClickHandler(LinkClickHandler handler) {
        mLinkHandler = handler;
    }

    public void setImageHandler(ImageClickHandler imageHandler) {
        mImageClickHandler = imageHandler;
    }

    public void setTableClickHandler(TableClickHandler handler) {
        mTableHandler = handler;
    }

    public void setCodeClickHandler(CodeClickHandler handler) {
        mCodeHandler = handler;
    }

    public void setDefaultHandlers(Context context) {
        setCodeClickHandler(new CodeDialog(context));
        setImageHandler(new ImageDialog(context));
        setTableClickHandler(new TableDialog(context));
    }

    public void setMarkdown(@NonNull String html) {
        setMarkdown(html, null);
    }

    public void setMarkdown(@RawRes int resId) {
        setMarkdown(resId, null);
    }

    private void setMarkdown(@RawRes int resId, @Nullable Html.ImageGetter imageGetter) {
        InputStream inputStreamText = getContext().getResources().openRawResource(resId);
        setMarkdown(convertStreamToString(inputStreamText), imageGetter);
    }

    public void setMarkdown(@NonNull final String markdown, @Nullable final Html.ImageGetter imageGetter) {
        // Override tags to stop Html.fromHtml destroying some of them
        final String overridden = HtmlTagHandler.overrideTags(Markdown.parseMD(markdown));
        final HtmlTagHandler htmlTagHandler = new HtmlTagHandler(this,
                imageGetter,  mLinkHandler, mImageClickHandler, mCodeHandler, mTableHandler
        );
        final Spanned text;
        if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
            text = removeHtmlBottomPadding(
                    Html.fromHtml(overridden, Html.FROM_HTML_MODE_LEGACY, imageGetter,
                            htmlTagHandler
                    ));
        } else {
            text = removeHtmlBottomPadding(
                    Html.fromHtml(overridden, imageGetter, htmlTagHandler));
        }

        // Convert to a buffer to allow editing
        final SpannableString buffer = new SpannableString(text);

        //Add links for emails and web-urls
        TextUtils.addLinks(buffer);
        setText(buffer);
        if(!(getMovementMethod() instanceof ClickableMovementMethod)) {
            setMovementMethod(ClickableMovementMethod.getInstance());
        }
    }

    @NonNull
    private static String convertStreamToString(@NonNull InputStream is) {
        final Scanner s = new Scanner(is).useDelimiter("\\A");
        return s.hasNext() ? s.next() : "";
    }

    @Nullable
    private static Spanned removeHtmlBottomPadding(@Nullable Spanned text) {
        if(text == null) {
            return null;
        }

        while(text.length() > 0 && text.charAt(text.length() - 1) == '\n') {
            text = (Spanned) text.subSequence(0, text.length() - 1);
        }
        return text;
    }

    public boolean isEditing() {
        return mIsEditing;
    }

    public void enableEditing() {
        if(mIsEditing) return;
        setFocusable(true);
        setFocusableInTouchMode(true);
        setCursorVisible(true);
        setEnabled(true);
        mIsEditing = true;
    }

    public void disableEditing() {
        if(!mIsEditing) return;
        setFocusable(false);
        setCursorVisible(false);
        mIsEditing = false;
    }

    public void saveText() {
        mSavedText = getText();
    }

    public void restoreText() {
        setText(mSavedText);
    }

    public Editable getInputText() {
        return mIsEditing ? getText() : mSavedText;
    }

    @Override
    public boolean isSuggestionsEnabled() {
        return mIsEditing;
    }

}

```

As before, the ```CodeSpan``` and ```TableSpan``` are initialised, and the default handlers are set.
The ```MarkdownEditText``` then removes padding on either side of it, as the spans should be shown across the entire canvas and have no access to the padding information.

As before, markdown can be set from a string or a resource id, with or without an ```ImageGetter```. 
As text is expected to change, the ```MarkdownEditText``` does not support caching the parsed spans.

In order to support two text states, the ```MarkdownTextView``` contains an editing flag, and an ```Editable``` containing any saved text.

```saveText``` saves the current text to this ```Editable```, and ```restoreText``` restores it.

The ```getInputText``` method is used to always return the text being edited, rather than the parsed form.

```enableEditing``` and ```disableEditing``` do as their names suggest, enabling or disable the cursor and focusing, and setting the mIsEditing flag.

#### Tag handling

Androids' ```Html.fromHtml``` method parses simple Html to a ```Spanned``` object.
The method supports the following tags:
- bold
- big
- blockquote
- break
- cite
- define
- emphasis
- font color and face
- headers
- italics
- paragraphs
- small
- strong
- subscript
- superscript
- underline
- href

Most of these spans only modify the ```TextPaint``` to draw text with a certain size, colour or styling.

In order to implement the spans rewuired for GitHub flavoured markdown, some of these tags must be overriden, and others implemented.

Overriden tags-
- unordered list
- ordered list
- list item
- blockquote
- href
- font
- image

Implemented tags
- table
- table row
- table header
- table data
- code
- strikethrough
- inlinecode

##### Overriding tags

Only tags which are not recognised are delegated to the ```TagHandler```.
In order to capture these tags they must be overriden prior to parsing.

This is done with the ```TextUtils``` replace method implemented earlier.

``` java
private static final String UNORDERED_LIST_TAG = "ESCAPED_UL_TAG";
private static final String ORDERED_LIST_TAG = "ESCAPED_OL_TAG";
private static final String LIST_ITEM_TAG = "ESCAPED_LI_TAG";
private static final String BLOCKQUOTE_TAG = "ESCAPED_BLOCKQUOTE_TAG";
private static final String A_TAG = "ESCAPED_A_TAG";
private static final String FONT_TAG = "ESCAPED_FONT_TAG";
private static final String IMAGE_TAG = "ESCAPED_IMG_TAG";

private static final Map<String, String> ESCAPE_MAP = new HashMap<>();

static {
    ESCAPE_MAP.put("<ul", "<" + UNORDERED_LIST_TAG);
    ESCAPE_MAP.put("</ul>", "</" + UNORDERED_LIST_TAG + ">");
    ESCAPE_MAP.put("<ol", "<" + ORDERED_LIST_TAG);
    ESCAPE_MAP.put("</ol>", "</" + ORDERED_LIST_TAG + ">");
    ESCAPE_MAP.put("<li", "<" + LIST_ITEM_TAG);
    ESCAPE_MAP.put("</li>", "</" + LIST_ITEM_TAG + ">");
    ESCAPE_MAP.put("<blockquote>", "<" + BLOCKQUOTE_TAG + ">");
    ESCAPE_MAP.put("</blockquote>", "</" + BLOCKQUOTE_TAG + ">");
    ESCAPE_MAP.put("<a", "<" + A_TAG);
    ESCAPE_MAP.put("</a>", "</" + A_TAG + ">");
    ESCAPE_MAP.put("<font", "<" + FONT_TAG);
    ESCAPE_MAP.put("</font>", "</" + FONT_TAG + ">");
    ESCAPE_MAP.put("<img", "<" + IMAGE_TAG);
    ESCAPE_MAP.put("</img>", "</" + IMAGE_TAG + ">");
}

private static final Pattern ESCAPE_PATTERN = TextUtils.generatePattern(ESCAPE_MAP.keySet());

public static String overrideTags(@Nullable String html) {
    return TextUtils.replace(html, ESCAPE_MAP, ESCAPE_PATTERN);
}
```

#### Handlers

The ```HtmlTagHandler``` is passed the handlers which are required for each of the span types.
It is also passed the ```TextView``` itself, which is required for measuring indentations.

**HtmlTagHandler.java**
``` java
public HtmlTagHandler(TextView tv, Html.ImageGetter imageGetter,
                          @Nullable LinkClickHandler linkHandler,
                          @Nullable ImageClickHandler imageClickHandler,
                          @Nullable CodeClickHandler codeHandler,
                          @Nullable TableClickHandler tableHandler) {
        mTextPaint = tv.getPaint();
        mSingleIndent = (int) mTextPaint.measureText("t");
        mImageGetter = imageGetter;
        mImageClickHandler = imageClickHandler;
        mLinkHandler = linkHandler;
        mCodeHandler = codeHandler;
        mTableHandler = tableHandler;
    }
```

#### Tag opening and closing

Each tag is delegated to the ```HtmlTagHandler``` through the ```handleTag``` method, which has the parameters ```boolean opening, String tag, Editable output, XMLReader xmlReader```.
If ```opening``` is true, the tag, output and xmlReader are passed to ```handleOpeningTag```. Otherwise the tag and output are passed to ```handleClosingTag```.

**HtmlTagHandler.java**
``` java
public void handleTag(final boolean opening, final String tag, Editable output, final XMLReader xmlReader) {
        if(opening) {
            handleOpeningTag(tag, output, xmlReader);
        } else {
            handleClosingTag(tag, output);
        }
        storeTableTags(opening, tag);
    }
```

```handleOpeningTag``` switches the tag to begin the correct span type in the ```Editable```

**HtmlTagHandler.java**
``` java
private void handleOpeningTag(final String tag, Editable output, final XMLReader xmlReader) {
        switch(tag.toUpperCase()) {
            case UNORDERED_LIST_TAG:
                mLists.push(
                        Triple.create(
                                tag,
                                safelyParseBoolean(
                                        getAttribute("bulleted", xmlReader, "true"), true
                                ),
                                ListNumberSpan.ListType.NUMBER
                        )
                );
                break;
            case ORDERED_LIST_TAG:
                final ListNumberSpan.ListType type = ListNumberSpan.ListType
                        .fromString(getAttribute("type", xmlReader, ""));
                mLists.push(
                        Triple.create(
                                tag,
                                safelyParseBoolean(
                                        getAttribute("numbered", xmlReader, "true"), true
                                ),
                                type
                        )
                );
                mOlIndices.push(Pair.create(1, type));
                break;
            case LIST_ITEM_TAG:
                if(output.length() > 0 && output.charAt(output.length() - 1) != '\n') {
                    output.append("\n");
                }
                if(!mLists.isEmpty()) {
                    final String parentList = mLists.peek().first;
                    if(parentList.equalsIgnoreCase(ORDERED_LIST_TAG)) {
                        start(output, new Ol());
                        mOlIndices.push(Pair.create(mOlIndices.pop().first + 1, mLists.peek().third));
                    } else if(parentList.equalsIgnoreCase(UNORDERED_LIST_TAG)) {
                        start(output, new Ul());
                    }
                } else {
                    start(output, new Ol());
                    mOlIndices.push(Pair.create(1, ListNumberSpan.ListType.NUMBER));
                }
                break;
            case "TABLE":
                start(output, new Table());
                if(mTableLevel == 0) {
                    mTableHtmlBuilder = new StringBuilder();
                }
                mTableLevel++;
                break;
            case FONT_TAG:
                final String font = getAttribute("face", xmlReader, "");
                final String fgColor = getAttribute("color", xmlReader, "");
                final String bgColor = getAttribute("background-color", xmlReader, "");
                final boolean rounded = safelyParseBoolean(getAttribute("rounded", xmlReader, ""),
                        false
                );
                if(font != null && !font.isEmpty()) {
                    start(output, new Font(font));
                }
                if(fgColor != null && !fgColor.isEmpty()) {
                    start(output, new ForegroundColor(fgColor));
                }
                if(bgColor != null && !bgColor.isEmpty()) {
                    start(output, new BackgroundColor(bgColor, rounded));
                }
                break;
            case "CODE":
                start(output, new Code());
                break;
            case "CENTER":
                start(output, new Center());
                break;
            case "S":
            case "STRIKE":
                start(output, new StrikeThrough());
                break;
            case "TR":
                start(output, new Tr());
                break;
            case "TH":
                start(output, new Th());
                break;
            case "TD":
                start(output, new Td());
                break;
            case "HR":
                start(output, new HorizontalRule());
                break;
            case BLOCKQUOTE_TAG:
                start(output, new BlockQuote());
                break;
            case A_TAG:
                start(output, new A(getAttribute("href", xmlReader, "invalid_url")));
                break;
            case IMAGE_TAG:
                handleImageTag(output, getAttribute("src", xmlReader, ""));
                break;
            case "INLINECODE":
                start(output, new InlineCode());
                break;

        }
    }
```

After the tag has been handled, any table tags are stored with ```storeTableTags```.
This checks if the current table depth is greater than 0, or the tag is "table".
If so, the opening bracket is added to the mTableHtmlBuilder ```StringBuilder```, along with the closing forward slash if the tag is being closed.
The tag is then appended and closed.

**HtmlTagHandler.java**
``` java
private void storeTableTags(boolean opening, String tag) {
        if(mTableLevel > 0 || tag.equalsIgnoreCase("table")) {
            mTableHtmlBuilder.append("<");
            if(!opening) {
                mTableHtmlBuilder.append("/");
            }
            mTableHtmlBuilder
                    .append(tag.toLowerCase())
                    .append(">");
        }
    }
```

This builds the HTML for a table so that it can be displayed later.

#### Span opening and closing

The ```TagHandler``` works by placing MARK spans in the ```Editable```.
A MARK span is a span which must be removed later and acts as a placeholder for a new span.

**HtmlTagHandler.java**
``` java
private void start(Editable output, Object mark) {
        final int point = output.length();
        output.setSpan(mark, point, point, Spannable.SPAN_MARK_MARK);
    }
```

The MARK span has 0 length and is placed at the end of the ```Editable```.

**HtmlTagHandler.java**
``` java
private void end(Editable output, Class kind, boolean paragraphStyle, Object... replaces) {
        final Object obj = getLast(output, kind);
        final int start = output.getSpanStart(obj);
        final int end = output.length();

        // If we're in a table, then we need to store the raw HTML for later
        if(mTableLevel > 0) {
            mTableHtmlBuilder.append(extractSpanText(output, kind));
        }

        output.removeSpan(obj);
        if(start != end) {
            int len = end;
            // paragraph styles like AlignmentSpan need to end with a new line!
            if(paragraphStyle) {
                output.append("\n");
                len++;
            }
            for(Object replace : replaces) {
                if(output.length() > 0) {
                    output.setSpan(replace, start, len, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
                }
            }
        }
    }
```

When a tag is ended it must be replaced by one or more spans, or removed.

The span is extracted with ```getLast```

**HtmlTagHandler.java**
``` java
private static <T> T getLast(Editable text, Class<T> kind) {
        final T[] objs = text.getSpans(0, text.length(), kind);
        if(objs.length == 0) {
            return null;
        } else {
            //In reverse as items are returned in order they were inserted
            for(int i = objs.length; i > 0; i--) {
                if(text.getSpanFlags(objs[i - 1]) == Spannable.SPAN_MARK_MARK) {
                    return objs[i - 1];
                }
            }
            return null;
        }
    }
```

This finds all of the objects of the given class and iterates backwards through them searching for a span with the MARK flags.

If the tag is within a table, the text contained within it is extracted and added to the table ```StringBuilder```.

The span start and end positions are then stored and it is removed from the ```Editable```.

If the span is of non-zero length, it is replaced.
If the span is an instance of ```ParagraphStyle```, a newline must be added after it.
Once this check has ben completed, each of the spans in the replaces array are place over the position which was previously occupied by the MARK span.

#### Span clases

Each span has its own marker class, which is used to extract it from the ```Editable```

``` java
private static class Ul {

}

private static class Ol {
}

private static class Code {
}

private static class Center {
}

private static class StrikeThrough {
}

private static class Table {
}

private static class Tr {
}

private static class Th {
}

private static class Td {
}

private static class HorizontalRule {
}

private static class BlockQuote {
}

private static class InlineCode {
}

private static class A {

    String href;

    A(String href) {
        this.href = href;
    }

}

private static class ForegroundColor {
    String color;

    ForegroundColor(String color) {
        this.color = color;
    }

}

private static class BackgroundColor {
    String color;
    boolean rounded;

    BackgroundColor(String color, boolean rounded) {
        this.color = color;
        this.rounded = rounded;
    }
}

private static class Font {
    String face;

    Font(String face) {
        this.face = face;
    }
}
```

#### Attribute extraction

Unfortunately, the ```TagHandler``` has no direct access to the attributes of the tags that are passed to it.
However, it is able to access them when the tag is being opened.

This is done with ```getAttribute```

**HtmlTagHandler.java**
``` java
private static String getAttribute(@NonNull String attr, @NonNull XMLReader reader, String defaultAttr) {
        try {
            final Field fElement = reader.getClass().getDeclaredField("theNewElement");
            fElement.setAccessible(true);
            final Object element = fElement.get(reader);
            final Field fAtts = element.getClass().getDeclaredField("theAtts");
            fAtts.setAccessible(true);
            final Object attrs = fAtts.get(element);
            final Field fData = attrs.getClass().getDeclaredField("data");
            fData.setAccessible(true);
            final String[] data = (String[]) fData.get(attrs);
            final Field fLength = attrs.getClass().getDeclaredField("length");
            fLength.setAccessible(true);
            final int len = (Integer) fLength.get(attrs);
            for(int i = 0; i < len; i++) {
                if(attr.equals(data[i * 5 + 1])) {
                    return data[i * 5 + 4];
                }
            }

        } catch(Exception e) {
            Log.e(TAG, "handleTag: ", e);
        }
        return defaultAttr;
    }
```

This method uses reflection to attempt to extract the attribute from the ```XMLReader```.

First the element field is collected, made accessible and accessed from the ```XMLReader```.
Next the attributes field is collected, made accessible and accessed from the element.
Next the data field is collected, made accessible and accessed from the attributes.

The ```data``` field is a string array containing the attribute names, types, and values.

A href to a user might appear as [, href, href, CDATA, https://github.com/user, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null]

while a font tag with a colour and background colour may appear as [, background-color, background-color, CDATA, navy, , color, color, CDATA, red, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null]

Once the attribute is first found in the array, the next item will be tha attribute again, the item after will be the data type, and the item after that will be the actual data.

The length value extracted from the attrs object is the actual number of attributes present.

The loop iterates through every 5th item, beginning at index 1, as index 0 is always empty.
If the the index contains the tag, the value 4 positions after is returned.

#### List tags

List tags and numberings are stored in two stacks.
```mLists``` is a stack of triples containing the tag, whether the list is bulleted, and the list type.
```mOlIndices``` is a stack of pairs of integers and ```ListTypes``` which is used when exiting a nested span to continue the previous lists' numbering.

Note:
The ```Triple``` is a generic class like ```Pair``` which holds three generic types.

``` java
private static class Triple<T, U, V> {

    T first;
    U second;
    V third;

    Triple(T t, U u, V v) {
        first = t;
        second = u;
        third = v;
    }

    static <T, U, V> Triple<T, U, V> create(T t, U u, V v) {
        return new Triple<>(t, u, v);
    }

}
```

##### Unordered list opening
If the tag is an unordered list tag, a new ```Triple``` is pushed to the stack, containing the tag, the "bulleted" attribute used to specify whether bullets should be shown, and the
NUMBER ```ListType```, although it will not be used.


##### Ordered list opening

If the tag is an ordered list tag, a new ```Triple``` is push to the stack, containing the tag, the numbered attribute, and the ```ListType``` found from the string value of the "type" attribute.

An index of 1 and the ```ListType``` are then push to the mOlIndices stack.

##### List item opening

If the tag is a list tag, the last character of the output is checked to ensure that it is a newline, otherwise the line will not wrap.

If mLists is not empty, the parent list tag is checked.
If it is an ordered list, the ```Ol``` span is started, and a ```Pair``` containing the top index in the stack plus one, and the parent ```ListType``` is pushed to the stack.

If it is an unordered list, the ```Ul``` span is started

If mLists is empty, this means that there is list item without a parent.
A new ```Ol``` span is started, and the new index is pushed to mOlIndices.

##### Unordered list closing

mLists is popped, removing the ```Ul``` tag.

##### Ordered list closing

mLists and mOlIndices are popped, removing the ```Ol``` tag and its index.

##### List item closing

If mLists is non-empty the parent tag is checked.
If it is an unordered list tag, the ```Editable``` is passed to ```handleULTag```.

**HtmlTagHandler.java**
``` java
private void handleULTag(Editable output) {
        if(output.length() > 0 && output.charAt(output.length() - 1) != '\n') {
            output.append("\n");
        }

        if(mLists.peek().second) {
            //Check for checkboxes
            if(output.length() > 2 &&
                    ((output.charAt(0) >= '\u2610' && output.charAt(0) <= '\u2612')
                            || (output.charAt(1) >= '\u2610' && output
                            .charAt(1) <= '\u2612')
                    )) {
                end(output, Ul.class, false,
                        new LeadingMarginSpan.Standard(
                                mListIndent * (mLists.size() - 1))
                );
            } else {
                end(output, Ul.class, false,
                        new LeadingMarginSpan.Standard(
                                mListIndent * (mLists.size() - 1)),
                        new BulletSpan(mSingleIndent)
                );
            }
        } else {
            end(output, Ul.class, false,
                    new LeadingMarginSpan.Standard(mListIndent * (mLists.size() - 1))
            );
        }
    }
```

This method checks the last character, ensuring that the tag ends in a newline.
It then checks if the list should have bullets, if not the tag is ended with a new ```LeadingMarginSpan``` proportional to the list size.

If the unordered list should have bullets, the bullets can either be bullet points or checkboxes.

If the ```Editable``` is sufficiently long for the character to be there, the output is checked to see whether the first or second character is within the range of the checkbox characters.
If it is, a ```LeadingMarginSpan``` is applied.
Otherwise, a ```LeadingMarginSpan``` and a ```BulletSpan``` are applied.

#### Table tags

##### Table tag opening

When a table tag is opened, the ```Table``` span is started.
If the table depth is 0, the ```StringBuilder``` is recreated.
The table level is then incremented.

##### Table tag closing

A closing table tag is passed to ```handleTableTag```

**HtmlTagHandler.java**
``` java
private void handleTableTag(Editable output) {
        mTableLevel--;
        // When we're back at the root-level table
        if(mTableLevel == 0) {
            final Table obj = getLast(output, Table.class);
            final int start = output.getSpanStart(obj);
            output.removeSpan(obj); //Remove the old span
            output.insert(start, "\n \n");
            final TableSpan table = new TableSpan(mTableHtmlBuilder.toString(), mTableHandler);
            output.setSpan(table, start, start + 2, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
            output.setSpan(new WrappingClickableSpan(table), start, start + 3,
                    Spanned.SPAN_EXCLUSIVE_EXCLUSIVE
            );

        } else {
            end(output, Table.class, false);
        }
    }
```

The table level is decremented, as we have just closed a table tag.

If the table level is 0, the last ```Table``` is found, its start position stored, and the span removed.
Two newlines separated by a space are then inserted, making space for the ```TableSpan```.
The ```TableSpan``` is created with the value of the table ```StringBuilder```, and the ```TableClickHandler```.
The span is inserted, and then a ```WrappingClickableSpan``` is inserted around the ```TableSpan```.

If the table level is not 0, the ```Table``` span is closed.

#### Font tags

##### Font tag opening

When a font tag is opened, four attributes are extracted.
These are the font, the text colour, the background colour, and whether the background colour should be rounded.

If the font is non-null and non-empty, a new ```Font``` span is started with the ```Font```.

If the foreground colour is non-null and non-empty, a new ```ForegroundColor``` span is started with the font colour.

If the background colour is non-null and non-empty, a new ```BackgroundColor``` span is started with the background colour, and whether it should be rounded.

##### Font tag closing

A closing font tag is passed to ```handleFontTag```

**HtmlTagHandler.java**
``` java
private void handleFontTag(Editable output) {
        final ForegroundColor fgc = getLast(output, ForegroundColor.class);
        final BackgroundColor bgc = getLast(output, BackgroundColor.class);
        final Font f = getLast(output, Font.class);
        if(fgc != null) {
            final int start = output.getSpanStart(fgc);
            final int end = output.length();
            output.removeSpan(fgc);
            output.setSpan(new ForegroundColorSpan(safelyParseColor(fgc.color)), start, end,
                    Spannable.SPAN_EXCLUSIVE_EXCLUSIVE
            );
        }
        if(bgc != null) {
            final int start = output.getSpanStart(bgc);
            final int end = output.length();
            output.removeSpan(bgc);

            final int color = safelyParseColor(bgc.color);
            if(bgc.rounded) {
                output.insert(end, "\u00A0");
                output.insert(start, "\u00A0");
                output.setSpan(new RoundedBackgroundEndSpan(color, false), start, start + 1,
                        Spanned.SPAN_INCLUSIVE_EXCLUSIVE
                );
                output.setSpan(new RoundedBackgroundEndSpan(color, true), end, end + 1,
                        Spanned.SPAN_EXCLUSIVE_INCLUSIVE
                );
                output.setSpan(new BackgroundColorSpan(color), start + 1, end,
                        Spannable.SPAN_INCLUSIVE_INCLUSIVE
                );
            } else {
                output.setSpan(new BackgroundColorSpan(color), start, end,
                        Spannable.SPAN_EXCLUSIVE_EXCLUSIVE
                );
            }

        }
        if(f != null) {
            final int start = output.getSpanStart(f);
            final int end = output.length();
            output.removeSpan(f);
            output.setSpan(new TypefaceSpan(f.face), start, end,
                    Spannable.SPAN_EXCLUSIVE_EXCLUSIVE
            );
        }
    }
```

This extracts the spans for each of the the three possible attributes.

If the ```ForegroundColor``` is non null, the start and end are stored, the span is removed, and a ```ForegroundColorSpan``` is set with the parsed color of the the ```ForegroundColor```
span.

The colour is parsed using ```safelyParseColor```.

**HtmlTagHandler.java**
``` java
private int safelyParseColor(String color) {
        try {
            return Color.parseColor(color);
        } catch(Exception e) {
            switch(color) {
                case "black":
                    return Color.BLACK;
                case "white":
                    return Color.WHITE;
                case "red":
                    return Color.RED;
                case "blue":
                    return Color.BLUE;
                case "green":
                    return Color.GREEN;
                case "grey":
                    return Color.GRAY;
                case "yellow":
                    return Color.YELLOW;
                case "aqua":
                    return 0xff00ffff;
                case "fuchsia":
                    return 0xffff00ff;
                case "lime":
                    return 0xff00ff00;
                case "maroon":
                    return 0xff800000;
                case "navy":
                    return 0xffff00ff;
                case "olive":
                    return 0xff808000;
                case "purple":
                    return 0xff800080;
                case "silver":
                    return 0xffc0c0c0;
                case "teal":
                    return 0xff008080;
                default:
                    return mTextPaint.getColor();
            }
        }
    }
```

The method attempts to parse the color with ```Color.parseColor```. If this fails, it switches the string across the different HTML colour values, before returning the
```TextPaint``` colour if a colour is not matched.

If the ```BackgroundColor``` is non-null, its positions are saved and it is removed.
The background colour is then parsed, and if the span should be rounded, no break space characters are inserted around the span, before two ```RoundedBackgroundEndSpans``` are inserted
around a ```BackgroundColorSpan```.
Otherwise, only the ```BackgroundColorSpan``` is inserted.

If the ```Font``` span is non null, its positions are saved and it is removed.
A ```TypeFaceSpan``` is then inserted across its previous range.

#### Code tags

A code tag is opened by starting a new ```Code``` span.

A code tag is closed with ```handleCodeTag```

**HtmlTagHandler.java**
``` java
private void handleCodeTag(Editable output) {
        final Object obj = getLast(output, Code.class);
        final int start = output.getSpanStart(obj);
        final int end = output.length();
        if(end > start + 1) {
            final CharSequence chars = extractSpanText(output, Code.class);
            output.removeSpan(obj);
            output.insert(start, "\n \n"); // Another line for our CodeSpan to cover
            final CodeSpan code = new CodeSpan(chars.toString(), mCodeHandler);
            output.setSpan(code, start, start + 2, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);
            output.setSpan(new WrappingClickableSpan(code), start, start + 3,
                    Spannable.SPAN_EXCLUSIVE_EXCLUSIVE
            );
        }
    }
```

The tag is extracted and its position saved.
If the span has a length greater than 1, the ```CodeSpan``` is to be inserted.

First the text is extracted with ```extractSpanText```.

**HtmlTagHandler.java**
``` java
private CharSequence extractSpanText(Editable output, Class kind) {
        final Object obj = getLast(output, kind);
        final int start = output.getSpanStart(obj);
        final int end = output.length();

        final CharSequence extractedSpanText = output.subSequence(start, end);
        output.delete(start, end);
        return extractedSpanText;
    }
```

This method finds the span, captures the subsequence from the ```Editable```, removes it, and then returns the extracted ```CharSequence```.

Once the ```CharSequence``` has been extracted, spacing for the ```CodeSpan``` is inserted, the ```CodeSpan``` itself is inserted, and a ```WrappingClickableSpan``` is inserted around
it.

#### Center tags

Center tags are opened by starting a ```Center``` span .

They are closed by ending the ```Center``` span with an ```AlignmentSpan``` with ```Layout.Alignment.ALIGN_CENTER```.

#### Strikethrough tags

Strikethrough tags are opened by starting a ```StrikeThrough``` span.

They are closed by ending the ```StrikeThroughSpan``` with a ```StrikeThroughSpan```.

#### Table row, header, and data

Table row, header, and data tags are started with ```Tr```, ```Th```, and ```Td``` spans respectively, and ended with these spans.

#### Horizontal rule tags

Horizontal rule tags are opened by starting a new ```HorizontalRule``` span.

They are ended with ```handleHorizontalRuleTag```

**HtmlTagHandler.java**
``` java
private void handleHorizontalRuleTag(Editable output) {
        final Object obj = getLast(output, HorizontalRule.class);
        final int start = output.getSpanStart(obj);
        output.removeSpan(obj); //Remove the old span
        output.replace(start, output.length(), " "); //We need a non-empty span
        output.setSpan(new HorizontalRuleSpan(), start, start + 1, 0); //Insert the bar span
    }
```

This finds the ```HorizontalRule``` span, stores its length, removes the span, inserts a space for the ```HorizontalRuleSpan``` to occupy, and then inserts the ```HorizontalRuleSpan```.

#### Blockquote tags

Blockquote tags are opened by starting a new ```BlockQuote``` span.

They are ended with ```handleBlockQuoteTag```

This finds the ```BlockQuote``` span, stores its start and end positions, removes the ```BlockQuote``` span, and inserts a ```QuoteSpan``` across these indices.

**HtmlTagHandler.java**
``` java
private void handleBlockQuoteTag(Editable output) {
        final Object obj = getLast(output, BlockQuote.class);
        final int start = output.getSpanStart(obj);
        final int end = output.length();
        output.removeSpan(obj);
        output.setSpan(new QuoteSpan(), start, end, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);
    }
```

#### A tags

A tags are opened by starting a new ```A``` tag with the extracted "href" attribute.

They are ended with ```handleATag```

**HtmlTagHandler.java**
``` java
private void handleATag(Editable output) {
        final A obj = getLast(output, A.class);
        final int start = output.getSpanStart(obj);
        final int end = output.length();
        output.removeSpan(obj);
        if(isValidURL(obj.href)) {
            output.setSpan(new CleanURLSpan(obj.href, mLinkHandler), start, end,
                    Spannable.SPAN_INCLUSIVE_EXCLUSIVE
            );
        } else {
            output.insert(start, obj.href.concat(" "));
        }
    }
```

This finds and removes the span.
It then checks if the href is valid.
If it is valid, a ```CleanURLSpan``` is inserted.
If it is not valid, the href is inserted before the a tag text.

#### Image tags

Image tags are handled immediately with ```handleImageTag```.

The ```Drawable``` is initially constructed as a transparent ```ColorDrawable```.
If the ```ImageGetter``` is non-null, the drawable is loaded from it.

The original length is stored, before an object replacement character, &#65532;, is inserted.

The ```ClickableImageSpan``` is then created, inserted, and wrapped with a ```WrappingClickableSpan```.

As the spans are inserted over the object replacement character, they will draw over it once they have been loaded.

As no MARK span is inserted, it does not need to be (and obviously cannot) be removed.

#### InlineCode tags

Inline code tags are started with ```InlineCode``` spans.

They are closed with ```handleInlineCodeTag```

**HtmlTagHandler.java**
``` java
private void handleInlineCodeTag(Editable output) {
        final Object obj = getLast(output, InlineCode.class);
        final int start = output.getSpanStart(obj);
        final int end = output.length();
        output.removeSpan(obj);
        output.setSpan(new InlineCodeSpan(mTextPaint.getTextSize()), start, end,
                Spannable.SPAN_INCLUSIVE_EXCLUSIVE
        );
    }
```

This replaces the ```InlineCode``` span with an ```InlineCodeSpan```, passing the correct text size.

<div style="page-break-after: always;"></div>

## Markdown editing

Now that markdown parsing has been implemented, markdown editing can be implemented. 

The first order objectives under objective 10 were to, "Implement toggling of a text editor between raw markdown and formatted markdown", "Add utility buttons for markdown features", and
"Add utility buttons for markdown features".

As each of these objectives are closely linked, being part of the same UI element, it makes sense to implement them together.

### Implementing a re-usable editor

The markdown editor will be used in numerous sections of the app.
In some cases such as editing project cards it needs only allow the input and preview of a single block of markdown content, whereas in others such as editing an Issue it must also be 
able to facilitate editing aspects of the model that it is working with, such as assignees and tags.

This can be achieved by using a single primary layout, containing the control elements for the editor, and a stub to contain the elements related to the particular model being edited.

**activity_markdown_editor.xml**
``` xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
                android:orientation="vertical"
                android:layout_width="match_parent"
                android:layout_height="match_parent">

    <LinearLayout
        android:id="@+id/markdown_activity_buttons"
        android:layout_width="match_parent"
        android:layout_height="?android:attr/actionBarSize"
        android:layout_alignParentTop="true"
        android:orientation="horizontal"
        android:background="@color/colorPrimary"
        android:elevation="2dp"
        android:baselineAligned="false">

        <FrameLayout
            android:layout_width="0dp"
            android:layout_weight="0.5"
            android:layout_height="wrap_content"
            android:layout_gravity="center_vertical">

            <Button
                android:id="@+id/markdown_editor_discard"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="center_horizontal"
                android:text="@string/action_discard"
                android:drawableStart="@drawable/ic_cancel"
                android:background="?android:attr/selectableItemBackground"
                style="@style/Widget.AppCompat.Button.Borderless"/>

        </FrameLayout>

        <FrameLayout
            android:layout_width="0dp"
            android:layout_weight="0.5"
            android:layout_height="wrap_content"
            android:layout_gravity="center_vertical">

            <Button
                android:id="@+id/markdown_editor_done"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="center_horizontal"
                android:text="@string/action_done"
                android:drawableStart="@drawable/ic_done"
                android:background="?android:attr/selectableItemBackground"
                style="@style/Widget.AppCompat.Button.Borderless"/>

        </FrameLayout>

    </LinearLayout>

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_below="@+id/markdown_activity_buttons"
        android:layout_above="@+id/markdown_edit_scrollview"
        android:fillViewport="true">

        <ViewStub
            android:id="@+id/editor_stub"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"/>

    </ScrollView>

    <HorizontalScrollView
        android:id="@+id/markdown_edit_scrollview"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:scrollbars="none"
        android:layout_marginTop="8dp"
        android:minHeight="48dp"
        android:background="@color/cardview_dark_background">

        <LinearLayout
            android:id="@+id/markdown_edit_buttons"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:orientation="horizontal">

        </LinearLayout>

    </HorizontalScrollView>

</RelativeLayout>
```

The markdown_editor layout contains three children.

![Markdown editor layout](http://imgur.com/L0yRDAr.png)

In vertical order, from top to bottom:

The first child contains the navigation buttons for the entire editor, allowing the edit action to be completed or discarded.

The second child is the content layout. The ```ScrollView``` wraps a ```ViewStub``` which will be inflated to display the editor layout.

The third and final child is a ```HorizontalScrollView``` containing a ```LinearLayout``` which will be used to display the list of editor action buttons.

#### The EditorActivity

**EditorActivity.java**
``` java
package com.tpb.projects.editors;

import android.app.ProgressDialog;
import android.content.Intent;
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.net.Uri;
import android.os.AsyncTask;
import android.os.Environment;
import android.os.Handler;
import android.os.Looper;
import android.os.ParcelFileDescriptor;
import android.provider.MediaStore;
import android.support.v4.content.FileProvider;
import android.support.v7.app.AlertDialog;
import android.support.v7.app.AppCompatActivity;
import android.util.Base64;
import android.util.Log;
import android.widget.EditText;
import android.widget.LinearLayout;
import android.widget.Toast;

import com.androidnetworking.error.ANError;
import com.tpb.github.data.Uploader;
import com.tpb.projects.BuildConfig;
import com.tpb.projects.R;
import com.tpb.projects.common.CircularRevealActivity;
import com.tpb.projects.util.UI;

import java.io.ByteArrayOutputStream;
import java.io.File;
import java.io.FileDescriptor;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * Created by theo on 16/02/17.
 */

public abstract class EditorActivity extends CircularRevealActivity {
    private static final String TAG = EditorActivity.class.getSimpleName();

    private static final int REQUEST_CAMERA = 9403; //Random request codes
    private static final int SELECT_FILE = 6113;
    private String mCurrentFilePath;
    protected ProgressDialog mUploadDialog;


    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if(resultCode == AppCompatActivity.RESULT_OK) {
            if(requestCode == EmojiActivity.REQUEST_CODE_CHOOSE_EMOJI) {
                emojiChosen(data.getStringExtra(getString(R.string.intent_emoji)));
            } else if(requestCode == CharacterActivity.REQUEST_CODE_INSERT_CHARACTER) {
                insertString(data.getStringExtra(getString(R.string.intent_character)));
            } else {
                final ProgressDialog pd = new ProgressDialog(this);
                pd.setCanceledOnTouchOutside(false);
                pd.setCancelable(false);
                if(requestCode == REQUEST_CAMERA) {

                    pd.setTitle(R.string.title_image_conversion);
                    pd.show();
                    AsyncTask.execute(() -> { // Execute asynchronously
                        final Bitmap image = BitmapFactory.decodeFile(mCurrentFilePath);
                        final ByteArrayOutputStream stream = new ByteArrayOutputStream();
                        image.compress(Bitmap.CompressFormat.PNG, 100, stream);
                        pd.cancel();
                        uploadImage(Base64.encodeToString(stream.toByteArray(), Base64.DEFAULT));
                    });

                } else if(requestCode == SELECT_FILE) {
                    final Uri selectedFile = data.getData();
                    pd.setTitle(R.string.title_image_conversion);
                    pd.show();
                    AsyncTask.execute(() -> {
                        try {
                            final String image = attemptLoadImage(selectedFile);
                            pd.cancel();
                            uploadImage(image);
                        } catch(IOException ioe) {
                            pd.cancel();
                            imageLoadException(ioe);
                        }
                    });
                }
            }
        }
    }

    abstract void imageLoadComplete(String url);

    abstract void imageLoadException(IOException ioe);

    void showImageUploadDialog() {
        final CharSequence[] items = {
                getString(R.string.text_take_a_picture),
                getString(R.string.text_choose_from_gallery),
                getString(R.string.text_insert_image_link),
                getString(R.string.action_cancel)
        };
        final AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.setTitle(getString(R.string.text_upload_an_image));
        builder.setItems(items, (dialog, which) -> {
            if(which == 3) {
                dialog.dismiss();
            } else if(which == 2) {
                displayImageLinkDialog();
            } else {
                if(mUploadDialog == null) {
                    mUploadDialog = new ProgressDialog(EditorActivity.this);
                    mUploadDialog.setTitle(R.string.title_image_upload);
                    mUploadDialog.setProgressStyle(ProgressDialog.STYLE_HORIZONTAL);
                }
                if(which == 0) {
                    attemptTakePicture();
                } else if(which == 1) {
                    final Intent intent = new Intent(
                            Intent.ACTION_GET_CONTENT,
                            MediaStore.Images.Media.EXTERNAL_CONTENT_URI
                    );
                    intent.setType("image/*");
                    startActivityForResult(intent, SELECT_FILE);

                }
            }
        });
        builder.show();
    }

    private void displayImageLinkDialog() {
        final LinearLayout wrapper = new LinearLayout(this);
        wrapper.setOrientation(LinearLayout.VERTICAL);
        wrapper.setPaddingRelative(UI.pxFromDp(16), 0, UI.pxFromDp(16), 0);

        final EditText desc = new EditText(this);
        desc.setHint(R.string.hint_url_description);
        wrapper.addView(desc);

        final EditText url = new EditText(this);
        url.setHint(R.string.hint_url_url);
        wrapper.addView(url);

        final AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.setTitle(R.string.title_insert_image_link);
        builder.setView(wrapper);

        builder.setPositiveButton(R.string.action_insert, (v, di) -> {
            insertString(String.format(getString(R.string.text_image_link_with_desc),
                    desc.getText().toString(),
                    url.getText().toString()
            ));
        });
        builder.setNegativeButton(R.string.action_cancel, null);

        builder.create().show();
    }

    private void attemptTakePicture() {
        final Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
        // Check if there is an activity which can take a picture
        if(intent.resolveActivity(getPackageManager()) != null) {
            File photoFile = null;
            try {
                //Create the file for the image to be stored in
                photoFile = createImageFile();
            } catch(IOException ioe) {
                Log.e(TAG, "attemptTakePicture: ", ioe);
                imageLoadException(ioe);
            }

            if(photoFile != null) {
                final Uri photoURI = FileProvider
                        .getUriForFile(this, "com.tpb.projects.provider", photoFile);
                intent.putExtra(MediaStore.EXTRA_OUTPUT, photoURI);
                startActivityForResult(intent, REQUEST_CAMERA);
            } else {
                imageLoadException(
                        new IOException(getString(R.string.error_image_file_not_created)));
            }
        } else {
            Toast.makeText(this, R.string.error_no_application_for_picture, Toast.LENGTH_SHORT)
                 .show();
        }
    }

    private File createImageFile() throws IOException {
        //Create an image file with a formatted name
        final String timeStamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
        final String imageFileName = "JPEG_" + timeStamp + "_";
        final File storageDir = getExternalFilesDir(Environment.DIRECTORY_PICTURES);
        final File image = File.createTempFile(
                imageFileName,  /* prefix */
                ".jpg",         /* suffix */
                storageDir      /* directory */
        );
        mCurrentFilePath = image.getAbsolutePath();
        return image;
    }

    private String attemptLoadImage(Uri uri) throws IOException {
        // Open FileDescriptor in read mode
        final ParcelFileDescriptor parcelFileDescriptor =
                getContentResolver().openFileDescriptor(uri, "r");
        final FileDescriptor fileDescriptor = parcelFileDescriptor.getFileDescriptor();
        //Decode to a bitmap, and convert to a byte array
        final Bitmap image = BitmapFactory.decodeFileDescriptor(fileDescriptor);
        final ByteArrayOutputStream stream = new ByteArrayOutputStream();
        image.compress(Bitmap.CompressFormat.PNG, 100, stream);
        //Return base64 string for Imgur
        return Base64.encodeToString(stream.toByteArray(), Base64.DEFAULT);
    }

    private void uploadImage(String image64) {
        new Handler(Looper.getMainLooper()).postAtFrontOfQueue(() -> mUploadDialog.show());
        Uploader.uploadImage(
                new Uploader.ImgurUploadListener() {
                                 @Override
                                 public void imageUploaded(String link) {
                                     mUploadDialog.cancel();
                                     final String snippet = String.format(getString(R.string.text_image_link), link);
                                     imageLoadComplete(snippet);
                                 }

                                 @Override
                                 public void uploadError(ANError error) {
                                     mUploadDialog.cancel();
                                     Toast.makeText(EditorActivity.this, error.getErrorBody(), Toast.LENGTH_SHORT).show();
                                 }
                             },
                image64,
                (bUP, bTotal) -> mUploadDialog.setProgress(Math.round((100 * bUP) / bTotal)),
                BuildConfig.IMGUR_CLIENT_ID
        );
    }

    protected abstract void emojiChosen(String emoji);

    protected abstract void insertString(String c);

}

```

The ```EditorActivity``` is an abstract class dealing with the process for creating and uploading images, as well as inserting special characters and emojis.

##### Image uploading

Objective 10.iii.a is to implement a feature allowing the user to upload an image of their choosing to a hosting service, retrieve the URL for the image, and insert it into the ```EditText```.

I chose to use Imgur to host user images as it is established, free, sufficiently fast, and provides an hourly upload limit of 1250 images for authenticated clients which is more than 
sufficient.

A client ID and secret can be generated on the Imgur website for an app linked to any Imgur user account.

The image upload endpoint is simple to use, requiring only authentication and the image.

| Method | POST |
| --- | --- |
| Route | https://api.imgur.com/3/image |
| Alternative Route | https://api.imgur.com/3/upload |
| Response Model | Basic |

Parameters
| Key | Required | Description | 
| --- | --- | --- |
| image	| required | A binary file, base64 data, or a URL for an image. (up to 10MB) |
| album	| optional | The id of the album you want to add the image to. For anonymous albums, {album} should be the deletehash that is returned at creation. |
| type	| optional | The type of the file that's being sent; file, base64 or URL |
| name	| optional | The name of the file, this is automatically detected if uploading a file with a POST and multipart / form-data |
| title	| optional | The title of the image. |
| description | optional | The description of the image. |

Image uploading is handled with the ```Uploader```.

**Uploader.java**
``` java
package com.tpb.github.data;

import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.util.Log;

import com.androidnetworking.AndroidNetworking;
import com.androidnetworking.common.Priority;
import com.androidnetworking.error.ANError;
import com.androidnetworking.interfaces.JSONObjectRequestListener;
import com.androidnetworking.interfaces.UploadProgressListener;

import org.json.JSONObject;

/**
 * Created by theo on 15/02/17.
 */

public class Uploader {

    private static final String IMGUR_AUTH_KEY = "Authorization";
    private static final String IMGUR_AUTH_FORMAT = "Client-ID %1$s";

    public static void uploadImage(@NonNull final ImgurUploadListener listener, String image64, @Nullable UploadProgressListener uploadListener, @NonNull String clientId) {
        AndroidNetworking.upload("https://api.imgur.com/3/image")
                         .addHeaders(IMGUR_AUTH_KEY, String.format(IMGUR_AUTH_FORMAT,
                                 clientId
                         ))
                         .addMultipartParameter("image", image64)
                         .setPriority(Priority.HIGH)
                         .build()
                         .setUploadProgressListener(uploadListener)
                         .getAsJSONObject(new JSONObjectRequestListener() {
                             @Override
                             public void onResponse(JSONObject response) {
                                 try {
                                     final String link = response.getJSONObject("data").getString("link");
                                     listener.imageUploaded(link);
                                 } catch(Exception e) {
                                     Log.e("Uploader", "onResponse: ", e);
                                 }
                             }

                             @Override
                             public void onError(ANError anError) {
                                 listener.uploadError(anError);
                             }
                         });
    }

    public interface ImgurUploadListener {

        void imageUploaded(String url);

        void uploadError(ANError error);

    }

}

```

Given an ```ImgurUploadListener```, base 64 encoded image, an ```UploadProgressListener```, and the client id, ```uploadImage``` attempts to upload the image.

##### Image upload process

When the user requests to insert an image link, there are multiple sources from which they may wish to choose their image.

First, they may wish to take a new picture.
Second, they may wish to choose an image from their gallery.
Third, they may already have a link to an image.

###### Image source choice

When ```showUploadDialog``` is called, it creates a dialog to display these options.

When the user selects an action, the item selection listener triggers the appropriate action.

If the user clicks cancel, the dialog is dismissed.

###### Pre-existing image link

If the user has selected, "Insert image link", ```displayImageLinkDialog``` is called.

This shows a dialog containing two ```EditTexts```, one for the title and one for the description of the image.
If the user selects the "Insert" bbutton, their input is formatted and passed to ```insertString``` for the implementation of ```EditorActivity``` to deal with.

If the user has selected another option, the image will need to be uploaded, so the ```ProgressDialog``` mUploadDialog is created if it has not been already.
If the selected option is to take a picture, ```attemptTakePicture``` is called.

###### New image capture

This creates a new ```Intent``` with the ```MediaStore.ACTION_IMAGE_CAPTURE``` action.
The ```Activity``` for the ```Intent``` is then resolved.
If it is null, there is no camera app, or no camera, and an error message is shown.
If there is a camera, a new file must be created in which to store the image.

```createImageFile``` is called, which attempts to create a new image file in the system pictures directory, with a name in the form JPEG_yyyyMMdd_HHmmss.jpg, with the date format filled
in with the current time. This should guarantee a unique image file.

If the file is created successfully, it is stored, and the Uri is passed as the OUTPUT extra for the ```Intent```. Otherwise ```imageLoadException``` is called.

The ```startActivityForResult``` call means that a result will be returned to the calling ```Activity```.
In ```onActivityResult``` the requestCode parameter will be the same as that sent with the ```Intent```. If the resultCode is RESULT_OK, the request was succesful.

In the case of an image taken with the camera, a new ```ProgressDialog``` is created and shown, and an ```AsyncTask``` is started to convert the ```BitMap``` image into a base64 encoded
string in the PNG format.
Once the conversion is complete, the ```ProgressDialog``` is cancelled, and ```uploadImage``` is called.

###### Existing image from gallery

If the user has selected "Choose from gallery", a new ```Intent``` with the ```Intent.ACTION_GET_CONTENT``` action, and the ```MediaStore.Images.Media.EXTERNAL_CONTENT_URI``` Uri is created.
The ```Intent``` type is set to image, and the ```Intent``` is started for a result with the ```SELECT_FILE``` request code.

If the user chooses a file, the result code will be RESULT_OK.
The Uri of the file is accessed from the data ```Intent```, the ```ProgressDialog``` is shown, and an ```AsyncTask``` is started to attempt to load the image from the ```File```.

```attemptLoadImage``` opens a ```ParcelFileDescriptor```  for the image in read only mode.
The ```ParcelFileDescriptor``` returns a Java.IO ```FileDescriptor``` which handles device specific file access.
In this case the ```BitMapFactory``` is used to load the file as a ```BitMap``` image, which is then read as a ```ByteArrayOutPutStream```, compressed, and returned as a base64 string.

###### Image upload

An image from the camera or the users files must now be uploaded.
This is done when ```uploadImage``` is called.

The method first posts to the front of the UI queue with a runnable to show the upload dialog.
It then begins the upload process, passing an ```ImgurUploadListener``` which cancels the dialog and formats the image link once the image has been uploaded, before calling
```imageLoadComplete```.
The call also passes an ```UploadProgressListener``` which sets the progress of the ```ProgressDialog``` to the percentage of bytes uploaded out of the total.

##### Character insertion

While most mobile keyboards provide a sufficient set of keys for general usage, most have no way to input less common characters.

Objective 10.iii.d is to implement a method for searching for an inserting unicode characters into the markdown being edited.

This has been achieved in the ```CharacterActivity```.

**CharacterActivity.java**
``` java
package com.tpb.projects.editors;

import android.content.Intent;
import android.os.AsyncTask;
import android.os.Bundle;
import android.support.annotation.Nullable;
import android.support.v4.util.Pair;
import android.support.v7.widget.GridLayoutManager;
import android.support.v7.widget.RecyclerView;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.EditText;
import android.widget.TextView;

import com.tpb.projects.R;
import com.tpb.projects.common.BaseActivity;
import com.tpb.projects.util.SettingsActivity;
import com.tpb.projects.util.input.SimpleTextChangeWatcher;

import java.util.ArrayList;
import java.util.List;

import butterknife.BindView;
import butterknife.ButterKnife;

/**
 * Created by theo on 24/03/17.
 */

public class CharacterActivity extends BaseActivity {

    public static final int REQUEST_CODE_INSERT_CHARACTER = 7438;

    @BindView(R.id.search_title) TextView mTitle;
    @BindView(R.id.search_recycler) RecyclerView mRecycler;
    @BindView(R.id.search_search_box) EditText mSearch;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        final SettingsActivity.Preferences prefs = SettingsActivity.Preferences
                .getPreferences(this);
        setTheme(prefs.isDarkThemeEnabled() ? R.style.AppTheme_Dark : R.style.AppTheme);
        setContentView(R.layout.activity_simple_search);
        ButterKnife.bind(this);
        mTitle.setText(R.string.title_activity_characters);
        mSearch.setHint(R.string.hint_search_characters);
        mRecycler.setLayoutManager(new GridLayoutManager(this, 3));
        final CharacterAdapter adapter = new CharacterAdapter();
        mRecycler.setAdapter(adapter);
        mSearch.addTextChangedListener(new SimpleTextChangeWatcher() {
            @Override
            public void textChanged() {
                adapter.filter(mSearch.getText().toString().toUpperCase());
            }
        });
        AsyncTask.execute(() -> {
            final ArrayList<Pair<String, String>> characters = new ArrayList<>();
            final int length = Character.MAX_CODE_POINT - Character.MIN_CODE_POINT;
            int lastIndex = 0;
            for(int i = Character.MIN_CODE_POINT; i < Character.MAX_CODE_POINT; i++) {
                if(Character.isDefined(i) && !Character.isISOControl(i)) {
                    characters.add(Pair.create(String.valueOf((char) i), Character.getName(i)));
                    // 50 gives ~10 chunks
                    if((characters.size() - lastIndex) > length / 250 || i == Character.MAX_CODE_POINT - 1) {
                        adapter.addCharacters(characters, lastIndex);
                        lastIndex = characters.size();
                    }
                }
            }
        });
    }

    class CharacterAdapter extends RecyclerView.Adapter<CharacterAdapter.CharacterViewHolder> {

        private final ArrayList<Pair<String, String>> mCharacters = new ArrayList<>();
        private ArrayList<Integer> mFilteredPositions = new ArrayList<>();
        private ArrayList<Integer> mWorkingPositions = new ArrayList<>();
        private int mSize = 0;
        private String mLastQuery = "";

        void addCharacters(List<Pair<String, String>> characters, int start) {
            for(int i = start; i < characters.size(); i++) mCharacters.add(characters.get(i));

            final int originalSize = mFilteredPositions.size();
            for(int i = originalSize; i < mCharacters.size(); i++) {
                mFilteredPositions.add(i);
            }
            CharacterActivity.this.runOnUiThread(() -> {
                mSize = mFilteredPositions.size();
                notifyItemRangeInserted(originalSize, mFilteredPositions.size());
            });
        }

        void filter(String query) {
            AsyncTask.execute(() -> {
                mWorkingPositions = new ArrayList<>();
                if(query.isEmpty()) {
                    for(int i = 0; i < mCharacters.size(); i++) {
                        mWorkingPositions.add(i);
                    }
                } else if(query.startsWith(mLastQuery)) {
                    for(int i = 0; i < mFilteredPositions.size(); i++) {
                        if(mCharacters.get(mFilteredPositions.get(i)).second.contains(query)) {
                            mWorkingPositions.add(mFilteredPositions.get(i));
                        }
                    }
                } else {
                    for(int i = 0; i < mCharacters.size(); i++) {
                        if(mCharacters.get(i).second.contains(query)) {
                            mWorkingPositions.add(i);
                        }
                    }
                }
                mLastQuery = query;
                CharacterActivity.this.runOnUiThread(() -> {
                    mFilteredPositions = mWorkingPositions;
                    mSize = mFilteredPositions.size();
                    notifyDataSetChanged();
                });
            });


        }

        private void choose(int pos) {
            CharacterActivity.this.choose(mCharacters.get(mFilteredPositions.get(pos)).first);
        }

        @Override
        public CharacterViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
            return new CharacterViewHolder(LayoutInflater.from(parent.getContext())
                                                         .inflate(R.layout.viewholder_text, parent,
                                                                 false
                                                         ));
        }

        @Override
        public void onBindViewHolder(CharacterViewHolder holder, int position) {
            holder.mCharacter.setText(mCharacters.get(mFilteredPositions.get(position)).first);
            holder.mName.setText(mCharacters.get(mFilteredPositions.get(position)).second);
        }

        @Override
        public int getItemCount() {
            return mSize;
        }

        class CharacterViewHolder extends RecyclerView.ViewHolder {

            @BindView(R.id.text_content) TextView mCharacter;
            @BindView(R.id.text_info) TextView mName;

            CharacterViewHolder(View itemView) {
                super(itemView);
                ButterKnife.bind(this, itemView);
                itemView.setOnClickListener(v -> choose(getAdapterPosition()));
            }
        }

    }

    protected void choose(String c) {
        final Intent result = new Intent();
        result.putExtra(getString(R.string.intent_character), c);
        setResult(RESULT_OK, result);
        finish();
    }

}

```

The layout for this ```Activity``` consists of an ```EditText``` for search input, and a ```RecyclerView``` in which to display the searchable content.

The viewholder layout used for displaying each character consists of two ```TextViews```, to display the character and its name.

``` XML
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="match_parent"
              android:layout_height="wrap_content"
              android:layout_margin="8dp"
              android:background="?attr/selectableItemBackgroundBorderless">

    <TextView
        android:id="@+id/text_content"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center_horizontal"
        android:textAppearance="@android:style/TextAppearance.Material.Title"/>

    <TextView
        android:id="@+id/text_info"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center_horizontal"/>

</LinearLayout>
```

In ```onCreate```, the layout is inflated, and the text is set on the title and search ```Views```. The ```RecyclerView``` is then setup with a ```GridLayoutManager``` to display
three viewholders per row, as each viewholder is quite small.

After the adapter is created, a ```SimpleTextChangeWatcher``` is appl,ied to the search ```EditText``` to capture input as the user types, and pass it to the adapter for filtering.

The characters are then loaded for the adapter.
The full range of characters defined in the ```Character``` class is from 0 to 1114111.
It is not reasonable to load all of these characters at once, especially not on the UI thread.

Instead, they must be loaded in chunks from another thread.
Within the ```AsyncTask``` a new ```ArrayList``` of pairs of strings is created.
The total length is set as ```MAX_CODE_POINT - MIN_CODE_POINT```, and ```lastIndex``` is set as 0, representing the index of the last block added to the adapter.
For each character in the range, if the character is defined and not a control character, it is added to the ```ArrayList``` along with its name.

If the current size of the ```ArrayList``` minus the last added index is greater than one 250<sup>th</sup> of the entire length, the characters are added to the adapter, and the lastIndex
is reset.
This results in the valid characters being chunked into around 56 blocks.

###### CharacterAdapter

The ```CharacterAdapter``` holds three ```ArrayLists```. The first is mCharacters, which holds the ```Pairs``` of strings which are to be displayed.
The other two ```ArrayLists``` are lists of integers which point to the positions in the first list.
mFilteredPositions is the ```ArrayList``` which the adapter uses to determine its length and to find the strings which it should be binding to to the viewholders.
mWorkingPositions is a separate ```ArrayList``` used during the filtering of positions on another thread. mFilteredPositions cannot be updated from another thread because the 
```RecyclerView``` expects the data-set not to change without it being notified, and the ```RecyclerView``` cannot be notified as each character is filtered.

When the ```Activity``` starts the ```AsyncTask``` begins to add blocks of characters to the adapter.
When ```addCharacters``` is called, each of the characters from the start position onwards are added to mCharacters.
Next, the original length of mFilteredPositions is saved, and each of the integer values from this point to the size of mCharacters is added to mFilteredPositions.
Finally, a new runnable is posted to the UI thread to change the mSize variable and call ```notifyItemRangeInserted``` to notify the ```RecyclerView``` that a new range has been
inserted. 
In this case there is no need for a working ```ArrayList``` because items are being added, not removed, and as such mSize will always be smaller than the length of mFilteredPositions, meaning that the ```RecyclerView``` will never request a position past this point.

When the user types something into the ```EditText```, ```filter``` is called with the query that they typed.
This executes an ```AsyncTask``` to search the characters for their query.
First, mWorkingPositions is re-created.
Next, there are three possibilities for the search:

-1. The query is empty, in which case all of the positions are added to mWorkingPositions
-2. The query starts with the last query, meaning that the user has types another character. In this case the method iterates through the currently filtered positions and only searches the characters at these positions in mCharacters for the new query, as if the other characters did not contain the shorter query, they will not contain this one.
-3. Otherwise, the entire mCharacters list is searched, and the matching positions are added to mWorkingPositions.

Once mWorkingPositions has been built, the last query is updated, and a new runnable is posted to the UI thread to update the filtered positions.
This swaps mFilteredPositions, updates the size, and notifies that the dataset has been changed.

![Character Activity](http://imgur.com/7VGtJwB.png)

###### Returning the chosen character

When the user clicks on a character, ```choose``` is called in the ```CharacterAdapter```, which calls the ```choose``` method in ```CharacterActivity``` with the character string.
This creates an ```Intent``` and adds the string as an extra, before setting the result code to RESULT_OK and the data to the ```Intent```, and finishing the ```Activity```.

Returning to the ```onActivityResult``` method of ```EditorActivity```, the resultCode will be ```CharacterActivity.REQUEST_CODE_CHARACTER_INSERTED```.
```insertString``` is then called with the character string.

##### Emoji insertion

The ```EmojiActivity``` is very similar to the ```CharacterActivity``` except that it does not have to work on another thread as there are far fewer emojis. The Unicode 9.0 specification includes 1126 emoji characters which are easily searchable without impacting UI performance.
The ```EmojiActivity``` does however store the last 9 emojis that the user used.

**EmojiActivity.java**
``` java
package com.tpb.projects.editors;

import android.content.Intent;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.support.annotation.Nullable;
import android.support.v7.widget.GridLayoutManager;
import android.support.v7.widget.RecyclerView;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.EditText;
import android.widget.TextView;

import com.tpb.mdtext.TextUtils;
import com.tpb.mdtext.emoji.Emoji;
import com.tpb.mdtext.emoji.EmojiLoader;
import com.tpb.projects.R;
import com.tpb.projects.common.BaseActivity;
import com.tpb.projects.util.SettingsActivity;
import com.tpb.projects.util.input.SimpleTextChangeWatcher;

import java.util.ArrayList;

import butterknife.BindView;
import butterknife.ButterKnife;

/**
 * Created by theo on 23/03/17.
 */

public class EmojiActivity extends BaseActivity {

    public static final int REQUEST_CODE_CHOOSE_EMOJI = 666;
    private static final String PREFS_COMMON_EMOJIS = "COMMON_EMOJIS";

    private SharedPreferences mCommonEmojis;
    @BindView(R.id.search_title) TextView mTitle;
    @BindView(R.id.search_recycler) RecyclerView mRecycler;
    @BindView(R.id.search_search_box) EditText mSearch;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        final SettingsActivity.Preferences prefs = SettingsActivity.Preferences
                .getPreferences(this);
        setTheme(prefs.isDarkThemeEnabled() ? R.style.AppTheme_Dark : R.style.AppTheme);
        setContentView(R.layout.activity_simple_search);
        ButterKnife.bind(this);
        mTitle.setText(R.string.title_activity_emoji);
        mSearch.setHint(R.string.hint_search_characters);
        mCommonEmojis = getSharedPreferences(PREFS_COMMON_EMOJIS, MODE_PRIVATE);
        mRecycler.setLayoutManager(new GridLayoutManager(this, 3));
        final EmojiAdapter adapter = new EmojiAdapter(mCommonEmojis);
        mRecycler.setAdapter(adapter);
        mSearch.addTextChangedListener(new SimpleTextChangeWatcher() {
            @Override
            public void textChanged() {
                adapter.filter(mSearch.getText().toString().toLowerCase().replace(" ", "_"));
            }
        });
    }

    class EmojiAdapter extends RecyclerView.Adapter<EmojiAdapter.EmojiViewHolder> {

        private ArrayList<Emoji> mEmojis = new ArrayList<>();
        private ArrayList<Emoji> mFilteredEmojis = new ArrayList<>();

        EmojiAdapter(SharedPreferences prefs) {
            mEmojis.addAll(EmojiLoader.getAllEmoji());
            if(prefs.getString("common", null) != null) {
                final String[] common = prefs.getString("common", "").split(",");
                for(String s : common) {
                    final Emoji e = EmojiLoader.getEmojiForAlias(s);
                    if(e != null) {
                        mEmojis.remove(e);
                        mEmojis.add(0, e);
                    }
                }
            }
            mFilteredEmojis.addAll(mEmojis);
        }

        void filter(String query) {
            mFilteredEmojis.clear();
            if(query.isEmpty()) {
                mFilteredEmojis.addAll(mEmojis);
            } else {
                for(Emoji e : mEmojis) {
                    boolean added = false;
                    for(String s : e.getAliases()) {
                        if(s.contains(query)) {
                            mFilteredEmojis.add(e);
                            added = true;
                            break;
                        }
                    }
                    if(!added) {
                        for(String s : e.getTags()) {
                            if(s.contains(query)) {
                                mFilteredEmojis.add(e);
                                break;
                            }
                        }
                    }
                }
            }
            notifyDataSetChanged();
        }

        private void choose(int pos) {
            EmojiActivity.this.choose(mFilteredEmojis.get(pos).getAliases().get(0));
        }

        @Override
        public EmojiViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
            return new EmojiViewHolder(LayoutInflater.from(parent.getContext())
                                                     .inflate(R.layout.viewholder_text, parent,
                                                             false
                                                     ));
        }

        @Override
        public void onBindViewHolder(EmojiViewHolder holder, int position) {
            holder.mEmoji.setText(mFilteredEmojis.get(position).getUnicode());
            holder.mName.setText(
                    String.format(":%1$s:", mFilteredEmojis.get(position).getAliases().get(0)));
        }

        @Override
        public int getItemCount() {
            return mFilteredEmojis.size();
        }

        class EmojiViewHolder extends RecyclerView.ViewHolder {

            @BindView(R.id.text_content) TextView mEmoji;
            @BindView(R.id.text_info) TextView mName;

            EmojiViewHolder(View itemView) {
                super(itemView);
                ButterKnife.bind(this, itemView);
                itemView.setOnClickListener(v -> choose(getAdapterPosition()));
            }
        }

    }

    private void choose(String alias) {
        String common = "";
        if(mCommonEmojis.getString("common", null) != null) {
            String current = mCommonEmojis.getString("common", "");
            if(current.contains(alias)) {
                final int index = current.indexOf(alias);
                current = current.substring(0, index) + current.substring(index + alias.length());
            }
            if(TextUtils.instancesOf(current, ",") > 8) {
                current = current.substring(current.indexOf(',') + 1);
            }
            common = current;
        }
        common += "," + alias;
        mCommonEmojis.edit().putString("common", common).apply();

        final Intent result = new Intent();
        result.putExtra(getString(R.string.intent_emoji), alias);
        setResult(RESULT_OK, result);
        finish();
    }

}

```

In ```onCreate``` the same layout as ```CharacterActivity``` is inflated, and the title and search hint are set to the appropriate string resources.
The ```SharedPreferences``` used for storing common emojis is then loaded, and the ```RecyclerView``` is set up with a ```GridLayoutManager```.
The ```EmojiAdapter``` is created, and set on the ```RecyclerView```, and finally a ```SimpleTextChangeWatcher``` is added to the search ```EditText```.

The ```SimpleTextChangeWatcher``` replaces spaces in the search string with underscores before passing it to the adapter, as multi-word emoji names are separate by underscores rather than spaces.

When the ```EmojiAdapter``` is created, it adds all of the ```Emoji``` from ```EmojiLoader``` to mEmojis.
It then checks if anything is stored under the "common" key in the ```EmojiActivity``` shared preferences.
If the ```EmojiActivity``` has been used before, the value under the "common" key will be a comma delimited list of emoji aliases, which are split and used to insert each of the common
emojis at the start of the array.

When the user enters a query, the currently filtered ```Emojis``` are cleared.
If the query is empty, all of the ```Emojis``` are added to mFilteredEmojis.
Otherwise, each ```Emoji``` is checked. If one of the aliases matches, the ```Emoji``` is added, if not the tags are checked for matches, and if one is found the ```Emoji``` is added.
```notifyDataSetChanged``` is then called.

![EmojiActivity](http://imgur.com/Bap3G8x.png)

When the user chooses an ```Emoji```, it is added to the ```SharedPreferences``` before being returned.
The common string is declared, and if mCommonEmojis contains the "common" key, the current value is loaded. If the emoji alias is already contained in the string, it is removed. Next, if the string contains more than 8 commas, 9 elements, the first item is removed. Finally, the common string is set to the current string.

Next, the new emoji alias is added to common, and the common string is written to ```SharedPreferences```.

Finally, the result ```Intent``` is created, the emoji alias is added as an extra, the result is set, and the ```EmojiActivity``` finishes.

<div style="page-break-after: always;"></div>

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
public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
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
public static void setDrawableForIntent(@NonNull ImageView iv, @NonNull Intent i) {
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
```

The ```onDraw``` method begins by measuring the size which it has to draw in.
Next, the number of columns to show horizontally can be calculated, as either the number of contributions split across 7 day weeks, or 52 weeks.

Each rectangle has an area of the view width over the horizontal number of contributions, however it must also account for the margin between each rectangle.
For each rectangular segment of the canvas, 90% of the width will be filled with the block, and the remaining 10% left as a margin.

Next the month text height is set as either the text size, or 0 dependent on whether month text should be drawn.

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
public void contributionsLoaded(List<ContributionsLoader.ContributionsDay> contributions) {
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
public static void displayUser(ViewGroup userInfoParent, User user) {
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
private static TextView getInfoTextView(Context context, @DrawableRes int drawableRes) {
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

The ```updated``` method is used for binding the following information, and ```loadComplete``` passes its return value through to ```updated```.

**UserInfoFragment.java**
``` java
public void updated(Boolean isFollowing) {
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

```openRepo``` creates the ```Intent``` to launch the ```RepoActivity```, begins preloading the data which will be used in ```RepoActvitiy```, and then launches ```RepoActivity```.

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
