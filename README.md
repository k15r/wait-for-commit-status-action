> **NOTE:** This is a general template that can be used for a project README.md, example README.md, or any other README.md type in all Kyma repositories in the Kyma organization. Except for the mandatory sections, use only those sections that suit your use case but keep the proposed section order.
>
> Mandatory sections: 
> - `Overview`
> - `Prerequisites`, if there are any requirements regarding hard- or software
> - `Contributing` - do not change this!
> - `Code of Conduct` - do not change this!
> - `Licensing` - do not change this!

# Wait-for-commit-status-action

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
- **`commit_ref`**: The commit reference. Can be a commit SHA, branch name (`heads/BRANCH_NAME`), or tag name (`tags/TAG_NAME`). ([Reference](https://docs.github.com/en/rest/commits/statuses?apiVersion=2022-11-28#get-the-combined-status-for-a-specific-reference))
- **`timeout`**: The time (Milliseconds) to wait for the status to succeed.
- **`check_interval`**: The time (in milliseconds) to wait before checking the commit status again.
  Note: The GitHub REST API uses rate limiting, so a small check interval may exhaust the request limit. ([More info](https://docs.github.com/en/rest/overview/resources-in-the-rest-api?apiVersion=2022-11-28#rate-limiting))

### Required ENV variables

- **`GITHUB_TOKEN`**: Token to authenticate with Github API. e.g. `${{ secrets.GITHUB_TOKEN }}`.
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
