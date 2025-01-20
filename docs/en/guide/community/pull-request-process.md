# PR Contribution Process
[Open Source Guide](https://docs.github.com/pull-requests)

## 1. Sync Fork from Upstream

[Github Documentation: syncing-a-fork](https://docs.github.com/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork)

To prevent conflicts caused by changes in the upstream repository, you should `sync fork` before making a `pr`, and resolve conflicts (try to resolve conflicts locally).

![sync-fork](/assets/sync-fork.png)

## 2. Sync from Remote Repository && Resolve Conflicts Locally
1. Use the `git pull` command locally to sync code from the remote repository

2. If there are no conflicts, that's great. If there are conflicts, refer to [about-merge-conflicts](https://docs.github.com/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts/about-merge-conflicts) to resolve them.

## 3. Commit && Push to Remote Repository

::: warning Code Formatting
Please use `mvn spring-javaformat:apply` to format your code before committing.
:::

1. A `pull request` can only contain one `commit`. If there are multiple `commits`, use the [Rebase command to merge commits](rebase-option)
2. Each `commit` should add corresponding modification records in the `CHANGELOG`.
3. Ensure the `commit message` follows the [Angular Commit Guidelines](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#commits) for proper formatting.
4. Use `git push` or `git push -f`(add `-f` if merging remote `commits`) to push `commit` to the remote repository.

- [Reference document for Rebase operation](rebase-option)

::: tip
- [Conventional Commits Specification](https://www.conventionalcommits.org/en/v1.0.0/)
- [Angular Commit Guidelines](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#commits)
:::


## 4. Create Pull Request

1. **Create a pull request**

![create-full-pequest](/assets/create-full-pequest.png)

2. **Fill in the title and comment carefully**. The `title` briefly describes your intention, and the `comment` provides a detailed description of the process. You can refer to closed `pr`s.

![image](/assets/pr.png)

3. **Handle reviews**. If your `pull request` is perfect, it will be directly accepted by the community. If the community `review` finds issues, there will be comments, and we can discuss directly. After the issue is resolved, click `Resolve conversation`.

![handle-reviews](/assets/handle-reviews.png)

> Note: If multiple commits occur during the problem-solving period, we need to use the rebase command to merge commits!