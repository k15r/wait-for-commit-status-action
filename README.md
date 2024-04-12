# Wait-for-commit-status-action

[![REUSE status](https://api.reuse.software/badge/github.com/kyma-project/wait-for-commit-status-action)](https://api.reuse.software/info/github.com/kyma-project/wait-for-commit-status-action)

## Overview
This action waits for a commit status to succeed, and it also returns the JSON of the commit status. It uses the Github API [Get the combined status for a specific reference](https://docs.github.com/en/rest/commits/statuses?apiVersion=2022-11-28#get-the-combined-status-for-a-specific-reference) to fetch the commit statuses and then checks the status of the specified status `context`.

## Prerequisites

- [Python](https://www.python.org/)
- [Docker](https://www.docker.com/)

## Usage

### Example
```
jobs:
  example_case:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run the action
        id: test_action
        uses: kyma-project/wait-for-commit-status-action@main
        with:
          context: "<CONTEXT_NAME>"
          commit_ref: "<COMMIT_REF | SHA>"
          timeout: <TIME_IN_MILLISECONDS>
          check_interval: <TIME_IN_MILLISECONDS>
        env:
          GITHUB_TOKEN: "${{ secrets.GITHUB_TOKEN }}"
          GITHUB_OWNER: "<OWNER>"
          GITHUB_REPO: "<REPO>"
      - name: Use the results
        run: |
          echo "${{ steps.test_action.outputs.state }}"
          echo "${{ steps.test_action.outputs.json }}"
```

### Required Inputs

- **`context`**: The context value of the status.
- **`commit_ref`**: The commit [reference](https://docs.github.com/en/rest/commits/statuses?apiVersion=2022-11-28#get-the-combined-status-for-a-specific-reference). Can be a commit SHA, branch name (`heads/BRANCH_NAME`), or tag name (`tags/TAG_NAME`).
- **`timeout`**: The time (Milliseconds) to wait for the status to succeed.
- **`check_interval`**: The time (in milliseconds) to wait before checking the commit status again.
  > **NOTE:** The GitHub REST API uses [rate limiting](https://docs.github.com/en/rest/overview/resources-in-the-rest-api?apiVersion=2022-11-28#rate-limiting), so a small check interval may exhaust the request limit.

### Required ENV variables

- **`GITHUB_TOKEN`**: Token to authenticate with Github API, such as `${{ secrets.GITHUB_TOKEN }}`.
- **`GITHUB_OWNER`**: The account owner of the repository. The name is not case sensitive.
- **`GITHUB_REPO`**: The name of the repository without the .git extension. The name is not case sensitive.

### Outputs

- **`state`**: The state value of the retrieved status.
- **`json`**: The stringified JSON of the retrieved status object.

## Contributing
<!--- mandatory section - do not change this! --->

See [CONTRIBUTING.md](CONTRIBUTING.md)

## Code of Conduct
<!--- mandatory section - do not change this! --->

See [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)

## Licensing
<!--- mandatory section - do not change this! --->

See the [LICENSE file](./LICENSE)
