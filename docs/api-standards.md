All the [General Pull Request Standards](./pull-request-standards.md) apply and the following

# Test Driven Design and Development
We use `composer test` to run our PHP Unit Tests

When developing code, start with your tests. Start with the evidence (test) to your claim (your code) thereby proving to yourself and others that it works and does so by not working first ("red" to "green")

> - Write a test for the next bit of functionality you want to add.
> - Write the functional code until the test passes.
> - Refactor both new and old code to make it well structured.

_quote by [Martin Fowler](https://martinfowler.com/bliki/TestDrivenDevelopment.html)_

![](https://s3.amazonaws.com/codecademy-content/programs/tdd-js/articles/red-green-refactor-tdd.png)
_image by [CodeAcdemy](https://www.codecademy.com/articles/tdd-red-green-refactor)_

A great place to start is with your API Contract testing

<br/>

## Resources
- https://www.thoughtworks.com/insights/blog/test-driven-development-best-thing-has-happened-software-design
- https://www.codecademy.com/articles/tdd-red-green-refactor
- https://medium.com/basic-people/test-driven-development-is-red-green-blue-development-f268e2150981
- http://theidlemonk.github.io/blogs/technical/blog/2015/02/15/t8-tech.html
 

# Migration Scripts
Do **not** modify existing migration scripts `database/migration`. Instead use the [Generate Migration command](https://laravel.com/docs/5.8/migrations#generating-migrations) `make:migration `. In other words, if you need to modify tables create a new migration script using [that command](https://laravel.com/docs/5.8/migrations#generating-migrations)

Migrations that have already ran on servers will not run again hence the reason why you need to create new ones.  

<br/>
 
_It is rare, but if you need to modify a migration script you must explicitly put the reason why in the PR description why you must modify an existing. One of the reasons might be a bug we found in the migration scripts. Again this is **rare**._ 

# Use RESTFul CRUD
Verbs are HTTP Methods not in the url path (see https://laravel.com/docs/8.x/routing and https://laravel.com/docs/8.x/controllers and existing APIs for the Fresh Platform)

# Use JSON API Spec via Laravel Spatie Builder
Make sure to check for existing API endpoints that could easily solve your problem through `filter` or other query params.

We are using https://github.com/spatie/laravel-query-builder/tree/1.17.5

Example
```
 $stores = QueryBuilder::for(Store::class, $request)
            ->allowedIncludes([
                'tags',
                'addresses',
                'events',
                'supplier',
                'supplier.admin'
            ])
            ->allowedSorts([
                'name',
                'status',
                'created_at',
            ])
            ->allowedFilters([
                'name',
                Filter::exact('status'),
                Filter::custom('tag', BelongsToWhereInUuidEquals::class, 'tags'),
                Filter::exact('uuid'),
                Filter::exact('supplier_uuid')
            ]);
```

# Use Actions where possible
Most of the time an Action is the best course of action 😛 

https://github.com/FreshinUp/fresh-bus-forms/blob/master/src/Actions/Action.php and https://medium.com/@remi_collin/keeping-your-laravel-applications-dry-with-single-action-classes-6a950ec54d1d

Examples
https://github.com/FreshinUp/fresh-bus-forms/blob/master/src/Actions/Action.php

Example of usage in a controller method:
```
 public function store(Request $request, CreateDocument $action)
 {
     $user = $request->user();
     $inputs = $request->input();
     $inputs['created_by_uuid'] = $user->uuid;

     $document = $action->execute($inputs);

     return new DocumentResource($document);
 }
```

# URL Resource Pathing
`api/{module}/resource`

Example:
`api/coaching/programs`

# Permission Related Endpoints
We created helpers to be used on permission-related store modules
(https://github.com/FreshinUp/core-ui/blob/master/src/store/utils/permissionsHelpers.js). `enabledFields`, `readonlyFields`, `validationRules` and `labels` should be used as getters on the module.

They rely on a structure that must be used by the API, therefore all permission-related pull requests must follow the structure:

```
'properties' => [
  'status' => [
    'label' => 'Status',
    'rules' => ['required'],
    'readonly' => false
  ],
  'first_name' => [
    'label' => 'First Name',
    'rules' => ['required'],
    'readonly' => false
  ],
  ...
]
```
<br/>

# Migrations naming
In order to avoid migration names collision don't be afraid to be as much as possible descriptive in the name of the migration file.

Example:
If I have to create a table named `activity_modifiers` it will be better to name the file `create_activity_modifiers_table` instead of `create_modifiers_table` when running `php artisan create:migration` command.


# Module Versioning
We follow [Semantic Versioning](https://semver.org/) which means all releases must use this versioning approach

>Given a version number MAJOR.MINOR.PATCH, increment the:
> 
> 1. **MAJOR** version when you make incompatible API changes,
> 2. **MINOR** version when you add functionality in a backwards compatible manner, and
> 3. **PATCH** version when you make backwards compatible bug fixes.
> Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

Pull requests for Modules (e.g. `fresh-inventory`) must update their respective versioning.

- **HTTP API (Laravel)** version is updated using semantic versioning rules (updated in the `composer.json`)

<br/>

**Example**

> ## Version 2.20.1
> - **FIX** Declaration and usage of methods of SearchUsers and FilterUsers Action classes
> ## Version 2.20.0
> - **ADD** `Forgotpassword` page in auth
> - **MODIFY** `Forgotpassword` link to a special page from popup on login modal

### Additional resource
- [Building an api](./building-api.md)
