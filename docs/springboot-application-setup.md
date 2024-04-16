## Spring Boot Application Setup

"Now, let's proceed to the final step where we can execute the application."

1. **Navigate to Directory:** Go to the directory `Scalar-Box-Event-Log-Tool`.

2. **Configuring Box Applications:** Modify the token access token validity time in seconds in the `application.properties` file located at path `Scalar-Box-Event-Log-Tool\src\main\resources\application.properties`.

    ```properties
    # TOKEN VALIDITY IN SECONDS
    JWT_VALIDITY=180000
    # this is spring boot application jwt validity

    # BOX-ENTERPRISE-PROPERTIES
    box.enterprise-id=1098769557
    # Mention your box enterprise id here

    # WEB-APP-INTEGRATION-BOX DETAILS
    box.web-app.integration.client-id=tfuo8d6oz56i1ljpdoeoboj0ccf04tak
    box.web-app.integration.secret-id=a6xuRXxblm42vk5aeHewuYrzr5UZ80qv
    # Mention your web-app-integration details here i.e client id and secret-id you can get this details from box developer console

    # SERVER-AUTHENTICATION-BOX-APP
    box.server-authentication.client-id=l0gpw7pmwvwv5pxii5qur7stlw0pblir
    box.server-authentication.client-secret=a8JlmvmvjOrFBfoKfEGyevuXznOwb6bz
    box.server-authentication.service-acc=AutomationUser_2140095_0hbK7pz8HB@boxdevedition.com
    # Mention your server authentication app details here i.e client id and secret-id and service acc user you can get this details from box developer console
    ```

3. **Ensure you have the following prerequisites:**
   - `client.properties` file: Make sure you have the `client.properties` file.
   - `scalardb.properties` file: Ensure you have the `scalardb.properties` file.
   - `config` folder: Keep the `config` folder containing client certificates in place.

4. **Run Build:**
   - Once configurations are done, build the JAR using the Spring Boot command:

    ```bash
    $ ./gradlew build
    ```

   - After a successful build, you'll find the Spring Boot JAR at `Scalar-Box-Event-Log-Tool\build\libs\Scalar-Box-Event-Log-Tool-0.0.1-SNAPSHOT.jar`.

   - **Run the JAR:** You can run the JAR file using the following command:

    ```bash
    $ java -jar path/to/Scalar-Box-Event-Log-Tool-0.0.1-SNAPSHOT.jar
    ```

