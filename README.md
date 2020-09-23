<div align="center">

## Compression / decompression : intrinsic to VB\.Net 2005


</div>

### Description

Zip Rar Tga etc intrinsic compression and decompression, extremely fast, 1.09mb -&gt; 17kb in less than a second, not my code, as it is plainly just a part of the .net framework
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[George E\.](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/george-e.md)
**Level**          |Beginner
**User Rating**    |4.7 (28 globes from 6 users)
**Compatibility**  |VB\.NET
**Category**       |[Algorithims](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/algorithims__10-29.md)
**World**          |[\.Net \(C\#, VB\.net\)](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/net-c-vb-net.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/george-e-compression-decompression-intrinsic-to-vb-net-2005__10-3912/archive/master.zip)





### Source Code

```
Imports System.IO
Imports System.IO.Compression
Public Class ZipUtil
 Public Sub CompressFile(ByVal sourceFile As String, ByVal destinationFile As String)
  ' make sure the source file is there
  If File.Exists(sourceFile) = False Then
   Throw New FileNotFoundException
  End If
  ' Create the streams and byte arrays needed
  Dim buffer As Byte() = Nothing
  Dim sourceStream As FileStream = Nothing
  Dim destinationStream As FileStream = Nothing
  Dim compressedStream As GZipStream = Nothing
  Try
   ' Read the bytes from the source file into a byte array
   sourceStream = New FileStream(sourceFile, FileMode.Open, FileAccess.Read, FileShare.Read)
   ' Read the source stream values into the buffer
   buffer = New Byte(CInt(sourceStream.Length)) {}
   Dim checkCounter As Integer = sourceStream.Read(buffer, 0, buffer.Length)
   ' Open the FileStream to write to
   destinationStream = New FileStream(destinationFile, FileMode.OpenOrCreate, FileAccess.Write)
   ' Create a compression stream pointing to the destiantion stream
   compressedStream = New GZipStream(destinationStream, CompressionMode.Compress, True)
   'Now write the compressed data to the destination file
   compressedStream.Write(buffer, 0, buffer.Length)
  Catch ex As ApplicationException
   MessageBox.Show(ex.Message, "An Error occured during compression", MessageBoxButtons.OK, MessageBoxIcon.Error)
  Finally
   ' Make sure we allways close all streams
   If Not (sourceStream Is Nothing) Then
    sourceStream.Close()
   End If
   If Not (compressedStream Is Nothing) Then
    compressedStream.Close()
   End If
   If Not (destinationStream Is Nothing) Then
    destinationStream.Close()
   End If
  End Try
 End Sub
 Public Sub DecompressFile(ByVal sourceFile As String, ByVal destinationFile As String)
  ' make sure the source file is there
  If File.Exists(sourceFile) = False Then
   Throw New FileNotFoundException
  End If
  ' Create the streams and byte arrays needed
  Dim sourceStream As FileStream = Nothing
  Dim destinationStream As FileStream = Nothing
  Dim decompressedStream As GZipStream = Nothing
  Dim quartetBuffer As Byte() = Nothing
  Try
   ' Read in the compressed source stream
   sourceStream = New FileStream(sourceFile, FileMode.Open)
   ' Create a compression stream pointing to the destiantion stream
   decompressedStream = New GZipStream(sourceStream, CompressionMode.Decompress, True)
   ' Read the footer to determine the length of the destiantion file
   quartetBuffer = New Byte(4) {}
   Dim position As Integer = CType(sourceStream.Length, Integer) - 4
   sourceStream.Position = position
   sourceStream.Read(quartetBuffer, 0, 4)
   sourceStream.Position = 0
   Dim checkLength As Integer = BitConverter.ToInt32(quartetBuffer, 0)
   Dim buffer(checkLength + 100) As Byte
   Dim offset As Integer = 0
   Dim total As Integer = 0
   ' Read the compressed data into the buffer
   While True
    Dim bytesRead As Integer = decompressedStream.Read(buffer, offset, 100)
    If bytesRead = 0 Then
     Exit While
    End If
    offset += bytesRead
    total += bytesRead
   End While
   ' Now write everything to the destination file
   destinationStream = New FileStream(destinationFile, FileMode.Create)
   destinationStream.Write(buffer, 0, total)
   ' and flush everyhting to clean out the buffer
   destinationStream.Flush()
  Catch ex As ApplicationException
   MessageBox.Show(ex.Message, "An Error occured during compression", MessageBoxButtons.OK, MessageBoxIcon.Error)
  Finally
   ' Make sure we allways close all streams
   If Not (sourceStream Is Nothing) Then
    sourceStream.Close()
   End If
   If Not (decompressedStream Is Nothing) Then
    decompressedStream.Close()
   End If
   If Not (destinationStream Is Nothing) Then
    destinationStream.Close()
   End If
  End Try
 End Sub
End Class
```

