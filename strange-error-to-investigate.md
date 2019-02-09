# Strange Error to Investigate

Given these two files, we get an error calling `<Tag id={5}/>`.
* Error:(13, 8) Cannot reference type `Tag` [1] from a value position.

```js
// ------------------------------
// index.js
// ------------------------------

// @flow
import * as React from "react";
import type { Tag, TagListProps } from "./components/Tag";

class TagList extends React.Component<TagListProps> {
  render() {
    return (
      <Tag id={5}/> //=> flow error thrown here
    );
  }
}

// ------------------------------
// Tag.js
// ------------------------------

// @flow
import * as React from "react";

type TagProps = {
  id: number
};

export type TagListProps = {
  tags: Array<React.Node>
};

export class Tag extends React.Component<TagProps, {}> {
  render() {
    return <span>hello world</span>;
  }
}
```
