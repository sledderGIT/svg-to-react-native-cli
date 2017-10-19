# svg-to-react-native-cli
Based on svg-to-react-cli.
A command line utility that takes a svg image file and outputs a fully formatted stateless functional React Native component with `height` and `width` for props. With flags to toggle formatting and remove style attributes.

## To Use
`npm install -g svg-to-react-naive-cli`

### One File

`svg-to-react-native <svgImage>(remove the .svg) <ComponentName>`

**NOTE**: image file must be in current working directory. Do not add the extension. If file is `image.svg`, then just enter `image` as the first argument. ComponentName will be the name of the sfc and filename with `.js` appended.

### Multi File

`svg-to-react-native dir`

or for all files in directory (will name all components in CamelCase based on image name. If image is `image.svg` then new component will be `Image` and file will be `Image.js`):


## Flags

Or use flags: `svg-to-react-native <svgImage> <ComponentName> --output ./components/svgComponents/ --no-format --rm-style`

**Optional Flags:**

`-o, --output <path>` - the output path. Do not include the filename.

`--no-format` - will skip line breaks and indentation to svg. If your svg is already formatted, use this flag.

`--rm-style` - removes all style attributes from svg tags.

`--help` - Prints out this readme.

`--example` - Prints an example of the i/o of this util.

## Example

Takes this:
```html
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" style="height: 512px; width: 512px;"><defs><filter id="glow"><feGaussianBlur stdDeviation="7" result="coloredBlur"></feGaussianBlur><feMerge><feMergeNode in="coloredBlur"></feMergeNode><feMergeNode in="SourceGraphic"></feMergeNode></feMerge></filter></defs><circle cx="256" cy="256" r="256" fill="#f5a623" opacity="1" stroke="#fff" stroke-width="0"></circle><path fill="#000000" opacity="1" d="M363.783 ..." transform="translate(25.6, 25.6) scale(0.9, 0.9) rotate(0, 256, 256)" clip-path="false" filter="url(#glow)"></path><g font-family="Arial, Helvetica, sans-serif" font-size="120" font-style="normal" font-weight="bold" text-anchor="middle" class="" transform="translate(256,300)" style="touch-action: none;"><text stroke="#000" stroke-width="30" opacity="1"></text><text fill="#fff" opacity="1"></text></g></svg>
```
And creates a new file with this:

```javascript
import React from 'react';
import {
  Svg,
  Circle,
  Ellipse,
  G,
  LinearGradient,
  RadialGradient,
  Line,
  Path,
  Polygon,
  Polyline,
  Rect,
  Symbol,
  Text,
  Use,
  Defs,
  Stop
} from 'react-native-svg';

export default function TestSvg(props) {
  return (
    <Svg heiGht="512" width="512" viewBox="0 0 512 512">
      <Defs />
      <Circle
        cx="256"
        cy="256"
        fill="#f5a623"
        opacity="1"
        r="256"
        stroke="#fff"
        strokeWidth="0"
      />
      <Path
        ClipPath="false"
        d="M363.783 ..."
        fill="#000000"
        opacity="1"
        transform="translate(25.6, 25.6) scale(0.9, 0.9) rotate(0, 256, 256)"
      />
      <G
        fontFamily="Arial, Helvetica, sans-serif"
        fontSize="120"
        TextAnchor="middle"
        transform="translate(256,300)"
      >
        <Text opacity="1" stroke="#000" strokeWidth="30" />
        <Text fill="#fff" opacity="1" />
      </G>
    </Svg>
  );
}


  );
}
```

## Width / height order of precedence
The order of precedence of how width/height is set on a generated component is as follows:

 - 1 - Passed in width and height props are always priority one. This gives run time control to the container.
 - 2 - If a svg has its width/height set, this will be the first fallback if width/height props are not provided.
 - 3 - If the svg has not provided width/height attributes, the svg's viewbox's width/height segments are used as the fallback if width/height props are not provided.
 - 4 - Finally, if the svg has no width/height attributes, and the svg also doesn't have a viewbox - a fallback of 50px is applied to the width and height
