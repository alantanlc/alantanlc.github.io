---
layout: post
categories: blog
---

# Implementing A Workflow Framework

## Overview

1. Introduction
1. Use Cases
1. The Framework

## Introduction

TODO

## Use Cases

1. Payment processors
1. Job application processors

## The Framework

We can design a framework using `Profile`, `Stage`, `Action` and `MessageContext`.

1. A **Profile** executes a list of **Stage** in sequence
1. A **Stage** executes a list of **Action** in sequence
1. An **Action** executes the actual logic
1. A **MessageContext** is an object instance that is passed from action to action during runtime. It stores details and states of the profile flow.

### Profile

Attributes:

1. Profile Id
1. Default Profile Id
1. Description

Consider `Profile Id` as a product name derived from unique combination of product attributes.

For example, a <insert_product_name> product can be uniquely identified using <insert_attributes>:

<insert_table>

### Stage

Attributes:

1. Stage Id
1. Stage Name
1. Stage Description
1. Stage Order
1. Flow Id
   I

### Action

Attributes:

1. Task Id
1. Task Name
1. Task Description
1. Task Order
1. Stage Id
