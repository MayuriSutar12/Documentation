## Configuration of the Backend Spring Boot Application for ScalarDL

1. **Navigate to Directory:** Go to the directory `Scalar-Box-Event-Log-Tool-Setup`.

2. **Client Certificates:**
   - Within the 'Config' folder, you'll find client (application) certificates named 'client.pem' and 'client-key.pem'.
   - If necessary, you can generate new certificates.
   - Ensure to replace the existing certificates with the new ones in the 'config' folder also in the Scalar-Box-Event-Log-Tool module.

3. **Build the Project:** Import the code and build the project using Java 8, ensuring that the '.class' files are generated.

4. **Configure client.properties:** Modify the 'client.properties' file to update the ledger host and auditor host configurations.

5. **Execute Registration Command:** While inside the 'Scalar-Box-Event-Log-Tool-Setup' folder, execute the following command:

    ```bash
    $pwd
    ```

    This command will give you the exact path of the current system.

6. **Register Client Certificates, Contracts, and Functions:** After obtaining the path, run the following command:

    ```bash
    $SCALAR_SDK_HOME=/path/to/Scalar-Box-Event-Log-Tool-Setup ./register
    ```

    This command will register the client certificates, contracts, and functions to ScalarDL.