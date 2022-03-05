# Intro





# Commonly Used Statements

## To clone a respository into an existing respository

* Intro

  Befor: respository A and respository B.

  After: respository A which contains files in B.

* Ref

  [How to Mirror (Copy) an Entire Existing Git Repository Into a New One](https://medium.com/cloud-native-the-gathering/how-to-mirror-copy-an-entire-existing-git-repository-into-a-new-one-3bb8faefad9e)

* Answer

  `git push --mirror $gitRespository`





# Visualization

## Pretty Commit Tree

REF:[Pretty Git branch graphs](https://stackoverflow.com/questions/1057564/pretty-git-branch-graphs)

I had typed `git config --global alias.adog "log --all --decorate --oneline --graph"`, so `git adog` can be invoked to print branch graphs, where the data of every commit contains one line.

I had typed `git config --global alias.ADOG "log --graph --abbrev-commit --decorate --date=relative --all"`, so `git ADOG` can be invoked to print branch graphs, where the data of every commit contains multiline data.

