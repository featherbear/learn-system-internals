Hugo Slides Template
---

> Starter template for presentation-per-post [reveal-hugo](https://github.com/dzello/reveal-hugo) sites

# Getting Started

* [Create a repository based off the template](https://github.com/featherbear/Slides-Hugo-Template/generate)
* Create a `PERSONAL_TOKEN` secret for a [personal access token](https://github.com/settings/tokens) with the `repo` scope
* Clone your new repository
* Navigate into the repository directory
* Clone the reveal-hugo theme
  * `git submodule add git@github.com:dzello/reveal-hugo.git themes/reveal-hugo`
  * Not sure why this isn't already done automatically...
* Modify the `config.toml` file to your liking
* $$$

Every time you push to this repository, the site will be built and published to the repository's `gh-pages` branch

# Editing your posts

Create a new post with `hugo new YOUR_SLIDE_NAME.md`  
This post will be created as a _draft_

When the post is finalised, remove the `draft: true` property from the front matter

# Previewing your posts

To preview the public view, run `hugo server`  
To preview draft post as well, run `hugo server -D`  
