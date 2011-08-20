jsrunner - Simple runner pattern for JavaScript

This works well with [jsassert](https://github.com/bebraw/jsassert). It actually uses this to test itself so check it out for a concrete demo.

The runner consists of one function: "tests". It is used both to collect tests to run and to actually run the tests.

A simple test set may look like this:

```javascript
tests('Color', {
    _: { // set up
        c: color()
    },
    initialValue: function() { // actual test
        assert(this.c.r()).equals(0); // do some actual asserts now
        // in this case we're using jsassert. you can use whatever
        // assert method you want
    },
    ... // more tests
});
```

In order to get the tests run, define some kind of HTML file as follows:

```html
<html>
<head>
    <title>Tests</title>

    <style type="text/css">
        .passed {
            color: green;
        }
        
        .failed {
            color: red;
            padding-top: 0.5em;
        }
        
        .started {
            padding-top: 0.5em;
            font-weight: bold;
        }

        .finished {
            border: 1px solid black;
            margin-top: 0.5em;
            padding-top: 0.5em;
            padding-bottom: 0.5em;
        }
        
        #playback {
            font-weight: bold;
        }
    </style>
    <script type="text/javascript" src="assert.js"></script>
    <script type="text/javascript" src="runner.js"></script>
    <script type="text/javascript" src="tests.js"></script>
    <script type="text/javascript">
        window.onload = function() {
            var outputArea = document.getElementById('testOutput');

            document.body.insertBefore(tests.playbackUI(), document.body.childNodes[0]);

            tests().run({
                output: tests.HTMLOutput(outputArea), // sink for output
                refresh: 2000 // in ms, default zero
            });
        }
    </script>
</head>
<body>
    <div id="testOutput"></div>
</body>
</html>
```

As you can see tests().run provides an output hook that you may attach your handler to. This handler receives report (individual test) containing its text and state (started, passed, failed, error, finished). "tests" also contains two predefined sinks, consoleOutput and HTMLOutput, that may you might find handy.

In addition tests contains simple playbackUI you might want to use. It makes it possible to stop the playback and resume it later (FIXME: currently expects that the runner is already running).
