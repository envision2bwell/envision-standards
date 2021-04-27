> This project is for documenting the Envision2BWell, some good practices and is seen as the general starting place for working on the Envision2BWell

## Modules
Our modules for the **FreshPlatform** are growing and improving in scope and size (both reduction, separation, and increase). The following are a list of the current scope.
Modular Based System focused on composability allowing us to:
* Quickly build out solutions for our clients.
* Providing custom experiences of the FreshPlatform features.
* Maintainability that can be shared allowing us to make patches and easily update clients with bug fixes and new features.

## Laravel specific 
Don't use fields such like `lastModifiedDate` or `DateCreated`. Laravel has `created_at` and `updated_at` out of the box. Set model property timestamps = false when not using them

## Mobile

## REST API
1. Don’t return plain text
2. Use plural rather than singular
3. Avoid using verbs in URIs
4. Always return a meaningful status code
5. Use trailing slashes consistently


## Standards
- Config and environment variable replace static information or credentials. Credentials should never be part of the code.
- Table and column names should use snake case (ie `envision_post_categories`). Table name should be in plural unlike the column name in singular.

## [Assessments](./assessments.md)

## Additional ressources
- [Tech scoping](./tech-scoping.md)
- [Pull request standards](./pull-request-standards.md)
- [API standards](./api-standards.md)
- [UI standards](./ui-standards.md)
- [Learning from old code](./learnings.md)



