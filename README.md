node-openalpr
============

This package binds OpenALPR with Node.js

# Installation and Example

Use npm to get the node-openalpr package. We'll attempt to use node-pre-gyp to compile from source, but if
that's not possible we'll fallback to precompiled binaries.

```npm install node-openalpr```

### Example

```javascript
var openalpr = require ("node-openalpr");

function identify (id, path) {
	console.log (openalpr.IdentifyLicense (path, function (error, output) {
		console.log (id +" "+ output.processing_time_ms);
	
		if (id == 349) {
			console.log (openalpr.Stop ());
		}
	}));
}

openalpr.Start ();
openalpr.GetVersion ();

for (var i = 0; i < 350; i++) {
	identify (i, "lp.jpg");
}
```

### Methods

This is a breakdown of all of the methods available for node-openalpr. Start needs to be called before any other method.

* `openalpr.Start ([config, [runtime, [count, [start_queue]]]])` - Initializes OpenALPR with default settings
  * config - Path to configuration file. On Windows defaults to the config file in node-openalpr directory, on Linux defaults to openalpr installation
  * runtime - Path to runtime data. On Windows defaults to "openalpr_runtime" folder in node-openalpr directory, on Linux defaults to openalpr installation
  * count - Number of concurrent OpenALPR processes to run - defaults to CPU core count
  * start_queue - Auto start queue monitoring thread - defaults to true
* `openalpr.Stop ()` - Stops the OpenALPR processes and clears out any queued images
* `openalpr.StartQueue ()` - Starts the OpenALPR queue monitoring thread (normally started automatically after calling Start ())
* `openalpr.StopQueue ()` - Stops the OpenALPR queue monitoring thread
* `openalpr.queueLoop ()` - Method used in checking queue - can be called manually if start_queue is false for finer control
* `openalpr.IdentifyLicense (path, options/callback[, callback])` - Begins the process of identifying a license from the given image
  * path - Path to image - if image does not exist an exception will be thrown
  * callback/options - Additional options for the image or a callback
    * options.state         (string) - State ("oh") license plates are in for additional validation
    * options.prewarp       (string) - Prewarp configuration information
    * options.detectRegion  (boolean) - Use detect region functionality of OpenALPR? (slower)
  * callback - Callback with results: function (errors, output)
* `openalpr.GetVersion ()` - Get the version of OpenALPR currently being run against

# How to Compile

0. [Download and install io.js v3.0.0+](https://iojs.org/en/index.html)
0. [Download and install git](https://git-scm.com/downloads)

#### Windows

0. [Download and install Visual Studio 2013/2015](https://www.visualstudio.com/)
0. Run PowerShell ISE as an administrator and execute: Set-ExecutionPolicy RemoteSigned
0. Run openalpr-install.ps1
0. Take output from openalpr/windows/build/dist and put into "lib" and "release/win32" folder in node-openalpr
0. Run npm install

#### Linux

0. Run openalpr-install.sh
0. Run npm install