# Octopress-CLI

[Octopress-CLI](http://octopress.lynnard.tk) is a shell script to make [octopress](http://octopress.org/) more command-line friendly.

Features include:

* access [octopress](http://octopress.org/) rake commands easily
* [automatic udpate](#automatic-update) to save you from typing the same commands over and over again (for updating the website)
* added drafting and queueing abilities

## Dependencies

* `lockrun`: for locking the running instance of the script

## Configuration

Open up the script and change the following

* `$OCTOPRESS_HOME`: the directory of your octopress repository

You should also create two directories `_drafts` and `_queue` under `source` to allow for drafting and queueing.

## Usage

    octopress rake <rake command>
    octopress update
    octopress help

* `rake <rake command>`: this is identical to `cd` into the octopress repository and perform `rake <rake command>`
* `update`: perform the [automatic udpate](#automatic-update)
* `help`: output help message

## Automatic Update

This does the following steps in turn

1. checking for finished drafts in `_drafts`
    * any draft with `published: true` in its YAML front matter will be moved to `_queue`
2. checking for publishable posts in `_queue`
    * any queued post with `date` in its YAML front matter before the current date is moved to `_posts` 
        * if without `date` then it's moved to `_posts` immediately
    * the posts are also normalized
        * `layout: post` is added if no `layout` property found
        * `title: <filename of the post>` is added if no `title` property found
        * `date: <current date>` is added if no `date` property found
3. check for any changes in the octopress directory using `git diff`
4. if there are new changes, commit the changes and run `rake gen_deploy`

## Integration with crontab

Add the following line to your crontab

    */5 * * * * octopress update

to check for updates every 5 minutes for example.

With this you don't even need to think about deployment - you just need to write the drafts/posts, and give the system some time to sort everything out automatically.
