---
categories:
  - programming
  - php
tags:
  - php
  - apache
layout: post
title: PHP memory_limit stress testing
created: 1356166459
---

I wrote this code a couple of years ago that I found it was very useful when I was troubleshooting PHP memory limit settings. The script essentially creates one huge array until it runs out of memory.

```bash
<?php
ini_set('display_errors', true);
 
while (1) {
        echo 'Hello' . nl2br("\n");
        $array = array(1,2);
        while(1) {
                $tmp = $array;
                $array = array_merge($array, $tmp);
                echo memory_get_usage() . nl2br("\n");
                flush();
                sleep(1);
        }
}
?>
```
