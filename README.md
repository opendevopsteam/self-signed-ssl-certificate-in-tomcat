# self-signed-ssl-certificate-in-tomcat
🔑 Self-signed SSL certificate in Tomcat

Self-signed SSL certificate and add into Java truststore.

**Step 1:**

*Generate the SSL certificate by running the following command*

`$ keytool -genkey -keyalg RSA -alias tomcat -keystore selfsigned.jks -validity 365 -keysize 2048`

- The number of days that indicates 365 is for which the certificate will be valid.
- The selfsigned.jks is the key store file.
- The aforementioned command exports the certificate that alias is tomcat.

**By default, the key store password is set to changeit; you can use the keytool utility -storepasswd option to change it to something more secure.**

**Step 2:**

*The aforementioned command has some default sets, and also prompts the developer to enter additional information as shown below:*

```
What is your first and last name?
  [Unknown]:  Orestis Pantazos
What is the name of your organizational unit?
  [Unknown]:  Open DevOps
What is the name of your organization?
  [Unknown]:  opendevops.dev
What is the name of your City or Locality?
  [Unknown]:  Athens
What is the name of your State or Province?
  [Unknown]:  Attiki
What is the two-letter country code for this unit?
  [Unknown]:  GR
Is CN=localhost, OU=Profile Software, O=profilesw.com, L=Athens, ST=Greece, C=GR correct?
  [no]:  yes
```

**Step 4:**

*Verify the contents of keystore by running the given command*

`$ keytool -list -v -keystore selfsigned.jks`

- The keytool utility -list option lists the contents of a specified key store file.
- The -v option tells the keytool utility to display certificate fingerprints in human-readable form.

**Step 5:**

*Import the certificate into your application’s trust store. The keytool utility -import option installs a certificate from a certificate file in a specified trust store.*

`$ keytool -import -noprompt -trustcacerts -alias tomcat -file selfsigned.cer -keystore "%JAVA_HOME%/jre/lib/security/cacerts" -storepass changeit`

**Step 6:**

*The certificate is already completed and can be used by Apache Tomcat server container by using the following configuration*

```xml
<Connector port="8080" protocol="HTTP/1.1"
           redirectPort="443"
           disableUploadTimeout="false"/>
<Connector port="443" protocol="HTTP/1.1" SSLEnabled="true"
           maxThreads="150" scheme="https" secure="true"
           keystoreFile="selfsigned.jks" keystorePass="<password>"
           clientAuth="false" acceptCount="100"/>
```

**Step 7:**

SSL port of the current instance is already for connection in `https://localhost:443/`.

> READY 
