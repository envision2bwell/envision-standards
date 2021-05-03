# Envision Standards
> This project is for documenting the Envision2BWell, some good practices and is seen as the general starting place for working on the Envision2BWell

Available here https://envision2bwell.github.io/standards

## SCRUM process
### Work item
- should be goal oriented
- split into small unit <6h of work
- work everyday as a team to achieve at least a deliverable (something the end user can see/feel)


## Current point of failure
- automated test
- progress on current task
- every work item should go through a pull request
  * we don't push on master unless all automated tests go green
  * PR example: https://gitlab.com/envisionwell-reboot/blog/-/merge_requests/1
  * only tech lead and tester should be able to merge PR as they are accountable for it working exactly as expected
- separate environments :checked:
- extract credential as environment variable from code

## Architecture
- Blog (to merge in main app)
- About (to merge in main app)
- Telewellness (to merge in main app)
- api: won't be package but instead directly injected in proper package (ie blog api in blog package) using laravel resource to return api data
- Envision2BWell: main web app consuming packages

## Needs
- Document every feature (todo, in progress and done)
  * describe as expected result (goal oriented)
  * able to share same objectives across different department (data, coding, marketing)
  * documentation means
- Remove sandbox repositories as it makes no sense when we can use git branches:
  * develop: staging (sandbox)
  * master: production

- Privacy: Developer should not have access to production data. We should use dummy data (faker).
This will ensure that developers are not accountable or handling sensitive/private user data.
- Security: Developer should not see user password
- Not having access at all. Developer does not work with data but with data structure.


## Next steps
- I would love to say we are good but we're not but good news: we have better then seek to be among the best. 
- Write test for core features
- While dev team working on new features to ship
- Refactor to package
- Integrate (merge) new changes since last commit before refactoring

## [Additional resources](./docs)

## [Our processes][./docs/process.md]
