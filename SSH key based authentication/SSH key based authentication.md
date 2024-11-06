
## SSH key based authentication

> [!NOTE]
> - Controlnode shoud be linux
> - If an command executed in managed node from controlnode, if the managed node wil be linux it communicate/login through `ssh`, and for windows it communicate through `winrm`.
> -  To install a package in managed node from controlnode it must have `authentication`, `authorization`. And also authenticate by a user and user must be sudo privileged.
> -  Mostly root login denied for security purpose.
> -  Reguler user can't install any package, can't write any configuration file.

Now, we login as `alex` user 
