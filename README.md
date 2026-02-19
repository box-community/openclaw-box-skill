# Box Skill for OpenClaw

> **Developer Preview** — This skill is experimental and intended for
> developer evaluation only. Do not use it in production environments.
> It is provided "as-is" without warranty of any kind. Box does not
> offer support for this integration.

A Box CLI integration for OpenClaw that enables authenticated file
operations and Box AI workflows directly from an OpenClaw agent.

This skill is built for developers who have their own OpenClaw instance
and want to extend it with Box capabilities. It is not a daily
productivity tool — it is a building block for developer workflows and
automation.

Designed for headless environments (Railway, CI, servers), this skill
uses **Client Credentials Grant (CCG)** or **JWT server
authentication**. Credentials must be supplied by the user — the skill
does not provision or manage secrets.

------------------------------------------------------------------------

## Important: Use a Dedicated Box Developer Account

This skill authenticates as a **service account**. All file operations
occur within the service account's content — not your personal Box
account. To see what the service account owns, go to the
[Box Admin Console](https://app.box.com/master) and navigate to **Content**.

Because this is a developer preview, you should **run it on a separate
Box instance** rather than your organization's primary account. You can do so by using a
**[free Box Developer account](http://account.box.com/signup/developer)**

------------------------------------------------------------------------

## Capabilities

-   Browse, upload, download, and search Box files
-   Manage metadata
-   Execute any supported `box` CLI command
-   Run Box AI operations (QA, extract, structured extraction)

AI operations are executed via [Box AI APIs](https://developer.box.com/ai-dev-zone).

------------------------------------------------------------------------

## Installation

1.  Copy this skill into your OpenClaw `skills/` directory.
2.  Provide your own Box [CCG](https://developer.box.com/guides/authentication/client-credentials) or JWT configuration file. Place it at:

```
/data/.secrets/box-ccg.json
```

3.  Ask OpenClaw to install the [Box CLI](https://github.com/box/boxcli) (if not already installed). For example:

``` bash
npm install -g @box/cli
```

4.  Ask OpenClaw to register the Box environment and verify authentication. For example:

``` bash
box configure:environments:add /data/.secrets/box-ccg.json --ccg-auth --name ccg --set-as-current
box users:get me
```

After this one-time setup, OpenClaw can invoke Box CLI commands and Box AI workflows directly.

------------------------------------------------------------------------

## CCG Configuration Template

[Create a Box application](https://developer.box.com/guides/getting-started/first-application) using **Server Authentication (Client Credentials Grant)**. Note your app's **client ID**, **client secret**, and **enterprise ID**.

Then [authorize the application](https://developer.box.com/guides/authorization/) in the Box Admin Console. Your app cannot make API calls until an admin has approved it.

Then create:

```
/data/.secrets/box-ccg.json
```

With:

``` json
{
  "boxAppSettings": {
    "clientID": "YOUR_CLIENT_ID",
    "clientSecret": "YOUR_CLIENT_SECRET"
  },
  "enterpriseID": "YOUR_ENTERPRISE_ID"
}
```

Secure the file:

``` bash
chmod 600 /data/.secrets/box-ccg.json
```

Do not commit credential files to version control.

------------------------------------------------------------------------

## Documentation

-   OpenClaw: https://docs.openclaw.ai/
-   OpenClaw Skills: https://docs.openclaw.ai/skills/
-   Box CLI: https://developer.box.com/guides/cli/
-   Box AI: https://developer.box.com/guides/box-ai/
