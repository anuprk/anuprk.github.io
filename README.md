# Anup's blog

## Blog Installation using Jekyll and Github
I have moved my blog from blogger to github/jekyll. Some installation tips on mac if you are new to jekyll. 

```bash
brew update 
brew install rbenv ruby-build
```

I am using rbenv. I moved from rvm to rbenv. I like *shim* concept and makes life a lot easier. But you may want to continue with rvm.

```bash
rbenv init # You may want to add this to your .bashrc so the env is initialized every time you open new terminal
rbenv install 2.7.1 # Go for latest ruby. 
rbenv global 2.5.1
ruby -v
# You shoud see output as follows
>ruby 2.7.0p0 (2019-12-25 revision 647ee6f091) [x86_64-darwin19]
```

Follow more details on rbenv [here]( https://github.com/rbenv/rbenv )

```bash
gem install jekyll bundler
# go to directory where you want to create site.
cd mysite
jekyll new blog
cd blog
bundle exec jekyll serve
```
You should see jekyll up at http://localhost:4000







