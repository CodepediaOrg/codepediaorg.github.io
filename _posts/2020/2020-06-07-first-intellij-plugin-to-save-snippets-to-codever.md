---
layout: post
title: My first IntelliJ plugin to help me save code snippets to Codever
description: "Shows the basic steps needed to create a simple IntelliJ plugins which eases the saving
 of code snippets to www.codever.land"
author: ama
permalink: /ama/my-first-intellij-plugin-to-help-me-bookmark-code
published: true
categories: [tutorial]
tags:
    - intellij
    - plugins
    - codever
    - productivity
---

So, I wrote my first IntelliJ plugin to help me save code snippets, aka codelets, easier to
[www.codever.land](https://www.codever.land). How does it work? **Select text** > **Right mouse click** > **Save to Bookmarks.dev**.

 ![Save to Codever showcase](/images/posts/2020-06-07-first-intellij-plugin/plugin-showcase-save-shorter-cover-800x456.gif)

> The plugin is available to download and install in
> [JetBrain Plugins Repository](https://plugins.jetbrains.com/plugin/14456-codever-snippets/) and the
> source code is available on [Github](https://github.com/codeverland/codever-intellij-plugin)

In this blog post we'll cover the steps to develop this simple IntelliJ plugin.

* TOC
{:toc}

<!--more-->

## Creating the plugin - generate new project for the plugin
Start with the [Plugin DevKit](https://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/using_dev_kit.html),
 which is a bundled IntelliJ IDEA plugin for developing plugins for the IntelliJ Platform using IntelliJ IDEA’s own build system.
  It provides its custom SDK type and a set of actions for building plugins within the IDE.

### Creating a Plugin Project
 In IntelliJ, on the main menu, choose **File \| New \| Project**. The *New Project* wizard starts
 and select IntelliJ PLatform Plugin

 ![New Plugin Project ](/images/posts/2020-06-07-first-intellij-plugin/new-plugin-project.png)

 Once you select a name (in our case `codever-intellij-plugin`), go to **File \| Project Structure**
 and select the *IntelliJ Platform SDK* as the default SDK for the plugin module

## The plugin implementation

To achieve the goal of the plugin we need to define a custom [action](https://www.jetbrains.org/intellij/sdk/docs/tutorials/action_system/working_with_custom_actions.html)

> I hope you are all aware of the **`Ctrl/Cmd+Shift+A`** shortcut to search and call
> all the possible actions in IntelliJ

### Creating the Custom Action

> Before going any further I recommend you read the [Action System Docs](https://www.jetbrains.org/intellij/sdk/docs/basics/action_system.html)

To create our custom action, we need to extend the abstract class [`AnAction`](https://upsource.jetbrains.com/idea-ce/file/idea-ce-ccdf7371b9620d4bde8be85eb8262a21447dade6/platform/editor-ui-api/src/com/intellij/openapi/actionSystem/AnAction.java).
Classes that extend it, should override `AnAction.update()` and `AnAction.actionPerformed()`.
* The `update()` method implements the code that enables or disables an action.
* The `actionPerformed()` method implements the code that executes when an action is invoked by the user.

Let's see the main class and then split into pieces and explain it individually:

```java
package codever.intellij.plugin;

import com.intellij.ide.BrowserUtil;
import com.intellij.lang.Language;
import com.intellij.openapi.actionSystem.AnAction;
import com.intellij.openapi.actionSystem.AnActionEvent;
import com.intellij.openapi.actionSystem.CommonDataKeys;
import com.intellij.openapi.actionSystem.PlatformDataKeys;
import com.intellij.openapi.editor.Editor;
import com.intellij.openapi.project.Project;
import com.intellij.openapi.vfs.VirtualFile;
import com.intellij.psi.PsiFile;

import java.io.UnsupportedEncodingException;
import java.net.URLEncoder;

public class SaveToCodeverAction extends AnAction {

    /**
     * Only make this action visible when text is selected.
     * <p>
     * The update method below is only called periodically so need
     * to be careful to check for selected text
     * https://jetbrains.org/intellij/sdk/docs/basics/action_system.html#overriding-the-anactionupdate-method
     *
     * @param e
     */
    @Override
    public void update(AnActionEvent e) {
        // Get required data keys
        final Project project = e.getProject();
        final Editor editor = e.getData(CommonDataKeys.EDITOR);

        // Set visibility only in case of existing project and editor and if a selection exists
        e.getPresentation().setEnabledAndVisible(project != null
                && editor != null
                && editor.getSelectionModel().hasSelection());
    }

    @Override
    public void actionPerformed(AnActionEvent e) {
        final String selectedCode = getSelectedCode(e);
        final String languageTag = getLanguageTag(e);
        final String title = getTitle(e);
        final String comment = getComment(e);
        final String sourceUrl = getSourceUrl(e);

        String url = getUrl(languageTag, sourceUrl, title, selectedCode, comment);
        if (url != null) {
            BrowserUtil.browse(url);
        }
    }

    private String getSelectedCode(AnActionEvent e) {
        final Editor editor = e.getRequiredData(CommonDataKeys.EDITOR);
        return editor.getSelectionModel().getSelectedText();
    }

    private String getLanguageTag(AnActionEvent e) {
        String languageTag = "";
        PsiFile file = e.getData(CommonDataKeys.PSI_FILE);
        if (file != null) {
            Language lang = e.getData(CommonDataKeys.PSI_FILE).getLanguage();
            languageTag = lang != null ? lang.getDisplayName().toLowerCase() : null;
        }
        return languageTag;
    }

    private String getTitle(AnActionEvent e) {
        return "Change me";
    }

    private String getComment(AnActionEvent e) {
        VirtualFile vFile = e.getData(PlatformDataKeys.VIRTUAL_FILE);
        String fileName = vFile != null ? vFile.getName() : null;

        final Project project = e.getData(PlatformDataKeys.PROJECT);
        String projectName = project != null ? project.getName() : null;
        StringBuilder sb = new StringBuilder();
        if (projectName != null) {
            sb.append("**Project**: `" + projectName + "`" );
        }
        if (fileName != null) {
            sb.append(" - **File**:  `" + fileName + "`" );
        }
        return sb.length() > 0 ? sb.toString() : null;
    }

    private String getSourceUrl(AnActionEvent e) {
        VirtualFile vFile = e.getData(PlatformDataKeys.VIRTUAL_FILE);
        return vFile != null ? vFile.getUrl() : null;
    }

    private String getUrl(String languageTag, String sourceUrl, String title, String selectedCode, String comment) {
        try {
            StringBuilder sb = new StringBuilder("https://www.codever.land/my-snippets/new?") ;
            sb.append("code=" + URLEncoder.encode(selectedCode, "UTF-8"));
            if (title != null) {
                sb.append("&title=" + URLEncoder.encode(title, "UTF-8"));
            }
            if (comment != null) {
                sb.append("&comment=" + URLEncoder.encode(comment, "UTF-8"));
            }
            if (sourceUrl != null) {
                sb.append("&sourceUrl=" + URLEncoder.encode(sourceUrl, "UTF-8"));
            }
            if (languageTag != null) {
                sb.append("&tags=" + URLEncoder.encode(languageTag, "UTF-8"));
            }

            return sb.toString();
        } catch (UnsupportedEncodingException unsupportedEncodingException) {
            unsupportedEncodingException.printStackTrace();
            return null;
        }
    }
}
```

> When an action is disabled `AnAction.actionPerformed()` will not be called.

> See [Working with Text](https://www.jetbrains.org/intellij/sdk/docs/tutorials/editor_basics/working_with_text.html) for
> more details about how to use actions to access caret placed in a document open in an editor.

<hr/>

#### The `actionPerformed()` method
This is the code executed when the user triggers the action:

```java
    @Override
    public void actionPerformed(AnActionEvent e) {
        final String selectedCode = getSelectedCode(e);
        final String languageTag = getLanguageTag(e);
        final String title = getTitle(e);
        final String comment = getComment(e);
        final String sourceUrl = getSourceUrl(e);

        String url = getUrl(languageTag, sourceUrl, title, selectedCode, comment);
        if (url != null) {
            BrowserUtil.browse(url);
        }
    }
```

This fills the required metadata for the snippet and passes it as query parameters (don't forget to encode the values)
 to [www.codever.land](https://www.codever.land) url via `BrowserUtil.browse(url)` method. This opens a new browser tab with the given `url`.

You will now go through the individual steps and see how you can achieve that with IntelliJ's SDK.

#### Get selected text in Editor

You can use the `SelectionModel` to access the selected code:
```java
    private String getSelectedCode(AnActionEvent e) {
        final Editor editor = e.getRequiredData(CommonDataKeys.EDITOR);
        return editor.getSelectionModel().getSelectedText();
    }
```

> For multi caret scenarios you should use the `CaretModel`, this does not apply in this context

#### Get the language tag of the current file in the IntelliJ project

When [saving snippets](https://www.codever.land/howto/snippets), you should define tags to easily identify them later.
In the IntelliJ plugin's case, I set the first tag of the snippet with the language of the file where you select the code from.
 You can access the metadata of files with the help of
 the [`PsiFile`](https://www.jetbrains.org/intellij/sdk/docs/basics/architectural_overview/psi_files.html)
and then its `Language` object:

```java
    private String getLanguageTag(AnActionEvent e) {
        String languageTag = "";
        PsiFile file = e.getData(CommonDataKeys.PSI_FILE);
        if (file != null) {
            Language lang = e.getData(CommonDataKeys.PSI_FILE).getLanguage();
            languageTag = lang != null ? lang.getDisplayName().toLowerCase() : null;
        }
        return languageTag;
    }
```

#### Get project and file name

The start of snippet's comment is automatically filled with the project's name plus the file where the code snippet
is selected from. You can access these attributes from a virtual file [`VirtualFile`](https://www.jetbrains.org/intellij/sdk/docs/basics/architectural_overview/virtual_file.html):

```java
    private String getTitle(AnActionEvent e) {
        VirtualFile vFile = e.getData(PlatformDataKeys.VIRTUAL_FILE);
        String fileName = vFile != null ? vFile.getName() : null;

        final Project project = e.getData(PlatformDataKeys.PROJECT);
        String projectName = project != null ? project.getName() : null;
        StringBuilder sb = new StringBuilder();
        if (projectName != null) {
            sb.append(projectName);
        }
        if (fileName != null) {
            sb.append(" - " + fileName);
        }
        return sb.length() > 0 ? sb.toString() : null;
    }
```

#### Get the file's path

Last but not least, I want to have the file's path to fill the `sourceUrl` attribute of the snippet. You can get it
by accessing the `getUrl()` method of the same `VirtualFile`:

```java
    private String getSourceUrl(AnActionEvent e) {
        VirtualFile vFile = e.getData(PlatformDataKeys.VIRTUAL_FILE);
        return vFile != null ? vFile.getUrl() : null;
    }
```

## Registering the action

There are two main ways to register an action: either by listing it in the `<actions>` section of a plugin's `plugin.xml` file, or through code.

In this plugin we use the configuration ``plugin.xml`` possibility:

```xml
    <actions>
        <action
                id="Codever.Save.Editor"
                class="codever.intellij.plugin.SaveToCodeverAction"
                text="Save to Codever"
                description="Save snippet to Codever"
                icon="SaveToCodeverPluginIcons.CODEVER_ACTION_ICON">
            <add-to-group group-id="EditorPopupMenu" anchor="last"/>
        </action>
        <action
                id="Codever.Search.Editor"
                class="codever.intellij.plugin.SearchOnCodeverDialog"
                text="Search on Codever"
                description="Launches dialog to input query search on Codever"
                icon="SaveToCodeverPluginIcons.CODEVER_ACTION_ICON">
            <add-to-group group-id="EditorPopupMenu" anchor="last"/>
        </action>
        <action
                id="Codever.Save.Console"
                class="codever.intellij.plugin.SaveToCodeverAction"
                text="Save to Codever"
                description="Save snippet to Codever"
                icon="SaveToCodeverPluginIcons.CODEVER_ACTION_ICON">
            <add-to-group group-id="ConsoleEditorPopupMenu" anchor="last"/>
        </action>
        <action
                id="Codever.Search.Console"
                class="codever.intellij.plugin.SearchOnCodeverDialog"
                text="Search on Codever"
                description="Launches dialog to input query search on Codever"
                icon="SaveToCodeverPluginIcons.CODEVER_ACTION_ICON">
            <add-to-group group-id="ConsoleEditorPopupMenu" anchor="last"/>
        </action>
    </actions>
```

> For the configuration via code possibility and further details check the [Register actions official docs](https://www.jetbrains.org/intellij/sdk/docs/basics/action_system.html#registering-actions)


## Plugin icon
This is supported beginning in version 2019.1. The plugin Logos are shown in [Plugins Repository](https://plugins.jetbrains.com/)
 and in [Marketplace](https://plugins.jetbrains.com/marketplace). They also appear in the Settings/Preferences Plugin Manager UI in IntelliJ Platform-based IDEs.
  Whether online or in the product UI, a Plugin Logo helps users to identify a plugin more quickly in a list.

### Plugin Logo Requirements
#### Naming convention
All the Plugin Logo images must be in SVG format and adhere to the following naming convention:
- `pluginIcon.svg`  is the default Plugin Logo. If a separate Logo file for dark UI Themes exists in the plugin, then this file is used solely for light UI Themes,
- `pluginIcon_dark.svg` is an optional, alternative Plugin Logo for use solely with dark IDE UI Themes.

#### Location
The `.svg` files must be placed in the `META-INF` folder of the plugin distribution file.

> For further details see the official documentation - [Plugin Logo / IntelliJ Platform SDK DevGuide](https://www.jetbrains.org/intellij/sdk/docs/basics/plugin_structure/plugin_icon_file.html)

## Action icon
Also to make it easier to visually recognize the desired action add an icon to it:
 ![Action icon in right click menu](/images/posts/2020-06-07-first-intellij-plugin/action-icon-in-right-click-menu.png)

The recommended way to organize icons is to put them to a dedicated `icons` source root marked as Resources Root, let's say `icons` under  `resources`.

Then define a class or an interface with icon constants in a top-level package called `icons`:
```java
package icons;

import com.intellij.openapi.util.IconLoader;

import javax.swing.*;

public interface SaveToBookmarksDevPluginIcons {
    Icon SAVE_TO_BOOKMARKS_DEV_ACTION = IconLoader.getIcon("/icons/icon.png");
}
```

> **NOTE** The path to the icon passed in as argument to `IconLoader.getIcon()` must start with leading `/`

Use these constants inside `plugin.xml`. Note that the package of `icons` will be automatically prefixed, and must not be added
manually:

```xml
    <actions>
        <action
                id="Codever.Save.Editor"
                class="dev.bookmarks.intellij.plugin.SaveToBookmarksDevAction"
                text="Save to Bookmarks.dev"
                description="Save code snippet to Bookmarks.dev"
                icon="SaveToBookmarksDevPluginIcons.SAVE_TO_BOOKMARKS_DEV_ACTION">
            <add-to-group group-id="EditorPopupMenu" anchor="last"/>
        </action>
        <action
                id="Codever.Save.Console"
                class="dev.bookmarks.intellij.plugin.SaveToBookmarksDevAction"
                text="Save to Bookmarks.dev"
                description="Save code snippet to Bookmarks.dev"
                icon="SaveToBookmarksDevPluginIcons.SAVE_TO_BOOKMARKS_DEV_ACTION">
            <add-to-group group-id="ConsoleEditorPopupMenu" anchor="last"/>
        </action>
    </actions>
```

## Testing the plugin
It's possible to run and debug a plugin directly from the *IntelliJ IDEA*. You need a configured special profile (a *Plugin* Run/Debug configuration)
 that specifies the plugin module, VM parameters and other specific options. When you run such profile, it launches the IDE with your plugin installed.

 ![Plugin run configuration](/images/posts/2020-06-07-first-intellij-plugin/plugin-run-configuration.png)

For information on how to change the Run/Debug configuration profile, refer to [Run/Debug Configuration](https://www.jetbrains.com/help/idea/run-debug-configuration.html)
 and [Run/Debug Configuration: Plugin](https://www.jetbrains.com/idea/help/run-debug-configuration-plugin.html) in *IntelliJ IDEA* Web Help.

## Deploying/installing the plugin

Right click the project in IntelliJ and select **"Prepare Plugin Module for Deployment"**. This will generate a JAR
file inside the project's directory, which you can install then in **Plugins > Install plugin from disk...**. You can
also use it to [publish the plugin to the JetBrains Plugins Repository](https://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/publishing_plugin.html).


## Conclusion
You've learned in this how to create a basic IntelliJ platform plugin. For more advanced functions the best place to
start is the [docs of IntelliJ Platform SDK](https://www.jetbrains.org/intellij/sdk/docs/intro/welcome.html).

Although I primarily use IntelliJ for development (thank you JetBrains for supporting me with an open source license),
I switch to Visual Studio Code from time to time. So, my next thing is to write a plugin to save codelets for that - stay tuned!

