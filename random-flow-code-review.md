# Random Flow Code Review

## Object Key Types

### Typing Redux State

```js
/*

using https://swapi.co/ as api for the resources and
assuming the redux state has the following shape:

{
  [entities]
    [planets]
      [byHash] = {
        "5": {...},
        "8": {...},
      }
      [byId] = [5, 8]
    [vehicles]
      [byHash] = {...}
      [byId] = [...]
}

*/

type Planet = {|
  name: string,
  rotation_period: string,
  orbital_period: string,
  diameter: string,
  climate: string,
  gravity: string,
  terrain: string,
  surface_water: string,
  population: string,
  residents: Array<string>,
  films: Array<string>,
  created: string,
  edited: string,
  url: string
|};

type State = {
  byHash: { [string]: Planet },
  byId: Array<string>
};
```

### Redux Actions

```js
const LOAD_PLANETS = "planets/LOAD_PLANETS";
const LOAD_PLANETS_SUCCESS = "planets/LOAD_PLANETS_SUCCESS";
const LOAD_PLANETS_FAIL = "planets/LOAD_PLANETS_FAIL";

const UPLOAD_PLANET = "planets/UPLOAD_PLANET";
const UPLOAD_PLANET_SUCCESS = "planets/UPLOAD_PLANET_SUCCESS";
const UPLOAD_PLANET_FAIL = "planets/UPLOAD_PLANET_FAIL";

const DELETE_PLANET = "planets/DELETE_PLANET";
const DELETE_PLANET_SUCCESS = "planets/DELETE_PLANET_SUCCESS";
const DELETE_PLANET_FAIL = "planets/DELETE_PLANET_FAIL";

const planetActions = {
  [LOAD_PLANETS]: LOAD_PLANETS,
  [LOAD_PLANETS_SUCCESS]: LOAD_PLANETS_SUCCESS,
  [LOAD_PLANETS_FAIL]: LOAD_PLANETS_FAIL,
};

type AllowedActions = $Keys<typeof planetActions>;

function testWorks() : AllowedActions {
  return LOAD_PLANETS; // ✓
}

function testFails() : AllowedActions {
  return "planets/UPLOAD_PLANET"; // ✗
}
```

### Eliminating Redux Actions with Reduce Example

* <https://stackoverflow.com/questions/48132147/how-can-i-use-a-constant-value-as-a-flowtype-literal>*

```js
// @flow

type State = { +value: boolean };

type FooAction = { type: "FOO", foo: boolean };
type BarAction = { type: "BAR", bar: boolean };

type Action = FooAction | BarAction;

function reducer(state: State, action: Action): State {
  switch (action.type) {
    case "FOO": return { ...state, value: action.foo };
    case "BAR": return { ...state, value: action.bar };
    default:
      (action: empty);
      return state;
  }
}
```

### Redux Links

* <https://medium.com/flow-type/even-better-support-for-react-in-flow-25b0a3485627>
* <https://github.com/facebook/flow/issues/4737>
* <https://github.com/facebook/flow/issues/2377#issuecomment-262894389>
* <https://stackoverflow.com/questions/42195354/flow-types-with-constant-strings-and-dependent-types/42202467#42202467>
* <https://stackoverflow.com/questions/44834435/use-external-constant-when-defining-flow-literal-type>
* <http://frantic.im/using-redux-with-flow>

## React Props with Object Destructuring in SFC vs. Container Component

```js
// @flow

import * as React from "react";
import classNames from "classnames";

type Props = {
  children: React.Element<any>,
  className?: string
};

const CardSFC = (props: Props) => (
  <div
    className={classNames(props.className, {
      "app--section__aside--article__item": true
    })}
  >
    {props.children}
  </div>
);

class Card extends React.Component<Props> {
  render() {
    const { children, className } = this.props;

    const cssClasses = classNames(className, {
      "app--section__aside--article__item": true
    });

    return <div className={cssClasses}>{children}</div>;
  }
}

// or inline props

const InlinedPropsForCardSFC = (props: {
  children: React.Element<any>,
  className?: string
}) => (
  <div
    className={classNames(props.className, {
      "app--section__aside--article__item": true
    })}
  >
    {props.children}
  </div>
);

class InlinedPropsForCard extends React.Component<{
  children: React.Element<any>,
  className?: string
}> {
  render() {
    const { children, className } = this.props;

    const cssClasses = classNames(className, {
      "app--section__aside--article__item": true
    });

    return <div className={cssClasses}>{children}</div>;
  }
}

// or inline props plus destructure object in function arguments

const PropsDestructuredForCardSFC = ({ children, className }: Props) => (
  <div
    className={classNames(className, {
      "app--section__aside--article__item": true
    })}
  >
    {children}
  </div>
);

const InlinedPropsDestructuredForCardSFC = ({
  children,
  className
}: {
  children: React.Element<any>,
  className?: string
}) => (
  <div
    className={classNames(className, {
      "app--section__aside--article__item": true
    })}
  >
    {children}
  </div>
);

export { Card };
```
