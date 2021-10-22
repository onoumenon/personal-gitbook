# React Native Intro

{% embed url="https://app.pluralsight.com/library/courses/react-native-fundamentals/table-of-contents" %}

### Setup

Install node and npm and other stuff as per [https://reactnative.dev/docs/environment-setup](https://reactnative.dev/docs/environment-setup)

`npm i -g react-native-cli`

`react-native init ProjectName`

OR

`create-react-native-app ProjectName`

Or use Ignite CLI for speed

## Development

React Native has built-in support for ES6

### debugging

`npm i -g react-devtools`

## Basics

Similar to React but has its own library of components

{% embed url="https://reactnative.dev/docs/intro-react-native-components" %}

### Handling text input:

```
import React, { useState } from 'react';
import { Text, TextInput, View } from 'react-native';

const PizzaTranslator = () => {
  const [text, setText] = useState('');
  return (
    <View style={{padding: 10}}>
      <TextInput
        style={{height: 40}}
        placeholder="Type here to translate!"
        onChangeText={text => setText(text)}
        defaultValue={text}
      />
      <Text style={{padding: 10, fontSize: 42}}>
        {text.split(' ').map((word) => word && '🍕').join(' ')}
      </Text>
    </View>
  );
}

export default PizzaTranslator;

```

### Lists:

The `FlatList` component displays a scrolling list of changing, but similarly structured, data. `FlatList` works well for long lists of data, where the number of items might change over time. Unlike the more generic [`ScrollView`](https://reactnative.dev/docs/using-a-scrollview), the `FlatList` **only renders elements that are currently showing on the screen, not all the elements at once.**\
****

If you want to render a set of **data broken into logical sections, maybe with section headers, similar to `UITableView`s on iOS, then a **[**SectionList**](https://reactnative.dev/docs/sectionlist)** is the way to go.**\


### Platform-specific code

```
import { Platform, StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  height: Platform.OS === 'ios' ? 200 : 100
});
```

`Platform.select` method

```
import { Platform, StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  container: {
    flex: 1,
    ...Platform.select({
      ios: {
        backgroundColor: 'red'
      },
      android: {
        backgroundColor: 'green'
      },
      default: {
        // other platforms, web for example
        backgroundColor: 'blue'
      }
    })
  }
});
```

```
const Component = Platform.select({
  ios: () => require('ComponentIOS'),
  android: () => require('ComponentAndroid')
})();

<Component />;
```

## Styles

```
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';

const LotsOfStyles = () => {
    return (
      <View style={styles.container}>
        <Text style={styles.red}>just red</Text>
        <Text style={styles.bigBlue}>just bigBlue</Text>
        <Text style={[styles.bigBlue, styles.red]}>bigBlue, then red</Text>
        <Text style={[styles.red, styles.bigBlue]}>red, then bigBlue</Text>
      </View>
    );
};

const styles = StyleSheet.create({
  container: {
    marginTop: 50,
  },
  bigBlue: {
    color: 'blue',
    fontWeight: 'bold',
    fontSize: 30,
  },
  red: {
    color: 'red',
  },
});

export default LotsOfStyles;

```

## Troubleshooting

{% embed url="https://reactnative.dev/docs/troubleshooting" %}

