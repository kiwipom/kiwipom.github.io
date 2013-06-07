---
layout: post
title: Compiling less on a Windows Azure website
---


### Using less in my node.js app

I'm writing a node.js app and trying to balance out a few principals:


- Using a css precompiler is a Good Idea (™)
- We shouldn't put compiled resources into source control
- Lists should always have three items


So here's the setup: I have a stylesheets folder that contain all my .less files and a single .less file that puts them all together, like this

    @import "shared.less";
    @import "homepage.less";
    @import "thispage.less";
    @import "thatpage.less";
    
To make this work locally on my machine is trivial:

1. Install less globally with `npm install less -g`
2. Compiling is as easy as `lessc public/stylesheets/styles.less > public/stylesheets/styles.css`
3. Profit!

As an aside, I run this in the start script of package.json, like this:

    {
        // omitted for brevity
        
        start: { lessc public/stylesheets/styles.less > public/stylesheets/styles.css; node app.js },
    }
    
Neat, eh?

So - now I want to deploy this app to Windows Azure and for this I use git deployments because

- It's easy
- The app was in a git repo anyway
- It's really *really* easy

First time I pushed the app up, all the styling was lost. Which makes sense because 

- Even though less is included in package.json, it won't be installed globally on the WAWS instance, so `lessc` won't be a known command
- I have no idea, but it's most likely that the app isn't started via `npm start` but directly with `node app.js`
- I really like lists with 3 items. No, really.

After a tiny bit of investigation, I discovered [custom deployments](http://blog.amitapple.com/post/38418009331/azurewebsitecustomdeploymentpart2) and that seemed like it could work.


So, step 1 - Install the azure-cli [command line tools](http://www.windowsazure.com/en-us/develop/nodejs/how-to-guides/command-line-tools/)

*this is going to be another 3-item list, isn't it?*

Step 2 - generate the deployment scripts with `azure site deploymentscript --node` which will create 4 files for you. The important one is `deploy.cmd` which contains the script that will run when you `git push` your repo up onto the WAWS instance. It's a bit scary at first, but it's not too bad. There's a bit fairly near the bottom that actually does the work, and will look something like this:

	:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::	:::::::::::::::::::::::::::::::::::::::::::::::
	:: Deployment
	:: ----------
	
	:Deployment
	echo Handling node.js deployment.

	:: 1. KuduSync
	call %KUDU_SYNC_CMD% -v 50 -f "%DEPLOYMENT_SOURCE%" -t "%DEPLOYMENT_TARGET%" -n "%NEXT_MANIFEST_PATH%" -p "%PREVIOUS_MANIFEST_PATH%" -i ".git;.hg;.deployment;deploy.cmd"
	IF !ERRORLEVEL! NEQ 0 goto error

	:: 2. Select node version
	call :SelectNodeVersion

	:: 3. Install npm packages
	IF EXIST "%DEPLOYMENT_TARGET%\package.json" (
	  pushd %DEPLOYMENT_TARGET%
	  call !NPM_CMD! install --production
	  IF !ERRORLEVEL! NEQ 0 goto error
	  popd
	)

*shit, this is a 3-item list within a 3-item list! List-ception, dude!*


Now - Notice that `%DEPLOYMENT_TARGET%` variable? That will be the path to the `\site\wwwroot` folder on the machine. We're going to use that:

Now step 3: Add this code: (*but not as item 4 on the list, cos that would fuck it all up*)


	IF NOT DEFINED LESS_COMPILER (
	  SET LESS_COMPILER=%DEPLOYMENT_TARGET%\node_modules\.bin\lessc
	)

	call %LESS_COMPILER% %DEPLOYMENT_TARGET%\public\stylesheets\styles.less > %DEPLOYMENT_TARGET%\public\stylesheets\styles.css


And that should do the trick. Now, when you push your changes via Git, the deployment script will find the deployed less compiler, and turn your .less file into a .css file, and the site will look purty again.

Note: My first attempt at this I tried referencing the less compiler installed by npm to `node_modules\less\bin\lessc` without any luck. Really not sure why Azure wouldn't find it there, and maybe it might work for you? but it didn't for me.

So that's that. Comments and feedback are welcome, as always.