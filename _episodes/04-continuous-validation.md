---
title: "Continuous Validation"
teaching: 0
exercises: 0
questions:
- ""
objectives:
- ""
keypoints:
- ""
---


In addition to this, the status of a pipeline (integrity over time) 
can be tracked by a [continuous integration (CI) service][ci]. In this 
section we describe how Popper integrates with some existing CI 
services.

## CI System Configuration

The [PopperCLI](https://github.com/systemslab/popper/popper) tool 
includes a `ci` subcommand that can be executed to generate 
configuration files for multiple CI systems. First, add a 
validation script to our pipeline. We add this file in 
`scripts/validate_output.py` with the following contents:

```python

#!/usr/bin/env python
import csv
import sys

fname = sys.argv[1]


with open(fname, 'r') as fi:
    r = csv.reader(fi)

    # get 0-based index of last column in CSV file
    last = len(next(r)) - 1

    for row in r:
        # for years greater than 1950, we should have non-zero mean
        if int(row[0]) < 1950:
            assert float(row[last]) == 0.0
        else:
            assert float(row[last]) != 0
```

The syntax of this command is the following:

```bash
popper ci <service-name>
```

Where `<service-name>` is the name of CI service (see `popper ci 
--help` to get a list of supported systems).

For our example, we will use [Travis CI](http://travis-ci.org). We 
first need to create an account on this service. Assuming our 
repository is already on GitHub, we can enable it on TravisCI so that 
it is continuously validated (see 
[here](https://docs.travis-ci.com/user/getting-started/) for a guide). 
Once the project is registered on Travis, we proceed to generate a 
`.travis.yml` file:

```bash
popper ci travis
```

And commit the file:

```bash
git add .travis.yml
git commit -m 'Adds TravisCI config file'
```

We then can trigger an execution by pushing to GitHub:

```bash
git push
```

After this, we go to the TravisCI website to see your pipelines being 
executed. Every new change committed to a public repository will 
trigger an execution of your pipelines. To avoid triggering an 
execution for a commit, include a line with `[skip ci]` as part of the 
commit message.

> **NOTE**: TravisCI has a limit of 50 minutes for running a workflow, 
> after which the test is terminated and failed.

[ci]: https://en.wikipedia.org/wiki/Comparison_of_continuous_integration_software
