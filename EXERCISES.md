# Overview
This guide provides the cut-and-dry steps that we will walk through during the GitHub Advanced Security Certification Workshop.

# Module 1 - Enabling Features
1. Sign in to github.com
2. Click on your avatar in the top right corner
3. Click on `Your organizations`
4. Look for an organization that follows the format `ghas-bootcamp-DATE-HADNLE` where `DATE` is today's date and `HANDLE` is your handle
5. Ensure the organization has `5` repositories
6. In the `ghas-bootcamp-webgoat` repository, go to **Settings** -> **Code security and analysis** and then click **Enable** for _Dependency graph_, _Dependabot alerts_, _Dependabot security updates_, and **GitHub Advanced Security**
7. Navigate back to the organization's **Repositories** tab, and click **New repository**
8. Name the repository `public-repo` with the visibility option set to `Public` and the `Add a README file` box **checked**
9. Click **Create repository**
10. Navigate to the repository's **Settings** tab --> **Code security & analysis**
11. Notice that **Dependency Graph is always on by default**, **there is no Advanced Security button**, (it's not needed to enable secret scanning & code scanning for public repos), and **Secret scanning sends alerts to partners by default**
12. Navigate back to the organization's **Settings** tab --> **Code Security and analysis** and click **Enable all** for _Dependency graph_, _Dependablot alerts_, _Dependabot security updates_, and _GitHub Advanced Security_.
13. 🎉Congratulations!🎉 you enabled Dependabot and Advanced Security for all of the repositories in your organization 🚀

# Module 2 - Supply Chain Security
1. Navigate to the `ghas-bootcamp-webgoat` repository
2. Navigate to the **Security** tab --> **Dependabot alerts** and select the `Apache Commons Compress denial of service vulnerability` alert
3. Click the **Actions** tab, click **New workflow** and search for `Dependency Review`
4. Click **Configure**
5. View the workflow template, and then select **Cancel changes** (the workflow already exists in the repository)
6. Click the **Code** tab, and navigate to the `pom.xml` file
7. Select the pen "Edit this file" button
8. Uncomment the section between `Line 154` and `Line 161`, commit the changes **_to a separate branch_**, and create a pull request to compare those changes against the `main` branch
9. Monitor the `Dependency Review` status check (it should fail)
10. Click the `Details` link next to the status check
11. Expand the `Vulnerabilities` tab to see the net new vulnerabilities that were introduced by the code change
12. Click the `GHSA` links to see more details about the vulnerabilities
13. Note the patched version of `2.17.1` in `GHSA-8489-44mv-ggj8`
14. Navigate back to the **Code** tab, then to the pom.xml file
15. _**Set your branch to the patch branch you created earlier**_
16. Select the pen "Edit this file" button
17. Scroll back down to (roughly) `line 158` and edit the version of `log4j-core` to `2.17.1`
18. Commit the change directly to your **_patch branch from earlier_**
19. Navigate to the **Pull requests** tab and open the pull request that you created earlier: `Update pom.xml`
20. The Dependency Review workflow should have been retriggered
21. Monitor the result (it should now pass)
22. 🎉Congratulations!🎉 you caught and fixed a vulnerability in your pull request with Dependency Review 🚀

# Module 3 - Secret Scanning
1. Navigate to the `ghas-bootcamp-javascript` repository
2. Navigate to the **Settings** tab --> **Code security and analysis**
3. In the **Secret scanning** section, click **Enable**
4. Navigate to the **Security** tab --> **Secret scanning** to look at the 4 alerts (you may need to refresh the page)
5. Note the false positive (`GitHub Personal Access Token`) found in the `bad-practice-example.md` file (it's been revoked)
6. Navigate to the **Code** tab --> `.github` --> `secret_scanning.yml`
7. Select the pen "Edit this file" button
8. Uncomment `lines 1-2` and commit the changes to the `main` branch
9. Navigate back to the **Security** tab --> **Secret scanning** and notice that the `GitHub Personal Access Token` alert has been removed
10. Navigate to the **Settings** tab --> **Code security and analysis**
11. In the **Secret scanning** section, click **New pattern**
12. Provide a name for the pattern (e.g. `my pattern`) and use the following secret format: `password = \w+`
13. Enter a test string that matches the pattern (e.g. `password = abc123`)
14. Click **Save and dry run**
15. Under the **Dry run** section, click the **Reload** button, then click **Publish pattern**
16. Navigate to the **Security** tab --> **Secret scanning** to look at the new alert that was found
17. 🎉Congratulations!🎉 you ignored a file with a `secret_scanning.yml` configuration and created your own custom pattern 🚀

# Module 4 - Code Scanning
1. Navigate to the `ghas-bootcamp-java` repository
2. Navigate to the **Settings** tab --> **Code security and analysis**
3. In the **Code scanning** section, click **Setup**
4. Select the **Advanced** option
5. This will generate a file - `.github/workflows/codeql.yml`
6. Add `paths: '**/src/main/**'` under the `push` key to allow the analysis to only trigger on commits that edit files under `src` on the `main` branch
7. Add `paths-ignore: 'README.md'` under the `pull_request` key to disallow the analysis from triggering on edits to the `README` file in pull requests made against the `main` branch
8. Uncomment `Line 63` - `# queries: security-extended,security-and-quality` and remove the `security-extended,` portion to customize the analysis to use the out-of-the-box query suite: `security-and-quality`
9. Click commit changes to make the commit to the `main` branch
10. Navigate to the file `Application.java` in the repo and click the pencil widget to edit the file
11. Add a comment to the top of the file and then click commit changes to the `main` branch to trigger an instance of the analysis
12. Navigate to the **Security** tab --> **Code scanning** to look at the new alerts that were found once the analysis completes
13. Click into the `Query built from user-controlled sources` alert to view its information and recommendations for fixes
14. 🎉Congratulations!🎉 you detected the vulnerabilites in the application using code scanning setup with CodeQL and a customized configuration 🚀

# Module 5 - Administration
## Part 1 - Setup a Security Policy
1. Navigate to the `ghas-bootcamp-java` repository
2. Navigate to the **Security** tab
3. Under **Reporting**, click on **Policy**
4. Click **Start setup** to generate the `SECURITY.md` file
5. Note the default sections containing information about supported versions of your project and how to report a vulnerability
5. Click commit changes to make the commit to the `main` branch
6. Click back to the main page of the repository
7. Click on the **Security Policy** link under the **About** section to see how users of the repository can easily find the security policy
8. 🎉Congratulations!🎉 you created a security policy for the repository 🚀

## Part 2 - Fill out a security advisory
1. Navigate to the `ghas-bootcamp-java` repository
2. Navigate to the **Security** tab
3. Under **Reporting**, click on **Advisories**
4. Click **New draft security advisory**
5. Fill out the following fields:
  * Title
  * Description
  * Ecosystem
  * Package name
  * Affected versions
  * Patched versions
  * Severity
6. Click **Create draft security advisory**
7. Notice there is now a button that repository admins have access to: **Request CVE**
8. 🎉Congratulations!🎉 you started the process to create a security advisory for the repository 🚀

## Part 3 - Investigate Organization Level Settings
1. Navigate to the `ghas-bootcamp-<date>-<handle>` organization page --> **Settings** --> **Actions** --> **General**
2. Scroll down to the **Workflow permissions** section
3. Notice the two options for the `GITHUB_TOKEN` permissions in workflows
4. Note that you can select the default permissions for the `GITHUB_TOKEN` to be more restrictive
5. Note that under the more restrictive option, some workflow files would require to specify additional permissions, for example, the code scanning workflow would need `security-events: write` enabled for the `GITHUB_TOKEN`
6. 🎉Congratulations!🎉 you have learned about workflow least required access control when using `GITHUB_TOKEN`s 🚀