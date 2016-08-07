---
layout: post
title: Symfony2笔记
category: 技术
tags: Symfony2
keywords: Symfony2, 笔记
---



```
# Clear cache
sudo php app/console cache:clear --env=prod
sudo php app/console cache:clear --env=dev

# Create Bundle
php app/console generate:bundle --namespace=Test/TestBundle

# Create Database
php app/console doctrine:database:create

# Create Class for entity
php app/console doctrine:generate:entities TestBundle

# Create Table
php app/console doctrine:schema:update --force

# Create CRUD Controller
php app/console doctrine:generate:crud --entity=TestTestBundle:Test --route-prefix=test --with-write --format=yml

# Generate public link
php app/console assets:install web --symlink

# Edit Route
php app/console router:debug

sudo chmod -R 777 app/cache
sudo chmod -R 777 app/logs
sudo setfacl -dR -m u::rwX app/cache app/logs
```
