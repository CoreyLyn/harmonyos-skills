# 权限管理参考

> **来源要求**: 本文档内容需对照官方文档验证。使用时请注明出处。
>
> 官方文档：
> - 权限管理指南: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/permissions
> - 申请权限: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-acquire-permissions
> - 权限列表: https://developer.huawei.com/consumer/cn/doc/harmonyos-references/permissions

## 权限类型

来源: [权限分类](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/permissions#权限分类)

### system_grant（系统授权）

安装时自动授予，包括：
- 网络访问
- 获取网络状态
- 查看网络连接

### user_grant（用户授权）

需要用户手动授予，包括：
- 相机、麦克风
- 位置信息
- 日历、联系人
- 存储、文件访问

## 配置权限

来源: [配置权限](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-acquire-permissions#配置权限)

### module.json5 配置

```json5
{
  "module": {
    "requestPermissions": [
      {
        "name": "ohos.permission.INTERNET",
        "reason": "$string:internet_reason",
        "usedScene": {
          "abilities": ["EntryAbility"],
          "when": "always"
        }
      },
      {
        "name": "ohos.permission.CAMERA",
        "reason": "$string:camera_reason",
        "usedScene": {
          "abilities": ["EntryAbility"],
          "when": "inuse"
        }
      }
    ]
  }
}
```

## 运行时请求权限

来源: [请求用户授权](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-acquire-permissions#请求用户授权)

### 单个权限请求

```typescript
import { abilityAccessCtrl, bundleManager, Permissions } from '@kit.AbilityKit';

async function requestPermission(context: Context, permission: Permissions) {
  let atManager = abilityAccessCtrl.createAtManager();
  let bundleInfo = await bundleManager.getBundleInfoForSelf(bundleManager.BundleFlag.GET_BUNDLE_INFO_WITH_APPLICATION);

  let grantStatus = await atManager.requestPermissionsFromUser(context, [permission]);

  if (grantStatus.authResults[0] === 0) {
    // 权限授予成功
    return true;
  } else {
    // 权限被拒绝
    return false;
  }
}

// 使用
await requestPermission(getContext(), 'ohos.permission.CAMERA');
```

### 多个权限请求

```typescript
let permissions: Permissions[] = [
  'ohos.permission.CAMERA',
  'ohos.permission.READ_MEDIA',
  'ohos.permission.WRITE_MEDIA'
];

let grantStatus = await atManager.requestPermissionsFromUser(getContext(), permissions);

// 检查每个权限的结果
permissions.forEach((permission, index) => {
  if (grantStatus.authResults[index] === 0) {
    console.log(`${permission} granted`);
  } else {
    console.log(`${permission} denied`);
  }
});
```

## 检查权限状态

来源: [校验权限](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-acquire-permissions#校验权限)

```typescript
async function checkPermission(context: Context, permission: Permissions): Promise<boolean> {
  let atManager = abilityAccessCtrl.createAtManager();
  let bundleInfo = await bundleManager.getBundleInfoForSelf(bundleManager.BundleFlag.GET_BUNDLE_INFO_WITH_APPLICATION);

  let grantStatus = await atManager.checkAccessToken(
    bundleInfo.appInfo.accessTokenId,
    permission
  );

  return grantStatus === abilityAccessCtrl.GrantStatus.PERMISSION_GRANTED;
}

// 使用
let hasPermission = await checkPermission(getContext(), 'ohos.permission.CAMERA');
if (!hasPermission) {
  // 请求权限
}
```

## 常用权限列表

来源: [权限列表](https://developer.huawei.com/consumer/cn/doc/harmonyos-references/permissions)

| 权限名 | 类型 | 说明 |
|--------|------|------|
| ohos.permission.INTERNET | system_grant | 网络访问 |
| ohos.permission.GET_NETWORK_INFO | system_grant | 获取网络信息 |
| ohos.permission.CAMERA | user_grant | 使用相机 |
| ohos.permission.MICROPHONE | user_grant | 使用麦克风 |
| ohos.permission.LOCATION | user_grant | 获取位置信息 |
| ohos.permission.APPROXIMATELY_LOCATION | user_grant | 模糊位置信息 |
| ohos.permission.READ_MEDIA | user_grant | 读取媒体文件 |
| ohos.permission.WRITE_MEDIA | user_grant | 写入媒体文件 |
| ohos.permission.READ_PREFERENCES | system_grant | 读取配置 |
| ohos.permission.WRITE_PREFERENCES | system_grant | 写入配置 |

## 权限最佳实践

来源: [权限最佳实践](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/permissions#最佳实践)

1. **最小权限原则**：只申请必要的权限（官方文档规定）
2. **及时请求**：在需要使用权限前再请求（推荐实践）
3. **解释原因**：清楚说明为什么需要该权限（官方文档规定）
4. **处理拒绝**：优雅处理用户拒绝权限的情况（推荐实践）
5. **动态检查**：每次使用前检查权限状态（推荐实践）