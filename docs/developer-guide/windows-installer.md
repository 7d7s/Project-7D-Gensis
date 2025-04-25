# Creating a Windows Installer

This guide provides instructions on how to create a Windows installer for the NextGen MedTech Platform.

## Prerequisites

*   [WiX Toolset](https://wixtoolset.org/) - A free and open-source toolset for creating Windows installers.
*   [.NET SDK](https://dotnet.microsoft.com/download) - Required for building the installer project.

## Steps

1.  **Install WiX Toolset:**
    *   Download and install the WiX Toolset from the official website.
    *   Ensure that the WiX Toolset binaries are added to your system's PATH environment variable.

2.  **Create a WiX Project:**
    *   Create a new directory for your installer project (e.g., `installer`).
    *   Create a new WiX source file (e.g., `Product.wxs`) in the installer directory.

3.  **Define the Product Information:**
    *   Open the `Product.wxs` file and define the product information, such as the product name, version, manufacturer, and installation directory.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <Wix xmlns="http://schemas.microsoft.com/wix/2006/wi">
        <Product Id="*" Name="NextGen MedTech Platform" Version="1.0.0.0" Manufacturer="Your Company" UpgradeCode="YOUR-UPGRADE-CODE">
            <Package InstallerVersion="200" Compressed="yes" InstallScope="perMachine" />

            <MajorUpgrade DowngradeErrorMessage="A newer version of [ProductName] is already installed." />
            <Media Id="1" Cabinet="media1.cab" EmbedCab="yes" />

            <Directory Id="TARGETDIR" Name="SourceDir">
                <Directory Id="ProgramFilesFolder">
                    <Directory Id="INSTALLFOLDER" Name="NextGen MedTech Platform">
                        <Component Id="ProductComponent" Guid="YOUR-GUID">
                            <File Source="$(var.YourProject.TargetPath)" KeyPath="yes" />
                        </Component>
                    </Directory>
                </Directory>
            </Directory>

            <Feature Id="ProductFeature" Title="NextGen MedTech Platform" Level="1">
                <ComponentRef Id="ProductComponent" />
            </Feature>
        </Product>
    </Wix>
    ```

4.  **Configure the File Source:**
    *   Replace `$(var.YourProject.TargetPath)` with the actual path to your application's executable file.
    *   Update the `Guid` attribute with a unique GUID for your component. You can generate a GUID using online tools or Visual Studio.
    *   Replace `YOUR-UPGRADE-CODE` with a unique GUID for your product's upgrade code.

5.  **Create a Build Script:**
    *   Create a build script (e.g., `build.bat`) to compile the WiX source file and create the installer package.

    ```batch
    candle.exe Product.wxs
    light.exe Product.wixobj -ext WixUIExtension
    ```

6.  **Build the Installer:**
    *   Run the build script from the command line.
    *   This will generate an MSI installer package in the installer directory.

7.  **Test the Installer:**
    *   Run the MSI installer package to install the NextGen MedTech Platform on your system.
    *   Verify that the application is installed correctly and that all features are working as expected.

## Customization

*   You can customize the installer by adding additional features, such as:
    *   Adding a custom user interface.
    *   Adding support for different languages.
    *   Adding custom actions to be performed during installation.

## Conclusion

By following these steps, you can create a Windows installer for the NextGen MedTech Platform using the WiX Toolset. This will allow users to easily install and use your application on their Windows systems.
