# Frequently Asked Questions

##### Can I use private registries and private images to build apps in Panamax?

Although we don't search private docker registries, you can use them in Panamax. To use a private image, first enter the CoreOS host with `$ panamax ssh` and pull your image using the docker pull command. After pulling the image, you can search for your image directly in Panamax. It will show up as a 'Local' image. You can now use it to build your application and template.

##### Are the templates I create in Panamax exposed to the public? My start-up is in Stealth Mode. 

They don't have to be. If the Github access token you provided to Panamax is scoped to access your private repos, you can save them there. We don't force saving to public repositories.

##### Can I create an application in Panamax that runs across multiple hosts? 

Not yet, currently an application exists on a single host but since CoreOS/Fleet has strong support for multi-hosts, we will eventually support it as well. 

##### Can I install Panamax on Windows?

Well, yes and no. Although Panamax can possibly (read not tested by us) be installed on Windows, we don't have an installer for Windows. But, that is a great opportunity for you to contribute to Panamax. Please find the Panamax installer [source code repo](https://github.com/CenturyLinkLabs/panamax-coreos) on Github.

##### Can I user the Docker command line ? 
Yes, you can `$ panamax ssh` will ssh into your coreos virtual machine.  Once inside you can run any command directly if you would like. 