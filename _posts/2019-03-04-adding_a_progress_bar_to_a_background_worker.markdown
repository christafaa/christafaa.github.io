---
layout: post
title:      "Adding a progress bar to a background worker"
date:       2019-03-04 13:02:21 -0500
permalink:  adding_a_progress_bar_to_a_background_worker
---

One of my Rails projects uses Sidekiq to upload XLSX files to Amazon S3 and later process those files. In order to tell the user when the job was done, I used `gem sidekiq-status` to track the progress of the background job and display updates using a progress bar. 

Right now the flow is: 
1. original XLSX files are sent to S3 via active storage
2. a sidekiq background worker begins a job and opens the original files from S3
3. the worker creates new XLSX files which are sent to S3, and deletes the original files
4. user is redirected to an index page with download links. 

[Here is a demo of all of the above happening](https://www.youtube.com/watch?v=ZfhEc7TRCes) 

