---
layout: post
title:      "Course Enrollment"
date:       2018-07-14 22:21:43 -0400
permalink:  course_enrollment
---

I decided to map out my object relationships as much as possible before I started to write any code for my Sinatra Project. Since I'm a beginner, this was really the first time I did all of my planning before writing any code. My goal was to create an app that allowed teachers to create courses that students could join or leave. It would have been *possible* to start setting up a database right away but I really wanted to think through all of my needs first. So, I wrote the following outline:

Teachers:

A teacher has many courses

A teacher has a name (string)

A teacher has an alma mater (string)

A teacher has years of experience (integer)

A teacher can create/edit their courses

A teacher can create/edit their teacher profile

———

Courses:

A course belongs to a teacher

A course has many enrollments

A course has many students through enrollments

A course has a subject (string)

A course has a day of the week (string)

A course has a teacher_id (integer)

———

Enrollment:

An enrollment belongs to a course

An enrollment belongs to a student

An enrollment has a course_id (integer)

An enrollment has a student_id (integer)

———

Students:

A student has many enrollments

A student has many courses through enrollments

A student has a name (string)

A student can sign up for a course but cannot edit a course

A student can create/edit their student profiles

When I sat down to start setting up the database, everything was practically already written out in my outline. This made database and model setup incredibly easy and I was able to jump into solving controller and view problems right away.

