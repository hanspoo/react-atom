# React + Atom IDE

React is a new software development paradigm, it has been called "the new jQuery", can be applied in different environments and devices. The main environment of course is the web, the web version is referred as ReactJS, other important environment is native mobile development called react-native.

A good IDE is important for any programmer, and one of the most difficult aspects of facing a new technology is to start, so this instructions will let you with a productive react working environment: you will be able to create react projects in a validated javascript programming environment using the Atom editor as your IDE.

This tutorial is for Ubuntu 18.04, but the instructions should work with minimal modifications in other linux distros (i.e. Use yum instead apt for red hat based distros).

## 1.- Nodejs installation

The React development environment requires nodejs, which is a javascript local runtime environment. In production nodejs is not necessary since when building the react project for production with "yarn build", a standard, static, self contained html project is generated, without any dependencies to nodejs. The next instructions will refresh your repo configuration, install the curl web client, and finally nodejs 10.

```
sudo apt update
sudo apt -y install curl
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt -y install nodejs
```

You can verify that it has been installed by executing the node command requesting the version number:

```
node -v
```

Credits to: https://joshtronic.com/2018/05/08/how-to-install-nodejs-10-on-ubuntu-1804-lts/

To manage packages in node projects, the "npm" command comes preinstalled, in react it's common the use yarn that does basically the same as npm, we will install it globally in our system with the following command:

```
sudo npm i -g yarn
```

## 2.- React installation

Now it's time to install the ReactJS project creation script: create-react-app, we'll use sudo to install it globally, available for the entire system, we'll use yarn to install it.

```
sudo yarn global add create-react-app
```

You can verify that it has been installed correctly by creating a hello world project:

```
create-react-app hello-world
```

If all goes well you will see messages with instructions to start the project.

Ok, we can create and run react projects, but without a good IDE we are not going to make much progress.

## 3.- Atom installation

Before install atom we'll need to install atom 1.39.1 dependencies for Ubuntu 18.04.

```
sudo apt install -y git libgconf-2-4 libgconf2-4 libgtk-3-0 libnotify4 libxtst6 libnss3 gvfs-bin xdg-utils libx11-xcb1 libxss1 libasound2 libxkbfile1 policykit-1
```

We will download atom first with curl using -L parameter to allow it to follow redirects. The url is debian specific so it must be modified in red-hat based distros:

```
curl -L  https://atom.io/download/deb --output atom.deb
```

Then install atom in the system, a low level installation with the base command dpkg:

```
sudo dpkg -i atom.deb
```

Next we'll add some packages to empower atom for react development. Note that personal Atom configuration is kept on the .atom folder in the user account: $HOME/.atom. The next command is run without sudo. The apm command stands for "atom package manager".

```
apm install linter linter-eslint prettier-atom linter-ui-default atom-import-module atom-react-autocomplete autocomplete-modules click-a-path react-snippets intentions busy-signal atom-sublime-monokai-syntax
```

From the previous atom packages, the code validator: linter and the code formatter: prettier, are the most important.
Using cat with a Here Document, we create a basic atom configuration file config.cson:

```
cat <<EOF>$HOME/.atom/config.cson
"*":
  core:
    telemetryConsent: "limited"
    themes: [
      "atom-dark-ui"
      "atom-sublime-monokai-syntax"
    ]
  editor:
    fontSize: 15
  "prettier-atom":
    formatOnSaveOptions:
      enabled: true
    useEslint: true
  welcome:
    showOnStartup: false
EOF
```

If you have trouble to create the file with cat and the here document, the file config.cson is available in the github repo referenced at the bottom of the page. In any case atom creates it if it doesn't exist. You can use atom preferences to manage it. 
 
## 4.- Eslint configuration

To convert Atom into a real IDE, we need to configure code validation as you type. The javascript validator is called eslint (Ecma Script Lint). We'll install it globally with:

```
npm i -g eslint
```

Then it must be configured in each project. Let's create a react project called test-lint in a temporary folder.

```
cd /tmp
create-react-app test-lint
cd test-lint
```

This next command will install the required dev packages and will create a basic linting configuration, which we'll override later.

```
eslint  --init
```

When asked, answer as indicated here:

How would you like to use ESLint? To check syntax, find problems, and enforce code style  
What type of modules does your project use? JavaScript modules (import/export)  
Which framework does your project use? React  
Where does your code run? Browser  

How would you like to define a style for your project? Use a popular style guide  
Which style guide do you want to follow? Airbnb (https://github.com/airbnb/javascript)  
What format do you want your config file to be in? JSON  

If it asks something else, just say "Yes"  

Let's override the basic eslint json configuration .eslintrc.json  file for react:  

```
cat <<EOF>.eslintrc.json 
{
  "parser": "babel-eslint",
  "env": {
    "browser": true
  },
  "extends": "airbnb",
  "rules": {
    "no-console": 0,
    "react/jsx-filename-extension": 0,
    "import/no-extraneous-dependencies": 0
  }
}
EOF
```

## 5.- Congratulations

Your React IDE is ready, remember you are in the project folder test-lint let's test it:

```
atom .
yarn start
```

Last command will start the project, as indicated, open it in your browser on the port: http://localhost.3000.

Try modifying App.js and you'll se indications of any typos or warnings in your code.
