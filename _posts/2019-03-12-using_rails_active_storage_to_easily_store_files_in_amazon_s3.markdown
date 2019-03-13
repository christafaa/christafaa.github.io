---
layout: post
title:      "Using Rails Active Storage to Easily Store Files in Amazon S3"
date:       2019-03-13 03:51:07 +0000
permalink:  using_rails_active_storage_to_easily_store_files_in_amazon_s3
---


**Setup:**
1. Declare an Amazon Active Storage service in `config/storage.yml`

```amazon:
  service: S3
  access_key_id: ""
  secret_access_key: ""
  bucket: ""
  region: "" # e.g. 'us-east-1'```
	
2. Use this Amazon service in production:
`config.active_storage.service = :amazon`
In: `config/environments/production.rb`

3. Add the AWS gem:
`gem "aws-sdk-s3", require: false`

**Attach a file to a record**
1. Use `has_one_attached` or `has_many_attached` macros in your model:
```class User < ApplicationRecord
  has_one_attached :profile_picture
end```

2. Call `profile_picture.attach` to attach a picture to an existing user:
`user.profile_picture.attach(params[:profile_picture])`

Your profile picture image is now saved to Amazon S3!
	
