# Nodepop

[![MIT License][license-image]][license-url]

Website and API to handle second hand products built with Nodejs, Express and MongoDB.

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

Download the repository from GitHub

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
npm run install_db
```

Get a development environment running

```shell
npm run dev
```

Open your browser and go to:

+ Website &ndash; <http://localhost:3003/ads/>
+ API &ndash; <http://localhost:3003/apiv1/ads>

## Url parameters

The web app will show a list of ads and will alow to search and filter with different criteria, the available filters are:

+ `name` (optional) &ndash; a String with the name of the item *starts*
+ `forSale` (optional) &ndash;  a Boolean to determine whether the item is for sale or to buy
+ `price` (optional) &ndash; a String with four possible values i.e. 10-50, 50-, -50, 50
+ `tag` (optional) &ndash; a String with four possible values *lifestyle*, *work*, *motor* and *mobile*
+ `short` (optional) &ndash; a String that defines the sort criteria
+ `limit` (optional) &ndash; a Number that set the limit of items per page
+ `page` (optional) &ndash; a Number that set the page shown

## RESTful API services

The four primary or most-commonly-used HTTP methods are implemented:

+ GET &ndash; returning the list of ads, ads by id and tags
+ POST &ndash; creating an ad
+ DELETE &ndash; deleting an ad
+ PUT  &ndash; updating an ad

## Testing

Showing all the ads with the `tag` *lifestyle* sorted by `name` listing 3 by page:

+ Website &ndash; <http://localhost:3003/ads?limit=3&sort=name&tag=lifestyle>
  
    ![Nodepop web version](https://raw.githubusercontent.com/melquiadesvazquez/Nodepop/master/public/images/misc/web.jpg)

+ API &ndash; <http://localhost:3003/apiv1/ads?limit=3&sort=name&tag=lifestyle>
    ```json
    {  
        "success":true,
        "result":[  
            {  
                "forSale":true,
                "tags":[  
                    "lifestyle",
                    "motor"
                ],
                "_id":"5b655f650fe24a26c02c1832",
                "name":"Bicycle",
                "price":230.15,
                "picture":"http://localhost:3003/images/ads/bicycle-407215_1920.jpg",
                "created":"2018-08-04T08:10:13.432Z"
            },
            {  
                "forSale":false,
                "tags":[  
                    "lifestyle",
                    "motor"
                ],
                "_id":"5b6591428675ea3a9088b962",
                "name":"Car",
                "price":9100,
                "picture":"http://localhost:3003/images/ads/cuba-1197800_1920.jpg",
                "created":"2018-08-04T11:42:58.086Z"
            },
            {  
                "forSale":false,
                "tags":[  
                    "lifestyle"
                ],
                "_id":"5b655f650fe24a26c02c1834",
                "name":"Football ball",
                "price":11,
                "picture":"http://localhost:3003/images/ads/football-1419954_1920.jpg",
                "created":"2018-08-04T08:10:13.433Z"
            }
        ],
        "pages":[  
            {  
                "number":1,
                "url":"/apiv1/ads?limit=3&sort=name&tag=lifestyle&page=1"
            },
            {  
                "number":2,
                "url":"/apiv1/ads?limit=3&sort=name&tag=lifestyle&page=2"
            },
            {  
                "number":3,
                "url":"/apiv1/ads?limit=3&sort=name&tag=lifestyle&page=3"
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
