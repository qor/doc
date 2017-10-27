# Deploy To Production

When deploying application, many people prefer binary deploys, which has many pros, that's also what we recommended way to deploy your application.

But [as you know](/admin/theming_and_customization.md#view-paths), the default AssetFS implemention of QOR Admin when looking up templates is from filesystem, that means, those tempaltes must exist on production servers, or it will cause template not found error.

We build [QOR Bindatafs](https://github.com/qor/bindatafs) to help that, it could compile QOR templates into binary, refer its [README](https://github.com/qor/bindatafs#README) for further help.
