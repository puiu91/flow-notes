# Array of React Items

This is allowed.

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

More specifically, the props type can be:

```js
type TagListProps = {
  tags: Array<React.Element<typeof Tag>>
};

// ...or

type TagListProps = {
  tags: React.ChildrenArray<React.Element<typeof Tag>>
};
```

But, this is not allowed:

```js
type TagListProps = {
  tags: Array<Tag>
};
```
