---
name: Monthly Release Issue
about: An issue to track the dates and tasks for a scheduled monthly release
labels: release management
title: Release [RELEASE NUMBER]
---

<!-- Replace all bracketed dates and release numbers below AND in the title above -->
Release [RELEASE NUMBER] is scheduled to be deployed to production on: **[RELEASE DATE]**.

### Events
<!-- The timeline listed here is a suggestion. Edit it as needed to account for holidays or key people's schedules -->
* **[RELEASE DATE minus 14 days]** (Monday): ✏️ Release prep meeting
* **[RELEASE DATE minus 12 days]** (Wednesday): 🧊 Freeze the schema that will be introduced in this release
* **[RELEASE DATE minus 7 days]** (Monday): 🧪 Begin testing final software versions in dev environments
* **[RELEASE DATE]** (Monday): 🚀 Deploy to production

### Non-standard tasks/steps
If there is a task/step you want the release process to include, which isn't already part of the [standard release process](https://github.com/microbiomedata/infra-admin/tree/main/releases) (documented in the private infra-admin repo), please describe it in a comment below.

### Blocking issues
If there are any issues which **must** be resolved before deploying the release to production, edit this description and list them here.
- 
