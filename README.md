# Example Documenation

## Introduction

This repo is built using the [Vuepress](https://vuepress.vuejs.org/) static site generator. This
is a very common style of creating documentation, especially in an open source setting.

In this model of documentation, the content is written in a plain text file format called [Markdown](https://daringfireball.net/projects/markdown/) which can easily be converted to
HTML. [Vuepress](https://vuepress.vuejs.org/) takes these markdown time along with some
additional config and then generates an HTML site which can be served to users.

## Challenge

This repo contains a subset of the information taken from the main [Fetch.ai Documentation Repo](https://github.com/fetchai/docs). As a technical writer in the [Fetch.ai](https://fetch.ai) team
you would be expected to help maintain similar repositories.

**Project Structure**

* `src/README.md` - This contains all the content of the documenation which was taken from the current documentation
* `.vuepress/config.js` - This is the configuration file that is used to build the documenation. It controls side bar

In terms of the structure of the repository there is a single markdown file which contains all the current documenation (`src/README.md`).

**Workflow**

1. [Fork the repository](https://docs.github.com/en/get-started/quickstart/fork-a-repo)
2. [Clone the repo](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) to your local machine
3. Make your changes.
4. Send back your proposed changes as a [Pull Request](https://docs.github.com/en/github/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)


**Challenge**

Read through the documenation that is present and have a think about the following points.

* Is the structure correct?
* Should the content be split to make it easier to understand?
* Is the ordering correct?
* Are there any broken links?

Then make the changes that you think are necessary. When you are happy with the changes send it back to us for review. While we do not expect you to setup up the development environment (see `CONTRIBUTING.md`) you might find it easier so you can see the final build.
