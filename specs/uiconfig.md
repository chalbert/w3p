# UI Config

Create a `uiconfig.json` file to configure the user interface.

* Defines the preferred patterns
* Controls the appearance of your components
* Provides an abstraction between your design spec and its implementation


Libraries integrate with your configuration.

* Libraries may advertise their support of ui patterns so you can know if they will integrate correctly with your app
* Libraries may use your config to customize their appearance and behavior
* Libraries may expose their own patterns that you may customize

```json
{
  "unit": {
    "default": "px"
  },
  "color": {
    "red": "#f00",
    "coal": "#111",
    "action": "red"
  },
  "font-size": {
    "xxs": "11px",
    "xs": "11px",
    "sm": "12px",
    "md-sm": "13px",
    "md": "14px",
    "md-lg": "16px",
    "lg": "18px",
    "xl": "20px",
    "xxl": "24px"
  },
  "gutter": {
    
  },
  "icon": {
    "type": "svg",
    "default": "small",
    "variants": {
      "small": {
        "size": {
          "height": "24px",
          "width": "24px"
        }
      }
    }
  },
  "font-color": {
    "default": "coal"
  },
  "loader": {
    "delay": 50,
    "minimum": 100,
    "maximum": 1200000
  },
  "button": {
    "gutters": {
      "horizontal": "4px",
      "vertical": "6px"
    },
    "loader": {
      "position": "left"
    },
    "variants": {
      "action": {
        "description": "",
        "font-size": "md" 
      }
    }
  }
}
```

## Themes

Themes can be expressed as an extension to the default `uiconfig` by creating a file with the pattern `uiconfig-${theme}.json`.

For example `uiconfig-dark.json` :

```json
{
  "color": {
    "background": "#333",
    "text": "#fff"
  }
}
```

This will result in the deep merge of the base congig plus the theme config.
