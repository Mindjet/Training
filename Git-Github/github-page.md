# Create your project page

	
After finishing coding your page, you push it to your Github. People still cannot visit your page, thought your codes are online. All they got is just your raw code. 
	
So here I am to introduce a way to create your project page with git command line.


### clone your repository to the local
Let's assume that the repository you want to clone is named Git-tricks, and it located at [https://github.com/Mindjet/Way2Android](https://github.com/Mindjet/Way2Android).  

This could be very simple with command line.

```zsh
git clone git@github.com:Mindjet/way2android.git
```

**Please make sure that there is a `index.html` in your repositoty, or your page has nothing to show.**

### create local branch based on the remote branch
Let the local branch' s name be gh-pages.

```zsh
git checkout -b gh-pages origin/master
```

You can modify your local branch or do nothing.


### push local branch to the Github.

```zsh
git push origin gh-pages
```
Then switch to the master branch.

```zsh
git checkout master
```


### merge the gh-pages to the master branch

```zsh
git merge gh-pages
```


### push master branch to the Github

```zsh
git push origin master
```


### Testing

Now you can visit `http://mindjet.github.io/way2android/` to see what happen! Amazing!


Maybe sometimes you want to delete the gh-pages, the command line following might help.

### Delete the romote branch

```zsh
git push origin :gh-pages
```
Note that there is a *space* after the word **origin**.


### Delete the local branch

```zsh
git branch -d gh-pages
```
