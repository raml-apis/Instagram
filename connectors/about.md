Configuring connector

- ​In order to run this connector you must register an Instagram account.
- In order to register an Instagram application go to http://instagram.com/developer/. After clicking "Register Your Application" you will be moved to the applications management page.
- Do not forget to set redirect URI for the application. In our case Mule application is launched locally and the redirect URI belongs to the localhost domain: http://localhost:9000/codeInstagram
- Specify Client Id, Client Secret and redirect URI of your application in the Authentication tab of the HTTP Request Configuration dialog. Alternatively, you may set corresponding attributes right in the XML file.
- As Instagram expects access token to be passed as query parameter (which is not supported by MuleStudio), on your classpath you need a special Java class which stores access token so that it becomes available via expressions. The example of such class is prueba.AccessTokenManager. If you want to use another class which provides same functionality, do not forget to specify correct class names and method calls in the XML. 


Running connector

- Make a call on your Local Authorization URL which is http://localhost:9000/authorizeInstagram. You will be prompted to authorize your application. Do not forget to select your own team on this step.
- Make a call on the http://localhost:8081/instagram URL. 