# Bitbucket Pipelines CI demo through REST API

This is a basic demonstration of what is needed to run a Bitbucket Pipelines CI build by calling their REST API.

We are using Figmagic for this demo, to demonstrate running it in CI. Even if you don't care for that specific tool, you'll get the gist how to automate stuff in CI here.

## Instructions

I am assuming that you will set up a repo in Bitbucket with the provided pipeline. If you have not, do so as the first step.

Set up Repository Variables as per:

- `FIGMA_TOKEN` as your Figma API token
- `FIGMA_URL` to point to your Figma file ID

Set both to be secret values.

### Authentication

The ideal use seems to be to use an application password, instead of any personal credentials.

To do this, first [create an app password](https://bitbucket.org/account/settings/app-passwords/). You will need the token (password) and your Bitbucket username ([you can see it in the Account Settings page](https://bitbucket.org/account/settings/); it'snot your email!) for authenticating.

## Example call to API

You will do a POST request. Set the following header:

`Content-Type`: `application/json`.

For authentication, in case you use a client like Insomnia, just use the built-in functionality to use Basic auth and set username and password there. For a `curl` solution to this, see the reference links.

```json
POST https://api.bitbucket.org/2.0/repositories/YOUR_WORKSPACE_OR_USER/YOUR_REPO/pipelines/#post

{
  "target": {
    "ref_type": "branch",
    "type": "pipeline_ref_target",
    "ref_name": "main"
  }
}
```

`ref_name` should match your branch name.

### Passing variables

This is same as above, but you should also pass `variables` in your payload. Use whatever you need for `key` and `value`. Using `secure` will attempt to mask the value in logs.

```json
POST https://api.bitbucket.org/2.0/repositories/YOUR_WORKSPACE_OR_USER/YOUR_REPO/pipelines/#post

{
  "target": {
    "ref_type": "branch",
    "type": "pipeline_ref_target",
    "ref_name": "main"
  },
  "variables": [{
    "key": "FIGMA_VERSION",
    "value": "something-here-alright",
    "secured": false
  },
  {
    "key": "FIGMA_MESSAGE",
    "value": "This is what happened",
    "secured": false
  }]
}
```

## References

- [Bitbucket API: Trigger a pipeline for a branch](https://developer.atlassian.com/bitbucket/api/2/reference/resource/repositories/%7Bworkspace%7D/%7Brepo_slug%7D/pipelines/#post)
- [Bitbucket API: Authentication methods](https://developer.atlassian.com/bitbucket/api/2/reference/meta/authentication)
