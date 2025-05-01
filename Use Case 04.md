# Use Case 3 – Set up Git integration for your Copy job in Fabric Data Factory

This hands-on lab guides you through implementing CI/CD (Continuous
Integration and Continuous Deployment) for a Copy Job in Microsoft
Fabric Data Factory. You will learn how to build, export, and deploy
Data Factory pipelines using Git integration, deployment pipelines, and
workspace publishing. The lab emphasizes automating the movement of data
pipelines across development, test, and production environments—enabling
faster, more reliable data integration workflows.

## Exercise 1 – Prerequisites for Git integration

Make sure you have enabled the **fabric capacity** in your tenant.

### **Task-1: Enable tenant switches from admin portal**

1.  ![](./media/image1.png)On the Microsoft Fabric portal, navigate to
    the **settings** option available in the top right pane. Select
    **Admin Portal** settings.

2.  ![](./media/image2.png)In the **Tenant** **Settings**, enable the
    **Users can create Fabric items** switch and click on **Apply**.

3.  ![](./media/image3.png)In the tenant settings under **Git**
    **integration** section, make sure **Users can synchronize workspace
    items with their Git repositories** switch is enabled

4.  ![](./media/image4.png)Under **Git integration** settings, make sure
    **Users can sync workspace items with GitHub repositories** switch
    is also enabled.

### **Task-2: Create a GitHub account**

Complete the following steps to create a new **Github account** with the
same tenant credentials.

1.  ![](./media/image5.png)Navigate to the GitHub with this link
    <https://github.com/> and click on **Sign up** to proceed further.

2.  ![](./media/image6.png)Enter the **email**, **password** and a
    unique **username** and click on **Continue** button.

3.  ![](./media/image7.png)Start the **verification** **puzzle** by
    following the instruction on the screen. Click on **Submit.**

4.  ![](./media/image8.png)Enter the **verification** **code** you’ve
    received on your mail.

5.  ![](./media/image9.png)Now, with your credentials sign-in to GitHub
    and click on **Sign in.**

6.  ![](./media/image10.png)You have successfully created a new account
    on GitHub.

### **Task-3: Create a Repository in GitHub**

1.  ![](./media/image11.png) After successfully setting up GitHub
    account login to your account, click on **Create Repository** under
    create your first project.

2.  After clicking **new repository** option, we will have to initialize
    some things like, **naming our repository**, choosing
    the **visibility, Add a README file** etc. After performing these
    steps click **Create Repository** button.

3.  ![](./media/image16.png)After clicking the button, we will be
    directed to below page. Right now, the only file we have is a
    **readme** file.

4.  To create a **new folder** in GitHub, you must first create a new
    file and add that file to your new folder at the same time. Do this
    by navigating to your **repository page**.

> ![](./media/image17.png)Next, click the **Add file** dropdown menu on
> the right. Select the first option labelled "**Create new file."**

5.  ![](./media/image18.png)To create a folder, provide a name to this
    **folder** **(Copyjob-Test)** as follows. The folder name will
    automatically generate a new folder. Now you can give your **file a
    name** **(CJ-Contents).**  
    Type the **folder name followed by /** and hit Enter.

6.  You'll need to **commit** your changes. Give a commit message and
    **select commit directly to the main branch** option. Click on
    **Commit changes**.

> ![](./media/image19.png)

### **Task-4: Generate a fine-grained token with *read* and *write* permissions for Contents, under repository permissions**

1.  ![](./media/image20.png)Go to your **GitHub profile settings** and
    select **settings** option.

2.  ![](./media/image21.png)Navigate to **Developer settings**.

3.  ![](./media/image22.png)Select **Personal access tokens**, and
    Choose **Fine-grained tokens**. Click on **Generate new token.**

4.  ![](./media/image23.png)Provide a descriptive **name** and optional
    **description** for the token. Set an **expiration** **date** for
    the token. Choose the **resource owner** as your GitHub username.

5.  ![](./media/image24.png)Choose the **specific repository** that you
    have created as **Copyjob-Dev** you want the token to access.

6.  **Specify Access Permissions:**

    - **Contents:** Select "Read and write" access for the "Contents"
      permission.

    - **Other Permissions:** You can also configure other permissions as
      needed, such as "Issues" or "Pull Requests".

![](./media/image25.png)

7.  ![](./media/image26.png)Click **Generate token.**

8.  **Important:** **Copy and securely store the generated token, as it
    will not be displayed again. **

![](./media/image27.png)

## Exercise 2 – Connect to a Git repository

To use Git integration with Copy job in Fabric, you first need to
connect to a Git repository, as described here.

1.  ![](./media/image28.png)Sign-in into Fabric and navigate to the
    workspace you want to connect to Git. Select **Workspace settings**.

2.  Select **Git integration**. Select your Git provider. Currently,
    Fabric only supports *Azure DevOps* or *GitHub*. If you use GitHub,
    you need to select **Add account** to connect your GitHub account.

![](./media/image29.png)

3.  ![](./media/image30.png)In Add **GitHub account page**, provide a
    **display name** for your account. In **personal access token**,
    paste the copied **fine-grained token** that we have generated from
    the GitHub and at last, in **repository URL**, copy the url from the
    GitHub. Click on **Add**.

4.  ![](./media/image31.png)After you sign in, select **Connect** to
    allow Fabric to access your GitHub account.

## Exercise 3 – Connect to a workspace

Once you connect to a Git repository, you need to connect to a
workspace, as described here.

1.  From the dropdown menu, specify the following details about the
    branch you want to connect to:

    - **Branch**: Specify the branch as ***main***.

    &nbsp;

    - **Folder**: The GitHub folder name that we have created earlier as
      ***Copyjob-Test.***

Select **Connect and sync**.

> ![](./media/image32.png)

2.  ![](./media/image33.png)After you connect, the Workspace displays
    information about **source control** that allows users to view the
    connected branch, the status of each item in the branch, and the
    time of the last sync.

## Exercise 4 – Commit changes to Git

**Note**: **These uncommitted changes will already by shown as synced if
you have created a deployment pipeline in your workspace only. It’ll
automatically sync the items in the workspace. If it’s showing
uncommitted changes then follow these steps.**

You can now commit changes to Git, as described here:

1.  Select the **Source control** icon. This icon might show the number
    of **uncommitted changes**.

2.  Select the **Changes** tab from the **Source control** panel. A list
    appears with all the items you changed, and an icon indicating the
    status.

3.  Select the items you want to **commit**. To select all items, check
    the top box.

4.  (Optional) Add a **commit** **comment** in the box.

5.  Select **Commit**.

![](./media/image34.png)After the changes are committed, the items that
were committed are removed from the list, and the workspace will point
to the new commit that it synced to.

## Exercise 5 – Get Started with deployment pipelines for Git

The objective of this exercise is to help you understand how to
**configure and use deployment pipelines with Git-enabled workspaces**
in Microsoft Fabric Data Factory. You will learn how to link Git
repositories, publish changes to the Fabric service, and promote content
across **development**, **test**, and **production environments** using
**deployment pipelines**, ensuring a smooth and automated CI/CD process
for your data integration workflows.

Before you get started, be sure to set up the following prerequisites:

- An active Microsoft Fabric subscription.

- Admin access of a Fabric workspace.

### **Task-1: Create and name the deployment pipeline and assign stages**

1.  From the **Workspaces** page, select **Create** **deployment
    pipeline**.

![](./media/image35.png)

2.  In the **Create deployment pipeline** dialog box, enter a name and
    description for the pipeline, and select **Next**.

![](./media/image36.png)

3.  Set your **deployment pipeline’s structure** by defining the
    required stages for your deployment pipeline. By default, the
    pipeline has three stages: *Development*, *Test*, and *Production*.
    We are keeping it **as it is** for now. Click on **Create and
    Continue**.

![](./media/image37.png)

### **Task-2: Assign a workspace to the deployment pipeline**

After creating a pipeline, you need to add content you want to manage to
the pipeline. Adding content to the pipeline is done by assigning a
workspace to the pipeline stage. 

1.  ![](./media/image38.png)In the stage you want to assign a workspace
    to, expand the dropdown titled **Add content to this stage** and
    select the workspace and click on the **tick** icon.

2.  ![](./media/image39.png)While clicking on this tick icon, you might
    encounter this pop-up which says - **Workspace include unsupported
    items (Lakehouse).** Click on **Cancel.**

3.  To manage this, navigate to the **DataFactory-Fabric** workspace
    page. Beside the **lakehouse**, click on the **Share** option.

![](./media/image40.png)

4.  Here, you need to grant the permission to access Lakehouse. Enter
    you **tenant name** and check all the boxes in **additional
    permissions**. Click on **Grant**.

![](./media/image41.png)

5.  Now, navigate to the deployment pipeline **DataFactory-Pipeline**
    that you have created, again follow the step to **select the
    workspace** from the drop-down and click on the **tick** icon. All
    the resources are successfully deployed in development stage.

> ![](./media/image42.png)

### **Task-3: Deploy to an empty stage**

When you finish working with content in one pipeline stage, you can
deploy it to the next stage. Deployment pipelines offer three options
for deploying your content:

- **Full deployment**: Deploy all your content to the target stage.

&nbsp;

- **Selective deployment**: Select which content to deploy to the target
  stage.

&nbsp;

- **Backward deployment**: Deploy content from a later stage to an
  earlier stage in the pipeline. Currently, backward deployment is only
  possible when the target stage is empty (has no workspace assigned to
  it).

For this lab, we prefer **Full deployment**. You can consider
**Selective deployment** by altering your stage items that needs to be
deployed to the next stage.

1.  ![](./media/image43.png)Select the **Test** stage and check the
    boxes you want to deploy to the test stage and click **Deploy**
    button which displays the number of resources to deployed.

&nbsp;

2.  Give a **confirmation** to deploy to the test stage by clicking on
    **Deploy** button.

![](./media/image44.png)

3.  You’ll see that all the stage items are successfully deployed in
    **Test** stage.

![](./media/image45.png)

4.  ![](./media/image46.png)Select the **Production** stage and check
    the boxes you want to deploy to the test stage and click **Deploy**
    button which displays the number of resources to deployed.

&nbsp;

5.  ![](./media/image47.png)**Review and confirm** the deployment to the
    **production** stage by clicking on **Deploy** button. You can also
    add a note by expanding **Add a note** option.

&nbsp;

6.  ![](./media/image48.png)You’ll see that all the stage items are
    successfully deployed in **Production** stage.

### **Task-4: Deploy content from one stage to another**

1.  ![](./media/image49.png)You can review the **deployment history** to
    see the last time content was deployed to each stage. To examine the
    differences between the two pipelines before you deploy,
    see [Compare content in different deployment
    stages](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/compare-pipeline-content).

![](./media/image50.png)

## Conclusion

By completing this lab, you’ve seen how easy and powerful it is to
integrate source control and automate deployments in Microsoft Fabric.
You’re now equipped to streamline data operations and deliver changes
confidently across your environments.
