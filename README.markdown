# Sandbox

Sandbox is a YUI 3 module that simplifies the process of creating isolated iframe sandboxes in which to evaluate JavaScript code for tasks like profiling or unit testing.

Note that these sandboxes, while isolated enough to be used for testing, are not secure. They should be used only to run trusted code, and are not intended to be used to execute arbitrary untrusted JavaScript.

## API Docs

You'll find API documentation at http://rgrove.github.com/sandbox/docs/

## Example

    YUI().use('gallery-sandbox', function (Y) {
        var sandbox = new Y.Sandbox();

        // Listen for the 'ready' event to ensure that the sandbox iframe has
        // been initialized before you try to use it.
        sandbox.on('ready', function () {
            // Run some code in the sandbox.
            sandbox.run('alert("Hello from " + document.title);');

            // You can also pass in a function, although it won't retain its
            // execution context when it runs in the sandbox since it will be
            // cast to a string and then evaled.
            sandbox.run(function () { alert("I like pie."); });

            // If your code will perform async operations and you'd like to be
            // notified when they've finished, call done() from the sandbox code
            // and pass in a callback to receive the finish notification.
            sandbox.run(function () {
                // Runs in sandbox.
                setTimeout(function () {
                    done();
                }, 1000);
            }, function () {
                // Runs in parent after the sandbox code calls done().
                alert('All done!');
            });

            // For easy profiling, run code with profile(). Call done() from the
            // sandbox code to indicate completion. Provide a callback to
            // receive the profile data once it's ready.
            sandbox.profile(function () {
                var i = 5000000;

                while (i--) {
                    Math.sqrt(Math.PI);
                }

                done();
            }, function (profileData) {
                alert("That took " + profileData.duration + " milliseconds.");
            });

            // Pass data between the parent and the sandbox using a shared
            // environment object.
            sandbox.setEnvValue('kittens', 'fuzzy');
            sandbox.run(function () {
                alert('Kittens are ' + sandbox.kittens);
                sandbox.ninjas = 'sneaky';
            });

            alert('Ninjas are ' + sandbox.getEnvValue('ninjas'));

            // See the API docs for even more useful stuff.
        });
    });

## License

Copyright (c) 2010 Ryan Grove (ryan@wonko.com)
All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

  * Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.
  * Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.
  * Neither the name of this project nor the names of its contributors may be used to endorse or promote products derived from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
