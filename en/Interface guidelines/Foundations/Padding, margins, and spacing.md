Obsidian uses a 4 pixel grid. The 4 pixel grid is chosen because it enables the interface to scale up and down across high/low DPI screens, and also provides convenient ratios for layouts.

UI elements use multiples of 4 for padding and spacing throughout the app. We recommend using the `--size` CSS variables to define all dimensions throughout your plugin.

Each size variable contains two numbers which represent the base and the multiple, for example:

-   `--size-4-1` represents `4*1 = 4px`
-   `--size-4-2` represents `4*2 = 8px`
-   `--size-4-4` represents `4*4 = 16px`

A few `--size-2-x` variables are available for small spaces where fractions of 4 can be useful, e.g. `--size-2-1` represents `2*1 = 2px`