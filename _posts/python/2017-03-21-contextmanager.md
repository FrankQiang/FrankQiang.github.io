---
layout: post
title: 上下文环境 
category: Python
tags: Python
keywords: Python
---

#### 日志打印 

```python

import logging


def t():
    logging.debug("Some debug data")
    logging.error("Error log here")
    logging.debug("More debug data")


t()

# ERROR:root:Error log here

```

#### 在上下文环境下日志打印 

```python

import logging
from contextlib import contextmanager
    
    
def t():
    logging.debug("Some debug data")
    logging.error("Error log here")
    logging.debug("More debug data")


@contextmanager
def debug_logging(level):
    logger = logging.getLogger()
    old_level = logger.getEffectiveLevel()
    logger.setLevel(level)
    try:
        yield
    finally:
        logger.setLevel(old_level)


with debug_logging(logging.DEBUG):
    print("Inside:")
    t()          # 日志打印的级别为 DEBUG
print("After:")  # 日志打印的级别重新恢复为ERROR
t()


# Inside:
# DEBUG:root:Some debug data
# ERROR:root:Error log here
# DEBUG:root:More debug data
# After:
# ERROR:root:Error log here

```
