#  Global-React-Webpack

Small repository to show how to extract React/ReactDOM from the webpack script bundle, so they can be referenced globally.  The benefit of this might be that should compiled React code live within a third party tool/bundle (Maxymiser for example) that code could be delivered to and executed on the client.

When Webpack bundles React/ReactDOM as standard, React/ReactDOM are not globally accessible.

## How it works

There are a few *bits* that make this work.

### Webpack.config.js

```javascript
module.exports = {
  ...
  module: {
    ...
    noParse: [/\/react\//g, /\/react-dom\//g]
  }
  resolve: {
    ...
    alias: {
      react: 'react.setup.js',
      'react-dom': 'react-dom.setup.js'
    }
  },
  externals: {
    react: 'React',
    'react-dom': 'ReactDOM'
  }
}
```

Add the `externals`, `alias`, and `noParse` to instruct webpack not to include these resources in the bundle.

### Index.html
Add references to React on a CDN of your choice.

```html
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react/16.4.1/umd/react.development.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/16.4.1/umd/react-dom.development.js"></script>
```

### Setup files
You need a setup file for React and React DOM

#### react.setup.js
```javascript
module.exports = window.React
```

#### react-dom.setup.js
```javascript
module.exports = window.ReactDOM
```

This is easy to test, spin up your web server, open the console and type `window.React` or `window.ReactDOM` and you should not see `undefined` but a reference to these libraries themselves.

  

