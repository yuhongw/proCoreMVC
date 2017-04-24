## 如何贡献项目

* 在 GitHub 上 `fork` 到自己的仓库，如 `user/proCoreMVC`，然后 `clone` 到本地，并设置用户信息。

  ```sh
  $ git clone git@github.com:user/proCoreMVC.git
  $ cd proCoreMVC
  $ git config user.name "yourname"
  $ git config user.email "your email"
  ```

* 修改代码后提交，并推送到自己的仓库。

  ```sh
  $ #do some change on the content
  $ git commit -am "Fix issue #1: change helo to hello"
  $ git push
  ```

* 在 GitHub 网站上提交 pull request。

* 定期使用项目仓库内容更新自己仓库内容。
  ```sh
  $ git remote add upstream https://github.com/yuhongw/proCoreMVC
  $ git fetch upstream
  $ git checkout master
  $ git rebase upstream/master
  $ git push -f origin master
  ```