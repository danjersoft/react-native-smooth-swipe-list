# react-native-smooth-swipe-list

#### A better swipe-able ListView

## Example
gifs coming soon...

#### Running example
```bash
git clone git@github.com:ProvataHealth/react-native-smooth-swipe-list.git
cd react-native-smooth-swipe-list
cd Example
npm install
react-native run-ios #or react-native run-android
```

## Installation
*note: you must be logged into an npmuser with proper permissions to access @provata*
```bash
npm install --save @provata/react-native-smooth-swipe-list
```

## Usage
A `SwipeList` builds a `ListView.DataSource` from its `props.rowData`. The DataSource is primarily the views provided by `rowData` wrapped by a `SwipeRow`
```javascript
...
import SwipeList from '@provata/react-native-smooth-swipe-list';

const ListParent = React.createClass({
    
    propTypes: {
        // takes in array of todo objects
        ...
    },
    
    componentDidMount() {
        // it's a good idea to store the derived rowData to prevent 
        // unnecessary re-renders of the rows in the ListView 
        this.rowData = this.props.todos.map(this.constructRowData);
    },
    
    componentWillReceiveProps(nextProps) {
        // however if you store the derived data you will need to handle the 
        // logic for whether a rowData element needs to be replaced
        ...
    },
    
    constructRowData(todo) {
        return {
            key: todo.id,
            rowView: this.getRowView(todo),
            leftSubView: this.getMarkCompleteButton(), //optional
            rightSubView: this.getArchiveButton(), //optional
            style: styles.row //optional but recommended to style your rows
        };
    },
        
    getRowView() {
        // return the view that will be the face of the row
        ...
    },
    
    getMarkCompleteButton() {
        // return your touchable view, it can be whatever 
        ...
    },
    
    getArchiveButton() {
        ...
    }
    
    render() {
        return <SwipeList rowData={this.rowData} />;
    }
});
```

##API

###SwipeList Component

####Props
* `rowData` - Object with the follow properties:
  * `key` - Key to assign to the row *(default: rowData index)*
  * `rowView`(required) - View to use as the row face
  * `leftSubView`- View to show on left when swiping to the right
  * `rightSubVIew` - View to show on right when swiping to the left
  * `rowProps` - Props to be assigned to the wrapping `SwipeRow` *(see `SwipeRow` props)*
  * `style` - Style to apply to the row parent
* `scrollEnabled` Whether to allow scrolling the ListVIew *(default: true)*
* `style` - Style applied to the ListView

###Methods
* `closeRow()` - Close any open row

To be implemented:
* *`openRow(rowKey)` - Opens row*
* *`scrollToRow(rowKey, skipAnimation)` - Scrolls to row. *Optionally skip animating*
* *`calloutRow(rowKey)` - Performs a shake animation on row*

###SwipeRow Component
See [React Native PanResponder](https://facebook.github.io/react-native/docs/panresponder.html) for information about gesture events.

####Props
* `swipeEnabled` - Where the row should respond to gestures
* `onSwipeStart` - Called when a gesture starts
* `onSwipeUpdate` - Called each update of the gesture after start and before end 
* `onSwipeEnd` - Called when the gesture ends
* `onOpen` - Called when the row opens
* `onClose` - Called when the row closes
* `onCapture` - Called when a gesture capture happens
* `leftSubView` - View to be rendered for right gestures
* `rightSubVIew` - View to be rendered for left gestures
* `startOpen` - Whether the row should start open
* `blockChildEventsWhenOpen` - If true will capture gesture events before they reach the rowView *(default: true)*

###Methods
* `close(skipAnimation)` - Close row. *Optionally skip animating*
* `open(side, skipAnimation)` - Open row on `side`. *Optionally skip animating*

## Feature Checklist
- [x] Support left/right sub views of arbitrary size
- [x] Support basic inertia
- [x] Minimize the number of renders / updates
- [ ] Passing left/right button props instead of views for ease of use
- [ ] Multi sub view staggered position translation
- [ ] Passing pan information to sub views (e.g. for animating icons, bg color, etc)
- [ ] Animate add/remove of SwipeRows from SwipeList
- [ ] Improve gesture inertia