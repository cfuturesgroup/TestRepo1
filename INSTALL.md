Notus admin project
=============

##### hosted at:  
1. **production** - [admin.notus.ua](http://admin.notus.ua)  
2. **stage** - [admin-stage.notus.com.ua](http://admin-stage.notus.com.ua)

Environment installation instructions
=============

1. Setup make, if not installed (missed on vagrant)

        apt-get install make

1. Setup nginx

        apt-get install nginx

2. Setup dotdeb.org repo php54 ([instructions here](https://ecommerce.atlassian.net/wiki/pages/viewpage.action?pageId=7012428)):  
        
        echo "deb http://packages.dotdeb.org squeeze-php54 all" > /etc/apt/sources.list.d/dotdeb.list
        echo "deb-src http://packages.dotdeb.org squeeze-php54 all" >> /etc/apt/sources.list.d/dotdeb.list
        curl -s http://www.dotdeb.org/dotdeb.gpg | apt-key add -
        apt-get update
        apt-get install php5 php5-cli php5-fpm php5-dev php5-common php5-gd php5-curl php-pear
        
3. Setup percona repo mysql ([instructions here](https://ecommerce.atlassian.net/wiki/pages/viewpage.action?pageId=7012431)):  
        
        gpg --keyserver  hkp://keys.gnupg.net --recv-keys 1C4CBDCDCD2EFD2A
        gpg -a --export CD2EFD2A | sudo apt-key add -
        sudo sh -c 'echo "deb http://repo.percona.com/apt precise main" > /etc/apt/sources.list.d/percona.list'
        sudo sh -c 'echo "deb-src http://repo.percona.com/apt precise main" >> /etc/apt/sources.list.d/percona.list'
        apt-get update
        apt-get install mysql-client mysql-server

4. If need setup mysql user with remote access rights run:

        CREATE USER 'user'@'localhost' IDENTIFIED BY 'password';
        GRANT ALL PRIVILEGES ON *.* TO 'user'@'localhost';
        FLUSH PRIVILEGES;

3. Install php mysql driver. If you have unmet dependencies - download package `libmysqlclient16` from ([http://www.ubuntuupdates.org/package/percona_server_with_xtradb/precise/main/base/libmysqlclient16](http://www.ubuntuupdates.org/package/percona_server_with_xtradb/precise/main/base/libmysqlclient16))

        apt-get install php5-mysql

4. Setup our *.deb repo and install nodejs ([instructions here](https://github.com/notusua/packages), [here](https://ecommerce.atlassian.net/wiki/pages/viewpage.action?pageId=6455308)):  

        sudo sh -c 'echo "deb [arch=amd64] http://repo.notus.com.ua/ precise main" > /etc/apt/sources.list.d/notus.list'
        apt-get update
        apt-get install -f nodejs

5. Install mongodb. No version dependencies

        apt-get install mongodb

6. Install php mongo driver

        pecl install mongo

After installation create file /etc/php5/conf.d/mongo.ini and add string `extension=mongo.so` to this file.

Project installation instructions
=============

#. Pull project (`git clone git@github.com:notusua/notus-backend.git`)
#. Pull submodules (`git submodule init`, `git submodule update`)
#. Run composer - `./composer.phar install`
#. Edit **build.prop.server** file and change properties (if need)
#. Check LEGACY_FRONTEND_PATH variable at build.prop.server - is a path to old notus website ([NOW](https://github.com/notusua/notus)). Used for files upload
   If you are not setup NOW - just run `vendor/bin/phing EmulateNOWDirs` and dirs will be created
#. Run phing deploy - `vendor/bin/phing deploy`
#. Take NGINX config, if need, from **common/config/nginx**
#. Feel lucky
