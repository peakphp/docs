---
title: Peak/Collection - Typed Structures
sb: sidebar/collection.html
---

{% include_relative _header.html %}

# Typed Structure

Typed structure is an easy to enforce types over an array or class while waiting for PHP 7.4

Here an example:

```php
use Peak\Collection\Structure\AbstractStructure;

class UserEntity extends AbstractStructure
{
    public function getStructure(): array
    {
        return [
            'id' => $this->integer(),
            'email' => $this->string(),        
            'banned' => $this->boolean(),
            'createdAt' => $this->object(\DateTime::class),
            'misc' => $this->any(),
        ];
    }
}

```