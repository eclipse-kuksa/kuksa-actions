# Reusable Workflows

## Checking if ghcr push shall happen

The workflow `check_ghcr_push.yml` can be used to check if a push of an image to `ghcr.io` shall happen or not.
It is based on two assumptions:

* We shall only push new images in two scenarios
     * When we have pushed something to a branch.
       This includes merge of pull requests.
     * When we tag a commit
* We shall only try to push to `ghcr.io` if we are working on an "official" KUKSA repository.
  If running this workflow for a fork it shall return false, as the fork does not have credentials to
  push to "official" KUKSA `ghcr.io` locations

In general KUKSA build scripts


tries to analyze if the run has sufficient credentials to be able
to push docker containers to [Github pakages](https://github.com/features/packages) (ghcr.io).
The analysis is performed by checking the context of the build, and if certain conditions are fulfilled it is assumed
that credentials are present.

The workflow can be used from other workflows like below.
The example specifies that latest commit on main branch shall be used.
Alternatively tags can be used to specify exactly which version of this workflow to use, like `check_ghcr_push.yml@3`

```
jobs:
  check_ghcr_push:
    uses: eclipse-kuksa/kuksa-actions/.github/workflows/check_ghcr_push.yml@main
    secrets: inherit
    
  build-something:
  
    name: "Build something"
    runs-on: self-hosted
    needs: check_ghcr_push

    steps:
    - name: Log in to the Container registry
      if: needs.check_ghcr_push.outputs.push == 'true'
      uses: docker/login-action@v2
      with:
          registry: ghcr.io
          username: ${{ github.repository_owner }}
          password: ${{ secrets.GITHUB_TOKEN }}
```