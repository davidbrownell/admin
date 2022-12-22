# admin
:robot: `admin` repo for [safe-settings](https://github.com/github/safe-settings).

Settings for all repositories under https://github.com/davidbrownell with a `v4-` prefix are managed from this repository using [safe-settings](https://github.com/github/safe-settings).

---

- Global settings are managed in [./.github/settings.yml](`./.github/settings.yml`).
- Repository settings are managed in repository-specific files residing in [./.github/repos](`./.github/repos`).

---

## Adding a New Repository

[../deployment-settings.yml](../deployment_settings.yml) should be able to restrict access to repositories, but it doesn't appear to be working in practice (if the safe-settings GitHub app has access to a repository, it is going to update the settings for that repository.

So, follow these steps when adding a new repository to be managed by this repository:

1) Give the `safe-settings` GitHub app access to the new repository:

    1. Visit [GitHub](https://www.github.com)
    2. User Settings
    3. Navigate to `Applications`
    4. Press the `Configure` button associated with the `safe-settings` GitHub app
    5. In `Repository Access`:
        a. Ensure that `Only select repositories` is selected
        b. Add the new repository
        c. Press the `Save` button

2) [**OPTIONAL**] Add configuration information for the new repository in this repository.

    1. Create `<new repo name>.yml` [./.github/repos](./.github/repos)
    2. Populate the created file with settings specific to the new repository.
    3. Commit and push the changes.


## Rebuilding the [safe-settings](https://github.com/github/safe-settings) GitHub App

[safe-settings](https://github.com/github/safe-settings) functionality is implemented via a [Probot](https://github.com/probot/probot)-based [GitHub app](https://docs.github.com/en/developers/apps). I didn't know what any of these words meant when starting to play with all of this, but it is actually quite easy to get up-and-running quickly after a steep learning ramp. Hopefully this information helps with that ramp for others (and me when I visit this page again in the future).

- [Probot](https://github.com/probot/probot) is a framework that makes it easy to write [GitHub apps](https://docs.github.com/en/developers/apps).
- [Probot](https://github.com/probot/probot) apps are trivially easy to securely configure (either running locally on your machine or in an deployment environment such as [Glitch](https://glitch.com/)) for use in your GitHub account.
- [Glitch](https://glitch.com/) is a free app hosting service.

### To Deploy the safe-settings GitHub app using Glitch:

To create a user-specific instance of the safe-settings GitHub app on Glitch:

1. Visit https://www.glitch.com
2. Click the `New Project` button
3. Click the `Import from GitHub` button (make sure that you authenticated with your GitHub account)
4. The repository URL will be `https://github.com/github/safe-settings.git`
5. [**TEMPORARY WORKAROUND**] The current release of safe-settings [v2.0.17](https://github.com/github/safe-settings/releases/tag/2.0.17) has a problem in [package.json](https://github.com/github/safe-settings/blob/5dd8e7987f6bf685a44ad857d0c0cc3092cd7002/package.json#L57) that must be fixed before code is deployed to glitch:
    a. Edit `package.json` and make the change:

        "engines": {                        "engines": {
          "node": ">= 12.14.1"     ==>        "node": "16.x"
        }                                   }
6. Glitch will churn and eventually host a page with the heading "Welcome to safe-settings"
7. Click the `Register GitHub App` button (note that I had to open the Glitch preview in a new window to get the button to work). This will redirect you to [GitHub](https://github.com).
8. Github will display a page with the heading "Create GitHub App"
9. Give the app a name; "Safe Settings" is a good default.
10. Click the `Create GitHub App for <your github username>`
11. Install Safe Settings for `Only select repositories`
12. Click the `Install` button.
13. Close the window and return to Glitch.
14. Within Glitch, the `.env` file should now be populated with secrets that were just created.
15. Add the environment variable `CRON` and set it to `0 0 */1 * * *` to automatically updated repository settings every hour.
16. That's it! Your GitHub app will now monitor changes in the `admin` repository and push those changes to all applicable repositories.
