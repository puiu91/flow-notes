# flow-notes

## Useful Links

* <https://medium.com/flow-type/even-better-support-for-react-in-flow-25b0a3485627>
* <https://github.com/facebook/flow/blob/master/website/en/docs/react/types.md> 
* <https://flow.org/en/docs/react/children/>
* <https://medium.freecodecamp.org/a-comprehensive-guide-to-type-checking-react-redux-and-react-redux-with-flow-4219bdc4ef89>

Particularily for this example
```js
import * as React from 'react';

class TabBarIOSItem extends React.Component<{}> {
 // implementation...
}

type Props = {
 children: React.ChildrenArray<React.Element<typeof TabBarIOSItem>>,
};

class TabBarIOS extends React.Component<Props> {
 static Item = TabBarIOSItem;
 // implementation...
}

<TabBarIOS>
 <TabBarIOS.Item>{/* ... */}</TabBarIOS.Item>
 <TabBarIOS.Item>{/* ... */}</TabBarIOS.Item>
 <TabBarIOS.Item>{/* ... */}</TabBarIOS.Item>
</TabBarIOS>;
```
