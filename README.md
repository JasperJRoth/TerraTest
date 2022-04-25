# TerraTest

This is a repository containing a docker-compose file to help demonstrate a bug that I am currently experiencing.

- [x] I have checked that I am on the latest version of Terra.
- [x] I have searched the github issue tracker for similar issues, including
  closed ones.
- [x] I have made sure that this is not a bug with another mod or plugin, and it
  is Terra that is causing the issue.
- [x] I have checked that this is an issue with Terra and not an issue with the
  pack I am using.
- [x] I have attached a copy of the `latest.log` file
- [x] I have filled out and provided all the appropriate information.

## Issue Description

Terra creates world files on server shutdown, even if those worlds have been deleted.

### Steps to reproduce

1. Clone the latest version of the repository
2. Have docker installed
3. Run ```docker-compose up -d```
4. Wait until server is ```Done (14.195s)! For help, type "help"```
5. Create file named .env in the same directory as the docker-compose.yml file
6. Place the following text into the .env file
  ```RCON_PASSWORD=[any password you want to use]```
8. Run ```docker exec -i terra-test rcon-cli```
9. Enter the following commands into the console:<br />
  (This creates the world for testing)<br />
  ```mv create testworld NORMAL -g Terra:MOON_BASIC```<br />
  (This deletes the world - there will be no world files after this step)<br />
  ```mv delete testworld```<br />
  (Confirms deletion)<br />
  ```mv confirm```<br />
  (Stops the server - there will be world files after this step)<br />
  ```stop```<br />
10. Check minecraft-data folder

### Expected behavior

If a world has been deleted or unloaded from bukkit, terra should not create further world files.

### Actual behavior

After a world has been deleted or unloaded from bukkit, terra creates an additional file. The file created has the following path:
```[WorldName]/gaea/chunks.bin```

### Full stacktrace
No stacktrace is created, there are thread dumps during world creation, but I think this is due to server not responding during world creation.

### Additional details
I am using multiverse-core to test this, because I am pretty sure bukkit does not have a built in command to delete world files. I do not think multiverse-core is the issue here, because multiverse-core is able to delete the files. Terra seems to add a file after the world is deleted, before server shutdown.
