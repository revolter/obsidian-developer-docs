---
alias: "obsidian.FileManager.renameFile.md"
cssClass: hide-title
---

<!-- Do not edit this file. It is automatically generated by API Documenter. -->

[`FileManager`](obsidian.FileManager.md) › [`renameFile`](obsidian.FileManager.renameFile.md)

## FileManager.renameFile() method

Rename or move a file safely, and update all links to it depending on the user's preferences.

**Signature:**

```typescript
renameFile(file: TAbstractFile, newPath: string): Promise<void>;
```

## Parameters

|  Parameter | Type | Description |
|  --- | --- | --- |
|  <code>file</code> | [`TAbstractFile`](obsidian.TAbstractFile.md) | the file to rename |
|  <code>newPath</code> | <code>string</code> | the new path for the file |

**Returns:**

`Promise``<void>`
