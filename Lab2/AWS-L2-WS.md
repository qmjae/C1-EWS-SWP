# C1 WS Console Common Tasks

## T1 - System Events

2. Navigate to Computers > Compute editor >  Overview > System Events and identify the date and time the computer was activated.

    `Win2022 Private`
    ![alt text](image.png)

    `Win2022 Public`
    ![alt text](image-1.png)

    `Ubuntu Server`
    ![alt text](image-2.png)

3. Provide the Event ID for Activated.

    `Win2022 Private`
    EID - 704

    `Win2022 Public` 
    EID - 704

    `Ubuntu Server`
    EID - 704

**Note**: EID of Activated Agents are 704. This checks out and indicates that the Deep Security Agent has been successfully activated. While EID 702 is for *Credentials Generated* and EID 705 is for *Activation Failed (Error)* this occurs when agent self-protection is enabled and prevents activation


## T2 - Policies

1. Login to the Cloud One - Workload Security Console

    a. Duplicate the Windows Server 2012 Policy and update its name to Windows Server 2019

    ![alt text](image-3.png)

    b. In the Windows 2019 Policy Configuration, Set the Agent Self Protection to Inherited

    ![alt text](image-4.png)

    c. Save the changes

2. Assign the Windows Server 2019 Policy to your computer and make sure that the Anti-Malware module is enabled

    ![alt text](image-5.png)

3. From the Policy Tree, update the top policy for Windows with the Policy name "Windows"

    a. Go to the Agent Self Protection Settings and change the current setting from "Inherited(No)" to Yes
    
    b. In the same window, set the Local override requires password to "Yes" and provide a password.

    c. Save the changes

    ![alt text](image-7.png)

4. RDP to your computer

    a. Open Services.msc and stop the Deep Security Agent Service

    ![alt text](image-9.png)

    **Note:** Can't stop DSA because Agent Self Protection Settings is Enabled

5. Open CMD and run the command manually send a heartbeat command. You may use the dsa_control command line utility.

    ![alt text](image-10.png)

    **Note:** Use `dsa_control -m`
    Can't

6. Provide 2 ways to disable the agent with self protection enabled without updating the configuration in the web console

    **Note:** For **Windows**, use `dsa_control --selfprotect=0 -p <password>`; For **Linux**, use `dsa_control --selfprotect=0 -p <password>`

    OR

    `dsa_control -r` // not usually used because this removes all config "reset"

    ![alt text](image-12.png)

**Best Practice:**
- Temporarily disable self-protection only when necessary
- Re-enable immediately after completing required tasks
- Set strong passwords for local override capability

## T3 - Diagnostic Package

2. From the Web Console, Select the Computer and Generate the Diagnostic Package from the Action Tab

    ![alt text](image-11.png)
    ![alt text](image-13.png)

3. RDP to the Computer and open a command Prompt.

4. Using dsa_control, generate a diagnostic package.

    ![alt text](image-14.png)

5. Collect both diagnostic package and compare the content

    ![alt text](image-15.png)

    **Note**: The folder that has the name "diagnostic" is the Web Console Generated, and the numbers only is the directly generated on the agent computer

6. Based on your knowledge, list the major difference of these two packages and explain how they would be useful for your troubleshooting.

**Difference:**

![alt text](image-16.png)
![alt text](image-17.png)

Based on this directory/file structure, I've observed that obviously, the agent computer generated only generated the Agent side of the diagnostic, on the other hand, the Web console generated contained an Agent and Manager folder.

![alt text](image-18.png)
![alt text](image-19.png)

Based on this file comparison on both the Agent and Web Console generated logs, I've found out that the error log is the same and no differences were encountered here.

![alt text](image-20.png)
Moving on to the Agent folder itself, using the `dir` command, I was able to see that the Web console generated more files compared to the Agent generated one. 

![alt text](image-21.png)
Creating a script shows the difference between the two generated directories. The additional files are the Web Console generated. This may be because of the additional checkboxes that is prompted in the Web Console UI, therefore additional files.


**Note:**
1. If the agent computer is configured to use Agent/Appliance Initiated communication, then the manager cannot collect all the required logs.
2. In this case, its better to generate locally so that the Agent logs are complete.

## T4 - Agent Version Control/Agent Upgrade


2. Go to the Agent Version Control Settings and set the Windows Servers to use the "Latest LTS"

    -> Go to Administration -> Software -> Agent Version -> Find Windows Server -> Latest LTS

    ![alt text](image-22.png)

3. Deploy a new agent using deployment script on a Windows Server 2019 Instance.

    ![alt text](image-23.png)
    ![alt text](image-24.png)

4. RDP to the Computer and verify the agent version installed on the system.

    ![alt text](image-25.png)
    
    **Note**: Plugin Version of the Update is 20.0.2-29760 for Windows LTS

    ![alt text](image-26.png)

5. Deactivate and Uninstall the agent.

    ![alt text](image-27.png)
    ![alt text](image-28.png)

6. Go back to the Agent Version Control Settings and update the Windows Servers to Select by Platform

    ![alt text](image-29.png)

7. Set the Windows Server 2019 to use the 20.0.0.6860 agent version.

    ![alt text](image-30.png)

8. Deploy the agent using deployment script on the Windows Server 2019 Instance.

    ![alt text](image-31.png)
    ![alt text](image-32.png)

9. RDP to the Computer and verify the agent version installed on the system.

    ![alt text](image-33.png)
    ![alt text](image-34.png)
    **Note**: Plugin Version of the Update is 20.0.0-6860 for Windows 2019 only

10. Upgrade the Agent to the latest version available via the Clouds One Workload Security console.

    ![alt text](image-35.png)
    ![alt text](image-36.png)
    ![alt text](image-37.png)

11. Generate the diagnostic package and from the information collected, Identify the version of the agent installed.
    
    **After Updating**

    ![alt text](image-38.png)
    ![alt text](image-39.png)
    ![alt text](image-40.png)


**Reference:**
`https://help.deepsecurity.trendmicro.com/20_0/on-premise/communication-manager-agent.html#Configur`
`https://help.deepsecurity.trendmicro.com/20_0/on-premise/command-line-interface.html?Highlight=Diagnostic%20generation%20in%20progress`