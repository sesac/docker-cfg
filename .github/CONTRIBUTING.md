# Contributing

## Branch Structure

We follow a git workflow similar to the one [described here](http://nvie.com/posts/a-successful-git-branching-model/).

We have three protected branches in this repository, `master`. `qa`, and `staging`.
Most pull requests (PRs) will be submitted to `staging`, reviewed, and merged into `staging`.
PRs merged into staging will be run through CI/CD to the staging environment where the dev team
can review that they have worked.

Once it has been determined that a set of new commits and features warrant testing
outside of the development team, a PR will be submitted from `staging` to `qa`, reviewed, and merged.
CI/CD will then deploy these changes to the QA environment for review by QA.

Once it has been determined by the dev team and QA that a set of features since the previous release
are stable and constitute a new release, a PR will be submitted from `qa` to `master` for review.
Once reviewed and merged, CI/CD will deliver the changes to production.

By following this pattern, QA will be able to review new changes without worrying about features moving out from
beneath their feed with the more rapid pace of development in the staging environment. We can also have `master`
remain stable while deployed into production, all while new features/versions can be developed on `staging`.

A simple example of a feature -> release workflow is as follows:

``` console
git clone git@github.com:rumblefishinc/sesac-api.git
cd sesac-api
git fetch
git checkout staging
git pull origin staging
git checkout -b new-feature
# Develop and commit new feature
git push -u origin new-feature

# Submit PR from new-feature to staging branch
# Team reviews PR
# PR is merged

# New staging release is deployed to staging (Capistrano integration with Codeship)

# Team submits PR from staging to qa
# Team reviews PR
# PR is merged

# New qa release is deployed to qa (Capistrano integration with Codeship)

# New release time!
# Team submits PR from qa to master
# Team reviews PR
# PR is merged

# New master release is deployed to production (Capistrano integration with Codeship)!
```

## PRs

Please submit your PRs using the following format:

>### Why is this pull request necessary?
> The API requires a new endpoint: `/v1/resources/heavy/metal`
>
>### How does it fix the issue?
> Adds the new edpoint to `SesacAPI::V1`, as well as tests for the new endpoint.
> Before, `/v1/resources/heavy` returned a response of '{"types":["metal","doors","water"]}'; now,
> `/vi/resources/heavy` will not return a response.
>
> Please note, endpoints for `heavy/doors` and `heavy/water` are being developed in separate branches.
>
>### What side effects might this have?
> Any client that depended on the response from `/v1/resources/heavy` will break.

In GitHub, you can create a saved reply using the following template to make your life easier:

``` markdown
### Why is this pull request necessary?


### How does it fix the issue?


### What side effects might this have?
```

As much as possible, please limit PRs to a single logical set of changes. For example, the developer
above submitted a PR for creating one new endpoint, `/v1/resources/heavy/metal`, instead of one PR
for all three new endpoints that needed to be created, `metal`, `doors`, and `water`. While this is
a contrived example, following this practice makes it easier on your fellow developers to review your
code and the changes contained therein.
