# react-native-swipe-list-view

```<SwipeListView>``` is a ListView with rows that swipe open and closed. Handles default native behavior such as closing rows when ListView is scrolled or when other rows are opened.

Also includes ```<SwipeRow>``` if you want to use a swipeable row outside of the ```<SwipeListView>```

# Example
![](http://i.imgur.com/6fTrdZa.gif) &nbsp;&nbsp;&nbsp;&nbsp; ![](http://i.imgur.com/3IdOA77.gif)

# Installation
```bash
npm install --save react-native-swipe-list-view
```

# Usage
```javascript
import { SwipeListView } from 'react-native-swipe-list-view';

render() {
	const ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
	return (
		<SwipeListView
			dataSource={ds.cloneWithRows(dataSource)}
			renderRow={ data => (
				<View style={styles.rowFront}>
					<Text>I am {data} in a SwipeListView</Text>
				</View>
			)}
			renderHiddenRow={ data => (
				<View style={styles.rowBack}>
					<Text>Left</Text>
					<Text>Right</Text>
				</View>
			)}
			leftOpenValue={75}
			rightOpenValue={-75}
		/>
	)
}
```

*See ```example.js``` for full usage guide (including using <SwipeRow> by itself)*

#### Note:
If your row is touchable (TouchableOpacity, TouchableHighlight, etc.)  with an ```onPress``` function make sure ```renderRow``` returns the Touchable as the topmost element.

GOOD:
```javascript
renderRow={ data => (
	<TouchableHighlight onPress={this.doSomething.bind(this)}>
	    <View>
	        <Text>I am {data} in a SwipeListView</Text>
	    </View>
	</TouchableHighlight>
)}
```
BAD:
```javascript
renderRow={ data => (
	<View>
	    <TouchableHighlight onPress={this.doSomething.bind(this)}>
	        <Text>I am {data} in a SwipeListView</Text>
	    </TouchableHighlight>
	</View>
)}
```

#### Manually closing rows:

If your row or hidden row renders a touchable child and you'd like that touchable to close the row note that the ```renderRow``` and ```renderHiddenRow``` functions are passed ```rowData```, ```secId```, ```rowId```, ```rowMap```. The ```rowMap``` is an object that looks like:
```
{
    rowId_1: ref_to_row_1,
    rowId_2: ref_to_row_2
}
```

Each row's ref has a public method called ```closeRow``` that will swipe the row closed. So you can do something like:
```
<SwipeList
    renderHiddenRow={ (data, secdId, rowId, rowMap) => {
        <TouchableOpacity onPress={ _ => rowMap[rowId].closeRow() }>
            <Text>I close the row</Text>
        </TouchableOpacity>
    }}
/>
```

If you are using the standalone ```<SwipeRow>``` you can just keep a ref to the component and call ```closeRow()``` on that ref.

## API

`SwipeListView` (component)
===========================

ListView that renders SwipeRows.

Props
-----

### `closeOnRowPress`

Should open rows be closed when a row is pressed

type: `bool`
defaultValue: `true`


### `closeOnScroll`

Should open rows be closed when the listView begins scrolling

type: `bool`
defaultValue: `true`


### `leftOpenValue`

TranslateX value for opening the row to the left (positive number)

type: `number`
defaultValue: `0`


### `renderHiddenRow` (required)

How to render a hidden row (renders behind the row). Should return a valid React Element.

type: `func`


### `renderRow` (required)

How to render a row. Should return a valid React Element.

type: `func`


### `rightOpenValue`

TranslateX value for opening the row to the right (negative number)

type: `number`
defaultValue: `0`


### `disableLeftSwipe`

Disable ability to swipe the row left

type: `bool`
defaultValue: `false`


### `disableRightSwipe`

Disable ability to swipe the row right

type: `bool`
defaultValue: `false`


`SwipeRow` (component)
======================

Row that is generally used in a SwipeListView.
If you are rendering a SwipeRow explicitly you must pass the SwipeRow exactly two children.
The first will be rendered behind the second.
e.g.
```javascript
  <SwipeRow>
      <View style={hiddenRowStyle} />
      <View style={visibleRowStyle} />
  </SwipeRow>
```
Props
-----

### `closeOnRowPress`

Should the row be closed when it is tapped

type: `bool`
defaultValue: `true`


### `friction`

Friction for the open / close animation

type: `number`


### `leftOpenValue`

TranslateX value for opening the row to the left (positive number)

type: `number`
defaultValue: `0`


### `onRowOpen`

Called when a swipe row is animating open. Used by the SwipeListView
to keep references to open rows.

type: `func`


### `rightOpenValue`

TranslateX value for opening the row to the right (negative number)

type: `number`
defaultValue: `0`


### `setScrollEnabled`

Used by the SwipeListView to close rows on scroll events.
You shouldn't need to use this prop explicitly.

type: `func`


### `tension`

Tension for the open / close animation

type: `number`


### `disableLeftSwipe`

Disable ability to swipe the row left

type: `bool`
defaultValue: `false`


### `disableRightSwipe`

Disable ability to swipe the row right

type: `bool`
defaultValue: `false`

# License

MIT