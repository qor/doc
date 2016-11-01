## QOR application structure overview

This image shows the high-level structure of a typical [QOR](https://github.com/qor/qor) application.

![QOR application structure](QOR application infrastucture.png)

The core of [QOR](https://github.com/qor/qor) is the "Engine". It is kind of a "resource box". Because every piece of data (aka content) is a resource and a resource is accessed/managed by a common interface using HTTP standard methods. The data is structured and represented in the [QOR Admin](https://github.com/qor/admin) interface as a resource.

We add advanced features to the engine by way of plugins. For example, `worker+exchange` for data import/export, or `media_library` for file uploading.

There are also some helpers for front-end needs, such as `i18n` for Internationalization, and `cache` for webpage caching.

For the front-end, we leave that work/code to developers (fully flexible) which means you can have fully customisable UI and Router without limitations.

OK, keep this image in your mind and let's go through each part of [QOR](https://github.com/qor/qor) in more depth...
