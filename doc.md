 
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

#page

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

#page

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

#page

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

#page

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

#page

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

#page

### Basic storage

While many applications use a variety of database solutions, it is not always necessary or the best solution to store simple values in a full database.

The ```SharedPreferences``` system is a persistent set of key value pairs which can be used to store application information such as settings.

Each 'preference' is a key value set stored under a particular name.
Once a ```SharedPreference``` object has been returned for the set, the key value pairs within the set can be read from and written to.


#page

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


#page

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

#page

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
                    <li>Parse horizontal rules and create the correct spans </li>
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
                    <li>Horizontal rules</li>
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
                    <li>Checkboxes</li>
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

#page

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

#page

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

#page

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

#page

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

#import "gitapi/src/main/java/com/tpb/github/data/APIHandler.java"

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

#import "gitapi/src/main/java/com/tpb/github/data/auth/GitHubSession.java"

```GitHubSession``` is a singleton class which saves and loads the user credentials and authorization token to and from shared preferences.

The private constructor is used to initialise the SharedPreferences instance, this either opens the pre-existing map or creates a new one if it does not exist.

When the user authorizes the app the access token is stored with  ```storeAccessToken(@NonNull String accessToken)```.

Once we have an authorization token, we can load the user's data and store it for later use.

The ```LoginActivity``` consists of two layouts, only one of which is visible at a time. 
 
The first layout is a ```WebView``` which is used to display the user authentication page, and the second is a layout to display the user's information once they have signed in.

#import "app/src/main/java/com/tpb/projects/login/LoginActivity.java"

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

#import "gitapi/src/main/java/com/tpb/github/data/auth/OAuthHandler.java"

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

#import "app/src/main/java/com/tpb/projects/login/LoginActivity.java $public void onPageStarted$"


#import "gitapi/src/main/java/com/tpb/github/data/auth/OAuthHandler.java $public void getAccessToken$"

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

#import "gitapi/src/main/java/com/tpb/github/data/models/DataModel.java"

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


#import "gitapi/src/main/java/com/tpb/github/data/Loader.java $public Loader loadIssue$"


this method is used to load a single ```Issue``` model given a full repository name (user login and repository name) and the issue number.

Some single methods also have prefetching when a null ```ItemLoader``` is passed to them:

#import "gitapi/src/main/java/com/tpb/github/data/Loader.java $public Loader loadProject$"

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

#import "app/src/main/java/com/tpb/projects/common/BaseActivity.java"

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

#import "app/src/main/java/com/tpb/projects/common/BaseActivity.java $public void onAttachFragment$"

```onAttachFragment``` adds a ```WeakReference``` to the ```Fragment``` to ```mWeakFragments```, and ```cancelNetworkRequests``` uses these to cancel network requests started by each ```Fragment```.

#import "app/src/main/java/com/tpb/projects/common/BaseActivity.java $private void cancelNetworkRequests$"

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

#page

### NetworkImageView

The ```NetworkImageView``` is a subclass of ```ImageView``` used to asynchronously load images from a given URL and display them once they have finished downloading.

#import "app/src/main/java/com/tpb/projects/common/NetworkImageView.java"

The ```NetworkImageView``` has three instance variables; the URL to be loaded as well as two resource identifiers for the loading and error states.

When the ```NetworkImageView``` is instantiated it checks the ```AttributeSet``` for resource identifiers set in XML.

#import "app/src/main/java/com/tpb/projects/common/NetworkImageView.java $private void init$"

The ```loadImage``` method is responsible for loading and displaying the image.

#import "app/src/main/java/com/tpb/projects/common/NetworkImageView.java $private void loadImage$"

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


#page

### ViewSafeFragment

The lifecycle of a ```Fragment``` is different than that of an ```Activity```.

The first method called is ```onAttach``` when the ```Fragment``` is attached to an ```Activity```.
Next, ```onCreate``` is called, which might be used to initialise non view-dependent logic.
Most importantly, ```onCreateView``` is called.

```onCreateView``` returns a ```View``` object and must created the layout for the ```Fragment```.

As the ```Activity``` is created prior to the ```Fragment``` ```View```s being created, the ```Fragment``` may have data to bind before it has to ```Views``` to bind them to.

In order to ensure that the ```Fragment``` doesn't attempt to bind data to a null ```View```, the ```Fragment``` has flag set when the its ```Views``` are created, and set back when its ```Views``` are destroyed.

#import "app/src/main/java/com/tpb/projects/common/ViewSafeFragment.java" 

In a concrete instance of ```ViewSafeFragment``` the mAreViewsValid flag should be set after inflation in ```onCreateView``` and used to check ```View``` validity before performing any binding.

### FabHideScrollListener

A ```FloatingActionButton``` is a button which, as its name suggests, floats over other ```Views```. It is often positioned in the bottom right of a screen to provide a button for the 
primary action.
The floating nature of the button can cause problems when it is displayed over a ```RecyclerView``` as it obscures the bottom most item.

To solve this the ```FloatingActionButton``` should hide when the ```RecyclerView``` scrolls down, and show again when it scrolls back up.

#import "app/src/main/java/com/tpb/projects/common/fab/FabHideScrollListener.java"

The ```FabHideScrollListener``` extends  ```RecyclerView.OnScrollListener``` and overrides ```onScrolled```, checking if the change in the y value is sufficient to indicate scrolling.

#page


### SimpleTextChangeWatcher

The ```SimpleTextChangeWatcher``` is a simplified abstract implementation of ```TextWatcher``` which forwards the ```onTextChanged``` call to ```textChanged``` without any parameters,
rather than requiring the implementation of ```beforeTextChanged```, ```onTextChanged```, and ```afterTextChanged``` when the logic only needs to know that the text has changed.

#import "app/src/main/java/com/tpb/projects/util/input/SimpleTextChangeWatcher.java"

#page

### KeyBoardVisibilityChecker

A long running gripe with Android's text input system is that there is no standard way to detect whether the keyboard is currently visible, or listen for when its visibility changes.

This functionality is achieved by listening for changes on the ```ViewTreeObserver``` of the root ```View``` of an ```Activity``` and comparing it to the display frame size.

#import "app/src/main/java/com/tpb/projects/util/input/KeyBoardVisibilityChecker.java"

The ```KeyBoardVisibilityChecker``` adds an ```onGlobalLayoutListener``` which checks the size of the window with ```getWindowVisibleDisplayFrame```.
This method applies the dimensions of "the overall visible display size in which the window this view is attached to has been positioned in" to a given ```Rect```.

When the keyboard is shown, the root content layout is pushed upward and resized. The bottom of the content layout is therefore at the same position as the top of the keyboard.

Next the screen height is found from the height of the root ```View``` returned by the content layout.

If the calculated height is greater than 15% of the screen, it can be assumed that the keyboard is showing.

The ```KeyBoardVisibilityListener``` is an interface which can be used to listen for changes in keyboard visibility.

#page

### Utility methods

The ```Util``` class contains numerous utility methods for formatting and finding array indices.

The ```indexOf``` methods are used to find the index of a value within an array of integers, strings, pairs, or a generic type.
Each method performs a linear search for the key item, returning -1 if it does not exist.

The  ```formatBytes``` and ```formatKB``` methods are used to format a number of bytes or kilobytes into a 2 decimal place string representation of the highest unit suffix.

The next method, ```formatDateLocally``` is used to format a date in the expected manner for the device locale.

```isNotNullOrEmpty``` is used to check that a string is not null, not empty, and not a "null" string returned from a ```JSONObject```.

Finally, the ```insertString``` methods are used to insert a string at the currently selected position in an ```EditText```, before moving the cursor to the end of the inserted string, or to a provided offset.

#page 

### User Interface utility methods

The ```UI``` class contains numerous helper methods for performing unit conversions as well as helping with ```View``` animations.

#import "app/src/main/java/com/tpb/projects/util/UI.java"

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


#page

### Logging

As explained in the analysis, logs are printed with the ```Log``` class and are a useful method of debugging. However, log messages should only be shown in the debug variant of the
application.
The ```Logger``` class wraps the methods in ```Log``` with checks for the ```BuildConfig``` flag ```IS_IN_DEBUG```.

#import "app/src/main/java/com/tpb/projects/util/Logger.java"

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

#page

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

#import "app/src/main/java/com/tpb/projects/flow/Interceptor.java"

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

#page

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/Markdown.java $public static String formatMD(@NonNull String s, @Nullable String fullRepoPath, boolean linkUsernames)$"

#import "markdowntextview/src/main/java/com/tpb/mdtext/Markdown.java $private static boolean isWhiteSpace$"

#import "markdowntextview/src/main/java/com/tpb/mdtext/Markdown.java $private static boolean isLineEnding$"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/Markdown.java $private static int parseUsername$"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/Markdown.java $private static int parseIssue$"

```parseIssue``` checks if each character is a numeric value, adding it to numBuilder if so.
If the character is instead whitespace or a line ending the issue link may be valid.
If we are at the end of the character array the final character must be checked for validity, and added to numBuilder, otherwise the loop breaks.
The link is built, and if the counter is not at the end of the array the original whitespace is appended.
If the character was not a valid issue link, the hash, "#", is appended and the original index is returned.

This method completes objective 9.ii.e.

#### Relative links

When Markdown is rendered in a GitHub repository, links can be relative to the repository.
In order to load content from these links they need to be changed to a full link including the repository path.

#import "markdowntextview/src/main/java/com/tpb/mdtext/Markdown.java $private static String concatenateRawContentUrl$"

A relative URL can be only a file name or it can start with either "/" or "./" specifying a path in the repository.

If the URL begins with "http://" or "https://" it is assumed to be valid and returned.
Otherwise, the offset is calculated and the URL is added as the file path in a link to githubusercontent.

The ```concatenateRawContentUrl``` function is used when parsing image links, as well as when checking links in a repository README.

#### Image links

Image links are checked both to ensure that they are not relative, and to add spacing around each image so that it does not interfere with the text line spacing.

#import "markdowntextview/src/main/java/com/tpb/mdtext/Markdown.java $private static int parseImageLink$"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/emoji/Emoji.java"

The emoji are loaded from the resource directory and added to a master list of emojis as well as maps for aliases and tags.

#import "markdowntextview/src/main/java/com/tpb/mdtext/emoji/EmojiLoader.java"

The json file is opened as an ```InputStream``` which is then converted to a ```String``` which can be read as a ```JSONArray``` for conversion to ```Emoji``` objects.
For each ```Emoji``` successfully created the object is added to EMOJIS, added to ALIAS_MAP, and added to a ```HashSet``` of ```Emojis``` in TAG_MAP.

The two ```HashMaps``` can later be used to retrieve ```Emojis``` by their tags or aliases.

```getEmojiForAlias``` also checks whether the alias is in the ":alias:" format and strips the colons before searching.

##### Displaying Emoji

#import "markdowntextview/src/main/java/com/tpb/mdtext/Markdown.java $private static int parseEmoji$"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/MDPattern.java"

#### Adding links

Having defined the regex for matching URLs and emails, the ```addLinks``` method can be explained.

#import "markdowntextview/src/main/java/com/tpb/mdtext/TextUtils.java $public static boolean addLinks$"

The method uses a matcher from the SPACED_MATCH_PATTERN and sets a ```CleanURLSpan```, a subclass of ```URLSpan``` to be explained later, across the indices of the match.

The ```Spanned.SPAN_EXCLUSIVE_EXCLUSIVE``` flag means that if a character or another span is inserted at either end of the ```CleanURLSpan``` it will not be viewed as part of the ```CleanURLSpan```.

This method completes objective 9.iii.

#### Multiple pattern matching and string escaping

In order not to attempt to display HTML tags in titles, and to replace HTML tags in order to stop Android capturing them, multiple string replace calls must be used.

Rather than calling the replace method multiple times, each incurring a full traversal of the string, multiple matches can be compiled into a single pattern.

#import "markdowntextview/src/main/java/com/tpb/mdtext/TextUtils.java $static Pattern generatePattern$"

```generateKeys``` takes a ```Set``` of strings and builds an or separated pattern from the strings.

In order to ensure that the strings themselves are only matched as text, they must be escaped.

Each match key is escaped with the "[\\<\\(\\[\\{\\\\\\^\\-\\=\\$\\!\\|\\]\\}\\)‌​\\?\\*\\+\\.\\>]" pattern, which matches regex control characters and allows them to be replaced with their escaped form.

Once a valid pattern has been generated, a single ```Matcher``` can be used to replace a set of key value pairs from a ```Map```.

#import "markdowntextview/src/main/java/com/tpb/mdtext/TextUtils.java $static String replace(@Nullable String s, Map<String, String> replacements, Pattern pattern)$"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/TextUtils.java $public static int getTextColorForBackground$"

This method completes objective 9.ii.h.

#### Other utility methods

#import "markdowntextview/src/main/java/com/tpb/mdtext/TextUtils.java"

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

#page

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/webview/MarkdownWebView.java $private void init$"

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

#import "markdowntextview/src/main/assets/html/js/md_preview.js"

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

#page

### Displaying short Markdown sections

In order to display GitHub Markdown formatted text in the Android ```TextView```, custom spans are required.

#### CleanURLSpan

This span type was reference earlier.
It is used to display links to web addresses and email addresses.

#import "markdowntextview/src/main/java/com/tpb/mdtext/views/spans/CleanURLSpan.java"

The ```CleanURLSpan``` uses the ```LinkClickHandler``` interface, which provides an onClick method for a URL

 #import "markdowntextview/src/main/java/com/tpb/mdtext/handlers/LinkClickHandler.java"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/views/spans/HorizontalRuleSpan.java"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/views/spans/QuoteSpan.java"

```QuoteSpan``` extends ```CharacterStyle```, allowing it to set modify the ```TextPaint```, as well as implementing ```LeadingMarginSpan``` in order to draw the quote line.

The ```QuoteSpan``` follows the material guidelines for secondary text, which specify using an opacity of 54% for secondary text using a dark text on light backgrounds, and using 70%
opacity for secondary text using a white text on light backgrounds.

Each Android colour is stored as an integer with alpha, red, green, and blue occupying each byte.
The alpha value is stripped from the colour by and-ing it with 00FFFFFF, and it is then compared to the middle colour value to approximate whether it is a light or dark colour.

In ```drawLeadingMarginSpan``` the original ```Style``` and colour are saved, and a rectangle is drawn across the whole vertical space of the line, and across 4 pixels horizontally. 
The original style and colour are then saved.

#page

#### ListNumberSpan

```ListNumberSpan``` implements ```LeadingMarginSpan``` and is used to draw the keys in an ordered list.

HTML ordered lists can specify four types of keys.
- Numbers, indexed from 1
- Letters, indexed from a
- Capital letters, indexed from A
- Roman numerals, indexed from i
- Capital Roman numerals, indexed from I

The ```ListNumberSpan``` needs to specify the margin for for list indentation, as well as drawing the list item number.

#import "markdowntextview/src/main/java/com/tpb/mdtext/views/spans/ListNumberSpan.java"

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


#import "markdowntextview/src/main/java/com/tpb/mdtext/views/spans/RoundedBackgroundEndSpan.java"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/views/spans/InlineCodeSpan.java"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/views/spans/CodeSpan.java"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/views/spans/TableSpan.java"

The key differences are that ```TableSpan``` only ever draws a constant string, and that it takes a ```TableClickHandler``` which only takes one parameter, the table HTML, rather than
code and a language.

![Table span](http://imgur.com/NDA1ydi.png)

This span completes objective 9.ii.i

##### ClickableImageSpan

```ClickableImageSpan``` extends ```ImageSpan``` and is used to implement click listening for the ```ImageClickHandler```.
It also ensures that the actual drawable is returned from a ```URLDrawable```, which will be explained in the "Image loading and caching" section.

#import "markdowntextview/src/main/java/com/tpb/mdtext/views/spans/ClickableImageSpan.java"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/views/spans/WrappingClickableSpan.java"

As was shown above, ```CodeSpan```, ```TableSpan``` and ```ImageSpan``` implement ```WrappedClickableSpan``` which allows touch events on a parent ```ClickableSpan``` to be forwarded to the 
```ReplacementSpan```.

###### Stopping the onClickHandler

The problem with having clickable elements in the ```TextView``` is that it interferes with any click listeners set on the ```TextView``` itself.

This problem is solved with a custom ```MovementMethod``` and ```TextView```.

#import "markdowntextview/src/main/java/com/tpb/mdtext/ClickableMovementMethod.java"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/handlers/CodeClickHandler.java"

#import "markdowntextview/src/main/java/com/tpb/mdtext/handlers/ImageClickHandler.java"

#import "markdowntextview/src/main/java/com/tpb/mdtext/handlers/LinkClickHandler.java"

#import "markdowntextview/src/main/java/com/tpb/mdtext/handlers/TableClickHandler.java"

The default implementations of ```CodeClickHandler```, ```ImageClickHandler``` and ```TableClickHandler``` are all dialogs used to show the content over a larger area.
They can be replaced with any other implementation of their respective interfaces.

#import "markdowntextview/src/main/java/com/tpb/mdtext/dialogs/CodeDialog.java"

The ```CodeDialog``` creates a dialog to display a ```HighlightJsView```, which is a ```WebView``` with the highlightjs library embedded.
It also attempts to find the correct language for highlighting the code.

This completes objective 9.ii.a.2

#import "markdowntextview/src/main/java/com/tpb/mdtext/dialogs/ImageDialog.java"

The ```ImageDialog``` is used to show an image across the entire screen, while maintaining its aspect ratio.

It uses a ```FillingImageView``` which overrides the ```onMeasure``` method of ```ImageView``` to ensure that the image aspect ratio is maintained.

This completes objective 9.ii.c.5

#import "markdowntextview/src/main/java/com/tpb/mdtext/dialogs/FillingImageView.java"

The method calculates the aspect ratio of the image, and the aspect ratio of the parent ```View```.

If the image has a wider ratio than the display ratio, then the width is calculated as the available width of the ```ImageView```, and the height is calculated as the ratio of this width
matching the ratio calculated earlier.

If the image has a taller ratio than the display ratio, then the height is calculated as the available height of the ```ImageView```, and the width is calculated as the ratio of this 
height matching the ratio calculated earlier.

#import "markdowntextview/src/main/java/com/tpb/mdtext/dialogs/TableDialog.java"

The ```TableDialog``` uses the ```MarkdownWebView``` created earlier to display the HTML of the table in a ```WebView``` using the GitHub style CSS.

This dialog completes objective 9.ii.i

##### Image loading and caching 

The ```HttpImageGetter``` implements ```Html.ImageGetter``` which is used when retreiving images for img tags.

As well as loading images, the ```HttpImageGetter``` also caches them, which is especially useful when editing markdown segments containing images or in a comment feed where multiple users may use the same image, as it stops the image being reloaded
each time the editor is toggled from raw to formatted markdown.

#import "markdowntextview/src/main/java/com/tpb/mdtext/imagegetter/HttpImageGetter.java"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/imagegetter/AssetsImageGetter.java"

The ```AssetsImageGetter``` creates an ```InputStream``` from an assets path and loads the drawable from it.

#import "markdowntextview/src/main/java/com/tpb/mdtext/imagegetter/ResImageGetter.java"

The ```ResImageGetter``` attempts to load a drawable from a resource name, checking the current package as well as the built in drawables.

##### MarkdownTextView

```MarkdownTextView``` is used to implement each of the individual objectives described above. It handles backgrround parsing of the markdown as well as dealing with click handling and
the caching of content.

The ```MarkdownTextView``` contains a ```LinkClickHandler```, ```ImageClickHandler```, ```TableClickHandler```, and ```CodeClickHandler```.

#import "markdowntextview/src/main/java/com/tpb/mdtext/views/MarkdownTextView.java"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/views/MarkdownEditText.java"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/HtmlTagHandler.java $public HtmlTagHandler$"

#### Tag opening and closing

Each tag is delegated to the ```HtmlTagHandler``` through the ```handleTag``` method, which has the parameters ```boolean opening, String tag, Editable output, XMLReader xmlReader```.
If ```opening``` is true, the tag, output and xmlReader are passed to ```handleOpeningTag```. Otherwise the tag and output are passed to ```handleClosingTag```.

#import "markdowntextview/src/main/java/com/tpb/mdtext/HtmlTagHandler.java $public void handleTag$"

```handleOpeningTag``` switches the tag to begin the correct span type in the ```Editable```

#import "markdowntextview/src/main/java/com/tpb/mdtext/HtmlTagHandler.java $private void handleOpeningTag$"

After the tag has been handled, any table tags are stored with ```storeTableTags```.
This checks if the current table depth is greater than 0, or the tag is "table".
If so, the opening bracket is added to the mTableHtmlBuilder ```StringBuilder```, along with the closing forward slash if the tag is being closed.
The tag is then appended and closed.

#import "markdowntextview/src/main/java/com/tpb/mdtext/HtmlTagHandler.java $private void storeTableTags$"

This builds the HTML for a table so that it can be displayed later.

#### Span opening and closing

The ```TagHandler``` works by placing MARK spans in the ```Editable```.
A MARK span is a span which must be removed later and acts as a placeholder for a new span.

#import "markdowntextview/src/main/java/com/tpb/mdtext/HtmlTagHandler.java $private void start$"

The MARK span has 0 length and is placed at the end of the ```Editable```.

#import "markdowntextview/src/main/java/com/tpb/mdtext/HtmlTagHandler.java $private void end$"

When a tag is ended it must be replaced by one or more spans, or removed.

The span is extracted with ```getLast```

#import "markdowntextview/src/main/java/com/tpb/mdtext/HtmlTagHandler.java $private static <T> T getLast$"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/HtmlTagHandler.java $private static String getAttribute$"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/HtmlTagHandler.java $private void handleULTag$"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/HtmlTagHandler.java $private void handleTableTag$"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/HtmlTagHandler.java $private void handleFontTag$"

This extracts the spans for each of the the three possible attributes.

If the ```ForegroundColor``` is non null, the start and end are stored, the span is removed, and a ```ForegroundColorSpan``` is set with the parsed color of the the ```ForegroundColor```
span.

The colour is parsed using ```safelyParseColor```.

#import "markdowntextview/src/main/java/com/tpb/mdtext/HtmlTagHandler.java $private int safelyParseColor$"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/HtmlTagHandler.java $private void handleCodeTag$"

The tag is extracted and its position saved.
If the span has a length greater than 1, the ```CodeSpan``` is to be inserted.

First the text is extracted with ```extractSpanText```.

#import "markdowntextview/src/main/java/com/tpb/mdtext/HtmlTagHandler.java $private CharSequence extractSpanText$"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/HtmlTagHandler.java $private void handleHorizontalRuleTag$"

This finds the ```HorizontalRule``` span, stores its length, removes the span, inserts a space for the ```HorizontalRuleSpan``` to occupy, and then inserts the ```HorizontalRuleSpan```.

#### Blockquote tags

Blockquote tags are opened by starting a new ```BlockQuote``` span.

They are ended with ```handleBlockQuoteTag```

This finds the ```BlockQuote``` span, stores its start and end positions, removes the ```BlockQuote``` span, and inserts a ```QuoteSpan``` across these indices.

#import "markdowntextview/src/main/java/com/tpb/mdtext/HtmlTagHandler.java $private void handleBlockQuoteTag$"

#### A tags

A tags are opened by starting a new ```A``` tag with the extracted "href" attribute.

They are ended with ```handleATag```

#import "markdowntextview/src/main/java/com/tpb/mdtext/HtmlTagHandler.java $private void handleATag$"

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

#import "markdowntextview/src/main/java/com/tpb/mdtext/HtmlTagHandler.java $private void handleInlineCodeTag$"

This replaces the ```InlineCode``` span with an ```InlineCodeSpan```, passing the correct text size.

#page

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

#import "app/src/main/res/layout/activity_markdown_editor.xml"

The markdown_editor layout contains three children.

![Markdown editor layout](http://imgur.com/L0yRDAr.png)

In vertical order, from top to bottom:

The first child contains the navigation buttons for the entire editor, allowing the edit action to be completed or discarded.

The second child is the content layout. The ```ScrollView``` wraps a ```ViewStub``` which will be inflated to display the editor layout.

The third and final child is a ```HorizontalScrollView``` containing a ```LinearLayout``` which will be used to display the list of editor action buttons.

#### Utilities

##### KeyBoardDismisingDialogFragment

One of the longest running irritations with Android is its handling of the keyboard.
There is no consistent method for changing the keyboard visibility, and the keyboard does not dismiss in scenarios when it would be expected to such as when a dialog is dismissed.

#import "app/src/main/java/com/tpb/projects/editors/KeyboardDismissingDialogFragment.java"

The ```KeyboardDismissingDialogFragment``` is a subclass of ```DialogFragment``` which ensures that the keyboard is closed when the dialog is dismissed.
This is done by checking if a ```View``` is focused, and if so hiding the keyboard from the focused ```View```.

##### MultiChoiceDialog

The ```MultiChoiceDialog``` extends ```KeyboardDismissingDialogFragment``` and builds a multi choice dialog using the ```AlertDialog``` builder, allowing colouring the ```Views```.

#import "app/src/main/java/com/tpb/projects/editors/MultiChoiceDialog.java"

The ```MultiChoiceDialogListener``` interface is used for returning the chosen items, or notifying that the choice selection has been cancelled.

When a ```MultiChoiceDialog``` is created, the choices must be set, and optionally the colours for each choice can be set.

When ```onCreateDialog``` is called, a new ```AlertDialog.Builder``` is created, and the title is set from the resource id passed to the dialog.
The multi choice items are then set, and listeners are added for the positive and negative buttons which call ```choicesComplete``` and ```choicesCancelled``` respectively.
The listener set with the multi choice items sets the value in the checked array at the toggled position.

The ```AlertDialog``` is then built, inflating the layout.
The ```ListView``` can then be extracted from the ```AlertDialog```, and if there is a colors array, a listener is added to colour each of the ```ListView``` items, as this is not a
built in feature.

```addBackgroundSetterListener``` creates an array of ```SpannableStringBuilders``` to cache the coloured spans.
It then adds an ```OnScrollListener```, and in ```onScroll``` it sets the text of each of the visible ```TextViews```.

If no ```SpannableStringBuilder``` has been built, it is constructed with a ```BackgroundColorSpan``` and a ```ForegroundColorSpan``` using ```TextUtils.getTextColorForBackground``` and then stored in the cache array.
The ```TextView``` text is then set to the ```SpannableStringBuilder```.

##### MarkdownButtonAdapter 

Objectives 10.ii and 10.iii are to implement buttons for inserting markdown control sequences into text.

Many apps, such as Facebook messenger below, augment the keyboard by showing an extra row of buttons above it, specifically for the content type being input.

![Messenger buttons](https://i.stack.imgur.com/FpZqh.png)

As explained above, the ```HorizontalScrollView``` in ```activity_markdown_editor``` will be used to display these buttons.
As the buttons are the same throughout each of the editors, a general adapter is used with an interface for inserting text or showing the format preview, which allows individual implementations of ```EditorActivity``` to deal with the ```Views``` that they have inflated.

#import "app/src/main/java/com/tpb/projects/editors/MarkdownButtonAdapter.java"

The ```MarkdownButtonAdapter``` is constructed with an ```EditorActivity```, the ```LinearLayout``` within which the ```Views``` are to be inserted, and the ```MarkdownButtonListener``` which will be called when a button is clicked.

The class variables are assigned, and ```initViews``` is called.
```initViews``` uses, ```createImageButton``` for each new ```ImageButton```. This method takes a resource id for the image to show on the button, inflates the button, sets its image resource id, adds the button to the ```LinearLayout``` and returns the button.

15 ```ImageButtons``` are created, for 15 different actions.

1. The first button is for displaying the markdown in its formatted state, and calls ```previewCalled``` on the listener.
2. The second button calls ```showInsertLinkDialog```
This creates a new ```LinearLayout``` with standard 16dp padding, inflates two ```EditTexts``` inside the ```LinearLayout```, and then builds an ```AlertDialog``` with the layout.
When the positive button is clicked, the ```snippetEntered``` method is called on the listener, inserting the formatted URL.
3. The third button calls ```showImageUploadDialog``` on the ```EditorActivity``` parent
4. The fourth button inserts the the quadruple asterisks for bold text, and positions the cursor between the two pairs of asterisks.
5. The fifth button inserts the double asterisks for italic text, and positions the cursor between the two.
6. The sixth button inserts the quadruple tildes for strikethrough text, and positions the cursor between the two.
7. The seventh button inserts the ticked checkbox characters, and positions the cursor after them.
8. The eighth button inserts the empty checkbox characters, and positions the cursor after them.
9. The ninth button inserts the triple hypens for a thematic break between two newlines, and positions the cursor after them.
10. The tenth button inserts a spaced asterisk for a bullet point list item, and positions the cursor after it.
11. The eleventh button inserts the first item in a numbered list, and positions the cursor after it.
12. The twelfth button inserts the right chevron and positions the cursor after it.
13. The thirteenth button inserts the two sets of triple backticks for a code block, separated by two newlines, and positions the cursor between the two sets.
14. The fourteenth button launches the ```EmojiActivity``` from the parent ```Activity```.
15. The fifteenth button launches the ```CharacterActivity``` from the parent ```Activity```.


#### The EditorActivity

#import "app/src/main/java/com/tpb/projects/editors/EditorActivity.java"

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

#import "gitapi/src/main/java/com/tpb/github/data/Uploader.java"

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

#import "app/src/main/java/com/tpb/projects/editors/CharacterActivity.java"

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

1. The query is empty, in which case all of the positions are added to mWorkingPositions
2. The query starts with the last query, meaning that the user has types another character. In this case the method iterates through the currently filtered positions and only searches the characters at these positions in mCharacters for the new query, as if the other characters did not contain the shorter query, they will not contain this one.
3. Otherwise, the entire mCharacters list is searched, and the matching positions are added to mWorkingPositions.

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

#import "app/src/main/java/com/tpb/projects/editors/EmojiActivity.java"

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

### Implementations of EditorActivity

The ```EditorActivity``` is used across multiple objectives.

3.v.f, 3.v.g, 4.ii.b, 4.ii.c, 4.iii, 5.iii.b, 5.iii.c, 6.v, 6.vi, 6.vii, and 6.viii. 

The ```EditorActivity``` is used for editing cards, comments, issues, and projects.

#### CardEditor

The ```CardEditor``` deals with the creation and editing of cards, including creating cards from issues.

It inflates the ```ViewStub``` with a layout containing a ```MarkdownEditText```, a button used when showing the list of available issues for card creation, and a button for clearing any issue preview information shown.

#import "app/src/main/res/layout/stub_card_editor.xml"

The ```MarkdownEditText``` is wrapped in a ```TextInputLayout``` allowing the character limit to be shown above the ```EditText``` when the ```EditText``` is in use.

#import "app/src/main/java/com/tpb/projects/editors/CardEditor.java"

When the ```CardEditor``` ```onCreate``` method is called, the markdown_editor layout is inflated, the ```ViewStub``` is found and inflated with the ```card_editor``` stub layout, and the ```Views``` are found.

If the launch ```Intent``` contains a ```Card```, a ```Card``` is being edited, so the ```Card``` is stored, and the ```EditText``` text is set to the contents of the ```Card``` note.
Otherwise, a new ```Card``` is created and ```addFromIssueButtonListeners``` is called with the launch ```Intent```.

This collects the full repository name from the ```Intent```, as well as a list of issue ids which have already been used in cards, and therefore cannot be used again.
The issue button is made visible add its ```OnClickListener``` is set.
When the button is clicked:
1. A new ```ProgressDialog``` is created is created with a title telling the user that it is loading the ```Issues```.
2. When the ```Issues``` are loaded, the first check is that the ```Activity``` is not closing, in which case the listener returns. Next, the method iterates through all of the returned ```Issues```, adding them to a new list if their id is not in the list of invalid ids.
3. If the list of valid ```Issues``` is empty, a toast error message is displayed, telling the user that there are no valid ```Issues```. The method then returns.
4. Otherwise, a string array of the ```Issue``` titles is created, and a single choice dialog is created in which to display them.
5. When an ```Issue``` is selected
    1. ```setFromIssue``` is called on the ```Card``` instance, setting its note and ```Issue```.
    2. The ```InputFilters``` on the ```EditText``` are removed.
    3. The character counter on the ```EditText``` is disabled.
    4. The ```Issue``` is bound:
        1. The span is built with ```buildIssueSpan``` with flags to format the title as a header, insert the numbered link to the ```Issue```, show the assignees, show the time that the issue was closed, and not show the comment count.
    5. The ```MarkdownEditText``` is made non-focusable so that it cannot be clicked.
    6. The clear issue button is made visible so that the issue being previewed can be removed.

The ```OnClickListener``` is then set on the clear button.
This clears the text in the ```MarkdownEditText```, re-enables the 250 character limit, re-enables the counter, re-creates the ```Card```, and hides the clear issue button.

If the ```Issues``` do not load successfully, the dialog is dismissed, and a toast message is shown with the error resource for the ```APIError```.

Returning to the ```onCreate``` method, an anonymous ```MarkdownButtonAdapter``` is created with the ```CardEditor``` ```Activity```, the mEditButtons ```LinearLayout```, and an anonymous ```MarkdownButtonListener```.
The ```MarkdownButtonListener``` implements, ```snippetEntered```, which uses ```Util.insertString``` to insert the snippet at a relative position.
```getText``` returns the ```MarkdownEditText``` ```Editable``` as a string.
```previewCalled``` checks if the ```MarkdownEditText``` is currently in the editing state. If it is it:
1. Calls ```saveText``` on the ```MarkdownEditText``` to save the raw markdown
2. Calls ```disableEditing``` on the ```MarkdownEditText``` to disable any input
3. Sets the formatted markdown with a new HttpImageGetter with the ```MarkdownEditText``` as its container.
Otherwise it:
1. Calls ```restoreText``` to set the text back to the value saved in ```saveText```.
2. Calls ```enableEditing``` to re-enable input in the ```MarkdownEditText```.

An instance of ```KeyBoardVisibilityChecker``` is created and assigned to ```mKeyBoardChecker``` with the root content layout, and an anonymous ```KeyBoardVisibilityChecker``` which hides the issue button when the keyboard is shown, and shows it again after the keyboard is hidden, if it has a listener.

Finally, a ```SimpleTextChangeWatcher``` is added to the ```MarkdownEditText``` which ORs mHasBeenEdited with the ```MarkdownEditText``` editing state.

The ```CardEditor``` implements character and emoji insertion using the ```Util.insertString``` method.

When ```onDone``` is called, a new ```Intent``` is created, the ```Card``` note is set, and the ```Card``` is added to the ```Intent``` which is then set as the result.
The mHasBeenEdited flag is set to false, indicating that the content has not been updated since the last time that it was saved, and ```finish``` is called.

```finish``` checks if mHasBeenEdited is true, and the ```EditText``` is non-empty. If so, it displays a dialog asking the user to confirm that they wish to dismiss their changes.
If mHasBeenEdited is false, or the user chooses to dismiss their changes, the keyboard is dismissed and ```super.finish``` is called to close the ```Activity```.

#### CommentEditor

The ```CommentEditor``` is very similar to the ```CardEditor``` and deals with editing comments for issues and commits.

#import "app/src/main/java/com/tpb/projects/editors/CommentEditor.java"

The key differences are in the ```onDone``` and ```previewCalled``` methods.
```onDone``` passes the ```Issue``` with the ```Intent``` if it is non-null.
```previewCalled``` extracts the repository URL from the ```Issue``` if it exists, and uses it when formatting the markdown.

#### IssueEditor

The ```IssueEditor``` is more complex as it also deals with setting the labels and collaborators on an issue, as well as creating issues form cards.

#import "app/src/main/java/com/tpb/projects/editors/IssueEditor.java"

After inflating the layout and ```ViewStub``` layout, there are more checks to perform than in the other editors.

##### onCreate

Every time the ```IssueEditor``` is launched, a repository path is provided for loading the labels and collaborators.
If the ```Intent``` contains an ```Issue``` model:
1. The ```Issue``` is extracted to mLaunchIssue
2. The ```Card``` is extracted to mLaunchCard
3. The title and body ```EditTexts``` are set with the ```Issue``` title and body.
4. If the launch ```Issue``` has a list of assignees:
    1. The assignee logins are added to the mAssignees ```ArrayList```.
    2. ```setAssigneesText``` is called. (To be explained)
5. If the launch ```Issue``` has a non-null and non-empty list of ```Labels```:
    1. An ```ArrayList``` of ```Pairs``` of strings and integers is created from the labels array.
    2. ```setLabelsText``` is called (To be explained).

If the ```Intent``` contains a ```Card``` model:
1. The ```Card``` is extracted to mLaunchCard
2. The ```Card``` note is stored.
3. The index of the first newline in the note is found.
4. If the index is -1, the index is set to 137
5. If the index is less than the length of the note, the note is split between the title and body and ellipsized.
6. Otherwise the note is set as the title.

A ```SimpleTextChangeWatcher``` is then added to both the title and body ```EditTexts``` which ORs mHasBeenEdited with the body editing state.

A ```KeyBoardVisibilityChecker``` is then used to hide the layout containing information about the issues' labels and assignees when the keyboard is shown, and to show it again once the keyboard has been hidden.

Finally, the ```MarkdownButtonAdapter``` is created, which also deals with the title ```EditText``` in this case, enabling or disabling it along with the body ```MarkdownEditText``` as well as hiding the layout for labels and assigneees.

##### Choosing assignees

When the assignees button is clicked, a new ```ProgressDialog``` is created and shown while the assignees are loaded.
A call is then made to load the collaborators for the current repository.
Once the collaborators are loaded a new ```MultiChoiceDialog``` is created, and each of the collaborators names are added to an array, with the respective checked flag set dependent on whether the collaborator name is already in the mAssignees list.
The dialog listener is then set to clear the current list of assignees and add the checked assignees before calling ```setAssigneesText``` to display the updated list.

```setAssigneesText``` checks if the assignees list is empty, if so it hides the ```TextView```, otherwise it bilds a list of links to each of the assignees GitHub user pages.

##### Choosing labels

When the labels button is clicked, a new ```ProgressDialog``` is created and shown while the labels are loaded.
Arrays for the label texts, colours, and current choices are then created before being set on the ```MultiChoiceDialog```.
The ```MultiChoiceDialogListener``` is then set to clear the current list of label names, and then add the checked names to the list as well as building an array of pairs of label names and their respective colours.
```setLabelsText``` is then called to display the newly selected labels.
If the labels list is empty, the labels ```TextView``` is hidden, otherwise a ```StringBuilder``` is used to created a non-bulleted list of labels using ```Formatter.getLabelString``` to create each font tag with the correct text colour and background colour.

##### onDone

The ```onDone``` method follows the save pattern as the other ```onDone``` methods, except tha it also adds the assignees and labels to the array if they are not empty.

#### ProjectEditor

The ```ProjectEditor``` is relatively simple, as it only deals with the project title and description.
When it is launched, the ```Intent``` is checked for a ```Project``` extra which is used to set the name and description, as well as storing the ```Project``` id for returning to the launching ```Activity```.

#import "app/src/main/java/com/tpb/projects/editors/ProjectEditor.java"
#page

## User Activity

Once a user has logged in, their account can be displayed.
This is objective 2.

The immediate sub-objectives of objective 2 are written to represent the different components shown when displaying a user in a paged ```Activity``` as described in the proposed design.

Each section will be shown as a ```Fragment``` within a ```ViewPager```.
This makes the ```UserActivity``` layout and class simple, as most logic is kept separate, with each ```Fragment``` only concerned with its own purpose.

#import "app/src/main/res/layout/activity_user.xml"

The ```UserActivity``` layout contains an ```AppBarLayout``` allowing the ```Toolbar``` to scroll with the contents of any ```ScrollViews``` within the ```Fragments``` which are contained in the ```ViewPager```.

#import "app/src/main/java/com/tpb/projects/user/UserActivity.java"

#import "app/src/main/res/layout/fragment_user_info.xml"

When it is created, ```UserActivity``` calls the ```super``` method (```BaseActivity```) which performs the access check, allowing ```UserActivity``` to return if the app doesn't have an access token.
If this check passes, ```onCreate``` method continues by checking the theme theme before inflating the ```activity_user``` layout and bindings its ```Views```. 

Once the ```Views``` have been inflated, the ```UserActivity``` can continue by creating the ```FragmentPagerAdapter``` which is responsible for creating each of the ```Fragments```.

The ```UserActivity``` then determines whether it has been started from a link to a user, in which case the ```User``` must be loaded, or if the app is starting from the homescreen to display the authenticated user.

## UserFragment

```UserFragment``` is an abstract class extending ```ViewSafeFragment``` used to add a ```User``` model instance, the ```userLoaded``` method , as well as to save and restore the ```User``` instance when the ```Fragment``` is added to or removed from the back stack.

#import "app/src/main/java/com/tpb/projects/user/fragments/UserFragment.java"

### UserInfoFragment

The first ```Fragment``` to be displayed is the ```UserInfoFragment```.
The ```UserInfoFragment``` is to fulfill objective 2.a, displaying information about the user.

The ```UserInfoFragment``` layout has two cards, the first displaying text based information about the user, and the second displaying a graph of the users' contributions.

#import "app/src/main/res/layout/fragment_user_info.xml"

The layout included within the first ```CardView``` is the same layout used in ```LoginActivity``` to display the user's information

#import "app/src/main/res/layout/shard_user_info.xml"

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

#import "app/src/main/java/com/tpb/projects/user/fragments/UserInfoFragment.java $public View onCreateView$"

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

#import "app/src/main/java/com/tpb/projects/util/UI.java $public static void setDrawable$"

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

#import "contributionsview/src/main/java/com/tpb/contributionsview/ContributionsLoader.java"

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

#import "contributionsview/src/main/java/com/tpb/contributionsview/ContributionsView.java"

The ```ContributionsView``` has attributes for its background colour, text colour, text size, and whether months should be displayed. A default user login can also be set via XML.

As the ```View``` is being repeatedly drawn, it is important to ensure that as few objects as possible are allocated in the ```onDraw``` method.
The single ```Paint``` objects for text and day (rectangle) painting are therefore re-used, as are the ```Rectangle``` used to store the ```View``` dimensions and the second ```Rectangle``` used to store text bounds.
The ```Calendar``` used for parsing dates is static and used in all instances of ```ContributionsView```.

The ```ContributionsView``` implements ```ContributionsRequestListener``` and contains the ```loadContributions``` method, which creates an anonymous ```ContributionsLoader``` and
begins loading the contributions for the given user.
In the ```onResponse``` method the ```ContributionsView``` stores the ```ArrayList``` of contributions and calls the ```invalidate``` method, forcing a redraw.
It also checks if its listener has been set, and notifies it of the newly loaded ```ArrayList``` of ```ContributionsDay``` objects.

The ```onDraw``` method needs to deal with two states, either the ```ContributionsView``` has a non empty list of ```ContributionsDays``` or it does not.

#import "contributionsview/src/main/java/com/tpb/contributionsview/ContributionsView.java $protected void onDraw$"

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

#import "app/src/main/java/com/tpb/projects/user/fragments/UserInfoFragment.java $public void contributionsLoaded$"

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

#import "app/src/main/java/com/tpb/projects/markdown/Formatter.java $public static void displayUser$"

The method first finds the ```NetworkImageView``` to display the user's avatar, and the ```TextView``` to display their username.
Once the username and avatar URL have been bound, a ```LayoutParams``` instance is created to ensure that each ```TextView``` uses the same margins.

The ```getInfoTextView``` method takes the ```Context``` required to instantiate a ```View``` and a drawable resource id to display at the start of the ```TextView```.

#import "app/src/main/java/com/tpb/projects/markdown/Formatter.java $private static TextView getInfoTextView$"

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

#import "app/src/main/java/com/tpb/projects/user/fragments/UserInfoFragment.java $public void updated$"

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

#import "app/src/main/res/layout/fragment_recycler.xml"

The ```UserReposFragment``` contains very little logic, as it only has to load the ```User``` and then pass this ```User``` to the ```RecyclerView``` adapter which contains logic for
loading the user's repositories and binding them to a list of ```Views```.

#import "app/src/main/java/com/tpb/projects/user/fragments/UserReposFragment.java"

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

#import "app/src/main/java/com/tpb/projects/common/RepositoriesAdapter.java"

The ```RepositoriesAdapter``` is constructed with a parent ```Activity```, used for launching the ```RepoActivity``` when a repository is clicked or a user when a user icon is clicked, and a ```SwipeRefreshLayout``` which is used to refresh the ```RecyclerView```.

The ```Loader``` is created, and the ```SwipeRefreshLayout``` is set to refreshing.
The ```OnRefreshListener``` is set on the ```SwipeRefreshLayout``` to reset the adapter conditions when it is refreshed.
Finally, the ```RepoPinChecker``` for sorting ```Repositories``` by their pin status is created, and the authencitaed user login is loaded from ```GitHubSession```.

#### States

The adapter will begin loading ```Repositories``` once ```setUser``` is called.
This method sets the mUser login string, the mIsShowingStars flag, and then clears the current list of ```Repositories``` before calling ```loadReposForUser``` with the flag to reset the page.

When ```loadReposForUser``` is called, the mIsLoading flag is set to true, and the ```SwipeRefreshLayout``` is set to refreshing.
If resetPage is true, the mPage value is reset to 1 and mMaxPageReached is reset to false.

Next, there are three network calls which may be made:
1. ```loadStarredRepositories``` if mIsShowingStars is true.
2. ```loadRepositories``` without a user parameter if the current user login is the same as the authenticated user.
3. ```loadRepositories``` with a user parameter otherwise.

Finally, the ```RepoPinChecker``` key is set to the current user login.

When the ```Repositories``` are loaded, they are returned through the ```ListLoader``` interface method ```listLoadComplete```.
The ```SwipeRefreshLayout``` is disabled, and mIsLoading is set to false.
If the list of ```Repositories``` is not empty, the old length is stored.
If mIsShowingStars is true, all of the new ```Repositories``` are added.
Otherwise ```insertPinnedRepos``` is called which uses the ```RepoPinChecker``` ```isPinned``` method when iterating through each ```Repository``` and if it is pinned, adds it to the start of the list.
Finally, ```notifyItemRangeInserted``` is called.

Otherwise, mMaxPageReached is set to true.

##### insertPinnedRepos

```insertPinnedRepos``` serves two purposes, first ensuring that the pinned ```Repositories``` which have been loaded are added to the start of the adapter, and second loading any pinned repositories which are
not loaded in the first page of the the ```Repositories``` returned by GitHub.
If the page is 1, each ```Repository``` is added either to the start or end of the array, and these initial positions are then passed to the ```RepoPinChecker``` before ```ensureLoadOfPinnedRepos``` is called.
Otherwise, the ```Repositories``` are added to the end of the array if they do not already exist in the array.

```ensureLoadOfPinnedRepos``` iterates through each of the repository names returned by ```RepoPinChecker.findNonLoadedPinnedRepositories``` (That's a bit of a mouthful), and loads each of the ```Repositories``` individually, inserting them into the array and ```RepoPinChecker``` if they do not already exist there, and calling ```notifyItemInserted(0)```.

#### RepoPinChecker

The ```RepoPinChecker``` which has been reference above is a class which controls a ```SharedPreferences``` instance and manages the pinned repositories.
Each time a user is loaded in ```RepositoriesAdapter``` the ```SharedPreferences``` is opened with the "PINS" key, and from this map a delimited string is loaded using the user login as a key.
This allows different repositories to be pinned for each user.

 The ```RepoPinChecker``` contains two ```ArrayLists```, one containing the names of the pinned repositories, and the other containing the names of the repositories loaded in their initial order, allowing them to be returned to these positions if they are unpinned.

When ```setKey``` is called, the KEY is st, and a string array is loaded with ```Util.stringArrayFromPrefs``` which loads the string and splits it around each comma.

When ```pin``` is called, if the repository is not already pinned, the repository name is added to the pins list and the new string from ```Util.stringArrayForPrefs``` is written to the ```SharedPreferences```.

When ```unpin``` is called, the repository is removed from the pins list, and the new string is again written to the ```SharedPreferences```.

```findNonLoadedPinnedRepositories``` checks each value in pins and if it is not in mInitialPositions, it is added to the list of non loaded pinned repositories which is then returned.

#### Binding

The ```RepoHolder``` sets ```OnClickListeners``` on its elements, and if the mIsShowingStars flag is false, it sets an ```OnClickListener``` on the ```NetworkImageView``` which would otherwise be used for displaying the user avatar. This listener calls ```togglePIn```, flips the isPinned flag and switches the image resource from the pinned to not pinned drawable.

### UserStarsFragment

The ```UserStarsFragment``` is very simple as all of the view logic is handled in the ```RepositoriesAdapter```.
The ```Fragment``` is only responsible for creating the layout, handling its own lifecycle, and notifying the adapter of scroll changes in the ```RecyclerView```.

#import "app/src/main/java/com/tpb/projects/user/fragments/UserStarsFragment.java"

### UserGistsFragment

#import "app/src/main/java/com/tpb/projects/user/fragments/UserGistsFragment.java"

### UserFollowingFragment

#import "app/src/main/java/com/tpb/projects/user/fragments/UserFollowingFragment.java"

### UserFollowersFragment

#import "app/src/main/java/com/tpb/projects/user/fragments/UserFollowersFragment.java"

## Search

#import "app/src/main/java/com/tpb/projects/util/search/FuzzyStringSearcher.java"

#import "app/src/main/java/com/tpb/projects/util/search/ArrayFilter.java"

## RepoActivity

#import "app/src/main/java/com/tpb/projects/repo/RepoActivity.java"

### RepoInfoFragment

#import "app/src/main/java/com/tpb/projects/repo/fragments/RepoInfoFragment.java"

### RepoReadmeFragment

#import "app/src/main/java/com/tpb/projects/repo/fragments/RepoReadmeFragment.java"

### RepoCommitsFragment

#import "app/src/main/java/com/tpb/projects/repo/fragments/RepoCommitsFragment.java"

#import "app/src/main/java/com/tpb/projects/repo/RepoCommitsAdapter.java"

### RepoIssuesFragment

#import "app/src/main/java/com/tpb/projects/repo/fragments/RepoIssuesFragment.java"

#import "app/src/main/java/com/tpb/projects/repo/RepoIssuesAdapter.java"

### RepoProjectsFragment

#import "app/src/main/java/com/tpb/projects/repo/fragments/RepoProjectsFragment.java"

#import "app/src/main/java/com/tpb/projects/repo/RepoProjectsAdapter.java"

## ContentActivity

#import "app/src/main/java/com/tpb/projects/repo/content/ContentActivity.java"

### ContentAdapter

#import "app/src/main/java/com/tpb/projects/repo/content/ContentAdapter.java"

### FileActivity

#import "app/src/main/java/com/tpb/projects/repo/content/FileActivity.java"

## ProjectActivity

#import "app/src/main/java/com/tpb/projects/project/ProjectActivity.java"

### ColumnFragment

#import "app/src/main/java/com/tpb/projects/project/ColumnFragment.java"

#### CardAdapter

#import "app/src/main/java/com/tpb/projects/project/CardAdapter.java"

#### CardDragListener

#import "app/src/main/java/com/tpb/projects/project/CardDragListener.java"

#### ProjectSearchAdapter

#import "app/src/main/java/com/tpb/projects/project/ProjectSearchAdapter.java"

## CommitActivity

#import "app/src/main/java/com/tpb/projects/commits/CommitActivity.java"

### CommitInfoFragment

#import "app/src/main/java/com/tpb/projects/commits/fragments/CommitInfoFragment.java"

#### CommitDiffAdapter

#import "app/src/main/java/com/tpb/projects/commits/CommitDiffAdapter.java"

### CommitCommentsFragment

#import "app/src/main/java/com/tpb/projects/commits/fragments/CommitCommentsFragment.java"

#### CommitCommentsAdapter

#import "app/src/main/java/com/tpb/projects/commits/CommitCommentsAdapter.java"

## IssueActivity

#import "app/src/main/java/com/tpb/projects/issues/IssueActivity.java"

### IssueInfoFragment

#import "app/src/main/java/com/tpb/projects/issues/fragments/IssueInfoFragment.java"

#### IssueEventsAdapter

#import "app/src/main/java/com/tpb/projects/issues/IssueEventsAdapter.java"

### IssueCommentsFragment

#import "app/src/main/java/com/tpb/projects/issues/fragments/IssueCommentsFragment.java"

#### IssueCommentsAdapter

#import "app/src/main/java/com/tpb/projects/issues/IssueCommentsAdapter.java"

## Notifications

### NotificationServiceStarterReceiver

#import "app/src/main/java/com/tpb/projects/notifications/receivers/NotificationServiceStarterReceiver.java"

### NotificationEventReceiver

#import "app/src/main/java/com/tpb/projects/notifications/receivers/NotificationEventReceiver.java"

### NotificationIntentService

#import "app/src/main/java/com/tpb/projects/notifications/NotificationIntentService.java"

# Testing

## Markdown

Throughout the construction of the project I added test cards to a GitHub project in my test repository.

These cards test each of the different markdown features which should be included.

### Objective 9.ii.a: Code blocks

#### Short code blocks

Objective 9.ii.a.1 is to display short code blocks within the text body, including code inline with other text.

A short code block is any code block with fewer than 10 lines.

The test card for short code blocks contains the following items which should be displayed as short code blocks:

- A short code block at the start of the text
- A short code block in the middle of a line, between other text
- A short code block within a list item
- A short code block with ten lines
- A short code block at the end of the text

The card used to test this is 

\```
System.out.println("Line");
System.out.println("Line 2");
System.out.println("Line 3");
\```

Some text with a \```print("Code block")\``` in the middle.

A list:
- Item 1
- Item 2 \```print("With code")\```

A block with 10 lines:
\```
print("Line")
print("Line 2")
print("Line 3")
print("Line 4")
print("Line 5")
print("Line 6")
print("Line 7")
print("Line 8")
print("Line 9")
print("Line 10")
\```

Text below
\```
print("This is the end code block.")
\```

| Test | Expected Result | Result |
| --- | --- | --- |
| Block at start of text | Block shown with gray background | Pass |
| Block in the middle of a line | Only the part of the line is shown with a gray background | Pass |
| Block in list item | Only the code part of the list item is shown with a gray background | Pass |
| Block with ten lines | Block shown with all ten lines within the text | Pass | 
| Block at end of text | Block shown with gray background | Pass |

The card is displayed as shown below:

![InlineCode test card](http://imgur.com/uzxPmU8.png)

#### Larger code blocks

Objective 9.ii.a.2 is to display large code blocks as placeholders within the text body, allowing them to be clicked to show the full code block.

A large code block is any code block with more than 10 lines.

The test card for large code blocks contains the following items which should be displayed as large code blocks:
- A large code block at the start of the text
- A large code block with a language
- A large code block without a language
- A large code block with eleven lines
- A large code block at the end of the text

The card text used to test this is:

\``` java
System.out.println("Line");
System.out.println("Line 2");
System.out.println("Line 3");
System.out.println("Line 4");
System.out.println("Line 5");
System.out.println("Line 6");
System.out.println("Line 7");
System.out.println("Line 8");
System.out.println("Line 9");
System.out.println("Line 10");
System.out.println("Line 11");
System.out.println("Line 12");
\```

A block with 11 lines and no language:
\```
print("Line")
print("Line 2")
print("Line 3")
print("Line 4")
print("Line 5")
print("Line 6")
print("Line 7")
print("Line 8")
print("Line 9")
print("Line 10")
print("Line 11")
\```

The end block (Actual code)
\```Python
import time

import cv2

cap = cv2.VideoCapture()

print(cap.open(0))

while True:
    start = time.time()
    ret, frame = cap.read()

    cv2.imshow("Image", frame)
    k = cv2.waitKey(33)
    if k != -1:
        break
    print("FPS = " + str((1.0/(time.time()-start))))

cv2.destroyAllWindows()
cap.release()
\```

| Test | Expected Result | Result |
| --- | --- | --- |
| Block at start of text | Block placeholder shown | Pass |
| Block with a language | Block placeholder shown with corresponding language | Pass |
| Block without language | Block shown without language | Pass | 
| Block clicked | Code dialog shown | Pass |
| Block at end of text | Block placeholder shown | Pass |

The code blocks and code dialog are shown as below:

| Code blocks | Code dialog |
| --- | --- | 
| ![Blocks](http://imgur.com/KFszbeL.png) | ![Dialog](http://imgur.com/yD7hjM3.png) |

### Objective 9.ii.b: Horizontal rules

Horizontal rules should be displayed in the place of any of the three character combinations "\---", "\***", or "\___".

The test card for horizontal rules tests the following which should display a horizontal rule:
- Any of the valid combinations on its own line in the text
- Any of the valid combinations at the start of the text
- Any of the valid combinations at the end of the text

And the following which should not display a horizontal rule:
- Any of the valid combinations in the middle of a line of text
- Any of the valid combinations preceded by a backslash

The card text used to test this is:

\---

Thematic breaks

\___

Do underscores work?

\---


Do dashes work?


\***

Do asterisks work?


Escaped values:

\\***

\\___

\\---

\---

| Test | Expected Result | Result |
| --- | --- | --- |
| Valid combination at start of text | Horizontal rule displayed | Pass |
| Valid combination on its own line | Horizontal rule displayed | Pass |
| Valid combination at the end of text | Horizontal rule displayed | Pass |
| Escaped combinations | Horizontal rule not displayed | Pass |

The card is displayed as shown below:

![Thematic break test](http://imgur.com/KdTXSUQ.png)

### Objective 9.ii.c: Image handling and relative link handling 

Images with a relative path in the repository should be shown.

In order to test this I added images of each type (png, jpg, gif, bmp, and webp) to the test repository and added relative links to the README as well as a relative path.

```

![Test 1](./test_1.png)

![Test 2](/test_2.jpg)

![Test 3](test_3.bmp)

![Test_4](./test_4.gif)

![Test 5](/test_5.webp)
```

The links are either the file name, a single forward slash, or a dot slash, which are all valid relative path formats.

The sets of images displayed correctly in both the ```MarkdownWebView``` and ```MarkdownTextView```.

Next, I moved the images to a directory within the repository and updated the links.

```
![Test 1](./test/test_1.png)

![Test 2](/test/test_2.jpg)

![Test 3](test/test_3.bmp)

![Test_4](./test/test_4.gif)

![Test 5](/test/test_5.webp)
```

The images were still displayed in the same manner as before.

| ```MarkdownWebView``` | ```MarkdownTextView```
| --- | --- |
| ![WV](http://imgur.com/B4aqUDZ.png) | ![TV](http://imgur.com/ClujAd6.png)



### Objective 9.ii.d: Username linking

The test card for username linking tests the following items which should be parsed as username links:

- Usernames linked at the start of the text
- Usernames with a single hyphen
- Usernames at the end of the text

And the following which should not:

- Usernames with two hyphens
- An email
- Usernames longer than 38 characters
- Usernames with invalid characters
- A single "@" character (0 length username)

| Test | Expected Result | Result |
| --- | --- | --- |
| Username at the start of the text | Converted to link | Pass |
| Username with a hyphen | Converted to link | Pass |
| Username at the end of the text | Converted to link | Pass |
| Username with two hypens | Not converted to link | Pass |
| Email address | Converted to email link | Pass | 
| Username longer than 38 characters | Not converted to link | Pass |
| Username with invalid characters | Not converted to link | Pass |
| Single "@" character | Not converted to link | Pass |

The display within the app is shown below:

![Username parsing test](http://imgur.com/KZlnSnV.png)

### Objective 9.ii.e: Issue linking

The test card for issue linking tests the following items which should be parsed as issue links:

- An issue at the start of the text
- An issue on its own line
- An issue between two other blocks of text
- An issue at the end of the text

And the following which should not:

- An issue which is conjoined with a word
- An issue with a negative number

| Test | Expected result | Result |
| --- | --- | --- |
| Issue at the start of the text | Converted to link | Pass |
| Issue on its own line | Converted to link | Pass
| Issue between two other blocks of text | Converted to link | Pass |
| Issue at the end of the text | Converted to link | Pass |
| Issue which is conjoined with a word | Not converted to a link | Pass |
| Issue with a negative number | Not converted to a link | Pass |

The display within the app is shown below:

![Issue parsing test](http://imgur.com/dnSAMmp.png)

### Objective 9.ii.f: Emoji parsing

The test card for emoji parsing tests a list of emoji aliases which should be converted to their respective emoji characters.

The raw text is below:

```
People

 :smile:   :laughing:
:blush:   :smiley:   :relaxed:
:smirk:   :heart_eyes:   :kissing_heart:
:kissing_closed_eyes:   :flushed:   :relieved:
:satisfied:   :grin:   :wink:
:stuck_out_tongue_winking_eye:   :stuck_out_tongue_closed_eyes:   :grinning:
:kissing:   :kissing_smiling_eyes:   :stuck_out_tongue:
:sleeping:   :worried:   :frowning:
:anguished:   :open_mouth:   :grimacing:
:confused:   :hushed:   :expressionless:
:unamused:   :sweat_smile:   :sweat:
:disappointed_relieved:   :weary:   :pensive:
:disappointed:   :confounded:   :fearful:
:cold_sweat:   :persevere:   :cry:
:sob:   :joy:   :astonished:
:scream:  :tired_face:
:angry:   :rage:   :triumph:
:sleepy:   :yum:   :mask:
:sunglasses:   :dizzy_face:   :imp:
:smiling_imp:   :neutral_face:   :no_mouth:
:innocent:   :alien:   :yellow_heart:
:blue_heart:   :purple_heart:   :heart:
:green_heart:   :broken_heart:   :heartbeat:
:heartpulse:   :two_hearts:   :revolving_hearts:
:cupid:   :sparkling_heart:   :sparkles:
:star:   :star2:   :dizzy:
:boom:   :collision:   :anger:
:exclamation:   :question:   :grey_exclamation:
:grey_question:   :zzz:   :dash:
:sweat_drops:   :notes:   :musical_note:
:fire:   :hankey:   :poop:
:shit:   :+1:   :thumbsup:
:1:   :thumbsdown:   :ok_hand:
:punch:   :facepunch:   :fist:
:v:   :wave:   :hand:
:raised_hand:   :open_hands:   :point_up:
:point_down:   :point_left:   :point_right:
:raised_hands:   :pray:   :point_up_2:
:clap:   :muscle:   :metal:   :walking:   :runner:
:running:   :couple:   :family:
:two_men_holding_hands:   :two_women_holding_hands:   :dancer:
:dancers:   :ok_woman:   :no_good:
:information_desk_person:   :raising_hand:   :bride_with_veil:
:person_with_pouting_face:   :person_frowning:   :bow:
:couplekiss:   :couple_with_heart:   :massage:
:haircut:   :nail_care:   :boy:
:girl:   :woman:   :man:
:baby:   :older_woman:   :older_man:
:person_with_blond_hair:   :man_with_gua_pi_mao:   :man_with_turban:
:construction_worker:   :cop:   :angel:
:princess:   :smiley_cat:   :smile_cat:
:heart_eyes_cat:   :kissing_cat:   :smirk_cat:
:scream_cat:   :crying_cat_face:   :joy_cat:
:pouting_cat:   :japanese_ogre:   :japanese_goblin:
:see_no_evil:   :hear_no_evil:   :speak_no_evil:
:guardsman:   :skull:   :feet:
:lips:   :kiss:   :droplet:
:ear:   :eyes:   :nose:
:tongue:   :love_letter:   :bust_in_silhouette:
:busts_in_silhouette:   :speech_balloon:   :thought_balloon:
:feelsgood:   :rage1:
:rage2:   :rage3:   :rage4:

Nature

:sunny:   :umbrella:   :cloud:
:snowflake:   :snowman:   :zap:
:cyclone:   :foggy:   :ocean:
:cat:   :dog:   :mouse:
:hamster:   :rabbit:   :wolf:
:frog:   :tiger:   :koala:
:bear:   :pig:   :pig_nose:
:cow:   :boar:   :monkey_face:
:monkey:   :horse:   :racehorse:
:camel:   :sheep:   :elephant:
:panda_face:   :snake:   :bird:
:baby_chick:   :hatched_chick:   :hatching_chick:
:chicken:   :penguin:   :turtle:
:bug:   :honeybee:   :ant:
:beetle:   :snail:   :octopus:
:tropical_fish:   :fish:   :whale:
:whale2:   :dolphin:   :cow2:
:ram:   :rat:   :water_buffalo:
:tiger2:   :rabbit2:   :dragon:
:goat:   :rooster:   :dog2:
:pig2:   :mouse2:   :ox:
:dragon_face:   :blowfish:   :crocodile:
:dromedary_camel:   :leopard:   :cat2:
:poodle:   :paw_prints:   :bouquet:
:cherry_blossom:   :tulip:   :four_leaf_clover:
:rose:   :sunflower:   :hibiscus:
:maple_leaf:   :leaves:   :fallen_leaf:
:herb:   :mushroom:   :cactus:
:palm_tree:   :evergreen_tree:   :deciduous_tree:
:chestnut:   :seedling:   :blossom:
:ear_of_rice:   :shell:   :globe_with_meridians:
:sun_with_face:   :full_moon_with_face:   :new_moon_with_face:
:new_moon:   :waxing_crescent_moon:   :first_quarter_moon:
:waxing_gibbous_moon:   :full_moon:   :waning_gibbous_moon:
:last_quarter_moon:   :waning_crescent_moon:   :last_quarter_moon_with_face:
:first_quarter_moon_with_face:   :moon:   :earth_africa:
:earth_americas:   :earth_asia:   :volcano:
:milky_way:   :partly_sunny: :squirrel:

Objects

:bamboo:   :gift_heart:   :dolls:
:school_satchel:   :mortar_board:   :flags:
:fireworks:   :sparkler:   :wind_chime:
:rice_scene:   :jack_o_lantern:   :ghost:
:santa:   :christmas_tree:   :gift:
:bell:   :no_bell:   :tanabata_tree:
:tada:   :confetti_ball:   :balloon:
:crystal_ball:   :cd:   :dvd:
:floppy_disk:   :camera:   :video_camera:
:movie_camera:   :computer:   :tv:
:iphone:   :phone:   :telephone:
:telephone_receiver:   :pager:   :fax:
:minidisc:   :vhs:   :sound:
:speaker:   :mute:   :loudspeaker:
:mega:   :hourglass:   :hourglass_flowing_sand:
:alarm_clock:   :watch:   :radio:
:satellite:   :loop:   :mag:
:mag_right:   :unlock:   :lock:
:lock_with_ink_pen:   :closed_lock_with_key:   :key:
:bulb:   :flashlight:   :high_brightness:
:low_brightness:   :electric_plug:   :battery:
:calling:   :email:   :mailbox:
:postbox:   :bath:   :bathtub:
:shower:   :toilet:   :wrench:
:nut_and_bolt:   :hammer:   :seat:
:moneybag:   :yen:   :dollar:
:pound:   :euro:   :credit_card:
:money_with_wings:   :email:   :inbox_tray:
:outbox_tray:   :envelope:   :incoming_envelope:
:postal_horn:   :mailbox_closed:   :mailbox_with_mail:
:mailbox_with_no_mail:   :door:   :smoking:
:bomb:   :gun:   :hocho:
:pill:   :syringe:   :page_facing_up:
:page_with_curl:   :bookmark_tabs:   :bar_chart:
:chart_with_upwards_trend:   :chart_with_downwards_trend:   :scroll:
:clipboard:   :calendar:   :date:
:card_index:   :file_folder:   :open_file_folder:
:scissors:   :pushpin:   :paperclip:
:black_nib:   :pencil2:   :straight_ruler:
:triangular_ruler:   :closed_book:   :green_book:
:blue_book:   :orange_book:   :notebook:
:notebook_with_decorative_cover:   :ledger:   :books:
:bookmark:   :name_badge:   :microscope:
:telescope:   :newspaper:   :football:
:basketball:   :soccer:   :baseball:
:tennis:   :8ball:   :rugby_football:
:bowling:   :golf:   :mountain_bicyclist:
:bicyclist:   :horse_racing:   :snowboarder:
:swimmer:   :surfer:   :ski:
:spades:   :hearts:   :clubs:
:diamonds:   :gem:   :ring:
:trophy:   :musical_score:   :musical_keyboard:
:violin:   :space_invader:   :video_game:
:black_joker:   :flower_playing_cards:   :game_die:
:dart:   :mahjong:   :clapper:
:memo:   :pencil:   :book:
:art:   :microphone:   :headphones:
:trumpet:   :saxophone:   :guitar:
:shoe:   :sandal:   :high_heel:
:lipstick:   :boot:   :shirt:
:tshirt:   :necktie:   :womans_clothes:
:dress:   :running_shirt_with_sash:   :jeans:
:kimono:   :bikini:   :ribbon:
:tophat:   :crown:   :womans_hat:
:mans_shoe:   :closed_umbrella:   :briefcase:
:handbag:   :pouch:   :purse:
:eyeglasses:   :fishing_pole_and_fish:   :coffee:
:tea:   :sake:   :baby_bottle:
:beer:   :beers:   :cocktail:
:tropical_drink:   :wine_glass:   :fork_and_knife:
:pizza:   :hamburger:   :fries:
:poultry_leg:   :meat_on_bone:   :spaghetti:
:curry:   :fried_shrimp:   :bento:
:sushi:   :fish_cake:   :rice_ball:
:rice_cracker:   :rice:   :ramen:
:stew:   :oden:   :dango:
:egg:   :bread:   :doughnut:
:custard:   :icecream:   :ice_cream:
:shaved_ice:   :birthday:   :cake:
:cookie:   :chocolate_bar:   :candy:
:lollipop:   :honey_pot:   :apple:
:green_apple:   :tangerine:   :lemon:
:cherries:   :grapes:   :watermelon:
:strawberry:   :peach:   :melon:
:banana:   :pear:   :pineapple:
:sweet_potato:   :eggplant:   :tomato:
:corn:

Places

:house:   :house_with_garden:   :school:
:office:   :post_office:   :hospital:
:bank:   :convenience_store:   :love_hotel:
:hotel:   :wedding:   :church:
:department_store:   :european_post_office:   :city_sunrise:
:city_sunset:   :japanese_castle:   :european_castle:
:tent:   :factory:   :tokyo_tower:
:japan:   :mount_fuji:   :sunrise_over_mountains:
:sunrise:   :stars:   :statue_of_liberty:
:bridge_at_night:   :carousel_horse:   :rainbow:
:ferris_wheel:   :fountain:   :roller_coaster:
:ship:   :speedboat:   :boat:
:sailboat:   :rowboat:   :anchor:
:rocket:   :airplane:   :helicopter:
:steam_locomotive:   :tram:   :mountain_railway:
:bike:   :aerial_tramway:   :suspension_railway:
:mountain_cableway:   :tractor:   :blue_car:
:oncoming_automobile:   :car:   :red_car:
:taxi:   :oncoming_taxi:   :articulated_lorry:
:bus:   :oncoming_bus:   :rotating_light:
:police_car:   :oncoming_police_car:   :fire_engine:
:ambulance:   :minibus:   :truck:
:train:   :station:   :train2:
:bullettrain_front:   :bullettrain_side:   :light_rail:
:monorail:   :railway_car:   :trolleybus:
:ticket:   :fuelpump:   :vertical_traffic_light:
:traffic_light:   :warning:   :construction:
:beginner:   :atm:   :slot_machine:
:busstop:   :barber:   :hotsprings:
:checkered_flag:   :crossed_flags:   :izakaya_lantern:
:moyai:   :circus_tent:   :performing_arts:
:round_pushpin:   :triangular_flag_on_post:   :jp:
:kr:   :cn:   :us:
:fr:   :es:   :it:
:ru:   :gb:   :uk:
:de:

Symbols

:one:   :two:   :three:
:four:   :five:   :six:
:seven:   :eight:   :nine:
:keycap_ten:   :1234:   :zero:
:hash:   :symbols:   :arrow_backward:
:arrow_down:   :arrow_forward:   :arrow_left:
:capital_abcd:   :abcd:   :abc:
:arrow_lower_left:   :arrow_lower_right:   :arrow_right:
:arrow_up:   :arrow_upper_left:   :arrow_upper_right:
:arrow_double_down:   :arrow_double_up:   :arrow_down_small:
:arrow_heading_down:   :arrow_heading_up:   :leftwards_arrow_with_hook:
:arrow_right_hook:   :left_right_arrow:   :arrow_up_down:
:arrow_up_small:   :arrows_clockwise:   :arrows_counterclockwise:
:rewind:   :fast_forward:   :information_source:
:ok:   :twisted_rightwards_arrows:   :repeat:
:repeat_one:   :new:   :top:
:up:   :cool:   :free:
:ng:   :cinema:   :koko:
:signal_strength:   :u5272:   :u5408:
:u55b6:   :u6307:   :u6708:
:u6709:   :u6e80:   :u7121:
:u7533:   :u7a7a:   :u7981:
:sa:   :restroom:   :mens:
:womens:   :baby_symbol:   :no_smoking:
:parking:   :wheelchair:   :metro:
:baggage_claim:   :accept:   :wc:
:potable_water:   :put_litter_in_its_place:   :secret:
:congratulations:   :m:   :passport_control:
:left_luggage:   :customs:   :ideograph_advantage:
:cl:   :sos:   :id:
:no_entry_sign:   :underage:   :no_mobile_phones:
:do_not_litter:    :no_bicycles:
:no_pedestrians:   :children_crossing:   :no_entry:
:eight_spoked_asterisk:   :eight_pointed_black_star:   :heart_decoration:
:vs:   :vibration_mode:   :mobile_phone_off:
:chart:   :currency_exchange:   :aries:
:taurus:   :gemini:   :cancer:
:leo:   :virgo:   :libra:
:scorpius:   :sagittarius:   :capricorn:
:aquarius:   :pisces:   :ophiuchus:
:six_pointed_star:   :negative_squared_cross_mark:   :a:
:b:   :ab:   :o2:
:diamond_shape_with_a_dot_inside:   :recycle:   :end:
:on:   :soon:   :clock1:
:clock130:   :clock10:   :clock1030:
:clock11:   :clock1130:   :clock12:
:clock1230:   :clock2:   :clock230:
:clock3:   :clock330:   :clock4:
:clock430:   :clock5:   :clock530:
:clock6:   :clock630:   :clock7:
:clock730:   :clock8:   :clock830:
:clock9:   :clock930:   :heavy_dollar_sign:
:copyright:   :registered:   :tm:
:x:   :heavy_exclamation_mark:   :bangbang:
:interrobang:   :o:   :heavy_multiplication_x:
:heavy_plus_sign:   :heavy_minus_sign:   :heavy_division_sign:
:white_flower:   :100:   :heavy_check_mark:
:ballot_box_with_check:   :radio_button:   :link:
:curly_loop:   :wavy_dash:   :part_alternation_mark:
:trident:   :black_square:   :white_square:
:white_check_mark:   :black_square_button:   :white_square_button:
:black_circle:   :white_circle:   :red_circle:
:large_blue_circle:   :large_blue_diamond:   :large_orange_diamond:
:small_blue_diamond:   :small_orange_diamond:   :small_red_triangle:
:small_red_triangle_down:
```

This contains all of the emohi which can be used on GitHub.

When displayed in a ```MarkdownTextView``` or ```MarkdownEditText``` it should be shown with each emoji as a single character.

The Emoji are displayed as shown below:

| | | 
| --- | --- | 
| ![First section](http://imgur.com/NWrQwLE.png) | ![Second section](http://imgur.com/zhDHYYm.png)

There are some emoji which are not shown with a character not found symbol. This is a limitation of the font used rather than the emoji parsing, as the original emoji aliases have been removed.

### Objective 9.ii.g: Checkboxes

The following sets of characters should be converted to unicode checkbox characters
- "[]" should be converted to ☐
- "[ ]" should be converted to ☐
- "[x]" should be converted to ☑

The checkbox test card contains the following which should be parsed to unicode ballot box characters:

- "[x]" at the start of the text immediately followed by an alphabet character
- "[]" on its own line
- "[ ]" at the start of a list line
- "[x]" at the start of a list line
- "[]" between two spaces within line
- "[x]" at the end of a line

And the following which should not:
- "\[]" escaped ballot
- "\[ ]" escaped ballot
- "\[x]" escaped ballot

The test note is shown below 

```
[x]Test for formatting checkboxes

Ballot on its own:

[]

Ballots at start of list

- [ ] Some text
- [x] Some more text
 

Ballot between text [] some more text

And the others  [ ] and [x]


Escape ballots:
\[]
\[ ]
\[x]

Ending ballot:

[X]
```

| Test | Expected result | Result |
| --- | --- | --- | 
| "[x]" at the start of the text | Unicode U+2611 character | Pass |
| "[]" on its own line | Unicode U+2610 character | Pass |
| "[ ]" at the start of a list line | Unicode U+2610 character | Pass |
| "[x]" at the start of a list line | Unicode U+2611 character | Pass |
| "[]" between text in a line | Unicode U+2610 character | Pass |
| "\[]" | "[]" | Pass |
| "\[ ]" | "[ ]" | Pass |
| "\[x]" | "[x]" | Pass |
| "[x]" at the end of the text | Unicode U+2611 character | Pass |

The card was displayed as shown below:

![Ballot text](http://imgur.com/NsNebAc.png)

### Objective 9.ii.h: Text background colours

### Objective 9.ii.i: Table placeholders


## 