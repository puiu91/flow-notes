# Array of React Items

```js
// @flow

import * as React from "react";

type TagProps = {
  id: number
};

class Tag extends React.Component<TagProps, {}> {
  render() {
    return <span>hello world</span>;
  }
}

type TagListProps = {
  tags: Array<React.Node>
};

class TagList extends React.Component<TagListProps> {}

<TagList
  tags={[
    <Tag id={1} />,
    <Tag id={1} />,
    <Tag id={1} />,
    <Tag id={1} />,
    <Tag id={1} />,
    <Tag id={1} />,
    <Tag id={1} />,
    <Tag id={1} />,
    <Tag id={1} />
  ]}
/>;
```
