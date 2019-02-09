# Strange Error to Investigate

## Error

Given these two files, we get an error when calling `<Tag id={5}/>`.

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
      <Tag id={5}/> //=> Error:(13, 8) Cannot reference type `Tag` [1] from a value position.
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

## Solution

Extracting the shared type to own file.

```js
// ------------------------------
// index.js
// ------------------------------

// @flow
import * as React from "react";
import { Tag } from "./components/Tag";
import type { TagListProps } from "./justTypes";

<Tag id={5} />;

class TagList extends React.Component<TagListProps> {
  render() {
    return (
      <Tag id={5}/>
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

class Tag extends React.Component<TagProps, {}> {
  render() {
    return <span>hello world</span>;
  }
}

export { Tag };

// ------------------------------
// types.js
// ------------------------------

// @flow
import * as React from "react";
import { Tag } from "./components/Tag";

export type TagListProps = {
  tags: React.ChildrenArray<React.Element<typeof Tag>>
};
```
