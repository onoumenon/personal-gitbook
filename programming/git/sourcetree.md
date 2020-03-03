# Sourcetree

{% embed url="https://blog.sourcetreeapp.com/2012/08/01/smart-branching-with-sourcetree-and-git-flow/" %}

## Release and Deployment

### Finish a feature

* If your feature relies on another repo's \(common library\) release, merge that first \(see below for release\)
* Change package.json’s for common library ver  
* Yarn install
* Make a commit \(msg: ‘use common-lib v1.0.0’\)
* Finish feature to merge to dev

### Start a Release \(1 or more\):

\(to merge sk-web feature after finishing feature &gt; dev\)

* start new release \(msg: v2.3.18 if prev is v2.3.17\)
* change package.json
* if common library's version is changed, change it in package.json as well
* \(don't change yarn or npm files, just yarn install\)
* commit the changes \(msg: Release v2.3.18\)
* Finish release
* push to dev + master \(with tags\)

### Start a Hotfix \(cherry-pick\):

* start a hotfix \(msg: v2.3.18 if prev is v2.3.17\)
* cherry pick commits needed for hotfix
* Note:
  * if common library's version is changed, change it in package.json as well
  * \(don't change yarn or npm files, just yarn install\)
* change package.json
* finish hotfix
* push to dev + master \(with tags\)

### Manual Deployment

on common-library, pull latest from master and dev

* make sure it's the correct version of common library
* point to master branches \(on both repos\)
* ember deploy staging —activate

{% hint style="warning" %}
The next step is important, as it's the last checkpoint before deployment.
{% endhint %}

* check staging site \(and verify is all commits look correct\)
* ember deploy production —activate
* check production \(and verify that nothing is broken\)



