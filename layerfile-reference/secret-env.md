# SECRET ENV

`SECRET ENV [secret name...]`

 The `SECRET ENV` instruction adds values from secrets to the runner's environment. 

Secrets are useful for storing sensitive information. They can hold passwords, API keys, or other private credentials. For security reasons, it is good practice to not keep this information within source code. Managing private data using secrets allows easy authentication with other services on your behalf.
 
webapp.io has a secrets manager built into the platform. This makes entering and editing secrets as simple as 1, 2, 3:

<br />

**Step 1:** Navigate to the secrets tab in your webapp.io account.
![View of secrets page in webapp.io](/docs/resources/secrets_1.png)

<br />

**Step 2:** Click ‘NEW’ in the top right corner. Follow the prompts to choose a secret name, value, and destination repository.
![View of dialogue box prompting secret creation in webapp.io](/docs/resources/secrets_2.png)

<br />

**Step 3:** All done!
![View of created secret in webapp.io](/docs/resources/secrets_3.png)

<br />

### Examples

- Use `SECRET ENV ENV_FILE` to expose your dotfile env `.env` and then use `RUN echo "$ENV_FILE" | base64 -d > ~/.env` to decode the uploaded env file to the specific location.

<br />

### Who can create secrets?


Only owners of an organization's webapp.io account can create and edit secrets. Permissions can be edited in the members tab, which can be found in the settings dropdown menu. The members tab displays all users in an organization. 

![View of webapp.io, highlighting the members tab within the settings menu](/docs/resources/secrets_4.png)

<br />

Click on the name of a user to display their permissions. Only users with owner-level access can create secrets. An organization’s owner(s) can edit permissions for other users.  

![View of how permissions are visible below a member's name in webapp.io's members tab](/docs/resources/secrets_5.png)

<br />