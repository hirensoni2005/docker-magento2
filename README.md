# Docler Magento 2.4.5

## Setup Steps

1. git clone https://github.com/hirensoni2005/docker-magento2

2. Host entry
    a. For Linux/Mac edit `/etc/hosts`
    b. For Windows `C:\Windows\System32\drivers\etc\hosts`
    `127.0.0.1 chalhoub.local`

3. Run docker composer
    ```
    docker-compose -f docker-compose.yaml up --build -d
    ```

4. Access the fpm containers cli
    ```
    docker exec -it chalhoub-fpm-1 bash
    ```

5. Install and set up Magento for 1st time
    1. use composer to setup Magento. (You will require Magento access key)
    ```
    composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition=2.4.5
    
    mv project-community-edition/* .

    rm -rf project-community-edition/
    ```

    2. Install Magento command
    ```
    bin/magento setup:install \
        --base-url=http://chalhoub.local \
        --db-host=demo_db_1 \
        --db-name=magento2 \
        --db-user=magento2 \
        --db-password=magento2 \
        --admin-firstname=admin \
        --admin-lastname=admin \
        --admin-email=admin@admin.com \
        --admin-user=admin \
        --admin-password=admin123 \
        --language=en_US \
        --currency=USD \
        --timezone=America/Chicago \
        --use-rewrites=1
        --search-engine=elasticsearch7 \
        --elasticsearch-host=elasticsearch.magento2.docker \
        --elasticsearch-port=9200
    ```
6. Install Shopfinder module
    ```
    composer require hsoni/module-shopfinder
    php bin/magento module:enable Hsoni_Shopfinder
    php bin/magento setup:upgrade
    php bin/magento setup:di:compile
    php bin/magento setup:static-content:deploy
    ```
