# GET-PROPERTIES-FROM-JSON-GH-ACTION

<!-- vscode-markdown-toc -->
* [code-of-conduct](#code-of-conduct)
	* [github-action](#github-action)

<!-- vscode-markdown-toc-config
	numbering=false
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc --># get-properties-from-json-gh-action

## <a name='code-of-conduct'></a>code-of-conduct

Go crazy on the pull requests :) ! The only requirements are:

> - Use [conventional-commits](#check-conventional-commits).
> - Include [jira-tickets](#check-jira-tickets-commits) in your commits.
> - Create/Update the documentation of the use case you are creating, improving or fixing. **[Boy scout](https://biratkirat.medium.com/step-8-the-boy-scout-rule-robert-c-martin-uncle-bob-9ac839778385) rules apply**. That means, for example, if you fix an already existing workflow, please include the necessary documentation to help everybody. The rule of thumb is: _leave the place (just a little bit)better than when you came_.

### <a name='github-action'></a>github-action

This actions will parse input json file based on input parameter *json-file-path* and search for element matching input parameter *account-name* and exposes following outputs:

- environment
- account_id
- account_name
- cfn_main_file
- stack_name
- capabilities

expected json file format is following:

```json
{
    "cfn_main_file": "main.yaml",
    "stack_name": "awesome_stack",
    "capabilities": "CAPABILITY_AUTO_EXPAND,CAPABILITY_NAMED_IAM",
    "accounts": [
        {
            "environment": "acc",
            "account_name": "client-acc",
            "account_id": "012345678900"
        },
        {
            "environment": "acc",
            "account_name": "internal-acc",
            "account_id": "012345678901"
        },
        {
            "environment": "acc",
            "account_name": "external-acc",
            "account_id": "012345678902"
        }
    ]
}
```

example usage:

```yaml
name: CI
on:
  pull_request:
    branches: ["main"]
jobs:
    build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Get Properties From Json
        id: get-properties
        uses: ohpensource/platform-cicd/actions/aws/cloudformation/get-properties-from-json@2.15.0.x
        with:
          account-name: "account-name-you-need"
          json-file-path: ./params/matrix.json
      - name: output usage
        run: |
          echo ${{steps.get-properties.outputs.account_id}}
          echo ${{steps.get-properties.outputs.account_name}}
          echo ${{steps.get-properties.outputs.environment}}
          echo ${{steps.get-properties.outputs.cfn_main_file}}
          echo ${{steps.get-properties.outputs.stack_name}}
          echo ${{steps.get-properties.outputs.capabilities}}
```