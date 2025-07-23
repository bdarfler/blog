---
title: "Monit alerts sent from a Dreamhost email account"
datePublished: Wed Sep 01 2010 14:35:11 GMT+0000 (Coordinated Universal Time)
cuid: cmdgets71000102jl654a7mgx
slug: monit-alerts-sent-from-a-dreamhost-email-account-966823843317

---

This little snippet of configuration took way to long to figure out so I’m sharing it in hopes of saving others the trouble. If you are using monit and trying to send alerts from a Dreamhost email account you will need to use the following settings.

set mailserver mail.your.host.name port 465

username "monit@your.host.name"

password "password"

using SSLV3

with timeout 15 seconds

set alert alerts@your.host.name

with mail-format { from: monit@your.host.name }

It turns out that monit and Dreamhost don’t get along on the default port 587. The solution [according to Dreamhost](http://wiki.dreamhost.com/Secure_E-mail) is to use port 465. This works as long as you use SSLv3 instead of TLSv1. The other important part of this configuration is making sure you set the from address in your mail format. Otherwise monit tries to send from monit@<hostname> which in most cases is not a fully qualified name and will cause Dreamhost to fail.