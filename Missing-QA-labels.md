# Labeling

Each valid issue in a release should have exactly one of the following 2 labels: `QA/Yes` or `QA/No`.  Unsurprisingly, you should use `QA/Yes` if you would like your issue to be QA'ed.

# What does `QA/Yes` mean?

It means the issue will be tested once by QA on all platforms before it is released.

# After I have tagged my issue with `QA/Yes`, how do I specify a test plan?

Fill out the Test Plan section in the issue's PR. Once the test plan has been specified, please add the label `QA/Test-plan-specified` to the issue.

# When does QA'ing happen?

QA is usually done on an issue when it reaches the Beta channel.

# What if I want something tested on each release?

Send a DM to one of the QA team members to make it happen.
They'll manually update some checklists that they maintain.
