### Break your documentation into logical sections

A good way to break down your documentation is into sections that logical fit the flow of installing and running your application. Here at CenturyLink, we like to use the following sections as our boilerplate and then work them from there.
* **Application Name:** Describe your application and its services (if any) here.

* **System Requirements:** Define minimum system requirements to successfully run the application.
  For Example:
  Recommended 2 Cores, 2GB of RAM

* **Setup:** Provide instructions on how to setup the application.

* **Post-Run Instructions:** Provide users of this template with any steps needed to complete the configuration of the application.

* **Port-Forwarding:** To enable access to this application outside of your Docker Host, please provide instructions.
  For Example:
  If using Virtual Box, use the following command in your local machine's terminal window to create the port forwarding rule:
  `VBoxManage controlvm panamax-vm natpf1 rule,tcp,,8997,,8080`
  or use a [link to our wiki page instructions.](https://github.com/CenturyLinkLabs/panamax-ui/wiki/How-To%3A-Port-Forwarding-on-VirtualBox)

* **Resources:** List any other resources that you may want to share.
