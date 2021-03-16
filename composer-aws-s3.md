# Using s3 bucket to serve composer packages

## Change code of original publisher

Example: the hubspot/api-client package did not work for php 7.0

Create a fork of the hubspot/api-client in your own repository

Change the files, commit and create a new tag (or delete the original tag in the fork and reuse it)

Make a release (don't know if that's actually needed, but can't hurt)



Let's say that you've created tag 2.8.1, and continue to the next step

##Clone satis repository to create composer json files

    git clone git@github.com:composer/satis.git

Go to satis folder

Create satis.json: 
    {
        "name": "<company-name>/packages",
        "homepage": "https://<company-url>",
        "repositories": [
            {
                "type": "vcs",
                "url": "https://github.com/<repository-fork>/hubspot-api-php"
            }
        ],
        "require": {
            "hubspot/api-client": "2.8.1"
        },
        "require-all": false,
        "require-dependencies": true,
        "require-dev-dependencies": true,
        "archive": {
            "directory": "dist"
        }
    }

Run:

    php bin/satis build satis.json web/

Note: satis needs php 7.3

Edit web/packages.json and change "metadata-url" to:

    "metadata-url": "/<bucket-name>/composer/p2/%package%.json",

Now copy to the s3 bucket:

aws s3 cp --recursive web/ s3://<bucket-name>/composer/ --profile <profile> --acl public-read —region=eu-west-1
