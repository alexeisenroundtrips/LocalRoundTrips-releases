# RoundTrips — System requirements

What a computer needs before RoundTrips is installed on it, and what the person installing it
needs to have arranged. This is the page the terms and conditions refer to. It is kept in step
with the product; the version it describes is the current release on this repository.

## The computer

| | Required? | Detail |
|---|---|---|
| **Windows 10 or 11, 64-bit** | Yes | There is no 32-bit build and no macOS or Linux build. |
| **Autodesk Navisworks Manage**, 2020 or newer | For building models | RoundTrips drives Navisworks' own `FiletoolsTaskRunner.exe`, which ships only with **Manage**. Simulate and Freedom do not include it. Navisworks is licensed Autodesk software and is not bundled; it must be installed and signed in on the same computer at least once. Everything except federating, cleaning and importing works without it: browsing, filtering, downloading, uploading, file syncs and transmittals. |
| **.NET runtime** | No | The build is self-contained; the .NET 9 runtime ships inside the application folder. Nothing is installed system-wide. |
| **Administrator rights** | No | Installs per user by default, into the user's own profile. A machine-wide install into Program Files is offered, not required. |
| **Disk** | About 150 MB for the program | Working data is bounded by settings: a download cache of up to 50 GB and run folders of up to 50 GB by default, both adjustable and both relocatable to a drive with room. Federated models are large; plan for the models, not the program. |
| **Memory and CPU** | What Navisworks needs | Federating is Navisworks' work. A machine that federates the same models by hand in Navisworks will federate them with RoundTrips. Up to six runs execute at once by default; lower it on a smaller machine. |
| **Full-disk encryption (BitLocker)** | Strongly recommended | Credentials go to Windows Credential Manager, but downloaded models, the task database and logs are ordinary files protected by Windows permissions only. Deploy to machines with BitLocker enabled. |

## The accounts

| | Required? | Detail |
|---|---|---|
| **An Autodesk account with access to the ACC or BIM 360 projects you will work on** | Yes | Each user signs in as themselves, in their own browser. There is no shared credential and no service account; RoundTrips acts with exactly that user's permissions. |
| **Authorisation by your ACC account administrator** | Yes, once per account | An administrator adds the RoundTrips Client ID under **Account Admin → Custom Integrations**. Until they do, sign-in succeeds and the project list is empty. The Connect screen shows the steps and the Client ID to copy. This is an Autodesk requirement; RoundTrips cannot perform it for you. |
| **A Microsoft account (OneDrive or SharePoint)** | Only for those destinations | Work or personal account; SharePoint document libraries need a work account and a plan that includes cloud drives. |
| **A Dropbox account** | Only for that destination | |
| **A RoundTrips licence** | After the 30-day trial | The trial starts by itself on first launch, one per computer. A licence covers one person on one computer and is paid monthly in advance. |

## The network

All traffic is outbound HTTPS on port 443, using the Windows TLS stack and whatever proxy Windows
is configured with, including a PAC script. There is no inbound listener except during sign-in.

| Host | Purpose | Needed |
|---|---|---|
| `developer.api.autodesk.com` | Autodesk Platform Services: sign-in, projects, folders, files | For any ACC work |
| `api.userprofile.autodesk.com` | The signed-in user's own name and email | For any ACC work |
| Autodesk's file storage on Amazon S3 | The file bytes, over a signed URL Autodesk issues per transfer | For any ACC download or upload |
| `login.microsoftonline.com`, `graph.microsoft.com` | Microsoft sign-in and OneDrive / SharePoint | Only for OneDrive tasks |
| `www.dropbox.com`, `api.dropboxapi.com`, `content.dropboxapi.com` | Dropbox sign-in and files | Only for Dropbox tasks |
| `api.keygen.sh` | Licence activation and a daily validation | For licensing; five days of offline grace |
| `roundtrips.io` | One call on first launch to start the trial | Once per computer |
| `api.github.com`, `github.com` | The update check, only when a user presses the button, and the release page it opens | Optional |
| `live.roundtrips.io` | RoundTrips Live, the optional status feed | Only if switched on |

**Sign-in callback.** During sign-in the application listens on the loopback address only, on port
8080, 8321 or 8322 (whichever is free) for Autodesk and Dropbox, and on any free port for
Microsoft, for the seconds until the browser returns. A firewall that blocks the loopback redirect
makes sign-in hang; allow `RoundTrips.exe`.

**A proxy that requires its own sign-in, or one that inspects TLS,** is not supported yet.

## Installing

`RoundTrips-<version>-setup.exe` is a standard installer. Silent install for deployment tooling:

```
RoundTrips-<version>-setup.exe /VERYSILENT /NORESTART /CURRENTUSER
```

Add `/ALLUSERS` (elevated) for a machine-wide install and `/LOG=<path>` for an install log. A
portable zip of the same application is published beside it. Every release is published with its
SHA-256 digests and a CycloneDX software bill of materials; verify the download against the digest
before installing. Updates are never automatic: the application only checks when asked, and opens
the release page in the browser rather than downloading anything itself.

The detailed IT chapter — where data lives, credentials, logging and the audit trail, licensing —
is in the manual inside the application (F1, *For IT*).
