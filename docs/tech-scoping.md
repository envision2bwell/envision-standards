# Deliverable Technical Scoping  "Tech How"

A Deliverable is comprised of the following
- **What** which is effectively the scope including Visuals and Business rules
- **Acceptance Criteria** What it takes to confirm the What / Scope
- **How** This is the Technical Direction and Design on how the Deliverable is executed. This includes which components and API foundation to use, which, if any, packages to rely on, which repositories to put the code in, and any standards to adhere to.

## Approach
Deliverables need to be deconstructed into Github issues asking "What Components" do we need or do we have to build this _Deliverable_? 

- [Atomic Design](http://atomicdesign.bradfrost.com/table-of-contents/)
- [Component Driven Design](https://whatjackhasmade.co.uk/component-driven-development)
- [Component Driven Design](https://itnext.io/a-guide-to-component-driven-development-cdd-1516f65d8b55)
- [Component Driven Design](https://medium.com/the-s-curve/why-component-driven-design-drives-great-software-products-7cace364e815)

### For the UI
1. [FPS](https://fps.freshinup.com/styleguide)
2. [Vuetify version 1](https://v15.vuetifyjs.com/en/) _We want to upgrade... :)_

### For the API
1. Laravel Spatie Query Builder used for to implement JSON API spec (e.g. filters, includes, etc)
2. 


## Tags
A Deliverable should have at minimum the `Deliverable` tag 

## Github Issues in Todo List
Each Deliverable ticket (Breeze) needs to have corresponding Github Issues that are marked in the ToDo List Section separated by each repository. The order should be listed by modules first (if needed) then the Client project since most of the time the Client project ticket(s) should just describe configuration.

## In the Github Issues
Each Github issue is created in the respective Repository of where the code should be written.

Issues for UI should call out specific Components that are to be used and any blockers a typical format should be available in your Repository

Issues for API should specify any thing that can be leveraged (e.g. any existing `Actions` or `Enums` or `Resources`)


