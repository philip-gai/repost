# (repo)st

(repo)st - A 🤖 (bot) for posting new GitHub discussions using your existing repository's pull request workflows 📬

## Quickstart Guide

1. [Install the GitHub App](https://github.com/apps/repo-st) and authorize for any repositories or teams you would like it to be able to post to or watch for markdown posts.
2. Add a `.github/repost-app.yml` (not `.yaml`) configuration file to any repositories you want the bot to watch. [Look here for an example in the demo repo][Repost demo config]
   1. Provide what folders you want (repo)st bot to watch and (optionally) what folders you would like it to ignore when new pull requests are open

    ```yml
    watch_folders:
    - docs/
    ignore_folders:
    - docs/demo/ignore
    ```

3. Now whenever you create a pull request with discussion markdown in those watch folders, (repo)st will ask for approvals to create discussions, and when the pull request is merged, it will create the discussions and post as the author!
4. See [Usage](#usage) for more specific usage instructions

## Usage

### Prerequisites

1. Follow the [Quickstart guide](#quickstart-guide) for information on getting started
2. If there is no `.github/repost-app.yml` file in your repository, (repo)st will not do anything

### App Configuration Options

These options should go in your repository's `.github/repost-app.yml` file.

| Name | Description | Required | Example
| --- | --- | --- | ---
| `watch_folders` | A list of what folders (relative paths) (repo)st should watch when new pull requests are open<br/>&nbsp;&nbsp;1. It is recommened to include the final `/` in the path<br/>&nbsp;&nbsp;2. (repo)st will also watch all subfolders unless you ignore them in `ignore_folders` | Yes | [See demo config][Repost demo config]
| `ignore_folders` | A list of what folders (relative paths) (repo)st should *ignore* when new pull requests are open | No | [See demo config][Repost demo config]

### Discussion Markdown

(repo)st needs to know certain information such as what repository or team to create the discussion in, and what the discussion category should be. This information should be provided in YAML metadata at the top of your markdown file.

#### Example

([Source in (repo)st demo repository](https://github.com/philip-gai/repost-demo/blob/main/docs/demo/hello-world.md))

```markdown
<!-- 
author: philip-gai
repository: https://github.com/philip-gai/repost-demo
category: announcements
-->

# Hello World! 👋

Hello beautiful world! 🌎

```

#### Metadata

| Name | Description | Required | Example |
| --- | --- | --- | --- |
| `author` | The author of the post. Should be your GitHub login (handle) | Yes | `philip-gai` |
| `repository` | The full url to the repository to create the discussion in<br/>**Prerequisites:**<br/>&nbsp;&nbsp;1. Discussions are enabled<br/>&nbsp;&nbsp;2. The app is installed on the repo | **Conditional**: Required if no `team` is provided | `https://github.com/philip-gai/repost-demo` |
| `team` | The full url to the team to create the discussion in<br/>**Prerequisites:**<br/>&nbsp;&nbsp;1. The app is installed on the team organization | **Conditional**: Required if no `repository` is provided | `https://github.com/orgs/elastico-group/teams/everyone` |
| `category` | The name of the discussion category | Yes | `announcements` |
| Discussion Title | The title of your discussion should be the first top-level header (i.e. `# Discussion Title`) | Yes | See [Example](#example-discussion-markdown) |
| Discussion Body | The body of your discussion is everything after the top-level header | Yes | See [Example](#example-discussion-markdown)

### Workflow for reviewing and posting a new discussion

1. Write your discussion post with with [(repo)st discussion markdown](#discussion-markdown)
2. Create a pull request
3. (repo)st will comment on the discussion markdown file asking for approval from the author to post the discussions. It will also notify you of any validation erros.
   1. An approval requires the author to react (not reply) to the comment with a 🚀
   2. If there are errors, fix them and recreate a new pull request so (repo)st can re-validate (Will add this in the future. See [issue](https://github.com/philip-gai/repost/issues/36))
4. Receive feedback from your teammates
5. Make updates (these will not be revalidated unless you recreate the pull request. See [issue](https://github.com/philip-gai/repost/issues/36))
6. Approve all the discussions you would like posted by reacting with a 🚀
7. If (repo)st bot asks, make sure to authenticate so it can post as the author and not as itself
8. Merge the pull request
9. (repo)st will create the discussions and reply to its comments with a link
   1.  If there were issues it will include them in the reply

## Contributing

If you have suggestions for how repost could be improved, or want to report a bug, open an issue! We'd love all and any contributions.

For more, check out the [Contributing Guide](CONTRIBUTING.md).

## License

[ISC](LICENSE) © 2021 Philip Gai <philipmgai@gmail.com>

[Repost demo]: https://github.com/philip-gai/repost-demo
[Repost demo config]: https://github.com/philip-gai/repost-demo/blob/main/.github/repost-app.yml
