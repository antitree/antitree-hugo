# antitree-hugo

* The docker container has an HTML folder this folder contains an archive of all the previous HTML from the wordpress site. DO NOT DELETE THIS FOLDER.
* To build the HTML, run hugo in the site path
* cp -R `public/*` to the html folder of the container
* That should be all you need to do but you can also ./run.sh to restart it

## TODO
* Remove hugo from the server and replace with live updates
* Review what happens when the old html pages are in there but you copy the new html on top of them. Are they call gone?
