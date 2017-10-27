# Deploy To Production

When deploying an application, many people prefer binary deploys, which has many pros, that's also what our recommended way to deploy your application.

But [as you know](/admin/theming_and_customization.md#view-paths), the default AssetFS implementation of QOR Admin when looking up templates is from the filesystem, that means, those templates must exist on production servers, or it will cause `template not found` error.

We build [QOR Bindatafs](https://github.com/qor/bindatafs) to help that, it could compile QOR templates into binary, refer its [README](https://github.com/qor/bindatafs#README) for further help.
