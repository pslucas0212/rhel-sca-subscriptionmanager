# RHEL - Using Subscription Manager with Simple Content Access Enabled

Updated - 11 May 2021

- Simple Content Access (SCA) allows you to register Red Hat software to access Red Hat software content without attaching a subscription to a particluar system or environment.  A great use case for SCA is when you want to manage the content of public cloud market place instance of RHEL with Red Hat Smart Management. SCA enables you to use a Red Hat Smart Management subscription with a public cloud market place instances of RHEL without consuming a RHEL subscription (no double counting subscription usage).  You need a Red Hat Smart Management subscrtption to support each public cloud market place instance of RHEL that will use Red hat Smart Management.

- With SCA enabled, you no longer can view Red Hat software consumption in the Red Hat Customer Portal subscription section.  You can see which repositories are attached to a particular instance.  If you want to see the consumption of a particular add-on like the Extended Life Cycle subscription, you can attach that specific subscription to a RHEL instance.

- Note: Even if you are not attaching a subscription to a Red Hat Software product, you are required to have an active subscription for every instance of that product even if the product is in Extended Phase Lifecycle.  For example, if you are running RHEL 4 or 5 along with any other current versions of RHEL, you are required to have active subscriptions to cover all those instances of RHEL.

- It is recommended that you use Subscription Watch to determine the number of RHEL instances that are being used vs. the number of subscriptions you are entitled to use.

- Insights is great tool for seeing and managing RHEL content, and automaticaly patching and remediating your RHEL instances.  Insights works with RHEL 6.4+, 7.0+ and 8.0+.  I would recommend enabling Insights regardless of your content subscription strategy.  

### Enable SCA on your Red Hat customer portal account
- Login in to the Red Hat Customer Portal  -> [Login](https://access.redhat.com/)

![Red Hat Customer Portal login page](/images/S01.png)

- On the Red Hat Customer Portal page, click the Subscriptions link in the upper left corner.

![Red Hat Customer Portal page - Subscriptions](/images/S02.png)

- On the Overview page, moved the slider switch to the right to enable SCA.  The background of the slider switch will turn blue and the text will change from Disabled to Enabled

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
- Register a system via subscription manager for RHEL 6.0+, 7.0+ and 8.0+.  Note: You will need to use sudo or be root to execute these commands.

      # subscription-manager register --org=xxxxxxxxx --activationkey=sca_enabled 
      
- Verify that the registered system content access mode is set to SCA.  Note: You will only see the SCA status on RHEL 7.0+ and 8.0+

      # subscription-manager status
      
- Check which repositories are enabled for RHEL 7.0+ and 8.0+

      # subscription-manager repos --list-enabled
     
- Repository enablement example

      # subscription-manager repos --enable rhel-6-server-els-rpms    
      
- You can still attach specific subscriptions (a RHEL 6 ELS subscription for example) if you want to track their subscription usage.  Find the Pool ID of the subcription you want to attach, and then subscribe to that pool.  Use subscription-manger list to see that the correct subscription is attached to your RHEL instance.  You can also validate that the subscription is attached to your RHEL instance on your Red Hat Customer Portal page under Subscriptions.

      # subscription-manager list --available
      # subscription-manager subscribe --pool=xxxxxxxxxxxxxxxxxxxxxxxx
      # subscription-manager list
      
- Since you are not assigning a subscription to the RHEL instance, we cannot know if the RHEL instance should count against Premium, Standard or Self-Support subscription.  You can set the RHEL SLA level from the command line with Subscription Manager.  Setting the SLA level for each RHEL instance well help you to better understand your subscription usage when viewing subscription consumption in Red Hat Insights.  

- Service Level for RHEL 6.0+ and 7.0+ (premium, standard, self-support).

      # subscription-manager service-level --set=premium
      # subscription-manager service-level --show
              
- Service Level for RHEL 8.0+ (Premium, Standard, or Self-Support).

      # subscription-manager service-level --set=Premium
      # subscription-manager service-level --show
      
- Insights setup for RHEL 8

      # insights-client --enable
      
- Insights setup for RHEL 6.4+ and 7.0+

      # yum -y install insights-client
      # insights-client --enable
      
---     
### Review your Registered Systems on the Red Hat Customer Portal

- Note: Even with SCA enabled, you can attach susbscriptions to systems if for some reason you want to track and control the subscription usage of a specific add-on like an ELS subscription.
- Click on the Systems tab link in the Red Hat Customer Portal (Note: You need to be in the Subscriptions section on the Red Hat Customer Portal.)
- You'll note that there is a question mark (?) by each System Name. This happens when SCA is enabled as you no longer need to attach a subscription to a registered system.  If any subscrptions are attached to a registered system, the number of subscriptions attached wil be registered in the third column.

![Sytems View](/images/S09.png)

- To see details of a registered system, click on the system name and make sure you are on the Details tab page.  On the Details tab page you will notice that the Subscription Management status is unknown and that no subscriptons are attached to the RHEL instance.

![Sytems Detail Tab](/images/S10.png)

- Click on the Subscriptions tab.  You'll notice that there is no subscription information on the Subscriptions Tab page.

![Subscriptions View](/images/S11.png)

- Chose a RHEL system from the Systems page that has a subscription attached to that system.  You will now see the attached subscription and subscription detail for that system.

![Subscription with attached subscription](/images/S12.png)

- Back on the Red Hat customer Portal page chose the Subscriptions link.  In the Subscriptions page, scroll through the list and choose a subscription to review.  In that Subscription page make sure you are on the Overview tab page.  Here you can see the subscription quantity available and the number of subscriptions consumed.

![Subscriptions Overview](/images/S13.png)

- In the same subcription page click the Systems tab to see which RHEL instances have this subscription attached to them.

![Subscriptions Systems](/images/S14.png)

---
### Reviewing your RHEL Systems in Insights for RHEL 6 Extended Lifecyc Updates.

- We won't be reviewing all of Insights capabilites in this section, but we will review a couple of things in Insights so that you can check that your RHEL ELS 6 content is available to your RHEL system with the ELS reposiotry enabled.  Note: There is a time lag between when you register a system to Insights and when the Insights client has updated your Insights view.

- Login to Insights -> [Login](https://cloud.redhat.com/)
- On the Hybrid Cloud Console, chose the the Red Hat Enterprise Link on the left side of the screen

![Red Hat Hybrid Cloud Console](/images/S15.png)

- On the Insights "home page", you'll be presented with a Dashboard view of your Insights registered RHEL systems.

![Insights Dashboard](/images/S16.png)

- Click on the Inventory Link on the left to get a list of RHEL systems registered with Insights.
- I'm going to chose server03 as it is a RHEL 6.10 system with the RHEL 6 ELS repository enabled.  

![Inventory View](/images/S17.png)

- Now can view the details of server03 on the General Informaton tab page.  While on the General Inforfmation tab page, scroll down to see the number of repositories enabled for server03.  You'll note in this example there are two repositories enabled: one for RHEL 6 RPMs and for RHEL 6 ELS.

![server03 example](/images/S18.png)

![server03 repos link](/images/S19.png)

- Click on the Repositories link to see the Repositories enabled for this system. You can set a filter on the dialog box to see just the Enabled repositories.

![Enabled Repositories](/images/S20.png)

- If we now click on the Patch tab, we see that because we enabled the RHEL 6 ELS repository with server03, there is ELS related content available (dated after 1 December 2020) for server03

![server03 patch content](/images/S21.png)

- Now we will compare the content available to a RHEL 6 instance (server05) that does not have the RHEL 6 ELS repository enabled.  We first review the repositories enabled on server05 and see that only one repository, RHEL 6 RPMs, is enabled

![server05 enabled repositories](/images/S23.png)
 
- Next we go to server05 Patch tab page.  We can see that there is no new content available to this server since 1 December 2020.  With SCA we simple enable the repositories we need associated with a particular RHEL instance to access that content with out attaching a subscription.

![server05 patch content](/images/S22.png)

- Finally you can track your subscription usage in the Subscriptions section of the Red Hat Enterprise Linux view of Red Hat Insights.  Note: This tracks your RHEL usage and subscriptions at a high level i.e. the number RHEL subsriptions of any type against the number of deployed RHE systems.  It doesn include add-ons in the subscripton usage view.  If you've have set the service-level with the subscription-manage (see stepa above), you can get a more granular view by filtering on SLA by Premium, Standard, Self-Support, or No SLA.

![Subscriptions View](/images/S24.png)


      

---
### Reference

- How do I enable Simple Content Access for Red Hat Subscription Management? - in the [Simple Content Access article](https://access.redhat.com/articles/simple-content-access)
- [Subscription Manager Command Cheat Sheet](https://access.redhat.com/sites/default/files/attachments/rh_sm_command_cheatsheet_1214_jcs_print.pdf)
