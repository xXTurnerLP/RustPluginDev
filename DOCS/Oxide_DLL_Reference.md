[[Back]](../README.md)

If you want to use functionality (class/method) from another dynamic library (DLL), you should tell Oxide loader to get the DLL's references, this is done by adding a comment in the start of the csharp file.</br>
</br>
&nbsp;&nbsp;&nbsp;&nbsp;*`.dll` extension should be ommited*
</br>
Example: `// Reference: LibraryName`</br>
