[![logo](https://i.imgur.com/3OtP3p8.png)](https://softwareengineeringdaily.com/)

# Software Engineering Daily Web Front End

The web client for the Software Daily platform, a Vue.js project. See the Open Source Guide [project overview](https://softwareengineeringdaily.github.io/High_Level/project_description/) for more.

## Getting Started

The [Software Daily](https://www.softwaredaily.com) web front end connects to the Software Daily [REST API](https://github.com/SoftwareEngineeringDaily/software-engineering-daily-api). Here's what you'll need to get started.

### Prerequisites

If you want to get up and running quickly to start development the easiest way is to use the Docker. This requires an OS-specific install of [Docker](https://docs.docker.com/install/) and [Docker Compose](https://docs.docker.com/compose/install/#prerequisites). During the CI process for the the API, the MongoDB image data is seeded from the staging instance. We do not include any users from the staging MongoDB instance. As a result, any forum posts created in the staging environment will not have authors when running locally. This could potentially cause problems when using the data on the front end. If you run into any challenges do not hesitate to ask for help in the Slack channel! 

``` bash
# cloning the project
git clone https://github.com/SoftwareEngineeringDaily/sedaily-front-end.git
cd sedaily-front-end/

# install dependencies
npm install

# run REST API, MongoDB, and Redis in Docker containers (pulls images and runs containers with docker-compose)
npm run backend:start

# serve with hot reload at localhost:8080, using API and event service API running locally
npm run dev
```

## Testing
Currently we have end-to-end testing using [Cypress](). These tests should mimic the user's flow when interacting with the web app. For guidance on how to achieve this review existing tests in `cypress/integration`, the [Cypress guide](https://docs.cypress.io/guides), the [Cypress "kitchen sink" example](https://github.com/cypress-io/cypress-example-kitchensink), and the [Cypress example recipes repo](https://github.com/cypress-io/cypress-example-recipes). Generally you want to add tests for anything you would have concerns around regressions being introduced as new features and bug-fixes are introduced. For further guidance on the what and how of Cypress testing please visit the project Slack channel and ask.

End-to-end tests take a long time to run (currently 1.3 minutes) in comparison to unit tests. While developing your feature you can use the Cypress interactive test-runner to only run a single spec. To do so use `npm run backend:start && npm run dev:test` then in another terminal run `npm run cy:open`. Implementation of a comprehensive set of faster unit-style tests should be considered later using [cypress-vue-unit-test](https://github.com/bahmutov/cypress-vue-unit-test). If done, fewer end-to-end tests would be necessary.

You can run all e2e tests in one command with `npm run test:e2e`. This should only be done prior to committing. It will also be done automatically when merging in Travis CI as a final check.

The database is not reset between each test block, mostly for both performance reasons. Because the `uuid` package is used to generate data, there should generally not be conflicts between tests (posts is tricky as the data is not generated by this app so future tests using posts may run into trouble with conflicting data). If you need to reset data during development run `npm run backend:reload`. This will remove the Docker containers and rebuild from the original images with base data. This is also run as part of the `npm run test:e2e` for a clean start for running the complete suite of tests.

## Contributing
Fork the repository and create a branch off of `master`. When your feature is ready, submit a pull request for the `master` branch. Be sure to include a short description of the feature or pull request and reference any related issues. The project is hosted on Heroku so if you would like a review app created just request it in the PR.

After the Travis-CI tests are successful and your pull request is approved, an admin will merge the PR. Any commits merged to `master` are deployed to the [staging environment](https://sedaily-frontend-staging.herokuapp.com). Once everything looks good an admin will promote staging to production and your feature will be live!

We have an active Slack community that you can reach out to for more information or just to chat with anyone. Check out the [<img src="https://upload.wikimedia.org/wikipedia/commons/7/76/Slack_Icon.png" alt="Slack Channel" width="20px"/> SED app development](https://softwaredaily.slack.com/app_redirect?channel=sed_app_development) slack channel. Also see the [Open Source Guide](https://softwareengineeringdaily.github.io/).
