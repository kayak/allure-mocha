# mocha-allure-reporter
This is a fork of mocha-allure-reporter (official Allure reporter for Mocha) with a fix for programmatically
skipped tests breaking the Categories tab.

## Installation

Assuming that you have [mocha](http://mochajs.org/) installed, install reporter via npm:

```
npm install mocha-allure-reporter@1.4.1-k.1
```

Then use it as any other mocha reporter:

```
mocha --reporter mocha-allure-reporter
```

After running tests you will get raw tests result into `allure-results` directory.
See [generator list](https://github.com/allure-framework/allure-core/wiki#generating-a-report)
on how to make a report from raw results.

Also check out [mocha-allure-example](https://github.com/allure-examples/mocha-allure-example) to see it in action.

## Supported options

* targetDir _(string)_ – directory where test results will be stored

## Runtime API

Allure is a test framework which provides more data from tests than usual. Once added `mocha-allure-reporter` will create global `allure` object with the following API:

* `allure.createStep(name, stepFn)` – define step function. Result of each call of this function will be recorded into report.
* `allure.createAttachment(name, content, [type])` – save attachment to test. If you're calling this inside step function or during its execution (e.g. asynchronously via promises), attachment will be saved to step function.
    * `name` (*String*) - attachment name. Note that it is not then name of the file, actual filename will be generated.
    * `content` (*Buffer|String|Function*) – attachment content. If you pass Buffer or String, it will be saved to file immediately. If you are passing Function, you will get decorated function and you can call it several times to trigger attachment. General purpose of the second case is an ability to create utility function to take screenshot. You can define function for you test framework only once and then call it each time you need a screenshot.
    * `type` (*String*, optional) – attachment MIME-type. If you omit this argument we'll try to detect type automatically via [file-type](https://github.com/sindresorhus/file-type) library
* `allure.description(description)` – set detailed test description, if test name is not enough.
* `allure.severity(severity)` – set test severity, one of: `blocker`, `critical`, `normal`, `minor`, `trivial`. You can also use constants like `allure.SEVERITY.BLOKER`.
* `allure.feature(featureName)` – assign feature to test
* `allure.story(storyName)` – assign user story to test. See [documentation](https://github.com/allure-framework/allure-core/wiki/Features-and-Stories) for details
* `allure.addArgument(name, value)` - provide parameters, which had been used in test. Unlike other languages, javascript test methods usually doesn't have special arguments (only callbacks), so developers use other way to populate parameters to test. This method is to provide them to Allure
* `allure.addEnvironment(name, value)` - save environment value. It is similar to `addArgument` method, but it is designed to store more verbose data, like HTTP-links to test page or used package version.
