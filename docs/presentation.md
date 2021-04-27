#
- Claiming to be a tech company
- Level up to tech standard. We're not Google but there is the strict minimum

## Summary


## Needs
- ssh access to envisionwellapp.io
- Document every feature (todo, in progress and done)
  * describe as expected result (goal oriented)
  * able to share same objectives across different department (data, coding, marketing)
  * documentation means
- Remove sandbox repositories as it makes no sense when we can use git branches:
  * develop: staging (sandbox)
  * master: production

## Current State failure
- automated test
- progress on current task
- every work item should go through a pull request
  * we don't push on master unless all automated tests go green
  * PR example: https://gitlab.com/envisionwell-reboot/blog/-/merge_requests/1
  * only tech lead and tester should be able to merge PR as they are accountable for it working exactly as expected
- scrum process
    * work item
      - should be goal oriented
      - split into small unit <6h of work
      - work everyday as a team to achieve at least a deliverable (somethiing the end user can see/feel)
- separate environments :checked:
- extract credential as environment variable from code
- From the UI to the implementation
  * once we receive the figma design or just image preview, the UI team (android, iOS, web) will build the page in html, css, xml only to have the visual match
  * Disable all button for interaction until the feature is ready
  * Enable the feature on the next iteration
  

## Architecture
- Blog (package)
- About (package)
- api: won't be package but instead directly injected in proper package (ie blog api in blog package) using laravel resource to return api data
- Envision2BWell: main web app consuming packages

## Progress
### Github org
- Makes clear that the company owns all the code https://github.com/orgs/envision2bwell
- [Documentation](./index.md)

## Wrap up & Next steps
- I would love to say we are good but we're not but good news: we have better then seek to be among the best. 
- Write test for core features
- While dev team working on new features to ship
- Refactor to package
- Integrate (merge) new changes since last commit before refactoring
