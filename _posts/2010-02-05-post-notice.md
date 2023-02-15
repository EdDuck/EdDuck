---
title: "Docker tricks"
categories:
  - Docker
tags:
  - Docker
  - Tricks
---

# **How to Stop All Docker Containers**

To stop all Docker containers, simply run the following command in your terminal:

`docker kill $(docker ps -q)`

## **How It Works**

The `docker ps` command will list all *running* containers. The `-q` flag will only list the IDs for those containers. Once we have the list of all container IDs, we can simply run the `docker kill` command, passing all those IDs, and they’ll all be stopped!

# **How to Remove All Docker Containers**

If you don’t just want to stop containers and you’d like to go a step further and remove them, simply run the following command:

`docker rm $(docker ps -a -q)`

## **How It Works**

We already know that `docker ps -q` will list all running container IDs. What is the `-a` flag? Well that will return *all* containers, not just the running ones. Therefore, this comman will remove all containers (including both running and stopped containers).

# **How To Remove All Docker Images**

To remove all Docker images, run this command:

`docker rmi $(docker images -q)`

## **How It Works**

`docker images -q` will list all image IDs. We pass these IDs to `docker rmi` (which stands for remove images) and we therefore remove all the images.


A notice displays information that explains nearby content. Often used to call attention to a particular detail.

When using Kramdown `{: .notice}` can be added after a sentence to assign the `.notice` to the `<p></p>` element. 

**Changes in Service:** We just updated our [privacy policy](#) here to better service our customers. We recommend reviewing the changes.
{: .notice}

**Primary Notice:** Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer nec odio. [Praesent libero](#). Sed cursus ante dapibus diam. Sed nisi. Nulla quis sem at nibh elementum imperdiet.
{: .notice--primary}

<div class="notice--primary" markdown="1">
**Primary Notice with code block:** Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer nec odio. [Praesent libero](#). Sed cursus ante dapibus diam. Sed nisi. Nulla quis sem at nibh elementum imperdiet.

```html
<html>
  <body>Some body.<body>
</html>
```
</div>

**Info Notice:** Lorem ipsum dolor sit amet, [consectetur adipiscing elit](#). Integer nec odio. Praesent libero. Sed cursus ante dapibus diam. Sed nisi. Nulla quis sem at nibh elementum imperdiet.
{: .notice--info}

**Warning Notice:** Lorem ipsum dolor sit amet, consectetur adipiscing elit. [Integer nec odio](#). Praesent libero. Sed cursus ante dapibus diam. Sed nisi. Nulla quis sem at nibh elementum imperdiet.
{: .notice--warning}

**Danger Notice:** Lorem ipsum dolor sit amet, [consectetur adipiscing](#) elit. Integer nec odio. Praesent libero. Sed cursus ante dapibus diam. Sed nisi. Nulla quis sem at nibh elementum imperdiet.
{: .notice--danger}

**Success Notice:** Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer nec odio. Praesent libero. Sed cursus ante dapibus diam. Sed nisi. Nulla quis sem at [nibh elementum](#) imperdiet.
{: .notice--success}

Want to wrap several paragraphs or other elements in a notice? Using Liquid to capture the content and then filter it with `markdownify` is a good way to go.

```html
{% raw %}{% capture notice-2 %}
#### New Site Features

* You can now have cover images on blog pages
* Drafts will now auto-save while writing
{% endcapture %}{% endraw %}

<div class="notice">{% raw %}{{ notice-2 | markdownify }}{% endraw %}</div>
```

{% capture notice-2 %}
#### New Site Features

* You can now have cover images on blog pages
* Drafts will now auto-save while writing
{% endcapture %}

<div class="notice">
  {{ notice-2 | markdownify }}
</div>

Or you could skip the capture and stick with straight HTML.

```html
<div class="notice">
  <h4>Message</h4>
  <p>A basic message.</p>
</div>
```

<div class="notice">
  <h4>Message</h4>
  <p>A basic message.</p>
</div>
