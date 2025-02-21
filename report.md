# Report for assignment 3

## Project

Name: scrapy

URL: https://github.com/scrapy/scrapy

Scrapy is a BSD-licensed fast high-level web crawling and web scraping framework, used to crawl websites and extract structured data from their pages. It can be used for a wide range of purposes, from data mining to monitoring and automated testing.(Description is from the README of the project)

## Onboarding experience

#### Teammates (https://github.com/TEAMMATES/teammates)

Our first project ended up having very little java-backend code we could find tests for. And we figured it would be hard to work with such a frontend-heavy project. So we switched project.

#### Scrapy (https://github.com/scrapy/scrapy)

This project was easy to set up, the only tool we needed to install was tox for testing.


## Complexity

1. What are your results for five **(three)** complex functions?
   * Did all methods (tools vs. manual count) get the same result?
   * Are the results clear?
      * **_process_spider_output@186-273@scrapy/core/spidermw.py:** lizard gave the CCN of 18, we counted by hand and got 18 as well.
      * **_get_inputs@159-195@scrapy/http/request/form.py:** lizard gave the CCN of 16, we counted by hand and got 15.
      *  **_get_serialized_fields@72-108@scrapy/exporters.py:** lizard give the CCN of 14, we counted by hand and got 16.

      We don't think the results are very clear, we also found it quite confusing counting some of the functions.
   
2. Are the functions just complex, or also long?

   The functions are just complex, not long. They are all mostly if and for loops.

3. What is the purpose of the functions?
   * **_process_spider_output:** ensures that both synchronous and asynchronous iterables from the spider output are handled correctly. It manages error handling by catching exceptions and it returns a chain of results.
   * **_get_inputs:** takes the inputs in the given form and returns a key-value pair for them.
   * **_get_serialized_fields:** takes the fields about to be exported and returns them as an iterable of tuples.

4. Are exceptions taken into account in the given measurements?

   We counted the exceptions and got the same result as the lizard tool on the **_process_spider_output** function which includes an exception which means it also counts the exceptions when measuring the cyclomatic complexity.
   
5. Is the documentation clear w.r.t. all the possible outcomes?
   * **_process_spider_output:** the documentation only states what the function returns and what the parameters are. There are some comments in the function to explain the code.
   * **_get_inputs:** we found no documentation for this function. The only comments for this function is what it returns.
   * **_get_serialized_fields:** we found no documentation for this function. The only comments for this function is what it returns.

## Refactoring

Plan for refactoring complex code and the estimated impact:
   * **_process_spider_output:** this function could be refactored in order to reduce its CC and improve readability. For example the logic for choosing the method and determining if an upgrade or downgrade is needed could be moved to a separate method. This would reduce the CC of the function by 4 and and would also make the function more manageble. 
   * **_get_serialized_fields:** by moving the logic for retriving the field iterator to a new method, the CC of this function could be reduced by 5.
   * **_get_inputs:** for this function, the logic for extracting values from the inputs could be moved to a new method which would reduce the CC by 5.

<!-- Carried out refactoring (optional, P+):

git diff ... -->

## Coverage

### Tools

**Document your experience in using a "new"/different coverage tool.** To measure coverage we used the `coverage` tool. We ran the tests using the `tox` tool and then ran the coverage tool to get the coverage report. The coverage tool was easy to use and the report was very easy to understand.

**How well was the tool documented? Was it possible/easy/difficult to integrate it with your build environment?** It was very easy to integrate the coverage tool with the build environment since it seems to use the last test result by `tox` by default. The tool was well documented.

### Your own coverage tool

> The DIY branch coverage tool works by manually annotating the code with `track_branch(func_name, branch_id)` calls. The tool then runs the original test suite and collects the coverage data which is then reported in the console.

> The tool can be found in `scrapy/diy_coverage/diycoverage.py`, and the runnable code is under `scrapy/diy_coverage/run_coverage.py`.

**Show a patch (or link to a branch) that shows the instrumented code to gather coverage measurements.**  See the commit history of the `dev-DIY-coverage` branch on the forked repository. https://github.com/DD2480-group16-VT25/scrapy/tree/dev-DIY-coverage.

<!-- The patch is probably too long to be copied here, so please add
the git command that is used to obtain the patch instead:

git diff ... -->

### Evaluation

1. How detailed is your coverage measurement?

The quality of the coverage measurement seems to be good but since the test environment of the project uses `tox` to download many dependencies, taking ~15 minutes to run, we settled on just running the tests using the Python standard `unittest` module and then accepting that some tests will throw errors due to missing dependencies. This results in the branch coverage being less than 100% for the 3 functions we measured.

2. What are the limitations of your own tool?

The tool cannot be used on ternary operators unless you manually expand them into if-else statements.

3. Are the results of your tool consistent with existing coverage tools?

Yes, considering the limitations of `unittest` compared to the `tox` test environment.

## Coverage improvement

Command to get branch coverage using their `tox` tool:

```bash
$env:COV_ARGS="--branch"; tox -e py -- tests
```

or 

```bash
python3 -m tox -e py -- tests
```

And to get the report afterwards:

```bash
coverage html
```

###### _process_spider_output

Old coverage: 100%, 24 branches. Higher than rest of code.

New coverage: N/A

###### _get_inputs

Old coverage: 100%, 6 branches. Higher than rest of code.

New coverage: N/A

###### _get_serialized_fields

Old coverage: 100%, 16 branches. Higher than rest of code.

New coverage: N/A

### Functions to improve coverage

Since the three functions with the highest CCN all had 100% coverage, we chose new functions to improve coverage on. 

###### get_func_args (scrapy/scrapy/utils/python.py)

Old coverage: 81%, 12 branches.

New coverage: 94%

###### dataReceived (scrapy/core/downloader/handlers/http11.py)

Old coverage: 90%, 10 branches.

New coverage: 100%.

###### run (scrapy\commands\settings.py)

Old coverage: 63%, 12 branches.

New coverage: 70%.

#### Evaluation

Show the comments that describe the requirements for the coverage.

Report of old coverage: [htmlcov/index.html](htmlcov/index.html) (open in browser for intractive report)

Report of new coverage: [link]

Test cases added:

git diff ...

Two tests for **get_func_args:**

```diff
diff --git a/tests/test_utils_python.py b/tests/test_utils_python.py
index a693d6b53..c3dd40a27 100644
--- a/tests/test_utils_python.py
+++ b/tests/test_utils_python.py
@@ -2,6 +2,7 @@ import functools
 import operator
 import platform
 import sys
+from unittest.mock import patch
 
 import pytest
 from twisted.trial import unittest
@@ -231,6 +232,9 @@ class UtilsPythonTestCase(unittest.TestCase):
         self.assertEqual(get_func_args(object), [])
         self.assertEqual(get_func_args(str.split, stripself=True), ["sep", "maxsplit"])
         self.assertEqual(get_func_args(" ".join, stripself=True), ["iterable"])
+        # The parameter has to be callable, if not, like this int, it triggers a TypeError,
+        # which was not previously reached.
+        self.assertRaises(TypeError, get_func_args, 123)
 
         if sys.version_info >= (3, 13) or platform.python_implementation() == "PyPy":
             # the correct and correctly extracted signature
@@ -245,6 +249,16 @@ class UtilsPythonTestCase(unittest.TestCase):
                 [[], ["args", "kwargs"]],
             )
 
+    @patch("inspect.signature", side_effect=ValueError("some error"))
+    def test_get_func_args_value_error(self, signature):
+        # Test that if inspect.signature raises a ValueError, get_func_args returns an empty list.
+        # This ValueError was not previously reached.
+        def f4(a, b):
+            pass
+
+        # When inspect.signature raises a ValueError, the function should return [] (args).
+        self.assertEqual(get_func_args(f4), [])
+
     def test_without_none_values(self):
         self.assertEqual(without_none_values([1, None, 3, 4]), [1, 3, 4])
         self.assertEqual(without_none_values((1, None, 3, 4)), (1, 3, 4))
```

One test for **dataReceived:**

```diff
diff --git a/tests/test_downloader_handlers.py b/tests/test_downloader_handlers.py
index 323a51002..61f4b6fd1 100644
--- a/tests/test_downloader_handlers.py
+++ b/tests/test_downloader_handlers.py
@@ -506,6 +506,39 @@ class Http11TestCase(HttpTestCase):
+    @defer.inlineCallbacks
+    def test_download_with_maxsize_very_large_file_should_warn(self):
+        """Test that a warning is logged when the download warn size is exceeded. Also test that the
+        download is aborted when the download max size is exceeded.
+        Inspired by test_download_with_maxsize_very_large_file.
+        """
+        with mock.patch("scrapy.core.downloader.handlers.http11.logger") as logger:
+            request = Request(self.getURL("largechunkedfile"))
+            # Set Spider warnsize to trigger warning
+            d = self.download_request(request, Spider("foo", download_maxsize=1500, download_warnsize=1000))
+            yield self.assertFailure(d, defer.CancelledError, error.ConnectionAborted)
+
+            def check(logger):
+                # Check for both warning messages
+                logger.warning.assert_has_calls(
+                    [
+                        mock.call(
+                            "Received more bytes than download warn size (%(warnsize)s) in request %(request)s.",
+                            {"warnsize": 1000, "request": request},
+                        ),
+                        mock.call(mock.ANY, mock.ANY),  # The maxsize warning message
+                    ],
+                    any_order=True,
+                )
+
+            # As the error message is logged in the dataReceived callback, we
+            # have to give a bit of time to the reactor to process the queue
+            # after closing the connection.
+            d = defer.Deferred()
+            d.addCallback(check)
+            reactor.callLater(0.1, d.callback, logger)
+            yield d
```

One test for **run:**

```diff
diff --git a/tests/test_commands.py b/tests/test_commands.py
index 1a0db1e03..1187a6d27 100644
--- a/tests/test_commands.py
+++ b/tests/test_commands.py
@@ -25,6 +25,7 @@ from twisted.trial import unittest
 import scrapy
 from scrapy.cmdline import _pop_command_name, _print_unknown_command_msg
 from scrapy.commands import ScrapyCommand, ScrapyHelpFormatter, view
+from scrapy.commands.settings import Command
 from scrapy.commands.startproject import IGNORE
 from scrapy.settings import Settings
 from scrapy.utils.python import to_unicode
@@ -55,6 +56,22 @@ class CommandSettings(unittest.TestCase):
         )
         self.assertEqual(dict(self.command.settings["FEEDS"]), json.loads(feeds_json))

+    def test_settings_getlist(self):
+        command = Command()
+        command.settings = Settings()
+        command.crawler_process = mock.Mock()
+        command.crawler_process.settings = Settings()
+        command.crawler_process.settings.set('TEST_LIST', ['a', 'b', 'c'])
+
+        parser = argparse.ArgumentParser()
+        command.add_options(parser)
+
+        args = parser.parse_args(['--getlist', 'TEST_LIST'])
+
+        with mock.patch('sys.stdout', new=StringIO()) as mock_stdout:
+            command.run([], args)
+            self.assertEqual(mock_stdout.getvalue().strip(), "['a', 'b', 'c']")
```

<!-- Number of test cases added: two per team member (P) or at least four (P+). -->

## Self-assessment: Way of working

**Current state according to the Essence standard:** Working Well

**Was the self-assessment unanimous? Any doubts about certain items?** It was unanimous.

**How have you improved so far?** We use GitHub tools more naturally, e.g. creating issues and reviewing other's pull requests.

**Where is potential for improvement?** We could be more effective when working individually, we also tend to have long meetings where we work, which could be made individually.

## Overall experience

**What are your main take-aways from this project? What did you learn?** Branch coverage and code coverage is not the same, it also reminded us that sometimes it's better to split up long and/or complex functions.

**Is there something special you want to mention here?**
