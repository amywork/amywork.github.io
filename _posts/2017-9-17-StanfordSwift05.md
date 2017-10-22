---
layout: post
title: "스탠포드 iOS 강의노트 L15,16"
author: "younari"
---

## Lecture 15,16
### Notification, Alert, File system, URL, LifeCycle, Segues

> **Course Description** Updated for iOS 10 and Swift. Tools and APIs required to build applications for the iPhone and iPad platforms using the iOS SDK. User interface design for mobile devices and unique user interactions using multi-touch technologies. Object-oriented design using model-view-controller paradigm, memory management, Swift programming language. Other topics include: object-oriented database API, animation, mobile device power management, multi-threading, networking and performance considerations.

- This course material is only available in the iTunes U app on iPhone or iPad.
- [Developing iOS 10 Apps with Swift
by Stanford](https://itunes.apple.com/us/course/developing-ios-10-apps-with-swift/id1198467120)

# NotificationCenter.default
- 커뮤니케이션을 위한 Radio Station
- 요기서는 Push Notification을 말하는 것은 아님!
- Use this when you want to **listen** to a radio station

{% highlight swift %}

var observer: NSObjectProtocol
observer = NotificationCenter.default.addObserver (
	forName: NSNotification.Name, // the name of the radio station
	object: Any?, // the broadcaster (or nil for anyone)
	queue: OperationQueue? // the queue on which to dispatch the closure below
) { (notification: Notification) -> Void in 
	let info: Any? = notification.userInfo
	// info is usually a dictionary of notification-specific information
}

{% endhighlight %}

# URL
- [Apple documentation 읽어보기](https://developer.apple.com/documentation/foundation/url)
- A value that identifies the location of a resource, such as an item on a remote server or the path to a local file.
- `appendingPathComponent`
- `resourceValues` -> `[URLResourceKey:Any]`
- .creationDataKey, .isDirectoryKey, .fileSizeKey

#### URLs are the preferred way to refer to local files. Most objects that read data from or write data to a file have methods that accept a URL instead of a pathname as the file reference. For example, you can get the contents of a local file URL as String by calling func init(contentsOf:encoding) throws, or as a Data by calling func init(contentsOf:options) throws.

# File System
- Create, read, write, and examine files and folders in the file system.
- [Apple Documentation 읽어보기](https://developer.apple.com/documentation/foundation/file_system)
- `func createDirectory`
- `func isReadableFile(atPath: String) -> Bool`
- `func write(to url: URL, atomically: Bool) -> Bool`
- atomically: safe write (write to a temp file first, and then swap it in, so running out of disk space while writing it, it wouldn't corrupt.)

## Steps of accessing files in the Unix filesystem
- Get the root of a path into an URL (Documents directory, Caches directory...)
- Append path components to the URL (The names of your files)
- Write to OR Read from files
- Manage the filesystem with FileManager

# FileManager (Class)
- [Apple Documentation 읽어보기](https://developer.apple.com/documentation/foundation/filemanager)
- Use it to find out about what's in the file system.
- **locate, create, copy, and move** files and directories.
- An object that provides a convenient interface to the contents of the file system.
- `FileManager.defaults.urls`(for directory: FileManager.SearchPathDirectory.documentDirectory, in domainMask: .userDominask)

### When specifying the location of files, you can use either NSURL or NSString objects. The use of the *NSURL class is generally preferred* for specifying file-system items because they can convert path information to a more efficient representation internally. 

# App Delegate
- It contains your app’s startup code. 
- It responds to key changes in the state of your app. 
- Specifically, it responds to both temporary interruptions and to changes in the execution state of your app, such as when your app transitions from the foreground to the background.
- It responds to **notifications** originating from outside the app, such as low-memory warnings, download completion notifications, and more. 
- It determines whether state preservation and **restoration** should occur and assists in the preservation and restoration process as needed. 
- It responds to events that target the app itself and are not specific to your app’s views or view controllers.
- You can use it to store your app’s central data objects or any content that does not have an owning view controller.
