# Sending Email via Java App
Simple Java Application to send emails.

## Creating a Maven Project
1. Create a folder to store the project
2. Run the command ``` mvn archetype:generate ``` to see the archetype options shared by Maven
3. Choose the default ```maven-archetype-quickstart```: *option 1947* (when this README have been written)
4. Continue choosing the *default options* until the end 
5. Define the *groupId* and *artifactId* for your project

## Including the dependencies for email sending
```
<dependency>
  <groupId>javax.mail</groupId>
  <artifactId>mail</artifactId>
  <version>1.4</version>
</dependency>
<dependency>
  <groupId>javax.activation</groupId>
  <artifactId>activation</artifactId>
  <version>1.1.1</version>
</dependency>
```

## Creating the Sender Email App
- Set the properties using the java.util.Properties class: ``` Properties properties = System.getProperties(); ```
  - Inform the smtp host that will be used (in this case, the gmail smtp): ``` properties.setPropertiy("mail.smtp.host", "smtp.gmail.com") ```
  - Enable secure connection: ``` properties.put("mail.smtp.starttls.enable", "true") ```
  - Enable authentication: ``` properties.put("mail.smtp.auth", "true") ```
  
- As the authentication was defined, now we have to configure the session authentication for email sending:
  ``` 
  Session session = Session.getDefaultInstance( properties, new Authentication(){
      @Override
      protected PasswordAuthentication getPasswordAuthentication(){
        return  new PasswordAuthentication([your_email], [your_password]);
      }
  }); 
  ```
  
- With the session authentication, now we can start the sending email process:
  - Using the javax.mail.internet MimeMessage class, we pass the session authenticated to its class: 
``` MimeMessage message = new MimeMessage(session); ```
  - Setting the attributes for the email message:
    - Who is sending the email: ``` message.setFrom(new InternetAddress([email_sender])); ```
    - Who is receiving the email: ``` message.addRecipient(Message.RecipientType.TO, new InternetAddress([email_recipient])); ```
    - What is the subject: ``` message.setSubject([write_the_email_subject_here]); ```
    - What is the body of the email: ``` message.setText([write_the_email_body_here]); ```
  - Sending the email:
    ``` 
    Trasport.send(message);
    System.out.println("Done! Email sent.");
    ```
> Obs: All these steps must be surrounded by a *try/catch block* to treat the *AddressException* and *MessagingException* that can occur.    
    
