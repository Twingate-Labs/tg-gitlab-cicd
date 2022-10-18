# Twingate Gitlab Pipeline CICD
The project provides an example of how to setup Twingate headless client in your Gitlab CICD Pipeline.

## Setup
1. Create [Twingate service account key](https://www.twingate.com/docs/services/) and assign resources to the service account based on your requirement
2. Base64 encode the Twingate service account key. For example in Linux, execute the following command
    ```
    echo '{"version": "1",
      "network": "xxxxx.twingate.com",
      "service_account_id": "xxxxx",
      "private_key": "-----BEGIN PRIVATE KEY-----\nxxxxx\n-----END PRIVATE KEY-----",
      "key_id": "xxxxx",
      "expires_at": "xxxxx",
      "login_path": "/api/v2/headless_node/login"
    }' | base64 -w 0   
    ```
3. Set the encoded service account key as Gitlab CI/CD variables
   * Go to Setting
   * CI/CD
   * Variables -> Expand
   * Add Variable (TWINGATE_SERVICE_ACCOUNT)
     * Key: TWINGATE_SERVICE_ACCOUNT
     * Value: encoded service account key
     * Type: Variable
     * Environment Scope: Based on your requirement
     * Protect Variable: recommend setting to true
     * Mask Variable: recommend setting to true
   * Add Variable (TWINGATE_PRIVATE_TEST_RESOURCE)
     * Key: TWINGATE_PRIVATE_TEST_RESOURCE
     * Value: A private resource address in your Twingate tenant which the service account has access to, e.g. `http://172.30.2.229:3000`
     * Type: Variable
     * Environment Scope: Based on your requirement
     * Protect Variable: recommend setting to true
     * Mask Variable: recommend setting to true
4. copy the sections `before_script` and `test` from .gitlab-ci.yml into yours
5. Confirm the valid response is returned by the resource in the CI/CD logs