 
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

The ```Activity``` will stay in the resumed state until it exits it, or another ```Activity```` java
package com.tpb.mdtext;

import android.graphics.Color;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.text.Editable;
import android.text.Spannable;
import android.text.Spanned;
import android.text.style.BackgroundColorSpan;
import android.text.style.ForegroundColorSpan;

import com.tpb.mdtext.views.spans.CleanURLSpan;
import com.tpb.mdtext.views.spans.RoundedBackgroundEndSpan;

import java.util.Map;
import java.util.Set;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

/**
 * Created by theo on 21/03/17.
 */

public class TextUtils {

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

    public static boolean addLinks(@NonNull Spannable spannable, @NonNull Pattern pattern) {
        boolean hasMatches = false;
        final Matcher m = pattern.matcher(spannable);

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

    public static void addRoundedBackgroundSpan(Editable editable, String subsequence, int bg) {
        final int start = editable.length();
        editable.append(" ");
        editable.append(subsequence);
        editable.append(" ");
        editable.setSpan(
                new RoundedBackgroundEndSpan(bg, false),
                start,
                start + 1,
                Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        editable.setSpan(
                new BackgroundColorSpan(bg),
                start,
                editable.length(),
                Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        editable.setSpan(
                new ForegroundColorSpan(
                        TextUtils.getTextColorForBackground(bg)
                ),
                start,
                editable.length(),
                Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);

        editable.setSpan(
                new RoundedBackgroundEndSpan(bg, true),
                editable.length() - 1,
                editable.length(),
                Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
    }

    public static int getTextColorForBackground(int bg) {
        double r = Color.red(bg) / 255d;
        if(r <= 0.03928) {
            r = r / 12.92;
        } else {
            r = Math.pow((r + 0.055) / 1.055, 2.4);
        }
        double g = Color.green(bg) / 255d;
        if(g <= 0.03928) {
            g = g / 12.92;
        } else {
            g = Math.pow((g + 0.055) / 1.055, 2.4);
        }
        double b = Color.blue(bg) / 255d;
        if(b <= 0.03928) {
            b = b / 12.92;
        } else {
            b = Math.pow((b + 0.055) / 1.055, 2.4);
        }
        return (0.2126 * r + 0.7152 * g + 0.0722 * b) > 0.35 ? Color.BLACK : Color.WHITE;
    }

}

```