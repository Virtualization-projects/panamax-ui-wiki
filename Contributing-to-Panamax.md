Panamax is comprised of a few git repos: [panamax-ui](https://github.com/CenturyLinkLabs/panamax-ui), [panamax-api](https://github.com/CenturyLinkLabs/panamax-api), and [panamax-coreos](https://github.com/CenturyLinkLabs/panamax-coreos). 
Panamax templates can be stored in a git repo as well, the one that comes pre-installed in panamax is [panamax-public-templates](https://github.com/CenturyLinkLabs/panamax-public-template), which acts as the canonical template source.

## Reporting Issues

Github Issues is a good place to track issues. However, it can be confusing to determine which repo an error occurred in. Installation issues should be logged in [panamax-coreos](https://github.com/CenturyLinkLabs/panamax-coreos/issues). Once Panamax is up and running it's a little more difficult to pinpoint which codebase contains the issue. Most folks start at [panamax-ui](https://github.com/CenturyLinkLabs/panamax-ui/issues), but issues can also be logged in [panamax-api](https://github.com/CenturyLinkLabs/panamax-api/issues), which is where the CoreOS interactions take place.

*Please make sure the issue does not already exist in one of the above 3 repositories*

## Feature Requests

Have a useful idea for a Panamax feature? Please use the [customer feedback](http://feedback.panamax.io/) site to comment on or request a feature.

## Wiki Contributions

We have made our wiki editable by the community. If questioning weather or not something should go in the wiki, consult one of our support channels.

## Code Contributions

### The contribution cycle

1. Fork the repository and clone a local copy.
2. Refer to the README to get the application set up and running locally.
3. Make the desired changes, make sure all new code is tested, and all tests are passing.
4. Issue a pull request.
5. Panamax Core team members will review the request and give feedback. Once two +1s have been given (by core team members) the PR will be merged.

Each commit should be a logical unit of work. Don't try to boil the ocean in a single commit, but do make sure that all tests are passing with each commit. When contributing new code, tests should be included in the same commit as the code being tested.

### Ruby code style guide

To ease the reading and ongoing maintenance of the code base we try hard to follow the [Ruby style guide](
https://github.com/bbatsov/ruby-style-guide) conventions (albeit with [a few tweaks](https://github.com/CenturyLinkLabs/panamax-api/blob/master/.hound.yml)). If you have [Rubocop](https://github.com/bbatsov/rubocop) installed locally you can use it to check syntax before commiting/pushing.


### Testing

Testing is something that we're pretty passionate about on the Panamax team and we try hard to maintain good coverage across all of our Ruby and JavaScript code. The sections below discuss some of the specific testing tools we're using across the various projects.

#### Testing Panamax UI

For instructions on running tests in the Panamax UI project see the [README](https://github.com/CenturyLinkLabs/panamax-ui#running-tests-etc).

##### Dependencies used for testing purposes:
* [Rspec](http://rspec.info/) - all our ruby tests are written in Rspec
* [Simplecov](https://github.com/colszowka/simplecov) - for code coverage metrics
* [Webmock](https://github.com/bblimke/webmock) - to stub http requests
* [Sinatra](http://www.sinatrarb.com) - for the mock panamax api
* [Jasmine v1.3](http://jasmine.github.io/1.3/introduction.html) - for JS testing
* [Teaspoon JS test runner](https://github.com/modeset/teaspoon) - to run the JS tests

##### Panamax UI has the following types of tests:
* Ruby unit tests for the Rails models, controllers, helpers, presenters, etc.
* Ruby integration tests (spec/features) - we keep these pretty light weight and simple. Currently we do not test any javascript behavior with the integration tests. Integration tests do not hit a real panamax api, there is a fake panamax sinatra app that returns stubbed responses.
* Jasmine JS tests (spec/javascripts)

#### Testing Panamax API

For instructions on running tests in the Panamax API project see the [README](https://github.com/CenturyLinkLabs/panamax-api/blob/master/README.md#run-the-automated-tests).

##### Dependencies used for testing purposes:
* [Rspec](http://rspec.info/) - all our ruby tests are written in Rspec
* [Simplecov](https://github.com/colszowka/simplecov) - for code coverage metrics
* [Shoulda Matchers](https://github.com/thoughtbot/shoulda) - for the mundane active record specs
* [Database Cleaner](https://github.com/DatabaseCleaner/database_cleaner)

##### Panamax API has the following types of tests:
* Ruby unit tests for the Rails models, controllers, Concerns, Plain ole Ruby Objects, etc.
* Routing specs - since this is an API we have routing specs instead of integration tests


#### Testing isn't your cup of tea or you're struggling to write appropriate tests for something?

Have a valuable PR, but struggling with the tests? Go ahead and open it, and ask for help. We are more than happy to help, especially if the PR is something like a valuable bug fix.
