# OS X
## Prerequisites
According to init.sh: make g++ build-essential redis-server mongodb maven default-jdk nodejs-legacy npm curl git. Xcode command line tools should be installed.   
`brew install mvn` 
`npm install webpack`
(Running `npm run build` will execute "build", in package.json "scripts".)

# Fedora
* Install dependencies: `sudo bash init/fedora.sh`
* Create .env file with required config values in KEY=VALUE format (see config.js for a full listing of options) `cp .env_example .env`
  * Note: If you have Steam Guard activated on your account you will
    either have to deactivate it or create a new account for use with
    the retriever (recommended).
* Build `npm run build`
* Fedora does not run Redis as a daemon by default, so you need to start it separately with `npm run redis` or create your own service script. This needs to be done every time you want to run `npm run dev`.
* Run `npm test` to make sure your install works correctly
* Run all services in dev mode (this will run under nodemon so file changes automatically restart the server): `npm run dev`. You can also start individual services.