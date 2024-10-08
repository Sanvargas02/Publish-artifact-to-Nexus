# Publish Artifact to Nexus Repository 

## Introduction
In this project, we will focus on uploading a Jar file to an existing hosted repository in Nexus using Maven or Gradle. 

### Key Steps:
- **Upload Jar File**: Push the file to the hosted repository in Nexus.
- **Maven/Gradle Command**: Use the appropriate command for pushing to the remote repository.
- **Configuration**: Set up both tools (Maven or Gradle) to connect to Nexus using the Nexus Repository URL and Credentials.
- **Permissions**: Ensure that the Nexus user has the permission to upload artifacts.

## Create Nexus User and Assign Permissions

To upload artifacts, we need to create a Nexus user and assign the necessary permissions. **Note**: Typically, users are not created manually; they are integrated from the company with LDAP. However, for this example, we will create a user manually.

### Steps:
1. Navigate to **Settings** > **Security** > **Users**.
   ![Alt Text](Nexus-Artifact-Repositorie-Manager/create-a-nexus-user/1-Select-user.png)
2. Click on **Create local user**.
   ![Alt Text](Nexus-Artifact-Repositorie-Manager/create-a-nexus-user/2-Create-local-user.png)
3. Now we need to create a role for the user:
   - Go to **Security** > **Roles**.
   - Click on **Create Role**.
   ![Alt Text](Nexus-Artifact-Repositorie-Manager/create-a-nexus-user/3-Create-role.png)

### Assign Permissions:
We have a list of permissions that we can assign to the role. Common permissions include: 
- **Add**
- **Browse**
- **Delete**
- **Edit**
- **Read**
 ![Alt Text](Nexus-Artifact-Repositorie-Manager/create-a-nexus-user/4-Permissions.png)
 ![Alt Text](Nexus-Artifact-Repositorie-Manager/create-a-nexus-user/5-Permissions.png) 

### Role Categories:
There are two main categories of roles:
- **Administrators**: These roles are usually for Nexus admins (DevOps), who manage tasks like backups, installing plugins, and general Nexus maintenance.
- **Developers**: These roles are for users who need to upload or fetch artifacts from Nexus.

In our case, we want the role to have permissions to:
- Upload the jar file.
- Retrieve information about the jar file.

### Assigning the Role:
- Select the **Views** option.
  ![Alt Text](Nexus-Artifact-Repositorie-Manager/create-a-nexus-user/6-View-privilege.png) 
- Apply the **Role Selections** if you want to.
- Save the role.

### Assigning the Role to the User:
1. Assign the role to the user you created.
   ![Alt Text](Nexus-Artifact-Repositorie-Manager/create-a-nexus-user/7-Assign-Role-to-user.png) 
2. Now, you can use this **user** in your project to provide the **Gradle** or **Maven credentials** to connect to Nexus. (Maven/Gradle → User → Nexus).

#### UNTIL HERE I HAVE THE FIRST PART - INTRO AND CREATION OF USER

## Gradle Project Configuration with Nexus

To set up a Gradle project for publishing artifacts to Nexus, follow these steps:

### Steps:
1. **Clone the Gradle Project** and open it in your IDE.
   - If a build has already been done, it might need to be deleted to apply the necessary configurations.

2. **Add the Maven Publishing Plugin**:
   - We are going to add a plugin to publish a Jar file to a Maven repository format (since Maven and Gradle use the same format to upload Java artifacts).
     ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/1-Adding-Plugin.png) 
   - This allows Gradle to connect to Nexus and push to its Maven repository.
   - Refresh/reload the Gradle project after adding the plugin.
     ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/2-Refresh.png) 

3. **Configure the Publishing Block**:
   - In this block, we configure the Jar file publications to be uploaded, and set the Nexus repository where the Jar will be uploaded.
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/3-Publishing-Config-Blocks.png)

4. **Set the Publications Block**:
   - In this block, add the path to the Jar file from the build directory.
   - Define the version in the same block.
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/4-Publication-block.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/5-Publication-block.png)

5. **Configure the Repositories Block**:
   - Add the Nexus information, including the repository name, URL, credentials, etc.
   - Each repository has its own URL, which can be seen in Nexus.
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/6-Repositorie-Block.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/7-Repositorie-Block.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/8-Repositorie-Block.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/9-Repositorie-Block.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/10-Repositorie-Block.png)

6. **Upload to Maven Snapshots**:
   - For development, we are going to upload to the `maven-snapshots` repository.
   - If we need to release a stable version, we will upload it to `maven-releases`.
   - **Note**: Snapshots are for development and testing, while releases are for production.
     ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/11-Upload-to-snapshots.png)
     ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/12-Upload-to-snapshots.png)

7. **Set the Credentials Block**:
   - Add the Nexus username and password. 
   - **Important**: Do not hardcode credentials directly in the code to avoid tracking sensitive information in Git/GitHub. Instead, create a Gradle properties file.
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/13-gradle-properties-file.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/14-gradle-properties-file.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/15-gradle-properties-file.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/16-gradle-properties-file.png)

8. **Update the `settings.gradle` file**:
   - Change the name of the app if necessary.
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/17-change-name-app.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/18-change-name-app.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/19-change-name-app.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/20-change-name-app.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/21-change-name-app.png)

9. **Allow Insecure Protocol**:
   - Since we are not using HTTPS (SSL encryption) to connect to Nexus yet, allow the insecure protocol in the configuration.
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/22-Insecure-Protocol.png)

10. **Reload Gradle**:
    - Reload or apply Gradle changes to ensure all configurations are updated.
    - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/23-Reload-Gradle-Project.png)
    
---

## Gradle Project Jar Upload

### Steps:
1. **Build the Project**:
   - Run `gradle build` to create the Jar file.
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/24-building-app.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/25-building-app.png)

2. **Prepare for Upload**:
   - With the Jar ready, search the build directory to confirm that it exist in the terminal.
   - Use the `gradle publish` command to upload the Jar. This command was enabled by the plugin added to the `build.gradle` file (it is not a default Gradle command).
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/26-Gradle-publish.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/27-Gradle-publish.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/28-Gradle-publish.png)

3. **Verify in Nexus**:
   - Go to the **Browse** option in Nexus to see the different components in your repository.
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/29-browse-repositorie.png)
   - Check the `maven-snapshots` repository to see your app. The package name (e.g., `my-app`) will appear, and other folders represent the configuration group.
   - Expand the package to view the Jar file and other metadata files.
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/30-my-app-upload.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/31-my-app-upload.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Gradle-Project-Config-and-Upload/32-my-app-upload.png)
     
#### UNTIL HERE I HAVE THE SECOND PART - GRADLE PROJECT CONFIG AND UPLOAD

## Maven Project Configuration with Nexus and Jar Upload

To configure a Maven project to upload a Jar file to Nexus, follow these steps:

### Steps:
1. **Configure the `pom.xml` File**:
   - The file where we are going to configure the upload is the `pom.xml`.
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Maven-Project-Config-and-Upload/1-file-to-config.png)

2. **Add the Plugin**:
   - First, configure the plugin that enables the upload of the Jar file to Nexus. 
   - In the first block, specify the version of the plugin and the plugin itself.
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Maven-Project-Config-and-Upload/2-Config-plugin.png)

3. **Reload Maven**:
   - After configuring the plugin, reload Maven to apply the changes.
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Maven-Project-Config-and-Upload/3-Load-Plugin.png)

4. **Configure the Nexus Repository**:
   - Set the location of the Nexus repository. The repository can have any `id`, but this `id` will help identify it between other repositories.
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Maven-Project-Config-and-Upload/4-Location-of-Nexus.png)

5. **Reload Maven for Snapshots**:
   - Reload Maven again to configure for snapshots.

6. **Configure User Credentials**:
   - The user credentials must be configured in a specific directory. 
   - This directory is located inside `.m2`, where Maven stores its dependencies.
   - Inside `.m2`, create a directory for global Maven credentials (accessible for all Maven projects).
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Maven-Project-Config-and-Upload/5-User-credential.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Maven-Project-Config-and-Upload/6-Maven-Global-Credential.png)

7. **Create `settings.xml`**:
   - Use the command `vim settings.xml` to create the file.
   - Set the credentials for the Nexus user and define the same repository `id` that you used in `pom.xml`.
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Maven-Project-Config-and-Upload/7-Set-credential.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Maven-Project-Config-and-Upload/8-Set-credential.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Maven-Project-Config-and-Upload/9-Set-credential.png)

8. **Upload the Jar to Nexus**:
   - All is set to upload the Jar file to Nexus.
   - First, build the Jar file.
   - Navigate to the project and run the Maven command to build it (`mvn build`).
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Maven-Project-Config-and-Upload/10-Creating-the-build.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Maven-Project-Config-and-Upload/11-Creating-the-build.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Maven-Project-Config-and-Upload/12-Creating-the-build.png)
   - The target folder will contain the Jar file.
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Maven-Project-Config-and-Upload/13-target-jar.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Maven-Project-Config-and-Upload/14-target-jar.png)

9. **Upload the Jar**:
   - Use the `mvn deploy` command to upload the Jar to Nexus.
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Maven-Project-Config-and-Upload/15-upload-jar.png)
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Maven-Project-Config-and-Upload/16-upload-jar.png)
   - You can now see the project Jar in the Browse section under `maven-snapshots`.
   - ![Alt Text](Nexus-Artifact-Repositorie-Manager/Maven-Project-Config-and-Upload/17-Jar-in-browser.png)

