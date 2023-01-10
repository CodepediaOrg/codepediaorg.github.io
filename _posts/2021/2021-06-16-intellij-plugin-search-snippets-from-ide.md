---
layout: post
title: How to use the input dialog in an IntelliJ plugin
description: "Shows how to use the IntelliJ input in a plugin to get query text to search snippets on Codever."
author: ama
permalink: /ama/how-to-use-input-dialog-in-intellij-plugin
published: true
categories: [tutorial]
tags: [intellij, productivity, java, codever, plugins]
---

## Use case
I thought it would be practical to be able to search for search snippets directly from IntelliJ, without having to go to
 the browser, something like in the following demo:

{% include image.html url="https://raw.githubusercontent.com/codeverland/codever-intellij-plugin/master/documentation/img/plugin-showcase-search-800x456.gif" description="IntelliJ Plugin - Search snippet from IDE demo" %}

The implementation is fairly easy, so let's see how it's done.

<!--more-->

## Implementation

All the action happens in the `SearchonCodeverDialog` class which extends `AnAction`[^1] and overrides its `actionPerformed`
method:

```java
public class SearchOnCodeverDialog extends AnAction {

    @Override
    public void actionPerformed(@NotNull AnActionEvent event) {
        Project project = event.getData(PlatformDataKeys.PROJECT);

        final Editor editor = event.getRequiredData(CommonDataKeys.EDITOR);
        final String selectedText = editor.getSelectionModel().getSelectedText();

        String queryTxt;
        if (selectedText != null) {
            queryTxt = Messages.showInputDialog("Input query to search in My Snippets", "Codever Search", Messages.getQuestionIcon(), selectedText, null);
        } else {
            queryTxt = Messages.showInputDialog(project, "Input query to search in My Snippets", "Codever Search", Messages.getQuestionIcon());
        }
        String url = "https://www.codever.dev/search?sd=my-snippets&q=" + queryTxt;
        if (queryTxt != null) {
            BrowserUtil.browse(url);
        }

    }
}
```

[^1]: <https://plugins.jetbrains.com/docs/intellij/basic-action-system.html>

The user is asked for input via the method `Messages.showInputDialog` where any `selectedText`, if existing, is passed.
The result, `queryTxt`, contains the user input.

Of course, you need to define this action in `plugin.xml` file, which I did both for the Editor Menu, and the Console Menu:

```xml
    <actions>
        .....
        <action
                id="Codever.Search.Editor"
                class="codever.intellij.plugin.SearchOnCodeverDialog"
                text="Search on Codever"
                description="Launches dialog to input query search on Codever"
                icon="SaveToCodeverPluginIcons.CODEVER_ACTION_ICON">
            <add-to-group group-id="EditorPopupMenu" anchor="last"/>
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

So simple and straight forward. If you want to learn also how to get the selected text, get file info and debug an IntelliJ
see my previous post related to IntelliJ Plugins - [My first IntelliJ plugin to help me save code snippets to Codever]({% post_url 2020-06-07-first-intellij-plugin-to-save-snippets-to-codever %})

> The plugin is available to download and install in
> [JetBrain Plugins Repository](https://plugins.jetbrains.com/plugin/14456-codever-snippets/)
> and the source code is available on [Github](https://github.com/CodeverDotDev/codever-intellij-plugin)
