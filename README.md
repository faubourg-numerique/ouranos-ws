# ouranos-ws

## Getting Started

> Please note that ouranos-ws is currently under development and some unintended uses could cause issues, such as deleting an entity that another depends on.

### Prerequisites

- [Debian](https://www.debian.org/)
- [Docker Engine](https://docs.docker.com/engine/install/debian/)
- [Docker Compose](https://docs.docker.com/compose/install/) (included with Docker Engine)
- [Certbot](https://certbot.eff.org/instructions?ws=other&os=debianbuster)
- [PHP](https://www.php.net/downloads/) `sudo apt install php-fpm`
- [Composer](https://getcomposer.org/download/)
- [Git](https://git-scm.com/) `sudo apt install git`
- Zip & Unzip `sudo apt install zip unzip`
- Wget `sudo apt install wget`

> PHP and Composer are only used to generate the encryption key, what can be done on another machine. If you are doing it on the same machine, make sure you are using a version of PHP that does not include Apache, to avoid a conflict with the use of ports 80 and 443.

### Installation

- Create type A DNS entries for the following subdomains

    - api.ouranos-ws.example.com
    - app.ouranos-ws.example.com
    - data-models.ouranos-ws.example.com
    - idm.example.com

- Generate HTTPS certificates

    - `sudo certbot certonly --standalone -d api.ouranos-ws.example.com`
    - `sudo certbot certonly --standalone -d app.ouranos-ws.example.com`
    - `sudo certbot certonly --standalone -d data-models.ouranos-ws.example.com`
    - `sudo certbot certonly --standalone -d idm.example.com`

- Clone this repository

    ```
    git clone https://github.com/faubourg-numerique/ouranos-ws.git
    ```

- Enter the cloned repository

    ```
    cd ./ouranos-ws
    ```

- Create and edit the docker compose environment file

    ```
    cp ./.env.example ./.env
    ```

- Create and edit the reverse proxy config file

    ```
    cp ./nginx.conf.example ./nginx.conf
    ```

- Create and edit the ouranos-ws config files

    ```
    cp ./config/ouranos-ws-api/.env.example ./config/ouranos-ws-api/.env
    cp ./config/ouranos-ws-app/config.js.example ./config/ouranos-ws-app/config.js
    ```

- Clone the ouranos-ws data model repository

    ```
    git clone https://github.com/faubourg-numerique/ouranos-ws-data-model.git ./data-models/ouranos-ws/1
    ```

- Set Apache as the owner of the **data-models** directory

    ```
    sudo chown www-data:www-data -R ./data-models
    ```

- Install the ouranos-ws modules

    - Download the modules

    ```
    wget -O "./ouranos-ws-api-data-services-module-1.0.1.zip" https://github.com/faubourg-numerique/ouranos-ws-api-data-services-module/archive/refs/tags/1.0.1.zip
    ```

    ```
    wget -O "./ouranos-ws-api-wot-module-1.0.0.zip" https://github.com/faubourg-numerique/ouranos-ws-api-wot-module/archive/refs/tags/1.0.0.zip
    ```

    ```
    wget -O "./ouranos-ws-api-dsc-module-1.0.0.zip" https://github.com/faubourg-numerique/ouranos-ws-api-dsc-module/archive/refs/tags/1.0.0.zip
    ```

    - Extract the modules

    ```
    unzip ./ouranos-ws-api-data-services-module-1.0.1.zip
    ```

    ```
    unzip ./ouranos-ws-api-wot-module-1.0.0.zip
    ```

    ```
    unzip ./ouranos-ws-api-dsc-module-1.0.0.zip
    ```

- Generate an encryption key
    > All sensitive data (passwords, private keys, etc) is stored encrypted.

    - Install the dependencies

        ```
        composer install
        ```

    - Generate an encryption key

        ```
        php ./vendor/defuse/php-encryption/bin/generate-defuse-key
        ```

    - Update the `ENCRYPTION_KEY` variable of the **./config/ouranos-ws-api/.env** file

- Generate a Google Maps API key (optional)

    > In order for the maps to work, a Google Maps API key must be provided.

    - Go to [**Google Maps Platform** → **Keys & Credentials**](https://console.cloud.google.com/google/maps-apis/credentials)

    - Click on **CREATE CREDENTIALS** → **API key**

        ![](images/be678877-7604-4b1c-a1e8-b78f73e60842.png)

        ![](images/eabd9743-a2c2-45cc-91c9-fea778e38990.png)

    - Update the `window.googleMapsApiKey` variable of the **./config/ouranos-ws-app/config.js** file

- Create a Google Service Account (optional)

    > To be able to import and export entities from Google Sheets, a Google service account must be provided.

    - Go to the [**Google Cloud Platform** → **IAM & Admin** → **Service Accounts**](https://console.cloud.google.com/iam-admin/serviceaccounts)

    - Click on **CREATE SERVICE ACCOUNT**

        ![](images/c8f7515f-5c9c-49b1-9fe6-3db70d83e9f2.png)

    - Follow the steps to create the service account

        ![](images/9fe47d42-54c4-429a-a6bf-cbdfbc897fa1.png)

        ![](images/6de4ce1a-41a2-4f0e-a69d-e0b14664f62a.png)

        ![](images/e69eb6af-1d1f-42a4-919c-29b1be740e88.png)

    - Click on **Add Key** → **Create new key** in the **Keys** tab of the service account

        ![](images/0fd60344-c1c4-4ec8-9a0c-1de2b44cc7b8.png)

    - Select the **JSON** key type and click **CREATE**

        ![](images/f4766098-6f5d-49d6-83db-50e4a35bd9ad.png)

    - Rename the downloaded file to **service-account.json** and move it to this directory

        ![](images/f06f66f0-4637-4e4e-a696-3eaa716e4f2a.png)

    - Update the `window.googleServiceAccount` variable of the **./config/ouranos-ws-app/config.js** file with the service account email

- Configure the identity manager

    - Start the services

        ```
        sudo docker compose up -d
        ```

    - Log in using the credentials provided in **.env**

        ![](images/97857790-4b98-40d3-889d-8d7456313086.png)

    - Create an application

        ![](images/e7a0751d-3fc1-406b-bbaa-78d07ade3eba.png)

        ![](images/c4507ff8-9275-4e30-b275-864a05d70877.png)

        ![](images/ab0081bf-5ac2-4925-8991-dd25a162b1e5.png)

    - Create an **Admin** role

        ![](images/72bbf4dd-544b-4d8a-a008-fb50496488c5.png)

        ![](images/49d99c7f-3f13-4fd6-8b18-86484e6a2bcf.png)

    - Select the **Admin** role

        ![](images/4bac22ca-6541-4f80-9ae3-f1493bc8332f.png)

    - Create a permission

        ![](images/f75caf35-d2e2-461e-9bfd-7f282b6fa4d1.png)

        ![](images/6bc9a291-a84d-4f59-8daf-df746eb5496e.png)

    - Repeat the operation for the **POST**, **PUT**, **PATCH** and **DELETE** methods

    - Select all created permissions

        ![](images/3ce015af-4160-496f-8151-6b37c3239a24.png)

    - Save the roles

        ![](images/fb564c35-edd3-462d-af83-7a98c2c9ffa3.png)

    - Deploy the OAuth2 Credentials section

        ![](images/3f0b10a7-0d2c-4c62-a615-e4a2d8e6295f.png)

        ![](images/da07810c-cfd1-4946-ab50-dc74ffa3ba39.png)

    - Update the `AUTHORIZATION_IDENTITY_MANAGER_APP_ID` variable of the **./config/ouranos-ws-api/.env** file with the application client id

    - Update the `window.identityManagerClientId` variable of the **./config/ouranos-ws-app/config.js** file with the application client id

    - Open the authorization section

        ![](images/d29a14b4-75a8-461c-84a9-deebee750e2b.png)

    - Add the role **Admin** to the **admin** user and save

        ![](images/36ea0958-f088-467e-acd1-182195a8c1df.png)

    - Stop the services

        ```
        sudo docker compose down
        ```

## Usage

- Start the services

    ```
    sudo docker compose up -d
    ```

- Stop the services

    ```
    sudo docker compose down
    ```

## Documentation (outdated)

- [API documentation](https://faubourg-numerique.gitbook.io/ouranos-ws-api/)
- [APP documentation](https://faubourg-numerique.gitbook.io/ouranos-ws-ui/)
