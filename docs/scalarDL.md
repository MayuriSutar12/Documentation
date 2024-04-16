## Scalar DL Setup

1. **Navigate to Scalar DL Directory:** Go to the `scalardl-event-log-fetcher` folder.

2. **Modify ledger.properties:** Update the `ledger.properties` file to enable multi-database configuration. Adjust Cassandra properties such as `scalar.db.multi_storage.storages.cassandra.contact_points`, `scalar.db.multi_storage.storages.cassandra.username`, `scalar.db.multi_storage.storages.cassandra.password`.

3. In the 'fixture' folder, we have included certificates for both the ledger and auditor. If needed, you can generate new certificates for either the ledger or auditor and replace the existing ones in this folder.

4. **Start Scalar DL with Docker Compose:** To start Scalar DL with Docker Compose, use the following command:
   
    ```bash
    $ sudo docker compose -f docker-compose-ledger-auditor.yml up -d
    ```

5. **Stop Scalar DL Docker Compose:** To stop Scalar DL Docker Compose, run:
   
    ```bash
    $ sudo docker compose -f docker-compose-ledger-auditor.yml down
    ```

    Keep the Docker Compose file running.

---

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
