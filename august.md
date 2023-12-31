# August 2023
## August 9, 2023
- Setting up log
- testing out github pages setup
- Testing out MD linking, good source on how to do it [here](https://stackoverflow.com/questions/7653483/github-relative-link-in-markdown-file)
- back linking to main README page
- link to [handy markdown cheat sheet](https://www.markdownguide.org/cheat-sheet/)

## August 11, 2023
- Installing Ruby and Jekyll
- wanting to add a theme to this page with Jekyll, [github documentation](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/adding-a-theme-to-your-github-pages-site-using-jekyll) was a bit unclear on how to do that, but found the [Jekyll install instructions](https://jekyllrb.com/docs/installation/macos/)
- ruby install step from documentation:`ruby-install ruby 3.1.3` is not particularily evergreen, so opting to us `ruby-install ruby` to get the most recent version at 3.2.2. ran the command `ruby -v` as a sanity check that it installed and my terminal can use it.
- skipped the step to configure my shell to automatically use `chruby`. Since the documentation didn't specify why I should do this, I did not initially follow this. According to a Google search: *chruby is essentially a very small shell script that sets a few environment variables (mostly $PATH ) to point at a given install of Ruby. It also has a separate, optional, script to auto-switch ruby versions when changing directories.*
- Installed Jekyll: `gem install jekyll`. Interesting error `You don't have write permissions for the /Library/Ruby/Gems/2.6.0 directory.` A few options that [some searching](https://stackoverflow.com/questions/51126403/you-dont-have-write-permissions-for-the-library-ruby-gems-2-3-0-directory-ma) gave me:
    1. install a bundler: `gem install bundler && bundle install`
    2. change the directory permissions recursively: `sudo chown -R $USER /Library/Ruby/Gems/`
    3. Try the chruby step above and admit defeat. update the path: ```
    echo "source $(brew --prefix)/opt/chruby/share/chruby/chruby.sh" >> ~/.zshrc
    echo "source $(brew --prefix)/opt/chruby/share/chruby/auto.sh" >> ~/.zshrc
    echo "chruby ruby-3.2.2" >> ~/.zshrc # the documentation uses ruby-3.1.3, but since we did the most recent version this is updated
    ```
- Tried option 3 first, same error. tried option 2, got a new error: 
```
Error installing jekyll:
    The last version of rouge (>= 3.0, < 5.0) to support your Ruby & RubyGems was 3.30.0. Try installing it with "gem install rouge -v 3.30.0" and then running the current command again
    rouge requires Ruby version >= 2.7. The current ruby version is 2.6.10.210.
``` 
- Going to try the suggested method `gem install rouge -v 3.30.0`. I'm beginning to think my liberties in deviating from the documentation are to blame.
- same error, but with `gem install google-protobuf -v 3.23.4` instead, hopefully not going to require a bunch of these...
- going to install their suggested version of ruby `ruby-install ruby 3.1.3` and retry by sticking with their`` documentation religiously.
- it also helps to remeber to restart the terminal when making changes to the shell profile. Forgot that step. now the correct version is appear when running the `ruby -v` command
- started a new jekyll project `jekyll new logbook` and hoisted the created files to the project root. Deploy successful! will keep the [Jekyll documentation](https://jekyllrb.com/docs/permalinks/) handy going forward

- using the VSCode terminal and remebering the hot keys **^`**, I wonder if there is a way to have VSCode always open with the terminal


## August 17, 2023
- Unlearning bashisms today. Appreciating that the shebang `#!/bin/sh` just means use the default shell, which can mean use bash, dash, or zsh. So when a shell script gets used in different environments certain bashisms can really mess you up.
- one particular case is with bash array, which are not supported in dash. So a line like `ARRAY_VARS=('thing 1' 'thing 2' ...)` gives the syntax error `syntax error: unexpected "("`
- This also lead into using arrays into loops such as `for array_vars in ${ARRAY_VARS[@]}; do...`, the easiest solution is to set the array indirectly as arguments to the script (assuming the elements of the array are predefined) `./my-script.sh 'thing 1' 'thing 2' ...` and then convert the for loop into `for array_vars in $@; do...` which will loop through the variables as argument ($1, $2, ...) which is kind of hacky, but seems to work.


## August 23, 2023
- using `export` in shell scripts for portability, using direct assignments like `var1="foo"` is a bashism, or is at least not portable to all shells. Writing `export var1="foo"` is preferable, especially if the shebang is shell agnostic `#!/bin/sh`


## August 27, 2023
- learned something interesting about EC2 instances, the user data can be changed and retested without terminating and creating a new launch instance. Instead the instance can be stopped, and then the user data can be update. the instance can be started up again to test the new user data script updates. This is a good time saving/complexity reducing technique.


## August 28, 2023
- `echo "new_password" | sudo passwd --stdin nameofuser` can change the password of a user in linux programatically, a bit of a naughty was to do this, but it works and is handy in certain situations, like when the AMI you just created refuses to reflect the user credentials you set for it...