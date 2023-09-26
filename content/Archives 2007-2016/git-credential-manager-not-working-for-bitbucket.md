---
title: Git Credential Manager not working for BitBucket App Password
date: 2023-03-21
layout: post
categories:
- Troubleshooting
tags:
- git
comments: true
aliases:
    - ../git-credential-manager-not-working-for-bitbucket
---

My blog is a static site currently hosted

[Both Git Credential Manager (GCM)]([GitHub - git-ecosystem/git-credential-manager: Secure, cross-platform Git credential storage with authentication to GitHub, Azure Repos, and other popular Git hosting services.](https://github.com/git-ecosystem/git-credential-manager)) (newer) and [Git Credential Manager for Windows]([GitHub - microsoft/Git-Credential-Manager-for-Windows: Secure Git credential storage for Windows with support for Visual Studio Team Services, GitHub, and Bitbucket multi-factor authentication.](https://github.com/microsoft/Git-Credential-Manager-for-Windows)) (older) always used [Windows Credential Manager](https://support.microsoft.com/en-us/windows/accessing-credential-manager-1b5c916a-6a16-889f-8581-fc16e8165ac0) under-the-hood to securely store git credentials.

When I tried to push or pull from BitBucket I started getting strange behaviour. First a floating Atlassian credentials browser window would pop up ![sdd](../images/atlassian-bitbucket-gcm-popup.png)

```
21:41:24.180045 ...\Application.cs:95   trace: [RunInternalAsync] Version: 2.0.886.37866
21:41:24.184064 ...\Application.cs:96   trace: [RunInternalAsync] Runtime: .NET Framework 4.0.30319.42000
21:41:24.184419 ...\Application.cs:97   trace: [RunInternalAsync] Platform: Windows (x86-64)
21:41:24.184419 ...\Application.cs:98   trace: [RunInternalAsync] OSVersion: 10.0 (build 19044)
21:41:24.184419 ...\Application.cs:99   trace: [RunInternalAsync] AppPath: C:\Program Files\Git\mingw64\bin\git-credential-manager.exe
21:41:24.184419 ...\Application.cs:100  trace: [RunInternalAsync] InstallDir: C:\Program Files\Git\mingw64\bin\
21:41:24.184419 ...\Application.cs:101  trace: [RunInternalAsync] Arguments: get
21:41:24.261454 ...GitCommandBase.cs:33 trace: [ExecuteAsync] Start 'get' command...
21:41:24.269725 ...GitCommandBase.cs:47 trace: [ExecuteAsync] Detecting host provider for input:
21:41:24.270705 ...GitCommandBase.cs:48 trace: [ExecuteAsync]   protocol=https
21:41:24.270705 ...GitCommandBase.cs:48 trace: [ExecuteAsync]   host=bitbucket.org
21:41:24.270705 ...GitCommandBase.cs:48 trace: [ExecuteAsync]   username=schneider
21:41:24.356708 ...viderRegistry.cs:149 trace: [GetProviderAsync] Performing auto-detection of host provider.
21:41:24.397704 ...viderRegistry.cs:162 trace: [GetProviderAsync] Auto-detect probe timeout is 2 ms.
21:41:24.399706 ...viderRegistry.cs:170 trace: [GetProviderAsync] Checking against 4 host providers registered with priority 'Normal'.
21:41:24.400704 ...GitCommandBase.cs:50 trace: [ExecuteAsync] Host provider 'Bitbucket' was selected.
21:41:24.445702 ...tHostProvider.cs:272 trace: [GetSupportedAuthenticationModesAsync] https://bitbucket.org/ is bitbucket.org - authentication schemes: 'All'
21:41:24.484705 ...tHostProvider.cs:103 trace: [GetStoredCredentials] Look for existing credentials under https://bitbucket.org ...
21:41:24.604723 ...tHostProvider.cs:113 trace: [GetStoredCredentials] Found stored credentials: schneider/********
21:41:24.644701 ...tHostProvider.cs:393 trace: [ValidateCredentialsWork] Validate credentials (schneider/********) are fresh for https://bitbucket.org/ ...
21:41:24.655717 ...tbucketRestApi.cs:36 trace: [GetUserInformationAsync] HTTP: GET https://api.bitbucket.org/2.0/user
21:41:24.655717 ...pClientFactory.cs:58 trace: [CreateClient] Creating new HTTP client instance...
21:41:25.370530 ...tbucketRestApi.cs:39 trace: [GetUserInformationAsync] HTTP: Response 401 [Unauthorized]
21:41:25.373564 ...tHostProvider.cs:407 trace: [ValidateCredentialsWork] Failed to validate existing credentials using OAuth
21:41:25.374526 ...tHostProvider.cs:408 trace: [ValidateCredentialsWork] ! error: 'Failed to resolve username. HTTP: Unauthorized'.
21:41:25.380558 ...tbucketRestApi.cs:36 trace: [GetUserInformationAsync] HTTP: GET https://api.bitbucket.org/2.0/user
21:41:25.552532 ...tbucketRestApi.cs:39 trace: [GetUserInformationAsync] HTTP: Response 403 [Forbidden]
21:41:25.553509 ...tHostProvider.cs:422 trace: [ValidateCredentialsWork] Failed to validate existing credentials using Basic Auth
21:41:25.553509 ...tHostProvider.cs:423 trace: [ValidateCredentialsWork] ! error: 'Failed to resolve username. HTTP: Forbidden'.
21:41:25.556531 ...tHostProvider.cs:126 trace: [GetRefreshedCredentials] Refresh credentials...
21:41:25.556531 ...tHostProvider.cs:132 trace: [GetRefreshedCredentials] Checking for refresh token...
21:41:25.568711 ...tHostProvider.cs:139 trace: [GetRefreshedCredentials] No stored refresh token found
21:41:25.568711 ...tHostProvider.cs:143 trace: [GetRefreshedCredentials] Prompt for credentials...
21:41:25.611071 ...enticationBase.cs:87 trace: [ThrowIfUserInteractionDisabled] GCM_INTERACTIVE / credential.interactive is false/never; user interactivity has been disabled.
fatal: Cannot prompt because user interactivity has been disabled.
   at GitCredentialManager.Authentication.AuthenticationBase.ThrowIfUserInteractionDisabled()
   at Atlassian.Bitbucket.BitbucketAuthentication.<GetCredentialsAsync>d__4.MoveNext()
   ...
```