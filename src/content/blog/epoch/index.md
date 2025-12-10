+++
authors = ["Devanandan N B"]
title = "Epoch"
description = "Details of how I set this blog up with Zola to my liking."
date = 2025-09-15
[taxonomies]
tags = ["duckquill", "zola"]
[extra]
featured = true
+++

I had always wanted a blog site for myself. This need grew even more as I started playing CTFs and looked for a space to jot down my thought process of solving challenges. That's the epoch of this site. Here's the steps I took to set it up.

# Tech Stack

There are many ways to set up a blog. You can manually code up the entire site yourselves in plain HTML, CSS and if needed JavaScript and then copy the template to create new blogs; this is what I initially did, however this takes up way too much time and effort. So then comes Static Site Generators(SSGs) like Jekyll, Hugo, Eleventy, Zola etc. which turn your markdown content into webpages which share a similar structure and theme. It usually also comes with many other features like automatic syntax highlighting for your code, automatic image compression etc. SSGs largely automates your workflow and make pushing blogs faster.

I chose to host with Github Pages and go with [Zola Static Site Generator](https://www.getzola.org), as it was

- relatively new
- single binary (no dependency packages to install on your system, no compilation)
- fast (bonus: written in rust)

The other components I used in this site is as follows:

- [Duckquill](https://duckquill.daudix.one) - A theme for zola with a lot of features including syntax highlighting, latex support etc.
- [Zola Deploy to Pages](https://github.com/marketplace/actions/zola-deploy-to-pages) - GitHub action for deploying zola to GitHub-Pages.

# Step By Step Setup

## Step 1 - Setting Up Your Repository

1. Create your repository for the blog on GitHub. Your repository must be named `<USERNAME>.github.io` as is convention with GitHub Pages (or you can name your repository, say, `blog` which would appear at `<USERNAME>.github.io/blog`. This is usefull when you already have a main site).

2. Clone this repository to your local device and intialise your site with:
	
	```sh
	zola init <BASE_DIR>
	```
	
	This will create a sub-directory `<BASE_DIR>` which is the source folder for your site.

3. Install Duckquill theme within the repository by creating a submodule. Visit [Duckquill theme's documentation](https://duckquill.daudix.one/#installation) for detailed steps.
	
	```sh
	cd <REPO>
	git submodule init
	git submodule add https://codeberg.org/daudix/duckquill.git <BASE_DIR>/themes/duckquill
	cd <BASE_DIR>/themes/duckquill
	git checkout tags/vX.Y.Z # see the theme documentation for latest release version
	```

	Edit your `config.toml` within `<BASE_DIR>` and add this line at the top:

	```toml
	theme = "duckquill"
	```
4. Optionally, add `README.md`, `.gitignore` and `LICENSE` files to your repository. I chose the `CC BY-NC-SA 4.0` license as it seemed fit for my content; mostly writeups and technical blogs.

5. Next step is to edit/extend the duckquill defaults to personalise your website. In my case, I extended the default footer to include the Creative-Commons license clause as well as modified the top nav-bar. 
	
	- The theme's footer template is located at `<BASE_DIR>/themes/duckquill/templates/partial
	footer.html`. To override this, we copy the file onto `<BASE_DIR>/templates/partial/footer.html`. [Checkout](https://github.com/nbdevanandan/nbdevanandan.github.io/blob/main/src/templates/partials/footer.html) my GitHub to see the changes I made to `footer.html` to make it display the license clause.
	
	- The website's appearance and link content on both top and bottom nav-bars are defined in the `config.toml` file. You can refer to the theme's copy in `<BASE_DIR>/themes/duckquill/config.toml` to replicate it's color scheme and edit the links/add new icons. It also contains plenty of other customisation options which you probably want to look at.

6. Now is the time to push a blog or two. To do that simply create a new folder with the blog title in the `<BASE_DIR>/content` directory and write the contents within s `README.md` file inside.

7. Finally to set up automatic deployment to github-pages, I used the [Zola Deploy to Pages](https://github.com/marketplace/actions/zola-deploy-to-pages) GitHub Action from GitHub Marketplace.Under your repository settings: `Settings > Actions > General`, in Workflow permissions, make sure that GITHUB_TOKEN has Read and Write permissions. Also under `Settings > Pages` select `Source` as 'Deploy from a branch' and select branch as `gh-pages`.
