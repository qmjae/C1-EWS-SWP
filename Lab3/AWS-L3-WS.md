# C1 WS Protection Modules

## TASK 1 | ANTI-MALWARE MODULE
### TASK 1A : Anti-Malware On-Demand Scan

1. Install and activate a Deep Security Agent without any policy.

    ![alt text](image.png)

1. Before assigning a policy, open a browser and navigate to www.eicar.org. Download one of the test eicar files and put it under temp folder (%temp%).

    ![alt text](image-1.png)

1. Assign a security policy to the Agent. Make sure that Anti-Malware is enabled.

    ![alt text](image-2.png)

1. Select the computer, and do “Scan for Malware”.
1. The Agent status will change to “Anti-Malware Manual Scan in Progress”.

    ![alt text](image-4.png)

1. Open the computer details screen after the scan has finished and check the Anti-Malware Events.

1. An Anti-Malware event will be retrieved with information about the eicar test file that was detected by Manual Scan.

    ![alt text](image-6.png)

    ![alt text](image-5.png)


Results:

The EICAR file was deleted, in this case, the eicar_com.zip located in the Temp file was located successfully. I tried doing the Quick Malware Scan and it wasn't detected, but doing the Full scan for malware did the job.

### TASK 1B : Anti-Malware Real-Time


1. Log into web console then assign a policy with the Anti-Malware Real-Time Scan enabled.

    ![alt text](image-7.png)

1. Open a browser, navigate to www.eicar.org, and download one of the test eicar files.

1. With real-time scan enabled, the file should be cleaned immediately.

    ![alt text](image-8.png)

1. Open the computer details screen after the scan has finished and check the Anti-Malware Events.

1. An Anti-Malware event will be retrieved with information about the eicar test file that was detected by Real-time Scan.

    ![alt text](image-9.png)

```bash
### TASK 1C : Anti-malware SPYWARE/GRAYWARE Exclusion -> Spycar_test.zip not avail

1. Copy and extract the `Spycar_tests.zip` package to the protected desktop.(Spycartestfilesareforinternaluseonly)

1. Input the password “N0virus1!” to extract.

1. Go to web console and double check the specific machine for Anti-Malware events.

1. Right click the events and click “Allow” if you want to add it to exclusion list.

1. Specify which scope that you want to apply, the computer only, or the policy level.

1. Test and verify if the exclusions work.

1. Go to Anti-Malware > Advanced and the entry should be added.
```
### TASK 1D : Anti-Malware Container Protection


1. Assign a policy with Anti-Malware enabled. Make sure that Anti-Malware Real-time scan for Container Protection is enabled.

    ![alt text](image-16.png)

1. Install Docker.
1. Check Docker version.

    ![alt text](image-11.png)
    ![alt text](image-12.png)

1. On this example, we will be installing an Ubuntu container.

    ![alt text](image-13.png)

1. Install vim.

    ![alt text](image-14.png)

1. Create a text file containing eicar string.

    ![alt text](image-15.png)

1. File should be cleaned up by Anti-Malware. Check Anti-Malware events for confirmation.

    ![alt text](image-17.png)
    ![alt text](image-18.png)
    ![alt text](image-19.png)

## TASK 2 | INTEGRITY MONITORING MODULE

### TASK 2: Detecting Changes in Files

1. Assign a policy to the client you would like to protect. Make sure Integrity Monitoring is enabled, a recommendation scan has been done and rules applied.

    ![alt text](image-20.png)
    ![alt text](image-21.png)
    ![alt text](image-22.png)

1. Double click on the Computer in the Computers screen and navigate to the Integrity Monitoring section.
1. Open the Baseline Viewer (View Baseline button).

    **Note**: As per TM's Docs, viewing baseline is removed (July 12, 2021) from WS console for improved performance. Reference: `https://docs.trendmicro.com/en-us/documentation/article/trend-micro-cloud-one-main-integrity-monitoring-baseline-remo`; However, we can generate a diagnostic package to see the baseline. Use `dsa_control -d` to generate diagnostic package and then locate in `C:\Program Data\ Trend Micro\Deep Security Agent\diag\*.zip\Agent\im\im_baseline.txt`

1. You should see a list of files. If you don't, click the Rebuild Baseline button to rebuild it (this can take a while).

    ![alt text](image-23.png)

1. On the machine/VM you are protecting, edit the hosts file (depending on the operating system, you may need to be the Administrator). Add a line like: “# this is a test.” and save the hosts file.

    ![alt text](image-24.png)

1. Return to the Integrity Monitoring page on the computer's Details page and Scan for Integrity.

    ![alt text](image-25.png)

1. When the Scan for Integrity button becomes active again, navigate to the Events tab and click the “Get Events” button.

1. There will be an event retrieved that indicates that the hosts file was modified.

    ![alt text](image-26.png)
    ![alt text](image-27.png)

    **Note**: IM checks or compares the baseline to the newly created file. In the image above, the attributes or entites checked were the `Last Modified`, `SHA-1`, and the `Size`. We didn't actually see the plain text or the changes, we saw only the differences or the values that were changed because of modification.

## TASK 3 | APPLICATION CONTROL
### TASK 3: Application Control

1. Install the DSA on Linux machine. Turn on the application control feature then wait for the application control inventory scan to be completed.

    ![alt text](image-30.png)
    ![alt text](image-28.png)

1. Create an sh file with sample content below then execute it. It should be blocked.

    ![alt text](image-31.png)

1. It should be recorded in the Application Control events.

    ![alt text](image-32.png)

1. Click “Change Rules” then verify under Policies > Common Objects > Rules > Application Control Rules > Software Rulesets if a rule has been added. If yes, run the file again. It should succeed.

    ![alt text](image-33.png)
    ![alt text](image-34.png)
    ![alt text](image-35.png)

## TASK 4 | LOG INSPECTION
### TASK 4A: Log Inspection Events

1. Assign an appropriate security policy to the computer you would like to inspect.

    ![alt text](image-36.png)

1. Go to Log Inspection and select a rule that matches an application installed on the agent/computer. 

    ![alt text](image-37.png)

1. Configure the rule to monitor the proper log file, set the file type and the kinds of events you would like to monitor. 

    `Nginx`
    
    ![alt text](image-38.png)

    `Docker`
    
    ![alt text](image-39.png)

1. Apply the changes, save to the policy and update the computer. 

1. Test by triggering the events defined in the rule (ie. Generate a 400 or 503 error code event) 

1. Run “get events” on the agent to pull latest events.

1. Check the Log Inspection Events for corresponding entries

    ![alt text](image-42.png)


## TASK 5 | SMART FOLDERS
### TASK 5: Managing Protected Computers

1. Login to the Cloud One Workload Security console, go to “Computers” tab and click “Smart Folders”. Click “Create a Smart Folder”.

    ![alt text](image-43.png)

2. Input the Name and select “Operating System”, “CONTAINS”, ”Microsoft Windows Server 2008 R2 ” and then “Save”.  

    ![alt text](image-44.png)

3. If you have Windows Server 2008 R2 machine installed, It will be listed in smart folders.

       **Note:** In this case, Windows Server 2022 is listed in the smart folder.

    ![alt text](image-45.png)


## TASK 6 | WEB REPUTATION MODULE
### TASK 6 : Block Websites with low reputation

1. Go to the Computers section and double-click on a computer to test on.

    ![alt text](image-46.png)

1. Select Web Reputation from the left hand menu and go to the General tab.

1. Change the Web Reputation state to “On” and select a security setting. Note the difference in the results. High security will be more aggressive in blocking sites.

    ![alt text](image-47.png)

1. Try navigating to http://wrs21.winshipway.com page and note if the page is blocked.

1. You should see a pop up notification when the site has been blocked.

    ![alt text](image-48.png)

1. View Web Reputation Events by selecting the computer, right click > Action > Get Events.

    ![alt text](image-49.png)

1. Check your browser if the Trend Micro Toolbar is included on your browser extensions. The logo appears to the right of the website address field.

    ![alt text](image-50.png)

Testing Tip

- Test the Allowed and Blocked settings under Exceptions.

    ![alt text](image-51.png)

    `Allowed`

    ![alt text](image-52.png)

    `Blocked`

    ![alt text](image-53.png)
    
- Test the custom blocking page and notifications.

**Observation**: 

![alt text](image-54.png)

Can access the page blocked thru incognito. Why? Because Web Rep relies on browser extensions to intercept and analyze web traffic. Without extension it will work. 

Solution:

1. Enable Extension in Incognito Mode (Works)
2. Disable Incognito Mode (Via Regedit)

```bash
REG ADD HKLM\SOFTWARE\Policies\Google\Chrome /v IncognitoModeAvailability /t REG_DWORD /d 1
```


## TASK 7 | RECOMMENDATION SCAN
### TASK 7A : Assigning rules with recommendation scan

1. From the Computers tab, select a computer from the list and right-click "Actions" and select "Scan for Recommendations".

    ![alt text](image-55.png)

    ![alt text](image-56.png)

1. To check recommendations, view the Computer Details page and select the "Intrusion Prevention" section in the left menu.

    ![alt text](image-57.png)

1. Under the General Tab, go to the section “Assigned Intrusion Prevention Rules” and click on “Assign/Unassign”.

1. Filter the list by "Assigned", "Recommended for Assignment" or "Recommended for Unassignment".

    `Assigned`

    ![alt text](image-58.png)

    `Recommended for Assignment`

    ![alt text](image-59.png)

    `Recommended for Unassignment`

    ![alt text](image-60.png)

Testing Tip

- Try with IPS inspection rules, integrity monitoring rules and log inspection rules.

    `IM Rules`

    ![alt text](image-61.png)
    
    `LI Rules`

    ![alt text](image-62.png)

- Test assigning recommendations, patching up the system, running another scan and un-assigning recommendations.


### TASK 7B : Automatic rule assignment
1. To properly test this feature, you will need a VM that has no assigned IPS/IM/LI rules assigned in the policy level.

1. To configure this setting click the Computer > Details screen and select “Intrusion Prevention

1. In the “Recommendations section”, select “Yes” from the dropdown to “Automatically implement Intrusion Prevention Rules Recommendations”, and click “Save”

1. Click “Scan for Recommendations”

    ![alt text](image-63.png)

## TASK 8 | FIREWALL MODULE
### TASK 8A : Firewall IPv4

1. Assign a policy to the VM you would like to protect and enable the Firewall module.

    ![alt text](image-64.png)

1. Click “Assign/Unassign …” and then create a new Firewall rule and named it as “Deny Ping”.

1. You may deny the ping packages to single IP address and set the direction to outgoing. Apply this rule to specific protected VM.

    **Before from Windows to Ubuntu**

    ![alt text](image-66.png)

    **Denying Ping to Ubuntu**
   
    ![alt text](image-67.png)

1. Ensure that the firewall is on and that the engine is operating in inline mode

    ![alt text](image-68.png)

1. Set the VMs “Firewall Stateful Configuration” to “No Stateful Inspection”. This is just to make sure the ICMP Stateful affects the testing results.

1. Save and update the setting to your VM.

    ![alt text](image-69.png)

1. Test and see if traffic is blocked, allowed, bypassed or logged depending on what you have set.

    **Note**: Traffic is blocked. We set a FW rule that blocks outgoing traffic to a specific machine. In the image below, we can see the ping request from Windows to the Ubuntu server timed out. This implied that the FW rule worked. 

    ![alt text](image-70.png)

    In addition, checking the Firewall events  triggered the rule that we created.

    ![alt text](image-71.png)
    ![alt text](image-72.png)

1. Test the other actions and compare the results. Check for firewall events in the events tab to see the results.

    `Allow Rule`

    ![alt text](image-73.png)


    **Note**: Creating an Allow action should still be denied because by default, the Allow action is at the lowest priority.

    Now, there's an issue here, I can't RDP now. Why? Because we created an Allow rule which made the Firewall operation a restrictive one. Therefore, we need to use "Force Allow" to avoid lockouts. 

    `Bypass Rule`

    ![alt text](image-74.png)

    ![alt text](image-75.png)

    **Note**: Creating a Bypass rule with a higher priority than the Deny rule will result into allowing the packet to pass through the outgoing firewall. The expected result is that the Windows Server should be able to ping again the Ubuntu server

    `Modifying Deny Rule with same priority with Bypass Rule`

    ![alt text](image-76.png)

    **Note**: The expected result should be the outgoing ping request from Win server should still pass through the firewall because both have the same priority, the top to bottom action hierarchy should be followed. The hierarchy is bypass -> log only -> force allow -> deny -> allow. 

    `Inbound Rule`

    ![alt text](image-77.png)

    ![alt text](image-78.png)


    **Note**: Creating an inbound rule with deny action and a priority of 3. Expected result is that Ubuntu can't ping request to Windows Server.

    `Force Allow Inbound Rule`

    ![alt text](image-80.png)
    
    ![alt text](image-79.png)

    **Note**: Creating an force allow inbound rule with a priority of 3. Expected result is that Ubuntu can ping request to Windows Server. Because if priority matches, proceed to action hierarchy.

### TASK 8B : Stateful firewall configuration

1. On the manager console, select a computer and add the SSH rule to the current firewall rules.

Note:

a. You can also add the rule via Policy/Security Profile to apply it on multiple machines.

b. Use the pre-created SSH rule that allows incoming SSH traffic.

![alt text](image-82.png)

c. There should be no firewall rule on outgoing SSH.

![alt text](image-83.png)

d. The stateful Inspection should still be disabled at this point. (from Task 8A #5)

![alt text](image-84.png)
    

2. From another computer, try to connect to the target computer using an SSH application such as Putty.

3. Check the firewall events. This should appear as "Out of allowed Policy".

    ![alt text](image-85.png)

4. Go back to the Computer/Policy view, and then go to Firewall.

5. Under Firewall Stateful Configuration, select Enable Stateful Inspection.

6. Click Save.

7. Connect again via SSH.

## TASK 9 | INTRUSION PREVENTION MODULE
### TASK 9 : IPS restrict download over HTTP

1. Assign the rule to restrict the download of EICAR file in the IPS Rules Assign/Unassign page.

1. From the Cloud One Workload Security console, double-click the protected machine.

1. Click Intrusion Prevention > Assign/Unassign

1. Enable the Rule ID 1005924 Restrict Download of EICAR Test File Over HTT

    ![alt text](image-86.png)

1. Download the EICAR file to the protected machine.

    ![alt text](image-87.png)
    
1. Check the IPS events to ensure it logs the event for blocking the EICAR file.

1. From the DSM console, double-click the protected machine.

1. Go to Intrusion Prevention > Events.

1. Click Get Events. When you find the detailed events for blocking the EICAR file, it means the IPS module works fine.

    ![alt text](image-88.png)
    ![alt text](image-89.png)

    **Note**: Using wget or curl, we can see the contents of the file. If IPS enabled, it'll be blocked. See contents below the IPS disabled and IPS enabled:

    `IPS enabled`

    ![alt text](image-90.png)

    `IPS disabled`
    
    ![alt text](image-92.png)