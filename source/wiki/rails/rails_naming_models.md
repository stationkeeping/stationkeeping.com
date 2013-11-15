---
title: "Rails Naming Models"
---

Join models should, wherever possible, represent a domain object. They are a thing so should be named using a noun.

* Author > Authorship < Book
* Reader > Readership < Book
* Reader > Reading < Article
* Person > Membership < Group
* Person > Attendance < Meeting
* Student > Enrolment < SchoolClass
* Employee > Assignment < Project
* Person > Registration < Conference
* Contributor > Constributorship > Project
* Patient > Appointment > Doctor
* Supplier > Account > Account History

Polymorphic associations are an aspect of a thing and should be named after the aspect:

* Taggable
* Assignable
* Rateable
* Sharable

