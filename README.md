# DLSFTPClient

SFTP Client library and sample app for iOS, using libssh2

## Overview

DLSFTPClient is a an Objective-C wrapper around libssh2, providing simple access to upload and download files, as well as perform directory operations.  It requires iOS 5.1 and leverages Grand Central Dispatch (libdispatch) heavily.

## Sample Usage

Most of the functionality is accessed via DLSFTPConnection.  Instances of this object store the hostname and credentials.

    DLSFTPConnection *connection = [[DLSFTPConnection alloc] initWithHostname:@"foo.bar.com"
                                                                         port:22
                                                                     username:@"username"
                                                                     password:@"password"];
                                                                     
The operation methods take blocks as arguments

    [connection downloadFileAtRemotePath:@"/Users/username/Desktop/file.txt"
                             toLocalPath:@"/path/to/sandbox/Documents/file.txt"
                                progressBlock:^(unsigned long long bytesSent, unsigned long long bytesTotal) {
                                    NSLog(@"Received %llu bytes", bytesSent);
                               } successBlock:^(DLSFTPFile *file, NSDate *startTime, NSDate *finishTime) {
                                    NSLog(@"Completed transfer of %@", file.filename");
                                } failureBlock:^(NSError *error) {
                                    NSLog(@"Transfer failed: %@", error);
                                }];

Blocks are dispatched to the global concurrent queue.                                                                     

    [connection disconnect];

## Features

1. Upload and download files via [SFTP](http://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol)
2. List files
3. Create directories
4. Rename/Move files/directories
5. Remove files/directories
6. Operations can be cancelled

## To-do

Quite a bit.  DLSFTPClient is still a work-in-progress, and the API is still subject to change.

## Project Dependencies

1. [libssh2](https://github.com/x2on/libssh2-for-iOS) - Provides a shell script to build libssh2.a Static Library and headers

## License

DLSFTPClient is open-source under the BSD license. See the LICENSE file for more info.

