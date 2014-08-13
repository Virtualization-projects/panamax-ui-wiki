# Frequently Asked Questions

##### Can I use private images to build apps in Panamax, while I am still in stealth mode?

Sure you can. To have a private image, you need to 'panamax ssh' into the coreos vm, and then build the private image locally there. Then when you search Panamax, you will see your private image. Now, you can use that private image to build your stealth application.

##### Can I save my stealth Panamax application as a template to a private repo?

Sure you can. As long as your Github access token in Panamax can access your private repos, you can save templates to private repos.

##### Can I create an application in Panamax that runs across multiple hosts? 

Not yet, currently an application exists on a single host but since CoreOS/Fleet has strong support for multi-hosts, we will support eventually as well. 

##### Can I install Panamax on Windows?

Well, yes and no. Although Panamax can possibly (read not tested by us) be installed on Windows, we don't have an installer for Windows. But, that is a great opportunity for you to contribute to Panamax. Please find the Panamax installer [source code repo](https://github.com/CenturyLinkLabs/panamax-coreos) on Github.