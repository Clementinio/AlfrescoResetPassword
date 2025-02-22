# AlfrescoResetPassword Perso

Alfresco Reset Password add-on is a great thing that allows you to solve missing capability of Alfresco to reset passwords for users who forgot owns password without contacting Administrator.

# Features
 - Sends email notification with a confirmation link
 - Reset password link will be expired after 24h
 - It works fine even if multitenancy feature is enabled

# User's reset password manual

 - Click Forgot Password button
 
 ![](readme_resources/1-config.png)
 
 - Enter username
 
 ![](readme_resources/4-config.png)
 
 - You'll receive email notification with link for changing password
 
 ![](readme_resources/2-config.png)
 
 - Enter your new password and confirm it.
 
 ![](readme_resources/3-config.png)

# Compatibility was tested on versions:
 - Alfresco 5.0d
 - Alfresco 5.1
 - Alfresco 5.2
 - Alfresco 6.0.7-ga
 - Alfresco 6.2.0-ga

# How to build
### The following tools must be installed:
- Java11
- Maven3

### Required settings
  - You have to configure Alfresco Outbound SMTP in {ALFRESCO LOCATION}/tomcat/shared/classes/alfresco-global.properties.
  ```sh
   mail.host={mail host}
   mail.port={mail port}
   mail.username={mail username}
   mail.password={mail password}
   mail.protocol=smtps
   mail.smtps.starttls.enable=true
   mail.smtps.auth=true
   mail.from.enabled=false
   mail.from.default={default sending email}
   ```
- It is highly recommended to change secure salt property as well that us used in token generation.
To generate new one use any BCrypt hash generator. First 29 characters is salt
```fs.passreset.salt=```

### Build steps
1. Clone repo
```sh
$ git clone https://github.com/FlexSolution/AlfrescoResetPassword.git
```
2. Compile repo amp 

```sh
$ cd ResetPasswordAddon/resetPassword
$ mvn clean package
```

3. Compile share amp

```sh
$ cd ../resetPassword-share
$ mvn clean package
```

# How to deploy add-on to Alfresco

1. Compile AMPs for alfresco and share how described above
2. Stop Alfresco
3. Copy file ResetPasswordAddon/resetPassword/target/reset-password-repo-${version}.amp into ${Alfresco_HOME}/amps folder
4. Copy file ResetPasswordAddon/resetPassword-share/target/reset-password-share-${version}.amp into ${Alfresco_HOME}/amps_share folder
5. Apply AMPs by executing command from command line
```sh
$ cd ${Alfresco_HOME}/bin
$ apply_amps.sh -force
```
6. Start Alfresco

##How to setup dev environment:
You need to have installed docker and configured to use without root privileges https://docs.docker.com/install

1. Add **Remote** Debugger cofiguration.
![](readme_resources/1-configuration.png)

1. Configure **Before launch: Tool window** and point at **start.sh** or **start.bat** as executable script.
(files within _docker folder should be executable).
![](readme_resources/4-configuration.png)
![](readme_resources/2-configuration.png)

1. Point at **logs.txt** as logs file.
![](readme_resources/3-configuration.png)

All alfresco configuration placed in **docker-compose.yml**. You can place additional configuration there if you need

To stop docker containers use **stop-all.sh** or **stop-all.bat** script.
**start.sh|bat** script also stops containers and starts again

To debug Share app java code create new **Remote** configuration. Use port 5006

