# Download workflow artifact GitHub Action

An action that downloads and extracts uploaded artifact associated with given workflow and commit or other criteria.

Let's suppose you have a workflow with a job in it that at the end uploads an artifact using `actions/upload-artifact` action and you want to download this artifact in another workflow that is run after the first one. Official `actions/download-artifact` does not allow this. That's why I decided to create this action. By knowing only the workflow name and commit SHA, you can download the previously uploaded artifact from different workflow associated with that commit and use it.

## Usage

> If `commit` or `pr` or `branch` or `run_id` or `workflow_conclusion` is not specified then the artifact from the most recent successfully completed workflow run will be downloaded.

**Do not specify `pr`, `commit`, `branch`, `run_id` together or `workflow_conclusion` and `run_id` together. Pick just one of each or none.**

```yaml
- name: Download artifact
  uses: dawidd6/action-download-artifact@v2
  with:
    # Optional, GitHub token, a Personal Access Token with `public_repo` scope if needed
    # Required, if artifact is from a different repo
    # Required, if repo is private a Personal Access Token with `repo` scope is needed
    github_token: ${{secrets.GITHUB_TOKEN}}
    # Required, workflow file name or ID
    workflow: workflow_name.yml
    # Optional, the status or conclusion of a completed workflow to search for
    # Can be one of a workflow conclusion:
    #   "failure", "success", "neutral", "cancelled", "skipped", "timed_out", "action_required"
    # Or a workflow status:
    #   "completed", "in_progress", "queued"
    workflow_conclusion: success
    # Optional, will get head commit SHA
    pr: ${{github.event.pull_request.number}}
    # Optional, no need to specify if PR is
    commit: ${{github.event.pull_request.head.sha}}
    # Optional, will use the branch
    branch: master
    # Optional, defaults to all types
    event: push
    # Optional, will use specified workflow run
    run_id: 1122334455
    # Optional, run number from the workflow
    run_number: 34
    # Optional, uploaded artifact name,
    # will download all artifacts if not specified
    # and extract them in respective subdirectories
    # https://github.com/actions/download-artifact#download-all-artifacts
    name: artifact_name
    # Optional, directory where to extract artifact(s), defaults to current directory
    path: extract_here
    # Optional, defaults to current repo
    repo: ${{github.repository}}
    # Optional, check the workflow run whether it has an artifact
    # then will get the last available artifact from previous workflow
    # default false, just try to download from the last one
    check_artifacts:  false
    # Optional, search for the last workflow run whose stored an artifact named as in `name` input
    # default false
    search_artifacts: false
```
