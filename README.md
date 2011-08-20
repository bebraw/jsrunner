jsrunner - Simple runner pattern for JavaScript

This works well with [jsassert](https://github.com/bebraw/jsassert). It actually uses this to test itself so check it out for a concrete demo.

The runner consists of one function: "tests". It is used both to collect tests to run and to actually run the tests.

A simple test set may look like this:

    tests('Color', {
        _: { // set up
            c: color()
        },
        initialValue: function() { // actual test
            assert(this.c.r()).equals(0); // do some actual tests now
            // in this case we're using jsassert. you can use whatever
            // assert method you want
        },
        ... // more tests
    });

In order to get the tests run, define some kind of HTML file as follows:

    <html>
    <head>
        <title>Tests</title>

        <style type="text/css">
            .passed {
                color: green;
            }
        
            .failed {
                color: red;
            }
        </style>
        <script type="text/javascript" src="assert.js"></script>
        <script type="text/javascript" src="runner.js"></script>
        <script type="text/javascript" src="tests.js"></script>
        <script type="text/javascript">
            window.onload = function() {
                var outputArea = document.getElementById('testOutput');

                // define a couple of sinks for output
                var HTMLOutput = function(report) {
                    outputArea.innerHTML += '<div class="' + report.state +
                        '">' + report.text + '</div>';
                };

                var consoleOutput = function(report) {
                    console.log(report.text);
                };

                // empty "tests" contains run
                tests().run({
                    output: HTMLOutput, // sink for output
                    refresh: 2000 // in ms, default zero
                });
            }
        </script>
    </head>
    <body>
        <div id="testOutput"></div>
    </body>
    </html>
