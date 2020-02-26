# GitHub Action for ZEIT

This Action wraps the [Now CLI](https://github.com/zeit/now-cli) to enable common Now commands.

**Update**: this action is obsolete since `now` cli's already installed at the GitHub runners (see [Software installed on GitHub-hosted runners](https://help.github.com/en/actions/reference/software-installed-on-github-hosted-runners)). You can see a new example of usage at [Updated Workflow](#updated-workflow).

## Usage

```workflow
workflow "Deploy on Now" {
  on = "push"
  resolves = ["alias"]
}

action "deploy" {
  uses = "actions/zeit-now@master"
  secrets = [
    "ZEIT_TOKEN",
  ]
}

action "alias" {
  needs = ["deploy"]
  uses = "actions/zeit-now@master"
  args = "alias"
  secrets = [
    "ZEIT_TOKEN",
  ]
}
```

For more examples, visit: [actions/example-zeit-now](https://github.com/actions/example-zeit-now).

### Secrets

* `ZEIT_TOKEN` - **Required**. The token to use for authentication with the ZEIT Now API ([more info](https://zeit.co/blog/introducing-api-tokens-management))

## Updated Workflow

You'll need your `ZEIT_ORG_ID` and `ZEIT_PROJECT_ID` to use the code below (they look similar to `team_WIAtYARApOHgu5xrSOaqdODq` and `dVd5KUENsmLu5aPvZ5nEzJbo6eAvdbyA9WaQFfHmvVE5hr` and can be obtained at `.now/project.json` after running `now` locally). Create a `main.yml` at `.github/worflows` filling the that variables. You'll also have to create a Zeit Token and insert it on your GitHub account.

```sh
name: Deploy on Now
on: [push]
jobs:
  deploy:
    name: Deploy on Now
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      - name: Deploy on Now
        env:
          ZEIT_ORG_ID: 
          ZEIT_PROJECT_ID: 
          ZEIT_TOKEN: ${{ secrets.ZEIT_TOKEN }}
        run:
          now -t $ZEIT_TOKEN
```

## License

The Dockerfile and associated scripts and documentation in this project are released under the [MIT License](LICENSE).

Container images built with this project include third party materials. See [THIRD_PARTY_NOTICE.md](THIRD_PARTY_NOTICE.md) for details.
