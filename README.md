# s3-static-site

Allows static websites deployment to Amazon S3 website buckets using Capistrano.

## Hosting your website with Amazon S3

S3 provides special website enabled buckets that allows you to deliver website pages directly from S3.
The most important difference is that theses buckets serves an index document (ex. index.html) whenever a user specifies the URL for the root of your website, or a subfolder. And you can point your domain name directly to the S3 bucket cname.

To learn how to setup your website bucket, see [Amazon Documentation](http://docs.amazonwebservices.com/AmazonS3/latest/dev/index.html?HostingWebsiteQS1.html).

## Getting started

Setup capistrano, create a public folder and set your S3 bucket configurations in `deploy.rb`.

    capify .
    mkdir public
    touch config/deploy.rb #see config instructions bellow
    cap deploy

And voilà!

### Configuring deployment

s3-static-site overrides the default Capistrano recipes for Rails projects with its own simple s3 publishing scripts.

    # config/deploy.rb
    require 's3-static-site'

    set :bucket, "www.cool-website-bucket.com"
    set :access_key_id, "CHANGETHIS"
    set :secret_access_key, "CHANGETHIS"

If you want to deploy to multiple buckets, have a look at 
[Capistrano multistage](https://github.com/capistrano/capistrano/wiki/2.x-Multistage-Extension)
and  configure a bucket per stage configuration.

#### S3 write options

s3-static-site sets files `:content_type` and `:acl` to `:public_read`, add or override with :

    set :bucket_write_options, {
        cache_control: "max-age=94608000, public"
    }

See aws-sdk [S3Object.write doc](http://rubydoc.info/github/amazonwebservices/aws-sdk-for-ruby/master/AWS/S3/S3Object#write-instance_method) for all available options.

## Rendering with HAML and SASS
  
On deployment, `.haml` and `.sass` are generated and uploaded (source .haml and .sass are not uploaded).

## Copyright 

Copyright (c) 2012 Josh Delsman & miomoba, Inc. See LICENSE.txt for details.