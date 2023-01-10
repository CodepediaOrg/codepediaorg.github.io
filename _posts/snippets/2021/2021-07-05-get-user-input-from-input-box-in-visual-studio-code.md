---
layout: post
title: Get user input from input box in visual studio code
description: "Get user input from input box in visual studio code code snippet"
author: ama
permalink: /snippets/60dbfb494095c204661309bf/get-user-input-from-input-box-in-visual-studio-code
published: true
snippetId: 60dbfb494095c204661309bf
categories: [snippets]
tags: [typescript, visual-studio-code, vscode-extensions, codever-snippets]
---

Use `vscode.window.showInputBox` - the returned value will be `undefined` if the input box was canceled (e.g. pressing **ESC**). Otherwise the returned value will be the string typed by the user or an empty string if the user did not type anything but dismissed the input box with OK.

```typescript
const searchQuery = await vscode.window.showInputBox({
  placeHolder: "Search query",
  prompt: "Search my snippets on Codever",
  value: selectedText
});
if(searchQuery === ''){
  console.log(searchQuery);
  vscode.window.showErrorMessage('A search query is mandatory to execute this action');
}

if(searchQuery !== undefined){
  const searchUrl = `https://www.codever.dev/search?q=${searchQuery}&sd=my-snippets`;
  vscode.env.openExternal(Uri.parse(searchUrl));
}
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://code.visualstudio.com/api/references/vscode-api#window.showInputBox" target="_blank" style="font-weight: lighter">
     https://code.visualstudio.com/api/references/vscode-api#window.showInputBox
  </a>
</span>

You can see it in action in the following demo:

{% include image.html url="https://raw.githubusercontent.com/codeverland/codever-vscode/main/resources/gif/codever-vscode-extension-search-snippet-800x457.gif" description="Search from Codever Snippets VSCode Extension Demo" %}

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="60dbfb494095c204661309bf" %}
