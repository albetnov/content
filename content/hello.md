---
title: 'Halo Dunia!'
description: 'Ini deskripsi'
---

# Mantapp

Lagi coba-coba

> oke

<!--more-->

This is the rest of content


```php
<?php

$test = new Fiber(function() {
    Fiber::suspend("hi");
    Fiber::suspend("ok");

    return "queued!"
});

$test->start(1);
// do something else
$test->resume(2);

$test->getReturn();
```
