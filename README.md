## Authorization Service

The Authorization service provides basic authorization for all other datawave
microservices. Authorization is a single endpoint that returns a signed
JSON Web Token (JWT) that represents a list of [DatawaveUser](api/src/main/java/datawave/security/authorization/DatawaveUser.java)
objects. Authorization may be performed by a trusted entity (i.e., a server) on
behalf of another user or chain of servers leading to a user.

The Authorization service caches authorized users and also provides an
administrative rest API to query and manage the cache.

### Root Context

*https://host:port/authorization/v1/*

---

### Authorization API

| Method | Operation | Description                            | Request Body |
|:---    |:---       |:---                                    |:---          |
| `GET`  | authorize | Authorizes the calling user            | N/A          |
| `GET`  | whoami    | Returns details about the calling user | N/A          |


### Admin API

Users must possess the **Administrator** role to access any of the admin methods.

| Method   | Operation                | Description                             | Request Body |
|:---      |:---                      |:---                                     |:---          |
| `DELETE` | admin/evictAll           | Deletes all users from the cache        | N/A          |
| `DELETE` | admin/evictUser          | Deletes the named user from the cache   | N/A          |
| `DELETE` | admin/evictUsersMatching | Deletes users with names containing the supplied string from the authorization cache | N/A |
| `GET`    | admin/listUsers          | Shows all users in the cache            | N/A          |
| `GET`    | admin/listUser           | Retrieves the named user from the cache | N/A          |
| `GET`    | admin/listUsersMatching  | Retrieves users with names containing the supplied string from the authorization cache | N/A |

* See [AuthorizationOperations](service/src/main/java/datawave/microservice/authorization/AuthorizationOperations.java)
  class for details

---

### Getting Started

1. First, refer to [services/README](https://github.com/NationalSecurityAgency/datawave-microservices-root/blob/master/README.md#getting-started)
   for launching the config service.

2. Launch this service as follows, with the `mock` profile to leverage test PKI
   materials and associated user configuration (see [authorization-mock.yml][auth-mock-yml]).
    
   ```
   java -jar service/target/authorization-service*-exec.jar --spring.profiles.active=dev,mock
   ```

3. Ensure that the [testUser.p12][testUser] (password: *ChangeIt*) cert is
   imported into your browser, and then visit any of the following:

   * https://localhost:8643/authorization/v1/authorize
   * https://localhost:8643/authorization/v1/whoami
   * https://localhost:8643/authorization/v1/admin/listAll
   * https://localhost:8643/authorization/v1/admin/listUser?username=test
   * https://localhost:8643/authorization/v1/admin/listUsersMatching?username=test
   * Perform PUT and POST API operations with your preferred HTTP client, as desired
   
   See [sample_configuration/authorization-dev.yml][authorization-dev-yml] and configure as desired

[auth-mock-yml]:https://github.com/NationalSecurityAgency/datawave-microservices-root/blob/master/sample_configuration/authorization-mock.yml
[testUser]:https://github.com/NationalSecurityAgency/datawave-spring-boot-starter/blob/master/src/main/resources/testUser.p12
[authorization-dev-yml]:https://github.com/NationalSecurityAgency/datawave-microservices-root/blob/master/sample_configuration/authorization-dev.yml.example
