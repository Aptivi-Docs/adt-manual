---
description: Welcome to ADT!
icon: hand-wave
metaLinks:
  alternates:
    - https://app.gitbook.com/s/ocz2kFW2PRMWNpWEjhbc/
---

# Welcome

Aptivi Development Toolkit (ADT) is a drop-in replacement for the older legacy script-based development toolkit for your projects. It allows you to implement a set of functions, such as build, clean, and test functions, without having to write platform-specific scripts, with Python as the scripting language to achieve cross-platform support.

{% hint style="info" %}
If you're still planning to use the legacy script-based system, please consult [this manual](https://app.gitbook.com/o/fj052nYlsxW9IdL3bsZj/s/ocz2kFW2PRMWNpWEjhbc/).
{% endhint %}

To get started, follow the instructions in the below page:

{% content-ref url="build-system/getting-started.md" %}
[getting-started.md](build-system/getting-started.md)
{% endcontent-ref %}

To learn more about the structure of the system, take a look here:

{% content-ref url="build-system/structure.md" %}
[structure.md](build-system/structure.md)
{% endcontent-ref %}

***

## <mark style="color:$primary;">Release history</mark> <a href="#release-history" id="release-history"></a>

Below is the release history of the toolkit:

{% updates format="full" %}
{% update date="2026-06-09" %}
## <mark style="color:$primary;">v1.2.0.2</mark>

<mark style="color:yellow;">Fixed JSON loading causing localizations to have garbled characters</mark>
{% endupdate %}

{% update date="2026-06-02" %}
## <mark style="color:$primary;">v1.2.0.1</mark>

<mark style="color:yellow;">Fixed project root obtaining</mark>
{% endupdate %}

{% update date="2026-05-31" %}
## <mark style="color:$primary;">v1.2.0.0</mark>

<mark style="color:green;">Added fetch and pull actions</mark>

<mark style="color:green;">Added project path switch</mark>

<mark style="color:green;">Added commit and push switch</mark>

<mark style="color:yellow;">General improvements and bug fixes</mark>
{% endupdate %}

{% update date="2026-05-10" %}
## <mark style="color:$primary;">v1.1.0.0</mark>

<mark style="color:green;">Added custom actions</mark>

<mark style="color:green;">Added listing remote branches</mark>

<mark style="color:yellow;">Attribute and type validation is now enforced</mark>

<mark style="color:yellow;">Push progress is now clearer</mark>

<mark style="color:yellow;">Fixed submodule changes causing git push fails</mark>

<mark style="color:yellow;">General improvements and bug fixes</mark>
{% endupdate %}

{% update date="2026-04-24" %}
## <mark style="color:$primary;">v1.0.2.0</mark>

<mark style="color:yellow;">Improved project handling</mark>

<mark style="color:yellow;">Help now shows for non-standalone actions when requested with</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">`-h`</mark>
{% endupdate %}

{% update date="2026-04-15" %}
## <mark style="color:$primary;">v1.0.1.0</mark>

<mark style="color:green;">Added</mark> <mark style="color:green;"></mark><mark style="color:green;">`-d`</mark> <mark style="color:green;"></mark><mark style="color:green;">or</mark> <mark style="color:green;"></mark><mark style="color:green;">`--dry`</mark> <mark style="color:green;"></mark><mark style="color:green;">options for creating commits</mark>

<mark style="color:green;">Added</mark> <mark style="color:green;"></mark><mark style="color:green;">`--self`</mark> <mark style="color:green;"></mark><mark style="color:green;">global parameter</mark>

<mark style="color:yellow;">Improved push output on completion</mark>
{% endupdate %}

{% update date="2026-03-05" %}
## <mark style="color:$primary;">v1.0.0.0</mark>

The initial version of the toolkit has been released!
{% endupdate %}
{% endupdates %}
