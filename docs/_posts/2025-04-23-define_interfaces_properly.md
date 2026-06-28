---
layout: post
title: "Define Interfaces Properly"
date: 2025-04-23 00:00:00 +0000
categories: others
---

I was recently reviewing some code and came across this interface designed for interacting with Redis:

```go
package port

import (
	"time"
)

type RedisPort interface {
	Set(key string, value string, ttl time.Duration) error
	Get(key string) (string, error)
	Del(keys ...string) error
	Exists(key string) (bool, error)
	SetJSON(key string, val any, ttl time.Duration) error
	GetJSON(key string, out any) error

	Close() error
}
```

At first glance, it gets the job done.
But there are a few points that could make it even better.

## Interfaces Should Be Descriptive

Interfaces or traits are **descriptive**, not blueprints for specific implementations.
They describe the **common behavior** of a group of types.

For example, imagine describing a *flower*.
You might mention its petals, color, and fragrance—properties common to all flowers.
But you wouldn’t describe a *Rose* as an interface, that’s too specific.

Naming an interface `RedisPort` is too specific.
It’s better to name it something more general, like `CachePort`.

## Extending Interfaces Through Composition

While Go doesn't support partial implementation of interfaces natively,
you can achieve similar results through struct embedding and method promotion.
This allows you to build richer functionality on top of a minimal interface
without forcing every implementation to carry the extra weight.

## A Cleaner, More Flexible Interface

Let's apply these improvements to create a cleaner, more robust interface:

```go
package port

import (
	"encoding/json"
	"time"
)

type Option func(*Options)

type Options struct {
	// Time-To-Live for cached item.
	TTL time.Duration
}

func WithTTL(ttl time.Duration) Option {
	return func(opt *Options) { opt.TTL = ttl }
}

type CachePort interface {
	Set(key string, value string, opts ...Option) error
	Get(key string) (string, error)
	Del(keys ...string) error
	Exists(key string) (bool, error)
}

type CacheWithJSON struct {
	CachePort
}

func (c CacheWithJSON) SetJSON(key string, val any, opts ...Option) error {
	bz, err := json.Marshal(val)
	if err != nil {
		return err
	}
	return c.Set(key, string(bz), opts...)
}

func (c CacheWithJSON) GetJSON(key string, out any) error {
	str, err := c.Get(key)
	if err != nil {
		return err
	}
	return json.Unmarshal([]byte(str), out)
}
```

With this setup, you only need to implement the core cache methods like `Set` and `Get` and
the JSON helpers come built-in at no extra effort.
Additionally, the interface is easily extensible through options,
providing the flexibility to support additional features in the future.
