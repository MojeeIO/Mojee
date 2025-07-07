---
icon: ":key:"
order: 50
---
# License Key Configuration

A license key is required to unlock all 1743 emojis supported by Mojee. Without a License Key, only the 100 most popular emoji's are supported. See [licensing](README.md#licensing) for more details.

Once a license key has been purchased, you will be sent an email with a key included. To add the license key to your Mojee project, two easy steps are required:

:::danger Please protect your license key
Please do not commit your Mojee license key to a public source code repository or share anywhere outside of your organization. It is your responsibility to protect your license key.
:::

## Steps to unlock Mojee

### :one: Set license key

In your project root, at the same level as the **.csproj** file, do you have an **appsettings.json** file? If no, please create that file, then add the following `Mojee` configuration section. Replace `your-license-key-here` with your actual license key string.

```json
{
    "Mojee": {
        "LicenseKey": "your-license-key-here"
    }
}
```

If your project already has an **appsettings.json** file, the final content might look something like the following:

```json
{
    "Logging": {
        "LogLevel": {
            "Default": "Information",
            "Microsoft": "Warning",
            "Microsoft.Hosting.Lifetime": "Information"
        }
    },
    "AllowedHosts": "*",
    "Mojee": {
        "LicenseKey": "your-license-key-here"
    }
}
```

### :two: Copy to output directory

Within your projects **.csproj** file, add the following section inside the `<Project>` node:

```xml
<ItemGroup>
    <None Update="appsettings.json">
        <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </None>
</ItemGroup>
```

The above section instructs the build process to copy the **appsettings.json** to your `/bin` folder during compilation. Your **appsettings.json** file should be deployed along side your projects compiled **.dll** files.

Mojee is now unlocked and ready for the world.
