## Getting started with Panamax

This video runs through the basics of creating your first application with Panamax: 

[![Installing Panamax Video](http://img.youtube.com/vi/J0XWLiIu41Y/0.jpg)](http://www.youtube.com/watch?v=J0XWLiIu41Y)

## Finding Images for your application
The Panamax UI provides built-in [Docker Hub](https://registry.hub.docker.com/) search capability, which returns any **public** Docker Image repositories available through Docker Hub.

### Search

![Search bar](http://i.imgur.com/gZ6KcT8.png)

The search functionality can be found prominently at the top of the home page, or by clicking on **Search** at the top menu. You can search for Images by typing in your query and clicking on the **Search** button.

### Search Results

![Image Results](http://i.imgur.com/HlZfLRM.png)

Search results will contain both Panamax public **Templates** and public **Images** from Docker Hub. To see more details about an Image, you can click on the Image name, which will open a new browser window to that repositories page on Docker Hub. Since you are creating an application and not deploying an application from an existing template, you want to select **Run Image** from your chosen Image under the **Images** section. You may also select a different version of that Image by clicking the dropdown menu next to **Tag** and selecting a different tag before running it.

## Managing your application

![Manage Application](http://i.imgur.com/f7gbuVc.png)

### Categories
Categories allow you to separate and structure your application into tiers. For example, you may want to create separate database and web categories for your application. This is not required, but makes it easier to visualize the structure of your application.

### Adding additional Images to your application
If you are creating a multi-tiered application using multiple Docker images, you will need to add more Images to your application. You can do this by clicking on **Add a Service** under your chosen category. This will allow you to search again for another Image. This search will only return Images (no templates) this time.

### Application Activity Log
The application activity log provides a view into the CoreOS Journal. This is helpful in seeing a more detailed status of your application, and any messages your applications services print to stdout. You can see more lines of output by clicking on **Show Full Activity Log**.

### Other Options
- You can change the name of your application by hovering over the name (in this example it is: postgres:latest_image) and clicking on the edit icon.
- You can change the name of the first category ("Uncategorized") by hovering over the name and clicking on the edit icon.
- You can delete Services or Categories from your application by hovering over them and clicking the **X** icon.
- You can save your application as a template, making it a single click deployment in the future by clicking on **Save as Template**. See [this document](https://github.com/CenturyLinkLabs/panamax-ui/wiki/How-To%3A-Make-a-Template) for more information.
- You can rebuild or delete your app by clicking on the appropriately labeled buttons.

#### To configure your individual services, hover over it and click on the magnifying class icon.

## Configuring your Services
From this page you can expose and bind ports, link services, set environment variables, mount host volumes, and pass arguments to the services entry point.

![Configure Service](http://i.imgur.com/5twcH6V.png)

In the example image, we have linked to our other service named postgres_latest and given it an alias of `database`. We have also bound port 80 to the services port 80 to allow external access to it, and set an environment variable of `foo` with a value of `bar`. If you were to do this from the Docker command line, it would look something like:

`docker run --link postgres_latest:database -p 80:80 -e "foo=bar" ...`

If you're not familiar with these Docker options, take a look at the [Docker Run Reference](http://docs.docker.com/reference/run/). Panamax does not currently support every run option at this time, however more options are planned.

####NOTE: The "Docker Run Command" input is not a free-form input for adding your own run options. It is used only for passing arguments to your Images entry point if provided, or as a replacement CMD if your entry point is not set. See [this document](http://docs.docker.com/reference/run/#cmd-default-command-or-options) if you are still confused.

This page also provides an activity log for the individual service you are configuring. Once you have set all of your service configuration options, click **Save all changes** and the service will be reloaded. You will need to configure the options for each of your services individually, just like we have done here for this one. 

That's it! At this point your application should be up and running. Depending on your Panamax platform, you may need to open or forward ports on your firewall to access your services. See [this document](https://github.com/CenturyLinkLabs/panamax-ui/wiki/How-To%3A-Port-Forwarding-on-VirtualBox) for info on how to do this with VirtualBox.