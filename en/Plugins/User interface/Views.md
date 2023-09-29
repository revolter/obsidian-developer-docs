Views determine how Obsidian displays content. The file explorer, graph view, and the Markdown view are all examples of views, but you can also create your own custom views that display content in a way that makes sense for your plugin.

To create a custom view, create a class that extends the [[ItemView|ItemView]] interface:

```ts
import { ItemView, WorkspaceLeaf } from "obsidian";

export const VIEW_TYPE_EXAMPLE = "example-view";

export class ExampleView extends ItemView {
  constructor(leaf: WorkspaceLeaf) {
    super(leaf);
  }

  getViewType() {
    return VIEW_TYPE_EXAMPLE;
  }

  getDisplayText() {
    return "Example view";
  }

  async onOpen() {
    const container = this.containerEl.children[1];
    container.empty();
    container.createEl("h4", { text: "Example view" });
  }

  async onClose() {
    // Nothing to clean up.
  }
}
```

> [!note]
> For more information on how to use the `createEl()` method, refer to [[HTML elements]].

Each view is uniquely identified by a text string and several operations require that you specify the view you'd like to use. Extracting it to a constant, `VIEW_TYPE_EXAMPLE`, is a good idea—as you will see later in this guide.

- `getViewType()` returns a unique identifier for the view.
- `getDisplayText()` returns a human-friendly name for the view.
- `onOpen()` is called when the view is opened within a new leaf and is responsible for building the content of your view.
- `onClose()` is called when the view should close and is responsible for cleaning up any resources used by the view.

Custom views need to be registered when the plugin is enabled, and cleaned up when the plugin is disabled:

```ts
import { Plugin } from "obsidian";
import { ExampleView, VIEW_TYPE_EXAMPLE } from "./view";

export default class ExamplePlugin extends Plugin {
  async onload() {
    this.registerView(
      VIEW_TYPE_EXAMPLE,
      (leaf) => new ExampleView(leaf)
    );

    this.addRibbonIcon("dice", "Activate view", () => {
      this.activateView();
    });
  }


  async activateView() {  
    const leaves = this.app.workspace.getLeavesOfType(VIEW_TYPE_EXAMPLE);  
    if(leaves.length === 0) {  
       await this.app.workspace.getRightLeaf(false).setViewState({  
          type: VIEW_TYPE_EXAMPLE,  
          active: true,  
       });  
    }  
  
    this.app.workspace.revealLeaf(  
       this.app.workspace.getLeavesOfType(VIEW_TYPE_EXAMPLE)[0]  
    );  
}
}
```

The second argument to [[registerView|registerView()]] is a factory function that returns an instance of the view you want to register.

> [!warning]
> Never manage references to views in your plugin. Obsidian may call the view factory function multiple times. Avoid side effects in your view, and use `getLeavesOfType()` whenever you need to access your view instances.
>
> ```ts
> this.app.workspace.getLeavesOfType(VIEW_TYPE_EXAMPLE).forEach((leaf) => {
>   if (leaf.view instanceof ExampleView) {
>     // Access your view instance.
>   }
> });
> ```


After you've registered a custom view for the plugin, you should give the user a way to activate it. The `activateView()` function is a convenient method that does three things:

- 
- Adds the custom view on the right leaf.
- Reveals the leaf that contains the custom view.

> [!tip]
> The condition in `activateView()` restricts your plugin to at most one leaf at a time. Try to change the condition to allow the user to create more than one leaf.

How you want the user to activate the custom view is up to you. The example uses a [[Ribbon actions|ribbon action]], but you can also use a [[Commands|command]].
