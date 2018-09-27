# Nodepop

[![MIT License][license-image]][license-url]

Website and API to handle second hand products built with Nodejs, Express and MongoDB.

Live demo available on https://melnodepop.herokuapp.com/

## Prerequisites

You need to install the following software on your machine before running this project:

1. Node.js  &ndash; <https://github.com/joyent/node/wiki/Installation>
2. npm (Node package manager)  &ndash; <https://github.com/isaacs/npm>
3. MongoDB (Database)  &ndash; <https://www.mongodb.com/download-center#community>

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

Download the repository from GitHub

```shell
git clone https://github.com/melquiadesvazquez/Nodepop.git
```

Install de project with NPM

```shell
cd Nodepop
npm install
```

Copy .env.example to .env and review the values to match your preferences.

```shell
mv .env.example .env
```

Initialize the database

```shell
npm run installDB
```

Get a development environment running

```shell
npm run dev
```

Open your browser and go to:

+ Website &ndash; http://localhost:3003/ads/
+ API &ndash; http://localhost:3003/apiv1/ads

## Url parameters

The web app will show a list of ads and will alow to search and filter with different criteria, the available filters are:

+ `name` (optional) &ndash; a String with the name of the item *starts*
+ `forSale` (optional) &ndash;  a Boolean to determine whether the item is for sale or to buy
+ `price` (optional) &ndash; a String with four possible values i.e. 10-50 (range), 50- (>=), -50 (<=), 50 (=)
+ `tag` (optional) &ndash; a String with four possible values *lifestyle*, *work*, *motor* and *mobile*
+ `sort` (optional) &ndash; a String that defines the sort criteria
+ `limit` (optional) &ndash; a Number that set the limit of items per page
+ `page` (optional) &ndash; a Number that set the page shown

## RESTful API services

The four primary or most-commonly-used HTTP methods are implemented:

+ GET &ndash; returning the list of ads, ads by id and tags
+ POST &ndash; creating an ad
+ DELETE &ndash; deleting an ad
+ PUT  &ndash; updating an ad

## Testing

Check the live demo [here](https://melnodepop.herokuapp.com/)

Showing all the ads listing 6 by page (default):

+ Website &ndash; <http://localhost:3003/ads>
  
    ![Nodepop web version](https://raw.githubusercontent.com/melquiadesvazquez/Nodepop/master/public/images/misc/web.jpg)

Showing all the ads with `price` 50 or less with the `tag` *lifestyle* sorted by `name` listing 3 by page and sorted by price:

+ API &ndash; <http://localhost:3003/apiv1/ads?limit=3&tag=lifestyle&price=-50&sort=price>
    ```json
    {  
        "success":true,
        "result":[  
            {  
                "forSale":false,
                "tags":[  
                    "lifestyle"
                ],
                "_id":"5b6706e27641bd4ad078b3f7",
                "name":"Football ball",
                "price":11,
                "picture":"http://localhost:3003/images/ads/football-1419954_1920.jpg",
                "created":"2018-08-05T14:17:06.540Z"
            },
            {  
                "forSale":true,
                "tags":[  
                    "lifestyle",
                    "work"
                ],
                "_id":"5b6706e27641bd4ad078b3ff",
                "name":"Shoes",
                "price":40,
                "picture":"http://localhost:3003/images/ads/shoes-1638873_1920.jpg",
                "created":"2018-08-05T14:17:06.542Z"
            },
            {  
                "forSale":false,
                "tags":[  
                    "lifestyle"
                ],
                "_id":"5b6706e27641bd4ad078b3fd",
                "name":"TV",
                "price":45.5,
                "picture":"http://localhost:3003/images/ads/tv-1844964_1920.jpg",
                "created":"2018-08-05T14:17:06.541Z"
            }
        ],
        "pages":[  
            {  
                "number":1,
                "url":"/apiv1/ads?limit=3&tag=lifestyle&price=-50&sort=price&page=1"
            },
            {  
                "number":2,
                "url":"/apiv1/ads?limit=3&tag=lifestyle&price=-50&sort=price&page=2"
            }
        ]
    }
    ```
Listing an ad by ID:

+ API &ndash; <http://localhost:3003/apiv1/ads/:id>
  
Listing the number of products per tag:

+ API &ndash; <http://localhost:3003/apiv1/ads/tags>
    ```json
    {  
        "success":true,
        "result":[  
            {  
                "lifestyle":8
            },
            {  
                "work":4
            },
            {  
                "motor":3
            },
            {  
                "mobile":3
            }
        ]
    }
    ```

Creating a product:

+ Place the product's picture on `Nodepop/public/images/ads`
+ API (POST x-www-form-urlencoded) &ndash; <http://localhost:3003/apiv1/ads/>
    ```json
    {
        "forSale": false,
        "tags": [
            "lifestyle",
            "mobile"
        ],
        "name": "iPhone 6S",
        "price": 50,
        "picture": "iphone-410311_1280.jpg"
    }
    ```

Deleting a product:

+ Get the ID of the product
+ API (DELETE x-www-form-urlencoded) &ndash; <http://localhost:3003/apiv1/ads/:id>
+ The deleted product will be returned
    ```json
    {  
        "success": true,
        "result": {  
            "forSale":false,
            "tags": [  
                "lifestyle",
                "mobile"
            ],
            "_id": "5b66fde6e5a6094094d329fb",
            "name": "iPhone 6S",
            "price": 50,
            "picture": "http://localhost:3003/images/ads/iphone-410311_1280.jpg",
            "created": "2018-08-05T13:38:46.273Z",
            "__v": 0
        }
    }
    ```

Updating a product:

+ API (PUT x-www-form-urlencoded) &ndash; <http://localhost:3003/apiv1/ads/:id>
    ```json
    {
        "tags": [
            "mobile"
        ]
    }
    ```

## Built with

+ [Nodejs](https://nodejs.org/) - JavaScript run-time environment
+ [Express](http://expressjs.com/) - Web application framework for Node.js
+ [MongoDB](https://www.mongodb.com/) - NoSQL document-oriented database program
+ [Bootstrap](https://getbootstrap.com/) - Front-end framework with CSS templates as well as JavaScript extensions

## Restrictions

+ Both the website and API will shown a maximum of 6 items per page with 24 in total, this can be changed on the .env file


## License

[MIT][license-url]


[license-image]: http://img.shields.io/badge/license-MIT-blue.svg?style=flat
[license-url]: LICENSE
