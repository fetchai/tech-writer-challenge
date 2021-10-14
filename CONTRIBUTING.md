## Setting up the development environment

### Install Node JS

If you don't already have it installed you will need to install NodeJS on your system. This will depend on your operating system:

You can follow the instructions from [https://nodejs.org/en/](https://nodejs.org/en/) - note: for this exercise either v14 or v16 will work

### Installing the dependencies

Once you have installed NodeJS you will need to install vuepress and all its dependencies. Open a command line in the source directory and run the following command

    npm install

This will take a couple of minutes but afterwards you should expect to see a `node_modules/`
folder in the same folder as your checkout

### Run the dev server

Vuepress comes with a handy tool which makes developing the documentation easy. Simple run the following command in the terminal:

    npm run dev

This will build the documentation and run it locally here [http://127.0.0.1:8080](http://127.0.0.1:8080).

More importantly the dev server monitors the projects files. When you edit one, it will detect it and immediatly rebuild and refresh what is displayed.
