---
layout: post
title: "访问相册和相机"
category:  Skill
---

可以用UIImagePickerController来调用摄像头和选择相册中的资源。
也可以不使用UI的形式来访问相册，那就要用到AssetsLibrary中的ALAssetsLibrary。

- 请求访问权限
可以直接用ALAssetsLibrary.authorizationStatus来判断当前是否允许访问照片，但是如果是第一次访问照片的话，我们要弹出是否允许访问照片，ALAssetsLibrary中没有直接弹出权限选择的方法，可以用以下方法替代：

```Objective-C
- (void)isAuthorizedToAccessAlbumWithCompletion:(void(^)(BOOL success))completion
{
    ALAuthorizationStatus status = ALAssetsLibrary.authorizationStatus;
    if (status == ALAuthorizationStatusNotDetermined) {
        [assetsLibrary enumerateGroupsWithTypes:ALAssetsGroupAlbum
                                     usingBlock:^(ALAssetsGroup *group, BOOL *stop) {
                                         completion(YES);
                                         stop = YES;
                                     } failureBlock:^(NSError *error) {
                                         completion(NO);
                                     }];
    } else if (status == ALAuthorizationStatusAuthorized) {
        completion(YES);
    } else {
        completion(NO);
    }
}
```

- 遍历Groups

```Objective-C
- (void)getAlbumGroupsWithCompletion:(void(^)(BOOL success, NSArray *groups))completion
{
    NSMutableArray *albumGroups = [NSMutableArray array];
    dispatch_async(dispatch_get_global_queue(QOS_CLASS_DEFAULT, 0), ^{
        [assetsLibrary enumerateGroupsWithTypes:ALAssetsGroupAll
                                     usingBlock:^(ALAssetsGroup *group, BOOL *stop) {
                                         if (group) {
                                            [albumGroups addObject:group];
                                         } else {
                                             // has stopped
                                             completion(YES, albumGroups);
                                         }
                                     } failureBlock:^(NSError *error) {
                                         // do not have authorization
                                         completion(NO, nil);
                                     }];
    });
}
```

- 遍历照片

```Objective-C
- (void)getAlbumPhotosInGroup:(ALAssetsGroup *)group WithCompletion:(void(^)(BOOL success, NSArray *assets))completion
{
    NSMutableArray *albumPhotos = [NSMutableArray array];
    dispatch_async(dispatch_get_global_queue(QOS_CLASS_DEFAULT, 0), ^{
        [group enumerateAssetsUsingBlock:^(ALAsset *result, NSUInteger index, BOOL *stop) {
            if (result) {
                NSString *assetType = [result valueForProperty:ALAssetPropertyType];
                if ([assetType isEqualToString:ALAssetTypePhoto]) {
                    [albumPhotos addObject:result];
                }
            } else {
                // has stopped
                completion(YES, albumPhotos);
            }
        }];
    });
}
```

- 写入Group

- 保存照片
