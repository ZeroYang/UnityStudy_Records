#iOS dylib not loaded Crash

> dyld: Library not loaded: @rpath/xxx.dylib
>   Referenced from: /var/mobile/Containers/Bundle/Application/3FC2DC5C-A908-42C4-8508-1320E01E0D5B/Stylist.app/Stylist
>   Reason: no suitable image found.  Did find:
>     /private/var/mobile/Containers/Bundle/Application/3FC2DC5C-A908-42C4-8508-1320E01E0D5B/testapp.app/Frameworks/libswiftCore.dylib: mmap() errno=1 validating first page of '/private/var/mobile/Containers/Bundle/Application/3FC2DC5C-A908-42C4-8508-1320E01E0D5B/testapp.app/Frameworks/xxx.dylib'
> (lldb) 

iOS 工程 使自定义或第三方dylib crash 

1. 确定xxx.dylib加入了Embedded Binaries

2. 确保build setting Architecture 选择了正确的Architecture armv7 armv7s arm64。(如果dylib支持的Architecture和build setting的不一致也会出现上面的crash日志)


解决办法：

There are two ways to solve this:

Click on Product -> Clean (or CMD-Shift-K)

Or by manually cleaning the Xcode setting files:
    
    rm -rf "$(getconf DARWIN_USER_CACHE_DIR)/org.llvm.clang/ModuleCache"
    rm -rf ~/Library/Developer/Xcode/DerivedData
    rm -rf ~/Library/Caches/com.apple.dt.Xcode