# Pull requests with missing milestones

Each valid pull request should specify a milestone for the version it landed in.

## How to determine which milestone

If you're landing in `master`, you can obtain the correct milestone by looking in `brave-core`'s `package.json`
If you're landing in an uplift in a specific branch such as `1.11.x`, then you should specify `1.11.x` as the milestone.
