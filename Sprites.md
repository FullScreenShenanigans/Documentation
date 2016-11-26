# Sprites

This is a guide for how sprite data is stored, parsed, and drawn to the visual canvas in GameStartr projects.
It's built on the [PixelDrawr](https://github.com/FullScreenShenanigans/PixelDrawr) and [PixelRendr](https://github.com/FullScreenShenanigans/PixelRendr) modules.

## Storage

A GameStartr instance's PixelRendr keeps sprite data stored as a library using a [StringFilr](https://github.com/FullScreenShenanigans/StringFilr).
Each sprite is stored as a series of numbers that represents the ordered pixels in a rectangle.

```javascript
"00000001112"
```

A mapping of which numbers represent which color are stored in a global "palette".
    
```javascript
[
    [0, 0, 0, 0],         // transparent
    [255, 255, 255, 255], // white
    [0, 0, 0, 255],       // black
    // ... and so on
]
```
    
Using the above palette, the sprite represents seven transparent pixels, three white pixels, and a black pixel.

Most images are much larger and more complex so a few compression techniques are applied.

1. **[Indexed Color](https://en.wikipedia.org/wiki/Indexed_color)**

    It is necessary to have a consistent number of digits in images, as `010` could be `[0, 1, 0]`, `[0, 10]`, or etc.
    Palettes with more than ten colors therefore have to prefix single-digit numbers with `0`.
    For example, `[1, 14, 1]` would use `["01", "14", "01"]`:

    ```javascript
    "011401011401011401011401011401011401011401"
    ```

    We can avoid this wasted character space by instructing a sprite to only use a subset of the pre-defined palette:

    ```javascript
    "p[1,14]010010010010010010010"
    ```

    The `p[0,14]` tells the renderer that this sprite only uses colors 0 and 14.
    The number 0 then refers to palette number 1 and the number 1 should refer to palette number 14.

2. **[Run-length Encoding](https://en.wikipedia.org/wiki/Run-length_encoding)**

    Take the following wasteful sprite:

    ```javascript
    "p[0]0000000000000000000000000000000000000000000000000"
    ```

    We know the 0 should be repeated 35 times, so the following notation is used to indicate "repeat ('x') 0 35 times (','))":

    ```javascript
    "p[0]x035,"
    ```

3. **Filters**

    Many sprites are different versions of other sprites, often simply identical or miscolored (the only two commands supported so far).
    A library may declare a filter:

    ```javascript
    "Sample": [ "palette", { "00": "03" } ]
    ```

    ...along with its sprites:

    ```javascript
    "foo": "p[0,7,14]000111222000111222000111222",
    "bar": [ "filter", ["foo"], "Sample"]
    ```

    The `"bar"` sprite will be a filtered version of foo, using the `"Sample"` filter.
    The `"Sample"` filter instructs the sprite to replace all instances of `"00"` with `"03"`, making `"bar"` equivalent to:
 
    ```javascript
    "bar": "p[3,7,14]000111222000111222000111222"
    ```
 
    Another instruction you may use is `"same"`, which is equivalent to directly copying a sprite with no changes:

    ```javascript
    "baz": [ "same", ["bar"] ]
    ```

4. **"Multiple" sprites**

    Sprites are oftentimes of variable height.
    Pipes in Mario, for example, have a top opening and a shaft of potentially infinite height.
    Rather than use two objects to represent the two parts, sprites may be directed to have one sub-sprite for the top/bottom or left/right, with a single sub-sprite filling in the middle.
    Pipes, then, would use a top and middle.

    ```javascript
    [ "multiple", "vertical", {
        "top": "{upper image data}",
        "bottom": "{repeated image data}"
    } ]
    ```
