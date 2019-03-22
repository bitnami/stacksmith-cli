# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [cmd/stacksmith/api/stackerfile.proto](#cmd/stacksmith/api/stackerfile.proto)
    - [DeploymentTemplates](#stacksmith.cli.DeploymentTemplates)
    - [ImageSpec](#stacksmith.cli.ImageSpec)
    - [ImageSpec.Files](#stacksmith.cli.ImageSpec.Files)
    - [Platform](#stacksmith.cli.Platform)
    - [Stackerfile](#stacksmith.cli.Stackerfile)
    - [Stackerfile.BuildOptions](#stacksmith.cli.Stackerfile.BuildOptions)
    - [Stackerfile.Files](#stacksmith.cli.Stackerfile.Files)
    - [Stackerfile.Region](#stacksmith.cli.Stackerfile.Region)
    - [UserScripts](#stacksmith.cli.UserScripts)
  
  
  
  

- [pkg/builder/api/base.proto](#pkg/builder/api/base.proto)
    - [BaseImageAws](#builder.BaseImageAws)
    - [BaseImageAzure](#builder.BaseImageAzure)
    - [BaseImageDocker](#builder.BaseImageDocker)
    - [BaseImages](#builder.BaseImages)
  
  
  
  

- [Scalar Value Types](#scalar-value-types)



<a name="cmd/stacksmith/api/stackerfile.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## cmd/stacksmith/api/stackerfile.proto



<a name="stacksmith.cli.DeploymentTemplates"></a>

### DeploymentTemplates
Deployment templates to upload.
<br>
They can be interpreted as directory names.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| docker | [string](#string) |  |  |
| aws | [string](#string) |  |  |
| azure | [string](#string) |  |  |






<a name="stacksmith.cli.ImageSpec"></a>

### ImageSpec
Description of an image that should be built


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  | Name of the image, must be unique within the Stackerfile |
| files | [ImageSpec.Files](#stacksmith.cli.ImageSpec.Files) |  |  |
| baseImages | [builder.BaseImages](#builder.BaseImages) |  | Base images to use for each target, overriding the default base images if desired |
| userData | [string](#string) |  | User-data to pass to the image when booting it during building Can be used to disable behaviour of the image that should only happen when running the image for real, not just running it to use as a base image for building |






<a name="stacksmith.cli.ImageSpec.Files"></a>

### ImageSpec.Files



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| userUploads | [string](#string) | repeated | list of local files to be uploaded. if empty, the content of the ./user-uploads dir will be used instead. |
| userScripts | [UserScripts](#stacksmith.cli.UserScripts) |  |  |






<a name="stacksmith.cli.Platform"></a>

### Platform



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| target | [string](#string) |  | target name, compatible with our API&#39;s target field. TODO(mkm): make platform/target an orthogonal choice, so you can build docker on azure and docker on aws. |






<a name="stacksmith.cli.Stackerfile"></a>

### Stackerfile



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| appId | [string](#string) |  | fully qualified application ID. In the spirit of github Web URL&lt;-&gt;repo equivalance, the application ID is identified via the URL shown to the user when navigating to the app, namely: https://stacksmith.bitnami.com/foobar/apps/64b0e9e0-c22e-0135-dac4-1a802b3213ed <br> The actual stacksmith base address can be omitted in case the public SaaS endpoint is used, namely: foobar/apps/64b0e9e0-c22e-0135-dac4-1a802b3213ed <br> The full URL is useful when users want to point to a private stacksmith instance and allows us to avoid teaching the users to use a flag for that purpose. But the main reason for supporting the full URL to identify the application is that it makes it dead simple for people to copy paste. |
| appName | [string](#string) |  | If appId doesn&#39;t exist, use this name when creating it. |
| appVersion | [string](#string) |  | if empty, reuses the one of the last revision. |
| files | [Stackerfile.Files](#stacksmith.cli.Stackerfile.Files) |  |  |
| platforms | [Platform](#stacksmith.cli.Platform) | repeated | platforms for which this this application is designed to be built. |
| publishingRegions | [Stackerfile.Region](#stacksmith.cli.Stackerfile.Region) | repeated |  |
| buildOptions | [Stackerfile.BuildOptions](#stacksmith.cli.Stackerfile.BuildOptions) |  |  |
| baseImages | [builder.BaseImages](#builder.BaseImages) |  |  |
| images | [ImageSpec](#stacksmith.cli.ImageSpec) | repeated |  |






<a name="stacksmith.cli.Stackerfile.BuildOptions"></a>

### Stackerfile.BuildOptions



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| assumeBaseImageFreshness | [bool](#bool) |  | If true, the build process will assume the base image is already kept up to date by the upstream issuer and stacksmith won&#39;t force the update of system packages upon build. |
| userData | [string](#string) |  | Cloud providers enable custom userdata to be present when launching an instance, and packer supports this both for aws (userData, with the contents available at http://169.254.169.254/latest/user-data) and azure (customDataFile, with the contents ending at /var/lib/waagent/CustomData ). This will not currently be available during a docker build, but we could do so in the future ensure that the Stacksmith build scripts put the contents in a common location across all platforms. This field should be per-image when we move to multi-image builds. |






<a name="stacksmith.cli.Stackerfile.Files"></a>

### Stackerfile.Files



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| userUploads | [string](#string) | repeated | list of local files to be uploaded. if empty, the content of the ./user-uploads dir will be used instead. |
| userScripts | [UserScripts](#stacksmith.cli.UserScripts) |  |  |
| deploymentTemplates | [DeploymentTemplates](#stacksmith.cli.DeploymentTemplates) |  |  |






<a name="stacksmith.cli.Stackerfile.Region"></a>

### Stackerfile.Region



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| region | [string](#string) |  |  |






<a name="stacksmith.cli.UserScripts"></a>

### UserScripts
Local user scripts to be uploaded.
<br>
If empty, the manually uploaded scripts are used/preserved.
This allows applications to be gradually moved to CI, having
one team manage the scripts (via the UI) and another team just
hook in the application binary upload.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| build | [string](#string) |  |  |
| boot | [string](#string) |  |  |
| run | [string](#string) |  |  |





 

 

 

 



<a name="pkg/builder/api/base.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## pkg/builder/api/base.proto



<a name="builder.BaseImageAws"></a>

### BaseImageAws



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| amiId | [string](#string) |  |  |
| sshUsername | [string](#string) |  |  |






<a name="builder.BaseImageAzure"></a>

### BaseImageAzure



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| id | [string](#string) |  | This is the resource id of a managed image. Something like: /subscriptions/&lt;subscription id&gt;/resourceGroups/&lt;resource group name&gt;/providers/Microsoft.Compute/images/&lt;image name&gt; |






<a name="builder.BaseImageDocker"></a>

### BaseImageDocker



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  |  |






<a name="builder.BaseImages"></a>

### BaseImages
BaseImages contains per-target base images to be used when creating an image.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| docker | [BaseImageDocker](#builder.BaseImageDocker) |  |  |
| aws | [BaseImageAws](#builder.BaseImageAws) |  |  |
| azure | [BaseImageAzure](#builder.BaseImageAzure) |  |  |





 

 

 

 



## Scalar Value Types

| .proto Type | Notes | C++ Type | Java Type | Python Type |
| ----------- | ----- | -------- | --------- | ----------- |
| <a name="double" /> double |  | double | double | float |
| <a name="float" /> float |  | float | float | float |
| <a name="int32" /> int32 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. | int32 | int | int |
| <a name="int64" /> int64 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. | int64 | long | int/long |
| <a name="uint32" /> uint32 | Uses variable-length encoding. | uint32 | int | int/long |
| <a name="uint64" /> uint64 | Uses variable-length encoding. | uint64 | long | int/long |
| <a name="sint32" /> sint32 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s. | int32 | int | int |
| <a name="sint64" /> sint64 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s. | int64 | long | int/long |
| <a name="fixed32" /> fixed32 | Always four bytes. More efficient than uint32 if values are often greater than 2^28. | uint32 | int | int |
| <a name="fixed64" /> fixed64 | Always eight bytes. More efficient than uint64 if values are often greater than 2^56. | uint64 | long | int/long |
| <a name="sfixed32" /> sfixed32 | Always four bytes. | int32 | int | int |
| <a name="sfixed64" /> sfixed64 | Always eight bytes. | int64 | long | int/long |
| <a name="bool" /> bool |  | bool | boolean | boolean |
| <a name="string" /> string | A string must always contain UTF-8 encoded or 7-bit ASCII text. | string | String | str/unicode |
| <a name="bytes" /> bytes | May contain any arbitrary sequence of bytes. | string | ByteString | str |

