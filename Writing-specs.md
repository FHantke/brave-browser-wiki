Here is a checklist to help with writing specs.

- [ ] The team who is assigned including QA, Designer, PM, and devs per platform.
- [ ] An objective: Generic description of what's being planned.
- [ ] The status of the spec `Pre-draft` (not ready for review), `Draft`, `In Development`, or `Complete` which signals that it is landed on Nightly.
- [ ] How does the feature relate to high level objectives of the company (ACRE).
- [ ] Non-goals of the spec.
- [ ] Target platforms that will be supported (e.g. Desktop, iOS, Android, or which sites it applies to etc.)
- [ ] Does clearing browsing data affect the thing being spec'ed?
- [ ] Does the thing being spec'ed change with different window types? E.g. Private window and Tor window?
- [ ] Will the feature be available on all channels? Is a feature flag needed so that work can land incrementally without reaching Release channel until it's ready?
- [ ] Are there any open questions? List them explicitly. 
- [ ] What are the states of what you are designing?
- [ ] Which state is persistent and which state is not.
- [ ] Did you specify not only how things should be but what has to change?
- [ ] Can a time estimation be provided? 


If there are UI component
- [ ] Do you have mockups for every view and every state?
- [ ] Did you specify dimensions?
- [ ] Did you specify color values?
- [ ] Did you specify how it works responsively?
- [ ] Did you specify if these things are different per platform?
- [ ] If there are links, make sure a plan is in place to get the documentation created before the code lands.

If there are technical components
- [ ] Provide technical information when possible, work with a developer if needed.

When there are security sensitive or privacy concerns
- [ ] File a security review issue.

Process considerations
- [ ] Post the specs for review on #specs.
- [ ] Callout major status changes when the draft enters a new status.