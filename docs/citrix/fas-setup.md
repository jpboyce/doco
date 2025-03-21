---
title: FAS Setup
date: 2022-08-17
status: deprecated
categories:
    - Citrix
tags:
    - Citrix
    - Deprecated
---

# FAS Setup

!!! warning "Deprecated Content"

    This content relates to the Citrix Xenapp Essentials product, which has been discontinued

1. Run the installer and complete it
2. Load the Administration Console

    ![Image](../images/7a5fe3e2-87c8-44e3-a64e-5453edbc5c6e.png)

3. Click on the Deploy certificate templates item.  You will see a prompt about the certificates that will be installed.  Click OK.  The status should update with a green tick if successful

    ![Image](../images/de699500-0410-4388-8c2d-6ad86870c1bb.png)

4. Click on the Publish button for the 2nd item.  A prompt will appear again with info about the cert templates and to select the Certificate Authority to publish to.  Click OK to complete the deployment.

    ![Image](../images/a9228df2-58d5-4706-acc7-e629d63be776.png)

5. If you manage the certificate authority, the cert templates should appear as available when this step is completed

    ![Image](../images/11127331-d2e2-415f-a79a-89d953ca89df.png)

6. Click on the Authorise button.  A confirmation window will appear, select the CA and click OK.  The step will sit in a pending state (spinning waiting icon).

    ![Image](../images/d26c5099-d869-45e5-a829-51a335763e67.png)

7. Go to your CA MMC and navigate to Pending requests

    ![Image](../images/09fb91ee-1235-47d3-977a-af50a21c14c1.png)

8. There should be a pending request from the FAS server.  Right-click and select All Tasks > Issue

    ![Image](../images/96cab920-67e5-4c82-a30e-4cdeda03921b.png)

9. The Authorise set in the FAS setup should update with a positive result.
10. Click on the Create button for the Create a Rule task.  This will start the Create a Rule wizard.  For now, select the creation of a default rule.

    ![Image](../images/abd8a433-e3d5-4890-b64f-6ac24ad9fdf2.png)

11. Once the wizard has been completed, click on the Connect button to connect to Citrix Cloud.  The wizard will start.

    ![Image](../images/f1ed2fd2-20c7-40c6-bcb1-b23ac0d7efe7.png)

12. As per the instructions, copy the Confirmation URL into a browser with internet access and the code.

    ![Image](../images/98ce1fce-e585-4ab0-8b48-4bfa838d7bc7.png)

13. In the browser, click on Register button

    ![Image](../images/937bd851-6c4d-4673-8342-9e79c774a4b8.png)

14. Enter the confirmation code

    ![Image](../images/2cbd5ecd-cbef-423f-a91b-28f270e18ff2.png)

15. A confirmation screen will appear with details of the registration.  Review and click Register

    ![Image](../images/1cc0b63f-be44-40ab-b01e-fd725d98fcd4.png)

16. Once the registration is completed on the Citrix Cloud side, the FAS wizard should update with the resource location to select.

    ![Image](../images/25ca280c-1a59-4130-b4f5-4f1165ce5f44.png)

17. In the Choose a rule prompt, select the appropriate option (most likely use an existing rule since we created the default one)

    ![Image](../images/b1a3cdd4-3d97-4ce5-a0bf-e5cce211f17d.png)

18. Review the summary screen and click Finish
19. Setup a GPO that sets the FAS Server for appropriate targets as per Install and configure | Federated Authentication Service (citrix.com)
