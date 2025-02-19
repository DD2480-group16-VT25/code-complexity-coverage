# Report for assignment 3

## Project

Name: scrapy

URL: https://github.com/scrapy/scrapy

Scrapy is a BSD-licensed fast high-level web crawling and web scraping framework, used to crawl websites and extract structured data from their pages. It can be used for a wide range of purposes, from data mining to monitoring and automated testing.(Description is from the README of the project)

## Onboarding experience

Did it build and run as documented?
    
See the assignment for details; if everything works out of the box,
there is no need to write much here. If the first project(s) you picked ended up being unsuitable, you can describe the "onboarding experience" for each project, along with reason(s) why you changed to a different one.

#### Teammates (https://github.com/TEAMMATES/teammates)

Our first project ended up having very little java-backend code we could find tests for. And we figured it would be hard to work with such a frontend-heavy project. So we switched project.

#### Scrapy (https://github.com/scrapy/scrapy)

Currently chosen project.


## Complexity

1. What are your results for five **(three)** complex functions?
   * Did all methods (tools vs. manual count) get the same result?
   * Are the results clear?


      * **_process_spider_output@186-273@scrapy/core/spidermw.py**

         Lizard gave the CCN of 18, we counted by hand and got 18 as well.

      * **_get_inputs@159-195@scrapy/http/request/form.py**

         Lizard gave the CCN of 16, we counted by hand and got 15.

      *  **_get_serialized_fields@72-108@scrapy/exporters.py**

         Lizard give the CCN of 14, we counted by hand and got 16.

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

Plan for refactoring complex code:
  * **_process_spider_output:** This Method could be refactored in order to reduce its CC and improve readability. For example the logic for chosing the method and determining if an upgrade or downgrade is needed could be moved to a separate method. This would reduce the CC of the Method by 4 and and would also make the methods more manageble. 

   * **_get_serialized_fields:** By moving the logic for retriving the field iterator to a new method. The CC of this Method could be reduced by 4.

   * **_get_inputs:** For this method the logic for extracting values from the inputs could be moved to a new method which would reduce the CC of the _get_inputs method by 3.




Estimated impact of refactoring (lower CC, but other drawbacks?).

Carried out refactoring (optional, P+):

git diff ...

## Coverage

### Tools

Document your experience in using a "new"/different coverage tool.

How well was the tool documented? Was it possible/easy/difficult to
integrate it with your build environment?

### Your own coverage tool

Show a patch (or link to a branch) that shows the instrumented code to
gather coverage measurements.

The patch is probably too long to be copied here, so please add
the git command that is used to obtain the patch instead:

git diff ...

What kinds of constructs does your tool support, and how accurate is
its output?

### Evaluation

1. How detailed is your coverage measurement?

2. What are the limitations of your own tool?

3. Are the results of your tool consistent with existing coverage tools?

## Coverage improvement

Command to get branch coverage using their `tox` tool:

```bash
$env:COV_ARGS="--branch"; tox -e py -- tests
```

###### _process_spider_output

Old coverage: 100%, 24 branches. Higher than rest of code.

New coverage: ...

###### _get_inputs

Old coverage: 100%, 6 branches. Higher than rest of code.

New coverage: ...

###### _get_serialized_fields

Old coverage: 100%, 16 branches. Higher than rest of code.

New coverage: ...

_next_request@167-207@scrapy/core/engine.py - coverage 94%, CCN 13
run@70-110@scrapy/commands/check.py - coverage 96%, CCN 13
_parse_sitemap@69-95@scrapy/spiders/sitemap.py - coverage 82%, CCN 12
_cb_bodyready@465-552@scrapy/core/downloader/handlers/http11.py - coverage 94%, CCN 11
dataReceived@650-700@scrapy/core/downloader/handlers/http11.py, - coverage 90%, CCN 11
xmliter_lxml@81-121@scrapy/utils/iterators.py - coverage 95%, CCN 11

get_func_args@215-241@scrapy/utils/python.py - coverage 81%, CCN 11 -- ellen is working on this, coverage now: 94%
   python3 -m tox  -e py -- tests/test_utils_python.py


#### Evaluation

Show the comments that describe the requirements for the coverage.

Report of old coverage: [link]

Report of new coverage: [link]

Test cases added:

git diff ...

Number of test cases added: two per team member (P) or at least four (P+).

## Self-assessment: Way of working

Current state according to the Essence standard: ...

Was the self-assessment unanimous? Any doubts about certain items?

How have you improved so far?

Where is potential for improvement?

## Overall experience

What are your main take-aways from this project? What did you learn?

Is there something special you want to mention here?
