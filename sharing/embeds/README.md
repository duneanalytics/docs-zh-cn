---
description: Embeds allow you to enjoy beautiful, updating dune charts across the web!
---

# Embeds

## Using Embeds

**Screenshots are boring and tech of the past.**

Instead of using static screenshots in varying forms of quality, Dune has a native embed function that works across most platforms.&#x20;

You can generate embed links by clicking on any query title and selecting the embed function in the top right corner.

![generating an embedd link](<../../.gitbook/assets/2021-11-01 14-30-33.gif>)



### Parameterized embeds

Embed links also work with parameterized queries, but it is a bit tricky to get them to work:

The embed link that gets generated does not include the necessary parameters yet, even if you have ran the query with it. We are already working on automating the link generation, but for now the  is the only way for us to handle this.

For now you need to manually prefix the parameter link with the parameters.

The syntax for this is:

`link?name_of_parameter1=xxxx&?name_of_parameter2=yyyy&...` __&#x20;

An example of this would be:

`>https://dune.xyz/embeds/118220/238460/aa002dd3-f9e2-4d63-86c8-b765569306c6NFT?address=0xff9c1b15b16263c61d017ee9f65c50e4ae0113d7&rolling_n_trades=500`\
``

### Embeds across different platforms

Here is a couple of exemplary use cases for Dune embeds:

{% content-ref url="discord.md" %}
[discord.md](discord.md)
{% endcontent-ref %}

{% content-ref url="twitter.md" %}
[twitter.md](twitter.md)
{% endcontent-ref %}

{% content-ref url="mirror.xyz.md" %}
[mirror.xyz.md](mirror.xyz.md)
{% endcontent-ref %}

{% content-ref url="webpages.md" %}
[webpages.md](webpages.md)
{% endcontent-ref %}



### Known Issues

Unfortunately, embeds do not work in a couple of fairly popular sites, this includes:

* substack
* medium
* gitbook
