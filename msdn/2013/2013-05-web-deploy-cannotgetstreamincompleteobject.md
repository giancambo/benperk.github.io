# Web Deploy – CannotGetStreamIncompleteObject

Web Deploy is a very powerful tool with lots of features, one of which is the ability to synchronize file between a source and 1-n destination servers.  So, from web farm perspective, you can apply all of your code modifications to a single server and then use Web Deploy to move those files to other web servers in the farm.

On rare occasions, you may have received an error like:

+ The stream data of '{0}' is not yet available.

The first step in understanding what this error comes from you need to understand what a stream is and what it is used for.  A stream is any ordered pair (s, Δ) where: s is a sequence or list of elements and Δ is a sequence of positive real-time intervals. From a Web Deploy perspective this is the list of files which need to synchronized and Δ is the number of times or the amount of time in which the tool uses to perform the synchronization.

A stream is used to read and write the bytes from a source like a file or memory.  Hence, the stream is used as a bridge to transfer data from a file into its byte form.  You can view how this is done using C# with a study of the [System.IO.Stream][LINK1] class.  The methods and properties in this class provide the previously mentioned functionality.

By understanding the above definition, you may now be able to extract the reason for the error message based on its name - ' CannotGetStreamIncompleteObject ' and value - ' The stream data of '{0}' is not yet available.'.  Before reading on, think about it and try to come up with your own explanation and better yet, how would you resolve it?

What it means is that the file Web Deploy is attempting to synchronize is probably in the middle of being written to or being written to disk.  This is why that in most cases, simply rerunning the command results in the successful execution of it.

How do you fix it?  Good question.  There is a retryAttempts attribute which you can pass along in the command, but note the default value is 5.  It is possible that you still receive this error message when you increase this value, but it may reduce the occurrence of
it.  Another possibility is that you make sure all files on the source location cannot be written to during synchronization and the storage of all files needing synchronization have been completely written to disk before running your Web Deploy command.

[LINK1]: http://msdn.microsoft.com/en-us/library/system.io.stream.aspx
