## QOR application structure overview

This image shows the high level structure of a [QOR](https://github.com/qor/qor) application.

![QOR application structure](QOR application infrastucture.png)

The core of [QOR](https://github.com/qor/qor) is the "Engine". It is kind of a "resource box". Because every content is a resource and a resource is accessed/managed by a common interface using HTTP standard methods. The data is structured and represented in the [QOR Admin](https://github.com/qor/admin) interface as a resource.

We add advanced features to the engine by plugins. Like `worker+exchange` for data import/export. `media_library` for file uploading.

There're also some helper for front-end, Like `i18n` for internationalization. `cache` for webpage cache.

For the front-end, We leave full flexible to developers which means you can have fully customisable UI and Router.

OK, Keep this image in your mind, let's go through each part of [QOR](https://github.com/qor/qor) deeper.
