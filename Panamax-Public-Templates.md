##What are the Panamax Public Templates

The [Panamax Public Template repository](https://github.com/CenturyLinkLabs/panamax-public-templates) contains Panamax templates available to all users of Panamax. They have been crafted with certain standards for the image and template, to ensure their functionality. Learn more about what we think makes a good image and template below.

##What we think makes a good image
* **Optimize your image!**- See <a href="http://www.centurylinklabs.com/optimizing-docker-images/">Brian DeHamer's blog post</a> on great ways to keep your image size under control. His section on chaining commands is worth the read! You'd be surprised how unwieldy an image can get! Downloading 200MB vs. 2GB is a BIG difference on time and needed storage space.

* **Know your sources!**- It's good to use official images from Docker when you can; however, they tend not to always publish a Dockerfile and you'll need to hunt for the repo and source (many of the official images can be found here <a href="https://github.com/docker-library">https://github.com/docker-library</a>). It's always a good idea to look at the project repo directly to see what exactly is in it and if it's right for you as a starting point.

* **Be specific!** - When creating your own image, try to be as specific as you can when it comes to applications or packages. This gives you more control of the functionality of the image and gives a better success rate if someone takes your image and tries to build it themselves. You don't want to get the latest of an app only to have it work when you pulled it, but break a couple months later due to an incompatibility. Opt for a specific version rather than just 'stable'. Image creation should also be about reliability and consistency.

* **Avoid too many supporting files** - if you can get the script information in `RUN` commands, do so. It is not always necessary to have supporting files if you leverage the full complement of commands in a Dockerfile. [ENTRYPOINT](https://docs.docker.com/reference/builder/#entrypoint) and [ONBUILD](https://docs.docker.com/reference/builder/#onbuild) are some powerful (and under utilized) commands. If scripts are used, avoid hardcoding environmental variables or referencing the link generated environmental variables that need the alias name. This avoids making the image too rigid and which can limit its reusability.

* **Be flexible!** - Panamax allows you to define port expose (--expose), port binding (-p), volumes (-v), and environmental variables (-e) and even commands, directly in the UI and lets you save them in a template for future use. This give more flexibility rather than needing to specify that in your Dockerfile. If you `EXPOSE 80` in a Dockerfile, that image has it permanently (as well as any images generated referencing the image with `FROM`!)

* **Know its History!** - If you use `FROM` in your Dockerfile, its going to come with some baggage. Everything from ports, environmental variables, volumes and the CMD can be propagated down to your image. Your image could be the last in line of several references. It's always best to do your homework on exactly what you are inheriting. `docker inspect <image_id>` can you give you those details but doesn't give you insight into how an image was constructed, especially if it doesn't have a published Dockerfile.

* **Publish your Dockerfile!** - The Docker Registry has a lot of goodness, but one badness is that Dockerfiles are not always published, making image construction a black hole. Here at CenturyLinkLabs, we always publish our Dockerfile and supporting files into a Github repo and then link it to the Docker Registry using their automated builds feature. If you find an image with no Dockerfile, check out [Brian DeHamer's dockerfile-from-image](https://registry.hub.docker.com/u/centurylink/dockerfile-from-image/), a great tool to reverse engineer an image.</li>

##What we think makes a good template.

* **Documentation is essential!** - We generally break our documentation down to a few sections : System Requirements - How much horsepower will it need? Setup - Do environmental variables need to be added? Post-Run Instructions - Is an username and password needed to access the app? Resources - Have a blog about your template?

* **Be specific, again!** - We try to avoid the 'latest' tag when we can for the images we reference in our templates. This cuts down on unexpected changes that could break your app.

* **Whats in a name?** - Make sure your name and description describes your template well. We use this for searching and it lets others know what your app provides.

* **Use the power of Panamax!** - Avoid hardcoded Docker specifics in your images. The power of Panamax is the ability to make these configurations flexible.

##Submit a Template!
1. Fork the official the official Panamax Template Repository from [https://github.com/CenturyLinkLabs/panamax-contest-templates](https://github.com/CenturyLinkLabs/panamax-contest-templates) to your personal GitHub account.

2. Press **Save as Template** in Panamax for your new application and save your template to your forked panamax-contest-templates repository.

3. Once your template is ready, go to GitHub and submit a pull request from your forked panamax-contest-templates repository.