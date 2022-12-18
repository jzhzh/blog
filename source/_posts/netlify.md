---
title: How to build a blog with Hexo + GitHub + Netlify
date: 2022-12-18 11:35:04
tags: blog
---

## Why did I choose Hexo + GitHub + Netlify?

### Hexo

[Hexo](https://hexo.io/) is a fast, simple & powerful blog framework which can help us to generate the static files of blog and deploy the sites. The reason why I choose **Hexo** is only [NexT](https://github.com/next-theme/hexo-theme-next) theme of Hexo.

### GitHub

I use GitHub to manage all the articles of my blog and regard it as a Cloud Drive to sync files.

### Netlify

The reasons why I choose [Netlify](https://www.netlify.com/) are:

- It can help me to generate and deploy the sites **automatically**, which means that I **don't** need to memorize the command of Hexo such as `hexo generate` and `hexo deploy`. I only need to `git push` my articles to my repo on GitHub.
- I can add my customized domain so easily. It can address DNS verification problem and also https problem in a very simple way.

Alternatively, you can also choose GitHub Pages (Maybe I will move my blog to GitHub Pages in the future).

## Steps to build your own blog

### Node.js and Git

#### Installation

You should install Node.js and Git on your own laptop first. It's recommended to follow the official website of [Node.js](https://nodejs.org/en/download/) and [Git](https://git-scm.com/downloads).

Here I also recommend to install [GitHub Desktop](https://desktop.github.com/) because of its convenience.

### GitHub

#### New repo

Create a new repo for storing all the files of your blog.

Notice that you **should not** name your repo as `yourname.github.io` if you want to use Netlify. Because if you use `yourname.github.io` as your repo name, GitHub will regards it as a GitHub Page automatically and do GitHub Actions (Build with Jekyll) to deploy the site automatically.

Also you should set the repo as **Public** and without **README.md** file.

![](https://i.imgur.com/Gyml609.png)

Here because I have created the repo named `blog`, it said that the repo exists on my account.

#### New SSH key

First you need to set the configurations of git on your laptop:

```bash
git config --global user.name "your_name"
git config --global user.email "your_email"
```

And then you need to generate a SSH key on your labtop using:

```bash
ssh-keygen -t rsa -C "your_email"
```

Open your profile of your GitHub. Click `settings` -> `SSH and GPG keys` -> `New SSH key`. You can set any name of **title**. Return to the bash and input:

```bash
cat ~/.ssh/id_rsa.pub
```

Copy the output to **Key** box. And Click `Add SSH key`.

Finally, for testing whether you have added SSH key successfully, you can use:

```bash
ssh -T git@github.com
```

If you can see your username of GitHub, you have added SSH key successfully.

### Hexo

#### Installation

It is highly recommended that you follow the steps on the [Hexo official website](https://hexo.io/) to install Hexo.

What you need to do is just:

```bash
npm install hexo-cli -g
hexo init blog
cd blog
npm install
```

Notice that `blog` in `hexo init blog` and `cd blog` is the folder name of your blog. You can customize it such as `hexo init myblog`.

#### NexT Theme 

##### Installation

Follow the official instructions to install [NexT Theme](https://github.com/next-theme/hexo-theme-next).

I also recommend the first method that mentioned in it.

```bash
cd blog
npm install hexo-theme-next
```

Notice that if you install NexT in this way (npm), the theme will appear in the **node_modules** folder. If you install NexT in the second way (git), it will appear in the **theme** folder.

##### Configure your NexT Theme

Follow the [official docs of Configuration of NexT Theme](https://theme-next.js.org/docs/getting-started/configuration.html).

1. Please ensure you are using Hexo 5.0 or later.

2. Create a config file in site's root directory, e.g. `_config.next.yml`.

3. Copy needed NexT theme options from NexT config file into this config file. If it is the first time to install NexT, then copy the whole NexT config file by the following command:

   ```bash
   # Installed through npm
   cp node_modules/hexo-theme-next/_config.yml _config.next.yml
   # Installed through Git
   cp themes/next/_config.yml _config.next.yml
   ```

#### Configuration of Hexo

Then, we need to set some configurations of Hexo.

Open the configuration file `_config.yml` in your blog folder. And set the variables as follows:

```yaml
# Site
title: your_site_name
subtitle: ''
description: ''
keywords:
author: your_name
language: en
timezone: ''
```

And also:

```yaml
## Themes: https://hexo.io/themes/
theme: next

# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repository: https://github.com/your_name/your_repo_name
  branch: main
```

### Push Blog to GitHub

Before we push the blog to GitHub, we need to make sure two things:

- Add a README.md file manually and put it in the blog root folder.

- Make sure you have a .gitignore file in the blog root folder.

Then you can push the blog files to GitHub just as follows:

```bas
cd blog
git init .
git commit -m "init"
git branch -M main
git remote add origin git@github…..git
git push -u origin main
```

**Notice that you should not run** `hexo generate` **or** `hexo g` **or** `hexo d` **because Netlify will help us to generate and deploy the site automatically.**

### Netlify

#### Deploy your blog site

Log in [Netlify website](https://www.netlify.com/), select import from an exitsting project and select GitHub. Choose the repo that you created just now (whose name is **not** username.github.io). And set Build command to `hexo generate` and set Publish directory to `public` as follows:

![](https://i.imgur.com/VkCweN6.png)

Then the site has been succefully deployed.

#### Custom domains

You can easily customize your own domain in **Domain management** on the side bar or the guide page if you deploy your site first time.

![](https://i.imgur.com/doheaCv.png)

#### DNS verification and HTTPS

You can do DNS verification and enable automatic TLS certificates so easily in the **HTTPS** part of **Domain management** on the side bar (just need to click the button).

![](https://i.imgur.com/nMkyZoJ.png)

## How should you write a new article

### Create a new article

You just need the following commands to create a new article:

```bash
cd blog
hexo new post new_article_file_name
```

Then, you will find a new md file whose name is `new_article_file_name` in ` source -> _posts` folder.

For the title of your article, you can set it on the top part of md file. 

Notice that:

1. `new_article_file_name` is the Markdown file name, but not the title of your article.
2. When you start writing Markdown file, the highest head should be head 2 not head 1 because you have set the title on the top part of md file.

Now you can edit the Markdown file to write the articles.

By the way, I use [Typora](https://typora.io/) + [iPic](https://apps.apple.com/us/app/ipic-image-file-upload-tool/id1101244278?mt=12) (Image Upload Tool) + [Imgur](https://imgur.com/) (Image hosting service) to write the blog (Mardown files). Here is also a perfect [Markdown tutorial website](https://commonmark.org/help/).

### Push the local repo to GitHub

#### Bash

You can use bash to push the repo to GitHub:

```bash
git add
git commit -m “”
git push -u origin main
```

#### GitHub Desktop

You can also simply use GitHub Desktop to commit and push. You can edit the comments on the left-bottom corner and push on the top button:

![](https://i.imgur.com/jOtbfOD.png)

## How should you delete the articles

You can simply remove md files in ` source -> _posts` folder. And push the local repo to GitHub again. Netlify will help you regenerate and deploy the blog site.

## Tips

You may notice that there is an alert of Netlify which said that **Insecure mixed content detected in**. The reason for that is the configuration file sets `url` as `http` and `example.com`.

So you need to modify the configuration file `_config.yml` of you blog.

```yaml
# URL
## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
url: https://your_domain.com
```

## Conclusion

Hexo + GitHub + Netlify is a very convenient combo for blog writing.

Enjoy the blogging!
