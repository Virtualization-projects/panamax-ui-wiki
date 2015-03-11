Panamax supports adding secure (https) and insecure (http) registries, as well as registries with basic auth and certificate authority. Based on the type of registries and authentication method (if any) can determine how to set it up in Panamax.

## Insecure and Secure Registries
When installing Panamax, along with CPU and RAM allocation, you can set whether you want to connect to insecure registries. By default, this option is set to NO, which matches the default setting for the docker daemon. With this option set to NO, docker doesn't allow access to any registry with http. To allow for legacy insecure registry access, select Y during the Panamax installer setup. To change your setting, run `$ panamax reinstall` from a command prompt.

## Registries with Basic Authorization and Certificates
In order to access a registry behind basic auth, you need to add your creds for that registry to docker itself. Here are the steps:

From a command prompt where you have Panamax installed:

1. `$ panamax ssh`
2. `$ docker login -e your_email.com -u your_username -p your_password https://your_registry_endpoint`

This gives the docker daemon your creds and registry endpoint. Within Panamax, you now can use the [search command filter](https://github.com/CenturyLinkLabs/panamax-ui/wiki/How-to:-Add-Registries#running-an-image-from-a-private-repository) to directly access a secure repository. You can repeat this for as many secure registries you have.

If you are using Certificate Authorities(CAs), you can add those as well. Here are the steps:

From a command prompt where you have Panamax installed:

1. `$ panamax ssh`
2. Drop the certificate authority PEM file into /etc/ssl/certs
3. Run the update-ca-certificates script

## Running an Image from a Private Repository
Panamax now supports a search command filter to allow you to enter the absolute URI to your image in a private registry or a docker private repository. For example, to use the image `my_private_repo/ubuntu:14.04` use the `!=` command in the search control to specific the absolute path to your image:

(screenshot)
`!=my_domain.com/my_private_repo/ubuntu:14.04`

Panamax will confirm your selection by displaying the image endpoint. To run the image, simply click the Run Image button to add it to your Panamax application. Be sure to check the journal output for the service to ensure no errors occurred.

_NOTE: If your repository is behind basic auth, or needs certificate authority, be sure to read [Registries with Basic Authorization and Certificates](https://github.com/CenturyLinkLabs/panamax-ui/wiki/How-to:-Add-Registries#registries-with-basic-authorization-and-certificates) for instructions on how to add those to your Panamax install._