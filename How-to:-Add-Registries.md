Panamax supports adding secure (https) and insecure (http) registries, as well as registries with basic auth and certificate authority. Based on the type of registries and authentication method (if any) can change determine how to set it up in Panamax.

## Insecure and Secure Registries
When installing Panamax, along with CPU and RAM allocation, you are also prompted for whether you what to connect to insecure registries. By default, this option is set to NO, which matches the default setting for the docker daemon. With this option set to no, one can only access registries with https. To allow for legacy registry access, you can select Y in order to allow connect to insecure registries. You can be re-prompted for this option each time you run a `$ panamax init` or `$ panamax reinstall`. 

## Registries with Basic Authorization and Certificates

## Running an Image from a Private Repository
Panamax now supports a search command filter to allow you to enter the absolute URI to your image in a private registry. For example, to use the image `my_private_repo/ubuntu:14.04` use the `!=` command in the search control to specific the absolute path to your image:

(screenshot)
`!=my_domain.com/my_private_repo/ubuntu:14.04`

Panamax will confirm your selection by displaying the image endpoint. To run the image, simply click the Run Image button to add it to your Panamax application. Be sure to check the journal output for the service to ensure no errors occurred.

_NOTE: If your your repository is behind basic auth, or needs certificate authority, be sure to read [Registries with Basic Authorization and Certificates](#) for instructions on how to add those to your Panamax install._

## Running an Image from a Docker Private Repository

