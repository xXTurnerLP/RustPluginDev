[[Back]](../README.md)

# What is sandbox mode?
Prevents oxide loader from loading "potentially malicious" libraries/code into the final assembly.

# Limitations
## The following namespaces are blacklisted (You can't add them with `using XXXXX;`)
1. `Harmony`
1. `Mono.CSharp`
1. `Mono.Cecil`
1. `Oxide.Core.ServerConsole`
1. `ServerFileSystem`
1. `System.IO`
1. `System.Net`
1. `System.Xml`
1. `System.Reflection.Assembly`
1. `System.Reflection.Emit`
1. `System.Threading`
1. `System.Runtime.InteropServices`
1. `System.Diagnostics`
1. `System.Security`
1. `System.Timers`

## Other limitaions
* You cannot use P/Invoke (No native access)

# How to disable sandbox mode
You need to create a file named `oxide.disable-sandbox` in directory `server\RustDedicated_Data\Managed` (server restart is required after that)