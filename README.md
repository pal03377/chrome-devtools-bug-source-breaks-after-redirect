# Bug in Chromium Issue Tracker
https://issues.chromium.org/issues/365405774

# Summary
When opening the DevTools after a 302 redirect, the links to JS code break.

# Steps to reproduce the problem
1. Open https://chrome-devtools-bug.pschwind.de/redirect WITHOUT the DevTools open. You will be redirected to https://chrome-devtools-bug.pschwind.de
1. Now open the DevTools. Open the Console tab. You will see a JavaScript error.
1. Follow the reference for the source of the error (index:3).
1. No source code is shown in the source tab!
1. Repeat the experiment, but already open the DevTools BEFORE opening the link in the tab. The source of the error is correctly shown in the Source tab.

# Steps to create the server for https://chrome-devtools-bug.pschwind.de:
1. Set up a server that sends a 302 redirect for the /redirect path to /
1. Set it up so that the server always sends this header: Cache-Control "no-cache, must-revalidate, max-age=0, no-store, private" (! IMPORTANT, otherwise the bug does not occur)
1. Serve this HTML from the document root (/): `<h1>Open the Developer Tools to see the error.</h1><script>throw new Error("This is a test error. Try to click the link on the right to see it in the Source tab.");</script>`

# Problem Description
The source of the JS error cannot be shown because the HTML in the DevTools "Source" tab does not load.

# Additional Comments
The bug is really annoying for debugging, since you cannot quickly see where an error occurs anymore. We are pretty limited by this because we cannot change the redirect nor the cache-control header in our development setup.
