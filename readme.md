# Test an app using Browser Monkey and karma

For this example we will create a small react application

```bash
yarn add karma karma-mocha karma-chrome-launcher karma-mocha mocha karma-webpack webpack --dev
yarn add react react-dom
```
Now create a test file: `test/greetingSpec.js`
For simplicity we will create our react application in the test file.

```js
const reactMonkey = require('browser-monkey/react')
class HelloWorld extends React.Component {
  render () {
    return React.createElement('div', {className: 'greeting'}, 'Hello World')
  }
}

describe('greeting', () => {
  it('renders a greeting', async () => {
    const monkey = reactMonkey(new HelloWorld())
    await monkey.find('.greeting').shouldHave({text: 'Hello World'})
  })
})
```

Create a karma config file `karma.conf.js`

```js
module.exports = function(config) {
  config.set({
    frameworks: ['mocha'],
    files: [
      'test/**/*Spec.js',
    ],
    preprocessors: {
      'test/**/*Spec.js': [ 'webpack' ]
    },
    reporters: ['progress'],
    port: 9876,
    colors: true,
    browsers: ['Chrome']
  })
}
```

Now you can run the testing using `karma start`

