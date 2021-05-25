# Automated Semantic Versioning Process

Problem: how to automated semantic versioning without having to manually maintain it (when, how and who and which part of the version).

# Manual process: 
##  When to updated version
  1. feature planning.
  2. feature deployment/released.
  3. feature development start.
  4. feature development completion.
  5. Prob more cases..
  
## Where to keep the versioning:
  1. variable in the CI/CD.
  2. version file.
  3. git tags.
  4. Enterprise asset db etc or some other versioning tool.
  
## Who can udpate the version:
  1. Any dev.
  2. Project/team Admins only.
  3. Project/team devops.
  4. release managers.
  
## Which part of the version to change and How:
  1. Which part to udpate. Major, minor, path, build.
  2. How to decide when its minor vs patch etc.
  
  
 # Automation:
 
 Taking example for rest api or public package (jar) and assuming that we have all test in the project code repo itself.
 
 ## Setup
 
 app code : src/main
 Test code: 
   1. unit test code: src/test/unit
      These are class,method level tests with class level mocks.
   2. integration test code: src/test/integration
      These test covers integration points especially from backend integration point of view, and not necesarily from where the call is getting triggered from (client call, scheduler, batch job etc)
   3. client test: src/test/client
      These only covers the client calls with mocks on any backend integration point.
      These can be postman,curl calls for api's.

## Versioning logic :
  1. Some format of 1.0.0-9 (Major.minor.patch-build) for the main/release branch.
  2. Maintain version in the git tags or CI/CD var.
  3. Version update process in the CI/CD as per following rules.
      1. Always update build number for every ci/cd run, wrt to latest git changes for that pipeline commit or diff from the last vesion tag.
      2. Increment patch if there is any change in unit tests (src/test/unit/** match).
      3. Increment minor number if there is any change in the integration tests (src/test/integration/** match) and reset minor number.
      4. Increment major number if there is any change in the client tests (src/test/client/** match) and reset minor and path numbers.
          - some way to distinguish old test vs new (to identifify backward compatibility of change) ?
          - if the for any backward change, only new tests/files are added and hence, if the change has any new files, this can be considered backward compatible and hence same major number but increment minor number only.
      5. For any non main/release branch add branch name (feature/add-logs) in the 1.0.1-feature-add-log-BuildXX, assuming that it would change some unit tests. Once the code get merged to the main, feature branch gets dropped and 1.0.1-BuildXX the new version from main.
  
  
  
# references: 
  - https://developer.okta.com/blog/2019/12/16/semantic-versioning
  - https://www.baeldung.com/cs/semantic-versioning
  - https://semver.org/
