[[Back]](../README.md)

When you try to do `harmony.load PluginName` you will see it probably wont work if you update the DLL file while the server is running (it will just be the last version)

To fix this you should follow these points:
* Your assembly should be Strongly Named (signed)
* Each time you load a new version of your mod it's AssemblyVersion should be increased (you can restart the versioning after server restart) 

###### *By my findings, this issue is caused by `Assembly.Load(byte[])` which will cache the assembly's different version assemblies until application restart (or destroying the AppDomain (which currently, without modifications of `Rust.Harmony.dll`, is the main application domain))*