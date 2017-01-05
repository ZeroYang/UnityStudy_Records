#iOS App Review Rejected & iCloud backUp

>On launch and content download, your app stores 26.68 MB on the user's iCloud, which does not comply with the iOS Data Storage Guidelines.

>##Next Steps

>Please verify that only the content that the user creates using your app, e.g., documents, new files, edits, etc. is backed up by iCloud as required by the iOS Data Storage Guidelines. Also, check that any temporary files used by your app are only stored in the /tmp directory; please remember to remove or delete the files stored in this location when it is determined they are no longer needed.

>Data that can be recreated but must persist for proper functioning of your app - or because users expect it to be available for offline use - should be marked with the "do not back up" attribute. For NSURL objects, add the NSURLIsExcludedFromBackupKey attribute to prevent the corresponding file from being backed up. For CFURLRef objects, use the corresponding kCRUFLIsExcludedFromBackupKey attribute.
>##Resources
>For additional information on preventing files from being backed up to iCloud and iTunes, see Technical Q&A 1719: How do I prevent files from being backed up to iCloud and iTunes.

**参考：
["Technical Q&A QA1719 How do I prevent files from being backed up to iCloud and iTunes?"](https://developer.apple.com/library/content/qa/qa1719/_index.html)**

代码片段：

	-(void)addNotBackUpiCloud
	{
	    NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
	    NSString *documentpath = ([paths count] > 0) ? [paths objectAtIndex:0] : nil;
	    [self addSkipBackupAttributeToItemAtPath:documentpath];
	    
	    NSString *foldername = @"res";
	    NSString *folderPath = foldername;
	    folderPath = [documentpath stringByAppendingPathComponent:foldername];
	    NSFileManager *fileMgr= [NSFileManager defaultManager];
	    if(![fileMgr fileExistsAtPath:folderPath])
	    {
	        [fileMgr createDirectoryAtPath:folderPath withIntermediateDirectories:YES attributes:nil error:nil];
	    }
	    
	    [self addSkipBackupAttributeToItemAtPath:folderPath];
	    
	    [self addSkipBackupAttributeToItemAtPath:documentpath];
	}
	
	- (BOOL)addSkipBackupAttributeToItemAtPath:(NSString *) filePathString
	{
	    NSURL* URL= [NSURL fileURLWithPath: filePathString];
	    assert([[NSFileManager defaultManager] fileExistsAtPath: [URL path]]);
	    
	    NSError *error = nil;
	    BOOL success = [URL setResourceValue: [NSNumber numberWithBool: YES]
	                                  forKey: NSURLIsExcludedFromBackupKey error: &error];
	    if(!success){
	        NSLog(@"Error excluding %@ from backup %@", [URL lastPathComponent], error);
	    }
	    
	//    NSLog(@"File %@  is excluded from backup %@", filePathString, [URL resourceValuesForKeys:[NSArray arrayWithObject:NSURLIsExcludedFromBackupKey] error:nil]);
	    
	    return success;
	}

	-(void)excludedDocumentForBackUp
	{
	    NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
	    NSString *basePath = ([paths count] > 0) ? [paths objectAtIndex:0] : nil;
	    NSArray *documents = [[NSFileManager defaultManager] contentsOfDirectoryAtPath:basePath error:nil];
	    NSURL *URL;
	    NSString *completeFilePath;
	    for (NSString *file in documents) {
	        completeFilePath = [NSString stringWithFormat:@"%@/%@", basePath, file];
	        URL = [NSURL fileURLWithPath:completeFilePath];
	        NSLog(@"File %@  is excluded from backup %@", file, [URL resourceValuesForKeys:[NSArray arrayWithObject:NSURLIsExcludedFromBackupKey] error:nil]);
	        
	        NSDictionary *attr = [URL resourceValuesForKeys:[NSArray arrayWithObject:NSURLIsExcludedFromBackupKey] error:nil];
	        id ret = [attr objectForKey:NSURLIsExcludedFromBackupKey];
	        if(![ret boolValue])
	        {
	            [self addSkipBackupAttributeToItemAtPath:completeFilePath];
	        }
	    }
	}
