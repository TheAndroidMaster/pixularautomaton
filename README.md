This "Pixular Automaton", as it has been called, is a basic [Cellular Automaton](https://en.wikipedia.org/wiki/Cellular_automaton) based on [Conway's Game of Life](http://mathworld.wolfram.com/GameofLife.html). There is no specific purpose for building this website other than to generate a pretty animation from input images, but its inner workings are reasonably complex, so allow me to explain them a little bit.

As mentioned previously, this automaton is based on Conway's Game of Life, a binary automaton with three main rules:

- Overpopulation: if a cell is surrounded by more than three active cells, it dies.
- Underpopulation: if a cell is surrounded by less than two active cells, it dies.
- Conception: if a cell is surrounded by exactly three active cells, it becomes active.

However, these cells only have an "on" and "off" state; in order for them to be applied to an image, they need to match specific colors. So, to prevent things from becoming too complex, I decided to split the grid of cells into "layers" by color; each color in the image is its own cellular automaton which overwrites others if they take up the same space. The program iterates through each color "layer" and evaluates every cell of that color before proceeding to the next layer. When evaluating a cell, another cell of the same color is considered "active", and any other color (or null) is considered to be "dead".

This means that there are in fact two ways for a cell to go about dying. It can either become "truly" inactive (not matching with any color), or it can become another color so that it no longer matches with the color that it was before and is thus considered "dead" by that layer. Because of this, there is a dropdown at the bottom of the page to switch between different "death modes". These alter the rules as follows:

- Die Empty: if a cell dies, it will always become "truly" inactive, or null.
- Die Mixed: if a cell dies of overpopulation, it becomes null, otherwise it becomes another color.
- Die Full: if a cell dies, it always becomes another color.

Of course, if any unprocessed image file is input into this program, not only will there be a ridiculous amount of layers, there will likely not be more than one or two active cells on each layer, which will not lead to a very entertaining result. In order to prevent this, when an image is selected, this website first picks a certain number of colors (configured by the "Granularity" setting) using a simple Traveling Salesman-based heuristic, then categorizes every pixel in the image as one of these colors, forming the layers to be used by the automaton. As this algorithm is O(n^2), it takes a while to compute for large files, which is why there is a "Pixelation" setting to sample a smaller amount of pixels from the original image.
