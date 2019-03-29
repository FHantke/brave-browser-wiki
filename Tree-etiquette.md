- Congratulations, if you're reading this wiki and you're part of the Brave team, you're a Tree Sheriff.
- Only merge something if brave-browser jenkins [[CI status|Checks-done-on-Pull-Request]] passes for the pipeline steps. If you don't know how to do this, check with @mihaiplesa for training.
- If you don't see the results from Jenkins, it might be because the branch is on a fork and not directly on `brave-core` or `brave-browser`.
- If you see something that's broken, or tests failing, instantly revert it. Do not wait to talk to the person that broke it, especially if they are not there.  Re-open the issue that broke it.
- If you accidentally break the build, apologize and pay it forward by helping out when someone else breaks the build.
- Check #brave-core Slack and only merge things if the channel topic indicates the branch you're landing to is open.

# Common fixes for tree breakage
- When dependencies are updated (for example, fixing `npm run audit`), please be sure the `DEPS` file is updated too! This is extremely important. Each channel will need a separate `DEPS` bump

# When does a tree close?
- A tree branch closes when builds fail, brave_unit_tests fail, brave_browser_tests fail, audit-deps fails, or the network audit fails. 

# Why revert so aggressively?

- Each pull request has CI run, if the tree is in a broken state, then you risk breaking every PR's build status. 
- It wastes time at scale for every person that spends time thinking that their own changes are breaking things. 
- QA and other departments are completely blocked if they can't get builds.
- Reverts can become much more complex when changes continue to land on top of the broken code