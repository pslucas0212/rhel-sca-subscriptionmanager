# RHEL - Using Subscription Manager with Simple Content Access Enabled

Updated - 11 May 2021

- Simple Content Access (SCA) allows you to register Red Hat software to access Red Hat software content without attaching a subscription to a particluar system or environment.  A great use case for SCA is when you want to manage the content of public cloud market place instances of RHEL with Red Hat Smart Management. SCA enables you to use a Red Hat Smart Management subscription with a public cloud market place instances of RHEL without consuming a RHEL subscription (no double counting subscription usage).  You do need a Red Hat Smart Management subscrtption to support each public cloud market place instance of RHEL that will use Red hat Smart Management.

- With SCA enabled, you no longer can view in the Red Hat Customer Portal subscription section the consumption of particluar add-ons used with your RHEL instances.  You can see which Repos are attached to a particular instance.  If you want to see the consumption of a particular add-on like the Extended Life Cycle subscription, you can attach that specific subscription to a RHEL instance.

- Insights is great tool for seeing and managing RHEL content, and automaticaly patching and remediating your RHEL instances.  Insights works with RHEL 6.4+, 7.0+ and 8.0+.  I would recommend enabling Insights regardless of your content subscription strategy.  

### Enable SCA on your Red Hat customer portal account
- Login in to the Red Hat Customer Portal  -> [Login](https://access.redhat.com/)

![Red Hat Customer Portal login page](/images/S01.png)

- On the Red Hat Customer Portal page, click the Subscriptions link in the upper left corner.

![Red Hat Customer Portal page - Subscriptions](/images/S02.png)

- On the Overview page slide the switch next to the Simple content access for Red Hat Subscription Management text to the right to enable SCA.  The background of the slider switch will turn blue and the text will change from Disabled to Enabled

![Simple Content Access Disabled](/images/S03.png)

![Simple Content Access Enabled](/images/S04.png)

---
### Create an activation key with no subscriptions attached. 

- For registering RHEL with Red Hat Subscription Management, I recommend using an Activation Key insteading linking the registration to a user.
- Click the Manage drop down link near the upper right of the Red Hat Subscription Mangement page and chose ActivationKeys.

![Chose Managae | Activation Keys](/images/S05.png)

- Click the New button on the right side of the Activation Keys for Organization ID:xxxxxxxxxx page to create a new Activation Key.

![Click New button](/images/S06.png)

- On the New Activation Key page, fill in the Name text field with name of your activation key.  Chose a name that makes sense for your organization.  Chose Disabled from the Auto Attach drop down.  Scroll to the bottom of the page and click the Create button.

![Create New Activation Key](/images/S07.png)

- Your activation key is ready to use for registering systems with SCA.

![New Activation Key Availabe](/images/S08.png)

---
### Registering System, Adding Repos and Enabling Insights
- Register a system via subscription manager for RHEL 6.0+, 7.0+ and 8.0+l  Note: You will need to use sudo or be root to execute these comands.

      # subscription-manager register --org=xxxxxxxxx --activationkey=sca_enabled 
      
- Verify that the registered system content access mode is set to SCA.  Note: You will only see the SCA status on RHEL 7.0+ and RHEL 8.0+

      # subscription-manager status
      
- Check which repos are enabled for RHEL 7.0+ and 8.0+

      # subscription-manager repos --list-enabled
     
- Repo enablement example

      # subscription-manager repos --enable rhel-6-server-els-rpms    
      
- You can still attach specific subs (ELS subscription) if you want to track their subscription usage.  Find the Pool ID of the subcription you want to attach, and then subscribe to that pool.  Use subscription-manger list to see that the correct subscription is attached to your RHEL instance.  You can also validate this on your Red Hat Customer Portal page under Subscriptions.

      # subscription-manager list --available
      # subscription-manager subscribe --pool=xxxxxxxxxxxxxxxxxxxxxxxx
      # subscription-manager list
      

- Insights setup for RHEL 8

      # insights-client --enable
      
- Insights setup for RHEL 6.4+ and 7.0+

      # yum -y install insights-client
      # insights-client --enable
      
---     
### Review your registered Systems on the Red Hat Customer Portal

---
### Reference

- How do I enable Simple Content Access for Red Hat Subscription Management? - in the [Simple Content Access article](https://access.redhat.com/articles/simple-content-access)
- [Subscription Manager Command Cheat Sheet](https://access.redhat.com/sites/default/files/attachments/rh_sm_command_cheatsheet_1214_jcs_print.pdf)
