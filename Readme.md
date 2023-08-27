# React Native Styling: Structure for Style Organization

## 1: Styles are important: make them easy to find
Keep styles in the root source folder.

Styling is a first class concern, and as such, styles should be accessible from a top-level folder in the application code.

MyReactNativeApp
  - src
    - assets
    - compontents
      - MyComponent.js
    - styles
      - colors.js
      - index.js
      - typography.js
      - ...
  ...

## 2. Get atomic!
Build complicated styles from simpler styles.

By using object destructuring in style declaration, we get really concise and readable styles which allows us to be declarative in our components.

    // buttons.js

    export const small = {
      paddingHorizontal: 10,
      paddingVertical: 12,
      width: 75
    };

    export const rounded = {
      borderRadius: 50
    };

    export const smallRounded = {
      ...base,
      ...small,
      ...rounded
    };
    // src/MyComponent/index.js

    const styles = StyleSheet.create({
      button: {
        ...Buttons.smallRounded,
      },
    })

is easier to understand the intent of and maintain than:

    // src/MyComponent/index.js

    const styles = StyleSheet.create({
      button: {
        paddingHorizontal: 10,
        paddingVertical: 12,
        width: 75,
        borderRadius: 50
      },
    })

# 3: Styles are important: make them easy to use
Group similar variables in modules and bundle them up in an index.js file.

Style variables are easier to find and understand when they’re organized by function. Therefore, they should live in purposeful files.

If we place an index.js in this folder we can take advantage of JavaScript ES6 import syntax for importing all the styles at once.

    - styles
      - colors.js
      - index.js
      - spacing.js
      - typography.js
      - buttons.js

---

    // src/styles/index.js

    import * as Buttons from './buttons'
    import * as Colors from './colors'
    import * as Spacing from './spacing'
    import * as Typography from './typography'

    export { Typography, Spacing, Colors, Buttons }

This allows us to:

import only what we need
import from the same file every time
give the variables descriptive and short names, contained in a descriptive object.
easily extend and modify the common styles
write more concise and expressive code.

    // src/MyComponent/index.js

    import { Typography, Colors, Spacing } from '../styles'

    ...

    const styles = StyleSheet.create({
      container: {
        backgroundColor: Colors.background,
        alignItems: 'center',
        padding: Spacing.base,
      },
      header: {
        flex: 1,
        ...Typography.mainHeader,
      },
      section: {
        flex: 3,
        ...Typography.section,
      }
    })

## 4. Keep styles close
Keep StyleSheets inline with components

Defining StyleSheets in the same file as your component can help make sure that:

styles for one component won’t be overwritten for another component in a future iteration
styles will be maintained as the component evolves
components can be flexible during design iteration
there is a minimum amount of mental overhead while implementing designs for components since there is one place to look for styles rather than many.
One reason you might want to not to use in-line stylesheets is to reduce the amount of duplication in the code, but now that you’re creating variables for global styles in functionally-named files, you can still be mindful of practicing D.R.Y. (Don’t Repeat Yourself) coding, without reusing stylesheets themselves.

    // src/MyNewComponent/index.js

    import { Typography } from '../styles'

    const MyNewComponent = () => (
      <View style={styles.container}>
        <View style={styles.header}>
          <MyComponent />
        </View>
        <View style={styles.body}>
          <MyOtherComponent />
        <View>
      </View>
    )

    const styles = StyleSheet.create({
      container: {
        flex: 1,
      },
      header: {
        ...Typography.header
      },
      body: {
        ...Typography.body
      },
    })

is more self contained than:

    // src/MyNewComponent/index.js

    import { styleSheetA, styleSheetB, styleSheetC } from './stylesheets'

    const MyNewComponent = () => (
      <View style={styleSheetC.container}>
        <View style={styleSheetA.header}>
          <MyComponent />
        </View>
        <View style={styleSheetB.body}>
          <MyOtherComponent />
        <View>
      </View>
    )

## Conclusion
Styles are important: make them easy to find: Keep styles in the root application folder
Get atomic!: Build complicated Styles from simpler Styles
Styles are important: make them easy to use: Bundle like styles and expose via an index.js file
Keep Styles close: Keep styles inline with components
All together, an application of styles to a component could look like this:

    // src/MyOtherComponent/index.js

    import React from 'react'
    import { StyleSheet, View } from 'react-native'

    import MyComponent from './MyComponent'

    import { Colors } from './styles'

    const MyOtherComponent = () => (
      <View style={styles.container}>
        <MyComponent />
      </View>
    )

    const styles = StyleSheet.create({
      container: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: Colors.background,
      },
    })

    export default MyOtherComponent

---

Reference and all credits to: https://thoughtbot.com/blog/structure-for-styling-in-react-native
