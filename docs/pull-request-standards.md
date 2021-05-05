# Standard Requirements
When getting a Pull Request approved here are what Approvers are looking for at minimum.

0. Before you start:
  - branch of from your default branch (`develop` most likely): 
  ```bash
  git checkout develop;
  git checkout -b issue/$issue
  ```
  - Start work
  - Push daily
1. PR title should match issue name
2. PR description should contain "closes #issue-number"


<br/>

## Closes What
- Pull requests must contain a "closes" _where `github-issue-number` is the actual Github Issue number_

```
closes #github-issue-number
```

- Github Issue must must have a description and Acceptance Criteria.
- Github there are times where we need to do a small patch or build change. In these moments, you can use the Pull Request to mark all the content above.

If it relates to an issue but doesn't close it (rare),

```
relates #github-issue-number
```


## Requires
If there is a requirement mark it

```
requires #pull-request-number
```

It is important that you watch your Pull Request for any gate failures. This means if tests fail or if a Change Request is made, you are responsible for updating the code.

<br/> 

## Tests Provide Evidence for the Feature Code

The approvers will ensure that there is appropriate code coverage. The team members should be practicing *Test Driven Development and Design*

  - UI Tests should follow the structure and practices as outlined by [UI Testing Guidelines](./ui-standards.md)
    - At minimum the component should be testing its `Methods`, `Visuals`, `Computed`, and `Props`
  - (_Rare Case_) Any database changes that need to be separated into a single Pull Request (sometimes due to size of data changes) should use [Laravel's Database Assertions](https://laravel.com/docs/master/database-testing)
  - Time is a controlled variable via mocking Date time objects in PHP and Javascript

<br/>

## Pull Request Size is Small to Medium
While there are rare exceptions, each pull request should have minimal amount of lines changed (_soon we'll have specific numbers on size range_). Large pull request pose a risk.

>  the code reviewer loses focus during review and sometimes miss something on the first review and that he/she easily finds on the next one due to the huge amount of files.

Large pull requests can also be discouraging if many Change Requests are made as it can take hours to make the adjustment thereby seeming like the deliverable isn't making incremental progress.


<br/>

### Code Complexity

More on this soon, but for now the approvers are looking to reduce code complexity

<br/>

### Improves or maintains code coverage thresholds
The approver must request changes for Pull Requests that decrease code coverage threshold.

<br/>

## Readability and consistency

While most of the time this should only be done through rules in our Code Style Automation (a.k.a "linters"), there are times that the linters cannot catch readability and consistency 

<br/>

# UI and HTTP API Specifics
The above is just the general standards for all Pull Requests. For the UI and the HTTP API make sure to also follow their respective additional / specific standards.   

- [HTTP API Standards](./api-standards.md)
- [UI Standards](./ui-standards.md)

<br/>

