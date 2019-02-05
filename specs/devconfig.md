# Dev config

Create a `devconfig.json` file to configure development standard. 

* Defines what standards developer should follow
* Enables features and guards in your project
* Provide an abstraction layer between other tool's configurations

Each spec defines its accepted config.

## Format

The format can be nested without limitation.

Example:

```json
{
  "spec": {
    "simpleConfig": true,
    "arrayConfig": [
       "simpleConfig",
       ["configWithOptions", { 
         "booleanOption": true,
         "stringOption": true,
         "arrayOption": [],
         "objectOption": {}
       }]
    ],
    "objectConfig": {
       "simpleConfig": true"
    }
  } 
}
```
