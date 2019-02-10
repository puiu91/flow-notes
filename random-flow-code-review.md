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
