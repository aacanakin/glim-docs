This section includes information about how to contribute to the project, versioning rules and project development workflow.
[block:api-header]
{
  "type": "basic",
  "title": "Versioning"
}
[/block]
Currently, glim is not using any versioning standards before 1.x releases. After the 1.0 release, the project will use [semantic versioning](http://semver.org/). Currently, the versioning is used only  to seperate milestones of development. The minor version numbers increase only in bug fixes. You can check the [Roadmap & Future Releases](doc:roadmap) section for future releases and its versions.
[block:api-header]
{
  "type": "basic",
  "title": "Development Workflow"
}
[/block]
We are using github for the issue tracking. All the issues are consolidated there. The workflow of development will be the following;

- User creates a github issue.
- If this is a bug, the issue will be replicated by admin and approved, else it will be discussed and approved.
- Developer creates a local branch if this is a minor issue, else creates a remote branch.
- Developer provides a pull request.
- Admin merges from feature branch to dev branch and tests the feature requests locally.
- Admin merges from dev branch to master branch

Currently, it is unfortunate that all the actors mentioned above is 1 person. Therefore, it is needed more and more contributors.
[block:callout]
{
  "type": "info",
  "title": "Please provide tests after solving issues",
  "body": "After the 0.11 releases of glim, writing unit tests will be an obligation."
}
[/block]