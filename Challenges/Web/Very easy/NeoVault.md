# NeoVault
At the beginning of the engagement I performed full web reconnaissance and created an account to authenticate into the application. After logging in, I monitored traffic with a proxy to enumerate API endpoints and application behavior.
<img width="923" height="500" alt="image" src="https://github.com/user-attachments/assets/3421d384-120a-4d57-9efa-16166c077720" />

While analyzing API calls I observed responses containing transaction data that referenced a different user. The payload revealed a user identifier in the application logs and responses:
User ID's
```bash
6915261a7f59f1fe63f682d9
```
This indicated the existence of discoverable resource identifiers that could be probed further.
<img width="927" height="449" alt="image" src="https://github.com/user-attachments/assets/2f587974-cc87-4641-ba2c-f3fe35ab51ac" />

The application’s public endpoints were predominantly namespaced under /api/v2. As a heuristic, I attempted the equivalent legacy path /api/v1 because older API versions often lack later security hardenings. When POSTing to /api/v1/transactions/download-transactions the server required a single parameter _id (error when omitted: {"message":"_id is not provided"}). This confirmed the endpoint performs input validation but did not yet indicate ownership checks.
<img width="958" height="481" alt="image" src="https://github.com/user-attachments/assets/62b2d676-86e9-4e36-813e-dae1309ebe1e" />

I submitted the observed identifier as the _id parameter to the legacy endpoint and successfully downloaded the associated transactions file. The downloaded artifact contained contextual data including a 
username: user_with_flag. 
This demonstrated that the legacy endpoint returned resource data for arbitrary provided IDs.
<img width="915" height="466" alt="image" src="https://github.com/user-attachments/assets/6fd29c08-087e-497d-9094-9eeda6f7a0e4" />

Using the application’s transfer functionality I created a transaction directed to user_with_flag. The server acknowledged the action and returned a new resource identifier:
User ID's
```bash
6915261a7f59f1fe63f682de
```
So I repet the steps before
<img width="907" height="451" alt="image" src="https://github.com/user-attachments/assets/b534b879-0aa0-4ce7-91ad-57faea4fa4df" />

Got the flag!
<img width="945" height="240" alt="image" src="https://github.com/user-attachments/assets/e00896af-ffaf-485f-9a39-e390f3c3b6c3" />
