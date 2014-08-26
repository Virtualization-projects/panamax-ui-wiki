In one of the next releases we will update the Panamax UI with a feature which will allow you to update your GitHub access token. Until then, here are the instructions for manually clearing your token from the database.

The instructions below will walk you through the process of gaining access to the running PMX_API container and starting up the Ruby on Rails console in order to manually update one of the records in the database. If the sound of any of that makes you feel uneasy, you may just want to wait until we've implemented this feature in the user interface.

1) SSH into the CoreOS virtual machine with the `panamax ssh` command.

2) Determine the PID for the PMX_API container by looking at the `docker inspect` output.

    core@panamax-vm ~ $ docker inspect PMX_API
    . . .
    "State": {
        "ExitCode": 0,
        "FinishedAt": "0001-01-01T00:00:00Z",
        "Paused": false,
        "Pid": 1514,
        "Running": true,
        "StartedAt": "2014-08-26T14:45:29.33270621Z"
    },
    . . .

The output in the example above has been truncated, but the value you're looking for is the one labeled "Pid" in the "State" block.

3) Use nsenter to enter the running PMX_API container. You'll need to specify the PID retrieved in the previous step as the value of the `--target` flag:

    core@panamax-vm ~ $ sudo nsenter --target 1514 --mount --uts --ipc --net --pid

4) You should now be at a prompt inside the PMX_API container. Change to the directory for the 'panamax_api' Rails application:

    root@cede0c97eb44:/# cd /usr/src/app

5) Start the rails console

    root@cede0c97eb44:/usr/src/app# rails console production

6) Clear the 'github_access_token' field on the `User` model:

    irb(main):001:0> User.instance.update_attribute('github_access_token', nil)

7) Type "exit" three times and you should be back at the prompt on your host machine.
  