All the [General Pull Request Standards](./pull-request-standards.md) apply and the following 

# Test Driven Design and Development
We use `yarn test` to run our Jest Tests

When developing code, start with your tests. Start with the evidence (test) to your claim (your code) thereby proving to yourself and others that it works and does so by not working first ("red" to "green")

> - Write a test for the next bit of functionality you want to add.
> - Write the functional code until the test passes.
> - Refactor both new and old code to make it well structured.

_quote by [Martin Fowler](https://martinfowler.com/bliki/TestDrivenDevelopment.html)_

![](https://s3.amazonaws.com/codecademy-content/programs/tdd-js/articles/red-green-refactor-tdd.png)
_image by [CodeAcdemy](https://www.codecademy.com/articles/tdd-red-green-refactor)_

<br/>

## Resources
- https://www.thoughtworks.com/insights/blog/test-driven-development-best-thing-has-happened-software-design
- https://www.codecademy.com/articles/tdd-red-green-refactor
- https://medium.com/basic-people/test-driven-development-is-red-green-blue-development-f268e2150981
- http://theidlemonk.github.io/blogs/technical/blog/2015/02/15/t8-tech.html

# Testing Anatomy
When testing a UI Component provide,  the following suites `describes` are what Pull Request Reviewers are looking for, can help keep code coverage high, and provide evidence for a `Test Driven Design and Development` approach.

* name of component under test
  * Visuals (Snapshots) - `mount`
  * Computed - `shallowMount`
  * Methods - `shallowMount`

One thing to watch out for is Code Complexity that is not reflected in the tests. While we don't do any Nth-Complexity testing branch coverage is done, so look for testing conditionals. In fact, when developing conditionals it is often something that should be separated into multiple functions and help your coverage.

## Structure for Component 
```javascript
describe(`${BUS_CATEGORY}| Edit User List`, () => {
   describe('Visuals', () => {
      test('default', () => {
         const wrapper = mount(Component, {}) // Full mount for snapshots
         expect(wrapper.element).toMatchSnapshot() 
      })
   })
   describe('Computed', () => {
      test('default prop of propertyA is XYZ', () => {
        const wrapper = shallowMount(Component, {})
        expect(wrapper.vm.propertyA).toEqual('XYZ') 
      }) 
   })
   describe('Methods', () => {
      test('getFriends returns "the best"', () => { // insert jokes
        const wrapper = shallowMount(Component, {})
        expect(wrapper.vm.getFriends()).toEqual('the best') 
      }) 
   })
}
```

## Smart Component / Page Testing
When invoking the main method of the page call `beforeRouteEnterOrUpdate` on the Page class and use a double (mock) for the HTTP API response handling. Also you the `mount` for Smart Component (Page) testing

```javascript
import { mount } from '@vue/test-utils'
import { createLocalVue } from 'fresh-bus/tests/utils'
import createStore from '~/store'
import Page from '~/pages/admin/searches/index.vue'
let vue, localVue, mock
    beforeEach(() => {
      vue = createLocalVue()
      localVue = vue.localVue
      mock = vue.mock

      mock
        .onGet('api/inventory/v1/deposit-statuses').reply(200, {
          data: depositStatuses
        })
        .onGet('api/inventory/v1/makes').reply(200, {
          data: makes
        })
        .onAny()
        .reply(200, { data: {} })
    })
    afterEach(() => {
      mock.restore()
    })

    test('uses query params except title to filter vehicles API', (done) => {
      const query = {
        model_name: 'F770B',
        min_year: '2016',
        title: 'TestTitle'
      }

      const $route = { query }

      const store = createStore({})

      const wrapper = mount(Page, {
        localVue,
        store,
        stubs: {
          SaveSearch: true
        },
        mocks: {
          $route
        }
      })

      Page.beforeRouteEnterOrUpdate(wrapper.vm, null, null, async () => {
        mock.history.get.forEach(call => {
          if (call.url === 'api/csm/vehicles') {
            expect(call.params['filter[model_name]']).toEqual('F770B')
            expect(call.params['filter[min_year]']).toEqual('2016')
            expect(call.params['filter[title]']).toBeUndefined()
          }
        })
        done()
      })
    })
```


<br/>


# Test Doubles
Remember there are several kinds of Test Doubles

> The generic term he uses is a Test Double (think stunt double). Test Double is a generic term for any case where you replace a production object for testing purposes. There are various kinds of double that Gerard lists:
>
> - **Dummy** objects are passed around but never actually used. Usually they are just used to fill parameter lists.
> - **Fake** objects actually have working implementations, but usually take some shortcut which makes them not suitable for production (an InMemoryTestDatabase is a good example).
> - **Stubs** provide canned answers to calls made during the test, usually not responding at all to anything outside what's programmed in for the test.
> - **Spies** are stubs that also record some information based on how they were called. One form of this might be an email service that records how many messages it was sent.
> - **Mocks** are pre-programmed with expectations which form a specification of the calls they are expected to receive. They can throw an exception if they receive a call they don't expect and are checked during verification to ensure they got all the calls they were expecting.
_quote by [Martin Fowler](https://martinfowler.com/bliki/TestDouble.html)_

For an in-depth look please read [Mocks Aren't Stubs](https://martinfowler.com/articles/mocksArentStubs.html)

<br/>

# Testing Tools
We use the following
- [Vue Test Utils](https://vue-test-utils.vuejs.org/guides/#getting-started)
- [FreshPlatform UI Testing Utils](https://github.com/FreshinUp/fresh-bus-forms/blob/master/tests/Javascript/utils.js)

# Atomic Design is followed

UI is built using components from atoms to molecules to organisms. Pages should not do things like `<div class="display-2">Title</div>` rather it should be something like `<f-title>Title</f-title>` Use the [component library](https://fps.freshinup.com/styleguide/)

<br/>

# Buttons that do nothing

need to have some sort of indicator like "coming soon" on click otherwise assume it is a bug and put a Change Request on the PR. Additional to this, buttons within component stories need to have an action associated to them. Example: With the [FBtnStatus.vue](https://fps.freshinup.com/styleguide/?path=/story/core-2-16-0-button-status--v-model) when the status is changed, actions are associated to that interaction.

<br/>

# CSS Class naming
In general avoid CSS code. Use our component library and Vuetify properties and class helpers (RTFM). 

When you must name then follow https://github.com/FreshinUp/fresh-platform/wiki/Web-Components-(CSS-Naming,-Concepts,-Fonts)

# Stories
Each Component _should_ have a Story for each of its states/modes and properties. Remember you're showing to other engineers and designers the possibilities and use cases of your component.

<br/>

# State Machine (Vuex)
Components have no knowledge of the HTTP API ("backend"). Never. 

Components are either
- Dumb Components. Only receive properties and dispatch events. They do not interact with the Vuex Store.
- Smart Components (aka Pages). These are aware and interact with the Vuex Store.

Request changes for any non-page component interacting with the Store.

# Imports for the UI Module
When you're importing within a UI "module" (e.g. https://github.com/FreshinUp/deals-ui) use explicit package or relative. The reason is that the consuming projects use a different webpack.

**Wrong**
From [Deals UI](https://github.com/FreshinUp/deals-ui/pull/33/files#diff-dc10812d58a4cea1b1212ee74e6fe5bfR214)
```
import ClearButton from '~/components/ClearButton.vue'
import FilterLabel from '~/components/FilterLabel'
import MultiSimple from '@freshinup/core-ui/src/components/FMultiSimple'
import MultiSelect from '@freshinup/core-ui/src/components/FMultiSelect'
```

**Correct**
```
import ClearButton from './ClearButton.vue'
import FilterLabel from './FilterLabel'
import MultiSimple from '@freshinup/core-ui/src/components/FMultiSimple'
import MultiSelect from '@freshinup/core-ui/src/components/FMultiSelect'
```

**Also Correct**
```
import ClearButton from '@freshinup/deals-ui/src/components/ClearButton.vue'
import FilterLabel from '@freshinup/deals-ui/src/components/FilterLabel'
import MultiSimple from '@freshinup/core-ui/src/components/FMultiSimple'
import MultiSelect from '@freshinup/core-ui/src/components/FMultiSelect'
```

# Module Versioning
We follow [Semantic Versioning](https://semver.org/) which means all releases must use this versioning approach

>Given a version number MAJOR.MINOR.PATCH, increment the:
> 
> 1. **MAJOR** version when you make incompatible API changes ("Breaking Change")
> 2. **MINOR** version when you add functionality in a backwards compatible manner, and
> 3. **PATCH** version when you make backwards compatible bug fixes.
> Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

Pull requests for Modules (e.g. `fresh-inventory`) must update their respective versioning.

- **HTTP API (Laravel)** version is updated using semantic versioning rules (updated in the `composer.json`)
- **UI (Node)** version is updated using semantic versioning rules (updated in the `package.json`)

Add to the `Changelog.md` with description of what that version is doing with titles like:
- **Added** - Used when adding features or components. Please write a brief description of their purpose
- **Fix** - Used when fixing a bug. Describe the problem that existed and how the PR fixes it
- **Modify** - Used for small changes to existing features or components

<br/>

## Changlog Formatting
To make the changelogs read faster and understand what's being worked on, we're using this structuring.

>## Version (number)
>## (whats being worked on ie "component", "state machine" "page")
>- (add, fix, modify etc.) "description goes here"

## Examples from Core UI


>## Version 1.44.0
># Mixin
>`FieldMeta`
>- **MOD** `readOnlyFields` to `readonlyFields` to be consistent with vuetify text-fields and other inputs >https://v15.vuetifyjs.com/en/components/text-fields/


>## Version 1.50.0
>## Components
>FInfoTooltip`
>- **ADD** `color` as props to allow customization

>`FTextField`
>`FInfoInput`
>- **MOD** prop `info` to accept object { label: <tooltip>, color: <color> }
