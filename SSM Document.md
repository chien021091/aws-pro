Plugin
- aws:downloadContent : allows ssm to download directly from a specified source (github, s3, HTTP)
- aws:configurePackage : installing and managing software package (s3, ssm distributor, HTTP, not from github)
- aws:softwareInventory : collect software inventory data (installed application, versions) on managed instance, not to download and execute script

Action: AWS-RunPatchBaseLine
- Scan for missing patch
- Install approved patch